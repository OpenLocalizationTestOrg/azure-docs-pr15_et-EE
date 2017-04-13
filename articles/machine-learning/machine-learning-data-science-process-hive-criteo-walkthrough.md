<properties
    pageTitle="Meeskonnatöö andmete teadus protsessi toiming: abil Hdinsightiga Hadoopi kogumite 1 TB Criteo andmekomplekti kohta | Microsoft Azure'i"
    description="Meeskonnatöö andmete teadus protsessi kasutamise lõpuni stsenaarium, mis kasutavad mõne Hdinsightiga Hadoopi kobar saavad luua ja juurutada mudeli kasutamine suurte (1 TB) avalikult kättesaadavaks andmekomplekti"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Toimingu - abil Azure Hdinsightiga Hadoopi kogumite 1 TB andmekogumis meeskonnatöö andmete teadus protsess

Ülevaate, näitame abil andmete meeskonna teadus protsessi koos mõne [Azure Hdinsightiga Hadoopi kobar](https://azure.microsoft.com/services/hdinsight/) -lõpuni stsenaariumi puhul talletada, uurida, funktsioon insener, ja alla üks avalikult kättesaadavaks [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) andmekomplektide näidisandmeid. Azure'i masina õ abil luua kahendarvu liigitamine mudeli andmed. Näitame ka kuidas avaldada ühte need mudelid veebiteenusest.

Samuti on võimalik kasutada ka IPython märkmiku tegemise juhendis esitatud tööülesannete. Kasutajad, kes soovivad proovige seda moodust peaks nõu teema [Criteo kiirtutvustus taru ODBC ühenduse abil](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Criteo andmekomplekti kirjeldus

Criteo, andmed on klõpsake ennustamine andmekomplekti, mis on ligikaudu 370GB gzip tihendatud TSV faile (~1.3TB tihendamata), mis sisaldab rohkem kui 4,3 miljardit kirjed. See on võetud 24 päeva, klõpsake andmete [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/)tehtud. Andmeteadlaste mugavuse huvides me lahti pakitud meile eksperimenteerida olemasolevaid andmeid.

Tuvastatavate iga kirje sisaldab 40 veerud.

- esimene veerg on sildi veerg, mis näitab, kas kasutaja klõpsab on **lisada** (väärtus 1) või klõpsake ühte (väärtus 0)
- Järgmine 13 veerud on arvuline, ja
- viimase 26 on kategooriline veerud

Veerud on anonüümseks ja kasutada mitmesuguseid nummerdatud nimed: "Col1" (veeru silt) jaoks soovite "Col40" (kategooriline andmeveerg) jaoks.            

Siin on kaks (ridade) märkused selle andmehulga veerud esimese 20 väljavõte:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Tuvastatavate kategooriline ja arvulised veerud on puuduvate väärtuste. Kirjeldame lihtsa meetodi puuduvate väärtuste käsitlemise. Lisateavet andmete uurida kui me talletada taru tabelitesse.

**Määratlus:** *Läbiklõpsamissageduse (KMS):* See on protsent klõpsuga andmeid. Tuvastatavate Criteo, on selle KMS umbes 3,3% või 0,033.

## <a name="mltasks"></a>Näiteid ennustamine tööülesanded
Kahe valimi ennustamine probleemide lahendamise ülevaate:

1. **Binaarsed liigitamine**: prognoosib, kas kasutaja klõpsanud lisada:
    - Klassi 0: Pole nuppu
    - Klassi 1: klõpsake

2. **Regressioonisirge**: prognoosib tõenäosuse ka ad klõpsake kasutaja funktsioone.


## <a name="setup"></a>Andmete Science seada üles an Hdinsightiga Hadoopi kobar

**Märkus:** See on tavaliselt **administraator** ülesanne.

Azure'i andmed teadus keskkonna ennustav lahenduste koos Hdinsightiga kogumite kolm toimingut häälestamiseks tehke järgmist.

1. [Loo konto salvestusruumi](../storage/storage-create-storage-account.md): seda salvestusruumi kontot kasutatakse Azure'i bloobimälu andmete talletamiseks. Hdinsightiga rühmades kasutatud andmed on salvestatud siin.

2. [Kohandada Azure Hdinsightiga Hadoopi kogumite andmete teadus](machine-learning-data-science-customize-hadoop-cluster.md): See toiming loob mõne Azure Hdinsightiga Hadoopi kobar kõik sõlmed installisite 64-bitine Anaconda Python 2,7. Lõpuleviimiseks Hdinsightiga kobar kohandamisel on kaks olulised toimingud (selles teemas kirjeldatud).

    * Salvestusruumi konto juhises loodud 1 koos Hdinsightiga klaster loomisel tuleb linkida. Seda salvestusruumi kontot kasutatakse juurdepääsuks andmeid, mida saab töödelda klaster sees.

    * Teil tuleb lubada Kaugpöördusteenuse klaster pea sõlme, pärast selle loomist. Pidage meeles Kaugpöördusteenuse identimisteabe siin määratud (erinevad kobar veebisaidil selle loomise jaoks määratud): peate need tegema järgmised toimingud.

3. [Mõne Azure'i ML tööruumi loomine](machine-learning-create-workspace.md): see Azure seadme Õppekeskuse tööruumi kasutatakse koostamise masina õppe mudelid pärast mille esialgne andmete uurimine ja valimite Hdinsightiga klaster alla.

## <a name="getdata"></a>Saada ja tarbimine avaliku andmeallika

Andmekomplekti [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) pääseb, klõpsates linki, vastu võtmist kasutustingimused ja esitada nimi. Hetktõmmise, mis see näeb välja nagu on näidatud siin:

![Criteo nõustumine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klõpsake nuppu **Jätka allalaadimine** Lisateave andmekomplekti ja nende kättesaadavuse kohta.

Andmed asuvad [Azure'i Bloobivahemälu salvestusruumi](../storage/storage-dotnet-how-to-use-blobs.md) avalikus kohas: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" viitab Azure'i bloobimälu asukoht. 

1. Selle avaliku bloobimälu andmete koosneb kolmest alamkaustad mahalaadimist andmete.

    1. Alamkausta *töötlemata/arv/* sisaldab andmeid - päevast 21 esimese päeva\_00 päevale\_20
    2. Alamkausta *töötlemata/rong/* koosneb ühe päeva andmete päeva\_21
    3. Alamkausta *töötlemata katse/* koosneb andmete, kaks päeva päeva\_22 ja päeva\_23

2. Neile, kes soovivad alustada töötlemata gzip andmetega, need on saadaval ka peamine kausta *töötlemata /* nimega day_NN.gz, kus NN läheb 00 kuni 23.

Alternatiivne lähenemisviisi juurde pääseda, uurida ja modelleerimise neid andmeid, mis ei nõua kohaliku Allalaadimisi selgitatakse edaspidi selles kiirtutvustus taru tabelite loomisel.

## <a name="login"></a>Logige sisse headnode kobar

Sisselogimiseks headnode klaster ning kasutada [Azure portaali](https://ms.portal.azure.com) klaster leidmiseks. Klõpsake vasakul Hdinsightiga elephant ikooni ja seejärel topeltklõpsake klaster nime. Liikuge vahekaarti **konfigureerimine** , topeltklõpsake selle lehe allservas ikooni Loo ühendus ja sisestage mandaat Kaugpöördusteenuse küsimise. See viib teid headnode soovitud klaster.

Siin on tüüpilised first sisse kobar headnode välja näeb.

![Logige sisse klaster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Klõpsake vasakul näeme "Hadoopi käsurea", mis on meie tööhobune andmete uurimine. Näeme ka kahe kasulik URL-id - "Hadoopi lõng Status" ja "Hadoopi nimi sõlme". Lõng olek URL-i kuvatakse töö edenemise ja nime sõlm URL-i leiate üksikasjaliku kobar konfigureerimine.

Nüüd meil on häälestatud ja olete valmis alustama esimene osa on kiirtutvustus: andmete uurimine, kasutades taru ja kuidas andmed Azure seadme õ valmis.

## <a name="hive-db-tables"></a>Taru andmebaasi ja tabelite loomine

Meie Criteo andmekogumi taru tabelite loomine, avamine ***Hadoopi käsurea*** pea sõlme töölaual ja sisestage taru kataloogi, sisestades käsu

    cd %hive_home%\bin

>[AZURE.NOTE] Käita kõik taru käsud taru prügikasti ülevaate / directory küsimus. See hoolitseb probleemidest tee automaatselt. Kasutame termineid "Taru directory viip", "taru prügikasti / directory viip", "Hadoopi käsurida" ja vaheldumisi.

>[AZURE.NOTE]  Mis tahes taru päringu käivitamiseks ühte Kasuta alati järgmised käsud:

        cd %hive_home%\bin
        hive

Pärast taru Kiirsõnumiga kuvatakse koos mõne "taru >"logige, lihtsalt Lõika ja kleebi päringu seda.

Järgmine kood loob andmebaasi "criteo" ja seejärel loob 4 tabelid.


* *tabeli loomiseks loendab* päeva päev tugineb\_00 päevale\_20,
* *tabeli kasutamiseks rongi andmekomplekti* tugineb päeva\_21, ja
* kahe *tabelite jaoks kasutada testi andmekomplektide* tugineb päeva\_22 ja päeva\_23 vastavalt.

Me tükeldada meie testi andmekomplekti kahte erinevat tabelisse, kuna üks päeva on puhkus ja soovime kindlaks teha, kui mudeli on tuvastanud erinevused pühade ja Püha klikkimise määra.

Skripti [valimi & #95; taru & #95; loomine & #95; criteo & #95; andmebaasi & #95; ja & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) mugavuse Siin kuvatakse:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Me Pange tähele, et kõik need tabelid on välised me lihtsalt osutage Azure'i bloobimälu (wasb) asukohad.

**On kaks võimalust, mis tahes taru päringu, mis on nüüd mainida.**

1. **Taru Kiirsõnumiga käsurea kaudu**: esimene on probleemi "Mesilaspere" käsu ja kopeerige ja kleepige päringu taru Kiirsõnumiga käsurea. Selleks tehke.

        cd %hive_home%\bin
        hive

    Nüüd juures olevat Kiirsõnumiga käsurea, lõikamiseks ja kleepimiseks päringu aktiveeritakse see.

2. **Päringute faili salvestamist ja käsku**: teine on päringute .hql faili salvestamiseks ([valimi & #95; taru & #95; loomine & #95; criteo & #95; andmebaasi & #95; ja & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) ja seejärel küsimus päringu järgmine käsk:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Veenduge, et andmebaasi ja tabeli loomine

Järgmiseks kinnitame andmebaasi järgmine käsk: taru aluse loomine / directory viip:

        hive -e "show databases;"

See annab:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

See kinnitab, et luua uus andmebaas, "criteo".

Millised tabelid lõime vaatamiseks me lihtsalt probleemi käsk taru kustutanud / directory viip:

        hive -e "show tables in criteo;"

Siis näeme järgmine väljund:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Andmete uurimine taru

Nüüd on valmis tegema mõned peamine andmete uurimine taru. Me alustada loendamiseks näited rongis ja testida andmetabeleid.

### <a name="number-of-train-examples"></a>Arvu rongi näited

[Valimi ja #95; taru & #95; count ja #95; rongi & #95; tabeli ja #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) sisu on siin näidatud:

        SELECT COUNT(*) FROM criteo.criteo_train;

Liitmine annab väärtuse:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Teise võimalusena ühte võib anda ka järgmine käsk: taru prügikasti / directory viip:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Arvu kahe testi andmekomplektide testi näited

Oleme nüüd arvu kahe testi andmekomplektide näited. Sisu [valimi & #95 taru & #95; count & #95; criteo & #95; testi & #95; päeva & #95; 22 & #95; #95;examples.hql & tabeli](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) on siin:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Liitmine annab väärtuse:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Nagu tavaliselt, võib ka kutsume skripti kaudu taru prügikasti / kataloog viip mõõt emissiooni käsk:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Lõpuks uurime testi näited testi andmekomplekti päeva alusel arvu\_23.

Käsu toiming on sarnane lihtsalt näidatud (vt [valimi & #95; taru & #95; count & #95; criteo & #95; testi & #95; päeva & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

See annab:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Sildi jaotuse rongi andmekogumis.

Sildi jaotuse rongi andmekogumis on huvi. Et näha selle, näitame [valimi & #95; taru & #95; criteo & #95; sildi & #95; jaotuse & #95; rongi & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql)sisu:

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Liitmine annab väärtuse sildi jaotuse:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Pöörake tähelepanu sellele, protsent positiivne siltide 3,3% (kooskõlas algse andmekomplekti).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogrammi jaotamine mõned rongi andmekomplekti arvuline muutujad

Kasutame taru 's native "histogramm\_arvuline" funktsioon välja selgitada arvuline muutujate jaotuse näeb. Siin on [valimi ja #95; taru & #95; criteo & #95; histogramm ja #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql)sisu.

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

See tagab järgmist:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

KÜLG vaade - Lõhu kombinatsiooni taru pakub andes SQL-like väljundi asemel tavaline loend. Pange tähele, et see tabeli esimene veerg vastab prügikasti keskele ja teine prügikasti sagedus.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Ligikaudse protsentiilid või mõne arvulise muutujate rongi andmekomplekti

Arvuline muutujate huvi on ligikaudse protsentiilid arvutamisel. Taru kasutaja kohalikke "protsentiili\_umbes" seda meile teeb. Sisu [valimi ja #95; taru & #95; criteo ja #95; ligikaudse & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) on:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Liitmine annab väärtuse:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Me märkus, et jaotuse protsentiilid on lähemalt histogrammina jaotuse mis tahes numbriline muutuja tavaliselt seotud.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Üheste väärtuste otsimine rongi andmekomplekti kategooriline veerge

Jätkuva andmete uurimine, me nüüd leiate mõned kategooriline veergude arvu need võtta kordumatuid väärtusi. Selleks näitame sisu [valimi ja #95; taru & #95; criteo ja #95; kordumatu & #95; väärtused ja #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Liitmine annab väärtuse:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Me Pange tähele, et Col15 19M kordumatuid väärtusi! Varem meetoditega nagu "üks kuum kodeeringus" kodeerida kõrge mitmedimensioonilised kategooriline muutujate on võimatu. Eelkõige selgitatakse ja demonstreerivad seda nimetatakse [Õ koos loendab](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) probleemi lahendamiseks tõhus võimas, töökindlate tehnika.

Me lõpuks selle punkti mõne muu kategooriline veeru ka Üheste väärtuste arvu. Sisu [valimi ja #95; taru & #95; criteo ja #95; kordumatu & #95; väärtused ja #95 mitme & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) on:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Liitmine annab väärtuse:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Uuesti näeme, et Col20, välja arvatud kõik veerud on palju kordumatuid väärtusi.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Kaasautorluse esinemiskord loendab paari rongi andmekomplekti kategooriline muutujad

Kaasautorluse esinemiskord loendab kategooriline muutujate paari on intressi. See määratleb, kasutades koodi [valimi ja #95; taru & #95; criteo ja #95; andmepunktipaaride & #95; kategooriline & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Me vastupidise loeb Järjestusalus nende esinemise ja vaadake ülaosas 15 Sel juhul. See annab meile:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Valimi andmekomplektide Azure seadme õppe allapoole

Uurida kogumid ja näidata, kuidas võib me seda tüüpi mis tahes muutujate (sh kombinatsioonid), saame kohe alla valimi uurimine andmekogumite nii, et saaksime ehitada Azure seadme õ mudelid. Tagasivõtmise, et saaksime keskenduda probleem on: anda näide atribuute (funktsioon väärtused Col2 - Col40), saame prognoosida, kui Col1 on 0 (klõpsake nuppu) või 1 (klõpsake).

Alla Kuulake meie rong ja testimine andmekomplektide 1% algse suuruse, kasutame taru 's kohalikke RAND() funktsioon. Järgmise skripti [valimi & #95; taru & #95; criteo & #95; Diskreedi & #95; rongi & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) see rongi andmekogumi:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Liitmine annab väärtuse:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Skripti [valimi & #95; taru & #95; criteo & #95; Diskreedi & #95; testi & #95; päeva & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) see katse päeva\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Liitmine annab väärtuse:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Lõpuks skripti [valimi & #95; taru & #95; criteo & #95; Diskreedi & #95; testi & #95; päeva & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) see katse päeva\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Liitmine annab väärtuse:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Seda, oleme meie alla valimisse rong ja testida andmekomplektide hoone Azure seadme õ andmemudelites valmis.

Enne me liikuda Azure'i masina õ, mis on seotud arv tabelis on lõplik oluline osa. Alamtäpi järgmise jaotise käsitleme seda üksikasjalikumalt.

##<a name="count"></a>Lühike arutelu arv tabelis

Nagu me nägite, kategooriline mitut muutujat on väga kõrge dimensionaalsus. Meie kiirtutvustus tutvustame nimega [Õ koos loendab](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) nende muutujate kodeerida tõhusa, töökindlate viisil võimas meetod. Lisateabe saamiseks selle meetodi kohta on esitatud linki.

**Märkus:** Ülevaate, saame keskenduda andes tihendatud esinduste kõrge mitmedimensioonilised kategooriline funktsioonide count tabelite abil. See ei ole ainus võimalus kodeerida kategooriline funktsioonid; Lisateavet teiste meetodite abil, huvi kasutajad vaadata [ühe-otsetee-kodeering](http://en.wikipedia.org/wiki/One-hot) ja [funktsioon räsi](http://en.wikipedia.org/wiki/Feature_hashing).

Count andmete koostamiseks count tabelid, kasutame andmete kausta töötlemata/arv. Jaotises modelleerimine näitame kasutajate koostamiseks need count tabelid kategooriline funktsioonide algusest peale ise luua, või kasutada oma uuringu valmismallide arv tabeli. Järgnevalt kui räägitakse "valmismallide count tabelid" me kõiki pakume count tabelite abil. Üksikasjalikud juhised juurdepääsu nendes tabelites on toodud järgmisest jaotisest.

## <a name="aml"></a>Azure'i masina õ mudel koostamine

Meie mudel tõstmiseks Azure seadme õppe sooritamiseks järgmisi juhiseid.

1. [Taru tabelite andmeid tuua Azure seadme õ](#step1)
2. [Looge katse: andmete puhastamiseks, valige õppurite ja featurize count tabelite abil](#step2)
3. [Koolitada mudel](#step3)
4. [Keskmine mudeli testi andmete põhjal](#step4)
5. [Kui seate mudeli hindamine](#step5)
6. [Avaldada mudeli veebiteenuse olema tarbimine](#step6)

Nüüd on kas olete valmis looma mudelite Azure seadme õ Studios. Meie alla valimisse andmed on salvestatud klaster taru tabelitena. Azure'i masina õ **Andmete importimine** mooduli abil andmete lugemine. Järgnevalt on esitatud mandaadi selle klaster salvestusruumi kontole juurde pääseda.

### <a name="step1"></a>Samm 1: Andmete toomine taru tabelitest Azure'i arvuti impordi andmed mooduli kasutamise õppimine ja valige selle learning katse masina

Alustage, valides mõne **+ Uus** -> **katse** -> **Tühja katse**. Klõpsake **vasakus ülanurgas otsinguväljale** kaudu otsida "Impordi andmed". Pukseerige **Andmete importimine** mooduli kellelegi katse lõuend (Kuva keskel osa) andmed Accessi mooduli kasutamiseks.

See on **Andmete importimine** näeb ajal andmete toomine tabelist taru:

![Andmeid saab andmete importimine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

**Andmete importimine** mooduli, parameetrid, mis on toodud pilti väärtused on vaid näited tuleb sisestada väärtusi sortida. Siin on mõned üldised juhised selle kohta, kuidas määrata **Andmete importimine** mooduli parameetri täitmine.

1. Klõpsake nuppu **andmeallika** "Taru päring"
2. **Taru andmebaasi päringu** väljale lihtsa SELECT * FROM < oma\_andmebaasi\_name.your\_tabeli\_nimi >-piisab.
3. **Hcatalog serveri URI**: kui klaster "abc", siis see on lihtsalt: https://abc.azurehdinsight.net
4. **Hadoopi konto kasutajanimi**: klaster kasutuselevõtmise ajal valitud kasutaja nimi. (Pole selle Kaugpöördusteenuse kasutajanimi!)
5. **Hadoopi kasutajakonto parooli**: parooli kasutuselevõtmise klaster ajal valitud kasutaja nimi. (Pole parooliga Kaugpöördusteenuse!)
6. **Väljundi andmete asukoht**: valige "Azure"
7. **Azure'i salvestusruumikonto nimi**: seotud klaster salvestusruumi konto
8. **Azure'i salvestusruumi konto võti**: salvestusruumi konto võti seostatud klaster.
9. **Azure'i container nimi**: kui kobar nimi on "abc", siis see on lihtsalt "abc", tavaliselt.


Pärast **Andmete importimine** lõppemist (näete roheline linnuke moodul) andmete toomine andmed salvestada andmekomplekt (koos teie valitud nimi). Kuidas see välja näeb:

![Andmete importimine andmete salvestamine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Paremklõpsake **Andmete importimine** mooduli väljund porti. See näitab **andmekomplekti nimega salvestamine** suvand ja **Visualiseeri** suvand. Suvandi **Visualiseeri** klõpsamisel kuvatakse 100 read andmete koos paremal, mis on abiks mõned kokkuvõte, statistika. Kui soovite salvestada andmed, lihtsalt valige **Salvesta nimega andmekomplekti** ja järgige juhiseid.

Valige salvestatud andmekomplekti kasutamiseks masina õ katse, leidke andmekomplektide, **Vt järgmist joonist otsinguvälja** kaudu. Lihtsalt tippige välja nimi andsite andmekomplekti osaliselt juurde pääseda, seda ja lohistage andmekomplekti peamised paneel. Kukutage see peamine paanile valib selle seadme õ modelleerimine kasutamiseks.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Tehke seda nii rongis ja andmekomplektide test. Pidage meeles ka andmebaasi nimi ja tabelite nimed, mida te andsite selleks kasutada. Joonisel kasutatavaid väärtusi on mõeldud vaid joonisel purposes.* *

### <a name="step2"></a>Samm 2: Looge lihtsa katse Azure seadme õppe prognoosida hiireklõpsuga / pole hiireklõpsuga

Meie Azure'i ML katse näeb välja umbes järgmine:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Nüüd uurime põhikomponentide sellest katsest. Meeldetuletuseks, läheb vaja lohistage meie salvestatud rong ja andmekomplektide edasi meie katse lõuend esmalt testida.

#### <a name="clean-missing-data"></a>Puhastada puuduvad andmed

Mooduli **Clean puuduvad andmed** ei, mis see nimi viitab: see puhastab puuduvad andmed viisidel, mis võib olla kasutaja määratud. Kas soovite selle mooduli sisse, näha:

![Puhasta puuduvad andmed](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Siin valisime kõigi puuduvate väärtuste asendamine 0. On ka muid võimalusi, kus rippmenüüst mooduli vaadates näha.

#### <a name="feature-engineering-on-the-data"></a>Matemaatika erifunktsioonid andmete esiletõstmine

Ei saa olla miljoneid kordumatuid väärtusi suurte andmekogumite kategooriline elemendi jaoks. Ühe kuum kodeering tähistav selliseid kõrge mitmedimensioonilised kategooriline funktsioone nagu varem meetodil on täiesti võimatu. Ülevaate, näitame kasutades sisseehitatud Azure seadme õ moodulid luua tihendatud esinduste nende kõrge mitmedimensioonilised kategooriline muutujate loendamine funktsioonide kasutamise kohta. – Tulemus on väiksem mudel ja kiiremini koolitus korda jõudluse mõõdikute, mis on üsna võrreldavad abil teiste meetodite abil.

##### <a name="building-counting-transforms"></a>Muudab koostamise loendamine

Loendamine funktsioonide koostamiseks, kasutame **Koostada lugedes muuta** moodul, mis on saadaval Azure seadme õ. Mooduli näeb välja umbes järgmine:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Olulise teabe märge** : me Sisestage väljale **veergude arv** nendes veergudes, mille soovime loendab sooritada. Tavaliselt on need (nagu mainitud) kõrge mitmedimensioonilised kategooriline veerud. Me mainitud alguses, et Criteo andmekomplekti on 26 kategooriline veerud: Col15, et Col40 kaudu. Siin me loendada neid kõiki ja anda nende indeksite (15-40 komadega eraldatult, nagu on näidatud).

Mooduli kasutamise MapReduce režiimis (vastav suurte andmekogumite), läheb vaja juurdepääsu mõne Hdinsightiga Hadoopi kobar (selleks otstarbeks saab taaskasutada kasutada funktsiooni uurimine) ja selle mandaat. Eelmise arvud näitavad, millised täidetud väärtused välja näeb (joonisel nendega, mis on oluline oma kasutamine puhul ette nähtud väärtuste asendamine).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Ülaltoodud joonisel näitame kuidas Sisestuskeel bloobimälu asukoha sisestamiseks. Selles asukohas on reserveeritud tuginedes arv tabeli andmeid.


Pärast selle mooduli on töö lõpetanud, saame säästa teisenduse jaoks hiljem, paremklõpsates mooduli ja valides suvandi **Salvesta nimega transformatsioon** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

Meie katse arhitektuur eespool näidatud andmekomplekti "ytransform2" vastab täpselt salvestatud count teisenduse. Ülejäänud sellest katsest, me oletame, et lugeja loendab loomiseks kasutatud **Koostada lugedes muuta** mooduli kohta mõned andmed ja saate need arvu loendamine funktsioonide rongis luua ja testida andmekomplektide.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Valimise, mis count funktsioonid kaasata raudtee- ja testi andmekomplektide osana

Kui oleme sõnaarvestuse muuta valmis, kasutaja saab valida, milliseid funktsioone kaasata oma rongi ja testimine andmekomplektide **Arv tabeli parameetrite muutmine** mooduli kasutamise. Näitame ainult selle mooduli siin täiendavalt, kuid lihtsat tegelikult Kasuta seda meie katse.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

Sel juhul nagu näha, valisime kasutada lihtsalt log-vastuolus ja ignoreerida tagasi välja veerg. Seadmiseks võite ka parameetrite prügi prügikasti piirmäära, nt mitu pseudo eelneva näiteid lisada Eksponentsilumise, ning kas kasutada mis tahes Laplacian müra või mitte. Kõik need on täiustatud funktsioone ja see on märkida, et vaikeväärtused on hea alguspunkti, funktsioon genereerimine seda tüüpi uute kasutajate jaoks.

##### <a name="data-transformation-before-generating-the-count-features"></a>Enne loendamine funktsioonide andmete teisendus

Nüüd on oluline punkt, muutes meie rong keskenduda ja testida enne tegelikult genereerimine loendamine funktsioonide andmete. Pange tähele, et on kaks **Käivitada R skripti** moodulid kasutanud me count transformatsioon meie andmetele rakendada.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Siin on esimene R skripti:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


Selle R kirjas, saame ümbernimetamine meie veergude nimed "Col1" "Col40". See on count transformatsioon eeldab, et selles vormingus nimed.

Teine R skript, leida positiivsete ja negatiivsete tunnid vahel (klassid 1 ja 0) alladiskreetimine negatiivne klassi järgi. R skript siin kirjeldatakse, kuidas seda teha.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

Selle lihtsa R skripti, kasutame "pos\_neg\_ratio" positiivne ja negatiivne tunnid määrata. See on oluline, mida teha, sest parandamine klassi tasakaalustamatuse tavaliselt on jõudlus liigitamine probleeme, kus klassi jaotus on moonutatud (tagasivõtmine, et meil on meie puhul 3,3% positiivsete ja negatiivsete klassi 96,7%).

##### <a name="applying-the-count-transformation-on-our-data"></a>Rakendades count teisendus meie andmete põhjal

Lõpuks me abil saate **Rakendada teisendus** mooduli rakendamine count teisendusteta meie rongis ja testida andmekomplektide. Selle mooduli võtab salvestatud count transformatsioon ühe sisendina ja muude sisendina rongis või testi andmekomplektide ja tagastab andmete loendamine funktsioonide abil. See on siin kujutatud:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Loendamine funktsioonide väljavõte näevad

See on õpetlik loendamine funktsioonide vaadata meie puhul. Siin näitame väljavõte järgmine:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

See väljavõte näitame, et jaoks veerud, mida me ei arvestata kohta, saame loeb ja logige Lisaks mõni oluline backoffs tõenäosus.

Meil on nüüd olete valmis looma Azure seadme õ mudelit, mis neid ümber andmekomplektide abil. Järgmise jaotise näitame, kuidas seda saab teha.

#### <a name="azure-machine-learning-model-building"></a>Azure'i masina õ mudeli building

##### <a name="choice-of-learner"></a>Õppurite valik

Kõigepealt, on vaja valida õppur. Me meie õppurite kasutamiseks kahe tunni võimendada Otsusepuu kuvamine. Siin on selles õppurite vaikesuvandite.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Meie katse puhul me valige sätte vaikeväärtust. Oleme tähele, et vaikesätteid on tavaliselt mõtestatud ja hea viis saada kiire võrdlusandmeid jõudlust. Saate parandada jõudlust pühkimine parameetrid, kui otsustate, kui teil on võrdlusalus.

#### <a name="train-the-model"></a>Koolitada mudel

Koolitus, lihtsalt autonoomsest mooduli **Rongi mudel** . Selle kaks sisendeid on kaks klassi võimendada otsustuspuu õppurite ja meie rongi andmekomplekti. See on siin kujutatud:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Keskmine mudel

Kui oleme koolitatud mudeli, oleme valmis, klõpsake testi andmekomplekti Keskmine ja oma tegevuse hindamiseks. Me toiming **Keskmine mudeli** moodul, mis on näidatud järgmisel joonisel on **Hinnata mudeli** mooduli koos abil:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Juhis 5: Hinnata mudel

Lõpuks soovime mudeli jõudluse analüüs. Tavaliselt on hea mõõt kahe tunni (kahendarvu) liigitamine probleemid, AUC. See visualiseerida, me ühenduse luua mõne **Hinnata mudeli** mooduli **Keskmine mudeli** moodul nii. Klõpsates moodulit **Hinnata mudeli** **Visualiseeri** liitmine annab väärtuse, nagu järgmine pilt:

![Mooduli Koondandmebaasi mudeli hindamine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

Kahendarvuks (või kahe tunni) liigitamine probleeme, ennustamine täpsuse hea mõõtühik on ala jaotises kõvera (AUC). Järgnevalt näitame meie tulemused meie testi andmekomplekti selle mudeli kasutamine. Saada, paremklõpsake **Hinnata mudeli** mooduli ja seejärel **Visualiseeri**väljund porti.

![Mudeli hindamine mooduli visualiseerimine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Samm 6: Avaldada mudeli edastamine veebiteenusele
Võimalus avaldada veebiteenuste koos ühiskasutama Azure seadme õ mudelit, mis on väärtuslik laiemalt kättesaadavaks tegemiseks. Pärast seda, kõik saavad helistada veebiteenuse sisendandmete, et nad vajavad prognoose ja veebiteenuse kasutab mudeli nende ennustatud väärtuste tagastamiseks.

Selleks me esmalt salvestada meie koolitatud mudel koolitatud mudeli objekti. Selleks paremklõpsake **Rongi mudeli** mooduli ja käsk **Salvesta nimega koolitatud mudel** .

Järgmiseks tuleb luua sisendit ja väljund pordid meie veebiteenuse:

* mõne Sisestuskeel port võtab andmed samal kujul, nagu andmed, mida läheb vaja prognoose
* mõne väljundi pordi tagastab viskas sildid ja seotud tõenäosus.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Valige mõni andmeread Sisestuskeel pordi

See on mugav kasutada ka **Rakendada SQL-i transformatsioon** mooduli vaid 10 read olla Sisestuskeel port andmete valimiseks. Valige lihtsalt nende ridade andmete meie Sisestuskeel Port joonisel SQL-päringu abil.

![Andmete sisestamise port](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Veebiteenuse
Nüüd on väike katse, mida saab avaldada meie veebiteenuse käivitamiseks valmis.

#### <a name="generate-input-data-for-webservice"></a>Luua andmeid webservice

Sammuna zeroth Kuna count tabel on suur, võtame paar rida testi andmeid ja luua selle väljundi andmete loendamine funktsioonide abil. See võib olla meie webservice sisendandmete vorming. See on siin kujutatud:

![Koondandmebaasi sisendandmete loomine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Sisendandmete vormingu, nüüd kasutada **Count Featurizer** mooduli väljund. Kui see katse on töö lõpetanud, salvestada väljund moodulist **Count Featurizer** Andmekomplekt. Tuvastatavate kasutatakse funktsiooni webservice sisestatud andmed.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Hinded katse jaoks avaldamise webservice

Esmalt näitame, milline see välja näeb. Oluliste struktuur on **Keskmine mudeli** moodul, mis aktsepteerib meie koolitatud mudeli objekti ja mõned read sisendandmete, mida me eelmistes toimingutes **Count Featurizer** mooduli kasutamise kohta. Kasutame "Valige veerud rakenduses andmekomplekti" projekti Scored sildid ja Keskmine tõenäosus.

![Valige veerud andmekomplekti](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Pange tähele, kuidas saab kasutada **Veergude valimine andmekomplekti** mooduli 'filtreerimise' andmete andmekogumi. Siin sisu kuvamine

![Andmekomplekti moodulis valige veeru filtreerimine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Sinine sisestus- ja väljundi pordid saamiseks võite lihtsalt nuppu **ettevalmistamine webservice** paremas allnurgas. Töötab see katse võimaldab avaldada veebiteenuse: ikooni **Avaldamine VEEBITEENUSE** , paremas allnurgas joonisel:

![Veebiteenuse avaldamine](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Kui funktsiooni webservice on avaldatud, saada meil ümber lehe, mis näeb välja nii:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Näeme kaks linkide webservices vasakus servas:

* **Taotluse/vastuse** teenuse (või RRS) on mõeldud ühe prognoose ja on, mida me kasutada see.
* **Paketi täitmise** teenuse (BES) kasutatakse paketi prognoosimiseks ja nõuab, et sisendandmete saab teha asu Azure'i bloobimälu prognoose.

Klõpsake linki **Taotluse/vastuse** suunab meile leht, mis annab meile eelnevalt konserveeritud kood C#, python ja R. Järgmine kood saab mugavalt kasutada funktsiooni webservice kõnede tegemiseks. Pange tähele, et sellel lehel API võti kasutatakse autentimist.

See on mugav üle selle python koodi kopeerimiseks IPython märkmikus uus lahter.

Siin näitame lõigust python koodi õige API võti.

![Python kood](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Pange tähele, et saaksime vaikimisi API võti asendada meie webservices API võti. Klõpsake nuppu **Käivita** See lahter IPython märkmikus annab järgmise vastuse:

![IPython vastus](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Näeme, et kahe testi näiteid me küsis (raames JSON python skript), saame tagasi vormi vastused "Scored siltide Scored tõenäosus". Pange tähele, et sel juhul valisime, vaikeväärtused, mis pakub eel-konservid koodi (0 kõik arvulised veerud ja string "value" kategooriline kõigi veergude jaoks).

Sellega lõpeb meie lõpuni kiirtutvustus näitab, kuidas toime suuremahuliste andmekomplekti Azure seadme õ abil. Me Teratavu andmete alustamine, ehitatud ennustamine mudeli ja juurutatud veebiteenuse pilveteenuses.
