<properties
    pageTitle="Meeskonnatöö andmete teadus protsessi toiming: kasutamine Hadoopi kogumite | Microsoft Azure'i"
    description="Meeskonnatöö andmete teadus protsessi kasutamise lõpuni stsenaarium, mis töötab ka Hdinsightiga Hadoopi kobar saavad luua ja juurutada mudeli kasutamine avalikult kättesaadavaks andmekomplekti."
    services="machine-learning,hdinsight"
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Meeskonnatöö andmete teadus protsessi toiming: Hdinsightiga Hadoopi kogumite abil

See kiirtutvustus kasutame abil talletada, uurida ja funktsioonide insener andmete avalikult kättesaadavaks [NYC takso reisi](http://www.andresmh.com/nyctaxitrips/) andmekomplekti ja alla valimi andmed on [Azure Hdinsightiga Hadoopi kobar](https://azure.microsoft.com/services/hdinsight/) -lõpuni stsenaariumis [Meeskonnatöö andmete teadus protsess (TDSP)](data-science-process-overview.md) . Mudelite andmed on ehitatud Azure seadme õ käsitlema kahendarvu ja multiclass liigitamine ja regressiooni sõnastikupõhise tööülesanded.

Lühiülevaade näitab reageerimine suuremat (1 Teratavu) andmekomplekti kasutamine andmete töötlemiseks Hdinsightiga Hadoopi kogumite sarnase stsenaariumi jaoks, leiate teemast [Meeskonnatöö andmete teadus protsessi - abil Azure Hdinsightiga Hadoopi kogumite andmekogumis 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Samuti on võimalik kasutada ka IPython märkmiku ülesannete, mis on esitatud ülevaade, kui kasutada 1 TB. Kasutajad, kes soovivad proovige seda moodust peaks nõu teema [Criteo kiirtutvustus taru ODBC ühenduse abil](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>NYC takso reisi andmekomplekti kirjeldus

NYC taksosõidu andmed on umbes 20GB tihendatud komaga eraldatud väärtuste (CSV) faili (~ 48GB tihendamata), mis sisaldab rohkem kui 173 miljonit üksikute reisi ja hinnad makstud iga reisi. Iga reisi kirje, mis sisaldab komplekteerimine ja esmase asukoht ja kellaaeg, anonüümsete hack (juht) litsentside arvu ja medallion (takso on kordumatu id) arvu. Andmete hõlmab kõiki reisi aasta 2013 ja on järgmised kaks andmekogumite ettenähtud iga kuu.

1. 'Trip_data' CSV-failid sisaldab reisi üksikasju, näiteks arv reisijate, komplekteerimine ja dropoff punkte, reisi kestus ja reisi pikkus. Siin on mõned valimi kirjed.

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'Trip_fare' CSV-failid sisaldavad üksikasju ja iga reisi makse tüüp, hind summa, lisatasu eest ja maksud, näpunäited ja kasutusmaksu, nt makstud makstud kogusumma. Siin on mõned valimi kirjed.

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Kordumatu liituda reisi\_andmed ja reisi\_hind koosneb väljad: medallion hack\_litsents ja komplekteerimine\_kuupäev ja kellaaeg.

Saada kõik üksikasjad kindla reisi jaoks oluline, piisab kolme abil liituda: "medallion", "hack\_litsentsi" ja "komplekteerimine\_kuupäev ja kellaaeg".

Kirjeldame andmed rohkem üksikasju, kui me salvestaks need taru tabelitesse varsti.

## <a name="mltasks"></a>Näiteid ennustamine tööülesanded
Kui lähenedes määramisel sellist prognoose, mida soovite muuta andmete põhjal analüüsi abil selgitada ülesanded, mida on vaja oma protsessi kaasata.
Siin on kolm näidet ennustamine probleeme, mida me aadress mille kujundamisel põhineb ülevaate selle *näpunäite\_summa*:

1. **Kahendarvu liigitamine**: prognoosida, kas Näpunäide makstud reisi, st on *näpunäite\_summa* mis on suurem kui $0, on positiivne näide ajal on *näpunäite\_summa* $ 0 on negatiivne näide.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass liigitamine**: prognoosida näpunäite summasid reisil vahemik. Jagame selle *näpunäite\_summa* viis aluste või tunnid:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressioonisirge tööülesande**: prognoosida väljaostetud reisi näpunäidet summa.  


## <a name="setup"></a>Täiustatud analüüsi jaoks soovitud Hdinsightiga Hadoopi kobar häälestamine

>[AZURE.NOTE] See on tavaliselt **administraator** ülesanne.

Saate häälestada Azure keskkond täiustatud analüüsi, mis töötab ka Hdinsightiga kobar kolm toimingut:

1. [Loo konto salvestusruumi](../storage/storage-create-storage-account.md): seda salvestusruumi kontot kasutatakse Azure'i bloobimälu andmete talletamiseks. Hdinsightiga rühmades kasutatud andmed asuvad ka siin.

2. [Täiustatud Analytics protsessi ja tehnoloogia kohandamine Azure Hdinsightiga Hadoopi kogumite](machine-learning-data-science-customize-hadoop-cluster.md). Selles etapis tuleb loob Azure Hdinsightiga Hadoopi kõik sõlmed installisite 64-bitine Anaconda Python 2,7 kobar. On kaks olulised toimingud ajal kohandamise klaster Hdinsightiga meeles pidada.

    * Ärge unustage salvestusruumi konto juhises loodud 1 koos Hdinsightiga klaster loomisel selle link. Selle konto salvestusruumi kasutatakse juurdepääsuks andmetega, klaster sees.

    * Pärast klaster on loodud, võimaldavad Kaugpöördusteenuse klaster pea sõlme. Liikuge vahekaarti **konfigureerimine** ja klõpsake nuppu **Luba Remote**. See toiming määrab kasutajatunnust, kasutatakse remote Logi sisse.

3. [Mõne Azure seadme Õppekeskuse tööruumi loomine](machine-learning-create-workspace.md): see Azure seadme Õppekeskuse tööruumi kasutatakse ehitada masina õppe mudelid. See toiming on adresseeritud pärast lõpetamist, mille esialgne andmete uurimine ja valimite abil Hdinsightiga kobar alla.

## <a name="getdata"></a>Avaliku andmeallika andmete toomine

>[AZURE.NOTE] See on tavaliselt **administraator** ülesanne.

[NYC takso reisi](http://www.andresmh.com/nyctaxitrips/) andmekomplekti toomine selle avaliku asukohta, võib kasutada mis tahes [Andmete teisaldamine ja sealt Azure'i bloobimälu](machine-learning-data-science-move-azure-blob.md) kirjeldatud andmete kopeerimiseks teie arvutis.

Siin kirjeldame kasutamise AzCopy faile, mis sisaldavad andmeid. Laadige alla ja installige AzCopy järgige [kasutamise](../storage/storage-use-azcopy.md)alustamine AzCopy käsurea kasuliku.

1. Käsuviiba aken, alates probleemi AzCopy järgmised käsud, asendades *< path_to_data_folder >* soovitud sihtkohta:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Kui koopia on lõpule jõudnud, on kokku 24 tihendatud failide kausta andmed on valitud. Pakkige see lahti allalaaditud failid teie kohalikus arvutis samasse kausta. Märkige üles kaust, kus asuvad tihendamata faile. Selle kausta edaspidi selle *< tee\_et\_unzipped_data\_failide\> * on järgnevalt.


## <a name="upload"></a>Andmete üleslaadimine vaikimisi ümbris, Azure Hdinsightiga Hadoopi kobar

>[AZURE.NOTE] See on tavaliselt **administraator** ülesanne.

AzCopy järgmised käsud, asendage järgmiste parameetrite Hadoopi kobar loomisel määratud tegelike väärtuste ja failide andmed.

* ***& #60; path_to_data_folder >*** kataloogi (tee) koos mahalaadimist andmefailid sisaldavad teie arvutis  
* ***& #60; salvestusruumikonto nimi Hadoopi klaster >*** seotud klaster HDInsight konto salvestusruumi
* ***& #60; vaikimisi ümbris Hadoopi kobar >*** vaikimisi ümbris kasutatavaid klaster. Märkus. vaikimisi s.o ümbris, mille nimi on tavaliselt ise klaster sama nimi. Kui klaster nimetatakse "abc123.azurehdinsight.net", on vaikimisi container abc123.
* ***& #60; salvestusruum konto võti >*** kasutatav klaster salvestusruumi konto võti

Käsuviip või Windows PowerShelli aken teie arvutis, käivitage järgmised kaks AzCopy käsud.

See käsk lisatud reisi andmete ***nyctaxitripraw*** directory Hadoopi klaster vaikimisi ümbrises.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

See käsk lisatud hind andmete ***nyctaxifareraw*** directory Hadoopi klaster vaikimisi ümbrises.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Andmete peaks nüüd Azure'i bloobimälu ja tarbitud Hdinsightiga kobar valmis.

## <a name="#download-hql-files"></a>Logida Hadoopi kobar pea sõlme ja ja uurimuslikud analüüsi ettevalmistamine

>[AZURE.NOTE] See on tavaliselt **administraator** ülesanne.

Kobar uurimuslikud analüüsimiseks ja alla valimite andmete pea sõlme juurdepääsuks toimige liigendatud Accessi [Pea sõlm Hadoopi kobar](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

Ülevaate, peamiselt kasutame päringute kirjutatud [taru](https://hive.apache.org/), SQL-like päringukeele esialgsed uuringu sooritamiseks. Päringute taru on talletatud .hql failid. Oleme seejärel alla näidisandmed selle Azure seadme õ mudelite jaoks kasutama.

Klaster valmistuda uurimuslikud analüüsi meil alla laadida .hql faile, mis sisaldavad oluline taru skriptide kaudu [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) pea sõlme kohalikku kausta (C:\temp). Selleks, avage **käsuviiba** kaudu klaster pea sõlme sees ja probleemi järgmised kaks käsud:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Need kaks käsud alla kõik vajalikud ülevaate kohaliku kataloogi ***C:\temp & #92;*** pea sõlme .hql failid.

## <a name="#hive-db-tables"></a>Taru andmebaas ja tabelid, liigendatud kuu loomine

>[AZURE.NOTE] See on tavaliselt **administraator** ülesanne.

Meil on nüüd valmis looma meie NYC takso andmekomplekti taru tabelid.
Hadoopi kobar pea sõlme ***Hadoopi käsurea*** pea sõlme töölaual avatud ja sisestage taru kataloogi käsu sisestamise teel

    cd %hive_home%\bin

>[AZURE.NOTE] **Käita kõik taru käsud ülaltoodud taru prügikasti ülevaate / directory küsimus. See hoolitseb probleemidest tee automaatselt. Kasutame termineid "Taru directory viip", "taru prügikasti / directory viip", "Hadoopi käsurida" vaheldumisi sisse juhendis ja.**

Kaudu taru directory viip, sisestage järgmine käsk rakenduses Hadoopi käsurea pea sõlme taru päringu loomiseks taru andmebaas ja tabelid, esitada:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Siin on sisu on ***C:\temp\sample\_taru\_loomine\_db\_ja\_tables.hql*** faili, mis loob taru andmebaasi ***nyctaxidb*** ja tabelite ***reisi*** ja ***Hind***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Taru skript loob kahe tabeli:

* "reisi"-tabel sisaldab reisi üksikasjad iga sõitma (draiveri üksikasjad, aeg, reisi kauguse ja korda)
* "hind" tabel sisaldab hind üksikasjad (hind summa, näpunäite summa, teemaks ja lisatasud).

Kui teil on vaja täiendavat abi järgmiste toimingute abil või soovite uurida alternatiivne neist, vaadake jaotist [taru esitada päringuid otse Hadoopi käsurea kaudu ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Andmete laadimine taru tabelitele, sektsioonid

>[AZURE.NOTE] See on tavaliselt **administraator** ülesanne.

NYC takso andmekomplekti on füüsiline eraldamine kuu, mida me kasutame lubamiseks kiiremini töötlemine ja päringu korda. "Reisi" ja "hind" taru tabelite liigendatud kuu hulga andmete laadimine PowerShelli käske allpool (antud taru kataloogist **Hadoopi käsurea**kaudu).

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Funktsiooni *valimi\_taru\_laadimine\_andmete\_,\_partitions.hql* fail sisaldab järgmisi käske **Laadi** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Teate, et kasutame avastamine protsessi taru päringute arv kaasata, kas soovite ühe sektsiooni või vaid paar sektsioonid. Kuid need päringud saanud käivitada kogu andmed üle.

### <a name="#show-db"></a>Kuva andmebaasidega Hdinsightiga Hadoopi kobar

Tekstivälja Hadoopi käsurea akna Hdinsightiga Hadoopi kobar loodud andmebaaside kuvamiseks käivitage järgmine käsk Hadoopi käsurea:

    hive -e "show databases;"

### <a name="#show-tables"></a>Kuva taru tabelid nyctaxidb andmebaasis

Nyctaxidb andmebaasi tabelid kuvamiseks käivitage järgmine käsk Hadoopi käsurea:

    hive -e "show tables in nyctaxidb;"

Me saame kinnitada, et tabelid on liigendatud väljastanud käsu alla:

    hive -e "show partitions nyctaxidb.trip;"

Oodatav väljund on allpool näidatud:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Samuti saate tagada, et tabeli hind on liigendatud väljastanud käsu alla:

    hive -e "show partitions nyctaxidb.fare;"

Oodatav väljund on allpool näidatud:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Andmete uurimine ja funktsioon tehnoloogia nii taru

>[AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Andmete uurimine ja tehnika tööülesannete laaditud taru tabelite andmete jaoks funktsiooni suunamist taru päringute abil. Siin on näited ülesandeid, et tutvustame teile selle jaotise.

- Mõlema tabeli ülemine 10 kirjete vaatamiseks.
- Tutvuge andmete jaotamine erineva aja Windows mõned väljad.
- Uurige andmete kvaliteedi pikkus- ja laiuskraadide välju.
- Binaarsed ja multiclass liigitamine siltide põhjal luua soovitud **näpunäite\_summa**.
- Luua funktsioonid nulliga otsese reisi kaugused.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Avastamine: Top 10 kirjete vaatamiseks tabelis koostada ärireisi

>[AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Milline näeb välja andmete kuvamiseks uurime iga tabeli 10 kirjed. Käivitage järgmised kaks päringud eraldi taru directory küsimuse Hadoopi käsurea konsooli kontrollimiseks kirjeid.

Saada ülemine 10 kirjeid tabelis "reisi" esimese kuu:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Saada ülemine 10 kirjeid tabelis "hind" esimese kuu:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Sageli on kasulik kirjete mugavalt faili salvestada. Väike muutus päringusse ülaltoodud millega järgmine:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Avastamine: Vaate kirjete iga 12 sektsioonide arv

>[AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Kuidas huvi on mitu korda muutub kalendriaasta jooksul. Kuu alusel rühmitamine võimaldab meil näha, milline näeb välja selline levitamine reisi.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

See annab meile väljund:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Siin esimene veerg on kuu ja teine on mitu korda sel kuul.

Me ka loendamine kirjete arv meie reisi andmehulgas väljastanud taru directory käsuviibale järgmine käsk.

    hive -e "select count(*) from nyctaxidb.trip;"

Liitmine annab väärtuse:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Sarnane näidatud reisi andmehulga käskude abil saame anda taru päringutele taru directory Küsi hind andmehulga valideerimiseks kirjete arvu.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

See annab meile väljund:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Pange tähele, et nii tagastatakse reisi kuus täpselt sama arvu. See pakub esimese kinnitamise andmed on laaditud õigesti.

Kirjete hind andmehulga koguarvu loendamine saab teha abil taru directory küsimuse all käsk:

    hive -e "select count(*) from nyctaxidb.fare;"

Liitmine annab väärtuse:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Mõlemas tabelis kirjete arv on ka sama. See pakub teise valideerimine, et andmed on laaditud õigesti.

### <a name="exploration-trip-distribution-by-medallion"></a>Avastamine: Reisi jaotus medallion

>[AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Selles näites tähistab medallion (takso arvud) ja rohkem kui 100 reisi antud aja jooksul. Päringu kasu sektsioonitud tabeli juurdepääs, kuna see on see tingitud partition muutuv **kuu**. Päringu tulemused kirjutada kohaliku faili queryoutput.tsv sisse `C:\temp` pea sõlme.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Siin on sisu *valimi\_taru\_reisi\_count\_,\_medallion.hql* faili kontrollimiseks.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medallion NYC takso andmehulgas tuvastab kordumatu kabiini. Saate selgitame, millised kabiinid on "hõivatud" küsides, millised tehtud reisi arv ületab teatud aja jooksul. Järgmises näites tuvastab taksode tehtud üle saja reisi esimese kolm kuud ja päringu tulemused kohaliku faili, C:\temp\queryoutput.tsv salvestatakse.

Siin on sisu *valimi\_taru\_reisi\_count\_,\_medallion.hql* faili kontrollimiseks.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Küsimus taru directory käsuviiba kaudu alltoodud käsk:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Avastamine: Reisi jaotuse medallion ja hack_license

>[AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Andmekomplekt uurides tahame sageli uurida co esinemisjuhtu rühmade väärtuste arvu. Selles jaotises pakuvad näide sellest, kuidas seda teha taksod ja juhtide jaoks.

Funktsiooni *valimi\_taru\_reisi\_count\_,\_medallion\_license.hql* faili pakuvad "medallion" ja "hack_license" hind andmehulga ja tagastab loendab iga kombinatsiooni. Allpool leiate selle sisu.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

See päring tagastab cab ja kindla draiveri kombinatsioon tellinud mitu korda laskuvas järjestuses.

Taru directory käsuviibas käivitage:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Kohaliku faili C:\temp\queryoutput.tsv päringutulemites kirjutada.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Avastamine: Andmete kvaliteedi hindamise, märkides ruudu sobimatu pikkus-ja laiuskraad kirjete

>[AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Levinud eesmärk uurimuslikud analüüsi on vigane või vigased kirjed rookima. Selle jaotise määrab, kas pikkus või laiuskraad väljad on väärtus palju väljaspool NYC. Kuna see on tõenäoline, et dokumentidest vigaseid laiuskraadide-Current, soovime kõrvaldada need andmed, mida kasutatakse modelleerimiseks.

Siin on sisu *valimi\_taru\_kvaliteedi\_assessment.hql* faili kontrollimiseks.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Taru directory käsuviibas käivitage:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Argument *-S* kaasatud selle käsu tõkestab oleku kuva väljaprindi taru MAPI/vähenda tööd. See on kasulik, sest see teeb ekraani printimine taru päringu väljund loetavamaks.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Avastamine: Kahendarvu klassi jaotamine reisi näpunäited

> [AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

[Näiteid ennustamine tööülesanded](machine-learning-data-science-process-hive-walkthrough.md#mltasks) jaotises kirjeldatud kahendarvu liigitamine probleem, on kasulik teada, kas Näpunäide anti või mitte. Näpunäiteid selle jaotus on kahendarvu:

* Näpunäide antud (1 tund, näpunäite\_summa > 0)  
* pole näpunäite (klassi 0, näpunäite\_summa = $0).

Funktsiooni *valimi\_taru\_otsaga\_frequencies.hql* seda faili allpool teeb.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Taru directory käsuviibas käivitage:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Avastamine: Multiclass säte klassi üldiste müügijaotustega tutvumine

> [AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

[Näiteid ennustamine tööülesanded](machine-learning-data-science-process-hive-walkthrough.md#mltasks) jaotises esitatud juhiseid multiclass liigitamine probleemi selle andmehulga ka sobiv loomulikus liigitamine, kus soovime prognoosida antud näpunäidete summa. Kasutame aluste näpunäite vahemike määratlemise päringu. Jaotuse tunni jaoks eri Näpunäide vahemike saamiseks kasutame funktsiooni *valimi\_taru\_näpunäite\_vahemikus\_frequencies.hql* faili. Allpool leiate selle sisu.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Hadoopi käsurea konsool käivitage järgmine käsk:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Avastamine: Arvutada otsese vahemaa laiuskraadide-laiuskraad kahes kohas

> [AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Võttes otsest kauguse mõõt võimaldab meile teada see ja tegeliku teekonna vahemaa vaheline erinevus. Me motiveerida seda funktsiooni, et reisija võib olla vähem tõenäoliselt Näpunäide. kui ta aru saada, et juht tahtlikult võtnud neid palju enam marsruutimiseks.

Tegelik reisi kauguse ja [Haversine vahemaa](http://en.wikipedia.org/wiki/Haversine_formula) ühest laiuskraadide-laiuskraad ("tore ringi" kauguse) võrdlus vaatamiseks kasutame Trigonomeetrilised funktsioonid saadaval sees taru, seega:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

Päringu ülal, R on miili maa ja teisendatakse pii radiaani. Pange tähele, et laiuskraadide-laiuskraad punktide "filtreeritakse" eemaldamiseks väärtused, mis ei ole NYC ala.

Sel juhul kirjutage meie tulemused kausta nimega "queryoutputdir". Allpool käskude järjestuse esmalt loob selle väljundi kausta ja seejärel käivitab käsu taru.

Taru directory käsuviibas käivitage:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Päringu tulemused kirjutada 9 Azure'i plekid ***queryoutputdir/000000\_0*** abil ***queryoutputdir/000008\_0*** jaotises vaikimisi container Hadoopi klaster.

Üksikute plekid mahu vaatamiseks me käivitage järgmine käsk taru directory viip:

    hdfs dfs -ls wasb:///queryoutputdir

Antud faili sisu kuvamiseks öelda 000000\_0, kasutame Hadoopi 's `copyToLocal` käsku, seega.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`võib olla väga aeglane suuri faile ja ei ole soovitatav kasutada nendega.  

Andmete asu mõne Azure'i bloobimälu kasuks on võib-olla uurime Azure seadme õ [Andmete importimine] asuvate andmete[ import-data] mooduli.


## <a name="#downsample"></a>Valimi andmed ja Koosta mudelite Azure seadme õ alla

> [AZURE.NOTE] See on tavaliselt **Andmete teadlane** ülesanne.

Pärast uurimuslikud analüüsi etapp, meil on nüüd valmis alla valimi koostamise mudelite Azure seadme õppe andmeid. Selles jaotises näitame alla valimi taru päringu abil andmeid, mis on seejärel kättesaadav [Andmete importimine] [ import-data] mooduli Azure seadme õppe.

### <a name="down-sampling-the-data"></a>Allapoole proovide andmed

See toiming on kaks toimingut. Esmalt liitumine **nyctaxidb.trip** ja **nyctaxidb.fare** tabelid kolme võtmed, mis on olemas kõik kirjed: "medallion", "hack\_litsentsi", ja "komplekteerimine\_kuupäev ja kellaaeg". Seejärel looge soovitud kahendarvu liigitamine sildi **otsaga** ja mitme klassi liigitamine sildi **näpunäite\_klassi**.

Saama abil valimisse andmed otse [Andmete importimine] [ import-data] mooduli Azure seadme õppe, on vaja talletage ülaloleval päringul sisemise taru tabelisse. Järgnevalt sisemise taru tabeli loomine ja selle sisu on ühendatud ja alla valimisse andmete asustamine.

Päringu kehtib taru funktsioonidele otse päev, weekday (Esmaspäev tähistab 1 ja 7 tähistab pühapäev) aasta nädal tunni loomiseks on "komplekteerimine\_kuupäev ja kellaaeg" välja ja otsese vahemaa komplekteerimine ja dropoff asukohad. Kasutajate võib viidata [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) ülesannete täieliku loendi.

Päringu seejärel alla näidised andmed, et Päringu tulemites mahub Azure seadme õ Studio. Ainult umbes 1% algse andmekomplekti imporditakse Studio.

Allpool on sisu *valimi\_taru\_ettevalmistamine\_jaoks\_koostamisel\_full.hql* fail, mille valmistab andmete koostamise Azure seadme õppe mudel.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Selle päringu käivitamiseks kaudu taru directory viip:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Meil on sisemine tabel "nyctaxidb.nyctaxi_downsampled_dataset", mida saab kasutada [Andmete importimine] [ import-data] mooduli Azure seadme õppe. Lisaks kasutame selle andmehulga jaoks seadme õppe mudelid.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Andmete importimine mooduli kasutamine Azure seadme õ alla valimisse andmetele juurdepääsuks

Kui eeltingimused väljastamise taru päringute [Andmete importimine] [ import-data] mooduli Azure seadme õppe, läheb vaja on Azure arvutis Õppekeskuse tööruumi ja juurdepääsuga klaster ja selle seotud salvestusruumi konto identimisteave.

[Andmete importimine] mõned üksikasjad[ import-data] mooduli ja sisestada parameetrid:

**HCatalog serveri URI**: kui kobar nimi on abc123, siis see on lihtsalt: https://abc123.azurehdinsight.net

**Hadoopi konto kasutajanimi** : kasutaja nime valida kobar (**mitte** Kaugpöördusteenuse kasutajanimi)

**Hadoopi teenuste konto parooli** : valitud kobar (**mitte** Kaugpöördusteenuse parool) parool

**Väljundi andmete asukoht** : see valitakse Azure.

**Azure'i salvestusruumikonto nimi** : salvestusruumi vaikekonto nimi seotud klaster.

**Azure'i container nimi** : see on klaster container vaikenime ja on tavaliselt sama kobar nimi. Klaster nimega "abc123", on see lihtsalt abc123.

> [AZURE.IMPORTANT] **Mis tahes tabeli soovime päringu abil [Andmete importimine] [ import-data] mooduli Azure seadme õppe peab olema on sisemine tabel.** Näpunäide kindlaks teha, kui tabeli T andmebaasi D.db on sisemine tabel on järgmine.

Küsimus taru directory käsuviiba kaudu käsk:

    hdfs dfs -ls wasb:///D.db/T

Kui tabel on sisemine tabel ja see on täidetud, peate selle sisu siin kuvada. Teine viis kindlaks teha, kas tabel on sisemine tabel on Azure salvestusruumi Exploreriga. Selle abil saate liikuda klaster container vaikenimi ja seejärel tabeli nime järgi filtreerida. Kui tabeli ja selle sisu kohale, see kinnitab, et see on sisemine tabel.

Siit leiate ülevaate taru päring ja [Andmete importimine] [ import-data] mooduli:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Tähele, et alates meie valimisse andmed asuvad vaikimisi ümbrises allapoole tulemuseks taru päringu Azure seadme õppe on väga lihtne ja on vaid mõne "valige *: nyctaxidb.nyctaxi\_jätate\_andmed".

Andmekomplekti võib kasutada lähtepunktina nüüd hoone masina õppe mudelid.

### <a name="mlmodel"></a>Azure'i masina õppe mudelite koostamine

Meil on nüüd võimalik mudeli building ja mudeli juurutamise [Azure seadme](https://studio.azureml.net)õppe. Andmed on valmis kasutama eespool ennustamine probleemide lahendamine:

**1. kahendarvu liigitamine**: prognoosida, kas Näpunäide maksma reisi.

**Kasutada õppurite:** Kaks klassi logistilise regressiooni

lisamine. Selle probleemi meie siht-(või tunni) silt on "otsaga". Meie algse alla valimisse andmekomplekti on mõned veerud, mis on target lekked selle liigitamine katse puhul. Täpsemalt: näpunäite\_klassi, Näpunäide\_summa ja kogusumma\_esile teavet target silt, mis pole saadaval testimise ajal summa. Oleme nende veergude eemaldamine tasu [Andmekomplekti veergude valimine] abil[ select-columns] mooduli.

Allpool hetktõmmise kuvatakse meie katse prognoosida, kas antud reisi maksma Näpunäide.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. See katse meie target sildi jaotuse olid umbes 1:1.

Allpool hetktõmmise kuvatakse näpunäite jaotumine klassi siltide kahendarvu liigitamine probleemi.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Selle tulemusena saame AUC 0,987, nagu on näidatud alloleval joonisel.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass liigitamine**: prognoosida näpunäite summasid reisil, kasutades eelnevalt määratletud tunnid vahemik.

**Kasutada õppurite:** Multiclass logistilise regressiooni

lisamine. Selle probleemi meie siht-(või tunni) silt on "näpunäite\_class" mis võib võtta üks viis väärtused (0,1,2,3,4). Nagu kahendarvu liigitamine, meil on mõned veerud, mis on target lekked selle katse puhul. Täpsemalt: otsaga, Näpunäide\_summa, kokku\_esile teavet target silt, mis pole saadaval testimise ajal summa. Me eemaldada need veerud, [Valige veerud andmekomplekti] abil[ select-columns] mooduli.

Allpool hetktõmmise näitab meie katse prognoosida, millised prügikasti on näpunäite võib jääda (klassi 0: näpunäite = 0, 1 tund: Näpunäide > 0 ja näpunäite < = $5, klassi 2: Näpunäide > $5 ja näpunäite < = $10, klassi 3: Näpunäide > $10 ja näpunäite < = 20, klassi 4: Näpunäide > 20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Nüüd näitame meie tegelik testi klassi jaotuse välja näeb. Näeme, et klassi 0 ja 1 tund on levinud, muud klassid on harva.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. See katse kasutame segadust maatriksi pilk meie ennustamine täpsus. See on näidatud allpool.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Pange tähele, et meie klassi täpsus kohta levinud klassid on üsna hea, mudel ei ole head tööd "õ" harvem kohta.


**3. regressiooni tööülesande**: prognoosida näpunäite väljaostetud reisi summa.

**Kasutada õppurite:** Võimendatud Otsusepuu kuvamine

lisamine. Selle probleemi meie siht-(või tunni) silt on "näpunäite\_summa". Sel juhul on meie target lekked: otsaga, Näpunäide\_klassi kokku\_summa; kõigi nende muutujate esile näpunäite summa, mis on tavaliselt saadaval testimisel aja teave. Me eemaldada need veerud, [Valige veerud andmekomplekti] abil[ select-columns] mooduli.

Hetktõmmise belows kuvatakse meie katse prognoosida antud Näpunäide summa.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Regressioonisirge probleemid, mõõta meie prognoosi täpsus vaadates ruutude viga prognoose, ruudu määramise ja meeldib. Näitame neid allpool.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Näeme, et ruudu määramise kohta on 0.709, tähendab umbes 71% dispersioon seletab meie mudeli kordajad.

> [AZURE.IMPORTANT] Azure'i masina õppimine ja kuidas vaadata ja kasutada seda kohta lisateabe saamiseks vaadake [mis on masina õ?](machine-learning-what-is-machine-learning.md). Väga kasulik ressurss, mille mängime masina õ katsete Azure seadme õ hulga on [Cortana ärianalüüsi Galerii](https://gallery.cortanaintelligence.com/). Galerii hõlmab igasuguseid katsete ja põhjalik toomist Azure seadme õ erinevaid võimalusi pakub.

## <a name="license-information"></a>Litsentsiteavet

Selle valimi kiirtutvustus ja selle kaasnevate skripte ühiskasutusse Microsoft alla MIT litsents. Kontrollige LICENSE.txt faili kataloogis proovi kood github rohkem üksikasju.

## <a name="references"></a>Viited

• [Andrés Monroy NYC takso reisi allalaadimine leht](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC's takso reisi andmete Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC takso ja limusiin vahendustasu teadus ja statistika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
