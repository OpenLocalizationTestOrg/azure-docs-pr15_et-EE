<properties 
    pageTitle="Andmete Factory Kasutusmall - toote soovitused" 
    description="Lisateavet mõne kasutamine juhul rakendada, kasutades Azure'i andmed Factory koos muude teenustega." 
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
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Kasutusmall - toote soovitused 

Azure'i andmed Factory on üks rakendamiseks Cortana ärianalüüsi komplekti lahenduse kiirendite palju teenuseid.  Vaadake [Cortana ärianalüüsi komplekti](http://www.microsoft.com/cortanaanalytics) lehelt selle komplekti kohta. Selles dokumendis on kirjeldatud levinud kasutamine kaasus, millele Azure'i kasutajate juba lahendatud ja rakendada Azure'i andmed Factory ja muud Cortana ärianalüüsi Komponentteenused abil.

## <a name="scenario"></a>Stsenaarium

Tavaliselt soovite veebikauplustelt ahvatleb oma klientidele osta tooteid, esitades neile need on tõenäoliselt kõige enam huvi pakkuda, ja seetõttu tõenäoliselt osta. Selleks on vaja veebikauplustelt kohandada oma kasutaja veebifunktsioonide teatud kasutaja jaoks isikupärastatud toote soovituste abil. Isikupärastatud soovituste on praeguse ja ajalooliste käitumine andmed, tooteteave, äsja kasutusele kaubamärkide ja toodetele ja klienditeenindusele osadeks andmete alusel.  Lisaks saate need annavad analüüsil üldine kasutus käitumine kõigi nende kasutajate ühendatud kasutaja toote soovitused.

Nende jaemüüjate eesmärk on kasutaja klõpsake-müük dokumenditeisenduste optimeerimine ja teenida suurem müügitulu.  Need saavutada ümberarvutamine pakkudes klientide huvide ja toimingute kontekstipõhine, käitumine toote soovitusi. Kasuta puhul kasutame veebikauplustelt näide asutused soovite optimeerida oma klientidele. Siiski neid põhimõtteid mõni ettevõte, mis soovib oma klientidele oma toodete ja teenuste ümber ja Täiustage oma klientide ostmise isikupärastatud toote soovitusi.

## <a name="challenges"></a>Probleemid

Puuduvad paljude probleemide veebikauplustelt poole, mis kui proovite seda tüüpi kasutamine juhul rakendada. 

Esmalt manustada märkimisväärselt erineva suuruse ja kujuga andmeid mitmest andmeallikast, asutusesiseselt ja pilves. Need andmed sisaldavad kasutaja andmete toote põhiandmed, ajalooliste kliendiandmete käitumine ja kasutajale sirvib veebikaubandus saidi. 

Teine, isikupärastatud toote soovitused peab olema mõistlik ja täpselt arvutatud ja ennustatud. Lisaks toote, kaubamärgi ja käitumine ja brauseri kliendiandmete, veebikauplustelt ka peavad sisaldama klientide tagasiside varasemate ostude parima toote soovitused kasutaja määramisel arvestada. 

Kolmandaks, tuleb kohe projektitulemi kasutajale sujuvalt sirvimise ja ostmist kogemusi ja kõige uuemaid ja asjakohaste soovituste soovitusi. 

Lõpuks jaemüüjad vaja mõõta oma lähenemisviisi tõhusust jälgimise üldine üles-müük rist-sell klõpsake-teisendamine müügi edu ja kohandada oma tulevaste soovitusi.

## <a name="solution-overview"></a>Lahendus ülevaade

Selles näites kasutamine juhul on lahendatud ja rakendavad real Azure'i kasutajate Azure'i andmed Factory ja muud Cortana ärianalüüsi Komponentteenused, sh [Hdinsightiga](https://azure.microsoft.com/services/hdinsight/) ja [Power BI](https://powerbi.microsoft.com/)abil.

Online edasimüüjalt kasutab Azure'i bloobimälu poest, asutusesisese SQL server, Azure SQL-i DB ja relatsiooniliste andmete mart nende andmete talletamise võimalused kogu töövoo.  Bloobimälu poe sisaldab kliendi teavet, kliendiandmete käitumine ja toote teavet andmeid. Toote teabe andmed sisaldavad kaubamärgi tooteteave ja toote kataloogi salvestatud asutusesisese SQL-i andmebaas. 

Kõik andmed on ühendatud ja sisestatakse toote soovitus süsteemi esitamisel isikupärastatud soovitused vastavalt kliendi huvide ja toiminguid, samal ajal, kui kasutaja sirvib toodete kataloogi veebisaidil. Klientide vaadata ka tooted, mis on seotud toote nad otsivad põhjal mis tahes ühe kasutaja seotud üldised veebisaidi mustreid.

![diagrammi](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabaiti töötlemata web logifailid luuakse iga päev online edasimüüjalt veebisaidilt poolstruktuur-failina. Töötlemata web logifailid ning kliendi- ja toote kataloogi teabe on märkimisväärselt regulaarselt Azure'i bloobimälu abil andmete Factory Globaalselt juurutatud andmete liikumine teenust sisse. Töötlemata logifailide päeva on liigendatud (aasta ja kuu) bloobimälu pikemaks jaoks sisse.  [Windows Azure Hdinsightiga](https://azure.microsoft.com/services/hdinsight/) kasutatakse partition töötlemata logifailide bloobimälu poes ja töötlemine sissevõetava logid tasandil taru nii siga skriptide abil. Sektsioonitud web logide andmeid töödeldakse seejärel eraldamiseks Õppekeskuse soovitus süsteemis luua isikupärastatud toote soovitused masina vaja sisendeid.

Soovitused süsteemi masina Õppekeskuse selles näites kasutatakse on avatud allika arvutisse, mis Õppekeskuse soovitus platvormi [Apache Mahout](http://mahout.apache.org/).  Mis tahes [Azure'i masina õ](https://azure.microsoft.com/services/machine-learning/) või kohandatud mudelit saab rakendada seda stsenaariumi.  Mahout mudelit kasutatakse prognoosida üksuste põhjal üldine mustreid veebisaidil seoseid ja luua isikupärastatud soovitused üksikkasutajate põhjal.

Lõpuks tulemite hulk isikupärastatud toote soovituste teisaldatakse relatsiooniliste andmete mart ettenähtud järgi veebisaidile edasimüüjalt.  Tulemite hulk võib ka otse bloobimälu korrad mõnda muusse rakendusse või muude tarbijatele ja kasutamise juhtudel täiendavad poed teisaldada.

## <a name="benefits"></a>Eelised

Optimeerides oma toote soovitus strateegia ja vastavusse ettevõtte eesmärkide, täidetud lahendus online edasimüüjalt merchandising ja turundus eesmärgid. Lisaks nad saavad kiireks ja hallata toote soovitus töövoo tõhusalt, usaldusväärseid ja tõhus. Lähenemisviisi lihtne neid värskendada oma mudeli ja selle tõhusust mõõdud müük nuppu-teisendamine õnnestumiste põhjal täpsustada. Azure'i andmed Factory abil need olid loobuda nende ja kulukad käsitsi pilve ressursside haldus ja nõudmisel cloud ressursihalduse viimine. Seetõttu need olid saate säästa aega, raha ja aega vähendada lahenduse juurutamine. Lihtne visualiseerimine ja intuitiivne andmete Factory jälgimise ja haldamise kasutajaliides, mis on saadaval Azure portaalist tõrkeotsing said pärandi andmevaadete ja funktsionaalseid teenuse seisund. Nüüd saate oma lahenduse ajastatud ja nii, et lõpetanud andmete usaldusväärselt toodeti ja saata kasutajatele ja inimsekkumist automaatselt hallatavate andmete ja sõltuvused töötlemine õnnestus.

Pakkudes selle isikupärastatud ostu kogemus online edasimüüjalt, mis on loodud rohkem konkurentsianalüüsi, kaasates klientide kogemusi ja seetõttu suurendada müügi ja kogu klientide rahulolu.



  