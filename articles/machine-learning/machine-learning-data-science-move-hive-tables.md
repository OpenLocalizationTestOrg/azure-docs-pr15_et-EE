<properties
    pageTitle="Loomine ja andmete laadimine taru tabelite bloobimälu | Microsoft Azure'i"
    description="Taru tabelite loomine ja bloobimälu taustal tabelite andmete laadimine"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Loomine ja andmete laadimine taru tabelite Azure'i bloobimälu

Sellest teemast leiate üldise taru päringud, mis taru tabelite loomine ja andmete laadimine: Azure'i bloobimälu. Mõned juhised on olemas eraldamine taru tabelid ja optimeeritud rea piklikku (VÕIC) päringu jõudluse parandamiseks vormingu abil.

See **menüü** linke teemadele, kus kirjeldatakse, kuidas neelata andmed üheks target keskkonnas, kus saate andmeid talletada ja töödelda ajal meeskonnatöö andmete teadus protsess (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on:

* Loodud Azure storage konto. Kui vajate juhiseid, lugege teemat [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md). 
* Ette valmistatud kohandatud Hadoopi kobar Hdinsightiga teenusega.  Kui vajate juhiseid, lugege teemat [kohandamine Azure Hdinsightiga Hadoopi kogumite täiustatud analüüsi jaoks](machine-learning-data-science-customize-hadoop-cluster.md).
* Lubatud Kaugpöördusteenuse klaster, sisse logitud ja avada Hadoopi käsurea konsooli. Kui vajate juhiseid, lugege teemat [Accessi pea sõlm Hadoopi kobar](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Laadi andmed Azure'i bloobimälu
Kui olete loonud mõne Azure virtuaalse masina [häälestamine on Azure virtuaalse masina täiustatud analüüsi jaoks](machine-learning-data-science-setup-virtual-machine.md)juhiseid järgides, seda skriptifail peaks on laaditud soovitud *C:\\kasutajate\\\<kasutajanimi\>\\dokumentide\\andmete teadus skriptide* kataloogi virtuaalse masina. Need taru päringud ainult jaoks on vaja ühendate oma andmete skeemi ja Azure'i bloobimälu salvestusruumi konfiguratsiooni väljadele olema esitamiseks valmis.

Me oletame, et taru tabelite andmeid on **tihendamata** tabeli vormingus ja, et andmed on üles laaditud vaikimisi (või täiendava) container kasutatavaid Hadoopi kobar salvestusruumi konto.

Kui soovite rakendada **NYC takso reisi andmed**, peate:

- **allalaadimine** on 24 [NYC takso reisi](http://www.andresmh.com/nyctaxitrips) andmefailid (12 reisi failide ja 12 hind failid)
- **pakkige see lahti** kõik failid CSV-failid ja seejärel
- **üles laadida** neid korras loodud Azure storage konto vaikimisi (või vastav container) liigendatud [kohandamine Azure Hdinsightiga Hadoopi kogumite Täpsemad Analytics protsessi ja tehnoloogia](machine-learning-data-science-customize-hadoop-cluster.md) teema. Protsessi üles laadida CSV-failid vaikimisi container salvestusruumi kontol leiate selle [lehe](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Kuidas esitada taru päringud

Saate esitada taru päringute abil:

1. [Esitage headnode Hadoopi klaster taru päringute Hadoopi käsurea kaudu](#headnode)
2. [Esitage taru päringute taru toimetaja](#hive-editor)
3. [Esitage taru päringute Azure PowerShelli käskude abil](#ps)

Taru päringud on SQL-like. Kui olete tuttav SQL-i, võite leida [taru SQL-i kasutajate Cheat lehel](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) kasulik.

Taru päringu esitamisel saate määrata taru päringud väljund sihtkohta, olgu see siis ekraanil või pea sõlme kohaliku faili või mõne Azure'i bloobimälu.


###<a name="headnode"></a>1. esitada headnode Hadoopi klaster taru päringute Hadoopi käsurea kaudu

Kui taru päring on keeruline, see esitada otse soovitud Hadoopi pea sõlme kobar tavaliselt viib kiiremini kui esitamist taru redigeerija või Azure PowerShelli skriptide abil ümber pöörata.

Hadoopi kobar pea sõlme sisse logida, Hadoop käsurea pea sõlme töölaual avatud ja sisestage käsk `cd %hive_home%\bin`.

On teil kolm võimalust taru päringute Hadoopi käsurea edastada.

* otse
* .hql failide kasutamine
* Taru käsk konsooli

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Esitage taru päringud otse sisse Hadoopi käsurea.

Käivitage käsk, näiteks `hive -e "<your hive query>;` taru lihtpäringuid otse sisse Hadoopi käsurea edastada. Siin on näide, kus punane väljal kirjeldatakse käsk esitab taru päringu ja roheline kast liigendab taru päringu väljund.

![Tööruumi loomine](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Taru päringute .hql failide edastamine

Kui taru päring on keerulisem ja on mitu rida, redigeerimise päringute käsurea või taru käsk konsooli ei ole praktiline. Alternatiiv on kasutada tekstiredaktoris Hadoopi kobar pea sõlme taru päringute kohalikku kausta pea sõlme .hql faili salvestada. Taru päringu .hql faili saate esitada, kasutades funktsiooni `-f` argumendi järgmiselt:

    hive -f "<path to the .hql file>"

![Tööruumi loomine](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Edenemise olek ekraani printimine taru päringute tõkestamine**

Vaikimisi pärast taru päring on esitatud Hadoopi käsurea edenemist MAPI/vähendada töö prinditakse ekraanil. Mis tahes Kuva Prindi MAPI/vähendada töö edenemisest, saate kasutada argumendina `-S` ("S" suurtähed) käsk joone järgmiselt:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Esitage taru päringute taru käsk konsooli.

Võite sisestada ka esmalt taru käsk konsooli käsu `hive` Hadoopi käsurea ja seejärel esitage taru päringute taru käsk konsooli. Siin on näide. Selles näites kaks punane lahtrid esile tõsta, saab sisestada taru käsk konsooli käsud ja taru päringu esitatud taru käsk konsooli vastavalt. Roheline ruut tõsta esile taru päringu väljund.

![Tööruumi loomine](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

Eelmistes näidetes väljund otse taru päringutulemite ekraanil. Saate kirjutada väljund kohaliku faili pea sõlme või mõne Azure'i bloobimälu. Seejärel saate kasutada muid tööriistu taru päringute väljund täpsemaks analüüsimiseks.

**Väljasta taru päringutulemite kohalik fail.**

Väljund taru päringutulemite pea sõlme kohalikku kausta, peate esitada päringu taru Hadoopi käsurea järgmiselt:

    hive -e "<hive query>" > <local path in the head node>

Järgmises näites on kirjutatud taru päringu väljund faili `hivequeryoutput.txt` kataloogis `C:\apps\temp`.

![Tööruumi loomine](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Väljundi taru päringutulemite mõne Azure'i bloobimälu**

Samuti saate väljund taru päringu tulemid Azure'i bloobimälu, vaikimisi container Hadoopi klaster sees. Taru päringu selleks on järgmine:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Järgmises näites on kirjutatud taru päringu väljund bloobimälu kataloogi `queryoutputdir` vaikimisi container Hadoopi klaster sees. Siin, peate ainult kausta nime, ilma bloobimälu nime andmiseks. Tõrge Expression.Error, kui annate nii kataloogi ja bloobimälu nimed, `wasb:///queryoutputdir/queryoutput.txt`.

![Tööruumi loomine](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Kui avate vaikimisi ümbris, Hadoop klaster Azure'i salvestusruumi Explorerit, näete väljund taru päringu, nagu on näidatud järgmisel joonisel. Saate rakendada filtri (rõhutatud punane kast) tuua ainult bloobimälu määratud tähtede nimed.

![Tööruumi loomine](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2 esitada taru päringute taru toimetaja

Võite kasutada ka päringu Console (taru Editor) sisestades URL-i vormi *https://&#60; Hadoopi kobar nimi >.azurehdinsight.net/Home/HiveEditor* veebibrauseris. Peate olema sisse loginud vt konsooli ja seega peate Hadoopi kobar identimisteave siin.

###<a name="ps"></a>3. Esitage taru päringute Azure PowerShelli käskude abil

PowerShelli abil saate esitada päringuid taru. Juhised leiate teemast [PowerShelli abil esitada taru tööde haldamine](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Taru andmebaasi ja tabelite loomine

Päringute taru jagataks [Github hoidla](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) ja seal saate alla laadida.

Siin on taru päring, mis loob taru tabeli.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Siin on kirjeldused välju, mida soovite ühendada ja muud konfiguratsioone.

- **& #60; andmebaasi nimi >**: nime andmebaas, mida soovite luua. Kui soovite lihtsalt kasutada vaikimisi andmebaasi, *luua andmebaasi...* päringu saate puudub.
- **& #60; tabeli nime >**: tabel, mille soovite luua määratud andmebaasi nimi. Kui soovite kasutada vaikimisi andmebaasi, tabeli saate otse läbivaatamiseks *& #60; tabeli nime >* ilma & #60; andmebaasi nimi >.
- **& #60; eraldaja >**: eraldav andmete faili üles laadida taru tabeli väljade eraldaja.
- **& #60; joon eraldaja >**: eraldaja eraldav andmefaili read.
- **& #60; talletuskoht >**: Azure'i talletuskoht taru tabelite andmed salvestada. Kui te ei määra *asukoht & #60; talletuskoht >*, andmebaas ja tabelid on talletatud *taru lao/* kataloog vaikimisi ümbrises taru klaster vaikimisi. Kui soovite määrata talletuskoht, talletuskoht peab olema sees vaikimisi container andmebaas ja tabelid. Selles asukohas on viidatavad kobar formaadis suhtes vaike-ümbrisest asukohaks *' wasb: / / / & #60; directory 1 > /'* või *' wasb: / / / & #60; directory 1 > / & #60; kausta 2 > /'*jne. Kui päring on täidetud, suhteline kataloogide luuakse vaikimisi pakendi sees.
- **TBLPROPERTIES("Skip.header.line.Count"="1")**: kui andmefaili on rea päise, peate lisama selle atribuudi **lõpus** päringu *tabeli loomine* . Muul juhul laaditakse kirjena tabelisse rea päise. Kui faili ei saa rea päise, saate selle konfiguratsiooni päringu välja jäetud.

## <a name="load-data"></a>Laadi andmete taru tabelitele
Siin on taru päring, mis laadib andmed tabelisse taru.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; tee bloobimälu andmete >**: kui bloobimälu faili üles laadida taru tabel on vaikimisi ümbrises Hdinsightiga Hadoopi klaster, on *& #60; tee bloobimälu andmed >* peaks olema vormingus *' wasb: / / / & #60; see ümbrises directory > / & #60; bloobimälu faili nimi >'*. Bloobimälu fail võib olla ka HDInsight Hadoopi klaster täiendavad ümbrises. Sel juhul *& #60; tee bloobimälu andmed >* peaks olema vormingus *' wasb: / / & #60; container name>@&#60;storage konto nimi >.blob.core.windows.net/ & #60; bloobimälu faili nimi >'*.

    >[AZURE.NOTE] Bloobimälu andmed üles laadida taru tabelis peab olema vaike- või täiendava salvestusruumi konto jaoks Hadoopi kobar ümbrises. Muul juhul *Laadi andmete* päring nurjub, kurdavad, et te ei pääse juurde andmed.


## <a name="partition-orc"></a>Täpsemad Teemad: liigendatud tabeli ja store taru andmete ORC vormingus

Kui andmed on suur, eraldamine tabel on kasulik päringud, mis on vaja ainult mõne tabeli sektsioonid skannida. Näiteks on mõistlik partition logiandmed veebisaidi kuupäevade järgi.

Lisaks eraldamine taru tabelid, on kasulik taru andmete talletamiseks optimeeritud rea piklikku (ORC) vormingus. ORC vormindamise kohta leiate lisateavet teemast <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">ORC kasutamine failide parandab jõudlust, kui on lugemine, kirjutamine, ja andmete töötlemiseks</a>.

### <a name="partitioned-table"></a>Sektsioonitud tabeli
Siin on taru päring, mis loob sektsioonitud tabeli ja selle andmete laaditakse.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Kui päringu piiretega tabelid, on soovitatav lisada partition tingimus **alguses** on `where` klausel, kuna see parandab tõhusust märkimisväärselt otsimine.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Taru andmete talletamiseks ORC vorming

Taru tabelitesse, mis on talletatud ORC vormingus ei saa andmeid laadida otse bloobimälu. Siin on mõned toimingud, mida te tegema laadimiseks andmete Azure plekid taru tabelitele ORC vormingus.

Saate luua bloobimälu Välistabel **TALLETATUD AS TEKSTIFAILIDE** ja laadi andmed tabelisse.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Sama skeemi välise tabelina sama välja eraldaja sammu 1, sisemine tabeli loomine ja taru andmed ORC vormingus salvestada.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Samm 1 välise tabeli andmed valida ja ORC tabeli lisamine

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Kui TEKSTIFAILIDE tabeli *& #60; andmebaasi nimi >. & #60; välise tekstifailide tabeli nime >* samm 3 sektsioonid, on selle `SELECT * FROM <database name>.<external textfile table name>` käsk valib partition muutuja väljana tagastatud andmehulgas. Selle üheks lisamist on *& #60; andmebaasi nimi >. & #60; ORC tabeli nime >* nurjub, kuna *& #60; andmebaasi nimi >. & #60; ORC tabeli nime >* ei ole partition muutuja tabeli skeemi välja. Sel juhul peate valima spetsiaalselt väljad, mille soovite lisada *& #60; andmebaasi nimi >. & #60; ORC tabeli nime >* järgmiselt:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

On ohutu kukutage soovitud *& #60; välise tekstifailide tabeli nime >* kui abil järgmine päring pärast kõik andmed on antud lisada *& #60; andmebaasi nimi >. & #60; ORC tabeli nime >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Pärast selle toimingu peaks olema tabeli andmetega ORC vormingus kasutamiseks valmis.  
