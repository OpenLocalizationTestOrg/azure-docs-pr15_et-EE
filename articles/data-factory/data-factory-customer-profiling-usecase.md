<properties 
    pageTitle="Kasutusmall - kliendi profiili" 
    description="Siit saate teada, kuidas saab luua Andmepõhiste töövoo (müügivõimaluste) profiili mängimine kliendid Azure'i andmed Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Kasutusmall - profiilide klient

Azure'i andmed Factory on üks rakendamiseks Cortana ärianalüüsi komplekti lahenduse kiirendite palju teenuseid.  Cortana ärianalüüsi kohta lisateabe saamiseks külastage [Cortana ärianalüüsi komplekti](http://www.microsoft.com/cortanaanalytics). Selles dokumendis kirjeldame lihtsa kasutamine juhtumi aitavad alustamine mõista, kuidas saate Azure'i andmed Factory analytics probleemide lahendamise.

Juurdepääs ja proovida juhul lihtne kasutamine on vaja on [Azure tellimust](https://azure.microsoft.com/pricing/free-trial/).  Näide, mis rakendab kasutamine sel juhul [näidised](data-factory-samples.md) artiklis kirjeldatud juhiseid järgides saate juurutada.

## <a name="scenario"></a>Stsenaarium

Contoso on mängimine ettevõte, mis loob visualiseerimine mitme platvormid: mängu Mängukonsoolid, toimub käsitsi seadmed ja arvutites (PC). Need mängida mängijad, suure hulga logiandmed on esitada mis jälgib mustreid, mängimine laad ja kasutajale eelistusi.  Kui koos demograafilisi, piirkondlike, ja toote põhiandmed, Contoso saate teha analüüsi, et suunata neid kohta, kuidas mängijad töötamist tõhustada ja target uuendamine neile ja -mängu ostud. 

Contoso's eesmärk on üles müüa/rist müüa võimalusi vastavalt oma mängijad mängimine ajalugu ja pilkupüüdvaid funktsioonide lisamine ketas ettevõtete kasvu ja sobivad paremini klientidele. Kasutamine juhul kasutame mängimine ettevõtte äritegevuse näidisena. Ettevõtte soovib optimeerida oma visualiseerimine mängijad käitumise põhjal. Mõni ettevõte, mis soovib oma klientidele ümber oma toodete ja teenuste ja nende klientide töötamist tõhustada neid põhimõtteid rakendada.

## <a name="challenges"></a>Probleemid


## <a name="solution-overview"></a>Lahendus ülevaade

Näide selle kohta, kuidas saate kasutada Azure'i andmed Factory neelata, koostada, muuta, analüüsida ja avaldada andmeid saab sel juhul lihtne kasutada.

![Töövoo lõpuni](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Järgmisel joonisel on esitatud torujuhtmete andmete kuvamise Azure portaali pärast nende juurutamist.

1.  **PartitionGameLogsPipeline** loeb bloobimälu töötlemata mängu sündmused ja loob sektsioonid aasta, kuu ja päeva alusel.
2.  **EnrichGameLogsPipeline** ühendab sektsioonitud mängu sündmused geo kood viite andmetega ja rikastab andmed, vastendades IP-aadresside vastavate geo asukohtadesse.
3.  Müügivõimaluste **AnalyzeMarketingCampaignPipeline** kasutab täiustatud andmeid ja luua lõplik väljund, mis sisaldab turundustegevuse turunduskampaania tõhustada andmetega reklaami töötleb.

Selles näites kasutatakse andmete Factory korraldab tegevused, mida sisendandmete, transformatsioon ja protsessi andmed kopeerida ja Azure SQL-andmebaasi lõplik andmeid.  Saate visualiseerida andmeid torujuhtmete võrgu, neid hallata ja jälgida olekut UI.

## <a name="benefits"></a>Eelised

Optimeerides oma kasutaja profiili analüüsi ja vastavusse ettevõtte eesmärkide, mängimine ettevõte on saavad kiiresti koguda mustreid ja selle turunduskampaaniate analüüsimiseks.




