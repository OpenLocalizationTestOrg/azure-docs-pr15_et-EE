<properties
    pageTitle="HBase õpetus: kasutamise alustamine rakenduses Hadoopi HBase | Microsoft Azure'i"
    description="Täitke selle HBase õpetuse Apache HBase Hadoopi rakenduses Hdinsightiga koos kasutamise alustamine. Tabelite loomine HBase kest ja nende päringu abil taru."
    keywords="Apache hbase hbase, Shellis hbase hbase õpetus"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase õpetus: Apache HBase koos Windowsi-põhiste Hadoopi rakenduses Hdinsightiga kasutamise alustamine

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Saate teada, kuidas luua Hdinsightiga HBase kogumite, HBase tabelite loomine ja päringu tabelite abil Apache taru. Üldteavet HBase vt [Hdinsightiga HBase ülevaade][hdinsight-hbase-overview].

Teave selles dokumendis on Windowsi-põhiste Hdinsightiga kogumite. Windowsi-põhiste kogumite kohta lisateabe saamiseks kasutage menüü Vaateselektori lehe ülaosas minna.

> [AZURE.NOTE] Klõpsake Windowsi-põhiste Hdinsightiga HBase (versioon 0.98.0) on saadaval Hdinsightiga 3,1 kogumite kasutamiseks ainult (Apache Hadoop ja LÕNG 2.4.0 põhjal). Versiooniteabe, lugege teemat [mis on uut Hadoopi kobar versioonides esitatud Hdinsightiga?][hdinsight-versions]

## <a name="before-you-begin"></a>Enne alustamist

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Enne alustamist õppeteema HBase, peab teil olema järgmised:

- **A Microsoft Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Visual Studio 2013 või suurem **töökoha** : juhised leiate teemast [Installida Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Looge HBase kobar

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Loomiseks on HBase kobar Azure portaali abil**

1. [Azure'i portaali]sisselogimine[azure-management-portal].
2. Klõpsake nuppu **Uus** või **+** ülemises vasakus nurgas ja seejärel käsku **andmete + Analytics**, **Hdinsightiga**.
3. Sisestage järgmised väärtused:

    - **Nime kobar** - sisestage tuvastamiseks see nimi.
    - **Tippige kobar** - valige **HBase**.
    - **Operatsioonisüsteemi kobar** - valige **Windowsi**.  Klaster Linux-põhine HBase loomise kohta leiate teemast [HBase õpetus: alustada Apache HBase kasutamist Hadoopi sisse Hdinsightiga (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Versiooni** – valige mõni HBase versioon.
    - **Tellimuse** – valige see loomiseks kasutatud Azure tellimuse.
    - **Ressursirühm** - luua uue Azure ressursirühma või valige olemasoleva. Lisateavet leiate teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md)
    - **Mandaadi** - põhise Windows kobar, saate luua kobar kasutaja (aka HTTP kasutaja, HTTP web kasutaja) ja kasutajale kaugtöölaua. Klõpsake nuppu **Luba kaugtöölaud** remote töölaua kasutajatunnust lisada. Järgmises jaotises nõuab RDP.
    - **Andmeallika** - Azure storage uue konto loomine või valige olemasoleva Azure storage konto klaster failisüsteemi Vaikimisi kasutatakse. Salvestusruumi konto vaikeasukoha määrab kobar asukoht asukoht. Salvestusruumi vaikekonto ja klaster peab asukohti sama andmekeskuse.
    - **Sõlm hinnad astme** – valige hulk piirkond servereid HBase kobar

        > [AZURE.WARNING] Kõrge HBase teenuste olemasolu, peate looma klaster, mis sisaldab vähemalt **kolm** sõlmed. See tagab, et kui ühe sõlme läheb alla, HBase andmed piirkondade on saadaval muude sõlmed.

        > Kui teil on Õppekeskuse HBase, alati kobar suurus 1 Valige ja kustutage klaster pärast iga abil vähendada.

    - **Valikuline konfigureerimine** - konfigureerimine Azure virtuaalse võrgu, konfigureerida skripti toiminguid ja lisada täiendavat salvestusruumi kontod.

4. Klõpsake nuppu **Loo**.

>[AZURE.NOTE] Pärast soovitud HBase kobar on kustutatud, saate luua teise HBase kobar sama vaikekonto salvestusruumi ja vaikimisi bloobimälu container abil. Uue kobar valivad kuni olete loonud algse kobar HBase tabelid. Vältida selliste vastuolude tekkimist, soovitame enne kustutamist klaster keelata HBase tabelid.

## <a name="create-tables-and-insert-data"></a>Tabelite loomine ja andmete sisestamine

Praegu on kaks viisi HBase juurdepääsu. Selles jaotises antakse ülevaade sellest, kasutades HBase. Järgmises jaotises antakse ülevaade sellest, kasutades .NET SDK.

Enamik inimesi, kuvatakse andmed tabelina vormindamine:

![hdinsightiga hbase tabelina esitatud andmed][img-hbase-sample-data-tabular]

Mis on BigTable rakendamise HBase, samad andmed näeb.

![hdinsightiga hbase bigtable andmed][img-hbase-sample-data-bigtable]

Seda teeme loogilisem pärast järgmise toimingu lõpetamist.  

**Kasutada HBase shell**

1. Kasutage RDP rakenduses Hdinsightiga klaster HBase ühendamiseks. RDP juhised leiate teemast [haldamine Hadoopi kogumite sisse portaalis Azure Hdinsightiga][hdinsight-manage-portal].
2. Klõpsake RDP-seansi jooksul **Hadoopi käsurea** asub töölaua otsetee.
3. Avage HBase shell.

        cd %HBASE_HOME%\bin
        hbase shell

4. Mõne HBase koos kahe veeru loomine

        create 'Contacts', 'Personal', 'Office'
        list
5. Mõned andmed lisamiseks tehke järgmist.

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsightiga Hadoopi hbase shell][img-hbase-shell]

6. Ühe rea hankimine

        get 'Contacts', '1000'

    Näete sama tulemuse nagu skannimine käsu abil, kuna on ainult üks rida.

    Hbase tabeli skeemi kohta leiate lisateavet teemast [Sissejuhatus HBase skeemi kujundus][hbase-schema]. Rohkemate käskude HBase leiate [Apache HBase Lühijuhend][hbase-quick-start].


6. Väljuge shell

        exit

**Laadi andmeid hulgi kontaktide HBase tabelisse**

HBase sisaldab tabelitesse andmete laadimine on mitu võimalust. Lisateavet leiate teemast [hulgi laadimine](http://hbase.apache.org/book.html#arch.bulk.load).


Näidisandmete fail on üles laaditud avaliku bloobimälu container, wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Faili sisu on:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Saate luua tekstifaili ja faili üleslaadimine oma salvestusruumi konto, kui soovite. Juhised leiate teemast, [üleslaadimine andmete Hadoopi töökohta, Hdinsightiga][hdinsight-upload-data].

> [AZURE.NOTE] Seda toimingut kasutatakse kontaktide HBase tabeli viimase toimingu loodud.

1. Klõpsake RDP-seansi jooksul **Hadoopi käsurea** asub töölaua otsetee.
2. Muuda kaust:

        cd %HBASE_HOME%\bin

3. Muuta andmete faili StoreFiles ja poe Dimporttsv.bulk.output määratud suhteline tee järgmine käsk:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Üles laadida /example/data/storeDataFileOutput andmed tabelisse HBase järgmine käsk:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Saate avada HBase shell ja skannimine käsu abil saate loetleda tabeli sisu.



## <a name="use-hive-to-query-hbase-tables"></a>Päringu HBase tabelite taru abil

Andmed salvestatakse HBase taru abil saate teha päringuid. Selles jaotises loob taru tabeli, mis HBase tabelisse kaartide ja kasutab seda andmepäringu HBase tabelis.

**Armatuurlaua kobar avamine**

1. Liikuge sirvides **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Sisestage Hadoopi kasutaja konto kasutajanimi ja parool. Vaikimisi kasutajanimi on **admin** ja parool on sisestatud loomisprotsessi ajal. Avatakse brauseris uus vahekaart.
6. Klõpsake nuppu **Redaktori taru** lehe ülaosas. Taru redaktoris näeb välja umbes järgmine:

    ![Hdinsightiga kobar armatuurlaud.][img-hdinsight-hbase-hive-editor]

**Taru päringuid käivitada**

1. Sisestamine järgmist HiveQL script Editor taru ja klõpsake nuppu **Edasta** taru tabeli, mis kaardid HBase tabeli loomiseks. Veenduge, et enda loodud näidistabeli viidatud selles õpetuses enne, kui käivitate selle lause HBase shell abil.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Oodake, kuni **Status** updates **lõpule**.

2. Sisestage järgmine HiveQL skript taru redaktor ja klõpsake nuppu **Edasta**. Päringu taru päringute HBase tabeli andmed:

        SELECT count(*) FROM hbasecontacts;

4. Taru päringu tulemused toomiseks linki **Kuva üksikasjad** **Töö seansi** aknas kui lõpetab töö töötab. Seal on ainult üks töö väljundfail kuna üks kirje pannakse HBase tabel.




**Sirvida väljundfail**

1. Päringu konsooli, klõpsake **Faili brauseris**.
2. Klõpsake Azure storage konto, mida kasutatakse failisüsteemi HBase kobar.
3. Klõpsake HBase kobar nime. Vaikimisi Azure storage konto container kasutab kobar nime.
4. Klõpsake **kasutaja**ja seejärel klõpsake nuppu **administraator**. (See on kasutajanimi Hadoopi.)
6. Klõpsake **Viimati muudetud** aega, mis vastab teie aega, kui päring valige taru parandusfunktsiooni töö nime.
4. Klõpsake **stdout**. Salvestage fail ja avage fail Notepad. On ühe väljundfail.

    ![Hdinsightiga HBase taru Editor faili brauseris][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Kasutage .NET HBase REST API kliendi teek

Peate alla laadima HBase REST API kliendi teek .net-i kaudu GitHub ja koostada projekt, nii et saate kasutada HBase .NET SDK. Järgmise toimingu sisaldab selle toimingu juhiseid.

1. Loo uus C# Visual Studio Windowsi töölaua konsooli rakendus.
2. Nugeti Package Manager konsooli avamiseks klõpsake **Tööriistad** > **Nugeti Package Manager** > **Package Manager konsooli**.
3. Käivitage järgmine käsk Nugeti konsooli:

        Install-Package Microsoft.HBase.Client

5. Lisage faili ülaosas **abil** järgmistest.

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Asendage funktsiooni **Main** järgmist:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Funktsiooni **Main** kolm esimest muutujate kogum.
8. Vajutage klahvi **F5** käivitage rakendus.

## <a name="check-cluster-status"></a>Kobar oleku kontrollimine

HBase sisse Hdinsightiga on Web UI kaasas kogumite jälgimiseks. Kasuta kasutajaliides Web, saate taotleda statistika või piirkondade teavet.

Kasutajaliides Web avamiseks peate RDP üheks kobar, ja seejärel klõpsake töölaua otsetee HMaster teave Web UI või kasutada veebibrauseris järgmine URL:

    http://zookeeper[0-2]:60010/master-status

Kõrge-saadavus kobar, leiate praeguse aktiivse HBase juhtslaidi sõlme hostib kasutajaliides Web link.

##<a name="delete-the-cluster"></a>Klaster kustutamine
Vältida selliste vastuolude tekkimist, soovitame enne kustutamist klaster keelata HBase tabelid.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Mis saab edasi?
Hdinsightiga HBase selles õpetuses, saate teada, kuidas luua mõne HBase kobar ja tabelite loomine ja andmete kuvamiseks nende tabelite HBase kest. Saate ka teada, kuidas kasutada taru päringu HBase tabelite ja HBase tabeli loomine ja andmete toomiseks tabeli HBase C# REST API-de kasutamine andmete.

Lisateavet leiate teemast:

- [Hdinsightiga HBase ülevaade][hdinsight-hbase-overview].
HBase on Apache, avatud lähtekoodi NoSQL andmebaasi tugineb Hadoopi, mis pakub muutmälu ja tugev järjepidevuse suurte andmehulkade struktureerimata ja semistructured.
- [Luua HBase kogumite Azure virtuaalse võrgu][hdinsight-hbase-provision-vnet].
Virtuaalse võrgu integreerimise, saate HBase kogumite kasutusele võtta rakenduste virtuaalse samasse võrku nii, et rakendused ei saa suhelda HBase otse.
- [HBase konfigureerimine paljundamine Hdinsightiga](hdinsight-hbase-geo-replication.md). Saate teada, kuidas konfigureerida HBase dispersioonanalüüs üle kahe Azure andmekeskuste.
- [Twitteri meeleolu HBase sisse Hdinsightiga koos analüüsida][hbase-twitter-sentiment].
Saate teada, kuidas teha reaalajas [meeleolu analüüsi](http://en.wikipedia.org/wiki/Sentiment_analysis) suurte Hadoopi kobar sisse Hdinsightiga HBase abil.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
