<properties
    pageTitle="Ülevaade andmete teadus säde kasutamine Windows Azure Hdinsightiga | Microsoft Azure'i"
    description="Säde MLlib tööriistakomplekt toob märkimisväärset masina õ modelleerimise võimalusi jaotatud Hdinsightiga keskkonnas."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Ülevaade andmete teadus säde kasutamine Windows Azure Hdinsightiga

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Selle komplekti teemade näitab, kuidas kasutada Hdinsightiga säde levinud andmete teadus toimingute tegemiseks andmete manustamisest, funktsioon matemaatika, modelleerimine ja mudeli hindamise. Kasutatud andmed on valim aia 2013 NYC takso reisi ja hind andmekomplekti. Sisseehitatud mudelid sisaldavad logistika ja lineaarse regressioonisirge, juhusliku metsade ja astmiku võimendatud puude. Teemade samuti näitab, kuidas salvestada need mudelid Azure'i bloobimälu (WASB) ja Keskmine ja hindate sõnastikupõhise töötamise kohta. Täpsemate teemade kataks, kuidas mudeleid saab koolitada rist – valideerimise ja hyper-parameeter pühkimine abil. Ülevaade selles teemas kirjeldatakse ka häälestamine säde kobar, mida peate täitma kolme juhendavad tutvustused, mis on esitatud juhiseid. 

[Spark](http://spark.apache.org/) on avatud lähtekoodi paralleelselt, mis töötlemine, mis toetab-mälu töötlemine andmete – suured analüütiline rakenduste jõudlust parandada. Säde töötlemine engine on loodud kiiruse hõlpsamaks kasutamine ja keeruka analüüsi jaoks. Säde tema mälu-jaotatud arvutus võimaluste oleks hea valik iteratiivne algoritmide masina õppimine ja Graphi arvutuste. [MLlib](http://spark.apache.org/mllib/) on säde 's scalable masina õ teek, mis toob selle jaotatud keskkonna modelleerimise võimalusi. 

[Hdinsightiga säde](../hdinsight/hdinsight-apache-spark-overview.md) on avatud lähtekoodi säde Azure majutatud pakkumine. See hõlmab ka tugi **Jupyter PySpark märkmike** säde klaster töötavad säde SQL-interaktiivne päringud muutes, filtreerimise ja visualiseerimine andmeid, mis on talletatud Azure plekid (WASB). PySpark on säde Python API-ga. Koodilõigud, mis pakuvad lahendusi ja Kuva oluline proovitükid siin käivitamiseks Jupyter märkmike installitud säde rühmad andmete visualiseerimiseks. Modelleerimine juhised järgmiste teemade sisaldavad koodi, mis näitab, kuidas koolitada, hinnata, salvestamine ja tarbimine igat tüüpi mudel. 

Häälestamise juhised ja lähtutud juhendis kood on Hdinsightiga 3.4 säde 1,6. Siiski koodi siin ja märkmikud on üldine ja mis tahes säde kobar peaks töötama. Kui te ei kasuta Hdinsightiga säde, kobar häälestamise ja haldamise toimingud võivad olla veidi erineda, mis on näidatud siin.

## <a name="prerequisites"></a>Eeltingimused

1. peate Azure tellimuse. Kui te ei ole veel üks, leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. peate mõne HDInsight 3.4 säde 1,6 kobar juhendis lõpuleviimiseks. Ühenduse loomiseks leiate juhiseid [Alustamine: Apache Spark loomine Windows Azure Hdinsightiga](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Kobar tüüp ja versioon on määratud menüüst **Kobar tüübi valimine** . 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Vaadake teema, mis näitab, kuidas kasutada Scala asemel Python teadus-lõpuni andmete protsessi toimingute tegemiseks, [andmete teadus kasutamine koos säde Azure Scala](machine-learning-data-science-process-scala-walkthrough.md).

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 takso andmed

NYC taksosõidu andmed on umbes 20 GB tihendatud komaga eraldatud väärtuste (CSV) faili (~ 48 GB tihendamata), mis sisaldab rohkem kui 173 miljonit üksikute reisi ja hinnad makstud iga reisi. Iga reisi kirje sisaldab Vali üles ja tagastamise koht ja kellaaeg, anonüümsete hack (juht) litsentside arvu ja medallion (takso on kordumatu id) arvu. Andmete hõlmab kõiki reisi aasta 2013 ja on järgmised kaks andmekogumite ettenähtud iga kuu.

1. 'Trip_data' CSV-failid sisaldab reisi üksikasju, näiteks kandevõime kättesaamine ja dropoff andmepunktide, reisi kestus ja reisi pikkus. Siin on mõned valimi kirjed.

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


Meil on võtnud 0,1% valimi need failid ja liitunud reisi\_andmed ja reisi\_hind CV-d failide sisse ühe andmekomplekti juhendis Sisestuskeel andmekomplekti kasutada. Kordumatu liituda reisi\_andmed ja reisi\_hind koosneb väljad: medallion hack\_litsents ja komplekteerimine\_kuupäev ja kellaaeg. Iga kirje andmekogumi sisaldab tähistav NYC taksosõidu järgmisi atribuute:


|Väli| Lühikirjeldus
|------|---------------------------------
| Medallion |Anonüümsete takso medallion (takso kordumatu id)
| hack_license |    Anonüümsete Hackney Carriage litsentside arv
| vendor_id |   Takso tarnija id
| rate_code | NYC takso määr, hind
| store_and_fwd_flag | Salvestamine ja edasisaatmine lipp
| pickup_datetime | Kuupäev ja kellaaeg kättesaamine
| dropoff_datetime | Dropoff kuupäev ja kellaaeg
| pickup_hour | Tund kättesaamine
| pickup_week | Aasta nädal kättesaamine
| WEEKDAY | WEEKDAY (vahemikus 1 – 7)
| passenger_count | Kandevõime takso reisi
| trip_time_in_secs | Reisi aeg sekundites
| trip_distance | Ärireisi läbitud miili
| pickup_longitude | Laiuskraadide kättesaamine
| pickup_latitude | Laiuskraad kättesaamine
| dropoff_longitude | Dropoff laiuskraadide
| dropoff_latitude | Dropoff laiuskraad
| direct_distance | Valige vahelise kauguse otse üles ja dropoff asukohad
| payment_type | Makse tüüp (cas, krediitkaardi jne)
| fare_amount | Hind summa
| lisatasu eest | Lisatasu eest
| mta_tax | MTA maksud
| tip_amount | Näpunäide summa
| tolls_amount | Kasutusmaksu summa
| total_amount | Kogusumma
| otsaga | Kaldus (0 ja 1 ei või Jah)
| tip_class | Näpunäide klassi (0: $0, 1: $0-5, 2: $6-10, 3: 11-20, 4: > 20)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Käivitada kood Jupyter märkmiku säde klaster 

Saate käivitada Jupyter märkmiku Azure portaalist. Oma armatuurlaual klaster säde otsimine ja klõpsake seda haldamise lehel sisestage klaster. Seotud säde kobar märkmiku avamiseks klõpsake **Kobar armatuurlaudade** -> **Jupyter märkmik** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Samuti saate sirvida ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** Jupyter märkmikele juurdepääsuks. Seda URL-i CLUSTERNAME osa asendamine oma kobar nime. Teil on vaja juurdepääsu märkmikud oma administraatori konto parool.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Valige PySpark kataloogi, mis sisaldab mõned näited eelnevalt pakitud märkmikke, mis kasutavad PySpark API kuvamiseks. Märkmike, koodinäiteid selle komplekti säde teema sisaldavate on saadaval [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Saate üles laadida otse Github märkmikud märkmiku Jupyter server klaster säde kohta. Teie Jupyter avalehel nuppu **üles** ekraani parempoolses osas. File Exploreris avaneb. Siin saate kleepida URL-i märkmiku Github (töötlemata sisu) ja klõpsake nuppu **Ava**. Märkmike PySpark on saadaval järgmisel URL.

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Kuvatakse selle asemel faili nime loendis Jupyter faili **üleslaadimine** nupuga uuesti. Klõpsake selle **üleslaadimiseks** nuppu. Nüüd olete importinud märkmik. Korrake neid juhiseid üles laadida: juhendis märkmikud.

> [AZURE.TIP] Saate brauseris linkide paremklõpsake ja valige **Kopeeri Link** , et saada github töötlemata sisu URL. Jupyter üles file explorer dialoogiboksi saate seda URL-i kleepida.

Nüüd saate teha järgmist.

- Vt koodi, klõpsates nuppu märkmik.
- Käivitada iga lahtrit, vajutades klahvikombinatsiooni **SHIFT + ENTER**.
- Käivitada kogu märkmiku klõpsates **lahtri** -> **käivitada**.
- Kasutage päringute automaatne visualiseerimine.

> [AZURE.TIP] PySpark tuum tänav visualiseeritakse automaatselt väljund (HiveQL) SQL-päringud. Suvandi valimiseks mitmesuguseid eri tüüpi visualiseeringud (tabel, sektordiagramm, rea, ala või riba) vahel, kasutades nuppe **Tüüp** menüü märkmikus on esitatud:

![Logistilise regressiooni ROC kõvera üldise lähenemisviisi](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Mis saab edasi?

Nüüd, kui on loodud mõne Hdinsightiga säde kobar ja Jupyter märkmike alla laadinud, olete valmis töö kaudu teemad, mis vastavad kolme PySpark märkmikud. Ta näitab, kuidas andmete analüüsimine ja seejärel kuidas luua ja kasutada mudelite. Täiustatud andmete uurimine ja modelleerimine märkmiku näitab, kuidas kaasata rist – valideerimise, hyper-parameeter pühkimine, ja hindamise mudel. 

**Andmete uurimine ja modelleerimine säde:** Andmekomplekti uurimine ja luua, Keskmine, ja hinnata masina õppe mudelid töö kaudu [luua kahendarvu liigitamine ja regressiooni mudelid andmete säde MLlib tööriistakomplekt](machine-learning-data-science-spark-data-exploration-modeling.md) teema järgi.

**Modelleerimise tarbimine:** Keskmine liigitamine ja regressiooni mudelid luuakse selle teema kohta leiate teavet teemast [Keskmine ja hindate säde ehitatud masina õppe mudelid](machine-learning-data-science-spark-model-consumption.md).

**Rist – valideerimise ja hyperparameter pühkimine**: kuidas mudeleid saab [Täpsemad andmete uurimine ja modelleerimine säde](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) näidata väljaõppe rist – valideerimise ja hyper-parameeter pühkimine abil

