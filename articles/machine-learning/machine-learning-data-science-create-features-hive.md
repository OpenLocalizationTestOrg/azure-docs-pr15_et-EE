<properties
    pageTitle="Loomine Hadoopi kobar, mis taru päringute kasutamine andmete jaoks | Microsoft Azure'i"
    description="Näiteid taru päringute andvate funktsioonid on Azure Hdinsightiga Hadoopi kobar talletatavad andmed."
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Hadoopi kobar, mis taru päringute kasutamine andmete funktsioonid loomine

Selle dokumendi näitab, kuidas luua funktsioone, mis on talletatud Azure Hdinsightiga Hadoopi kobar, mis taru päringute kasutamine andmete jaoks. Need taru päringud kasutada manustatud taru kasutaja määratletud funktsioonid (UDF), skriptide, mille jaoks on olemas.

Toimingud, mis on vaja luua funktsioonid võivad olla mälu intensiivne. Taru päringute jõudlust muutub rohkem kriitiline sellisel juhul ja häälestades teatud parameetrite parandada. Lõplik jaotises käsitletakse järgmiste parameetrite häälestamine.

Päringud, mis esitatakse näiteid [NYC takso reisi andmete](http://chriswhong.com/open-data/foil_nyc_taxi/) teatud stsenaariumide on toodud ka [Github hoidla](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Need päringud juba on määratud andmete skeem ja käivitamiseks valmis. Lõplik jaotises käsitletakse ka parameetrid, mida kasutajad saavad häälestada nii, et taru päringute jõudlust parandada.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]See **menüü** erinevaid funktsioone andmete loomist kirjeldavate teemade lingid. See toiming on juhises [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on:

* Loodud Azure storage konto. Kui vajate juhiseid, lugege teemat [Azure Storage konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account)
* Ette valmistatud kohandatud Hadoopi kobar Hdinsightiga teenusega.  Kui vajate juhiseid, lugege teemat [Kohandamine Azure Hdinsightiga Hadoopi kogumite Analyticsi täpsemalt](machine-learning-data-science-customize-hadoop-cluster.md).
* Andmed on laaditud Azure Hdinsightiga Hadoopi rühmades taru tabelitele. Kui see pole, järgige [taru tabelite loomine ja laadi andmed](machine-learning-data-science-move-hive-tables.md) esmalt taru tabelite andmete üleslaadimiseks.
* Lubatud Kaugpöördusteenuse klaster. Kui vajate juhiseid, lugege teemat [Accessi pea sõlm Hadoopi kobar](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Funktsioon genereerimine

Selles jaotises kirjeldatakse mitmesuguseid võimalusi, kus saate luua funktsioone, taru päringute kasutamine. Kui olete loodud lisafunktsioone, saate neid lisada olemasolevasse tabelisse veergude või uue tabeli loomine lisavõimalusi ja seejärel liita koos algse tabeli primaarvõti. Siin on toodud näidetes on:

1. [Sagedus vastavalt funktsiooni genereerimine](#hive-frequencyfeature)
2. [Riske või kategooriline muutujate kahendarvu liigitamine](#hive-riskfeature)
3. [Ekstraktida välja kuupäeva ja kellaaja funktsioonid](#hive-datefeatures)
4. [Funktsioonide ekstraktida teksti väli](#hive-textfeatures)
5. [GPS koordinaadid vahelise kauguse arvutamine](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Sagedus vastavalt funktsiooni genereerimine

Sageli on kasulik tasemeid kategooriline muutuja sagedus või teatud kombinatsioone tasanditel mitme kategooriline muutuja sagedus arvutada. Kasutajad saavad kasutada järgmist skripti arvutada nende sagedus:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Riske või kategooriline muutujate kahendarvu liigitamine

Binaarsed liigitamine, peame mittearvulise kategooriline muutujate teisendada arvuline funktsioone kui mudelid kasutusel ainult arvulised funktsioone. Seda tehakse, asendades iga taseme mittearvulise arvuline riski. Selles jaotises kuvame üldise taru päringute kategooriline muutuja risk väärtusi (log tõenäosus).


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Selles näites muutujate `smooth_param1` ja `smooth_param2` määratakse sujuv risk andmete põhjal arvutatud väärtused. Riskid on vahemikus -Inf ja Inf vahel. Riske > 0 näitab, et tõenäosus, et eesmärk on võrdne 1 on suurem kui 0,5.

Pärast riski arvutatakse tabeli kasutajad saate määrata risk väärtuste tabeli, ühendades see risk tabel. Taru liitumist päringu anti eelmisest jaotisest.

###<a name="hive-datefeatures"></a>Kuupäeva ja kellaaja väljad väljavõte funktsioonid

Taru kaasas komplekt UDF töötlemiseks kuupäeva ja kellaaja väljad. Kuupäeva ja kellaaja vaikevormingu on taru, "yyyy-MM-dd 00:00:00" ("1970-01-01 12:21:32" näiteks). Selles jaotises näitame näiteid, et eraldada päeva, kuu, kuu kuupäeva ja kellaaja välja ja teisi näiteid, mis vormingus datetime stringi teisendada, kui stringi kuupäeva ja kellaaja vaikevormingu vaikimisi vormindamine.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Selle taru päringu eeldab, et selle *& #60; kellaajavälja >* on kuupäeva ja kellaaja vaikevormingus.

Kui välja kuupäeva ja kellaaja vaikevormingu pole, peate esmalt Unix ajatempel kuupäeva ja kellaaja välja teisendamine, ja seejärel teisendage Unix ajatempel kuupäeva ja kellaaja string, mis on vaikevormingu. Kui soovitud kuupäev ja kellaaeg on vaikimisi vorming, saavad kasutajad rakendada manustatud kuupäeva ja kellaaja UDF-ID eraldamiseks funktsioone.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Selles päringus, kui selle *& #60; kuupäeva ja kellaaja välja >* mustri, nagu on *03/26/2015 12:04:39*, on *"& #60; mustri kuupäeva ja kellaaja välja >"* peaks olema `'MM/dd/yyyy HH:mm:ss'`. Katsetada, saate käivitada kasutajad

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* selles päringus on eelinstallitud klõpsake kõigi Azure Hdinsightiga Hadoopi kogumite vaikimisi kui rühmad on ette valmistatud.


###<a name="hive-textfeatures"></a>Funktsioonide ekstraktida tekstiväljad

Kui taru tabelil on tekstiväli, mis sisaldab sõnu, mis on eraldatud tühikutega string, järgmine päring ekstraktib string ja stringi sõnade arvu.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Arvutamine GPS koordinaadid komplekti vahel

Selles jaotises päringu saab NYC takso reisi andmed otse rakendada. Selle päringu eesmärk kuvamiseks taru luua funktsioonid on manustatud matemaatiliste funktsioonide rakendamise kohta.

Väljad, mida kasutatakse selles päringus on GPS koordinaadid komplekteerimine ja dropoff asukohad, nimega *komplekteerimine\_laiuskraadide*, *komplekteerimine\_laiuskraad*, *dropoff\_laiuskraadide*, ja *dropoff\_laiuskraad*. Päringud, mis arvutavad otsese vahemaa komplekteerimine ja dropoff koordinaate on:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Matemaatilised võrrandid, mis arvutab kahe GPS koordinaadid vahelise kauguse leiate saidil <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Asja tüüp skriptide</a> autoriks Peter Lapisu. Oma JavaScript funktsiooni `toRad()` on lihtsalt *lat_or_lon*pii/180*, mis teisendab kraadid radiaanideks. Siin *lat_or_lon * on laiuskraad või laiuskraadide. Kuna taru ei paku funktsiooni `atan2`, kuid pakub funktsiooni `atan`, `atan2` funktsioon rakendatakse `atan` funktsioon ülaltoodud taru päringu abil antud <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>määratlus.

![Tööruumi loomine](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Täieliku loendi leiate siit leiate ülevaate manustatud **Sisseehitatud funktsioonid** jaotises <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache taru viki</a>taru).  

## <a name="tuning"></a>Täpsemad Teemad: Tune taru parameetrid päringu kiiruse parandamiseks

Taru kobar parameetri vaikesätted ei pruugi sobib taru päringud ja päringute töötlemise on andmeid. Selles jaotises käsitleme mõned parameetrid, mida kasutajad saavad häälestada taru päringute jõudluse parandamiseks. Kasutajate tuleb lisada parameetri häälestamine päringute enne päringute andmete töötlemiseks.

1. **Java hunnik ruumi**: päringuid suurte andmekogumite liitumisel või töötlemine kaua kirjeid, **otsa hunnik ruumi** on üks levinud viga. See saab häälestatud seadmisega parameetritel *mapreduce.map.java.opts* ja *mapreduce.task.io.sort.mb* soovitud väärtused. Siin on näide:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Seda parameetrit eraldab 4GB mälu Java hunnik ruumi ja ka muudab sortimise efektiivsemaks jagamisega rohkem mälu seda. See on hea mõte esitamiseks nende jaotus, kui määratud on mõni töö nurjumise hunnik ruumi seotud tõrgete.

2. **DFS blokeerida suurus** : see parameeter määrab väikseim üksus andmeid, mis talletab failisüsteemis. Näiteks kui DFS Blokeeri maht on 128MB, klõpsake mis tahes andmete maht väiksem kui ja kuni talletatakse 128MB ühe rühmana, samal ajal andmeid, mis on suurem kui 128MB on määratud eest plokid. Väga väike Blokeeri suurus põhjustab suure üldkulud Hadoopi Kuna sõlme nimi on oluline blokeering seotud faili otsimiseks palju veel taotlused. Soovitatav säte, kui tegelevad gigabaiti (või suurema) andmed on:

        set dfs.block.size=128m;

3. **Optimeerimine liitumine taru toiming** : ajal ühendamistoimingute MAPI/vähenda raames tavaliselt toimuda vähendamine etapp vahel suuri kasu saavutamiseks plaanimine ühenduste kaardi etapis (nimetatakse ka "mapjoins"). Selleks võimaluse taru suunamiseks me saate määrata.

        set hive.auto.convert.join=true;

4. **Määrab, et taru mappers mitu** : ajal Hadoopi võimaldab kasutajal määrata pärast arvu, mappers arv on tavaliselt kasutaja ei määrata. Aga, mis võimaldab juhtelemendi seda numbrit teatav on valida Hadoopi muutujate, *mapred.min.split.size* ja *mapred.max.split.size* iga tööülesande kaardi suurus määratakse:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    *Mapred.min.split.size* vaikeväärtus on tavaliselt 0, mis *mapred.max.split.size* **Long.MAX** ja mis *dfs.block.size* on 64 MB. Nagu näeme, andmete suurust, arvestades häälestamine järgmiste parameetrite ", milles" need võimaldab meil häälestada mappers kasutatud arv.

5. Mõne muu veel **Täpsemad suvandid** taru jõudluse optimeerimiseks on allpool. Need võimaldavad teil määrata vastendamine ja tööülesannete vähendamiseks eraldatud mälu ja võib olla kasulik tutistamine jõudlust. Pidage meeles, et *mapreduce.reduce.memory.mb* ei saa olla suurem kui iga töötaja sõlme Hadoopi kobar füüsilise mälu suurust.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
