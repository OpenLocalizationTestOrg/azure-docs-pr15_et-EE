<properties 
    pageTitle="Rakenduse kaart rakenduse ülevaated | Microsoft Azure'i" 
    description="Selle rakenduse komponendid, sõltuvuste visuaalse esituse sildistatud KPI-d ja teatisi." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Rakenduse kaart rakenduse ülevaated

[Visual Studio rakenduse ülevaated](app-insights-overview.md), on rakenduse kaardi sõltuvus seosed rakenduse komponentide visuaalse paigutus. Iga komponendi kuvatakse KPI-d, nt laadi, jõudluse, tõrgete ja teatised, aidata leida mis tahes osa põhjustab JÕUDLUSPROBLEEM või tõrge. Võite klõpsata kaudu, mis tahes osa üksikasjalikumat diagnostika, rakenduse ülevaated, nii et ja, kui teie rakendus kasutab Azure teenused - Azure diagnostika, nagu SQL-i andmebaasi Advisor soovitused.

Muud diagrammid, nt saate kinnitada rakenduse vastenduse Azure armatuurlaud, kus on täiesti töökorras. 

## <a name="open-the-application-map"></a>Avage rakendus kaart

Avage rakenduse keelest ülevaade kaart.

![Avage rakendus kaart](./media/app-insights-app-map/01.png)

![rakenduse kaart](./media/app-insights-app-map/02.png)

Kaardil kuvatud.

* Kättesaadavus kontrollib
* Kliendi pool osa (JavaScripti SDK)
* Serveri pool komponent
* Kliendi ja serveri komponendid sõltuvused

Saate laiendada ja ahendada sõltuvus link rühmad:

![ahendamine](./media/app-insights-app-map/03.png)
 
Kui teil on suur hulk sõltuvused ühte tüüpi (SQL-i, HTTP jne), nad võivad ilmuda rühmitatud. 


![rühmitatud sõltuvused](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Koht probleemid

Iga sõlme on oluline mõõdikud, nagu on mõeldud komponendi laadimine, jõudlus ja tõrge. 

Hoiatus ikoonid esile tõsta võimalikud probleemid. Mõne oranž Hoiatus tähendab, et esinevad tõrked taotlusi, lehe vaated ega sõltuvus kõnesid. Punane tähendab, et ebaõnnestumiste määra üle 5%.


![tõrge ikoonid](./media/app-insights-app-map/04.png)

 
Aktiivne teatiste häälestamine ka kuva: 


![aktiivse teatised](./media/app-insights-app-map/05.png)
 
Kui kasutate SQL Azure'i, on ikoon, mis näitab, kui seal on soovitused, kuidas saate parandada jõudlust. 


![Azure'i soovitus](./media/app-insights-app-map/06.png)

Klõpsake mis tahes ikooni rohkem üksikasju.


![Azure'i soovitus](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnostika käsku läbi

Kõik sõlmed kaardil pakub kaudu suunatud nuppu diagnostika. Suvandid sõltuvad sõlme tüüp.

![serveri suvandid](./media/app-insights-app-map/09.png)

 
Komponendid, mis on majutatud Azure suvandid lisada neile otselingid.


## <a name="filters-and-time-range"></a>Filtrid ja ajavahemiku

Vaikimisi on kaardi Kokkuvõte kõik andmed, mis on valitud ajavahemiku jaoks saadaval. Kuid saate seda, et kaasata ainult teatud toimingu nimed või sõltuvused filtreerida.

* Toimingu nimi: See hõlmab nii lehe vaated ja küljel taotluse tüübid. See suvand, kuvatakse kaardi KPI server/kliendi pool sõlm üksnes valitud toimingute jaoks. See näitab teatud toimingute kontekstis nimetatakse sõltuvused.
* Sõltuvus base nimi: See hõlmab AJAXI brauseri Serveripoolsed sõltuvused ja Serveripoolsed sõltuvused. Kui te aruande kohandatud sõltuvus telemeetria TrackDependency API-ga, need kuvatakse ka siin. Saate valida sõltuvused kaardil kuvada. Pange tähele, et sel ajal, see pole filtreeritakse serveri pool taotlusi või kliendi pool lehe vaated.


![Filtrite määramine](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Filtrite salvestamine

Rakendatud filtrite salvestamiseks kinnitada peale [armatuurlaua](app-insights-dashboards.md)filtreeritud vaatesse.


![Armatuurlauale kinnitamine](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Tagasiside

Palun [tagasiside kaudu portaali tagasiside suvand](app-insights-get-dev-support.md).


![MapLink-1 pilt](./media/app-insights-app-map/13.png)


