<properties 
    pageTitle="Rakenduse ülevaated SDK ApplicationInsights.config või .xml konfigureerimine" 
    description="Luba või Keela andmete kogumise moodulid ja jõudluse hinnale ja muude parameetrite lisamine" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Rakenduse ülevaated SDK ApplicationInsights.config või .xml konfigureerimine

Rakenduse ülevaateid .NET SDK koosneb mitmest Nugeti paketid. [Core paketi](http://www.nuget.org/packages/Microsoft.ApplicationInsights) pakub API rakenduse ülevaated telemeetria saatmiseks. [Täiendavad paketid](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) pakkuda telemeetria _moodulid_ ja _initializers_ automaatselt jälgimise telemeetria rakenduse ja konteksti. Konfiguratsioonifail kohandades saate lubamine või keelamine telemeetria moodulid ja initializers ja mõned neist parameetrite määramine.

Otsingukonfiguratsiooni faili nimi `ApplicationInsights.config` või `ApplicationInsights.xml`, sõltuvalt teie rakenduse tüüp. See lisatakse automaatselt projekti kui [installida enamik versioone SDK][start]. See on ka lisatud web appi [Oleku jälgimine IIS-i serveris][redfield], või kui valite kδesolevasse ülevaateid [laiend Azure veebisaidi või VM](app-insights-azure-web-apps.md).

Ei saa määrata [SDK veebilehele]võrdväärse failina[client].

Selles dokumendis kirjeldatakse jaotised kuvatakse konfiguratsiooni faili, kuidas need määrata Tarkvaraarenduskomplektist, komponentide ja NuGet-paketid laadimine need komponendid.

## <a name="telemetry-modules-aspnet"></a>Telemeetria moodulid (ASP.net-i)

Iga telemeetria mooduli kogub mõnda kindlat tüüpi andmete ja kasutab core API saata andmed. Moodulid installitud eri Nugeti pakette, mis lisada ka .config fail nõutavad jooned.

Iga mooduli konfiguratsioonifailis on sõlm. Mooduli keelamiseks sõlme kustutada või selle välja kommentaar.



### <a name="dependency-tracking"></a>Sõltuvus jälgimine

[Sõltuvus jälgimise](app-insights-dependencies.md) kogub telemeetria kõned rakenduse teeb andmebaasid ja välisteenused ja andmebaaside kohta. Kui soovite lubada selle mooduli tööle IIS-i serveris, peate [installima oleku jälgimine][redfield]. Azure'i veebirakenduste ja VMs, [Valige rakenduse ülevaated pikendamise](app-insights-azure-web-apps.md)kasutada.

Samuti saate kirjutada oma sõltuvus jälgimise koodi [TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency)abil.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) Nugeti pakett.

### <a name="performance-collector"></a>Jõudluse koguja

[Kogub süsteemi jõudluse hinnale](app-insights-performance-counters.md) nagu CPU, mälu ja võrgu alla laadida IIS-i installid. Saate määrata mis hinnale kogumiseks, sh jõudluse hinnale olete ise loonud.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) Nugeti pakett.


### <a name="application-insights-diagnostics-telemetry"></a>Rakenduse ülevaateid diagnostika telemeetria

Funktsiooni `DiagnosticsTelemetryModule` kuvatakse tõrketeateid rakenduse ülevaated instrumentation kood ise. Näiteks kui kood ei pääse jõudluse hinnale või mõne `ITelemetryInitializer` põhjustab erandi. Jälita telemeetria jälitatud selles mooduli kuvatakse [Diagnostika otsing][diagnostic]. Diagnostikaandmete saadab dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) Nugeti pakett. Kui installite selle paketi, ApplicationInsights.config fail pole luuakse automaatselt. 

### <a name="developer-mode"></a>Arendaja režiimis

`DeveloperModeWithDebuggerAttachedTelemetryModule`sunnib rakenduse ülevaated `TelemetryChannel` andmete kohe telemeetria ühe üksuse korraga, kui Silur on seotud rakenduse protsessi saata. See vähendab aja vahel, kui rakenduse jälitab telemeetria ja rakenduse ülevaated portaalis kuvamisel. CPU ja võrgu läbilaskevõime põhjustab märgatavat pea kohal.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Rakenduse ülevaateid Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Nugeti pakett

### <a name="web-request-tracking"></a>Veebipäringu jälgimine

Aruannete [vastuse aja ning koodi](app-insights-asp-net.md) HTTP päringuid. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) Nugeti pakett

### <a name="exception-tracking"></a>Erandi jälgimine

`ExceptionTrackingTelemetryModule`jälitab töötlemata erandid oma veebirakenduse. [Tõrked]ja erandid[exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) Nugeti pakett


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-jälgib [märkamatu tööülesande erandid](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-jälgib töötlemata erandid töötaja rollid, Windowsi teenuste ja konsooli rakendusi.
* [Rakenduse ülevaateid Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Nugeti pakett.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Microsoft.ApplicationInsights pakett sisaldab [core API](https://msdn.microsoft.com/library/mt420197.aspx) SDK. Muude telemeetria moodulid kasutada, ja saate ka [seda oma telemeetria määratlemiseks kasutada](app-insights-api-custom-events-metrics.md).

* Pole kirje ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) Nugeti pakett. Kui installite ainult selle Nugeti, luuakse .config faili.

## <a name="telemetry-channel"></a>Telemeetria kanal

Telemeetria kanalit haldab buffering või edastamine telemeetria rakenduse ülevaated teenusega. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`on vaikimisi kanali teenuste jaoks. See puhvrite andmete mälu.
* `Microsoft.ApplicationInsights.PersistenceChannel`on alternatiivne konsooli rakendusi. Selle saate salvestada unflushed andmeid püsivate mäluruumi kui teie rakendus sulgub ja saatke see rakendus uuesti käivitamisel.


## <a name="telemetry-initializers-aspnet"></a>Telemeetria Initializers (ASP.net-i)

Telemeetria initializers seada kontekstis atribuudid, mis saadetakse koos iga telemeetria üksus. 

Saate [kirjutada oma initializers](app-insights-api-filtering-sampling.md#add-properties) konteksti atribuutide seadmine.

Standardse initializers on kõik korras kas veebis või WindowsServer Nugeti paketid:


* `AccountIdTelemetryInitializer`määrab atribuudi AccountId.
* `AuthenticatedUserIdTelemetryInitializer`määrab atribuudi AuthenticatedUserId JavaScripti SDK on määranud.
* `AzureRoleEnvironmentTelemetryInitializer`värskendused on `RoleName` ja `RoleInstance` atribuudid on `Device` kontekstis kõigi telemeetria üksuste ekstraktimist Azure runtime keskkond teabega.
* `BuildInfoConfigComponentVersionTelemetryInitializer`värskendused on `Version` atribuut on `Component` kontekstis kõigi telemeetria üksuste ekstraktimist väärtusega on `BuildInfo.config` faili toodetud MS Build.
* `ClientIpHeaderTelemetryInitializer`värskenduste `Ip` atribuut on `Location` kõik telemeetria üksuste konteksti põhjal on `X-Forwarded-For` taotluse HTTP-päis.
* `DeviceTelemetryInitializer`värskendab järgmised atribuudid on `Device` kontekstis telemeetria kõik üksused.
 - `Type`on määratud "PC"
 - `Id`on seatud arvuti domeeni nimi, kus töötab veebirakenduse.
 - `OemName`ekstraktimist väärtuseks on seatud soovitud `Win32_ComputerSystem.Manufacturer` välja WMI abil.
 - `Model`ekstraktimist väärtuseks on seatud soovitud `Win32_ComputerSystem.Model` välja WMI abil.
 - `NetworkType`ekstraktimist väärtuseks on seatud soovitud `NetworkInterface`.
 - `Language`nimi on seatud soovitud `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`värskendused on `RoleInstance` atribuut on `Device` kontekstis kõigi telemeetria üksuste arvutis, kus töötab veebirakenduse domeeni nimi.
* `OperationNameTelemetryInitializer`värskendused on `Name` atribuut on `RequestTelemetry` ja `Name` atribuut on `Operation` telemeetria kõigi üksuste kontekstipõhine HTTP meetod, kui ka ASP.net-i MVC domeenikontrolleri ja järgida taotluse toimingu nimed.
* `OperationIdTelemetryInitializer`või `OperationCorrelationTelemetryInitializer` värskendused soovitud `Operation.Id` seoses vara kõik telemeetria üksuste jälitatud automaatselt genereeritud taotluse töötlemise ajal `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`värskendused on `Id` atribuut on `Session` kontekstis kõigi telemeetria üksuste ekstraktimist väärtusega on `ai_session` küpsise loodud ApplicationInsights JavaScripti instrumentation koodi, mis töötab kasutaja brauseris. 
* `SyntheticTelemetryInitializer`või `SyntheticUserAgentTelemetryInitializer` värskendused soovitud `User`, `Session` ja `Operation` kontekstides atribuudid telemeetria kõigi üksuste jälitatud käsitlemisel taotluse sünteetiliste allikast, näiteks saadavus testida või otsimootori robot otsida. Vaikimisi ei kuvata [Mõõdikute Exploreri](app-insights-metrics-explorer.md) sünteetiliste telemeetria. 

    Funktsiooni `<Filters>` tuvastamise taotluste atribuutide seadmine.
* `UserAgentTelemetryInitializer`värskendused on `UserAgent` atribuut on `User` kõik telemeetria üksuste konteksti põhjal on `User-Agent` taotluse HTTP-päis.
* `UserTelemetryInitializer`värskendused on `Id` ja `AcquisitionDate` atribuudid `User` kontekstis kõigi telemeetria üksuste ekstraktimist väärtustega on `ai_user` küpsise loodud rakenduse ülevaateid JavaScripti instrumentation koodi, mis töötab kasutaja brauseris.
* `WebTestTelemetryInitializer`määrab kasutaja ID-d, seansi ID-d ja sünteetiliste andmeallika atribuudid HTTP-päringud, mis on pärit [kättesaadavus kontrollib](app-insights-monitor-web-app-availability.md).
Funktsiooni `<Filters>` tuvastamise taotluste atribuutide seadmine.

## <a name="telemetry-processors-aspnet"></a>Telemeetria protsessorite (ASP.net-i)

Telemeetria protsessorite saate muuta iga üksuse telemeetria just enne saatmist SDK portaali ja filtreerida.

Saate [kirjutada oma telemeetria protsessorite](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Kohandatavad valimite telemeetria protsessor (alates 2.0.0-beta3)

See on vaikimisi. Kui teie rakendus saadab palju telemeetria, eemaldatakse see protsessor osa sellest.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parameetri pakub target, mis algoritmi proovib saavutada. Iga eksemplari SDK töötab sõltumatult, kui teie server on kobar mitme masinad, telemeetria mahtu tegeliku korrutatakse vastavalt sellele.

[Lisateavet leiate teemast valimite kohta](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Kindlaksmääratud valimite telemeetria protsessor (alates 2.0.0-beta1)

Olemas on ka standardse [valimite telemeetria protsessor](app-insights-api-filtering-sampling.md#sampling) (alates 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanali parameetreid (Java)

Järgmiste parameetrite mõjutavad kuidas Java SDK tuleks salvestada ja telemeetria andmete kogutud.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Saate talletada salvestusruum – SDK-mälu telemeetria üksuste arv. See arv jõudmisel telemeetria puhvri on tühjendada - st telemeetria üksuste rakenduse ülevaated serverisse saata.

-   Min: 1
-   Max: 1000
-   Vaikimisi: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Määratleb sageduse andmeid, mis on talletatud mälu peaks tühjendada (saadetud rakenduse ülevaated).

-   Min: 1
-   Max: 300
-   Vaikimisi: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Määratleb maksimummaht MB, mis on määratud püsiv hoidla kohalikule kettale. See salvestusruumi kasutatakse püsib telemeetria üksused, mida ei saanud edastada rakenduse ülevaated lõpp-punkti. Salvestusruumi suurus täitmisel hüljatakse telemeetria uued üksused.

-   Min: 1
-   Max: 100
-   Vaikimisi: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

See määratleb, kus kuvatakse teie andmed rakenduse ülevaated ressurss. Tavaliselt loote eraldi ressurssi, koos eraldi klahvi, iga rakenduste.

Kui soovite määrata klahvi dünaamiliselt – näiteks, kui soovite saata tulemused erinevad ressursid – rakenduse argument konfiguratsioonifail võtme ja määrake selle koodi.

Võtme seadmiseks TelemetryClient kõik esinemisjuhud, standard telemeetria moodulid, sh määrata klahvi TelemetryConfiguration.Active. Lähtestamine meetodi, nt ASP.net-i teenuse global.aspx.cs tehke järgmist.

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Kui soovite lihtsalt saata määratud sündmuste muu ressursiga, saate määrata teatud TelemetryClient jaoks võti.

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Lisateavet API][api].

Saada on uue tootenumbri, [saate luua uue ressursi rakenduse ülevaated portaalis][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

