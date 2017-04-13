<properties 
    pageTitle="Sõiduki telemeetria analytics lahenduse playbook | Microsoft Azure'i" 
    description="Cortana ärianalüüsi võimaluste kasutamiseks saada reaalajas ja ennustav ülevaateid sõiduki seisundi ja juhtimise tarbetu." 
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
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Sõiduki telemeetria analytics lahenduse playbook

See **menüü** linke selle playbook peatükke. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Ülevaade
Super arvutid liikunud välja ja nüüd on pargitud meie garaažis! Need tipptasemel automobiles sisaldavad hulgaliselt andurid, andes neile jälgimiseks ja sündmuste miljoneid iga teine võimalus. Loodame, et 2020 enamik nende auto on on antud Interneti-ühendus. Kujutage ette, kasutades seda hulgaliselt andmeid esitada klassi turvalisuse, töökindluse ja juhtimise kogemus kõige paremini ära! Microsoft on see unistus koos Cortana ärianalüüsi tegelikult teinud.

Microsofti Cortana Ajateabe on on täielikult hallatud big data ja täiustatud analüüsi komplekt, mis võimaldab teil muuta oma andmete nutikad toiming. Soovime tutvustada Cortana ärianalüüsi sõiduki telemeetria Analytics lahenduse mall. See lahendus näitab, kuidas autosalongides, auto tootjad ja kindlustusseltside saate kasutada Cortana ärianalüüsi võimaluste reaalajas ja harjumused sõnastikupõhise teadmisi sõiduki seisundi ja juhtimine. 

Lahendus on rakendada [lambda arhitektuur mustri](https://en.wikipedia.org/wiki/Lambda_architecture) kuvatud täieliku võimalikud Cortana ärianalüüsi platvormi reaalajas ja Pakktöötlus. Lahendus: 

- pakub sõiduki telemaatika simulaatorit
- mõjutab sündmuse jaoturi söömisega Azure'i sisse miljoneid jäljendatud sõiduki telemeetria sündmuste jaoks 
- kasutab voo Analytics saada reaalajas teadmisi sõiduki seisundi kohta
-  püsib andmed üheks pikemaks rikkalikumat paketi Analyticsi. 
- ära masina õ tuvastamine normaalne jaoks reaalajas ja partii töötlemine sõnastikupõhise teadmisi saada.
- mõjutab Hdinsightiga muuta andmete skaala ja andmete Factory käsitlema korraldamise, ajastamise, ressursside haldamine ja Pakktöötlus müügivõimaluste jälgimine 
- See lahendus annab rikkaliku armatuurlaua reaalajas andmed ja ennustav visualiseerimisi Power BI

## <a name="architecture"></a>Arhitektuur

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Joonis 1 – sõiduki telemeetria Analytics lahenduse arhitektuur*

See lahendus sisaldab järgmisi **Cortana ärianalüüsi komponendid** ja tutvustatakse nende lõpuni integreerimine


- **Sündmuse jaoturi** söömisega miljoneid sõiduki telemeetria sündmuste Azure'i sisse.
- **Voo Analytics** üha reaalajas teadmisi, sõiduki seisund ei lahene andmeid pikemaks rikkalikumat paketi Analyticsi sisse.
- **Seadme õ** normaalne avastamiseks reaalajas ja Pakktöötlus sõnastikupõhise teadmisi saada.
- **Hdinsightiga** on võimendada, et muuta andmete skaala
- **Andmete Factory** tegeleb korraldamise, kavandamine, ressursside haldamine ja Pakktöötlus müügivõimaluste jälgimine.
- **Power BI** annab see lahendus rikkaliku armatuurlaua reaalajas andmed ja ennustav visualiseeringuid.

See lahendus pääseb kaks eri **andmeallikatega**: 

- **Modelleerida sõiduki signaale ja diagnostika**: sõiduki telemaatika simulaatorit eraldab diagnostikateave ja olek kuvatakse auto-ja juhtimise mustri antud hetkel signaale teineteisele aega. 
- **Sõiduki kataloogi**: viide andmekomplekti, mis sisaldab mõnda VIN mudeli kaardistamine.
