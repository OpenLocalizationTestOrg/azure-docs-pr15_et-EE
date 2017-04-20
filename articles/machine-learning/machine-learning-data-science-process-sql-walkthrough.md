<properties
    pageTitle="Meeskond andmete teaduse protsessi tegevuses: kasutades SQL Server | Microsoft Azure"
    description="Täiustatud Analytics protsessi ja tehnoloogia huviorbiidis"  
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
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Meeskond andmete teaduse protsessi tegevuses: SQL serveri abil

Aastal See õpetus, mida teid läbi ülesehitamisel ja juurutamine SQL Serverit ning üldkasutatavate andmekogumis - [NYC Taxi reisid](http://www.andresmh.com/nyctaxitrips/) andmekomplekti masinõpet mudel. Menetlus järgib standardandmetes teaduse töövoo: neelata ja saate andmeid, insener funktsioonid soodustama õppimist, siis ehitada ja juurutada mudel.


## <a name="dataset"></a>NYC Taxi reisid andmekogumi kirjeldus

NYC Taxi reisi andmed on pakitud CSV faile (~ 48GB pakkimata) umbes 20GB, mis koosneb rohkem kui 173 miljonit individuaal Reisid ja hinnad maksta iga reisi. Iga reisi kirje sisaldab pickup ja drop-off asukoht ja aeg, anonüümsete hack (juhi) litsentsi number ja medallion (takso on unikaalne id) number. Andmed hõlmab kõik reisid aastal 2013 ja on toodud järgmised kaks andmekogumite kohta kuus:

1. "Trip_data" CSV sisaldab reisi üksikasju, nagu reisijad, komplekteerimine ja dropoff punkte, reisi kestus ja reisi pikkus. Siin on mõned proovi kirjed.

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. "trip_fare" CSV sisaldab iga reisi nagu liik, piletihind summa, lisatasu eest ja maksud, vihjed ja teemaksud, makstud ja tasutud kogusumma. Siin on mõned proovi kirjed.

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Kordumatu võti liituda reisi\_andmed ja reisi\_hind koosneb väljad: medaljon, hack\_litsentsi ja pikap\_kuupäev ja kellaaeg.

## <a name="mltasks"></a>Ennustus ülesannete näiteid

Me koostab kolm ennustus probleeme vastavalt selle *otsa\_summa*, nimelt:

1. Binaarne liigitus: ennustada, kas otsa maksti reisile, st on *otsa\_summa* mis on suurem kui 0 $, on positiivne näide, samas on *otsa\_summa* $ 0 on negatiivne näide.

2. Multiclass liigitus: ennustada otsa makstud reis vahemikus. Jagame selle *otsa\_summa* viis aluseid või klassid:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regressiooni ülesanne: ennustada summa otsa sõidu eest maksta.  


## <a name="setup"></a>Seade üles Azure andmed teadus keskkonna täiustatud analytics

Nagu näete Guide [Planeerida oma keskkonda](machine-learning-data-science-plan-your-environment.md) , on mitmeid võimalusi töötada NYC Taxi reisid dataset Azure:

- Töö andmetega Azure plekid siis mudel Azure'i masinõppe
- Laadida andmeid SQL serveri andmebaasi siis Azure'i masinõppe mudel

Sel juhendaja näitame paralleelselt mahtlasti impordiväärtused andmete SQL Server, andmete uurimine, funktsioon inseneri ja kehtestatakse proovivõtu abil SQL Server Management Studio, samuti kasutades IPython sülearvuti. [Näidisskriptid](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) ja [IPython märkmikud](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) on ühised GitHub. Proovi IPython märkmiku andmete kasutamine Azure plekid on saadaval ka samas kohas.

Azure andmed teadus keskkonna seadistamine

1. [Luua ladustamise konto](../storage/storage-create-storage-account.md)

2. [Azure'i masinõppe tööruumi loomine](machine-learning-create-workspace.md)

3. [Sätte virtuaalarvuti andmed teadus](machine-learning-data-science-setup-sql-server-virtual-machine.md), mis toimib SQL Server samuti IPython sülearvuti server.

    > [AZURE.NOTE] Näidisskriptid ja IPython märkmikud laaditakse andmed teadus virtuaalarvuti installimise käigus. Kui VM skripti pärast installi on lõpule jõudnud, saab proovid oma VM dokumenditeeki:  
    > - Proovi skriptid:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Proovi IPython märkmikud:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > Kui `<user_name>` on oma VM Windows kasutajanimi. Me suunab proovi kaustadesse **Näidisskriptid** ja **Proovi IPython märkmikud**.


Vastavalt valitud Azure'i eesmärk keskkonna andmekogumi suurus ja andmeallika asukoht, sel juhul on sarnane [stsenaariumi \#5: suure andmekogumi kohalike failide suunatud Azure VM SQL serveri](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Saada andmed avalik allikas

Saada [NYC Taxi reisid](http://www.andresmh.com/nyctaxitrips/) andmekomplekti selle avalikku kohta, võib kasutada [Andmete teisaldamine ja sealt Azure bloobimälu](machine-learning-data-science-move-azure-blob.md) kirjeldatud meetodite virtuaalne seadme andmete kopeerimiseks.

AzCopy andmete kopeerimine

1. Logige sisse oma virtuaalarvuti (VM)

2. Luua uus kataloog ning VM andmed kettale (Märkus: Kasuta ajutist ketas, mis on andmete ketas VM).

3. Käivitage käsuviiba aknas järgmine Azcopy käsurealt, asendades < path_to_data_folder > andmete kausta loodud (2):

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Kui selle AzCopy on lõpule jõudnud, kokku 24 ZIP-CSV faili (12 reis\_andmed ja 12 reis\_hind) peaks olema andmete kausta.

4. Unzip allalaaditud failid. Märkus Kui tihendamata failid asuvad kaustas. Selle kausta edaspidi on < tee\_et\_andmete\_failid\>.

## <a name="dbload"></a>Andmete importimine SQL serveri andmebaasi

Laadimine/üle väga palju andmeid SQL-andmebaasi ja päringutes võimsust saab suurendada kasutades _piiretega tabelid ja vaated_. Selles jaotises jälgime [Paralleelselt lahtiselt andmete Import kasutades SQL partitsiooni tabelid](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) luua uue andmebaasi ja koormuse andmed sektsioonitud tabelisse samal ajal kirjeldatud juhiseid.

1. Olles sisse logitud oma VM, käivitage **SQL Server Management Studio**.

2. Ühendage Windowsi autentimisega.

    ![SSMS ühendust][12]

3. Kui seni on muutunud SQL serveri autentimisrežiimi ja uue SQL sisselogimise kasutaja loodud, avage skripti faili nimega **muuta\_auth.sql** **Näidisskriptid** kausta. Vaikimisi nime ja parooli vahetada. Kliki **! Käivitada** skripti käivitamiseks tööriistaribal.

    ![Käivitada skripti][13]

4. Kontrollimiseks ja/või muuta SQL serveri andmebaasi ja Logi vaikekaustad tagamaks, et uutele andmebaasi salvestatakse andmed kettale. SQL serveri VM pilt, mis on optimeeritud datawarehousing koormused on eelhäälestatud andmete ja logide kettaid. Kui teie VM ei sisaldanud andmeid kettale ja lisatud uue virtuaalse kõvaketta installimisel VM, muuta vaikekaustad järgmiselt:

    - Paremklõpsake soovitud SQL serveri nime vasakul paanil ja klõpsake nuppu **Atribuudid**.

        ![SQL serveri atribuudid][14]

    - Valige **Andmebaasi sätted** loendist **Valige lehe** vasakule.

    - Kontrollimiseks ja/või muuta **andmebaasi vaikeasukohad** **Andmeketas** kohtadele valitud. See on, kui uute andmebaaside ela kui asukoha vaikimisi loodud.

        ![SQL andmebaasi vaikesätted][15]  

5. Uue andmebaasi ja filegroups hoida piiretega tabelid komplekti loomiseks avage skripti näide **luua\_db\_default.sql**. Skript loob uue andmebaasi nimega **TaxiNYC** ja 12 filegroups andmete vaikeasukohta. Iga failirühma korraldab ühe kuu reisi\_andmed ja reisi\_hakkama andmeid. Kui soovitud, muuta andmebaasi nimi. Kliki **! Käivitada** skripti käivitamiseks.

6. Järgmisena Looge kaks partitsiooni tabelid, üks reis\_andmed ja teine reis\_hind. Avage skripti näide **luua\_sektsioonitud\_table.sql**, milles näidatakse:

    - Saate luua sektsiooni funktsiooni Split andmed kuude kaupa.
    - Saate luua sektsiooni kava kaardi iga kuu andmed teise failirühma.
    - Luua kahe sektsioonitud tabeli vastendada Laiendpartitsiooni kava: **nyctaxi\_reis** korraldab reisi\_andmed ja **nyctaxi\_hind** korraldab reisi\_hakkama andmeid.

    Kliki **! Käivitada** skripti käivitada ja luua piiretega tabelid.

7. Kaustas **Näidisskriptid** on kaks proovi PowerShelli skriptide tõendaks paralleelselt lahtiselt impordi andmed SQL serveri tabelid.

    - **bcp\_paralleelne\_generic.ps1** on üldine skripti paralleelse impordi andmete tabelisse. Muuta seda skripti sisendi ja eesmärgi muutujate määramisel vastavalt märkuseridade skripti.
    - **bcp\_paralleelne\_nyctaxi.ps1** on eelhäälestatud versioon üldine skripti ja kasutamist – mõlemas tabelis NYC Taxi reisi andmete laadimiseks.  

8. Paremklõpsake selle **bcp\_paralleelne\_nyctaxi.ps1** skripti nimi ja klõpsake nuppu **Redigeeri** avamiseks PowerShelli. Etteantud muutujate läbi vaadata ja muuta valitud andmebaasi nimi, sisendandmete kausta, Logi sihtkaust ja teed proovi formaadis failid **nyctaxi_trip.xml** ja **nyctaxi\_fare.xml** (ette **Näidisskriptid** kausta).

    ![Andmete Import][16]

    Saate valida ka autentimise režiim, vaikimisi on Windowsi autentimist. Klõpsake rohelist noolt käivitamiseks tööriistaribal. Skript käivitab paralleelselt, 12 iga sektsioonitud tabeli 24 lahtiselt tegevus. Andmete impordi arengut võib jälgida vastavalt eespool SQL serveri andmete kausta avades.

9. PowerShell skripti aruanded algus- ja lõppajad. Kui kõik pakendamata kaupade impordi täielik lõppaeg on teatatud. Sisse Logi sihtkaust kontrollimaks, et suurem osa impordi olid edukad, st ühtegi tõrget ei esitatud eesmärgi Logi kausta.

10. Andmebaasi saab nüüd kasutada avastamine, funktsioon inseneri ja muid soovitud toiminguid. Kuna tabelid on liigendatud vastavalt selle **komplekteerimine\_kuupäev ja kellaaeg** välja, päringuid, mis sisaldavad **pikap\_kuupäev ja kellaaeg** tingimuste punktis **kus** kasu Laiendpartitsiooni kava.

11. **SQL Server Management Studio**, uurida antud proovi skripti **proovi\_queries.sql**. Käivitada ühtegi proovi päringud, valitavate ridade päringut ja klõpsake **! Käivitada** tööriistaribal.

12. NYC Taxi reisi andmed on laaditud kahes eraldi tabelis. Liitumistoimingud parandamiseks on äärmiselt soovitatav indekseeritavad tabelid. Skripti näide **luua\_sektsioonitud\_index.sql** loob sektsioonitud indeksid komposiit liitumise võti **medaljon, hack\_litsents, ja pikap\_kuupäev ja kellaaeg**.

## <a name="dbexplore"></a>Andmete uurimine ja inseneri funktsiooni SQL Server

Selles jaotises teostame andmeuuringu ja funktsiooni põlvkonna töötab SQL päringuid otse kasutades varem loodud SQL serveri andmebaasi **SQL Server Management Studio** . Proovi skripti nimega **proovi\_queries.sql** on nähtud **Näidisskriptid** kausta. Muuta skripti muuta andmebaasi nime, kui see erineb default: **TaxiNYC**.

Seejuures me:

- Ühendada kas Windowsi autentimist või SQL-i autentimise ja SQL sisselogimisnimi ja parool **SQL Server Management Studio** .
- Uurida andmete jaotamine mõned väljad erineval ajal Windows.
- Pikkus- ja laiuskraadide väljad andmete kvaliteeti uurida.
- Luua kahe- ja multiclass klassifikatsioon silte vastavalt selle **otsa\_summa**.
- Luua funktsioonid ja Arvuta/Võrdle trajektoorile.
- Ühendage need kaks tabelit ja juhusliku proovi, mida kasutatakse ehitada mudeleid.

Kui olete valmis jätkama Azure'i masinõppe, võib kas:  

1. Lõplik ekstrakti ja proovi andmeid ja copy-paste päringu otse [Andmete importimine] SQL päringu salvestamine[ import-data] moodul Azure'i masinõppe, või
2. Püsivad valimisse ja tehnilise andmeid kavatsete kasutada hoone uue andmebaasi tabel mudel ja kasutada uue tabeli [Andmete importimine] [ import-data] moodul Azure'i masinõppe.

Selle jaotise Hoiame Kokku lõplik päring ekstrakti ja proovi andmeid. Teine meetod on tõestatud osa [andmeuuringu ja funktsiooni Engineering IPython sülearvuti](#ipnb) .

Ridade ja veergude asustatud varem kasutades paralleelselt mahtlasti impordiväärtused, tabelite arvu kiire kontrollimiseks

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Uurimine: Reis jaotus medallion

Näites tähistab medaljon (takso numbrit) ja üle 100 reisi teatava aja jooksul. Päringu võivad saada kasu on sektsioonitud tabeli kuna see on see tingitud sektsiooni kava **komplekteerimine\_kuupäev ja kellaaeg**. Päringute täieliku andmekogumi teeb kasutavad sektsioonitud tabeli ja/või indekseerida skannimine.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Uurimine: Reis jaotus medallion ja hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Andmete kvaliteedi hindamise eesmärgil: Et külastajad vastavuses vale pikkus ja/või laius

Selles näites uurib kui pool ja/või laius välju sisaldada sobimatut väärtust (Radiaan kraadi peaks olema -90 – 90), või on (0, 0) koordinaadid.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Uurimine: Tipped vs mitte kallutada reisid jaotus

Selles näites leiab reiside, mis olid otsaga vs mitte kallutada ajahetkel periood (või täieliku andmekogumi kui katab terve aasta). See jaotus peegeldab binaarne silt jaotus kasutatakse hiljem binaarne klassifikatsiooni modelleerimine.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Uurimine: Otsa klassi/vahemik jaotus

See näide arvutab otsa vahemikud jaotus teatud kindlas ajavahemikus (või täieliku andmekogumi kui katab terve aasta). See on silt klassid, mida hiljem kasutatakse multiclass liigitamist modelleerimine jaotus.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Uurimine: Arvutada ja võrrelda teepikkuse

Näites teisendab pickup ja drop-off pikkus ja laius SQL geograafia juhib, kasutades SQL geograafia punktide erinevus teepikkuse arvutab ja tagastab juhusliku valimi tulemuste võrdlus. Näiteks piirab tulemused kehtivad ainult kasutades andmete kvaliteedi hindamise päring varem koordinaadid.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Funktsioon inseneri SQL päringuid

Sildi põlvkonna ja geograafia konverteerimise uurimine päringuid saab ka luua sildid/funktsioone eemaldades inventuuri. Täiendav funktsioon engineering SQL näited on esitatud jaotises [andmeuuringu ja funktsiooni Engineering IPython sülearvuti](#ipnb) . On mõttekam funktsioon põlvkonna päringuid käivitada täielik andmekogum või suur osa kasutades SQL päringuid, Comma otse sõitma selle SQL Serveri eksemplari. Päringuid **SQL Server Management Studio**, IPython sülearvuti või mis tahes arengu tööriist/keskkond, mis andmebaasile juurdepääsu kohalikult või kaugühendusega täita.

#### <a name="preparing-data-for-model-building"></a>Andmete ettevalmistamine mudeli ehitamisele

Järgmist päringu ühendused on **nyctaxi\_reis** ja **nyctaxi\_hind** tabelid, tekitab on binaarne klassifikatsiooni silt **kallutada**, mitme klassi liigitamise silt **otsa\_klassi**, ja võtab 1% juhusliku valimi täis liidetud andmekomplekti. Selle päringu saate kopeerida seejärel kleepida otse [Azure masin õppe Studio](https://studio.azureml.net) [Impordiandmete] [ import-data] otsesed andmed allaneelamine Azure SQL serveri andmebaasi astme moodul. Päring ei hõlma vastavuses vale (0, 0) koordinaadid.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Andmete uurimine ja funktsiooni inseneri IPython sülearvuti

Selles jaotises teostame andmeuuringu ja funktsiooni loomine kasutades Python ja SQL päringuid SQL serveri andmebaasist varem loodud. Proovi IPython märkmik nimega **machine-Learning-data-science-process-sql-story.ipynb** on toodud **Proovi IPython märkmike** kaustas. See märkmik on ka saadaval [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Suurandmete töötamisel soovitatav järjestus on järgmine:

- Väikese valimi andmeid lugeda-mälu andmeid raami.
- Teha mõned visualiseeringute ja uuringu valimisse kuuluvate andmete.
- Proovige funktsiooni inseneri valimisse kuuluvate andmetega.
- Suurem andmete uurimiseks, andmete käsitlemise ja funktsiooni inseneri, kasutada Python väljastada otse vastu Azure VM SQL serveri andmebaasi SQL päringuid.
- Otsustada kasutada Azure'i masinõppe mudeli ehitus on valimi.

Kui soovite jätkata Azure'i masinõppe, võib kas:  

1. Lõplik ekstrakti ja proovi andmeid ja copy-paste päringu otse [Andmete importimine] SQL päringu salvestamine[ import-data] moodul Azure'i masinõppe. See meetod on tõestatud [Hoone mudelid Azure'i masinõppe](#mlmodel) jagu.    
2. Püsivad valimisse ja tehnilise andmed kasutatav mudel hoone uue andmebaasi tabelis, siis saate uue tabeli [Andmete importimine] [ import-data] moodul.

Järgnevalt mõned andmed avastamine, andmete visualiseerimine ja inseneri näited funktsiooni. Rohkem näiteid leiate proovi SQL IPython sülearvuti **Proovi IPython märkmike** kaustas.

#### <a name="initialize-database-credentials"></a>Lähtestamine andmebaasimandaat

Lähtestab teie andmebaasi ühenduse sätted järgmised muutujad:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Luua andmebaasi ühendus

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Aruande arv ridu ja veerge tabeli nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Ridade arv = 173179759  
- Veergude = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Loe-in väike andmed valimisse SQL serveri andmebaasi

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Lugeda näidistabelit on 6.492000 sekundit  
Välja otsitud ridade ja veergude arv = (84952, 21)

#### <a name="descriptive-statistics"></a>Kirjeldav statistika

Nüüd on valmis uurima valimisse kuuluvad andmed. Alustame kirjeldavat statistikat vaadates on **mõeldud\_kaugus** (või muu) välja:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Visualiseerimine: Krundi näide

Edasi vaatleme kasti krundi jaoks teepikkuse visualiseerida ning quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 joonistada][1]

#### <a name="visualization-distribution-plot-example"></a>Visualiseerimine: Jaotus krundi näide

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 joonistada][2]

#### <a name="visualization-bar-and-line-plots"></a>Visualiseerimine: Baar ja rea krundid

Selles näites me bin teepikkuse viis alustele ja visualiseerida binning tulemused.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Saame joonistada eespool aluse levik Baar või rida allpool krundi

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 joonistada][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 joonistada][4]

#### <a name="visualization-scatterplot-example"></a>Visualiseerimine: Scatterplot näide

Näitame hajumine proovitüki vahel **reis\_aega\_in\_nn** ja **reisi\_kaugus** kas on mingisugust korrelatsiooni

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Joonistada #6][6]

Samuti kontrollime suhet **määr\_kood** ja **mõeldud\_kaugus**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 joonistada][8]

### <a name="sub-sampling-the-data-in-sql"></a>Osaproovide võtmine andmeid SQL

Kui valmistab ette andmeid mudel hoone [Azure masin õppe](https://studio.azureml.net)Studio, võite otsustada **kasutada mooduli andmete importimine SQL päring** või püsivad projekteeritud ja valimisse kuuluvate andmete uue tabeli, mida võiks kasutada [Andmete importimine] [ import-data] moodul lihtne * *Valige* saatja < teie\_uue\_tabeli\_nimi >**.

Selle jaotise loome hoida valimisse ja tehnilise andmed uude tabelisse. Näiteks mudeli ehitus otse SQL-päring on esitatud lõigus [andmeuuringu ja funktsiooni SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Luua valimi tabeli ja määrata 1% ühendatud tabelis. Tilk tabeli esimene, kui see on olemas.

Selles jaos me tabeleid liita **nyctaxi\_reis** ja **nyctaxi\_hind**, 1% juhusliku proovi ja püsivad valimisse kuuluvate andmete uue tabeli nimi **nyctaxi\_ühe\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Andmete uurimise abil SQL päringuid IPython sülearvuti

Selles jaos me uurida andmete distributsioonid 1% valimisse andmetest, mis on kestnud lõime üle uue tabeli. Pange tähele, et sarnase uuringu saab teha kasutades originaal tabelid, kasutades soovi korral piirata uurimise proovi või piirates tulemusi teatud aja jooksul kasutades **TABLESAMPLE** ning **komplekteerimine\_kuupäev ja kellaaeg** partitsiooni, nagu kirjeldatud jaotises [andmeuuringu ja inseneri funktsioon SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>Uurimine: Igapäevane jaotamine reisid

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Uurimine: Reis jaotamise kohta medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Funktsioon põlvkonna kasutades SQL päringuid IPython sülearvuti

Käesolevas paragrahvis me luua uusi silte ja funktsioone kasutades SQL päringuid, tegutsevad 1% näidistabelit oleme loonud eelmises osas.

#### <a name="label-generation-generate-class-labels"></a>Sildi genereerimine: Luua klassi silte

Järgmises näites, me luua kahte tüüpi silte kasutada modelleerimine:

1. Binaarne klassi sildid **vms** (ennustavad kui otsa antakse)
2. Multiclass sildid **otsa\_klassi** (ennustavad otsa aluse või vahemik)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Funktsioon inseneri: Count funktsioonid kategooriline veerud

Näiteks muudab kategooriline väli numbrivälja asendades iga kategooria andmed oma sündmuste arv.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Funktsioon inseneri: Bin funktsioone arvulised veerud

Näiteks muudab pidev numbrivälja loodud kategooria vahemikud, st transformatsioon numbriväljale kategooriline väljale.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Funktsioon inseneri: Asukoht funktsioone väljavõte kümnendkoha laiuskraadi

Näites jaguneb kümnendkoha kujutise laius ja/või pool välja mitu piirkond välja erinevad granulaarsus, näiteks, riik, linn, linna, plokk jne. Pange tähele, et uued geo-väljad pole vastendatud tegeliku kohtades. Kaardistamine geocode paikade kohta lisateavet [Bingi kaardid ülejäänud teenused](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Kontrollida featurized tabeli lõplikku

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Oleme nüüd valmis mudeli ehitus ja mudeli juurutamine [Azure'i masinõppe](https://studio.azureml.net). Andmed on valmis mõni ennustus esinevad probleemid, nimelt:

1. Binaarne liigitus: ennustada kas otsa oli sõidu eest maksta.

2. Multiclass liigitus: ennustada otsa makstud varem kindlaksmääratud klasside vahemikus.

3. Regressiooni ülesanne: ennustada summa otsa sõidu eest maksta.  


## <a name="mlmodel"></a>Hoone mudelid Azure'i masinõpet

Modelleerimine kasutamise alustamiseks logige sisse oma Azure'i masinõppe tööruumi. Kui te pole veel loonud õppe tööruumi masin, vt [loomine Azure masin õppe tööruumi](machine-learning-create-workspace.md).

1. Alustamiseks Azure'i masinõppe, Vaata [mis on Azure masin õppe Studio?](machine-learning-what-is-ml-studio.md)

2. Logi [Azure'i masin õppe Studio](https://studio.azureml.net).

3. Studio kodulehekülg pakub hulgaliselt teavet, videod, õpetused, lingid moodulid viide ja muid ressursse. Lisateabe saamiseks Azure'i masinõppe, esile nõu [Azure masina dokumentatsiooni õppekeskus](https://azure.microsoft.com/documentation/services/machine-learning/).

Tüüpilisteks keskmise katse koosneb järgmistest:

1. Luua **+ Uus** eksperiment.
2. Saada andmeid Azure'i masinõppe.
3. Eelnevalt töödelda, muuta ja manipuleerida andmeid vastavalt vajadusele.
4. Luua funktsioone vastavalt vajadusele.
5. Andmed jagatud andmekogumite koolitus/kontrollimine/katsetamine (või eraldi kogumid on iga).
6. Valige mõni masin õppe algoritme sõltuvalt õppe probleemi lahendada. Näiteks, binaarne liigitus, multiclass klassifikatsioon, regressiooni.
7. Ühe või mitme mudeli koolitus kui kasutada rongi.
8. Keskmine hinne valideerimise DataSeti kasutades koolitatud geoloogilises.
9. Hinnata geoloogilises arvutada olulised mõõdikud õppe probleem.
10. Trahvi tune on geoloogilises ja valige parim mudel juurutada.

Põhisõnavara, meil on juba uurida projekteeritud SQL serveri andmete ja valimi suuruse otsustas Azure'i masinõppe neelata. Ehitada mõnega otsustasime prognoosivad mudelid:

1. Saada andmeid [Andmete importimine] Azure'i masinõppe[ import-data] moodul, saadaval jaotises **andmete sisend ja väljund** . Lisateabe saamiseks vt [Andmete importimine] [ import-data] moodul lk.

    ![Azure'i masinõpet andmete importimine][17]

2. Valige **Azure SQL andmebaasi** **andmeallika** **Properties** paneelil.

3. Sisestage **andmebaasi serveri** nimi andmebaasi DNS-i nimi. Formaat:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Sisestage vastavale väljale **andmebaasi nimi** .

5. Sisestage **SQL-i kasutaja nimi** on **Server aqccount kasutajanimi ja parool on **serveri kasutaja konto parool **.

6. Saate **aktsepteerida mis tahes serveri sertifikaadi** võimalus.

7. **Andmebaasi päringu** redigeerimise teksti ala, kleebi päringu, mis pakib vajaliku andmebaasi (sealhulgas sildid nagu arvutatud välju) ja alla näidised on soovitud valimi andmeid.

Alloleval joonisel on näiteks binaarne liigitus katse SQL serveri andmebaasist andmete lugemisel. Sarnaseid eksperimente on võimalik konstrueerida multiclass klassifitseerimise ja regressiooni probleeme.

![Azure'i masin õppe rongi][10]

> [AZURE.IMPORTANT] Modelleerimine andmete kaevandamise ja proovivõtu päringu näited olemas eelmistes osades, **kõik sildid kolm modelleerimine harjutusi on kaasatud päring**. Oluline (nõutav) samm iga modelleerimise harjutused on **välistada** mittevajalikke sildid kaks probleemi, ja mis tahes muu **eesmärgi lekib**. Näiteks kasutades binaarne liigitus, Kasuta silti **kallutada** ja välistada väljad **otsa\_klassi**, **otsa\_summa**, ja **kokku\_summa**. Viimane on eesmärgi lekked, kuna need eeldavad otsa makstud.
>
> Välistada tarbetuid veerge ja/või suunatud lekkeid, võib kasutada [Andmekogumis Vali veerud] [ select-columns] moodul või [Redigeerida metaandmete][edit-metadata]. Vaadake lisateavet jaotisest [Valige veerud andmekogumis] [ select-columns] ja [Redigeerida metaandmed] [ edit-metadata] viitavad lehed.

## <a name="mldeploy"></a>Azure'i masinõpet mudelite juurutamine

Kui mudelis on valmis, saab seda lihtsalt kasutada on veebipõhine teenus otse eksperiment. Lisateavet juurutamise Azure'i masinõppe veebiteenuste [Deploy Azure'i masinõppe veebiteenus](machine-learning-publish-a-machine-learning-web-service.md).

Juurutada uus veebiteenus, peate:

1. Looge punktisüsteem eksperiment.
2. Juurutamise veebiteenus.

Luua hinded eksperiment **lõpetatud** koolitus eksperiment, klõpsake **Luua HINDED KATSETADA** väiksema meetmeid bar.

![Azure'i hinded][18]

Azure'i masinõpet püüab luua hinded eksperiment komponentide koolitus eksperimendi. Eelkõige käsitletakse järgmist:

1. Salvesta koolitatud mudel ja eemaldada mudel koolitusmooduleid.
2. Loogiline **sisendpordiga** esindama eeldatav sisendandmete skeemi ID.
3. Loogiline **väljundport** esindama eeldatav web service väljund skeemi ID.

Kui hinded katse, läbi vaatama ja vastavalt vajadusele reguleerida. Tüüpiline korrigeerimine on asendada sisend andmekogumi ja/või päringu ühe, mis välistab etikett väljad, kui need on kättesaadavad kui teenuse. Samuti on mõistlik vähendada suurus sisendi andmekogumi ja/või päringu paar üksikut, parajalt näitamaks sisendi skeemi. Väljundi jaoks, on tavaline, et välistada kõik väljad ja vaid **Viskas märgiste** ja **Viskas tõenäosuste** [Vali veerud andmekogumis] väljund[ select-columns] moodul.

Alloleval joonisel on hinded katse proov. Kui soovite kasutada, klõpsake nuppu **Avaldamise VEEBITEENUSE** madalama meetmeid bar.

![Azure'i masinõpet avaldada][11]

Sulgege, läbikäiguks õpetamisel on loodud Azure andmed teadus keskkond, töötanud suure avaliku andmekogumi kõik kaugel andmekogumis koolitus ja Azure'i masinõpet veebiteenuse juurutamine.

### <a name="license-information"></a>Litsentsiteave

Proovi läbikäiguks ja sellega kaasneva skripte ja IPython notebook(s) jagavad Microsoft, alla MIT litsents. Palun kontrollige LICENSE.txt faili kataloogi kood github lähemalt.

### <a name="references"></a>Viited

• [Andrés Monroy NYC Taxi reisid Lae leht](http://www.andresmh.com/nyctaxitrips/)  
• [Insertimise ja kilekotti pakkimise NYC's takso reisi andmed poolt Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi-limusiin komisjoni teadusuuringute ja statistika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
