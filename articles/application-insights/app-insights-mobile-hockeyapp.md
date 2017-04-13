<properties
    pageTitle="Jõudluse jälgimine mobiilne veebirakenduste arendaja Analytics | Microsoft Azure'i"
    description="Rakenduse jõudlus ja kasutuse jälgimise mobiilirakenduse arendajatele. , töölaua, veebiteenuse ja rakenduse ülevaated ja HockeyApp taustväärtus rakendused."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobiilse Analytics HockeyApp ja rakenduse ülevaated

Jälgida jõudlus ja oma beeta-testi ja [HockeyApp](https://hockeyapp.net/)juurutatud mobiil- ja lauaarvutite apps kasutamist. Täiendavad veebi teenuseid ja kirjutamata rakenduste [Visual Studio rakenduse](app-insights-overview.md)ülevaated jälgimine. Kliendi ja serveri rakenduste Analytics andmete analüüsimine ja diagrammide kõrvuti kuvamine rakenduse Azure armatuurlaud.

Microsoft arendaja Analytics pakub kahte lahendusi jälgige jõudlust:

* **HockeyApp Mobile** ja töölaua rakendused.
 * Test versioonid kindlatele kasutajatele levitada.
 * Analüüsi krahhi.
 * Kohandatud telemeetria Kasutusanalüüsi jaoks.
* **Rakenduse ülevaated veebilehtede** ja teenuste ja taustväärtus rakendused.
 * Mõõdikute jõudlus ja kasutus ja teatised.
 * Erandi aruandlus- ja diagnostika jälgimine.
 * Diagnostika otsing telemeetria filtreerimise ja seotud lingid.

Mõlema lahenduse pakuvad.

 * Võimas **[analüütiline päringukeele](app-insights-analytics.md)** diagnostika- ja analüüsida.
 * **[Andmete eksportimine](app-insights-export-telemetry.md)** oma salvestusruumi.
 * **Integreeritud armatuurlaua** kuvamine analüütiliste diagrammide ja tabelite.

## <a name="monitor-your-app-components"></a>Jälgida oma rakenduse komponendid

Järgige neid lehti jälgimise rakenduse installimiseks SDK koodi ja.

### <a name="web-apps---application-insights"></a>Veebirakenduste - rakenduse ülevaated

* [ASP.net-i veebirakenduse](app-insights-asp-net.md) 
* [Java web app](app-insights-java-get-started.md)
* [Node.js web app](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure pilveteenused](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobiilirakenduste - HockeyApp

* [iOS-i rakendus](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X rakendus](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Androidi](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universaalne Windowsi rakenduse](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 ja 8.1 rakendus](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windowsi esitlus Foundationi rakenduse](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Windowsi töölaua ja rakenduste, soovitame HockeyApp. Kuid võite ka [saata telemeetria rakenduse ülevaate saamiseks Windowsi rakenduse kaudu](app-insights-windows-desktop.md). Võite seda teha rakenduse ülevaated eksperimenteerida.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analüüsi- ja ekspordi puhul HockeyApp telemeetria

Saate uurida HockeyApp kohandatud ja logige telemeetria rakenduse ülevaated analüüsi- ja pidev eksportimine funktsioonide häälestamine [silla](app-insights-hockeyapp-bridge-app.md)abil.




