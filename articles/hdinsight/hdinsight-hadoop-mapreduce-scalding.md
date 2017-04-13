<properties
 pageTitle="Töötada koos Maven põletushaavad MapReduce töö | Microsoft Azure'i"
 description="Saate teada, kuidas kasutada Maven looge põletushaavad MapReduce töö, siis juurutada ja käivitada töö Hadoopi Hdinsightiga kobar."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>
<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Arendamise põletushaavad MapReduce töökohtade Apache Hadoopi Hdinsightiga

Põletushaavad on Scala teek, mis lihtsustab loomine Hadoopi MapReduce tööde haldamine. See pakub sisutihedaid süntaks, samuti tihedalt integreerimine Scala.

Selles dokumendis saate teada, kuidas luua lihtsa word count MapReduce töö kirjutatud Scalding Maven abil. Seejärel saate teada, kuidas juurutada ja selle töö käitamist on Hdinsightiga kobar.

## <a name="prerequisites"></a>Eeltingimused

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A Windowsi või Linuxi vastavalt Hadoopi Hdinsightiga kobar**. Lisateabe saamiseks vaadake [sätte Linux-põhine Hadoopi Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md) või [sätte Windowsi-põhiste Hadoopi Hdinsightiga](hdinsight-provision-clusters.md) .

* **[Maven](http://maven.apache.org/)**

* **[Java platvormi JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 või uuem versioon**

## <a name="create-and-build-the-project"></a>Luua ja koostada projekt

1. Kasutage Maven uue projekti loomiseks järgmine käsk:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    See käsk Loo uus kaust nimega **scaldingwordcount**ja tellinguid Scala rakenduse loomine.

2. Kataloogis **scaldingwordcount** **pom.xml** faili avada ja asendage sisu järgmist:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
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
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Selle faili kirjeldatakse projekti, sõltuvused ja lisandmoodulid. Siin on oluline kirjeid.

    * **Maven.compiler.Source** ja **maven.compiler.target**: seab selle projekti Java versioon

    * **hoidlate**: hoidlate, mis sisaldavad selle projekti kasutatav sõltuvus fail

    * **põletushaavad core_2.11** ja **hadoop-core**: Scalding nii Hadoopi core pakettide sõltub selle projekti

    * **Maven-scala-lisandmoodul**: scala rakenduste koostada lisandmoodul

    * **Maven-varju-lisandmoodul**: lisandmooduli loomiseks taustaga purgid (paks). Selle lisandmooduli kehtib filtrid ja teisendused; specificially:

        * **filtrid**: rakendatud filtrid jar faili kaasas metaandmete teavet muuta. Et vältida sisselogimise erandid käitusajal, välja arvatud erinevaid signatuuri faile, mis võib olla kaasatud sõltuvused.

        * **täitmised**: paketi etapp täitmise konfiguratsiooni määrab **com.twitter.scalding.Tool** klassi peamiseks klassi paketi. Ilma selleta peate com.twitter.scalding.Tool, samuti ainekursust sisaldava rakenduse loogika, hadoop käsk projekti käivitamisel määrata.

3. Kustutada **src katse** kataloogi, kui te pole loome kontrollib käesoleva näite.

4. Avage fail **src/main/scala/com/microsoft/example/App.scala** ja asendage sisu järgmist:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    See rakendatakse põhilised word count töö.

5. Salvestage ja sulgege failid.

6. Järgmine käsk kataloogist **scaldingwordcount** abil saate koostada ja paketti rakendus:

        mvn package

    Pärast selle töö lõpulejõudmist leiate **target/scaldingwordcount-1.0-SNAPSHOT.jar**paketi WordCount taotlus.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Töö käitamist Linux-põhine kobar

> [AZURE.NOTE] Järgmiste juhiste kasutada SSH ja Hadoopi käsk. Muud viisid MapReduce tööde, leiate [Kasutamine MapReduce Hadoopi Hdinsightiga sisse](hdinsight-use-mapreduce.md).

1. Järgmise käsu abil saate paketi üleslaadimine klaster Hdinsightiga:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Selle valiku puhul kopeeritakse failid kohalik süsteem pea sõlme.

    > [AZURE.NOTE] Kui kasutasite parooli konto SSH, küsitakse teilt, kas soovite parooli. Kui olete kasutanud mõnda SSH võti, peate kasutama funktsiooni `-i` parameetri ja privaatvõti teed. Näiteks`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Järgmise käsu abil saate ühendada kobar pea sõlme:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Kui kasutasite parooli konto SSH, küsitakse teilt, kas soovite parooli. Kui olete kasutanud mõnda SSH võti, peate kasutama funktsiooni `-i` parameetri ja privaatvõti teed. Näiteks`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Pärast ühendatud pea sõlme, kasutage järgmist käsku Wordi count töö

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    See käivitab WordCount klassi, saate rakendada varasemas versioonis. `--hdfs`juhendab töö HDFS kasutama. `--input`määrab teksti sisestamine faili, samal ajal `--output` määrab soovitud Väljastuskoht.

4. Pärast töö lõpulejõudmist, kasutage järgmist vaatamiseks väljund.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Sellega kuvatakse järgmine teave:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Windowsi-põhiste klaster töö

Windows PowerShelli kasutamine toimige järgmiselt. Muud viisid MapReduce tööde, leiate [Kasutamine MapReduce Hadoopi Hdinsightiga sisse](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Alustage Azure PowerShelli ja logige sisse Azure'i kontosse. Pärast oma mandaatide käsk tagastab oma konto teavet.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Kui teil on mitu tellimust, sisestage tellimuse id, mida soovite kasutada juurutamiseks.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Saate kasutada `Get-AzureRMSubscription` saada kõik tellimused oma konto, mis sisaldab iga tellimuse Id-ga seostatud loendit.

4. Kasutage järgmist skripti üles laadida ja käivitada WordCount töö. Asendage `CLUSTERNAME` Hdinsightile oma nime klaster ning veenduge, et `$fileToUpload` on õige __Scaldingwordcount-1,0-SNAPSHOT.jar__ faili tee.

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Skripti käivitamisel palutakse teil Hdinsightiga klaster sisestage administraatori kasutajanime ja parooliga. Vigu töö töötamise logitakse konsooli.
     
6. Pärast töö lõpulejõudmist väljund laaditakse fail __output.txt__ praegust kausta. Järgmise käsu abil saate kuvada tulemused.

        cat output.txt

    Faili peaks sisaldama väärtused, mis on järgmine:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas luua MapReduce töö jaoks Hdinsightiga Scalding abil, kasutage järgmisi linke uurida muul viisil töötada Windows Azure Hdinsightiga.

* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)

* [Hdinsightiga MapReduce töö kasutamine](hdinsight-use-mapreduce.md)
