<properties 
    pageTitle="SQL-i andmebaas TB andmete laadimine | Microsoft Azure'i" 
    description="Näitab, kuidas 1 TB andmeid saab laadida SQL Azure'i andmebaas jaotises 15 minutit koos Azure'i andmed Factory" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>1 TB laadimine SQL Azure'i andmebaas alla 15 minutit koos Azure'i andmed Factory [Kopeeri viisardi]
[SQL Azure'i andmebaas](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) on võimeline töötlemiseks suuri andmemahtusid, relatsiooniline- ja relatsiooniline pilvepõhist, skaala-out andmebaas.  Ehitatud oluliselt paralleelne töötlemine (MPP) arhitektuuri, SQL-i andmebaas on optimeeritud ettevõtte andmed ladu töökoormus.  See pakub pilveteenuse elastsuse paindlikult mastaapimiseks salvestusruumi ja arvutada sõltumatult.

Alustamine: SQL Azure'i andmebaas on nüüd lihtsam kui **Azure'i andmed Factory**rakenduse abil.  Azure'i andmed Factory on täielikult hallatud pilvepõhist andmete integreerimise teenust, mida saab kasutada SQL-i andmebaas teie olemasoleva süsteemi ja säästab väärtuslik ajal, kui hindamisel SQL-i andmebaas ja teie Analyticsi lahenduste peal andmetega asustada.  Siin on võtme eelised Azure SQL-i andmebaas Azure'i andmed Factory abil andmete laadimine.

- **Lihtne häälestamine**: 5 sammu intuitiivne viisardi abil skriptimise, nõutav.
- **Rikkaliku andmete talletamiseks tugi**: suurel hulgal erinevaid asutusesisese ja pilvepõhise andmete tugi.
- **Turvaline ja nõuetele vastavuse**: andmete kantakse üle HTTPS- või ExpressRoute ja globaalne teenuse kohaloleku tagab, et teie andmed ei kao geograafiliste äärist
- **Enneolematu jõudluse abil PolyBase** – Polybase abil on kõige tõhusam viis andmete teisaldamiseks SQL Azure'i andmebaas. Lavastus bloobimälu funktsiooni abil saate saavutada suure koormuse kiirust kõik tüüpi andmete poed peale Azure'i bloobimälu, mis on Polybase toetab vaikimisi.

Selles artiklis kirjeldatakse, kuidas laadimine 1 TB andmete Azure'i bloobimälu Azure SQL-i andmebaas väljal 15 minutit, üle 1.2 Gbit läbilaskevõime andmete Factory koopia viisardi abil.

Selles artiklis antakse üksikasjalikud juhised liiguvad SQL Azure'i andmebaas andmete kopeerimine viisardi abil. 

> [AZURE.NOTE] Vt artikli [ja sealt Azure SQL-i andmebaas Azure'i andmed Factory kasutamine andmete teisaldamiseks](data-factory-azure-sql-data-warehouse-connector.md) andmete Factory üldist teavet rakenduses andmete teisaldamine SQL Azure'i andmebaas/võimaluste kohta. 
> 
> Samuti saate koostada torujuhtmete Azure'i portaalis, Visual Studio PowerShelli, jne. Vt [õpetus: Azure'i bloobimälu andmete kopeerimine Azure'i SQL-andmebaasi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kiirülevaate selgituse koos üksikasjalike juhiste abil Kopeeri tegevuse Azure'i andmed Factory.  

## <a name="prerequisites"></a>Eeltingimused
- Azure'i bloobimälu: see katse kasutab Azure'i bloobimälu salvestusruumi (GRS) talletamiseks TPC-H testimise andmekomplekti.  Kui teil pole Azure storage konto, saate teada, [Kuidas luua konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account).
- Andmete [TPC-H](http://www.tpc.org/tpch/) : me kasutada TPC-H testimise andmekomplekti.  Selleks, et peate kasutama `dbgen` TPC-H tööriistakomplekt, mis aitab teil luua andmekomplekti.  Kas saate alla laadida lähtekoodi `dbgen` [TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) tööriistad ja koostada ise või alla [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools)kompileeritud kahendarvuks.  Käivitage järgmised käsud 1 TB lamefaili jaoks luua dbgen.exe `lineitem` tabeli ümbruses üle 10 faili:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Nüüd kopeerimine genereeritud failid Azure'i bloobimälu.  [Andmete teisaldamine ja sealt kohapealse failisüsteemi, kasutades Azure'i andmed Factory](data-factory-onprem-file-system-connector.md) kohta, kuidas seda teha viidata kasutades ADF eksemplari.   
- SQL Azure'i andmebaas: see katse laadib andmed Azure SQL-i andmebaas loodud 6000 DWUs

    Vaadake [loomine Azure SQL-i andmebaas](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) üksikasjalikud juhised, kuidas luua andmebaasi SQL-i andmebaas.  Parima võimaliku laadi jõudluse sattuda SQL-i andmebaas abil Polybase valisime andmete ladu ühikute (DWUs) jõudluse säte, mis on 6000 DWUs lubatud.

    > [AZURE.NOTE] 
    > Azure'i bloobimälu alla laadimisel laadimine jõudluse andmed on otse proportsionaalne arv DWUs konfigureerite SQL-i andmebaas:
    > 
    > 1 TB laadimine 1000 DWU SQL-i andmebaas suunab 87min (~ 200MBps läbilaskevõime) DWU SQL-i andmebaas 2000 1 TB laadimine võtab 46min (~ 380MBps läbilaskevõime) 1 TB laadimine 6000 DWU SQL-i andmebaas võtab 14min (~1.2GBps läbilaskevõime) 

    Luua SQL-i andmebaas 6000 DWUs, nihutage liugurit jõudluse täiesti õige:

    ![Jõudluse liugur](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Olemasoleva andmebaasi, mis on konfigureeritud 6000 DWUs, saate selle häälestada Azure'i portaalis skaala.  Liikuge andmebaasi Azure portaali ja on näidatud järgmisel pildil **Ülevaade** paanil **skaala** nupp.

    ![Skaala nupp](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Järgmised paani avamiseks nuppu **skaala** nihutage liugurit suurima väärtuse, ja klõpsake nuppu **Salvesta** .

    ![Dialoogiboksi skaala](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    See katse laadib andmed, kasutades SQL Azure'i andmebaas `xlargerc` ressursi klassi.

    Parima võimaliku läbilaskevõime saavutamiseks Kopeeri tuleb kasutada SQL-i andmebaas kasutaja, kes kuulub `xlargerc` ressursi klassi.  Saate teada, kuidas teha, et järgmised [muutmine kasutaja ressursside klassi näide](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Sihtkoha tabeli skeemi loomine SQL Azure'i andmebaas andmebaasi, käitades järgmine DDL-lause:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Eelnevalt nõutud juhiseid lõpule viia, oleme nüüd valmis konfigureerimine kopeerimine tegevuse Kopeeri viisardi abil.

## <a name="launch-copy-wizard"></a>Käivitage viisard kopeerimine

1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
2.  Valige **+ Uus** ülemises vasakus nurgas nuppu **ärianalüüsi + analytics**, ja klõpsake **Andmete Factory**. 
6. Klõpsake **uue andmete factory** keel:
    1. Sisestage **LoadIntoSQLDWDataFactory** **nimi**.
        Azure'i andmed factory nimi peab olema globaalselt kordumatu. Kui kuvatakse tõrketeade: **andmete factory nimi "LoadIntoSQLDWDataFactory" on pole saadaval**, muuta andmete factory (nt yournameLoadIntoSQLDWDataFactory) nime ja proovige uuesti luua. Nimede reeglid andmete Factory artefakte [Andmete Factory - nime andmise reeglid](data-factory-naming-rules.md) teemat.  
     
    2. Valige oma Azure'i **tellimus**.
    3. Ressursirühma, tehke järgmist. 
        1. Valige **Kasuta olemasolevat** ressursi olemasolevasse rühma valimiseks.
        2. Valige **Loo uus** sisestada ressursirühma nimi.
    3. Valige andmete factory **asukoht** .
    4. Märkige ruut **Kinnita armatuurlaua** tera allosas.  
    5. Klõpsake nuppu **Loo**.
10. Pärast loomist on lõpule jõudnud, kuvatakse **Andmete Factory** tera, nagu on näidatud järgmisel pildil.

    ![Andmete factory Avaleht](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Klõpsake avalehel andmete Factory, **Kopeerige andmed** paani **Kopeeri viisardi**käivitamiseks. 

    > [AZURE.NOTE] Kui näete, et veebibrauseri on ummikus "Lubatakse...", Keela ja tühjendage ruut **Blokeeri kolmanda osapoole küpsised ja saidi andmed** säte (või) jätta lubatud ja **login.microsoftonline.com** erand luua ja proovige seejärel uuesti käivitamist nõustaja.


## <a name="step-1-configure-data-loading-schedule"></a>Samm 1: Konfigureerida andmete laadimine ajakava
Kõigepealt tuleb teil konfigureerida andmete laadimine ajakava.  

Klõpsake lehe **Atribuudid** :
1. Sisestage **tööülesande nime** **CopyFromBlobToAzureSqlDataWarehouse**
2. Valige suvand **Käivita kohe üks kord** .   
3. Klõpsake nuppu **edasi**.  

![Wizard – lehe atribuutide kopeerimine](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Samm 2: Konfigureerimine allikas
Selles jaotises kuvatakse allika konfigureerimine juhiseid: Azure'i bloobimälu, mis sisaldab 1 TB TPC-H rida üksuse failid.

Valige **Azure'i bloobimälu** , nagu andmete talletamiseks ja klõpsake nuppu **edasi**.

![Kopeerige Wizard – valige allikas lehel](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Sisestage Azure'i bloobimälu salvestusruumi konto ühenduse teave ja klõpsake nuppu **edasi**.

![Kopeerige Wizard – andmeallika ühendusteabe](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Valige **kaust** , kus TPC-H reaüksuse failid ja klõpsake nuppu **edasi**.

![Kopeerige viisard – valige Sisestuskeel kaust](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Korral klõpsake nuppu **edasi**, tuvastatakse automaatselt faili vorming sätteid.  Veenduge, et veeru eraldaja on "|"asemel vaikimisi semikoolon",".  Klõpsake nuppu **edasi** , kui teil on andmete eelvaade.

![Kopeerige Wizard – faili vormingusätted](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Samm 3: Konfigureerimine sihtkoht
Selles jaotises kirjeldatakse, kuidas konfigureerida sihtkoht: `lineitem` SQL Azure'i andmebaas andmebaasi tabelisse.

Valige sihtkoha poe **SQL Azure'i andmebaas** ja klõpsake nuppu **edasi**.

![Kopeerige Wizard – valige sihtkoht andmesalve](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Sisestage SQL Azure'i andmebaas ühenduse teave.  Veenduge, et teie määratud kasutajale, mis on selle rolli liige `xlargerc` (vt **grammatikavigu üksikasjalikud juhised** ), ja klõpsake nuppu **edasi**. 

![Kopeerige Wizard – sihtkoha ühenduse teave](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Valige sihttabeli ja klõpsake nuppu **edasi**.

![Kopeerige Wizard – tabeli vastenduse lehel](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Nõustuge veeru vastendamise vaikesätteid ja klõpsake nuppu **edasi**.

![Kopeerige Wizard – skeemi vastenduse lehel](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Samm 4: Jõudluse sätted

**Luba polybase** on vaikimisi märgitud.  Klõpsake nuppu **edasi**.

![Kopeerige Wizard – skeemi vastenduse lehel](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Juhis 5: Juurutada ja laadi tulemuste jälgimine
Klõpsake nuppu **valmis** juurutamine. 

![Kopeerige Wizard – kokkuvõtte leht](media/data-factory-load-sql-data-warehouse/summary-page.png)

Kui juurutamise on lõpule jõudnud, klõpsake nuppu `Click here to monitor copy pipeline` Kopeeri käivitada edenemist jälgida.

Valige Kopeeri kohaletoimetamisel, mis on loodud **Tegevuse Windows** loendis.

![Kopeerige Wizard – kokkuvõtte leht](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

Saate vaadata käivitada üksikasjad **Tegevuse akna Exploreri** paremal, sh andmete maht allikast lugeda ja kirjutada sihtkoht, kestus ja Keskmine läbilaskevõime Käivita koopia.

Nagu näete järgmist ekraanipilt, kopeerida 1 TB: Azure'i bloobimälu SQL-i andmebaas võttis saavutada 1,22 Gbit läbilaskevõime 14 minutiga!

![Kopeerige Wizard – õnnestus dialoogiboks](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Head tavad
Siin on mõned soovitused, mis töötab SQL Azure'i andmebaas andmebaasi.

- Kasutage suuremat ressursi ainekursuse, kui laadimine rühmitatud COLUMNSTORE register.
- Tõhusam ühendused, on soovitatav kasutada, valige veeru vaikimisi round jaan jaotuse asemel räsi jaotuse.
- Kiirem laadimine kiirust, on soovitatav kasutada hunnik siirdamiseks andmete jaoks.
- Luua statistika, kui olete lõpetanud, laadimine SQL Azure'i andmebaas.

Üksikasjad leiate [head tavad SQL Azure'i andmebaas](../sql-data-warehouse/sql-data-warehouse-best-practices.md) . 

## <a name="next-steps"></a>Järgmised sammud
- [Andmete Factory kopeerimise viisard](data-factory-copy-wizard.md) – selles artiklis antakse Kopeeri viisardi üksikasjad. 
- [Kopeeri tegevuse jõudlus ja häälestamise juhend](data-factory-copy-activity-performance.md) – selles artiklis sisaldab viide jõudluse mõõtmed ja häälestamise juhend.

