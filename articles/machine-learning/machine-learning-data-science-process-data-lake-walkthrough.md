<properties
    pageTitle="Skaleeritav andmete teadus Azure'i andmed Lake: on-lõpuni kiirtutvustus | Microsoft Azure'i"
    description="Kuidas kasutada Azure andmete Lake andmete uurimine ja kahendarvuks liigitamine tööülesandeid andmekogumis teha."  
    services="machine-learning"
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
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Skaleeritav andmete teadus Azure'i andmed Lake: on-lõpuni kiirtutvustus

Juhendis näitab, kuidas Azure'i andmed Lake abil teha andmete uurimine ja kahendarvu liigitamine tööülesannete NYC taksosõidu valimil ja hind andmekomplekti prognoosida, kas Näpunäide tasub hind. See juhendab teid [Meeskonnatöö andmete teadus protsessi](http://aka.ms/datascienceprocess)juhiseid--lõpuni, andmeid tuua mudel koolitus ning seejärel veebiteenus, mis avaldab mudeli kasutuselevõtt.


### <a name="azure-data-lake-analytics"></a>Azure'i andmeanalüüsi Lake

[Microsoft Azure'i andmed Lake](https://azure.microsoft.com/solutions/data-lake/) on kõik vajalikud võimalused hõlpsalt andmeteadlaste suurus, kuju ja kiiruse andmete talletamiseks ja andmete töötlemise, täiustatud analüüsi ja seadme Õppekeskuse modelleerimine koos kõrge skaleeritavus viisil.   Maksate töö kohta eraldi, vaid siis, kui andmed on tegelikult töödeldud. Azure'i Lake andmeanalüüsi sisaldab U-SQL-i, jaotatud keel, mis ühendab deklaratiivseid laadi SQL-i abil anda scalable C# väljendusvõimalustega power query võimalus. See võimaldab teil töötlemine struktureerimata andmed skeemi rakendamise kohta vt, lisada kohandatud loogika ja kasutaja määratletud funktsioonid (UDF) ja sisaldab laiendatavuse peene tekstuuriga juhtida, kuidas täita tasandil. Kujundus filosoofia U-SQL-i kohta leiate lisateavet teemast [Visual Studio ajaveebipostitusest](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Andmeanalüüsi Lake on ka Cortana Analytics komplekti ja töötab SQL Azure'i andmebaas, Power BI ja andmete Factory oluline osa. Tulemuseks on lõpule viidud pilve big data ja täiustatud analüüsi platvormi.

Juhendis algab, mis kirjeldab eeltingimused ja ressursse, mis on vajalikud andmed Lake Analytics andmete teadus protsess ja kuidas installida neid moodustavaid toimingu lõpuleviimiseks. Siis esitatakse U-SQL-i kasutamine andmete töötlemiseks juhiseid ja Lõpetuseks näitab, kuidas kasutada Python ja taru Azure seadme õ Studio abil luua ja juurutada soovitud prognoosmudelite. 


### <a name="u-sql-and-visual-studio"></a>A-SQL-i ja Visual Studio

Juhendis soovitab redigeerimine U-SQL-i skripte töötlemine andmekomplekti Visual Studio abil. Skriptide U-SQL-is on siin kirjeldatud ja esitatud hoitakse eraldi failis. Protsessi sisaldab söömisega, uurida ja proovide andmed. Samuti näitab käivitamise skriptitud U-SQL-i töö Azure portaalist. Taru tabelid on loodud andmed on seostatud Hdinsightiga kobar building ja Azure seadme õ Studios kahendarvu liigitamine mudeli kasutuselevõtt.  


### <a name="python"></a>Python

Juhendis sisaldab ka osa, mis näitab, kuidas luua ja juurutada sõnastikupõhise mudeli Python Azure seadme õ Studio abil.  Pakume Jupyter märkmiku Python skripte järgmist toimingut. Märkmiku sisaldab koodi mõned täiendavad funktsiooni engineering etapid ja mudelite ehituse nagu multiclass liigitamine ja regressiooni Lisaks siin kirjeldatud kahendarvu mudeli modelleerimine. Regressioonisirge tööülesanne on prognoosida põhjal muude funktsioonide näpunäite näpunäidet summa. 


### <a name="azure-machine-learning"></a>Azure'i masina õpetused
Azure'i masina õ Studio abil luua ja juurutada soovitud prognoosmudelite. Seda tehakse kahel viisil: esmalt Python skripte ja seejärel taru tabelid on kobar Hdinsightiga (Hadoopi).


### <a name="scripts"></a>Skriptide

Ainult põhisumma juhised on toodud juhendis. Saate alla laadida täielik **U-SQL-i skripti** ja **Jupyter märkmiku** [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Eeltingimused

Enne alustamist järgmisi teemasid, peab teil olema järgmised:

- Azure'i tellimuse. Kui te ei ole veel üks, leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Soovitatav] Visual Studio 2013 või 2015. Kui te pole juba mõne nende versiooni installitud, saate alla laadida tasuta ühenduse edition [siin](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Klõpsake jaotises Visual Studio **allalaadimine ühenduse 2015** nuppu. 

>[AZURE.NOTE] Visual Studio asemel saate ka Azure portaali esitada Azure'i andmed Lake päringud. Pakume juhiseid, kuidas seda teha nii Visual Studio ja jaotises pealkirjaga **töötleb andmeid U-SQL-i**portaal. 

- Azure'i andmete Lake eelvaate kasutajaks registreerumine

>[AZURE.NOTE] Peate kasutama Azure'i andmed Lake poe (ADLS) ja Azure andmete Lake Analytics (ADLA), kui need teenused on eelvaade heakskiidu saamine. Teil palutakse logida, kui loote oma esimese ADLS või ADLA. Sigh üles, klõpsake **eelvaate kuvamiseks registreeruda**, lugege läbi leping ja klõpsake nuppu **OK**. Siin on näiteks ADLS registreeruda lehel:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Andmete teadus keskkonna Azure'i andmed Lake ettevalmistamine
Andmete teadus keskkonna juhendis koostada loomine järgmistest allikatest:

- Azure'i andmesalve Lake (ADLS) 
- Azure'i Lake andmeanalüüsi (ADLA)
- Azure'i bloobimälu salvestusruumi konto
- Azure'i masina õ Studio konto
- Azure'i andmed Lake Tools for Visual Studio (soovitatav)

Selles jaotises antakse juhiseid, kuidas luua iga need ressursid. Kui kasutate taru tabelite Azure seadme õppimisega asemel Python, koostamiseks mudeli, ka peate mõne Hdinsightiga (Hadoopi) kobar ettevalmistamise. See alternatiivne juhiseid artiklis kirjeldatud vastav allpool olevat jaotist.
<br/>
>AZURE'I. Pange tähele, et **Azure'i andmesalve Lake** saab luua kas eraldi või kui loote **Azure'i andmeanalüüsi Lake** vaikimisi salvestamise. Luua iga nende ressursside eraldi allpool viidatud juhiseid, kuid ei vaja andmete Lake salvestusruumi konto luua eraldi.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Azure'i Lake andmesalve loomine

Luua mõne ADLS [Azure portaali](http://portal.azure.com). Lisateavet leiate teemast [Azure portaali loomine mõne Hdinsightiga kobar andmete Lake poe kasutamise](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Ärge unustage häälestamine kobar AAD identiteedi **andmeallika** tera **Valikuline konfigureerimine** tera seal on kirjeldatud. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Azure'i andmeanalüüsi Lake konto loomine
Looge konto ADLA [Azure portaali](http://portal.azure.com). Lisateavet leiate teemast [õpetus: Azure'i Lake andmeanalüüsi Azure'i portaalis alustamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Azure'i bloobimälu salvestusruumi konto loomine
Looge konto Azure'i bloobimälu salvestusruumi [Azure portaali](http://portal.azure.com). Lisateavet leiate teemast [Azure kohta salvestusruumi kontod](../storage/storage-create-storage-account.md)jaotis salvestusruumi konto loomine.
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Azure'i masina õ Studio konto häälestamine
Logige sisse üles- / sisse Azure seadme õ Studio [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) lehe kaudu. Klõpsake nuppu **Alusta kohe** ja seejärel valige "Tasuta tööruumi" või "Standard tööruumi". Pärast seda on võimalik luua katsete Azure'i ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Installige Azure'i Lake Andmeriistad [soovitatav]
Installige Azure'i Andmeriistad Lake oma versioon: [Azure'i andmed Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)Visual Studio.

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Kui installimine on lõpule jõudnud edukalt, avada Visual Studio. Peaksite nägema andmete järve vahekaardil menüü ülaosas. Oma Azure ressursse peaks Azure'i kontosse sisselogimisel kuvatakse vasakul paanil.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Andmekomplekti NYC takso reisi
Oleme siin kasutatud andmehulga on avalikult kättesaadavaks andmekomplekti-- [NYC takso reisi andmekomplekti](http://www.andresmh.com/nyctaxitrips/). Andmete NYC taksosõidu koosneb umbes 20GB tihendatud CSV-failid (tihendamata ~ 48GB), salvestamise rohkem kui 173 miljonit üksikute reisi ja hinnad makstud iga reisi. Iga reisi kirje sisaldab komplekteerimine ja esmase asukohad ja korda, anonüümsete hack (juht) litsentside arvu ja arvu medallion (takso on kordumatu id). Andmete hõlmab kõiki reisi aasta 2013 ja on järgmised kaks andmekogumite ettenähtud iga kuu.

 - 'trip_data' CSV sisaldab reisi üksikasju, näiteks arv reisijate, komplekteerimine ja dropoff punkte, reisi kestus ja reisi pikkus. Siin on mõned valimi kirjed.

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - 'trip_fare' CSV sisaldab üksikasju ja iga reisi makse tüüp, hind summa, lisatasu eest ja maksud, näpunäited ja kasutusmaksu, nt makstud makstud kogusumma. Siin on mõned valimi kirjed.

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Kordumatu liituda reisi\_andmed ja reisi\_hind koosneb kolmest järgmised väljad: medallion hack\_litsents ja komplekteerimine\_kuupäev ja kellaaeg. Töötlemata CSV-failide pääseb juurde avaliku Azure storage bloobimälu. Selle ühenduse U-SQL script on [Liitu reisi ja hind tabelid](#join) jaotises.

## <a name="process-data-with-u-sql"></a>Protsessi andmete U-SQL-i abil

Selles jaotises kirjeldatud andmetöötlus tööülesannete kaasata söömisega, kvaliteedi kontrollimine, uurida ja proovide andmed. Näitame ka reisi ja hind tabelite ühendamine tehke järgmist. Lõplik jaotises kuvatakse Käivita U-SQL-i skriptitud töö Azure portaalist. Siin on lingid iga lõikes.

- [Andmete manustamisest: lugeda andmeid avaliku bloobimälu](#ingest)
- [Andmete kvaliteedi kontrollimiseks](#quality)
- [Andmete uurimine](#explore)
- [Reisi ja hind tabelite ühendamine](#join)
- [Andmete valimite](#sample)
- [Käivitage U-SQL tööde haldamine](#run)

Skriptide U-SQL-is on siin kirjeldatud ja esitatud hoitakse eraldi failis. Saate alla laadida täielik **U-SQL-i skriptide** [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Käivitada U-SQL-i, avage Visual Studio, klõpsake nuppu **faili--> uus--> projekti**, valige **U-SQL-i projekti**nimi ja salvestage see kausta.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] On võimalik kasutada käivitada Visual Studio asemel U-SQL Azure'i portaal. Saate liikuda Azure'i andmeanalüüsi Lake ressursi portaalis ja esitada otse nagu järgmisel joonisel illustreeritud päringuid.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Andmete manustamisest: Lugeda andmeid avaliku bloobimälu

Azure'i bloobimälu olevate andmete asukoha viidatakse nimega **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** ja saab eraldada **Extractors.Csv()**. Asendada oma container nimi ja salvestusruumi konto nime jaoks järgmised skriptide container_name@blob_storage_account_name wasb meiliaadress. Kuna sama vormingus failide nimesid, saame kasutada **reisi\_data_ {\*\}CSV** lugeda 12 reisi kõik failid. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Kuna seal on päised tabeli esimeses reas, läheb vaja Eemalda päised ja muuta veerutüübid sobivaks. Me ei saa kumbki Salvesta töödeldud andmete Azure'i Lake andmesalv abil **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ või Azure'i bloobimälu konto kasutate **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Samuti saate me lugeda hind andmekogumi. Paremklõpsake valikut Azure'i andmesalve Lake ja soovi korral saate vaadata andmete **Azure portaali--> andmete Explorer** või **File Explorer** Visual Studio sees. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Andmete kvaliteedi kontrollimiseks

Pärast tabelite reisi ja hind on loetud sisse, saab teha andmete kvaliteedi kontrollimiseks järgmiselt. Saadud CSV faile saab väljundi Azure'i bloobimälu või Azure andmesalve Lake. 

Medaljonid arv ja kordumatute arvu Medaljonid leidmiseks tehke järgmist.

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Nende Medaljonid, mis olnud üle 100 reisi leidmiseks tehke järgmist.

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Need vigaste kirjete osas pickup_longitude leidmiseks tehke järgmist.

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Mõned muutujad puuduvate väärtuste otsimine

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Andmete uurimine

Me saame teha mõned andmete uurimine andmete parema ülevaate saamiseks.

Jaotuse kaldus ja-otsaga leidmiseks tehke järgmist.

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Näpunäide summa koos piirväärtuse jaotuse otsimine: 0,5,10 ja 20 dollarit.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Lihtsa statistika reisi kauguse leidmiseks tehke järgmist.

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Protsentiilid reisi kauguse leidmiseks tehke järgmist.

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Reisi ja hind tabelite ühendamine

Reisi ja hind tabeleid saab liitunud medallion, hack_license ja pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Iga taseme reisijate arv, kirjete, Keskmine näpunäite summa, dispersioon näpunäite summa protsendi kaldus reisi arvu arvutamine.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Andmete valimite

Esmalt me CEIP valige 0,1% andmete ühendatud tabelist.

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Seejärel tehke stratifitseeritud valimite kahendarvu muutuv tip_class järgi.

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Käivitage U-SQL tööde haldamine

Kui olete U-SQL-i skriptide redigeerimise lõpetanud, võite pöörduda server Azure'i andmeanalüüsi Lake konto kaudu. Klõpsake nuppu Vali **Andmed Lake**, **Esitage töö** **Analytics konto**, valige **paralleelsus**ja klõpsake nuppu **Edasta** .  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Kui töö on kinni edukalt, kuvatakse teie töö oleku Visual Studio jälgimiseks. Pärast töö on töö lõpetanud, saate isegi kordus töö täitmise protsess ja teada saada, kitsaskoht juhiseid oma tööd tõhustada. Samuti saate avada Azure'i portaalis oma A-SQL-i töö oleku vaatamine.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Nüüd saate väljund faili Azure'i bloobimälu või Azure portaali. Kasutame stratifitseeritud Näidisandmete meie modelleerimine järgmises etapis.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Luua ja juurutada Azure seadme õ andmemudelites

Näitame kahe saadaolevad suvandid saate andmete toomine lisandmoodulisse Azure seadme õ koostamiseks ja 

- Esimene variant valimisse andmeid, mis on kirjutatud mõne Azure'i bloobimälu ( **andmete valimite** ülal kirjeldatud toimingus) ja Python abil saate luua ja juurutada Azure seadme õ mudelid. 
- Klõpsake teise võimaluse, saate päringu Azure'i andmed Lake andmed otse taru päringu abil. See suvand nõuab, et luua uus Hdinsightiga klaster või kasutada mõne olemasoleva Hdinsightiga kobar, kus taru tabelid osutage Azure'i andmesalv Lake NY takso andmed.  Käsitleme mõlemad järgmised võimalused. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Variant 1: Kasutamine Python saavad luua ja juurutada masina õppe mudelid

Luua ja juurutada masina õppe mudelid Python abil, et luua Jupyter märkmiku teie kohalikus arvutis või Azure seadme Õppekeskuse Studio. Esitatud [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) Jupyter märkmik sisaldab täielik uurida, visualiseerida andmeid, funktsioon matemaatika, modelleerimine ja juurutamine. Selles artiklis näitame ainult modelleerimine ja juurutamine. 

### <a name="import-python-libraries"></a>Python teekide importimine

Selleks, et käivitada valimi Jupyter märkmikku või Python skript faili järgmised Python paketid on vaja. Kui kasutate teenust AzureML märkmik, on need paketid eelinstallitud.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Bloobimälu andmete lugemine

- Ühendusstring   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Tekstina lugeda

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Lisa veergude nimed ja eraldi veerud

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Muuta mõne veergude arv

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Seadme õ mudelite koostamine

Siin saame luua kahendarvu liigitamine mudel prognoosida, kas reisi on otsaga või mitte. Märkmiku Jupyter leiate muu kaks mudelit: multiclass liigitamine ja regressiooni mudelid.

- Kõigepealt on vaja luua fiktiivsete muutujate, mida saab kasutada scikit – siit saate teada, mudelite

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Andmete modelleerimine raami loomine

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Koolitus ja testimine 60-40 tükeldamine

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistilise regressiooni komplekti koolitus

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Keskmine testimise andmehulgas.

        Y_test_pred = logit_fit.predict(X_test)

- Arvutamine hindamise mõõdikud

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Veebi teenus API luua ja kasutada seda Python

Soovime Õppekeskuse mudeli, kui see on sisseehitatud masina käivitama. Siin kasutada kahendarvu logistilise mudeli näide. Veenduge, et selle scikit – siit saate teada, kui teie kohalikus arvutis versioon on 0.15.1. Te ei pea muretsema, kui kasutate teenust Azure ML studio.

- Leiate Azure'i ML studio sätted tööruumi mandaat. Azure'i masina õ Studio, klõpsake nuppu **sätted** --> **nimi** --> **Autoriseerimine sõned**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Veebiteenuse loomine

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Web mandaati hankimine

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Helistage veebi teenus API. Peate ootama 5 – 10 minutit pärast eelmist toimingut.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Variant 2: Luua ja kasutusele võtta otse andmemudelites Azure seadme õ

Azure'i masina õ Studio otse Azure'i andmesalve Lake saate andmeid lugeda ja seda kasutada luua ja juurutada mudelid. Seda moodust kasutab taru tabeli, mis osutab Azure'i andmesalve Lake. Selleks, et ette valmistada eraldi Windows Azure Hdinsightiga kobar, klõpsake taru tabel luuakse. Järgmistes jaotistes näitab, kuidas seda teha. 

### <a name="create-an-hdinsight-linux-cluster"></a>Luua ka Hdinsightiga Linux kobar

Luua mõne Hdinsightiga kobar (Linux) [Azure portaali](http://portal.azure.com). Leiate jaotisest **loomine mõne Hdinsightiga kobar juurdepääsu Azure andmesalve Lake** [loomine mõne Hdinsightiga kobar andmete Lake poe abil Azure portaali](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)sisse.

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Hdinsightiga taru tabeli loomine

Nüüd saame luua taru tabelid kasutada Azure seadme õ Studios Hdinsightiga kobar, kasutades eelmises etapis Azure'i andmesalve Lake talletatud andmed. Avage äsja loodud Hdinsightiga klaster. Klõpsake nuppu **sätted** --> **Atribuudid** --> **Kobar AAD identiteedi** --> **ADLS juurdepääsu**, veenduge, et konto Azure andmesalve Lake lisatakse loendi read, kirjutamine ja käivitada õigused. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Valige **armatuurlaud** kõrval nuppu **sätted** ja avaneb aken. Ja te lehe paremas ülanurgas nuppu **Taru vaade** kuvatakse **Päringuredaktor**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Kleepige järgmine taru skripte tabeli loomiseks. Azure'i andmesalve Lake viite sel viisil andmeallika asukoht: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/file_name**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Kui päring on töö lõpetanud, näete tulemusi umbes järgmine:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Luua ja juurutada mudelite Azure seadme õ Studio

Meil on nüüd valmis luua ja juurutada mudelit, mis prognoosib kas Näpunäide makstakse Azure seadme õppimisega. Stratifitseeritud Näidisandmete on valmis, kasutatakse seda kahendarvu kategooriat (Vihje või mitte) probleemi. Prognoosmudelite, multiclass liigitamine (tip_class) ja regressiooni (tip_amount) abil saate ka ehitatud ja Azure seadme õ Studio juurutada, kuid siin näitame ainult abil kahendarvu mudeli registri reageerimine.

1. Azure'i ML abil **Andmete importimine** moodul, mis on saadaval jaotises **andmete sisestamine ja väljundi** andmeid tuua. Lisateabe saamiseks vaadake [andmete importimine mooduli](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) viide lehte.
2. Valige **Päringu taru** **andmeallika** paneelil **Atribuudid** .
3. Dokumendi kirjutuskaitstuks järgmised taru **taru andmebaasi päringu** redaktoris

        select * from nyc_stratified_sample;

4. Sisestage URI, Hdinsightiga kobar (see leiate Azure'i portaalis), Hadoop mandaat, asukoht ja väljundi andmete Azure'i salvestusruumikonto nimi/key/container nimi.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Näiteks kahendarvu liigitamine katse taru tabelist andmete lugemine on näidatud alloleval joonisel.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Katse on loodud, klõpsake nuppu **Määra veebiteenuse** --> **Sõnastikupõhise veebiteenuse**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Käivitage automaatselt loodud hinded katse, kui see on lõppenud, klõpsake nuppu **Veebiteenus juurutamine**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Web teenuse armatuurlaua kuvatakse vahetult:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Kokkuvõte

Täites juhendis on loonud andmete teadus keskkonna jaoks scalable lõpuni lahenduste Azure'i andmed Lake. Selles keskkonnas kasutati analüüsi suurte avaliku andmekomplekti, võttes selle kanoonilise juhised andmete teadus protsessi kaudu andmeid tuua mudeli koolituse kaudu, ning seejärel nimega veebiteenuse mudeli kasutuselevõtt. U-SQL kasutati töötlemine, uurida ja kuulake andmed. Python ja taru kasutati Azure seadme õ Studio luua ja juurutada prognoosmudelite.

## <a name="whats-next"></a>Mis saab edasi?

Õppeteema jaoks [Meeskonnatöö andmete teadus protsess (TDSP)](http://aka.ms/datascienceprocess) sisaldab linke teemad, mis kirjeldab iga toimingu täiustatud analüüsi käigus. On loetletud lehel [meeskonnatöö andmete teadus protsessi juhendavad tutvustused](data-science-process-walkthroughs.md) juhendavad tutvustused, mis eksponeerivad ressursid ja teenuste kasutamine eri ennustav stsenaariumid sarja.

- [Meeskonnatöö andmete teadus protsessi toiming: abil SQL-andmebaas](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Meeskonnatöö andmete teadus protsessi toiming: Hdinsightiga Hadoopi kogumite abil](machine-learning-data-science-process-hive-walkthrough.md)
- [Meeskonnatöö andmete teadus protsessi: abil SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Ülevaade andmete teadus protsessi säde kasutamine Windows Azure Hdinsightiga](machine-learning-data-science-spark-overview.md)

