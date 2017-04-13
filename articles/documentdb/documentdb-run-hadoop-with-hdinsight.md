<properties
    pageTitle="Hadoopi töö DocumentDB ja HDInsight abil | Microsoft Azure'i"
    description="Saate teada, kuidas käivitada lihtsa taru, siga ja MapReduce töö DocumentDB ja Azure Hdinsighti."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Hadoopi töö DocumentDB ja Hdinsightiga abil

Selle õpetuse näitab, kuidas käivitada [Apache taru][apache-hive], [Apache siga][apache-pig], ja [Apache Hadoop] [ apache-hadoop] MapReduce DocumentDB's Hadoopi Connectori Windows Azure Hdinsightiga tööd. DocumentDB's Hadoopi connector võimaldab DocumentDB toimida nii lähte- ja taru, siga ja MapReduce töö jaoks. Selle õpetuse kasutab DocumentDB nii andmete lähte-kui ka Hadoopi töökohta.

Pärast selle õpetuse, on teil saama vastavad järgmistele küsimustele.

- Kuidas DocumentDB taru, siga või MapReduce töö abil andmete laadida?
- Kuidas DocumentDB taru, siga või MapReduce töö abil andmete talletamiseks?

Soovitame, jälgides järgmisest videost, kus oleme läbida taru tööd DocumentDB ja HDInsight kasutamise alustamine.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Tagastab funktsioon selle artikli kui saate kõiki üksikasju kohta, kuidas saate teha analytics töö DocumentDB oma andmete põhjal.

> [AZURE.TIP] Selle õpetuse eeldab, et teil on eelnev kogemus Apache Hadoop, taru ja/või siga abil. Kui olete uus Apache Hadoop, taru ja siga, soovitame külastamise [Apache Hadoop dokumentatsiooni][apache-hadoop-doc]. Selle õpetuse eeldab, et teil on eelnev kogemus DocumentDB ja DocumentDB kontot. Kui teil on uus DocumentDB või teil on DocumentDB konto, palun vaadake meie [Alustamine] [ getting-started] lehe.

Ei ole aega õpetuse ja soovite lihtsalt taru, siga ja MapReduce täielik PowerShelli näidisskriptid saamiseks? Pole probleem, neid saada [siin][documentdb-hdinsight-samples]. Allalaadimine sisaldab ka need näidised hql, siga ja java faile.

## <a name="NewestVersion"></a>Uusim versioon

<table border='1'>
    <tr><th>Hadoopi konnektor versioon</th>
        <td>1.2.0</td></tr>
    <tr><th>Skripti Uri</th>
        <td>https://portalcontent.blob.Core.Windows.net/scriptaction/documentdb-Hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Muutmiskuupäev</th>
        <td>26-04-2016</td></tr>
    <tr><th>Toetatud Hdinsightiga versioonid</th>
        <td>3,1, 3,2</td></tr>
    <tr><th>Muutmine Logi</th>
        <td>Värskendatud DocumentDB Java SDK 1.6.0</br>
            Nii lähte-ja valamu sektsioonitud saidikogumid lisatud tugi</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Eeltingimused
Enne juhiste selles õpetuses, veenduge, et on järgmine:

- DocumentDB konto, andmebaasi ja kogumi dokumentidega sees. Lisateabe saamiseks lugege teemat [Alustamine DocumentDB][getting-started]. Näidisandmete abil [DocumentDB importimine tööriista]DocumentDB kontole importimiseks[documentdb-import-data].
- Läbilaskevõime. Loeb ja kirjutab hdinsightist loendatakse teie määratud taotluse üksused teie saidikogumid suunas. Lisateabe saamiseks vt [Provisioned läbilaskevõime, taotluse üksused, ja andmebaasi toimingud][documentdb-manage-throughput].
- Võimsus täiendavad salvestatud iga kord väljund saidikogumi. Salvestatud toimingute kasutatakse saadud dokumentide edastamise. Lisateavet leiate teemadest [saidikogumite ja ettevalmistatud läbilaskevõime][documentdb-manage-document-storage].
- Taru, siga või MapReduce töökohta tulemuseks dokumentide maht. Lisateabe saamiseks vt [DocumentDB haldamine ja tulemuslikkuse][documentdb-manage-collections].
- [*Valikulised*] Kogumise täiendav võimsus. Lisateabe saamiseks vt [Provisioned dokumendi salvestamiseks ja index kohal][documentdb-manage-document-storage].

> [AZURE.WARNING] Uue saidikogumi tööd, mis tahes ajal loomine vältimiseks saate saate printida stdout tulemid, väljund salvestada oma WASB container või määrata mõne olemasoleva saidikogumi. Puhul mõne olemasoleva saidikogumi määramisele uusi dokumente luuakse kogumine ja juba olemasolevate dokumentide ainult mõjutab, kui seal on konflikti *ID-d*. **Konnektor automaatselt kirjutab olemasolevad dokumendid id konflikte**. Te saate selle funktsiooni välja lülitada, määrates suvandi upsert väärtuseks väär. Kui upsert on false ja konflikt ilmneb juhul, nurjub Hadoopi töö; aruandlusteenuste id konflikti tõrge.


## <a name="ProvisionHDInsight"></a>Samm 1: Loo uus Hdinsightiga kobar
Selles õpetuses kasutab skripti toimingu portaalist Azure Hdinsightiga klaster kohandamiseks. Selles õpetuses kasutame portaalis Azure Hdinsightiga klaster loomiseks. Juhised, kuidas kasutada PowerShelli cmdlet-käskude või Hdinsightiga .NET SDK, vaadake [kohandamine Hdinsightiga kogumite skripti toimingu abil] [ hdinsight-custom-provision] artikkel.

1. [Azure'i portaali]sisselogimine[azure-portal].

2. Valige **+ Uus** vasakpoolsel navigeerimisalal otsimine **Hdinsightiga** ülemise otsinguriba uue enne peal.

3. **Hdinsightiga** avaldatud **Microsoft** kuvatakse tulemite ülaosas. Klõpsake seda ja seejärel klõpsake nuppu **Loo**.

4. Uue Hdinsightiga klaster loomine blade, sisestage oma **Kobar nimi** ja valige **tellimus** , mida soovite selle ressursi jaotises ettevalmistamise.

    <table border='1'>
        <tr><td>Kobar nimi</td><td>Klaster nimi.<br/>
   DNS-i nimi peab käivitamine ja lõppevad mõne märgi arvulise alfa võib sisaldada kriipsud.<br/>
   Väli peab olema stringi 3 kuni 63 märki.</td></tr>
        <tr><td>Tellimuse nimi</td>
            <td>Kui teil on rohkem kui üks Azure'i tellimus, valige tellimus, Hdinsightiga klaster majutavad. </td></tr>
    </table>

5. Klõpsake **Kobar tüübi valimine** ja määrake järgmised atribuudid määratud väärtused.

    <table border='1'>
        <tr><td>Kobar tüüp</td><td><strong>Hadoopi</strong></td></tr>
        <tr><td>Taseme kobar</td><td><strong>Standard</strong></td></tr>
        <tr><td>Operatsioonisüsteem</td><td><strong>Windows</strong></td></tr>
        <tr><td>Versioon</td><td>uusim versioon</td></tr>
    </table>

    Nüüd, klõpsake nuppu, **Valige**.

    ![Hadoopi Hdinsightiga algse kobar üksikasjad][image-customprovision-page1]

6. Klõpsake **mandaadi** login ja Kaugpöördusteenuse mandaadi määramiseks. Valige oma **Kobar sisselogimise kasutajanimi** ja **Kobar sisselogimise parool**.

    Kui soovite remote klaster sisse, valige *Jah* tera allosas ja sisestada kasutajanimi ja parool.

7. Klõpsake **Andmeallika** andmetele juurdepääsu oma peamine asukoha määramiseks. Valige **Valiku meetod** ja määrata juba olemasoleva salvestusruumi konto või looge uus.

8. Sama enne määrata **Vaikimisi Container** ja **asukoht**. Ja klõpsake nuppu **Vali**.

    > [AZURE.NOTE] Valige oma DocumentDB konto piirkond parema jõudluse lähedal

8. **Hinnakirjad** arv ja tüüp sõlmed valimiseks klõpsake nuppu. Saate säilitada vaikimisi konfiguratsiooni ja mastaapimiseks töötaja sõlmed arvu uuem versioon.

9. Klõpsake nuppu **valikuline konfiguratsioon**, siis valikuline konfigureerimine tera **skripti toimingud** .

    Skripti toimingud, sisestage järgmine teave kohandamiseks HDInsight klaster.

    <table border='1'>
        <tr><th>Atribuut</th><th>Väärtus</th></tr>
        <tr><td>Nimi</td>
            <td>Määrake skripti toimingu nimi.</td></tr>
        <tr><td>Skripti URI</td>
            <td>Määrake URI skripti, mis on vaja järgida klaster kohandamiseks.</br></br>
   Sisestage: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Juhi</td>
            <td>Märkige ruut peale pea sõlme PowerShelli skripti käivitamiseks.</br></br>
            <strong>Märkige see ruut</strong>.</td></tr>
        <tr><td>Töötaja</td>
            <td>Märkige ruut peale töötaja sõlme PowerShelli skripti käivitamiseks.</br></br>
            <strong>Märkige see ruut</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Märkige ruut selle Zookeeper peale PowerShelli skripti käivitamiseks.</br></br>
            <strong>Pole vaja</strong>.
            </td></tr>
        <tr><td>Parameetrid</td>
            <td>Määrake soovitud parameetrid skripti vajadusel.</br></br>
            <strong>Parameetrite ei vaja</strong>.</td></tr>
    </table>

10. Kas uue **Ressursirühma** või kasutage jaotises tellimuse Azure'i ressursi olemasolevasse rühma loomine.

11. Nüüd, märkige ruut **Kinnita armatuurlaua** jälgida oma juurutamise ja klõpsake nuppu **Loo**!

## <a name="InstallCmdlets"></a>Samm 2: Installida ja konfigureerida Azure PowerShell

1. Installige Azure'i PowerShelli. Juhised leiate [siit][powershell-install-configure].

    > [AZURE.NOTE] Teise võimalusena ainult jaoks taru päringuid, saate kasutada Hdinsightile oma online taru redaktor. [Azure portaali]sisselogimine selleks[azure-portal], klõpsake **Hdinsightile** oma Hdinsightiga kogumite loendi kuvamiseks vasakpoolsel paanil. Klõpsake klaster soovite käivitada taru päringud ja klõpsake **Päringu konsooli**.

2. Avage Azure'i PowerShelli integreeritud skriptimine keskkonnas.
    - Arvutis, kus töötab Windows 8 või Windows Server 2012 või uuem versioon, saate kasutada sisseehitatud otsing. Avakuvale ja tippige **PowerShelli ise** ja klõpsake nuppu **Sisesta**.
    - Varem Windows 8 või Windows Server 2012 versiooniga arvutis, kasutage menüü Start. Klõpsake menüü Start **Käsuviip** tippige otsinguväljale ja seejärel klõpsake tulemiloendis, valige **Käsuviip**. Käsuviip, tippige **powershell_ise** ja klõpsake nuppu **Sisesta**.

3. Azure'i konto lisamine.
    1. Paanil konsooli tippige **Lisa-AzureAccount** ja klõpsake nuppu **Sisesta**.
    2. Tippige oma Azure tellimusega seostatud meiliaadress ja klõpsake nuppu **Jätka**.
    3. Tippige väljale parool Azure tellimuse.
    4. Klõpsake nuppu **Logi sisse**.

4. Järgmisel joonisel tuvastab Azure PowerShelli skriptimise keskkonna oluline osa.

    ![Azure'i PowerShelli diagramm][azure-powershell-diagram]

## <a name="RunHive"></a>Samm 3: Käivitada taru töö DocumentDB ja Hdinsightiga abil

> [AZURE.IMPORTANT] Kõik muutujad, mis on tähistatud < > peab olema täidetud abil sätete konfigureerimine.

1. Määrake järgmised muutujad oma PowerShelli skripti paanil.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Alustame ehitamine Otsitav. Me kirjutada taru päring, mis suunab kõigi dokumentide süsteemi genereeritud ajatemplid (_ts) ja kordumatu ID (_rid) DocumentDB kollektsioonist langeks kõigi dokumentide minutite ja seejärel salvestab tulemused tagasi uue DocumentDB saidikogumi.</p>

    <p>Esmalt meie DocumentDB kollektsioonist taru tabeli loomine. Lisada järgmised koodilõigu soovitud PowerShelli skripti paani <strong>pärast</strong> koodilõigu # 1. Veenduge, et kaasata valikuline DocumentDB.query parameetri t trim meie dokumendid lihtsalt _ts ja _rid.</p>

    > [AZURE.NOTE]**Nime panemise DocumentDB.inputCollections ei vea.** Jah, me lubada lisada mitu saidikogumite sisendina: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Järgmiseks väljundi kogumi taru tabeli loomine. Väljundi dokumendi atribuudid on kuu, päev, tunni, minuti ja sündmuste arv.

    > [AZURE.NOTE]**Taas nimetamine DocumentDB.outputCollections oli mitte viga.** Jah, me lubada lisada mitu saidikogumite väljund: </br>
"*DocumentDB.outputCollections*"="*\<DocumentDB väljund saidikogumi nimi 1\>*,*\<DocumentDB väljund saidikogumi nimi 2\>*" </br> Saidikogumi nimed on ilma tühikuteta, kasutades ainult ühe komaga eraldatud. </br></br>
Dokumendid on jaotatud round-jaan üle mitme saidikogumid. Paketi dokumendid on salvestatud ühe saidikogumi, siis teine paketi dokumentide salvestatakse järgmise kogumise ja jne.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Lõpuks loome ühtivad dokumendid, kuu, päev, tunni ja minutiga ja tulemuste uuesti lisada väljund taru tabel.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Lisage järgmised skripti koodilõigu taru töö määratlus eelmise päringu loomiseks.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Saate kasutada ka HiveQL skripti faili HDFS-faili lüliti.

6. Lisage järgmised koodilõigu salvestamiseks algusaeg ja esitage taru töö.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Lisage järgmine ootama taru töö lõpuleviimiseks.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Lisage järgmine printida väljundit ja algus- ja lõpukellaajad.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Käivitage** oma uus! **Klõpsake** nuppu roheline käivitamine.

10. Märkige ruut tulemused. [Azure'i portaali]sisselogimine[azure-portal].
    1. Klõpsake vasakul paanil nuppu <strong>Sirvi</strong> . </br>
    2. Klõpsake nuppu <strong>Kõik</strong> Sirvi paani paremas ülanurgas. </br>
    3. Otsige ja valige <strong>DocumentDB kontod</strong>. </br>
    4. Järgmisena otsige <strong>DocumentDB konto</strong>ja seejärel <strong>DocumentDB andmebaas</strong> ja <strong>DocumentDB saidikogumi</strong> seostatud määratud taru päringu väljund kogumist.</br>
    5. Lõpuks klõpsake <strong>Dokumendi Exploreri</strong> all <strong>Arendaja tööriistad</strong>.</br></p>

    Näete oma taru päringu tulemused.

    ![Päringutulemite taru][image-hive-query-results]

## <a name="RunPig"></a>Samm 4: Käivitada siga töö DocumentDB ja HDInsight abil

> [AZURE.IMPORTANT] Kõik muutujad, mis on tähistatud < > peab olema täidetud abil sätete konfigureerimine.

1. Määrake järgmised muutujad oma PowerShelli skripti paanil.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Alustame ehitamine Otsitav. Me kirjutada siga päring, mis suunab kõik dokumendid süsteemi genereeritud ajatemplid (_ts) ja kordumatu ID (_rid) DocumentDB kollektsioonist langeks kõik dokumendid, minutid ja siis salvestab tulemused tagasi uue DocumentDB saidikogumi.</p>
    <p>Esmalt asetage dokumendid kaudu DocumentDB Hdinsightiga. Lisada järgmised koodilõigu soovitud PowerShelli skripti paani <strong>pärast</strong> koodilõigu # 1. Veenduge, et lisada DocumentDB päringu valikuline DocumentDB Päringuparameetri meie dokumendid lihtsalt _ts trimmimine ja _rid.</p>

    > [AZURE.NOTE]Jah, me lubada lisada mitu saidikogumite sisendina: </br>
"*\<DocumentDB sisestatud nimi 1\>*,*\<DocumentDB sisestatud nimi 2\>*"</br> Saidikogumi nimed on ilma tühikuteta, kasutades ainult ühe komaga eraldatud. </b>

    Dokumendid on jaotatud round-jaan üle mitme saidikogumid. Paketi dokumendid on salvestatud ühe saidikogumi, siis teine paketi dokumentide salvestatakse järgmise kogumise ja jne.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Järgmiseks loome ühtivad dokumendid, kuu, päev, tunni, minuti ja sündmuste arv.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Lõpuks vaatame talletage meie uut väljundi kollektsiooni.

    > [AZURE.NOTE]Jah, me lubada lisada mitu saidikogumite väljund: </br>
"\<DocumentDB väljund saidikogumi nimi 1\>,\<DocumentDB väljund saidikogumi nimi 2\>"</br> Saidikogumi nimed on ilma tühikuteta, kasutades ainult ühe komaga eraldatud.</br>
Dokumendid on jaotatud round-jaan üle mitme saidikogumid. Paketi dokumendid on salvestatud ühe saidikogumi, siis teine paketi dokumentide salvestatakse järgmise kogumise ja jne.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Lisage järgmised skripti koodilõigu siga töö määratlus eelmise päringu loomiseks.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Saate kasutada ka siga skripti faili HDFS-faili lüliti.

6. Lisage järgmised koodilõigu salvestamiseks algusaeg ja esitage siga töö.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Lisage järgmine ootama siga töö lõpuleviimiseks.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Lisage järgmine printida väljundit ja algus- ja lõpukellaajad.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Käivitage** oma uus! **Klõpsake** nuppu roheline käivitamine.

10. Märkige ruut tulemused. [Azure'i portaali]sisselogimine[azure-portal].
    1. Klõpsake vasakul paanil nuppu <strong>Sirvi</strong> . </br>
    2. Klõpsake nuppu <strong>Kõik</strong> Sirvi paani paremas ülanurgas. </br>
    3. Otsige ja valige <strong>DocumentDB kontod</strong>. </br>
    4. Järgmisena otsige <strong>DocumentDB konto</strong>ja seejärel <strong>DocumentDB andmebaas</strong> ja <strong>DocumentDB saidikogumi</strong> seostatud määratud siga päringu väljund kogumist.</br>
    5. Lõpuks klõpsake <strong>Dokumendi Exploreri</strong> all <strong>Arendaja tööriistad</strong>.</br></p>

    Näete oma siga päringu tulemused.

    ![Päringutulemite siga][image-pig-query-results]

## <a name="RunMapReduce"></a>Juhis 5: Käivitada MapReduce töö DocumentDB ja Hdinsightiga abil

1. Määrake järgmised muutujad oma PowerShelli skripti paanil.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Me MapReduce tööd, mis langeks esinemiskordade iga dokumendi atribuudi oma DocumentDB kollektsioonist saate käivitada. Lisage see skripti koodilõigu **pärast** ülaltoodud koodilõigu.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties-v01.jar on kohandatud installi DocumentDB Hadoopi konnektor.

3. Lisage järgmine käsk MapReduce töö esitada.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Lisaks MapReduce töö määratlus, annate HDInsight kobar nime, kuhu soovite käivitada MapReduce töö ja mandaadi. Algus-AzureHDInsightJob on asynchronized tegemine. Töö lõpetamise, kasutage *Ootamine-AzureHDInsightJob* cmdlet-käsk.

4. Lisage järgmine käsk koos MapReduce töö vigu kontrollida.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Käivitage** oma uus! **Klõpsake** nuppu roheline käivitamine.

6. Märkige ruut tulemused. [Azure'i portaali]sisselogimine[azure-portal].
    1. Klõpsake vasakul paanil nuppu <strong>Sirvi</strong> .
    2. Klõpsake nuppu <strong>Kõik</strong> Sirvi paani paremas ülanurgas.
    3. Otsige ja valige <strong>DocumentDB kontod</strong>.
    4. Järgmisena otsige <strong>DocumentDB konto</strong>ja seejärel <strong>DocumentDB andmebaas</strong> ja <strong>DocumentDB saidikogumi</strong> seostatud tööpäevad MapReduce määratud väljundi kogumist.
    5. Lõpuks klõpsake <strong>Dokumendi Exploreri</strong> all <strong>Arendaja tööriistad</strong>.

    Kuvatakse tulemused MapReduce tööpäevad.

    ![MapReduce Päringutulemid][image-mapreduce-query-results]

## <a name="NextSteps"></a>Järgmised sammud

Palju õnne! Käivitasite just teie esimesed taru, siga ja MapReduce tööd Azure'i DocumentDB ja Hdinsighti kaudu.

Meil on avatud hangitud meie Hadoopi konnektor. Kui olete huvitatud, saate kaasa [github][documentdb-github].

Lisateabe saamiseks lugege järgmisi artikleid:

- [Java rakendus Documentdb koos töötada][documentdb-java-application]
- [Hadoopi rakenduses HDInsight arendada Java MapReduce programmid][hdinsight-develop-deploy-java-mapreduce]
- [Alustamine Hadoopi kasutamine rakenduses Hdinsightiga taru analüüsimiseks mobiiltelefoni kasutamine][hdinsight-get-started]
- [Hdinsightiga MapReduce kasutamine][hdinsight-use-mapreduce]
- [Hdinsightiga taru kasutamine][hdinsight-use-hive]
- [Kasutage siga Hdinsightiga][hdinsight-use-pig]
- [Kohandada HDInsight kogumite skripti toimingu abil][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
