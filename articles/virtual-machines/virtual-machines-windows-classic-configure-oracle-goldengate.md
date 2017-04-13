<properties
    pageTitle="Konfigureerimine Oracle'i GoldenGate VMs | Microsoft Azure'i"
    description="Juhis läbi loomise ja rakendamise Oracle'i GoldenGate Azure Windows Server VMs kõrge kättesaadavus ja Avariijärgne taaste jaoks."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Oracle'i GoldenGate Azure konfigureerimine


Selle õpetuse demonstreerib häälestamise Oracle'i GoldenGate kõrge-saadavus ja avariitaastet Azure'i Virtuaalmasinates keskkonnas. Õpetuse keskendub piirkondlik Oracle'i andmebaaside [kahesuunalise kopeerimise](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) ja nõuab, et mõlemad saidid on aktiivne.

Oracle'i GoldenGate toetab andmete jaotust ja andmete integreerimine. See võimaldab teil andmeid jaotuse ja andmete sünkroonimine lahendus kaudu Oracle'i-Oracle'i dispersioonanalüüs konfiguratsiooni häälestamine ja annab juurdepääsu paindlikele kõrge-saadavus lahenduse. Oracle'i GoldenGate täiendab Oracle'i andmete Guard selliste dispersioonanalüüs luba kogu ettevõtte teave jaotust ja null-tööseisakute täienduste ja migratsioon. Üksikasjalikku teavet leiate teemast [Abil Oracle'i GoldenGate Oracle'i andmete valvur](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle'i GoldenGate sisaldab järgmiste: eraldada, andmete pump Replicat, rajad või faile, postkastid, juhataja ja koguja ekstraktida. Kahesuunalise dispersioonanalüüs on kaks saitide vahel, peate loomine ja käivitamine kõik komponendid mõlemad saidid. Oracle'i GoldenGate arhitektuur kohta leiate üksikasjalikumat teavet teemast [Oracle GoldenGate juhend](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Selle õpetuse eeldab, et teil on juba teoreetilised ja praktilised teadmised Oracle'i andmebaasi kättesaadavuse ja avariitaastet põhimõtet kui ka [Oracle'i GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Lisateavet leiate teemast [Oracle'i veebisaidi](http://www.oracle.com/technetwork/database/features/availability/index.html).

Lisaks õpetuse eeldab, et teil on juba rakendanud järgmine kohustuslik tarkvara:

- Juba olete [Oracle'i virtuaalse masina pildid – mitmesugused kaalutluste](virtual-machines-windows-classic-oracle-considerations.md) teemas kättesaadavuse ja katastroofi taastamine kaalutluste jaotist läbi vaadata. Pange tähele, et Azure'i toetab praegu autonoomse Oracle'i andmebaasiga eksemplari, kuid mitte Oracle'i otse rakenduse kogumite (Oracle piirkondlik).

- [Oracle'i allalaadimist](http://www.oracle.com/us/downloads/index.html) veebisaidilt allalaaditud tarkvara Oracle'i GoldenGate. Valitud toote Pack Oracle'i Fusion vahevara – andmete integreerimine. Seejärel Oracle'i GoldenGate Oracle'i v11.2.1 Media Pack klõpsake Microsoft Windowsi x64 (64-bitine) jaoks valitud 11 g Oracle'i. Järgmiseks allalaadimine Oracle'i GoldenGate V11.2.1.0.3 Oracle 11g 64-bitise Windowsi 2008 (64-bitine).

- Olete loonud Oracle'i Enterprise'i kasutamine Windows Server Azure'i kaks Virtuaalmasinates (VMs). Veenduge, et et selle Virtuaalmasinates on [sama pilveteenuses](virtual-machines-linux-load-balance.md) ja samasse [Virtuaalse võrgu](https://azure.microsoft.com/documentation/services/virtual-network/) tagamaks, et nad pääsevad üksteisest üle püsivate privaatne IP-aadress.

- Azure'i klassikaline portaalis olete Määrake saidi a "MachineGG1" ja "MachineGG2" saidi b virtuaalse masina nimed.

- Teie loodud test andmebaaside "TestGG1" klõpsake saidi A ja "TestGG2" klõpsake saidi B.

- Logite oma Windows server administraatorite rühma liige või **ORA_DBA** rühma liige.

Selles õppetükis saate küll:

1. Andmebaasi saidi A ja B saidi häälestamine  

    1. Tehke algse andmete laadimine

2. Andmebaasi tiražeerimine saidi A ja B saidi ettevalmistamine

3. Luua kõik vajalikud objektid, mis toetavad DDL Dispersioonanalüüs

4. Saidi A ja B saidi GoldenGate halduri konfigureerimine

5. Saidi A ja B saidi ekstraktida rühma loomine ja andmete Pump protsessid

    1. Ekstrakti ja andmete Pump protsesside saidi loomine

    2. Saidi b GoldenGate kontrollpunkt tabeli loomine

    3. Lisage REPLICAT saidil B

    4. Saidi b ekstrakti ja andmete Pump protsesside loomine

    5. Klõpsake saidi GoldenGate kontrollpunkt tabeli loomine

    6. Lisage REPLICAT saidil A

    7. Klõpsake saidi A ja B saidi trandata lisamine

    8. Saidi ekstrakti ja andmete Pump protsesside käivitamine

    9. Saidi B ekstrakti ja andmete Pump protsesside käivitamine

    10. Klõpsake saidi REPLICAT alustamiseks

    11. Saidi b REPLICAT alustamiseks

6. Veenduge, et protsessi kahesuunalise dispersioonanalüüs

>[AZURE.IMPORTANT] Selles õpetuses on häälestamise ja järgmine tarkvara konfiguratsioon testitakse:
>
>|                        | **Saidi andmebaasi**              | **Saidi B andmebaas**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle väljaanne**     | Oracle11g väljaanne 2 – (11.2.0.1) | Oracle11g väljaanne 2 – (11.2.0.1) |
>| **Arvuti nimi**       | MachineGG1                       | MachineGG2                       |
>| **Operatsioonisüsteem**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle'i SID**         | TESTGG1                          | TESTGG2                          |
>| **Skeemi dispersioonanalüüs** | SCOTTI                            | SCOTTI                            |

Oracle'i andmebaas ja Oracle'i GoldenGate edaspidised viimine, võib olla täiendavaid muudatusi, mida soovite rakendada. Kõige ajakohasemat versiooni konkreetset teavet dokumentatsioonist [Oracle'i GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) ja [Oracle'i andmebaasiga](http://www.oracle.com/us/corporate/features/database-12c/index.html) Oracle'i veebisaidil. Näiteks, väljaanne 11.2.0.4 allika andmebaasi ja hiljem sellise DDL on asünkroonselt logmining server ja nõuab pole teisiti päästikute, tabelite või muude andmebaasiobjektidega olema installitud. Oracle'i GoldenGate uuendamine saab teha ilma peatamine kasutajate rakendusi. A DDL päästik ja toetavad objektid on nõutav, kui ekstrakti on integreeritud režiimis Oracle'i 11g allika andmebaasiga, mis on varasem versioon 11.2.0.4. Üksikasjalikud juhised leiate teemast [installimine ja konfigureerimine Oracle'i GoldenGate Oracle'i andmebaasiga](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. andmebaasi saidi A ja B saidi häälestamine
Selles jaotises selgitatakse, kuidas teha andmebaasi eeltingimused, saidi-ja saidi B. Kõik selle jaotise juhiseid tuleb sooritada mõlemad saidid: saidi ja saidi B.

Esimese, remote töölaua saidi A ja B saidi Azure klassikaline portaali kaudu. Avada Windowsi Käsuviip ja home kataloogi Oracle'i GoldenGate häälestamise failide loomine.

    mkdir C:\OracleGG

Pakkige see lahti, seejärel installige Oracle'i GoldenGate tarkvara selles kaustas. Pärast seda toimingut saate algatada GoldenGate tarkvara käsk Tõlgi (GGSCI), käitades järgmine käsk:

    C:\OracleGG\.\ggsci

Saate kasutada [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) mitu käsud, mis konfigureerimine käivitamiseks, kontrolli ja kuvari Oracle'i GoldenGate.

Järgmine, käivitage järgmine käsk installipaketi alamkaustade loomiseks:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Väljumiseks GGSCI käsuviipa järgmine käsk:

    GGSCI (Hostname) 1> EXIT

Seejärel, peate looma andmebaasi kasutaja, mida kasutatakse Oracle'i GoldenGate juhataja, ekstrakti ja kopeerimise abil. Pange tähele, et saate luua üksikkasutajate iga protsessi või konfigureerimine levinud ainult üks kasutaja. Selles õpetuses loome ühe kasutaja, mis on kutsutud ggate. Seejärel anname selle kasutaja vajalikke õigusi. Pange tähele, et teil tuleb teha järgmisi toiminguid saidi ja saidi B.

Avage SQL-i\*pluss käsuviiba aken saidi A ja B saidi andmebaasi administraatori õigustega abil **SYSDBA**, näiteks:

    Enter username: / as sysdba

Ja käivitada.

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Järgmisena otsige üles soovitud Käivitamise\<DatabaseSID\>. ORA faili % ORACLE'I\_Avaleht %\\andmebaasi kausta saidi A ja B saidi ja lisab andmebaasi järgmisi INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Oracle'i GoldenGate GGSCI kõik käsud täieliku loendi leiate teemast [viide Oracle'i GoldenGate for Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Tehke algse andmete laadimine

Andmebaasi saab teha algse andmete laadimine, järgides mitu võimalust. Näiteks saate [Oracle'i GoldenGate otsese laadi](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) või tavalise impordi ja ekspordi Utiliidid saidi tabeli andmete eksportimine saidi B.

Oracle'i GoldenGate dispersioonanalüüs protsessi näitamaks selles õpetuses näitab tabeli saidi A ja B saidi loomise, kasutades järgmisi käske.

Kõigepealt avada SQL-i\*Plus akna ja käivitage järgmine käsk saidi A ja B saidi andmebaase varude tabeli loomiseks:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Järgmiseks lisage piirangu saidi A ja B saidi andmebaaside vastloodud tabel.

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Seejärel andke kõik õigused klõpsake uue kasutaja ggate saidi ja saidi B: laoseisu tabeli

    grant all on scott.inventory to ggate;

Järgmiseks loomine ja lubamine andmebaasi käivitamiseks, INVENTORY_CDR_TRG, veenduge, et kõik tehingud uude tabelisse on salvestatud, kui kasutajal pole ggate vastloodud tabeli. Selle toimingu saidi ja saidi B.

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2 saidi A ja B saidi ettevalmistamine andmebaasi tiražeerimine.
Selles jaotises selgitatakse, kuidas andmebaasi tiražeerimine saidi A ja B saidi ettevalmistamine. Kõik selle jaotise juhiseid tuleb sooritada mõlemad saidid: saidi ja saidi B.

Esimese, remote töölaua saidi A ja B saidi Azure klassikaline portaali kaudu. Lülitumine andmebaasi archivelog abil SQL * Plus käsuviiba aken:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Seejärel lubada minimaalsete [täiendavatele logimine](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) .

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Järgmiseks ettevalmistamiseks toetamiseks DDL (andmekirjelduskeel) kopeerimine andmebaasi:

    SQL> alter system set recyclebin=off scope=spfile;

Seejärel, sulgemine ja uuesti andmebaas.

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. luua kõik vajalikud objektid, mis toetavad DDL Dispersioonanalüüs
Selles jaotises loetletud skriptide, mida vajate luua kõik vajalikud objektid toetamiseks DDL kopeerimise abil. Peate käivitama skriptide määratud selles jaotises saidi-ja saidi B.

Avage käsuviip Windows ja liikuge Oracle'i GoldenGate kausta, nt C:\\OracleGG. Käivitage SQL-i\*pluss Käsuviip andmebaasi administraatori õigustega, näiteks saidi A ja B. saidi **SYSDBA** kasutamine

Käivitage järgmisi:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Oracle'i GoldenGate tööriista kasutamiseks DDL (andmekirjelduskeel) tugiteenuste tabeli taseme sisselogimist. Mis on luba täiendavatele logimine tabeli tasemel käsuga lisa TRANDATA miks. Andmebaasi sisselogimiseks Oracle'i GoldenGate käsk Tõlgi aknas avada, ja seejärel käivitage käsk Lisa TRANDATA:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. konfigureerimine GoldenGate halduri saidi A ja B saidi
Oracle'i GoldenGate halduri teostab mitmeid funktsioone, nagu teised GoldenGate protsessid, rada log failide haldamiseks ja aruandlus alustades.

Peate Konfigureerige Oracle'i GoldenGate halduri protsessi nii saidi saidi B. Selleks tehke järgmist saidi ja saidi B.

Avage Windows akna ja algatada Oracle'i GoldenGate käsk Tõlgi.

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Logib GGSCI seansi andmebaasi nii, et täidate käsud, mis mõjutavad andmebaasi.

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Oleku kuvamine ja viivitusega kõik juhataja, ekstrakti ja Replicat protsesside süsteemi (vajaduse korral).

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Saate kuvada oleku ja viivitusega kõik juhataja, ekstrakti ja Replicat protsesside süsteemi (vajaduse korral).

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Logib GGSCI seansi andmebaasi nii, et täidate käsud, mis mõjutavad andmebaasi.

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Halduri alustamiseks:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. ekstrakti loomine ja andmete Pump protsesside saidi A ja B saidi

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Ekstrakti ja andmete Pump protsesside saidi loomine

Peate looma ekstrakti ja andmete Pump protsesside saidi ja saidi B. kaugtöölaua saidi A ja B saidi Azure klassikaline portaali kaudu. Avatud akna GGSCI käsk Tõlgi. Klõpsake saidi v: käivitage järgmised käsud

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave: GGSCI (MachineGG1) 18 > Redigeeri http Authentication liidesesse ext1 ekstrakti liidesesse ext1 kasutajanimi ggate, parooli ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate tabeli scott.inventory, GETBEFORECOLS (sees UPDATE KEYINCLUDING (prod_category, qty_in_stock, last_dml), ON kustutamine KEYINCLUDING (prod_category, qty_in_stock, last_dml));

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Saidi b GoldenGate kontrollpunkt tabeli loomine

Järgmiseks peate kontrollpunkt tabeli lisamiseks klõpsake saidi B. Selle tegemiseks peate GoldenGate käsk Tõlgi akna avada ja käivitada:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Ja seejärel lisage kontrollpunkt tabeli andmebaasi, kus on ggate omanik.

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Lisada GLOBAALSED faili target serveris, mis on saidi B selles etapis tuleb sisse punkti tabeli nimi. Saidi B: GLOBAALSED faili redigeerimine

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Seejärel lisab CHECKPOINTTABLE parameetri olemasoleva GLOBAALSED:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Viimase sammuna salvestage ja sulgege fail GLOBAALSED parameeter.


###<a name="add-replicat-on-site-b"></a>Lisage REPLICAT saidil B
Selles jaotises kirjeldatakse, kuidas lisada REPLICAT protsessi "REP2" saidi B.

LISA REPLICAT käsu abil saate luua Replicat rühma saidi B:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Saidi b ekstrakti ja andmete Pump protsesside loomine

Selles jaotises kirjeldatakse, kuidas luua saidi B: ekstrakti protsessi "EXT2" ja uute andmete pump protsessi "DPUMP2"

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Klõpsake saidi GoldenGate kontrollpunkt tabeli loomine

Avage aken Oracle'i GoldenGate käsk Tõlgi ja kontrollpunkt tabeli loomine.

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Samuti peate lisama sisse punkti tabeli nimi GLOBAALSED faili saidil A.

Oracle'i GoldenGate käsk Tõlgi aken avada ja redigeerida saidi v: GLOBAALSED faili

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Salvestage ja sulgege fail GLOBAALSED parameeter.

###<a name="add-replicat-on-site-a"></a>Lisage REPLICAT saidil A

Selles jaotises kirjeldatakse, kuidas lisada REPLICAT protsessi "REP1" saidil A.

Järgmine käsk loob Replicat rühma rep1 rada ja seotud checkpointtable nime.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Käsk Redigeeri http AUTHENTICATION parameetri faili avamiseks ja seejärel lisada järgmine teave:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Klõpsake saidi A ja B saidi trandata lisamine

Luba täiendavatele logimine tasemel tabeli lisamine TRANDATA käsu abil. Andmebaasi sisselogimiseks Oracle'i GoldenGate käsk Tõlgi aknas avada, ja seejärel käivitage käsk Lisa TRANDATA.

Kaugtöölaua MachineGG1, et avada Oracle'i GoldenGate käsk Tõlgi ja käivitada.

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Kaugtöölaua MachineGG2, et avada Oracle'i GoldenGate käsk Tõlgi ja käivitada.

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Kuva tabel taseme täiendavatele olek teavet logimine:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Klõpsake saidi A ja B saidi trandata lisamine

Luba täiendavatele logimine tasemel tabeli lisamine TRANDATA käsu abil. Andmebaasi sisselogimiseks Oracle'i GoldenGate käsk Tõlgi aknas avada, ja seejärel käivitage käsk Lisa TRANDATA.

Kaugtöölaua MachineGG1, et avada Oracle'i GoldenGate käsk Tõlgi ja käivitada.

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Kaugtöölaua MachineGG2, et avada Oracle'i GoldenGate käsk Tõlgi ja käivitada.

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Saidi ekstrakti ja andmete Pump protsesside käivitamine

Saidi v: ekstrakti protsessi liidesesse ext1 käivitamine

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Saidi v: andmete pump protsessi dpump1 käivitamine

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Kuvada teavet ekstrakti rühma liidesesse ext1: GGSCI (MachineGG1) 32 > teave ekstraktida liidesesse ext1 ekstrakti liidesesse EXT1 viimase alustamine 2013-11-25 08:03 olek töötab kontrollpunkt Lag 00:00:00 (värskendatud 00:00:02 tagasi) Log lugege kontrollpunkt Oracle'i uuesti logid 2013-11-25 08:03:18 Seqno 6, RBA 3230720 SCN 0.1074371 (1074371)

Oleku kuvamine ja viivitusega kõik juhataja, ekstrakti ja Replicat protsesside süsteemi (vajaduse korral).

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Saidi B ekstrakti ja andmete Pump protsesside käivitamine

Saidi B: ekstrakti protsessi ext2 käivitamine

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Saidi B: andmete pump protsessi dpump2 käivitamine

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Saate kuvada oleku ja viivitusega kõik juhataja, ekstrakti ja Replicat protsesside süsteemi (vajaduse korral).

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Klõpsake saidi REPLICAT alustamiseks

Selles jaotises kirjeldatakse, kuidas alustada REPLICAT "REP1" saidil A.

Saidi v: Replicat protsessi käivitamine

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Rühma Replicat oleku kuvamiseks tehke järgmist.

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Saidi b REPLICAT alustamiseks

Selles jaotises kirjeldatakse, kuidas alustada REPLICAT protsessi "REP2" saidi B.

Saidi B: Replicat protsessi käivitamine

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Rühma Replicat oleku kuvamiseks tehke järgmist.

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. kontrollida kahesuunalise dispersioonanalüüs

Oracle'i GoldenGate konfiguratsiooni kinnitamiseks Sisestage rea saidi A. kaugtöölaua saidi A. avada SQL-andmebaasi * pluss käsuviiba aken ja Käivita: SQL-i > valige nimi v$ andmebaasist;

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Märkige siis, kui rea on kopeeritud saidi B. Selle tegemiseks kaugtöölaua saidi B. avatud SQL-i pluss aken ja käivitage:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Uue kirje lisamiseks veebisaidil saidi B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Saidi ja kontrollige, kas selle dispersioonanalüüs toimunud Kaugtöölaud:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Lisaressursid
[Oracle'i virtuaalse masina pilte Azure](virtual-machines-linux-classic-oracle-images.md)
