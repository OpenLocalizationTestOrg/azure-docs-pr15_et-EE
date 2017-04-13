<properties
pageTitle="Koostada Maven rakendus HBase ja võtta kasutusele Windowsi-põhiste Hdinsightiga | Microsoft Azure'i"
description="Saate teada, kuidas koostada Java-põhine Apache HBase rakenduse, siis selle juurutama Windowsi-põhise Windows Azure Hdinsightiga kobar Apache Maven abil."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Maven abil saate koostada Java rakendusi, mida kasutada HBase Windowsi-põhiste Hdinsightiga (Hadoopi)

Saate teada, kuidas luua ja koostada Java rakenduse [Apache HBase](http://hbase.apache.org/) Apache Maven abil. Klõpsake rakenduse kasutamine Windows Azure Hdinsightiga (Hadoopi).

[Maven](http://maven.apache.org/) on tarkvara projektihaldus ja mõistmist tööriista, mis võimaldab teil luua tarkvara, dokumente ja aruannete Java projektide jaoks. Selles artiklis saate teada, kuidas abil luua lihtsa Java rakendus, mis loob, päringud ja kustutab on HBase tabel, klõpsake mõnda Windows Azure Hdinsightiga kobar.

> [AZURE.NOTE] Selle dokumendi juhistes eeldatakse, et kasutate Windowsi-põhiste Hdinsightiga kobar. Linux-põhine Hdinsightiga kobar kasutamise kohta leiate teemast [Kasutada Maven luua Java rakendusi, mis kasutavad HBase koos Linuxi-põhiste Hdinsightiga](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Nõuded

* [Java platvormi JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 või uuem versioon

* [Maven](http://maven.apache.org/)


* [Windowsi-põhiste Hdinsightiga kobar HBase abil](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] Juhised selle dokumendi on testitud Hdinsightiga kobar versioonide 3,2 ja 3.3. Hdinsightiga 3.3 kobar on esitatud näidetes vaikeväärtused.

##<a name="create-the-project"></a>Projekti loomine

1. Teie arenduskeskkond käsureal muutmine kataloogide asukohta, kuhu soovite luua projekti, näiteks `cd code\hdinsight`.

2. Käsk __mvn__ , mis installitakse koos Maven abil saate luua projekti tellinguid.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    See käsk loob kataloog praeguses asukohas, mille nimi on määratud __artifactID__ parameetri (käesoleva näite**hbaseapp** .) See kaust sisaldab järgmist:

    * __POM.XML__: projekti objektimudelile ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) sisaldab projekti koostamiseks kasutatud andmed ja konfiguratsiooni üksikasjad.

    * __src__: __main\java\com\microsoft\examples__ kaust, kus kuvatakse Autor rakenduse sisaldava kausta.

3. Kustutage fail __src\test\java\com\microsoft\examples\apptest.java__ , kuna see ei kasutata selles näites.

##<a name="update-the-project-object-model"></a>Projekti objektimudelile värskendamine

1. __Pom.xml__ faili redigeerimiseks ja lisage järgmine kood sees on `<dependencies>` jaotis:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Selles teemas kirjeldatakse Maven, et projekt nõuab __hbase-kliendi__ versiooni __1.1.2__. Kompileerimise ajal laaditakse selle sõltuvus vaikimisi Maven hoidla. Saate lisateavet selle sõltuvus [Maven keskses hoidlas otsing](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Versiooninumber peab vastama HBase, mis on esitatud Hdinsightiga klaster versiooni. Õige versiooninumber otsimiseks kasutada järgmises tabelis.

  	| Hdinsightiga kobar versioon | HBase versiooni |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Hdinsightiga versioonid ja komponentide kohta lisateabe saamiseks vaadake teemat [mis on erinevate Hadoopi komponendid Hdinsightiga saadaval](hdinsight-component-versioning.md).

2. Kui kasutate mõnda Hdinsightiga 3.3 kobar, peate lisama ka järgmist funktsiooni `<dependencies>` jaotis:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    See sõltuvus laaditakse phoenix tuumaga komponendid, mida kasutavad Hbase versioon 1.1.x.

2. Järgmine kood __pom.xml__ faili lisada. Selles jaotises peab asuma selle `<project>...</project>` siltide faili, näiteks `</dependencies>` ja `</project>`.

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

    Funktsiooni `<resources>` jaotis konfigureerib ressurss (__conf\hbase-site.xml__), mis sisaldab konfiguratsiooniteavet HBase.

    > [AZURE.NOTE] Samuti saate konfiguratsiooni väärtused koodi kaudu. Vt __CreateTable__ näide, mis kohta, kuidas seda teha järgmiselt.

    See `<plugins>` jaotis konfigureerib [Maven koostaja lisandmoodul](http://maven.apache.org/plugins/maven-compiler-plugin/) ja [Maven varju lisandmoodul](http://maven.apache.org/plugins/maven-shade-plugin/). Lisandmooduli koostaja kasutatakse topoloogia koostada. Lisandmooduli varju kasutatakse JAR paketti, mis on loodud Maven litsentsi kattumise vältimiseks. Seda kasutatakse põhjuseks on, et dubleeritud litsentsi failide põhjustada tõrke käitusajal Hdinsightiga klaster. Maven-varju-lisandmoodul, mille abil soovitud `ApacheLicenseResourceTransformer` rakendamist takistab selle tõrke.

    Maven varju lisandmooduli toodab ka uber jar (või paks jar), mis sisaldab kõiki sõltuvusi nõuab.

3. Salvestage fail __pom.xml__ .

4. Looge uus kaust nimega __conf__ __hbaseapp__ kataloogis. Kataloogis __conf__ looge fail nimega __hbase-site.xml__. Kasutage järgmist faili sisu:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Laadimiseks HBase konfiguratsiooni mõne Hdinsightiga kobar kasutatakse seda faili.

    > [AZURE.NOTE] See on minimaalne hbase-site.xml faili ja see sisaldab Hdinsightiga kobar tühjal minimaalsed sätted.

3. Salvestage fail __hbase-site.xml__ .

##<a name="create-the-application"></a>Rakenduse loomine

1. Minge __hbaseapp\src\main\java\com\microsoft\examples__ kataloogi ja __CreateTable.java__app.java faili ümber nimetada.

2. Avage fail __CreateTable.java__ ja asendage olemasolev sisu järgmine kood:

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. Kataloogis __hbaseapp\src\main\java\com\microsoft\examples__ nimega __SearchByEmail.java__uue faili loomine. Kasutage seda faili sisu järgmine kood:

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

6. Kataloogis __hbaseapp\src\main\hava\com\microsoft\examples__ nimega __DeleteTable.java__uue faili loomine. Kasutage seda faili sisu järgmine kood:

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

1. Avage käsuviip ja muuta kataloogide __hbaseapp__ kataloogi.

2. Järgmise käsu abil saate koostada JAR faili, mis sisaldab rakenduse:

        mvn clean package

    See puhastab mis tahes eelmise koostamine esemeid, allalaadimist sõltuvusi, mis ei ole juba installitud, siis koostab ja rakenduse paketid.

3. Kui käsk on lõpule jõudnud, sisaldab __hbaseapp\target__ kataloogi fail nimega __Hbaseapp-1,0-SNAPSHOT.jar__.

    > [AZURE.NOTE] __Hbaseapp-1,0-SNAPSHOT.jar__ fail on mõne uber jar (mõnikord nimetatakse paks jar,), mis sisaldab kõiki sõltuvusi nõutav rakenduse käivitamiseks.

##<a name="upload-the-jar-file-and-start-a-job"></a>JAR faili üles laadida ja alustada tööd

On palju võimalusi, kuidas faili üleslaadimine Hdinsightiga klaster [üleslaadimine andmete Hadoopi töökohta, Hdinsightiga](hdinsight-upload-data.md)kirjeldatud. Azure'i PowerShelli kasutamine toimige järgmiselt.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Pärast installimist ja konfigureerimist Azure PowerShelli, nimega __hbase-runner.psm1__uue faili loomine. Kasutage seda faili sisu järgmine:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    See fail sisaldab kahte moodulid:

    * __Lisa-HDInsightFile__ - kasutatakse Hdinsightiga failide üleslaadimine

    * __Algus-HBaseExample__ - tunnid, mis on varem loodud käivitamiseks kasutatud

2. Salvestage fail __hbase-runner.psm1__ .

3. Azure PowerShelli uues aknas avada, muuta kataloogide __hbaseapp__ kataloogi ja seejärel käivitage järgmine käsk.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Saate muuta varem loodud __hbase-runner.psm1__ faili asukoha tee. See registreerib mooduli Azure PowerShelli selle seansi jaoks.

2. Järgmise käsu abil üles laadida __Hbaseapp-1,0-SNAPSHOT.jar__ klaster Hdinsightile.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Asendage __hdinsightclustername__ klaster Hdinsightiga nime. Käsu lisatud __Hbaseapp-1,0-SNAPSHOT.jar__ esmane hoidmiseks Hdinsightiga klaster __näide/purgid__ kohta.

3. Pärast seda, kui failid on üles laaditud, kasutada __hbaseapp__kasutamine tabeli loomiseks järgmine kood:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Asendage __hdinsightclustername__ klaster Hdinsightiga nime.

    See käsk loob uue nimega klaster Hdinsightiga __inimesed__ . See käsk Kuva kõik väljundi konsooli aken.

2. Tabeli otsimiseks kasutada järgmine käsk:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Asendage __hdinsightclustername__ klaster Hdinsightiga nime.

    See käsk kasutab **SearchByEmail** klassi otsida kõiki ridu, kus __contactinformation__ veeru pere ja __e-posti__ veerg sisaldab stringi __contoso.com__. Peaksite nägema järgmised tulemused:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Kasutades __fabrikam.com__ jaoks soovitud `-emailRegex` väärtus tagastab kasutajatele, kes on e-posti väli __fabrikam.com__ . Kuna see otsing on tavaline avaldise põhinev filtri abil, võite ka sisestada regulaaravaldised, näiteks __^ r__, mis tagastab kirjed, kus e-posti algab r-täht.

##<a name="delete-the-table"></a>Tabeli kustutamine

Kui olete lõpetanud, näiteks, kasutada __inimesed__ selles näites kasutatakse tabeli kustutamiseks Azure PowerShelli seansi järgmine käsk:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Asendage __hdinsightclustername__ klaster Hdinsightiga nime.

##<a name="troubleshooting"></a>Tõrkeotsing

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Ootamatud tulemused algus-HBaseExample kasutamisel ega tulemused

Kasutage funktsiooni `-showErr` vaatamiseks standardviga (STDERR), mis on toodetud töö käitamisel parameetrite.
