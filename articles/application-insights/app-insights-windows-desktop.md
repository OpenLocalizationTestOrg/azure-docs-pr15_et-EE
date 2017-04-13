<properties 
    pageTitle="Kasutus- ja jõudluse Windowsi töölaua rakenduste jälgimine" 
    description="Kasutus- ja jõudluse Windowsi töölaua rakenduse rakenduse ülevaated ja HockeyApp analüüsimine." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Kasutus- ja jõudluse Windowsi töölaua ja rakenduste jälgimine

*Rakenduse ülevaated on eelvaade.*

[Visual Studio rakenduse ülevaated](app-insights-overview.md) ja [HockeyApp](https://hockeyapp.net) abil saate jälgida oma juurutatud rakendusega kasutus ja jõudluse.

> [AZURE.IMPORTANT] Soovitame [HockeyApp](https://hockeyapp.net) levitamise ja töölaua- ja seadme rakenduste jälgimine. HockeyApp, kus saate hallata jaotuse, reaalajas testimine ja kasutajale tagasiside, samuti kasutus- ja krahhi aruannete jälgimine. Samuti saate [eksportida ja päringu oma telemeetria Analytics](app-insights-hockeyapp-bridge-app.md).

> Kuigi telemeetria saab saata rakenduse ülevaated töölauarakendus, see on peamiselt silumine ja katse eesmärgil kasulik.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Telemeetria saatmiseks rakenduse ülevaated Windowsi rakendus

1. [Azure'i portaali](https://portal.azure.com) [loomine on rakenduse ülevaated ressurss](app-insights-create-new-resource.md). Rakenduse tüüp, valida ASP.net-i rakendus.
2. Võtta koopia Instrumentation võti. Äsja loodud uue ressursi Essentialsi ripploendis leida võti. 
3. Visual Studios, rakenduse projekti NuGet-paketid redigeerimine ja lisage Microsoft.ApplicationInsights.WindowsServer. (Või valige Microsoft.ApplicationInsights, kui soovite lihtsalt selle tühjal API ilma standard telemeetria saidikogumi moodulid.)
4. Haldusteenuse võtme seadmiseks ühte koodi:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*tootevõti*`";` 

    või ApplicationInsights.config (kui olete installinud üks standard telemeetria paketid):
 
    `<InstrumentationKey>`*tootevõti*`</InstrumentationKey>` 

    Kui kasutate ApplicationInsights.config, veenduge, et selle atribuute Solution Exploreris on seatud **koostada toimingu Kopeeri väljundi kataloogi sisu = = Kopeeri**.
5. [Kasutage API](app-insights-api-custom-events-metrics.md) telemeetria saata.
6. Rakenduse käivitamine ja leiate Azure'i portaali loodud ressursi telemeetria.

## <a name="telemetry"></a>Näide kood

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Järgmised sammud

* [Armatuurlaua loomine](app-insights-dashboards.md)
* [Diagnostika otsing](app-insights-diagnostic-search.md)
* [Mõõdikute uurimine](app-insights-metrics-explorer.md)
* [Päringute Kasutusanalüüsi kirjutamine](app-insights-analytics.md)
 
