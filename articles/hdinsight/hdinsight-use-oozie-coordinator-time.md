<properties
    pageTitle="Ajapõhiste Hadoopi Oozie koordinaator kasutamine Hdinsightiga | Microsoft Azure'i"
    description="Kasutage ajapõhiste Hadoopi Oozie koordinaator Hdinsightiga suur andmete teenust. Saate teada, kuidas määratleda Oozie töövood ja koordinaatorid ja edastab tööd."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Ajapõhiste Oozie koordinaator kasutamine Hadoopi rakenduses Hdinsightiga määratlemine töövood ja koordineerimine tööde haldamine

Selles artiklis õpite määratlemine töövood ja koordinaatorid ja kuidas käivitamine koordinaator tööde, aeg. Oleks kasulik läbida [Kasutamine Oozie koos Hdinsightiga] [ hdinsight-use-oozie] enne seda artiklit lugema. Lisaks Oozie, saate ka plaanida Azure'i andmed Factory abil. Azure'i andmed Factory leiate teemast [kasutamine siga ja taru Factory andmeid koos](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Selles artiklis nõuab Windowsi-põhiste Hdinsightiga kobar. Oozie, sh ajapõhiste tööd, Linux-põhine kobar kasutamise kohta teavet teemast [Kasutamine Oozie koos Hadoopi määratlemine ja Linux-põhine Hdinsightiga klõpsake töövoo käivitamine](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Mis on Oozie

Apache Oozie on töövoo/koordineerituna haldava Hadoopi tööde haldamine. See on integreeritud Hadoopi virnas ja Hadoopi töö toetab Apache MapReduce, Apache siga, Apache taru ja Apache Sqoop. See saab kavandada süsteemi, nt Java programmid või shell skriptide tööd.

Järgmisel pildil on kujutatud saate rakendab töövoog:

![Töövoo diagramm][img-workflow-diagram]

Töövoog sisaldab kaks toimingut.

1. Taru toiming käivitatakse HiveQL skripti log4j logifail iga log taseme tüüp esinemiskordade loendamine. Iga log4j logifailide koosneb väljade rida, mis sisaldab [Logi tase] välja kuvamiseks tüüp ja raskusaste, näiteks:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Taru skripti väljundi sarnaneb.

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Taru kohta leiate lisateavet teemast [Kasutamine taru koos Hdinsightiga][hdinsight-use-hive].

2.  Sqoop toimingu ekspordib HiveQL toimingu väljund SQL Azure'i andmebaasi tabelisse. Sqoop kohta leiate lisateavet teemast [Kasutamine Sqoop koos Hdinsightiga][hdinsight-use-sqoop].

> [AZURE.NOTE] Toetatud Oozie versioonide Hdinsightiga kogumite, lugege teemat [mis on uut esitatud Hdinsightiga kobar versioonides?] [hdinsight-versions].


##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **An Hdinsightiga kobar**. Mõne Hdinsightiga kobar loomise kohta leiate teemast [loomine Hdinsightiga kogumite][hdinsight-provision], või [Alustamine Hdinsightiga][hdinsight-get-started]. Peate läbima õpetuse järgmised andmed:

    <table border = "1">
    <tr><th>Atribuut kobar</th><th>Windows PowerShelli muutuja nimi</th><th>Väärtus</th><th>Kirjeldus</th></tr>
    <tr><td>Hdinsightiga kobar nimi</td><td>$clusterName</td><td></td><td>Hdinsightiga kobar, mil käivitate selle õpetuse.</td></tr>
    <tr><td>Hdinsightiga kobar kasutajanimi</td><td>$clusterUsername</td><td></td><td>Hdinsightiga kobar kasutajanimi. </td></tr>
    <tr><td>Hdinsightiga kobar kasutaja parooli </td><td>$clusterPassword</td><td></td><td>Hdinsightiga kobar kasutaja parooli.</td></tr>
    <tr><td>Azure'i salvestusruumikonto nimi</td><td>$storageAccountName</td><td></td><td>Azure Storage konto Hdinsightiga klaster saadaval. Selles õpetuses kasutada käigus kobar sätte määratud vaikekonto salvestusruumi.</td></tr>
    <tr><td>Azure'i bloobimälu container nimi</td><td>$containerName</td><td></td><td>Selle näite puhul kasutada Azure'i bloobimälu salvestusruumi container, mida on kasutatud Hdinsightiga kobar failisüsteemi. Vaikimisi on Hdinsightiga kobar sama nimi.</td></tr>
    </table>

- **An Azure SQL-andmebaas**. SQL-andmebaasi server, kui soovite lubada juurdepääsu oma töökoha tulemüüri reegli tuleb konfigureerida. Juhised leiate Azure'i SQL-i loomise andmebaasi ja konfigureerida tulemüüri, leiate [Azure'i SQL-andmebaasi kasutamise alustamine][sqldatabase-get-started]. Selles artiklis on toodud Windows PowerShelli skripti Azure SQL-i andmebaasi tabelit, mida teil on vaja selle õpetuse loomise kohta.

    <table border = "1">
    <tr><th>SQL-i andmebaasi atribuut</th><th>Windows PowerShelli muutuja nimi</th><th>Väärtus</th><th>Kirjeldus</th></tr>
    <tr><td>SQL-i andmebaasiserveri nimi</td><td>$sqlDatabaseServer</td><td></td><td>SQL serveri andmebaasi Sqoop ekspordib andmed. </td></tr>
    <tr><td>SQL-andmebaasi kasutajanimi</td><td>$sqlDatabaseLogin</td><td></td><td>SQL-andmebaasi kasutajanimi.</td></tr>
    <tr><td>SQL-i andmebaasi sisselogimise parool</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL-andmebaasi sisselogimise parool.</td></tr>
    <tr><td>SQL-i andmebaasi nimi</td><td>$sqlDatabaseName</td><td></td><td>Azure'i SQL-andmebaasi Sqoop ekspordib andmed. </td></tr>
    </table>

    > [AZURE.NOTE] Vaikimisi lubab Azure'i SQL-andmebaasiga ühendused Azure'i teenuseid, näiteks Windows Azure Hdinsightiga. Kui tulemüüri see säte on keelatud, tuleb see lubada Azure'i portaalis. Juhiseid SQL-andmebaasi loomine ja konfigureerimine tulemüüri reeglid, vt [loomine ja konfigureerimine SQL-andmebaasi][sqldatabase-get-started].


> [AZURE.NOTE] FILL-in tabelite väärtused. Oleks kasulik läbi selles õpetuses.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie töövoog ja seotud HiveQL skripti määratlemine

Oozie töövoogude määratlused on kirjutatud hPDL (mõne XML-i protsess määratlus language). Vaikimisi töövoo faili nimi on *workflow.xml*.  Saate töövoo faili kohalikult salvestada, ja seejärel juurutada Hdinsightiga klaster Azure PowerShelli abil hiljem selles õpetuses.

Töövoo toimingu taru kõned HiveQL skripti faili. Skripti fail sisaldab kolme HiveQL laused:

1. **Lause DROP The TABLE** kustutab log4j taru tabel, kui see on olemas.
2. **Lause CREATE The TABLE** loob log4j taru välise tabeli, mis viitab asukoha log4j logifaili;
3.  **Logifaili log4j asukoht**. Välja eraldaja on ",". Vaikimisi rea eraldaja on "\n". Taru Välistabel kasutatakse vältimiseks algsesse asukohta, eemaldatakse andmefailiga juhuks, kui soovite mitu korda Oozie töövoo käivitada.
3. **The lisamine kirjutada lause** loendab iga taseme log tüüp tabelist log4j taru esinemiskordade ja salvestab väljund Azure'i bloobimälu salvestusruumi kohta.

**Märkus**: on taru tee teadaolev probleem. Kas tekib probleemi mõne Oozie töö esitamisel. Selle probleemi lahendamine juhised leiate TechNeti viki: [Hdinsightiga taru tõrge: ei saa ümber nimetada][technetwiki-hive-error].

**Määratleda töövoo kutsumist HiveQL skriptifail**

1. Luua tekstifaili sisu on järgmine:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    On kolme muutuja skripti kasutada.

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Töövoo definitsioonifail (workflow.xml selles õpetuses) läheb käitusajal selle HiveQL skripti need väärtused.

2. Salvestage fail **C:\Tutorials\UseOozie\useooziewf.hql** kodeerimine ANSI (ASCII) abil. (Kasutage Notepad, kui teie tekstiredaktorit ei paku selle suvandi.) See skriptifail juurutatakse Hdinsightiga klaster hiljem sisse õpetuse.



**Töövoo määratlemine**

1. Luua tekstifaili sisu on järgmine:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    On määratletud töövoo kaks toimingut. Käivita toiming on *RunHiveScript*. Kui toiming töötab *OK*, on järgmine toiming *RunSqoopExport*.

    Funktsiooni RunHiveScript sisaldab mitut muutujat. Kui annate Oozie töö oma töökoha Azure PowerShelli abil, läheb väärtused.

    <table border = "1">
    <tr><th>Töövoo muutujaid.</th><th>Kirjeldus</th></tr>
    <tr><td>${jobTracker}</td><td>Määrake URL-i Hadoopi töö jälgimine. Kasutage <strong>jobtrackerhost:9010</strong> Hdinsightiga kobar versiooni 3.0 ja 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Määrake URL-i sõlme Hadoopi nimi. Kasutage vaikimisi faili süsteemi wasbs: / / aadress, näiteks <i>wasbs: / /&lt;ümbrisenimi&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${queueName}</td><td>Määrab järjekorra nimi, mis esitatakse töö. <strong>Vaikimisi</strong>kasutada.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Taru toimingu muutuja</th><th>Kirjeldus</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Tabeli loomine taru käsu allikas kataloog.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Lause lisamine kirjutada väljund kaust.</td></tr>
    <tr><td>${hiveTableName}</td><td>Taru tabel, mis viitab log4j andmefailid nimi.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop toimingu muutuja</th><th>Kirjeldus</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>SQL-andmebaasi ühendusstringi.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Azure SQL-i andmebaasi tabel, kus andmed eksporditakse.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Lause taru lisamine kirjutada väljund kaust. See on samas kaustas Sqoop eksportimiseks (ekspordi-dir).</td></tr>
    </table>

    Oozie töövoog ja toiminguid töövoo kasutamise kohta leiate lisateavet teemast [Apache Oozie 4.0 dokumentatsiooni] [ apache-oozie-400] (jaoks Hdinsightiga kobar versiooni 3.0) või [Apache Oozie 3.3.2 dokumentatsiooni] [ apache-oozie-332] (Hdinsightiga kobar versioon 2.1) jaoks.

2. Salvestage fail **C:\Tutorials\UseOozie\workflow.xml** kodeerimine ANSI (ASCII) abil. (Kasutage Notepad, kui teie tekstiredaktorit ei paku selle suvandi.)

**Määratleda koordinaator**

1. Luua tekstifaili sisu on järgmine:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    On viis määramisel määratluse faili.

  	| Muutuja          | Kirjeldus |
  	| ------------------|------------ |
  	| ${coordFrequency} | Töö paus aeg. Sagedus on alati väljendatud minutit. |
  	| ${coordStart}     | Töö algusaeg. |
  	| ${coordEnd}       | Töö-ja lõppaeg. |
  	| ${coordTimezone}  | Oozie protsesside koordinaator töökohtade fikseeritud ajavöönd ei lähe kellaaeg (tavaliselt esitatud abil UTC). Ajavööndi nimetatakse "Oozie töötlemine ajavöönd." |
  	| ${wfPath}         | Tee on workflow.xml.  Kui töövoo faili nimi pole vaikimisi faili nimi (workflow.xml), määrake see. |

2. Salvestage fail **C:\Tutorials\UseOozie\coordinator.xml** kodeering ANSI (ASCII) abil. (Kasutage Notepad, kui teie tekstiredaktorit ei paku selle suvandi.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Projekti Oozie juurutada ja õpetuse ettevalmistamine

Käivitate on Azure PowerShelli skripti teha järgmist:

- Kopeerige HiveQL script (useoozie.hql) Azure'i bloobimälu wasbs:///tutorials/useoozie/useoozie.hql.
- Kopeerige workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
- Kopeerige coordinator.xml wasbs:///tutorials/useoozie/coordinator.xml.
- Kopeerige andmed fail (/ example/data/sample.log) wasbs:///tutorials/useoozie/data/sample.log abil.
- SQL Azure'i andmebaasi tabeli Sqoop ekspordi andmete talletamiseks loomine. Tabeli nimi on *log4jLogCount*.

**Hdinsightiga salvestusruumi mõistmine**

Hdinsightiga kasutab Azure'i bloobimälu andmesalv. wasbs: / / on Microsofti Hadoopi fail hajusfailisüsteemiga (HDFS) Azure'i bloobimälu rakendamine. Lisateabe saamiseks lugege teemat [kasutamine Azure'i bloobimälu Hdinsightiga][hdinsight-storage].

Kui olete ette mõne Hdinsightiga kobar, konto Azure'i bloobimälu salvestusruumi ja teatud container sellelt kontolt on määratud vaikimisi failisüsteemi, nt HDFS. Lisaks selle salvestusruumi konto, saate lisada täiendavat salvestusruumi kontod sama Azure tellimuse või muu Azure'i tellimused ebausaldusväärsete ajal. Juhised täiendav salvestusruum kontode lisamise kohta leiate teemast [sätte Hdinsightiga kogumite][hdinsight-provision]. Azure'i PowerShelli skripti kasutada selles õpetuses lihtsustamiseks kõik failid on salvestatud vaikimisi faili süsteemi container */tutorials/useoozie*asub. Vaikimisi on see ümbris Hdinsightiga kobar nime sama nimi.
Süntaks on järgmine:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Ainult selle *wasb: / /* süntaks on toetatud Hdinsightiga kobar versiooni 3.0. Vanemate *asv: / /* süntaks on toetatud Hdinsightiga 2.1 ja 1,6 kogumite, kuid see ei toeta Hdinsightiga 3.0 rühmades.

> [AZURE.NOTE] Funktsiooni wasb: / / tee on virtuaalne tee. Lisateabe saamiseks vaadake [teemasid Azure'i bloobimälu Hdinsightiga][hdinsight-storage].

Faili, mis on talletatud vaikimisi faili süsteemi container pääseb hdinsightist, kasutades ühte on järgmine URI-d (ma kasutan workflow.xml näide):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Kui soovite failile juurdepääsu kontolt salvestusruumi, bloobimälu faili nimi on:

    tutorials/useoozie/workflow.xml

**Mõista taru sise- ja tabelid**

On mõned asjad, mida peate teadma taru sise- ja tabelite kohta.

- TABELI loomine käsk loob sisemise tabeli, tuntud ka kui hallatava tabeli. Vaike-ümbrisest peab asuma andmefaili.
- TABELI loomine käsu viib andmefaili /hive/ladu/<TableName> vaike-ümbrisest kausta.
- VÄLISE tabeli loomine käsk loob mõne välise tabeli. Vaikimisi väljastpoolt võib asuda andmefaili.
- VÄLISE tabeli loomine käsu ei liigu andmefaili.
- VÄLISE tabeli loomine käsu ei luba alamkaustade asukoht-klauslis määratud kausta. See on põhjus, miks muudab õpetuse sample.log faili koopia.

Lisateavet leiate teemast [Hdinsightiga: taru sise- ja välise tabeleid sissejuhatav][cindygross-hive-tables].

**Õpetuse ettevalmistamiseks**

1. Avage Windows PowerShell ISE (Windows 8 avakuva, tippige **PowerShell_ISE**ja seejärel klõpsake nuppu **Windows PowerShell ISE**. Lisateavet leiate teemast [Windows PowerShelli käivitamine opsüsteemis Windows 8 ja Windows][powershell-start]).
2. Alumise paani, käivitage järgmine käsk Azure tellimuse loomiseks:

        Add-AzureAccount

    Teil palutakse sisestada mandaat Azure'i konto. See meetod tellimuse ühenduse ajalõpp ja 12 tunni pärast on teil käivitage cmdlet-käsk uuesti.

    > [AZURE.NOTE] Kui teil on mitu Azure tellimust ja vaikimisi tellimus pole see, mida soovite kasutada, <strong>Valige-AzureSubscription</strong> cmdlet-käsu abil saate valida tellimust.

3. Kopeerige järgmine skript skripti paani ja seejärel seadke esmalt kuus muutujate:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Muutujate rohkem kirjeldused leiate jaotisest [eeltingimused](#prerequisites) selles õpetuses.

3. Lisab skripti paanil skripti:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Klõpsake nuppu **Käivita skript** või vajutage **klahvi F5** skripti käivitamiseks. Väljund on sarnane:

    ![Väljundi kuueosalisest ettevalmistamine][img-preparation-output]

##<a name="run-the-oozie-project"></a>Käivitage Oozie projekt

Azure'i PowerShelli praegu ei paku mis tahes cmdlettide määratlemiseks Oozie tööde haldamine. **Invoke-RestMethod** cmdlet-käsu abil saate autonoomsest Oozie veebiteenused. Oozie web services API on HTTP ülejäänud JSON API. Oozie veebiteenuste API kohta leiate lisateavet teemast [Apache Oozie 4.0 dokumentatsiooni] [ apache-oozie-400] (jaoks Hdinsightiga kobar versiooni 3.0) või [Apache Oozie 3.3.2 dokumentatsiooni] [ apache-oozie-332] (Hdinsightiga kobar versioon 2.1) jaoks.

**Esitada mõne Oozie töö**

1. Avage Windows PowerShell ISE (Windows 8 avakuval, tippige **PowerShell_ISE**ja seejärel klõpsake nuppu **Windows PowerShell ISE**. Lisateavet leiate teemast [Windows PowerShelli käivitamine opsüsteemis Windows 8 ja Windows][powershell-start]).

3. Kopeerige järgmine skript skripti paani ja seejärel seadke esmalt 14 muutujate (siiski vahele jätta **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Muutujate rohkem kirjeldused leiate jaotisest [eeltingimused](#prerequisites) selles õpetuses.

    $coordstart ja $coordend on töövoo algus- ja lõppaeg. UTC/GMT aega leiate otsimine veebisaidil bing.com "utc aega". Funktsiooni $coordFrequency on sageduse minutites, mida soovite töövoo käivitada.

3. Lisab skripti. See osa määratleb Oozie last:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Suur erinevus võrreldes töövoo esitamise last fail on muutuv **oozie.coord.application.path**. Kui saadate töövoo töö, kasutage **oozie.wf.application.path** selle asemel.

4. Lisab skripti. See osa kontrollib Oozie web teenuse olek.

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Lisab skripti. See osa loob mõne Oozie töö.

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Kui saadate töövoo töö, peate tegema mõne muu veebiteenuse kõne alustada tööd pärast töö loomist. Sel juhul käivitab koordinaator töö aeg. Töö käivitub automaatselt.

6. Lisab skripti. See osa kontrollib Oozie töö olek.

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Valikuline) Lisab skripti.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Lisab skripti:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Eemaldada # märke, kui soovite käivitada veel funktsioone.

7. Kui klaster Hdinsightiga versioon 2.1, asendage "https://$clusterName.azurehdinsight.net:443/oozie/v2/" ja "https://$clusterName.azurehdinsight.net:443/oozie/v1/". Hdinsightiga kobar versioon 2.1 ei mitte veebiteenuste toetab versiooni 2.

7. Klõpsake nuppu **Käivita skript** või vajutage **klahvi F5** skripti käivitamiseks. Väljund on sarnane:

    ![Õppeteema Käivita töövoog väljund][img-runworkflow-output]

8. Ühenduse loomine SQL-andmebaasiga, eksporditud andmete kuvamiseks.

**Töö tõrkelogi kontrollida**

Töövoo tõrkeotsinguks Oozie logifaili leiate C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log kobar headnode kaudu. RDP kohta leiate teavet teemast [portaalis Azure Hdinsightiga haldamise kogumite][hdinsight-admin-portal].

**Uuesti õpetuse**

Käivitage uuesti töövoo, peate tegema järgmist:

- Kustutage taru skripti väljundfail.
- Log4jLogsCount tabeli andmed kustutada.

Siin on näide Windows PowerShelli skripti, mille abil saate:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Järgmised sammud
Selles õpetuses soovite õpitut määratlemine on Oozie töövoog ja mõne Oozie koordinaator ja käivitamise on Oozie koordinaator töökohtade Azure PowerShelli abil. Lisateabe saamiseks lugege järgmisi artikleid:

- [Alustamine Hdinsightiga][hdinsight-get-started]
- [Azure'i bloobimälu abil Hdinsightiga][hdinsight-storage]
- [Azure'i PowerShelli abil Hdinsightiga haldamine][hdinsight-admin-powershell]
- [Laadi andmed Hdinsightiga][hdinsight-upload-data]
- [Hdinsightiga Sqoop kasutamine][hdinsight-use-sqoop]
- [Hdinsightiga taru kasutamine][hdinsight-use-hive]
- [Kasutage siga Hdinsightiga][hdinsight-use-pig]
- [Töötada Java MapReduce programmide Hdinsightiga][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
