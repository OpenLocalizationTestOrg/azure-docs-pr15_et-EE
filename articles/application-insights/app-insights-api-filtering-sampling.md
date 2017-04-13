<properties 
    pageTitle="Filtreerimine ja eeltöötlus rakenduse ülevaateid SDK | Microsoft Azure'i" 
    description="Kirjutage telemeetria protsessorite ja telemeetria Initializers SDK filtreerimiseks või enne telemeetria saadetakse rakenduse ülevaated portaali andmete atribuudid lisada." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtreerimine ja telemeetria rakenduse ülevaateid SDK eeltöötlus

*Rakenduse ülevaated on eelvaade.*

Saate kirjutada ja lisandmoodulite rakenduse ülevaateid SDK kohandada, kuidas telemeetria on jäädvustada ja enne saatmist rakenduse ülevaated teenuse konfigureerimine. 

Need funktsioonid on praegu saadaval ASP.net-i SDK.

* [Proovide](app-insights-sampling.md) vähendab mõjutamata teie statistika telemeetria mahtu. See hoiab koos seotud andmepunktid nii, et need kui diagnoosimise probleem seas saate liikuda. Portaalis on kokku loendab korrutatakse hüvitamine proovide.
* [Filtreerimine telemeetria protsessorid](#filtering) võimaldab valida või muuta telemeetria SDK enne saatmist serveriga. Näiteks võib telemeetria maht vähendada, jättes taotlusi robotid. Kuid filtreerimine on veel tavaline lähenemine liiklust kui valimite vähendamine. See võimaldab teil rohkem kontrolli selle üle, mis on edastatud, kuid peate olema teadlik, et see mõjutab oma statistika – näiteks kui saate filtreerida kõigi õnnestunud taotluste.
* Mis tahes rakenduste, sh telemeetria standard moodulid kaudu saadetud telemeetria [telemeetria Initializers lisada atribuudid](#add-properties) . Näiteks saate lisada arvutatud väärtused; või versioonide numbrid, mille portaalis andmete filtreerimiseks.
* Kohandatud sündmused ja mõõdikute saatmiseks kasutatakse [The SDK API -ga](app-insights-api-custom-events-metrics.md) .


Enne alustamist:

* Installige [Rakendus ülevaateid SDK ASP.net-i v2](app-insights-asp-net.md) rakenduse. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filtreerimine: ITelemetryProcessor

Selle tehnika annab teile, mis on lisatud või telemeetria voo välistatud rohkem otse üle. Saate seda kasutada koos valimite, või eraldi.

Telemeetria filtreerimiseks telemeetria protsessor kirjutamine ja registreerida SDK. Kõik telemeetria läheb läbi oma protsessor ja pukseerige see voo või lisada atribuudid saate valida. See hõlmab telemeetria standard moodulid, nt HTTP taotluse koguja ja sõltuvus koguja kaudu, samuti telemeetria, mida olete kirjutanud ise. Saate, näiteks filtreerida välja telemeetria taotlusi robotid või eduka sõltuvus kõnede kohta. 

> [AZURE.WARNING] Filtreerimine saadetud SDK telemeetria protsessorite abil saate skew statistika, mida näete portaali ja raskendada järgige seostuvad üksused.
> 
> Selle asemel, kaaluge [valimite](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Looge telemeetria protsessor

1. Veenduge, et rakendus ülevaateid SDK projektis versioon 2.0.0 või uuem versioon. Paremklõpsake oma projekti Visual Studio Solution Exploreris ja valige haldamine NuGet-paketid. Nugeti paketi Manager, märkige ruut Microsoft.ApplicationInsights.Web.

1. Filtri loomine rakendada ITelemetryProcessor. See on teise laiendatavus punkt, nt telemeetria mooduli, telemeetria initializer ja telemeetria kanali. 

    Pange tähele, et telemeetria protsessorite ehitada ahelas töötlemine. Kui te väärtustada telemeetria protsessor, te kaotate lingi järgmise protsessor ahelas. Kui protsess meetodil on möödas telemeetria andmepunkti, ei oma töö ja siis helistab järgmise telemeetria protsessor ahelas.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Lisage see ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(See on sama jaotis, kus saate lähtestada filtri valimite.)

Stringi väärtuse mööda .config failist esitada oma tunni avaliku nimega atribuudid. 

> [AZURE.WARNING] Olge ettevaatlik, et tüüp nimi ja mis tahes atribuutide nimesid .config faili kood klassi ja atribuudi nimed vastavad. Kui .config faili viitab atribuudi või puudub tüüp, ei pruugi SDK vaikselt mis tahes telemeetria saata.

 
**Teise võimalusena** saate saate filtri-koodi lähtestada. Sobiva lähtestamine tunni – näiteks AppStart Global.asax.cs - lisada ahelas Protsessor:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients, mis on loodud pärast seda kasutatakse teie protsessorite.

### <a name="example-filters"></a>Näide filtrid

#### <a name="synthetic-requests"></a>Sünteetilisi taotlused

Filtreerida web-ja eest. Kuigi mõõdikute Explorer annab teile võimalus filtreerida sünteetiliste allikad, vähendab see suvand Filtreeri neid veebisaidil SDK liikluse.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Nurjunud autentimine

Filtreerida taotlused "401" vastusega. 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Kiire remote sõltuvus kõned välja filtreerimine

Kui soovite diagnoosimine kõnesid, mis on aeglane, filtreerida kiiresti need. 

> [AZURE.NOTE] See on viltune statistika kuvatakse portaalis. Sõltuvus diagrammi otsib, kui sõltuvus kõned on kõik ebaõnnestumist.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Sõltuvus probleemid diagnoosimine

[Selles blogis](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) kirjeldatakse projekti diagnoosida sõltuvus probleemid, saates automaatselt tavaline ping sõltuvused.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Lisage atribuudid: ITelemetryInitializer

Telemeetria initializers abil saate määratleda globaalne atribuudid, mis saadetakse koos kõigi telemeetria; ja alistada valitud standard telemeetria moodulid toimimist. 

Näiteks rakenduse ülevaated Web paketi kogub telemeetria HTTP päringuid kohta. Vaikimisi lipud nagu mis tahes taotluse vastuse koodi nurjus > = 400. Kuid kui soovite käsitleda õnnestumise 400, saate sisestada telemeetria initializer, mis määrab atribuudi edu.

Kui annate telemeetria initializer, seda nimetatakse iga kord, kui nimetatakse Track*() viise. See hõlmab võimalust nimega standard telemeetria moodulid. Mess, mida need moodulid vara, mis on juba määratud mõne initializer pole määratud. 

**Teie initializer määratlemine**

*C#:*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Laadige oma initializer**

Klõpsake ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Teise võimalusena* saate lähtestada initializer koodi, näiteks Global.aspx.cs:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Lisateave selle proovi.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScripti telemeetria initializers

*JavaScripti*

Kohe pärast lähtestamine kood, mis teil portaalist telemeetria initializer lisamiseks tehke järgmist. 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Saadaval on telemetryItem-kohandatud atribuutide ülevaate leiate teemast [andmemudeli](app-insights-export-data-model.md#lttelemetrytypegt).

Saate lisada nii palju initializers, kui soovite. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor ja ITelemetryInitializer

Mis on telemeetria protsessorite ja telemeetria initializers vaheline erinevus?

* On mõned kattumise rakenduses, mida saate teha neid: nii saab kasutada telemeetria atribuutide lisamiseks.
* Alati käivitada enne TelemetryProcessors TelemetryInitializers.
* TelemetryProcessors võimaldavad täielikult asendada, või telemeetria üksuse hülgamiseks.
* TelemetryProcessors ei töödelda jõudluse counter telemeetria.



## <a name="persistence-channel"></a>Kanali püsimine 

Kui teie rakendus töötab, kui Interneti-ühendus ei ole alati saadaval või aeglane, kaaluge püsimine kanali vaike-mälu kanalit asemel. 

Vaike-mälu kanali lähevad kaotsi kõik telemeetria, mis pole saadetud rakendus suletakse ajaks. Kuigi saate `Flush()` proovib saada kõik andmed jäävad puhvri endiselt kaotamisel andmete kui Interneti-ühendust pole, või kui rakenduse sulgub enne edastamine on lõpule viidud.

Seevastu püsimine kanali puhvrite telemeetria faili enne saatmist portaali. `Flush()`tagab, et andmed on salvestatud fail. Kui mis tahes andmeid ei saadeta rakendus suletakse ajaks, jääb faili. Rakenduse taaskäivitamisel andmed saadetakse siis, kui seal on Interneti-ühendus. Andmete liidetakse faili nii kaua, kui see on vajalik, kuni ühendus on saadaval. 

### <a name="to-use-the-persistence-channel"></a>Püsimine kanalit kasutada

1. Saate importida Nugeti pakett [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Järgmine kood kaasamiseks rakenduse sobiv käivitamise kohta.
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Kasutage `telemetryClient.Flush()` enne rakenduse sulgub, veendumaks, et andmed on saadetud portaali või salvestatud fail.

    Pange tähele, et Flush() on sünkroonse püsimine kanali, kuid asünkroonne muude kanalite.

 
Kanali püsimine on optimeeritud seadmete stsenaariumid, kus toodetud rakenduse sündmuste arv on üsna väike ja ühendus on sageli usaldusväärsed. Selle kanali on kirjutada sündmuste kettale usaldusväärne salvestusruumi esmalt ja seejärel proovige seda saata. 

#### <a name="example"></a>Näide

Oletame, et soovite jälgida töötlemata erandid. Olete tellinud selle `UnhandledException` sündmus. Tagasihelistamise, võite lisada kõne tühjendamise veenduge, et telemeetria on jätkunud.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Kui rakendus sulgub, kuvatakse faili `%LocalAppData%\Microsoft\ApplicationInsights\`, mis sisaldab tihendatud sündmused. 
 
Järgmine kord, kui käivitate selle rakenduse kanali kättesaamine selle faili ja esitamisel telemeetria rakenduse ülevaated, kui see on võimalik.

#### <a name="test-example"></a>Testi näide

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Kanali püsimine kood on [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Viide dokumendid

* [API ülevaade](app-insights-api-custom-events-metrics.md)

* [ASP.net-i viide](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK kood

* [ASP.net-i Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET-I 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScripti SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Järgmised sammud


* [Otsi sündmuste ja logid][diagnostic]
* [Valimite](app-insights-sampling.md)
* [Tõrkeotsing][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
