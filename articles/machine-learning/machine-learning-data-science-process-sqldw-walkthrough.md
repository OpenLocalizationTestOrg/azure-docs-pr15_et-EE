<properties
    pageTitle="Meeskonnatöö andmete teadus protsessi toiming: SQL-i andmebaas abil | Microsoft Azure'i"
    description="Täiustatud analüüsi protsess ja toimingu tehnoloogia"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Meeskonnatöö andmete teadus protsessi toiming: abil SQL-andmebaas

Selles õpetuses tutvustame teile koostamise ja juurutamine Õppekeskuse mudel SQL-i andmebaas (SQL-i DW) abil masina kaudu avalikult kättesaadavaks andmekogumi-- [NYC takso reisi](http://www.andresmh.com/nyctaxitrips/) andmekomplekti. Binaarsed mudeli ehitatud prognoosib kas Näpunäide makstakse reisi ja mudelite multiclass liigitamiseks ja regressiooni, käsitletakse ka mis jaotuse tasutud näpunäite summa.

Protseduur järgib töövoo [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Näitame häälestamine andmete teadus keskkonnas, kuidas SQL-i DW andmete laadimine ja kuidas kasutada SQL-i DW või mõne IPython märkmiku insener nende andmete uurimine funktsioonid mudeli. Näitame seejärel kuidas luua ja juurutada Azure seadme õ mudel.


## <a name="dataset"></a>Andmekomplekti NYC takso reisi

Andmete NYC taksosõidu koosneb umbes 20GB tihendatud CSV-failid (tihendamata ~ 48GB), salvestamise rohkem kui 173 miljonit üksikute reisi ja hinnad makstud iga reisi. Iga reisi kirje sisaldab komplekteerimine ja esmase asukohad ja korda, anonüümsete hack (juht) litsentside arvu ja arvu medallion (takso on kordumatu id). Andmete hõlmab kõiki reisi aasta 2013 ja on järgmised kaks andmekogumite ettenähtud iga kuu.

1. **Trip_data.csv** fail sisaldab reisi üksikasjad, näiteks arv reisijate, komplekteerimine ja dropoff punkte, reisi kestus ja reisi pikkus. Siin on mõned valimi kirjed.

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. **Trip_fare.csv** fail sisaldab üksikasju ja iga reisi makse tüüp, hind summa, lisatasu eest ja maksud, näpunäited ja kasutusmaksu, nt makstud makstud kogusumma. Siin on mõned valimi kirjed.

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Kordumatu võti** kasutada liituda reisi\_andmed ja reisi\_hind koosneb kolmest järgmised väljad:

- medaljon,
- Hack\_litsents ja
- komplekteerimine\_kuupäev ja kellaaeg.

## <a name="mltasks"></a>Kolme tüüpi ennustamine tööülesanded

Oleme välja töötada kolme ennustamine probleemide põhjal on *näpunäite\_summa* illustreerimiseks modelleerimine tööülesannete kolme tüüpi:

1. **Kahendarvu liigitamine**: prognoosida, kas Näpunäide maksma reisi, st on *näpunäite\_summa* mis on suurem kui $0, on positiivne näide ajal on *näpunäite\_summa* $ 0 on negatiivne näide.

2. **Multiclass liigitamine**: prognoosida näpunäite väljaostetud reisi vahemik. Jagame selle *näpunäite\_summa* viis aluste või tunnid:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressioonisirge tööülesande**: prognoosida näpunäite väljaostetud reisi summa.  


## <a name="setup"></a>Täiustatud analüüsi jaoks Azure'i andmed teadus keskkonna häälestamine

Azure'i andmed teadus keskkonna seadistamiseks toimige järgmiselt.

**Looge oma Azure'i bloobimälu salvestusruumi konto**

- Kui olete ette oma Azure'i bloobimälu, valige geograafilise asukoha sisse oma Azure'i bloobimälu või et **Lõuna Kesk-USA**, mis on võimalikult lähedalt NYC takso andmete talletuskoht. Andmed kopeeritakse ümbris-ümbrisest avaliku bloobimälu salvestusruumi AzCopy kasutamine oma konto salvestusruumi. Lähemal Azure'i bloobimälu oma Lõuna keskse meile, kiiremini täidetakse selle ülesande (samm 4).
- Oma Azure storage konto loomiseks järgige esitatud [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md). Ärge unustage teha märkmeid järgmised salvestusruumi konto identimisteave väärtused, nagu need on vaja hiljem juhendis.

  - **Salvestusruumikonto nimi**
  - **Salvestusruumi konto võti**
  - **Container nimi** (, kuhu soovite andmed salvestatakse Azure'i bloobimälu)

**Ettevalmistamise oma DW Azure SQL-i eksemplari.**
Järgige dokumentatsioonis ettevalmistamise SQL-i andmebaas eksemplari [loomine SQL-i andmebaas](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) . Veenduge, et teete märgete järgmine SQL-i andmebaas identimisteavet, mida hiljem toimingutes kasutatakse.

  - **Serveri nimi**: <server Name>. database.windows.net
  - **Nime SQLDW (projekt)**
  - **Kasutajanimi**
  - **Parooli**

**Installige Visual Studio 2015 ja SQL Server Data Tools.** Juhised leiate teemast [installida Visual Studio 2015 ja/või SSDT (SQL Server Data Tools) jaoks SQL-i andmebaas](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Ühenduse loomine oma Azure SQL-i DW Visual Studio abil.** Lisateabe saamiseks vaadake juhiseid 1 ja 2 [ühenduse loomine SQL Azure'i andmebaas Visual Studio abil](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Käivitage järgmine SQL-päringu SQL-i andmebaas (päring, mis on esitatud teemas ühendamine sammus 3) asemel loodud andmebaasi loomiseks **lahti**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Luua mõne Azure seadme õ tööruumi jaotises Azure tellimuse.** Juhised leiate teemast [loomine mõne Azure seadme õ tööruumi](machine-learning-create-workspace.md).

## <a name="getdata"></a>Andmete laadimine SQL-andmebaas

Avage Windows PowerShelli käsu konsooli. Käivitage järgmine PowerShelli käskude allalaadimiseks näide SQL skript jagame teiega github parameetriga määratud kohalikku kausta failide *- DestDir*. Saate muuta parameeter *-DestDir* mis tahes kohaliku kataloogiga. Kui *- DestDir* olemas, luuakse see PowerShelli skripti.

>[AZURE.NOTE] Peate **Käivita administraatorina** täitmisel järgmist PowerShelli skripti, kui *DestDir* kataloogi peab loomiseks või kirjutada selle administraatori õigusi.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Pärast eduka täitmise, muudab oma praegust töötavat kausta *- DestDir*. Peaks oskama Kuva nagu allpool:

![][19]

Klõpsake oma *-DestDir*käivitada administraatoriõigustes järgmist PowerShelli skripti:

    ./SQLDW_Data_Import.ps1

Kui PowerShelli skripti töötab esimest korda, palutakse teil sisestada oma Azure SQL-i DW ja Azure'i bloobimälu salvestusruumi konto teave. Kui see PowerShelli skripti on lõpule jõudnud töötab esimest korda, identimisteabe teie sisestatud kuvatakse on kirjutatud konfiguratsioonifail SQLDW.conf Esita töötavat kausta. Tulevaste Käivita PowerShelli skripti faili on võimalus lugeda, selle konfiguratsiooni failist parameetrite korral kõik vajalikud. Kui teil on vaja muuta mõningaid parameetreid, saate valida sisendparameetrid korral viip ekraanil kustutades selle konfiguratsioonifail ja sisestanud parameetrite väärtused, kui seda küsitakse või muuta, redigeerides SQLDW.conf faili kataloogis *- DestDir* parameetrite väärtused.

>[AZURE.NOTE] Need, mis on juba olemas oma Azure SQL-i DW parameetrite SQLDW.conf failist lugemise ajal koos skeemi nimekonfliktide vältimiseks juhusliku arvu 3-kohaline lisatakse skeemi nimi SQLDW.conf faili vaikenime skeemi iga Käivita. PowerShelli skripti võidakse teilt skeemi nimi: nimi võib määratud kasutajale äranägemisel.

Selle **PowerShelli skripti** faili lõpetab järgmisi toiminguid:

- **Laadib alla ja installib AzCopy**, kui AzCopy on juba installitud

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Eksemplaride andmete privaatne bloobimälu salvestusruumi konto** kaudu avaliku bloobimälu AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- Järgmised käsud kontolt privaatne bloobimälu salvestusruumi **laadimise andmeid oma Azure SQL-i DW Polybase (täites LoadDataToSQLDW.sql) abil** .

    - Skeemi loomine

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Andmebaasi, kus otsinguulatuseks on määratud mandaadi loomine

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Mõne salvestusruumi Azure'i bloobimälu välise andmeallika loomine

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Saate luua ka välise failivorming CSV-faili. Andmed on tihendamata ja väljad on Triibu märgiga eraldatud.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Azure'i bloobimälu luua välise hind ja NYC takso andmekomplekti reisi tabelid.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Laadi andmetega välise tabeleid Azure'i bloobimälu SQL-andmebaas

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Andmetabeli näidis (NYCTaxi_Sample) loomine ja selle valimine reisi ja hind tabelid SQL-päringud andmete lisamine. (Mõned juhised juhendis peab kasutama selle näidistabeli.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Kontode salvestusruumi geograafiline asukoht mõjutab laadimisajaga.

>[AZURE.NOTE] Olenevalt teie isiklike bloobimälu salvestusruumi konto geograafiline asukoht, protsessi avaliku bloobimälu andmete kopeerimine privaatne salvestusruumi konto võib võtta umbes 15 minutit või isegi kauem ja andmete laadimine salvestusruumi konto abil oma Azure SQL-i DW võib kuluda 20 minutit või rohkem.  

On teil otsustada, mida, kui teil on dubleeritud lähte-kui ka failide.

>[AZURE.NOTE] Kui CSV failid kopeeritakse avaliku bloobimälu kontole privaatne bloobimälu salvestusruumi teie isiklike bloobimälu salvestusruumi konto juba olemas, AzCopy küsitakse teilt, kas soovite need üle kirjutada. Kui te ei soovi kirjutatakse üle, sisendi küsimise **n** . Kui soovite kirjutada **Kõik** neile, Sisestuskeel **soovitud** küsimise. Samuti saate Sisestuskeel **y** kirjutada CSV-faili ükshaaval.

![Diagrammile #21][21]

Saate oma andmetega. Kui teie andmed on reaal elavamaks rakenduse kohapealne arvuti, saate siiski kasutada AzCopy kohapealne andmete üleslaadimiseks Azure'i bloobimälu oma privaatne. Soovite muuta **andmeallikate** asukoht, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, AzCopy käsk PowerShelli skripti faili kohaliku kausta, mis sisaldab teie andmeid.


>[AZURE.TIP] Kui teie andmed on juba privaatne Azure'i bloobimälu oma reaal elavamaks rakenduse, saate PowerShelli skripti jätkake AzCopy ja andmed otse Azure SQL-i DW üles laadida. Selleks on vaja täiendavaid parandusi skripti kohandada seda oma andmete vorming.


See PowerShelli skripti ka ühendatakse Azure SQL-i DW teave avastamine näide andmefailid SQLDW_Explorations.sql, SQLDW_Explorations.ipynb ja SQLDW_Explorations_Scripts.py nii, et need kolm failid on valmis proovima kohe pärast PowerShelli skripti lõpulejõudmist.

Pärast eduka täitmise, näete ekraanil nagu allpool:

![][20]

## <a name="dbexplore"></a>Andmete uurimine ja funktsioon tehnoloogia nii SQL Azure'i andmebaas

Selles jaotises me teha andmete uurimine ja funktsioon genereerimine töötab SQL-päringud Azure SQL-i DW otse kasutades **Visual Studio Andmeriistad**suhtes. Kõik selles jaotises kasutatud SQL-päringud leiate nimega *SQLDW_Explorations.sql*skripti näide. See fail on juba laaditud teie kohaliku kataloogi PowerShelli skripti. Saate kuulata ka see [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)kaudu. Kuid Github fail pole ühendatud DW Azure SQL-i teave.

Azure SQL-i DW Visual Studio SQL-i DW kasutajanime ja parooli abil ühendust ja avada **SQL-i objekti Exploreri** kinnitamiseks andmebaas ja tabelid importida. Saate tuua *SQLDW_Explorations.sql* faili.

>[AZURE.NOTE] Paralleelselt ladu (PDW) akna päringuredaktor avamiseks kasutada **Uus päring** käsku oma PDW **SQL-i objekti Exploreri**valitud. Standard SQL-i päringuredaktoris ei toeta PDW.

Siin on tüüpi andmete uurimine ja funktsioon genereerimine ülesannete selles jaotises:

- Tutvuge andmete jaotamine erineva aja Windows mõned väljad.
- Uurige andmete kvaliteedi pikkus- ja laiuskraadide välju.
- Binaarsed ja multiclass liigitamine siltide põhjal luua soovitud **näpunäite\_summa**.
- Luua funktsioonid ja Arvuta/Võrdle reisi kaugused.
- Kahe tabeli liitumine ja mudelite koostamiseks kasutatud juhusliku proovi.

### <a name="data-import-verification"></a>Andmete importimine kontrollimine

Need päringud pakkuda kiirülevaate kontrollimine ridade arv ja täidetud varem abil Polybase's paralleelselt hulgi tabeli veerud importida,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Väljund:** Saate 173,179,759 ridade ja veergude 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Avastamine: Reisi jaotus medallion

Selles näites päringu tuvastab Medaljonid (takso arvud) rohkem kui 100 reisi määratud aja jooksul lõpule. Päringu kasu sektsioonitud tabeli juurdepääs, kuna see on see tingitud sektsiooni kava **komplekteerimine\_kuupäeva ja kellaaja**. Päringute täielik andmekomplekti teeb kasutavad sektsioonitud tabeli ja/või skannimine indekseerida.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Väljund:** Päringu peaks tagastama 13,369 Medaljonid (taksod) ja reisi 2013 lõpule arvu määratlemine reaga tabel. Viimane veerg sisaldab arvu reisi lõpule.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Avastamine: Reisi jaotuse medallion ja hack_license

Selles näites tuvastab Medaljonid (takso arvud) ja hack_license arvud (draiverid) lõpetatud rohkem kui 100 reisi määratud aja jooksul.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Väljund:** Päring peaks tagastama täpsustades 13,369 auto/juhi ID-d, mis on lõpetatud selle 100 reisi 2013 13,369 reaga tabel. Viimane veerg sisaldab arvu reisi lõpule.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Andmete kvaliteedi hindamise: vale laiuskraadide ja/või laius kirjete kinnitamine

Selles näites uurib kui laiuskraadide ja/või laiuskraad välju sisaldada sobimatu väärtuse (Radiaan kraadi peaks olema -90 – 90), või olema (0; 0) koordinaatide alusel.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Väljund:** Päring tagastab 837,467 reisi, mis sisaldavad lubamatuid laiuskraadide ja/või laiuskraad väljad.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Avastamine: Otsaga vs ei kaldus reisi jaotuse.

Selles näites otsitakse reisi, mis olid otsaga vs arvu, mis olid otsaga, määratud aja jooksul (või kui kataks terve aasta, kui see on häälestatud siin täielik andmekomplekti) arvu. Selle jaotuse kajastab kasutatakse hiljem kahendarvu liigitamine modelleerimine kahendarvu sildi jaotuse.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Väljund:** Päring peaks tagastama näpunäite esinemissageduse aasta 2013: 90,447,622 otsaga ja 82,264,709 ei ole otstega.

### <a name="exploration-tip-classrange-distribution"></a>Uurimine: Jaotuse näpunäite klassi/vahemik.

Selles näites arvutab jaotuse näpunäite vahemikud (antud aja jooksul või täielik andmekomplekti kui kataks terve aasta). See on sildi tunnid, mida kasutatakse hiljem multiclass liigitamine modelleerimiseks jaotuse.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Väljund:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Avastamine: Arvutada ja võrrelda reisi kauguse

Selles näites teisendab komplekteerimine ja esmase pikkuskraad ja laiuskraad, et SQL-i geograafia punktidest, arvutab teekonna vahemaa abil SQL-i geograafia punktide vahe ja tagastab juhusliku valimi tulemuste võrdlus. Näiteks piirab tulemused kehtivad ainult abil andmete kvaliteedi hindamise päringu varem koordinaate.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Funktsioon matemaatika SQL-i funktsioonide abil

Mõnikord saab SQL-funktsioonides tõhusa suvand funktsiooni matemaatika erifunktsioonid. See kiirtutvustus meil määratletud SQL-i funktsiooni arvutamiseks otsest vahemaa komplekteerimine ja dropoff asukohad. Käivitage järgmine SQL-i skripte **Visual Studio**Andmetööriistad.

Siin on SQL-skripti, mis määratleb funktsiooni kaugus.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Siin on selle funktsiooni SQL-päringu ja funktsioonide loomiseks, näiteks:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Väljund:** Selle päringu genereeritud komplekteerimine ja dropoff laius ja laiuskraadid ning vastavate otsese kaugused miili tabeli (2,803,538 ridadega). Siin on tulemused esimese 3 read.

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Mudeli building andmete ettevalmistamine

Järgmine päring ühendused soovitud **nyctaxi\_reisi** ja **nyctaxi\_hind** tabelid, loob, on kahendarvu liigitamine sildi **otsaga**, mitme klassi liigitamine sildi **näpunäite\_klassi**, ja täielik ühendatud andmekomplekti valimi ekstraktib. Valimi tehakse toomine alamhulk reisi põhjal aeg.  Selle päringu saate kopeeritud siis kleebitud otse [Azure seadme õ Studio](https://studio.azureml.net) [Andmete importimine] [ import-data] mooduli Azure SQL-i andmebaasi eksemplarist otsekoheseks andmete kohta. Päringu välistab kirjed, millel on vale (0; 0) koordinaatide alusel.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Kui olete valmis jätkama Azure seadme õ, võib kas:  

1. Lõplik SQL-päringu eraldamiseks ja kuulake andmed ja kopeerige-kleepige päringu otse [Andmete importimine] salvestamine[ import-data] mooduli Azure seadme õppe, või
2. Valmistatud kuuluvad kui kavatsete kasutada mudeli koostamise SQL-i DW uude tabelisse ja uue tabeli [Andmete importimine] andmed säilivad[ import-data] mooduli Azure seadme õppe. PowerShelli skripti varasemas etapis on see teie eest teinud. Saate lugeda otse moodulis andmete importimine see tabel.


## <a name="ipnb"></a>Andmete uurimine ja funktsioon matemaatika IPython märkmikus

Selles jaotises me täidab andmete uurimine ja funktsioon genereerimine abil nii Python ja varem loodud SQL-päringud SQL-i DW suhtes. Valimi IPython märkmiku nimega **SQLDW_Explorations.ipynb** ja Python skriptifail **SQLDW_Explorations_Scripts.py** on laaditud kohaliku kataloogi. Need on saadaval [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Need kaks failid on identne Python skriptide. Python skriptifail on saadaval juhuks, kui teil on, IPython märkmiku server. Nende kahe valimi Python failid on loodud jaotises **Python 2.7**.

Vajalik Azure SQL-i DW teave oma arvutisse alla laadida IPython märkmiku ja Python skriptifail valimis on antud ühendatud PowerShelli skripti varem. Need on käivitatava muutmata.

Kui olete loonud juba mõne AzureML tööruumi, saate otse AzureML IPython märkmiku teenuse valimi IPython märkmik üles ja käivitage see töötab. Siin on juhiseid AzureML IPython märkmiku teenuse üles laadida.

1. Logige sisse oma AzureML tööruumi, klõpsake ülaosas nuppu "Studio" ja "Märkmike" lehe vasakus servas nuppu.

    ![Diagrammile #22][22]

2. Klõpsake veebilehe vasakus allnurgas nuppu "Uus" ja valige "Python 2". Seejärel sisestage märkmiku nimi ja klõpsake uue tühja IPython märkmiku loomiseks.

    ![Diagrammile #23][23]

3. Klõpsake vasakus ülanurgas uue märkmiku IPython "Jupyter" sümbol.

    ![Diagrammile #24][24]

4. Lohistage ja kukutage valimi IPython märkmiku lehele **puu** AzureML IPython märkmiku teenust ja klõpsake **Laadi üles**. Seejärel laaditakse valimi IPython märkmiku AzureML IPython märkmiku teenusega.

    ![Diagrammile #25][25]

Selleks, et käivitada valimi IPython märkmikku või Python skript faili järgmised Python paketid on vaja. Kui kasutate teenust AzureML IPython märkmik, on need paketid eelinstallitud.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Soovitatav järjestus kui analüütiliste Täpsemad lahendused tuginedes AzureML suure andmed on järgmine:

- Väikese valimi andmeid lugeda mõne-mälu andmeid raami sisse.
- Teha mõned visualiseeringute ja uuringu valimisse andmete kasutamine.
- Katsetage funktsiooni matemaatika valimisse andmete kasutamine.
- Suurem andmete uurimine, andmete käsitlemise ja funktsioon matemaatika abil Python probleemi SQL-päringud otse vastu DW SQL-i.
- Otsustage, sobivad Azure seadme õ mudeli building valimi suurus.

Funktsiooni järgmiste on mõned andmed avastamine, andmete visualiseerimine ja funktsioon tehnika näited. Lisateavet andmete uuringu leiate valimi IPython märkmiku ja Python script näidisfaili.

### <a name="initialize-database-credentials"></a>Andmebaasi identimisteabe lähtestamine

Lähtestab andmebaasi ühendussätete järgmised muutujad:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Andmebaasiga ühenduse loomine

Siin on ühendusstring, mis loob ühenduse andmebaasi.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Aruande arv ridu ja veerge tabeli < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Ridade koguarvust = 173179759  
- Veergude arv = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Aruande arv ridu ja veerge tabeli < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Ridade koguarvust = 173179759  
- Veergude arv = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Lugemis-in small andmete valimi SQL-ladu andmebaasist

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Aega lugeda näidistabeli on 14.096495 sekundit.  
Ridade ja veergude arv tuua = (1000, 21).

### <a name="descriptive-statistics"></a>Kirjeldav statistika

Nüüd olete valmis valimisse andmeid uurima. Alustame vaadates mõned kirjeldava statistika jaoks soovitud **reisi\_kauguse** (või muud väljad, mida soovite määrata).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualiseeringu: Diagrammi näide

Järgmiseks vaatame väljal diagrammi reisi vahemaa visualiseerida soovitud quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Diagrammile #1][1]

### <a name="visualization-distribution-plot-example"></a>Visualiseeringu: Jaotuse diagrammi näide

Visualiseerida jaotuse ja valimisse reisi kaugused histogrammi märkida.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Diagrammile #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualiseeringu: Lint- ja kujundab joon

Selles näites salvenumbreid teekonna vahemaa viis sageduskastidesse ja binning tulemused visualiseerida.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Me diagrammile ülaltoodud prügikasti jaotuse ribal või joone graafiku abil:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Diagrammile #3][3]

ja

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Diagrammile #4][4]

### <a name="visualization-scatterplot-examples"></a>Visualiseeringu: Scatterplot näited

Näitame punkt graafiku vahel **reisi\_aja\_sisse\_nn** ja **reisi\_kauguse** kas on mis tahes korrelatsioonikordaja

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Diagrammile #6][6]

Sarnaselt saame näha seos **määr\_kood** ja **reisi\_kauguse**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Diagrammile #8][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Andmete uurimine valimisse IPython märkmiku kasutamine SQL-päringud andmete põhjal

Selles jaotises uurime valimisse andmetega, mis on säilis lõime üle uue tabeli andmete jaotuse. Pange tähele, et sarnaseid uuringu saab teha kasutades algse tabeli.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Avastamine: Aruande arv ridu ja veerge valimisse tabelis

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Avastamine: Otsaga ei hädaseisatud jaotuse.

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Avastamine: Näpunäite klassi jaotuse.

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Avastamine: Joonistada klassi näpunäite jaotuse.

    tip_class_dist['tip_freq'].plot(kind='bar')

![Diagrammile #26][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Avastamine: Jaotuse reisi iga päev.

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Avastamine: Reisi jaotuse medallion kohta.

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Avastamine: Reisi jaotus medallion ja hack litsentsi

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Avastamine: Reisi aja jaotuse.

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Uurimine: Jaotuse reisi kaugus.

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Uurimine: Jaotuse makse tüüp.

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Veenduge, et lõplikku featurized tabel

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Azure'i masina õppe mudelite koostamine

Meil on nüüd valmis jätkama mudeli building ja mudeli juurutamise [Azure seadme](https://studio.azureml.net)õppe. Andmed on valmis mis tahes ennustamine probleeme tuvastatud varasemas versioonis, milleks kasutada:

1. **Binaarsed liigitamine**: prognoosida, kas Näpunäide maksma reisi.

2. **Multiclass liigitamine**: prognoosida näpunäite makstud vastavalt eelnevalt määratletud tunnid vahemik.

3. **Regressioonisirge tööülesande**: prognoosida näpunäite väljaostetud reisi summa.  



Modelleerimine kasutamise alustamiseks logige sisse oma **Azure seadme õ** tööruumi. Kui te pole veel masina Õppekeskuse tööruumi loonud, lugege teemat [loomine mõne Azure'i ML tööruumi](machine-learning-create-workspace.md).

1. Alustamiseks Azure seadme õ, lugege teemat [mis on Azure seadme õ Studio?](machine-learning-what-is-ml-studio.md)

2. Sisse logida [Azure seadme Õppekeskuse Studio](https://studio.azureml.net).

3. Studio avalehe pakub hulgaliselt teavet, videod, õpetused, lingid moodulid viide ja muud ressursid. Azure'i masina õ kohta lisateabe saamiseks leiate [Azure'i seadme dokumentatsiooni õppekeskus](https://azure.microsoft.com/documentation/services/machine-learning/).

Tüüpilised koolitus katse koosneb järgmist:

1. Looge **+ Uus** katse.
2. Azure'i ML andmeid tuua.
3. Eelnevalt protsessi, muuta ja andmeid töödelda vastavalt vajadusele.
4. Luua funktsioone vastavalt vajadusele.
5. Andmete tükeldamine koolitus/valideerimine/testimine andmekomplektide (või teil on eraldi andmekomplektide iga).
6. Valige üks või mitu arvuti õ algoritmide sõltuvalt õ probleemi lahendada. Nt kahendarvu liigitamine, multiclass liigitamine, regressiooni.
7. Ühe või mitme mudeli kasutamise koolitus andmekomplekti koolitada.
8. Keskmine valideerimine andmekomplekti koolitatud geoloogilises abil.
9. Hinnake geoloogilises arvutada olulised mõõdikud õ probleemi.
10. Viimistlemiseks häälestamine on geoloogilises ja valige parim mudeli juurutamiseks.

Järgmises me on juba uurida insenerirajatis andmed SQL-i andmebaas ja Azure ml neelata otsustanud valimi suurus. Siin on luua ühe või mitme ennustamine mudelite toimingut:

1. Azure'i ML [Andmete importimine] andmeid tuua[ import-data] moodul, mis on saadaval jaotises **andmete sisestamine ja väljundi** . Lisateavet leiate teemast [Andmete importimine] [ import-data] mooduli viide lehe.

    ![Azure'i ML impordi andmed][17]

2. Valige paneelil **Atribuudid** **andmeallika** **Azure'i SQL-andmebaasi** .

3. **Andmebaasiserveri nimi** väljale Sisestage andmebaasi DNS-i nimi. Vorming:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Sisestage **andmebaasi nimi** vastavale väljale.

5. Sisestage *kasutajanimi SQL* **Serveri konto kasutajanimi**ja *parool* väljale **serveri kasutajakonto parooli**.

6. Märkige ruut **Aktsepteeri kõik Serveri sert** .

7. **Andmebaasi päringu** redigeerimise teksti ala, päring, mis ekstraktib vajalikud veebiandmebaasi väljad (sh kõik sildid arvutatud väljadel) ja alla kleebi näidised andmed soovitud valimi suurus.

Alloleval joonisel on näiteks kahendarvu liigitamine katse andmete lugemine andmebaasist SQL-i andmebaas (eemaldage kindlasti asendada tabeli nimed nyctaxi_trip ja nyctaxi_fare skeemi nimi ja tabelinimede, saate kasutada oma kiirtutvustus). Sarnaselt katsete saate ehitatakse multiclass liigitamine ja regressiooni probleemide korral.

![Azure'i ML Train][10]

> [AZURE.IMPORTANT] Andmete modelleerimine olemas eelmiste jaotiste, **kõigi siltide kolme modelleerimine harjutused kaasatakse päringusse**eraldamine ja proovide päringu näited. Oluline (nõutav) samm iga modelleerimine harjutused on **välistamine** mittevajalike siltide kaks probleemi ja mis tahes muud **target lekib**. Näiteks kahendarvu liigitamine kasutamisel kasutada sildi **otsaga** ja välistamine väljad **näpunäite\_klassi**, **näpunäite\_summa**, ja **kokku\_summa**. Viimane on target lekked, kuna need eeldavad näpunäidet makstud.
>
> Mis tahes mittevajalike veergude välistamine ega lekkeid, võite kasutada [Veergude valimine andmekomplekti] [ select-columns] mooduli või [Metaandmete redigeerimine][edit-metadata]. Lisateavet leiate teemast [Andmekomplekti veergude valimine] [ select-columns] ja [Metaandmete redigeerimine] [ edit-metadata] viitavad lehed.

## <a name="mldeploy"></a>Azure'i masina õppe mudelite juurutamine

Kui mudelisse on valmis, saate selle hõlpsalt juurutada veebiteenuse otse katse nimega. Juurutamine Azure'i ML veebiteenuste kohta leiate lisateavet teemast [teenuse Azure seadme õ web Deploy](machine-learning-publish-a-machine-learning-web-service.md).

Uus veebiteenus juurutamiseks peate:

1. Saate luua hinded katse.
2. Veebiteenuse juurutamine.

**Lõpetatud** koolitus katse hinded katse koostamiseks klõpsake **Loomine HINDED katsetamiseks** alumise toiminguribal.

![Azure'i hinded][18]

Azure'i masina õ proovib hinded katse, koolitus katse komponentide loomiseks. Eelkõige siis:

1. Salvestage mudel koolitatud ja mudeli koolitus moodulid eemaldada.
2. Leidke loogiline **Sisestuskeel pordi** tähistada oodatud sisendandmete skeemi.
3. Leidke loogiline **väljundi pordi** tähistada oodatud web teenuse väljundi skeemi.

Kui hinded katse on loodud, üle vaadata ja muuta, muutke vastavalt vajadusele. Asendamiseks Sisestuskeel andmekomplekti ja/või päring, mis ei hõlma reasiltide väljad, nagu need ei ole saadaval, kui teenus on tüüpilised kohandamine. Samuti on hea tava Sisestuskeel andmekomplekti ja/või päringu mahu vähendamiseks paar kirjet, et just nii, et märkida Sisestuskeel skeemi. Väljundi pordi, on levinud jätta kõik väljad ja **Viskas sildid** ja **Viskas tõenäosus** ainult [Veergude valimine andmekomplekti] abil väljund kaasamine[ select-columns] mooduli.

Alloleval joonisel on esitatud valimi katse hinded. Kui olete valmis juurutada, nuppu **AVALDA VEEBITEENUSE** alumise toiminguribal.

![Azure'i ML avaldamine][11]


## <a name="summary"></a>Kokkuvõte
Sulgege, mida me selles õpetuses kiirtutvustus teinud, olete loonud Azure'i andmed teadus keskkonnas, töötanud suurte avaliku andmekomplekti, võttes see meeskonnatöö andmete teadus protsessi, täiesti andmeid tuua mudel koolitus, ja seejärel juurutusega on Azure seadme õ veebiteenuse kaudu.

### <a name="license-information"></a>Litsentsiteavet

See näide kiirtutvustus ja sellega kaasneva skripte ja IPython notebook(s) jagataks Microsoft alla MIT litsents. Kontrollige LICENSE.txt faili kataloogis proovi kood github rohkem üksikasju.

## <a name="references"></a>Viited

• [Andrés Monroy NYC takso reisi allalaadimine leht](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC's takso reisi andmete Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC takso ja limusiin vahendustasu teadus ja statistika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
