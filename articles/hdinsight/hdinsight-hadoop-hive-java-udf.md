<properties
pageTitle="Java kasutaja määratletud funktsiooni (UDF) kasutamine rakenduses Hdinsightiga taru | Microsoft Azure'i"
description="Saate teada, kuidas luua ja kasutada Java kasutaja määratletud funktsiooni (UDF) taru Hdinsightiga sisse."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Kasutage Java UDF-i abil rakenduses Hdinsightiga taru

Taru sobib hästi Hdinsightiga andmetega töötamine, kuid mõnikord on vaja rohkem üldine eesmärk keel. Taru võimaldab teil luua kasutaja määratletud funktsioonid (UDF) kasutades erinevaid programmeerimise keeled. Selles dokumendis saate teada, kuidas kasutada Java UDF taru.

## <a name="requirements"></a>Nõuded

* Azure'i tellimuse

* Mõne Hdinsightiga kobar (Windowsi või Linuxi-põhine)

    > [AZURE.NOTE] Enamik juhiseid selles dokumendis töötavad mõlemat kobar siiski kasutada üles laadida kompileeritud UDF klaster ja käivitage see juhised kehtivad teatud kogumite Linux-põhine. Lingid on esitatud teavet, mida saab kasutada koos Windowsi-põhiste kogumite.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 või uuem versioon (või samaväärne, nt OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Java IDE või tekstiredaktoris

    > [AZURE.IMPORTANT] Kui kasutate Linux-põhine Hdinsightiga server, kuid Python failid Windowsi kliendi loomist peate kasutama toimetaja mis kasutab LF rea lõpus. Kui te pole kindel, kas teie redaktor kasutab LF või CRLF, jaotisest [tõrkeotsingu](#troubleshooting) juhiste CR märgi Utiliidid kasutamine Hdinsightiga kobar eemaldamise kohta.

## <a name="create-an-example-udf"></a>Näide UDF-i loomine

1. Mõne käsurea kaudu kasutage Maven uue projekti loomiseks järgmist.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Kui kasutate PowerShelli, Pange parameetrid jutumärkidesse. Näiteks `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    See loob uue kataloogi __exampleudf__, mis sisaldab Maven projekt.

2. Kui projekt on loodud, kustutada __exampleudf src/katse__ kataloogi, mis on loodud projekti; See ei kasutata selles näites.

3. Avage __exampleudf/pom.xml__ja olemasoleva `<dependencies>` järgmise kirje:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Saate määrata need kirjed Hadoopi ja taru kaasas Hdinsightiga 3.3 ja 3.4 kogumite versiooni. Leiate teavet Hadoopi ja taru varustatud Hdinsightiga dokumendist [Hdinsightiga komponendi Versioonimine](hdinsight-component-versioning.md) versioonid.

    Lisada mõne `<build>` jaotise enne selle `</project>` rea lõpus olevat faili. See jaotis sisaldab järgmist:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Need kirjed määrata, kuidas luua projekti. Täpsemalt versioon Java, mis kasutab projekti ja kuidas luua mõne uberjar klaster juurutamiseks.

    Salvestage fail, kui on tehtud muudatusi.

4. Nimeta __exampleudf/src/main/java/com/microsoft/examples/App.java__ __ExampleUDF.java__ja avage fail oma redaktor.

5. Asendage __ExampleUDF.java__ faili sisu järgmine, salvestage fail.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    See rakendatakse UDF aktsepteerib stringiväärtus, ja tagastab stringi väiketähtedeks versiooni.

## <a name="build-and-install-the-udf"></a>Koostamine ja installige UDF

1. Järgmise käsu abil saate koostada ja paketti UDF:

        mvn compile package

    See koostamine ja seejärel paketti UDF __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__sisse.

2. Kasutage soovitud `scp` käsk Hdinsightiga kobar-faili kopeerimine.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Asendage __myuser__ klaster SSH kasutajakontoga. Asendage __mycluster__ kobar nimi. Kui kasutasite turvamiseks SSH konto parool, palutakse teil sisestada parool. Kui kasutasite sert, peate kasutama funktsiooni `-i` parameetri määrama privaatse võtme faili.

3. Ühenduse klaster SSH abil. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Hdinsightiga SSH kasutamise kohta leiate lisateavet teemast järgmised dokumendid.

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Kopeerige SSH seansiga Hdinsightiga salvestusruumi jar-faili.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Kasutage UDF taru kaudu

1. Kasutage järgmist Linnulennutee kliendi SSH seansi käivitamine.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    See käsk eeldab, et kasutada klaster login konto jaoks vaikimisi __administraator__ .

2. Kui jõuate selle `jdbc:hive2://localhost:10001/>` Küsi, sisestage järgmine lisada UDF taru ja jätke funktsiooni.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. UDF abil saate teisendada soovite väiketähtedes stringide väärtuste tabeli.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    See seadme platvormi (Android, Windows, iOS-i jne) Valige tabelist, lower juhul string teisendamine ja kuvage need. Väljundisse kuvatakse järgmise sisuga.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Järgmised sammud

Muul viisil töötada taru, leiate teemast [Kasutamine taru koos Hdinsightiga](hdinsight-use-hive.md).

Lisateavet, vt [taru tehtemärgid ja kasutaja määratletud funktsioonid](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) jaotises taru viki veebisaidil apache.org Hive User-Defined funktsioonid.
