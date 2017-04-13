<properties
    pageTitle="Vabastage marginaalid jaoks rakenduse ülevaated | Microsoft Azure'i"
    description="Juurutamise lisada või luua oma mõõdikute Exploreri diagrammide rakenduse ülevaated markerid."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Klõpsake rakenduse ülevaated väljaanne marginaalid

Vabastage [Mõõdikute Exploreri](app-insights-metrics-explorer.md) virnlintdiagrammidel kuvatakse kui olete juurutanud uue Koosta marginaale. Need võimaldavad hõlpsalt vaadata, kas teie muudatused oli mingit mõju rakenduse jõudlus. Ta saab automaatselt luua [Visual Studio Team Services koostada süsteemi](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)ja saate teha ka [need PowerShelli kaudu luua](#create-annotations-from-powershell).

![Näide: nähtav vastavus serveri aega ja marginaalid](./media/app-insights-annotations/00.png)

Väljalaske marginaalid on funktsioon pilvepõhist koostamine ja vabastage teenuse Visual Studio meeskonnatöö teenuste. 

## <a name="install-the-annotations-extension-one-time"></a>Installige marginaalid laiend (üks kord)

Võimalik luua väljaanne marginaalid, peate installima saadaval palju meeskonnatöö teenuse laiendid Visual Studio turuplatsi.

1. Logige sisse oma [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.
2. Visual Studio turuplatsil, [saada väljaanne marginaalid laiend](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), ja lisage see meeskonnatöö teenuste kontole.

![At teenuste meeskonnatöö veebileht, avatud turuplatsi parempoolses ülemises nurgas. Valige Visual meeskonnatöö teenused ja seejärel jaotises Koosta ja väljaanne, valige Vaata veel.](./media/app-insights-annotations/10.png)

Ainult peate tegema üks kord konto Visual Studio Team Services. Nüüd saab väljaanne marginaalid konfigureerida projekti teie konto. 

## <a name="get-an-api-key-from-application-insights"></a>Rakenduse ülevaated on API võti toomine

Peate tegema iga väljaanne malli loomiseks väljaanne marginaalid.


1. [Microsoft Azure portaali](https://portal.azure.com) sisse ja avage rakenduse ülevaated ressurss, mis jälgib rakenduse. (Või [kohe](app-insights-overview.md), kui te pole seda veel teinud).
2. Avage **API Access**ja tutvuge **Rakenduse ülevaateid Id**koopia.

    ![Avage oma rakenduse ülevaated ressursi portal.azure.com, ja valige sätted. Avage Accessi API-ga. Kopeerige rakenduse ID](./media/app-insights-annotations/20.png)

2. Eraldi brauseriaknas, avage (või looge) Väljalaske Mall, mida haldab teie juurutuste Visual Studio meeskonnatöö teenuste. 

    Tööülesande lisamine ja valige menüüst käsk rakenduse ülevaateid väljaanne marginaali tööülesande.

    Kleepige **Rakenduse Id** keelest API juurdepääsu kopeeritud.

    ![Visual Studio meeskonnatöö teenused, avage väljaanne, valige väljaanne määratluse ja nuppu Redigeeri. Klõpsake tööülesande lisamiseks ja valige rakenduse ülevaateid väljaanne marginaale. Kleepige ülevaateid rakenduste ID-ga.](./media/app-insights-annotations/30.png)

3. Määrake väljal **APIKey** muutuja `$(ApiKey)`.

4. Olles tagasi kohas API juurdepääsu labale Loo uus API võti ja sellest koopia teha.

    ![Klõpsake Azure aknas labale API juurdepääsu loomine API võti. Pakkuda kommentaari, märkige ruut kirjutamine ja marginaalid ja klõpsake nuppu Loo tootevõti. Kopeerige uue tootenumbri.](./media/app-insights-annotations/40.png)

4. Avage vahekaart konfiguratsiooni väljaanne malli.

    Muutuv definitsiooni loomine `ApiKey`.

    Kleepige oma API võti ApiKey muutuv määratlus.

    ![Meeskonnatöö teenuste aknas valige vahekaart konfigureerimine ja klõpsake nuppu Lisa muutuv. Saate määrata nime ApiKey ja väärtuse, kleepige äsja loodud võti.](./media/app-insights-annotations/50.png)


5. Lõpuks **salvestamine** väljaanne määratlus.

## <a name="create-annotations-from-powershell"></a>PowerShelli marginaalid loomine

Saate luua ka marginaale mis tahes protsess teile meeldib (ilma VS meeskonnatöö süsteemi abil). 

Saan [PowerShelli skripti kaudu GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

Kasutage seda umbes järgmine:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Hankida soovitud `applicationId` ja `apiKey` oma rakenduse ülevaated ressurss: Avage sätted, API juurdepääsu ja kopeerige rakenduste ID-d. Klõpsake nuppu Loo API võti ja kopeerige võti. 

## <a name="release-annotations"></a>Vabastage marginaalid

Nüüd, kui väljaanne malli abil saate juurutada uus versioon, marginaali saadetakse rakenduse ülevaated. Diagrammide mõõdikute Explorer kuvatakse valitud marginaalid.

Klõpsake üksikasjade väljaanne, sh planeerimisüksused andmeallika juhtelemendi haru avamiseks mis tahes marginaali marker, vabastage määratlus, keskkonna ja muu.


![Klõpsake mis tahes väljaanne marginaali marker.](./media/app-insights-annotations/60.png)
