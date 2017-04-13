<properties
    pageTitle="Koostada HBase rakenduse Maven ja Java abil, siis võtta kasutusele Linuxi-põhiste Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas koostada Java-põhine Apache HBase rakenduse, siis selle juurutama pilveteenuses Azure Hdinsightile Linux-põhine Apache Maven abil."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Maven abil saate koostada Java rakendusi, mida kasutada HBase Linux-põhine Hdinsightiga (Hadoopi)

Saate teada, kuidas luua ja koostada Java rakenduse [Apache HBase](http://hbase.apache.org/) Apache Maven abil. Seejärel kasutada rakenduse Linux-põhine Hdinsightiga kobar.

[Maven](http://maven.apache.org/) on tarkvara projektihaldus ja mõistmist tööriista, mis võimaldab teil luua tarkvara, dokumente ja aruannete Java projektide jaoks. Selles artiklis teada, kuidas seda luua lihtsa Java rakendus, mida loob, päringute, kasutada ja kustutab Linux-põhine Hdinsightiga klaster on HBase tabel.

> [AZURE.NOTE] Selle dokumendi juhistes eeldatakse, et kasutate Linux-põhine Hdinsightiga kobar. Windowsi-põhiste Hdinsightiga kobar kasutamise kohta leiate teemast [Kasutada Maven luua Java rakendusi, mis kasutavad HBase koos Windowsi-põhiste Hdinsightiga](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Nõuded

* [Java platvormi JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 või uuem versioon

* [Maven](http://maven.apache.org/)

* [Koos HBase on Linux-põhine Windows Azure Hdinsightiga kobar](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Juhised selle dokumendi on testitud Hdinsightiga kobar versioonide 3,2, 3.3 ja 3.4. Vaikeväärtused esitatud näidetes on Hdinsightiga 3.4 kobar.

* **Tundmine SSH ja SCP**. SSH ja SCP Hdinsightiga kasutamise kohta lisateabe saamiseks vaadake järgmist:

    * **Linux, Unix või OS X kliendid**: vt [Kasutamine SSH koos Linux-põhine Hadoopi Linux, OS X või Unix Hdinsightiga](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windowsi kliendid**: vt [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Projekti loomine

1. Käsurea sisse oma arenduskeskkond, muuta kataloogide asukohta, kuhu soovite luua projekti, näiteks `cd code/hdinsight`.

2. Käsk __mvn__ , mis installitakse koos Maven abil saate luua projekti tellinguid.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    See loob uue kausta soovitud praegust kausta nimi on määratud __artifactID__ parameetri (käesoleva näite**hbaseapp** .) See kaust sisaldab järgmist:

    * __POM.XML__: projekti objektimudelile ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) sisaldab projekti koostamiseks kasutatud andmed ja konfiguratsiooni üksikasjad.

    * __src__: kataloogi, mis sisaldab kataloogi __põhi/java/com/microsoft/näited__ , kus näidatakse Autor rakendus.

3. Kustutage fail __src/test/java/com/microsoft/examples/apptest.java__ , kuna see ei tohi kasutada järgmises näites.

##<a name="update-the-project-object-model"></a>Projekti objektimudelile värskendamine

1. __Pom.xml__ faili redigeerimiseks ja lisage järgmine kood sees on `<dependencies>` jaotis:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    See ütleb Maven, et projekt nõuab __hbase-kliendi__ versiooni __1.1.2__. Kompileerimise ajal laaditakse see vaikimisi Maven hoidla. Saate lisateavet selle sõltuvus [Maven keskses hoidlas otsing](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Versiooninumber peab vastama HBase, mis on esitatud Hdinsightiga klaster versiooni. Õige versiooninumber otsimiseks kasutada järgmises tabelis.

  	| Hdinsightiga kobar versioon | HBase versiooni |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 ja 3.4 | 1.1.2 |

    Hdinsightiga versioonid ja komponentide kohta lisateabe saamiseks vaadake teemat [mis on erinevate Hadoopi komponendid Hdinsightiga saadaval](hdinsight-component-versioning.md).

2. Kui kasutate ja Hdinsightiga 3.3 või 3.4 kobar, peate lisama ka järgmist funktsiooni `<dependencies>` jaotis:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    See laadib phoenix tuumaga komponendid, mis on vajalikud Hbase versiooniga 1.1.x.

2. Järgmine kood __pom.xml__ faili lisada. See peab asuma selle `<project>...</project>` siltide faili, näiteks `</dependencies>` ja `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.7</source>
                <target>1.7</target>
              </configuration>
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

    See konfigureerib ressursi (__conf/hbase-site.xml__), mis sisaldab konfiguratsiooniteavet HBase.

    > [AZURE.NOTE] Samuti saate konfiguratsiooni väärtused koodi kaudu. Vt __CreateTable__ näide, mis kohta, kuidas seda teha järgmiselt.

    See on ka konfigureerib [Maven koostaja lisandmoodul](http://maven.apache.org/plugins/maven-compiler-plugin/) ja [Maven varju lisandmoodul](http://maven.apache.org/plugins/maven-shade-plugin/). Lisandmooduli koostaja kasutatakse topoloogia koostada. Lisandmooduli varju kasutatakse JAR paketti, mis on loodud Maven litsentsi kattumise vältimiseks. Seda kasutatakse põhjuseks on, et dubleeritud litsentsi failide põhjustada tõrke käitusajal Hdinsightiga klaster. Maven-varju-lisandmoodul, mille abil soovitud `ApacheLicenseResourceTransformer` rakendamist takistab selle tõrke.

    Maven varju lisandmooduli toodab ka uber jar (või paks jar,), mis sisaldab kõiki sõltuvusi nõuab.

3. Salvestage fail __pom.xml__ .

4. Looge uus kaust nimega __conf__ __hbaseapp__ kataloogis. Seda kasutatakse konfiguratsiooniteavet ühenduse HBase korraldada.

5. Järgmise käsu abil saate kopeerida HBase konfiguratsiooni serverist Hdinsightile __conf__ kataloogi. Asendage **kasutajanimi** on teie SSH login nime. Asendage **CLUSTERNAME** oma Hdinsightiga kobar nimega.

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Kui kasutasite SSH konto parool, palutakse teil sisestada parool. Kui kasutasite mõne SSH võti kontoga, peate kasutama funktsiooni `-i` võtme faili tee Määrake-parameeter. Järgmises näites laaditakse alla privaatvõti `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Rakenduse loomine

1. Minge __hbaseapp/src/main/java/com/microsoft/näiteid__ kataloogi ja __CreateTable.java__app.java faili ümber nimetada.

2. Avage fail __CreateTable.java__ ja asendage olemasolev sisu järgmist:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    See on __CreateTable__ ainekursust, mis loob tabelis nimega __inimesed__ ja asustada mõned eelmääratletud kasutajad.

3. Salvestage fail __CreateTable.java__ .

4. Kataloogis __hbaseapp/src/põhi/java/com/microsoft/näiteid__ __SearchByEmail.java__nimega uue faili loomine. Kasutage seda faili sisu järgmine:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    Klassi __SearchByEmail__ saab kasutada päringusse ridade e-posti aadress. Kuna see kasutab Lihtavaldise filtri, saate sisestada stringi või tavalise avaldis ainekursuse kasutamisel.

5. Salvestage fail __SearchByEmail.java__ .

6. Kataloogis __hbaseapp/src/põhi/hava/com/microsoft/näiteid__ __DeleteTable.java__nimega uue faili loomine. Kasutage seda faili sisu järgmine:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    See tund on puhastamiseks selles näites keelamise ja pukseerimine __CreateTable__ klassi loodud tabel.

7. Salvestage fail __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Koostamine ja pakkimine rakendus

2. Kasutage kataloogist __hbaseapp__ koostamiseks JAR faili, mis sisaldab rakenduse järgmine käsk:

        mvn clean package

    See puhastab mis tahes eelmise Koosta esemeid, allalaadimist sõltuvusi, mis ei ole juba installitud, siis koostab ja rakenduse paketid.

3. Kui käsk on lõpule jõudnud, sisaldab __hbaseapp/siht__ kataloogi fail nimega __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] __Hbaseapp-1,0-SNAPSHOT.jar__ fail on mõne uber jar (mõnikord nimetatakse paks jar,), mis sisaldab kõiki sõltuvusi nõutav rakenduse käivitamiseks.

##<a name="upload-the-jar-file-and-run-jobs"></a>JAR faili üles laadida ja käivitada tööde haldamine

1. Kasutada järgmisi Hdinsightiga kobar purki üles. Asendage **kasutajanimi** on teie SSH login nime. Asendage **CLUSTERNAME** oma Hdinsightiga kobar nimega.

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    See üles laadida faili home directory SSH kasutajakonto.

    > [AZURE.NOTE] Kui kasutasite SSH konto parool, palutakse teil sisestada parool. Kui kasutasite mõne SSH võti kontoga, peate kasutama funktsiooni `-i` võtme faili tee Määrake-parameeter. Järgmises näites laaditakse alla privaatvõti `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Kasutage SSH ühenduse Hdinsightiga kobar. Asendage **kasutajanimi** on teie SSH login nime. Asendage **CLUSTERNAME** oma Hdinsightiga kobar nimega.

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Kui kasutasite SSH konto parool, palutakse teil sisestada parool. Kui kasutasite mõne SSH võti kontoga, peate kasutama funktsiooni `-i` võtme faili tee Määrake-parameeter. Järgmises näites laaditakse alla privaatvõti `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Kui ühendus on loodud, kasutage järgmist Java rakenduse abil HBase uue tabeli loomine:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    See nimega __inimesed__HBase uue tabeli loomine ja selle andmetega asustada.

4. Järgmiseks e-posti aadressid, mis on talletatud tabelis otsimiseks kasutada järgmist:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Peaksite nägema järgmised tulemused:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Tabeli kustutamine

Kui olete lõpetanud, näiteks, kasutada __inimesed__ selles näites kasutatakse tabeli kustutamiseks Azure PowerShelli seansi järgmine käsk:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

