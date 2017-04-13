<properties
    pageTitle="Konfigureerimine Oracle'i andmete Guard VMs | Microsoft Azure'i"
    description="Juhis läbi loomise ja rakendamise Oracle'i andmete Guard Azure'i virtuaalmasinates kõrge kättesaadavus ja Avariijärgne taaste jaoks."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Oracle'i andmete Guard Azure konfigureerimine


Selle õpetuse näitab, kuidas häälestada ja rakendada Oracle'i andmete Guard Azure'i Virtuaalmasinates keskkonna kättesaadavuse ja katastroofiabi. Õpetuse keskendub ühesuunalise dispersioonanalüüs piirkondlik Oracle'i andmebaaside jaoks.

Oracle'i andmete Guard toetab andmekaitse ja avariitaastet Oracle'i andmebaasiga. See on lihtne, suure jõudlusega, drop-in lahendus avariitaastet, andmekaitse ja kõrge kättesaadavus kogu Oracle'i andmebaasiga.

Selle õpetuse eeldab, et teil on juba teoreetilised ja praktilised teadmised Oracle'i andmebaasi kättesaadavuse ja avariitaastet kontseptsioonide. Lisateavet leiate teemast [Oracle'i veebisaiti](http://www.oracle.com/technetwork/database/features/availability/index.html) ja ka [Oracle'i andmete Guard põhimõtet ja administreerimiskeskuse juhend](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Lisaks õpetuse eeldab, et teil on juba rakendanud järgmine kohustuslik tarkvara:

- Juba olete [Oracle'i virtuaalse masina pildid – mitmesugused kaalutlused](virtual-machines-windows-classic-oracle-considerations.md) teemas kättesaadavuse ja katastroofi taastamine kaalutlused jaotist läbi vaadata. Azure'i toetab praegu autonoomse Oracle'i andmebaasiga eksemplari, kuid mitte Oracle'i otse rakenduse kogumite (Oracle piirkondlik).


- Olete loonud Azure kasutades sama platvormi esitatud Oracle'i Enterprise'i pilt kaks Virtuaalmasinates (VMs). Veenduge, et selle Virtuaalmasinates on [sama pilveteenuses](virtual-machines-windows-load-balance.md) ja sama virtuaalse võrgu tagamaks, et nad pääsevad üksteisest üle püsivate privaatne IP-aadress. Lisaks on soovitatav VMs paigutamiseks soovitud sama [kättesaadavus](virtual-machines-windows-manage-availability.md) lubada Azure need asetada eraldi viga domeenid ja uuendada domeenid. Oracle'i andmete Guard on saadaval ainult selliste Enterprise'i Oracle'i andmebaasist. Iga seadme peab olema vähemalt 2 GB mälu ja 5 GB vaba kettaruumi. Kõige ajakohasemat teavet esitatud VM suurused platvormi leiate teemast [Azure virtuaalse masina suurused](virtual-machines-windows-sizes.md). Kui vajate täiendavaid kettadraivil oma vms, saate täiendavaid ketast manustada. Lisateavet leiate teemast [Kuidas lisada andmed kettale virtuaalse masina](virtual-machines-windows-classic-attach-disk.md).



- Olete seadnud virtuaalse masina nimed nimega "Machine1" esmane VM ja "Machine2" valige VM Azure klassikaline portaal.

- Olete loonud **ORACLE_HOME** keskkonna muutuja osutamiseks sama Oracle'i juurkausta Installitee primaarne ja puhkerežiimis olev Virtuaalmasinates, näiteks `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Logite oma Windows server **administraatorite** rühma liige või **ORA_DBA** rühma liige.

Selles õppetükis saate küll:

Rakendada füüsilise valige andmebaasi keskkonnas

1. Esmane andmebaasi loomine

2. Valige andmebaasi loomine esmase andmebaasi ettevalmistamine

    1. Jõustatud logimise lubamine

    2. Parooli faili loomine

    3. Valige uuestitegemine Logi konfigureerimine

    4. Luba arhiivimine

    5. Esmane andmebaasi lähtestamine parameetrite seadmine

Valige füüsilise andmebaasi loomine

1. Lähtestamine-parameeter faili valige andmebaasi ettevalmistamine

2. Kuulajale ja tnsnames toetamiseks põhi- ja valige masinad andmebaasi konfigureerimine

    1. Mõlemal serverid korraldada nii andmebaaside kirjete listener.ora konfigureerimine

    2. Hoidke kirjete nii põhi- ja valige andmebaaside jaoks, saate konfigureerida tnsnames.ora Virtuaalmasinates põhi- ja valige. 

    3. Käivitage kuulajale ja kontrollige tnsping nii Virtuaalmasinates nii teenuste kohta.

3. Valige eksemplari nomount olekus alustada

4. Kasutage RMAN klooni andmebaas ja valige andmebaasi loomine

5. Valige füüsilise andmebaasi käivitamine hallatavate taastamise režiimi

6. Veenduge, et füüsilise valige andmebaas

> [AZURE.IMPORTANT] Selles õpetuses on koostatud ja testitakse järgmist riist- ja tarkvara konfiguratsiooni:
>
>|                      | **Esmane andmebaas**                      | **Valige andmebaas**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle väljaanne**   | Oracle11g ettevõtte väljaanne (11.2.0.4.0) | Oracle11g ettevõtte väljaanne (11.2.0.4.0) |
>| **Arvuti nimi**     | Machine1                                  | Machine2                                  |
>| **Operatsioonisüsteem** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle'i SID**       | TEST                                      | TEST\_STBY                                |
>| **Mälu**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Kettaruumi**       | Min 5 GB                                  | Min 5 GB                                  |

Oracle'i andmebaas ja Oracle'i andmete Guard edaspidised viimine, võib olla täiendavaid muudatusi, mida soovite rakendada. Kõige ajakohasemat konkreetse versiooni leiate artiklist [Andmete kaitse](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ja [Oracle'i andmebaasiga](http://www.oracle.com/us/corporate/features/database-12c/index.html) dokumentatsiooni Oracle'i veebisaiti.

##<a name="implement-the-physical-standby-database-environment"></a>Rakendada füüsilise valige andmebaasi keskkonnas
### <a name="1-create-a-primary-database"></a>1. esmane andmebaasi loomine

- Esmane andmebaasi loomine "TEST" esmane Virtual kohapeal. Lisateavet leiate teemast loomine ja konfigureerimine Oracle'i andmebaasiga.
- Andmebaasi nime kuvamiseks andmebaasiga ühenduse SQL roll SYSDBA kasutajana SYS * pluss Käsuviip ja käivitage järgmine tekst:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Järgmiseks päringu dba_data_files süsteemi vaates andmebaasifailide nimed:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2 esmase andmebaasi ettevalmistamine valige andmebaasi loomine

Enne valige andmebaasi loomist on soovitatav esmane andmebaas on õigesti konfigureeritud tagada. Järgmises loendis, mida peate tegema järgmist:

1. Jõustatud logimise lubamine
2. Parooli faili loomine
3. Valige uuestitegemine Logi konfigureerimine
4. Luba arhiivimine
5. Esmane andmebaasi lähtestamine parameetrite seadmine

#### <a name="enable-forced-logging"></a>Jõustatud logimise lubamine

Rakendada puhkerežiimis olev andmebaasi, läheb vaja selleks 'Sunnitud logige' esmane andmebaasi. See suvand tagab isegi juhul, kui 'nologging' toiming on lõpule jõudnud, Jõusta logimine ülimuslik ja kõik toimingud on sisse logitud rakendusse uuestitegemine logid. Seetõttu me veenduge, et kõik esmane andmebaasi logitud ja dispersioonanalüüs selle ootele kaasab kõik toimingud esmane andmebaas. Jõusta logimise lubamiseks käivitage lause alter andmebaasi.

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Parooli faili loomine

Saaks saata ja rakendada arhiivitud logid esmane serverist puhkerežiimis olev server, sys parooli peab olema sama nii põhi- ja valige serverites. Mis on põhjus, miks te esmane andmebaasi parooli faili loomine ja selle kopeerimine puhkerežiimis olev serveriga.

>[AZURE.IMPORTANT] Kui kasutate Oracle'i andmebaasiga 12c, on uus kasutaja, **SYSDG**, mille abil saate hallata Oracle'i andmete Guard. Lisateavet leiate teemast [muudatused rakenduses Oracle'i andmebaasiga 12 c Release](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Lisaks veenduge, et ORACLE\_Avaleht keskkond, mis on juba määratletud Machine1. Kui ei, määrata keskkonna muutuja, mis dialoogiboksi keskkonna muutujate abil. Selle dialoogiboksi avamiseks käivitage **süsteemi** kasuliku topeltklõpsake ikooni süsteem **Juhtpaneel**; seejärel klõpsake vahekaarti **Täpsemalt** ja valige **Keskkonna muutujate**. Keskkonna muutujate seadmiseks klõpsake jaotises **Süsteem muutujate**nuppu **Uus** . Pärast häälestamist keskkonna muutujate, sulgege olemasoleva Windowsi Käsuviip ja avada uus.

Käivitage järgmine lause üle minna Oracle\_Avaleht kaust, näiteks C:\\OracleDatabase\\toode\\11.2.0\\dbhome\_1\\andmebaasi.

    cd %ORACLE_HOME%\database

Looge parooli faili parooli faili loomine kasuliku [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm)abil. Sama Windowsi käsuviipa sisse Machine1, käivitage järgmine käsk **SYS**parool parool väärtuse seadmine:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

See käsk loob parooli faili, PWDTEST.ora on Oracle nimeks\_HOME\\andmebaasi kataloogi. Selle faili peaks kopeerimiseks % ORACLE'I\_Avaleht %\\andmebaasi kataloogi Machine2 käsitsi.

#### <a name="configure-a-standby-redo-log"></a>Valige uuestitegemine Logi konfigureerimine

Seejärel peate valige Logi uuesti konfigureerimiseks nii, et esmane õigesti saada soovitud tee uuesti, kui see on mõne puhkerežiimis olev. Valige uuestitegemine logid automaatselt luua selle puhkerežiimis ka eelnevalt luua neid siin võimaldab. See on oluline konfigureerimine puhkerežiimis olev uuesti logid (SRL) sama suur kui online uuesti logid. Valige tee uuesti praeguse logifailide mahu peab täpselt vastama praeguse esmane andmebaasi online uuestitegemine logifailid suurust.

Käivitage järgmine lause SQL\*pluss Machine1 Käsuviip. V$ logifail on süsteemide vaade, mis sisaldab teavet uuestitegemine logifailid.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Pange tähele, et 52428800 on 50 MB.

Klõpsake SQL\*pluss aknas käivitada järgmistest lisada uue puhkerežiimis olev log faili jaotises Valige andmebaasi uuesti ja määrata arv, mis tuvastab jaotises rühma klausel abil. Rühma arvudega saate teha haldamiseks valige uuestitegemine log faili rühmade lihtsamaks.

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Järgmiseks käivitage järgmised süsteemide vaade loendi teabele uuestitegemine logifailid. See toiming kinnitab ka loodud valige uuestitegemine log faili rühmad:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Luba arhiivimine

Seejärel lubada, käitades järgmistest panna esmase andmebaasi ARCHIVELOG režiimi ja luba automaatne arhiivimine arhiivimine. Saate lubada arhiivi logi režiimi paigaldus andmebaas ja seejärel käsu archivelog.

Esmalt logige sysdba. Käivitage Windows Käsuviip.

    sqlplus /nolog

    connect / as sysdba

Seejärel sulgumist SQL andmebaasis\*pluss Käsuviip:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Sooritage käivitus ühendamine käsu andmebaasi ühendada. See tagab, et Oracle'i seostab määratud andmebaas eksemplari.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Käivitage:

    SQL> alter database archivelog;
    Database altered.

Käivitage muuta andmebaasi lause tavaliseks kasutamiseks kättesaadavaks andmebaasi avamine klausliga:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Esmane andmebaasi lähtestamine parameetrite seadmine

Konfigureerida andmete Guard, peate loomiseks ja konfigureerimiseks valige parameetrid tavaline pfile (initialization parameetri tekstifail) esimese. Kui soovitud pfile on valmis, peate teisendamiseks serveri parameetri faili (SPFILE'I).

Saate määrata selle Käivitamise parameetrid kasutamine andmete Guard keskkonna. ORA fail. Kui pärast selle õpetuse, peate värskendama esmase andmebaasi Käivitamise. ORA, nii et see mahub nii rollid: esmase või puhkerežiimis olev.

    SQL> create pfile from spfile;
    File created.

Järgmiseks peate redigeerimiseks pfile lisamiseks valige parameetrid. Selle tegemiseks avage soovitud INITTEST. ORA faili asukoha % ORACLE'I\_Avaleht %\\andmebaasi. Seejärel lisab järgmistest INITTEST.ora. Teie Käivitamise nimereeglistik. ORA fail on Käivitamise\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Eelmise ajaploki lause sisaldab kolme olulist häälestamise üksused.
-   **LOG_ARCHIVE_CONFIG...:** Määratlege selle lause abil kordumatu andmebaasi ID-d.
-   **LOG_ARCHIVE_DEST_1...:** Määratlege selle lause abil kohaliku arhiivi kaust. Soovitame teil looge uus kaust oma andmebaasi arhiveerimise vajadustele ja selle lause konkreetselt kasutamise asemel abil Oracle'i vaikimisi kaust % ORACLE_HOME%\database\archive kohaliku arhiivi asukoha määramine.
-   **LOG_ARCHIVE_DEST_2... LGWR ASÜNKROONSE...:** määratlete asünkroonne log kirjutaja protsess (LGWR) tehingu uuestitegemine andmete kogumise ja edastab selle valige sihtkohta. Siin on DB_UNIQUE_NAME määrab sihtkoha valige serveri andmebaasi kordumatu nimi.

Kui parameeter uue faili on valmis, peate selle SPFile'i koostada.

Esmalt väljalülitamine andmebaasi:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Järgmisena käivitage nomount käivituskäsule järgmiselt:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Nüüd looge SPTemplateFileType:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Seejärel väljalülitamine andmebaasi:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Seejärel kasutage käivituskäsule käivitamiseks, näiteks:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Valige füüsilise andmebaasi loomine
Selles jaotises käsitletakse toiminguid, mis tuleb sooritada Machine2 ettevalmistamiseks füüsilise valige andmebaas.

Esmalt peate kaugtöölaua Machine2 Azure klassikaline portaali kaudu.

Seejärel valige server (Machine2), luua kõik vajalikud kaustad, valige andmebaasi, nt C:\\\<YourLocalFolder\>\\TEST. Selle õpetuse järgides veenduge, et et kausta ülesehitust vastab Machine1 säilitada kõik vajalikud failid, nt controlfile, andmefaile, redologfiles, udump, bdump ja cdump failide kausta struktuuri. Lisaks määratlemine ORACLE\_– kodu ja ORACLE'I\_BASE keskkonna muutujate Machine2. Kui ei, määratleda need keskkonna muutuja, mis dialoogiboksi keskkonna muutujate abil. Selle dialoogiboksi avamiseks käivitage **süsteemi** kasuliku topeltklõpsake ikooni süsteem **Juhtpaneel**; seejärel klõpsake vahekaarti **Täpsemalt** ja valige **Keskkonna muutujate**. Keskkonna muutujate seadmiseks klõpsake jaotises **Süsteem muutujate**nuppu **Uus** . Pärast häälestamist keskkonna muutujate, peate olemasoleva Windowsi Käsuviip sulgemiseks ja muudatuste nägemiseks uus avada.

Seejärel tehke järgmist.

1. Lähtestamine-parameeter faili valige andmebaasi ettevalmistamine

2. Kuulajale ja tnsnames toetamiseks põhi- ja valige masinad andmebaasi konfigureerimine

    1. Mõlemal serverid korraldada nii andmebaaside kirjete listener.ora konfigureerimine

    2. Konfigureerige tnsnames.ora Virtuaalmasinates põhi- ja valige pikalt kirjeid nii põhi- ja valige andmebaasid

    3. Käivitage kuulajale ja kontrollige tnsping nii Virtuaalmasinates nii teenuste kohta.

3. Valige eksemplari nomount olekus alustada

4. Kasutage RMAN klooni andmebaas ja valige andmebaasi loomine

5. Valige füüsilise andmebaasi käivitamine hallatavate taastamise režiimi

6. Veenduge, et füüsilise valige andmebaas

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. mõne parameetri käivitusfaili valige andmebaasi ettevalmistamine

Selles jaotises demonstreerib lähtestamine-parameeter faili valige andmebaasi ettevalmistamine. Selleks esmalt kopeerige soovitud INITTEST. ORA fail arvutisse 1 Machine2 käsitsi. Mida tuleks vaadata selle INITTEST. ORA faili % ORACLE'I\_Avaleht %\\mõlemad masinad andmebaasi kausta. Muutke INITTEST.ora fail Machine2 võrku häälestama valige roll järgmiselt:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Eelmise ajaploki lause sisaldab kahte olulist häälestamise üksused.

-   **\*. LOG_ARCHIVE_DEST_1:** peate Machine2 c:\OracleDatabase\TEST_STBY\archives kausta käsitsi luua.
-   **\*. LOG_ARCHIVE_DEST_2:** see on valikuline juhis. Saate määrata selle, nagu võib olla vaja esmane seade on hooldus ja valige seade muutub esmane andmebaasi.

Seejärel peate alustamiseks valige eksemplari. Valige andmebaasiserver, sisestage Oracle'i eksemplari loomine Windowsi teenuse loomisega Windows käsuviibale järgmine käsk:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

Käsk **Oradim** loob Oracle'i eksemplari, kuid ei käivitu. Te ei leia seda C:\\OracleDatabase\\toode\\11.2.0\\dbhome\_1\\kausta Prügikast.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Kuulajale ja tnsnames toetamiseks põhi- ja valige masinad andmebaasi konfigureerimine
Enne valige andmebaasi loomist peate veenduge, et põhi- ja valige oma konfiguratsioon andmebaasid saate üksteisega rääkida. Selle tegemiseks peate konfigureerida nii kuulajale ja TNSNames käsitsi või network configuration kasuliku NETCA abil. See on kohustuslikud tööülesande taastamine halduri kasuliku (RMAN) kasutamisel.

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Mõlemal serverid korraldada nii andmebaaside kirjete listener.ora konfigureerimine

Kaugtöölaua Machine1 ja listener.ora fail nimega määratud all Redigeeri. Listener.ora faili redigeerimisel alati veenduge, et avamine ja paremsulg joondada samas veerus. Leiate listener.ora faili järgmises kaustas: c:\\OracleDatabase\\toode\\11.2.0\\dbhome\_1\\võrgu\\administraator\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Järgmise, Remote'i töölaua Machine2 ja Redigeeri selle listener.ora faili järgmiselt: # listener.ora võrgu konfigureerimise faili: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Konfigureerige tnsnames.ora Virtuaalmasinates põhi- ja valige pikalt kirjeid nii põhi- ja valige andmebaasid

Kaugtöölaua Machine1 ja tnsnames.ora fail nimega määratud all Redigeeri. Leiate tnsnames.ora faili järgmises kaustas: c:\\OracleDatabase\\toode\\11.2.0\\dbhome\_1\\võrgu\\administraator\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Kaugtöölaua Machine2 ja Redigeeri selle tnsnames.ora faili järgmiselt:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Käivitage kuulajale ja kontrollige tnsping nii Virtuaalmasinates nii teenuste kohta.

Avada nii põhi- ja valige virtuaalmasinates uus Windowsi Käsuviip ja käivitage järgmised avaldised.

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Valige eksemplari nomount olekus alustada
Kui soovite häälestada keskkonna toetamiseks valige andmebaasi valige Virtual arvutis (MACHINE2).

Esmalt parooli faili kopeerimine esmane arvutist (Machine1) (Machine2) valige seadme käsitsi. See on vajalik, nagu **sys** parooli peab olema sama nii arvutites.

Seejärel avage käsuviiba Windowsi Machine2 ja häälestada keskkonna muutujate osutamiseks puhkerežiimis olev andmebaas järgmiselt:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Järgmiseks nomount olekus puhkerežiimis olev andmebaasi käivitamine ja seejärel looge SPTemplateFileType.

Andmebaasi käivitamiseks tehke järgmist.

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Kasutage RMAN klooni andmebaas ja valige andmebaasi loomine
Mis tahes füüsilise valige andmebaasi loomiseks esmase andmebaasi varukoopia tegemiseks saate taastamine halduri kasuliku (RMAN).

Valige virtuaalse masina (MACHINE2) ja Käivita RMAN kasuliku, määrates täieliku ühenduse kaugtöölaua tekstistringi nii sihtrühmade (esmane andmebaas, Machine1) ja AUXILLARY (valige andmebaas, Machine2) eksemplari.

>[AZURE.IMPORTANT] Kasutage operatsioonisüsteemi autentimine, nagu ei ole andmebaasi valige serveri kohapeal veel.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Valige füüsilise andmebaasi käivitamine hallatavate taastamise režiimi
Selle õpetuse näitab, kuidas luua füüsilise valige andmebaas. Loogilise valige andmebaasi loomise kohta leiate teavet teemast Oracle'i dokumentatsiooni.

Avage SQL-i\*pluss Käsuviip ja lubada andmete Guard valige virtuaalse masina või serverisse (MACHINE2) järgmiselt:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Valige andmebaasi avamisel režiimis **MOUNT** arhiivi Logi saatmine jätkub ja hallatavate taastamise protsess jätkub log rakendamist valige andmebaasi. See tagab, et valige andmebaasi jääb esmane andmebaasi ajakohane. Pange tähele, et valige andmebaasi ei saa puuetega inimestele juurdepääsetavate aruandluseks selle aja jooksul.

Valige andmebaasi avamisel **Lugeda ainult** režiimis arhiivi Logi saatmine ei lahene. Kuid hallatavate taastamise protsess katkeb. See põhjustab valige andmebaasi saada järjest aegunud enne jätkamist hallatavate taastamise protsess. Pääsete valige andmebaasi aruandluse jaoks sel ajal, kuid andmed vasta viimaste muudatuste kohta.

Soovitame üldiselt valige andmebaasi jätma **MOUNT** režiimi andmed valige andmebaasi ajakohasena hoidmiseks kui esmase andmebaasi tõrke. Siiski saate hoida valige andmebaasi **Lugeda ainult** režiimis aruandluse eesmärgil sõltuvalt teie taotlus nõuetele. Järgmised sammud kirjeldavad lubamine kirjutuskaitstud režiimis SQL-i abil andmete valvur\*lisaks:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Veenduge, et füüsilise valige andmebaas
Selles jaotises näitab, kuidas kontrollida kõrge-saadavus konfiguratsiooni administraatorina.

Avage SQL-i\*Plus käsuviiba aken ja märkige arhiivitakse uuesti Logi sisse puhkerežiimis olev virtuaalse masina (Machine2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Avage SQL * Plus käsuviiba aken ja vahetamise logfiles esmane arvutis (Machine1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Kontrollige arhiivitud uuestitegemine Logi sisse puhkerežiimis olev virtuaalse masina (Machine2).

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Kontrollige iga vahe sisse puhkerežiimis olev virtuaalse masina (Machine2).

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Mõne muu kinnitusmeetod võib olla Tõrkesiirde valige andmebaas ja testige kui see on võimalik, et failback esmane andmebaasi. Valige esmane andmebaasi andmebaasi aktiveerimiseks kasutama järgmistest:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Kui teil on lubatud Flash esmane algse andmebaasi, on soovitatav, et algne esmane andmebaasist ja valige andmebaasi uuesti luua.

Soovitame Flash andmebaasi esmase ja valige andmebaasid lubamine. Kui Tõrkesiirde juhtub, saate esmase andmebaasi vilksatas tagasi selle Tõrkesiirde aega ja kiiresti teisendatakse valige andmebaasi.

##<a name="additional-resources"></a>Lisaressursid
[Oracle'i virtuaalse masina pilte Azure](virtual-machines-windows-classic-oracle-images.md)
