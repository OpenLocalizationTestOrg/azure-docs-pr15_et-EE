<properties 
    pageTitle="Tõrkeotsing ja küsimused rakenduse ülevaated" 
    description="Midagi rakenduses Visual Studio rakenduse ülevaated segane või ei tööta? Proovige siin." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Küsimused - ASP.net-i rakenduse ülevaated

## <a name="configuration-problems"></a>Konfigureerimisprobleemide

*Mul on probleeme minu:*

* [.Net-i rakendus](app-insights-asp-net-troubleshoot-no-data.md)
* [Rakendus töötab juba jälgimine](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure'i diagnostika](app-insights-azure-diagnostics.md)
* [Java web app](app-insights-java-troubleshoot.md)
* [Muude platvormide jaoks loodud](app-insights-platforms.md)

*Pole andmete toomine minu server*

* [Automaatkorrektuuri erandite määramine tulemüüri](app-insights-ip-addresses.md)
* [ASP.net-i serveri seadistamine](app-insights-monitor-performance-live-website-now.md)
* [Java serveri häälestamine](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Kas saan kasutada rakenduse arusaamiseks...

[Vt platvormid][platforms]


## <a name="is-it-free"></a>See on tasuta?

* Jah, kui valite tasuta [hinnad taseme](app-insights-pricing.md). Saate enamik funktsioone ja rikkalikku piirmäära andmed. 
* Tuleb sisestada oma krediitkaardi andmed registreerida Microsoft Azure'i, kuid pole teha juhul, kui teise tasulise Azure'i teenust kasutada või konkreetselt intressi taseme versioonitäiendust.
* Kui teie rakendus saadab rohkem andmeid, kui kuu kvoodi tasuta kiht, lakkab logitud. Sel juhul saate saate valida alustada pöörates või oodake, kuni kvoodi lähtestatakse kuu lõpu seisuga.
* Peamine kasutus- ja seansi andmete ei kuulu kvoodi.
* Olemas on ka 30-päevane tasuta prooviversioon, mil, siis saate tasuta makstud funktsioonid.
* Iga ressurssi eraldi kvoot ja seate oma hinnakirjad taseme sõltumatult, mis tahes muud.

#### <a name="what-do-i-get-if-i-pay"></a>Mida ma saan, kui ma maksta?

* Suurem [kuu piirmäära andmed](https://azure.microsoft.com/pricing/details/application-insights/).
* Võimalus maksta aegunud andmete kogumise üle kuu kvoodi jätkata. Kui teie andmed läheb üle piirmäära, saate nende eest arve Mb kohta.
* [Pidev eksport](app-insights-export-telemetry.md).


## <a name="q14"></a>Mida rakenduse ülevaated minu projekti muuta?

Üksikasjad sõltuvad projekti tüüp. Veebirakenduse:


+ Projekti lisab need failid:

 + ApplicationInsights.config. 
 + AI.js


+ Installib need Nugeti paketid:

 -  *Rakenduse ülevaateid API* - API core

 -  *Rakenduse ülevaateid API veebirakenduste* - serverist telemeetria saatmiseks

 -  *Rakenduse ülevaateid API JavaScripti rakenduste* - kliendi telemeetria saatmiseks

    Paketid on järgmised nende assemblereid.

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Üksuste importimine lisatakse:

 - Web.config

 - packages.config

+ (Uus projektid ainult – kui saate [lisada rakenduse ülevaated olemasoleva projekti][start], tuleb teha seda käsitsi.) Lisab Koodilõigud kliendi ja serveri koodi lähtestada need rakenduse ülevaated ressursi ID-ga. Näiteks rakenduses MVC lisatakse koodi juhtlehe Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Kuidas uuendada SDK versioonidest?

Lugege teemat [vabastage märkmete](app-insights-release-notes.md) Tarkvaraarenduskomplektist, mis on asjakohane teie tüüpi rakendus. 



## <a name="update"></a>Kuidas muuta mis Azure ressursi oma projekti saadab andmeid?

Paremklõpsake Solution Exploreris `ApplicationInsights.config` ja klõpsake nuppu **Värskenda rakenduse ülevaated**. Saate saata Azure ressursside olemasolevad või uued andmed. Värskendamise viisard muudab instrumentation klahvi ApplicationInsights.config, mis määratleb, kui server SDK saadab oma andmed. Kui tühjendate "Värskenda kõik", see muudab kuvamiskoha veebilehel võti.


#### <a name="data"></a>Kui kaua andmeid säilitatakse portaali? See on turvaline?

Heitke pilk [andmete säilitamise ja privaatsuse][data].

## <a name="logging"></a>Logimine

#### <a name="post"></a>Kuidas ma näen postituse andmete diagnostika otsing?

Me ei Logi postituse andmed automaatselt, kuid saate kasutada TrackTrace kõne: sõnumi parameetrit andmed panna. See on rohkem suuruspiirangut piirangud stringi atribuudid, kui, kuigi seda ei saa filtreerida. 

## <a name="security"></a>Turvalisus

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Kas mu andmed on turvaline portaali? Kui kaua säilitatakse?

Vt [andmete säilitamise ja privaatsuse][data].


## <a name="q17"></a>Mul on lubatud kõik rakenduse ülevaated?

<table border="1">
<tr><th>Peaksite nägema</th><th>Hankimine</th><th>Miks soovite</th></tr>
<tr><td>Kättesaadavus diagrammid</td><td><a href="../app-insights-monitor-web-app-availability/">Web testide</a></td><td>Oma veebirakenduse on üles leida</td></tr>
<tr><td>Serveri rakenduse täiuslik: vastuse korda,...
</td><td><a href="../app-insights-asp-net/">Rakenduse ülevaated lisamine projekti</a><br/>või <br/><a href="../app-insights-monitor-performance-live-website-now/">Installige AI oleku jälgimine server</a> (või kirjutage oma kood <a href="../app-insights-api-custom-events-metrics/#track-dependency">sõltuvuste</a>jälitamiseks)</td><td>Täiuslik probleemide tuvastamine</td></tr>
<tr><td>Sõltuvus telemeetria</td><td><a href="../app-insights-monitor-performance-live-website-now/">Installige server AI oleku jälgimine</a></td><td>Diagnoosimine andmebaasid ja muud välise komponendid probleemid</td></tr>
<tr><td>Erandid virnas jälgi toomine</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">Lisa TrackException kõned koodi</a> (kuid mõned esitatakse automaatselt)</td><td>Tuvastada ja erandid diagnoosimine</td></tr>
<tr><td>Otsingu log jälgi</td><td><a href="../app-insights-search-diagnostic-logs/">Logimise adapterit lisamine</a></td><td>Diagnoosimine erandid täiuslik probleemid</td></tr>
<tr><td>Kliendi kasutamise põhialused: lehe vaateid, seansid,...</td><td><a href="../app-insights-javascript/">JavaScripti initializer veebilehtedel</a></td><td>Kasutusanalüüsi</td></tr>
<tr><td>Kliendi kohandatud mõõdikud</td><td><a href="../app-insights-api-custom-events-metrics/">Veebilehtede kutsub jälgimine</a></td><td>Kasutaja töötamist tõhustada</td></tr>
<tr><td>Serveri kohandatud mõõdikud</td><td><a href="../app-insights-api-custom-events-metrics/">Serveri kood kõnede jälgimine</a></td><td>Ärianalüüs</td></tr>
</table>


## <a name="automation"></a>Automatiseerimine

Saate [kirjutada PowerShelli skriptide](app-insights-powershell.md) loomine ja värskendamine rakenduse ülevaated ressursid.

## <a name="more-answers"></a>Rohkem vastuseid

* [Rakenduse ülevaateid Foorum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 