<properties
   pageTitle="ASP.net-i veebi-API suhtlemine teenuse | Microsoft Azure'i"
   description="Saate teada, kuidas rakendada teenuse side samm-sammult, kasutades ASP.net-i veebi-API OWIN omas hosting, usaldusväärseid teenuste API."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Alustamine: teenuse struktuuri Web API teenustega OWIN ise hosting

Azure teenuse struktuuri asetab power käed kui kaalute, kuidas soovite oma teenuste kasutajatega ja omavahel suhelda. Selle õpetuse keskendub rakendamise teenuse suhtlus abil ASP.net-i veebi-API avatud Web kasutajaliidese .net-i (OWIN) omas hosting teenuse struktuuri usaldusväärne Services API. Meil kuvatakse süveneda sügavalt usaldusväärsed teenused ühendatav side API. Samuti kasutame Veebiteenuste üksikasjalike näites näete, kuidas häälestada kohandatud side kuulajale.


## <a name="introduction-to-web-api-in-service-fabric"></a>Veebi-API sisse teenuse struktuuri tutvustus

ASP.net-i veebi-API on populaarsed ja võimas raamistik koostamise HTTP API-de .NET Frameworki peal. Kui te ei ole juba tuttav raames, vt lisateavet [ASP.net-i Veebiteenuste 2 alustamine](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) .

Veebi-API sisse teenuse struktuuri on sama ASP.net-i Web API teate ja kuulda. Kuidas on teil *host* Veebiteenuste rakenduse. Te ei kasuta Microsoft Internet Information Services (IIS). Paremini mõistmiseks vahe, vaatame see tühistada kaheks:

 1. Rakenduse Web API (sh kontrollerid ja andmemudelid)
 2. Host (veebiserver, tavaliselt IIS-i)

Veebiteenuste rakenduse, ise ei muuda. See ei erine olete kirjutanud varem Veebiteenuste rakendustest ja peaks oskama lihtsalt liikuda suurema osa oma rakenduse koodi. Aga kui te olete hosting IIS, kus host rakendus võib olla pisut erinev, mida olete harjunud. Enne saame majutusteenuse osa, alustame midagi enamat tuttav: Veebiteenuste rakendus.


## <a name="create-the-application"></a>Rakenduse loomine

Alustage ühe kodakondsuseta teenusega Visual Studio 2015 uue teenuse struktuuri taotluse loomine.

![Loo uus rakendus teenuse struktuuri](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Visual Studio malli abil Veebiteenuste kodakondsuseta teenuse on teile saadaval. Selles õpetuses me tuleb koostada Veebiteenuste projekti algusest peale ise luua, mis annab tulemuseks, mida soovite saada kui valisite selle malli.

Valige tühi kodakondsuseta teenuse projekti, saate teada, kuidas koostada Veebiteenuste projekti nullist või saate alustada kodakondsuseta teenuse Web API Mall ja lihtsalt jälgida.  

![Ühe kodakondsuseta teenuse loomine](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Esimese asjana tõmmata mõned Nugeti paketid Veebiteenuste jaoks. Soovime kasutada pakett on Microsoft.AspNet.WebApi.OwinSelfHost. See pakett sisaldab kõik vajalikud veebi-API paketid ja *host* paketid. See on oluline hiljem.

![Veebi-API Nugeti Package Manager abil luua](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Kui paketid on installitud, saate hakata hoone läbi Veebiteenuste projekti põhistruktuur. Kui olete kasutanud Veebiteenuste, projekti struktuur peaks välja nägema väga tuttav. Alustuseks lisage soovitud `Controllers` kataloogi ja lihtsate väärtuste kontrolleril:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Järgmiseks lisada projekti juurkausta registreerida marsruudi, formatters ja konfiguratsiooni seadistamine käivitus ainekursuse. See on ka, kus Web API ühendatakse *host*, mis hiljem uuesti vaadata. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

See on õige rakendus osa. Selles etapis olete häälestanud lihtsalt tavalise Veebiteenuste projekti paigutus. Nii palju, ei peaks otsima palju erinevaid Web API projekte olete kirjutanud varem või põhimalli Web API-ga. Teie äriloogika läheb kontrollerid ja mudelite nagu tavaliselt.

Nüüd mis teeme hosting nii, et saaksime tegelikult käivitada?

## <a name="service-hosting"></a>Teenuse majutusteenuse

Klõpsake teenuse struktuuri, teenust töötab *teenus Majutaja protsess*töötab teie teenuse kood täitmisfail. Kui kirjutate teenuse abil usaldusväärne Services API, koostab teenuse projekti lihtsalt täitmisfail registreerib teie teenuse tüüp ja töötab teie koodi. See kehtib enamasti teenuse kirjutamisel teenuse struktuuri .NET-is. Program.cs avamisel kodakondsuseta teenuse project peaksite nägema:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Kui mis näeb välja kahtlaselt sisenemiskoha konsooli rakendus, mis on, sest see on.

Lisateavet teenuse Majutaja protsess ja teenuse registreerimise ei käsitleta käesoleva artikli. Kuid see on oluline teada nüüd selle *oma protsessi töötab teie teenuse kood*.

## <a name="self-host-web-api-with-an-owin-host"></a>Omas majutada Web API on OWIN host

Et Veebiteenuste rakenduse koodis on majutatud oma protsessi, kuidas teha te pange see veebiserverisse? Sisestage [OWIN](http://owin.org/). OWIN on lihtsalt .net-i veebirakenduste ja web serverite vahelist leping. Traditsiooniliselt ASP.net-i (kuni MVC 5) kasutamisel veebirakenduse on kindlalt haagitud IIS-i kaudu System.Web mitu. Siiski Veebiteenuste rakendab OWIN, kirjutamise veebirakendus, mis on sidumata majutava veebiserver kaudu. Seetõttu saate *ise majutatud* OWIN veebiserverile, mis saate alustada oma käigus. See sobib suurepäraselt me lihtsalt kirjeldatud teenuse struktuuri majutusteenuse mudel.

Selles artiklis kasutame Katana OWIN hostiks Veebiteenuste rakenduse. Katana on avatud lähtekoodi OWIN host rakendamine tugineb [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) ja Windows [HTTP serveri API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [AZURE.NOTE] Lisateavet Katana, minge [Katana saidi](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Kiirülevaate sellest, kuidas kasutada Katana uusimale Veebiteenuste kohta leiate teemast [Kasutada OWIN Self-Host ASP.net-i veebi-API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Veebiserver häälestamine

Usaldusväärne Services API pakub side sisendpunkti, kui ühendate communication virnadena, mis võimaldavad kasutajate ja klientide teenusega ühenduse loomiseks.

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Veebiserver (ja muu suhtlus virnas kasutate edaspidi, nt WebSockets) peaksite kasutama ICommunicationListener kasutajaliideses õigesti integreerida süsteem. Põhjus on selles muutub selgemaks toimige järgmiselt.

Esmalt looge klassi nimetatakse OwinCommunicationListener, mis rakendab ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

ICommunicationListener liides võimaldab hallata side kuulajale teie teenuse jaoks kolm võimalust:

 - *OpenAsync*. Käivitage kuulamine taotlused.
 - *CloseAsync*. Lõpeta kuulamine taotluste, valmis mis tahes lennu taotlusi ja sulgeda nõtkelt.
 - *Kas soovite katkestada*. Kõik tühistamine ja peatamine kohe.

Alustamiseks privaatne klassi liikmed kuulajale tuleb funktsioon asju lisada. Need läbi ehitaja lähtestada ja hiljem kasutada, kui häälestate kuulamise URL-i.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Rakendada OpenAsync

Veebiserver häälestada, peate kahe andmeühikuga teavet.

 - *URL-i tee eesliide*. Kuigi see pole kohustuslik, on hea, saate selle seadistada nüüd nii, et mitme veebiteenuste turvaline majutada oma rakenduse.
 - *Pordi*.

Enne pordi saamiseks veebiserver, on oluline mõista, et teenuse struktuuri pakub rakenduse kiht, mis toimib ka puhvrit rakenduse ja aluseks operatsioonisüsteemi, et see töötab. Sellisel kujul teenuse struktuuri võimaldab konfigureerida oma teenuste *lõpp-punktid* . Teenuse struktuuri tagab lõpp-punktid kasutama teie teenuse jaoks saadaval. Sellisel viisil, pole teil konfigureerida nende enda aluseks OS-i keskkonnas. Saate hõlpsasti majutada teenuse struktuuri rakenduse erinevates keskkondades ilma muudatusi teha rakenduse. (Nt saate majutada sama rakenduse Azure või oma andmekeskuses.)

Konfigureerimiseks lõpp HTTP PackageRoot\ServiceManifest.xml.

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

See toiming on oluline, kuna teenuse Majutaja protsess töötab piiratud identimisteabe (võrgu teenus Windows). See tähendab, et teenust ei pääse häälestamine lõpp HTTP eraldi. Lõpp-punkti konfiguratsiooni abil teenuse struktuuri teab proper pääsuloendi (ACL) häälestamiseks URL, mis kuulavad teenuse kohta. Teenuse struktuuri pakub ka standardse kohta konfigureerimiseks lõpp-punktid.


Olles tagasi kohas OwinCommunicationListener.cs, saate alustada OpenAsync rakendamine. See on, kui hakkate veebiserver. Esmalt lõpp-punkti teave ja looge URL, mis kuulavad teenuse kohta. URL on erinevad olenevalt sellest, kas kuulajale kasutatakse kodakondsuseta teenuse või stateful teenus. Stateful teenuse kuulajale peab looma iga stateful teenuse koopia selle kuulab unikaalne aadress. Kodakondsuseta teenuste, võib olla lihtsam aadress. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Pange tähele, et "http://+" on siin kasutatud. See on veenduge, et veebiserver on kuulamise kõik saadaolevad aadressid, sealhulgas localhost, FQDN ja arvuti IP.

OpenAsync rakendamist on üks kõige olulisemad põhjused, miks veebiserver (või mis tahes side virnas) on rakendatud otse avamisel kuvatakse ICommunicationListener, mitte ainult paluge see `RunAsync()` teenus. Tagastatav väärtus: OpenAsync on aadress, mis on kuulamise veebiserver. Kui see aadress on tagasi süsteemi, registreerib aadressi teenuse. Teenuse struktuuri pakub API, mis võimaldab kliendid ja muud teenused, siis selle aadressi küsida teenuse nimi. See on oluline, kuna teenuse aadress pole staatiline. Teenused on liigutada kobar ressursi tasakaalustamiseks ja kättesaadavus eesmärkidel. See on süsteem, mis võimaldab kuulata aadressi teenuse kliendid.

Seda silmas pidades OpenAsync käivitab veebiserver ja tagastab aadress, et see on kuulamise. Teate, et see kuulab "http://+", kuid enne OpenAsync tagastab aadress on "+" on asendatud funktsiooniga IP- või FQDN sõlme praegu on sisse lülitatud. Aadress, mis tagastatakse see meetod on, mis on registreeritud süsteemi. Samuti on mida kliendid ja muude teenuste näevad, kui nad küsivad teenuse aadressi. Klientidele ühenduse õigesti, tuleb tegelik IP- või FQDN aadress.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Pange tähele, et see viitaks käivitus ainekursust, mis võeti vastu OwinCommunicationListener ehitaja. Käivitus eksemplar on kasutada veebiserver Veebiteenuste rakendus.

Funktsiooni `ServiceEventSource.Current.Message()` rida kuvatakse aknas diagnostika sündmuste hiljem kinnitada, et veebiserver käivitatud rakendus.

## <a name="implement-closeasync-and-abort"></a>Rakendada CloseAsync ja katkestamist

Lõpuks rakendada nii CloseAsync ja katkestamist veebiserver lõpetada. Veebiserver saate peatada kõrvaldamise ajal OpenAsync loodud serveri pidet.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

Selles näites rakendamist CloseAsync nii katkestamist lihtsalt peatada veebiserver. Kui valite teha veel nõtkelt koordineeritud sulgumist veebiserver CloseAsync. Näiteks sulgemist võiks oodata parda taotlusi täidetakse enne tagasi.

## <a name="start-the-web-server"></a>Veebiserver käivitamine

Nüüd oletegi valmis looma ja tagastada OwinCommunicationListener alustamiseks veebiserver eksemplari. Tagasi klassis teenuse (WebService.cs), alistada soovitud `CreateServiceInstanceListeners()` meetod:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

See on kui Veebiteenuste *rakenduse* ja OWIN *host* lõpuks vasta. Host (OwinCommunicationListener) on antud *rakenduse* (veebi-API) kaudu käivitus klassi eksemplari. Teenuse struktuuri haldab seejärel elutsükli. Tavaliselt saab selle sama mustri koos mis tahes side virnas järgida.

## <a name="put-it-all-together"></a>Seda kõike koos

Selle näite puhul ei pea midagi teha, selle `RunAsync()` meetod nii, et selle alistamine lihtsalt saab eemaldada.

Lõplik teenuse rakendamise peaks olema väga lihtne. Ainult loomiseks vaja läheb side kuulajale:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Täieliku `OwinCommunicationListener` klassi:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Nüüd, kui teil on kehtestanud kõik osad, projekti peaks välja nägema tüüpiline Veebiteenuste rakendus usaldusväärne Services API sissepääsud ja mõne OWIN host:


![Veebi-API usaldusväärne Services API sissepääsud ja OWIN host](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Käivitamine ja ühendage veebibrauseri kaudu

Kui te pole seda veel teinud, [häälestada oma arenduskeskkond](service-fabric-get-started.md).


Nüüd saate luua ja juurutada teenust. Vajutage **klahvi F5** , luua ja juurutada rakenduse Visual Studio. Diagnostika sündmuste akna, peaksite nägema teade, mis näitab, et veebiserver avada http://localhost:8281 /.


![Visual Studio diagnostika sündmuste aken](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Kui mõni muu protsess teie arvutis juba avatud pordi, võidakse kuvada tõrge siin. See näitab, et kuulajale ei saa avada. Kui see on nii, proovige kasutada erinevat porti ServiceManifest.xml konfiguratsiooni lõpp-punkti jaoks.


Kui teenus töötab, avage brauser ja liikuge [http://localhost:8281/api/väärtused](http://localhost:8281/api/values) proovimislink.

## <a name="scale-it-out"></a>Selle välja skaala

Skaala läbi kodakondsuseta veebirakenduste tavaliselt tähendab, et lisada rohkem masinad ja valmistamata neid web apps. Teenuse struktuuri korraldamise otsimootori saate seda teie jaoks iga kord, kui lisatakse uus sõlmed klaster. Kui loote kodakondsuseta teenuseid, saate määrata loodava eksemplaride arv. Teenuse struktuuri paigutab selle eksemplaride arv sõlmed klaster. Ja see muudab kindlasti rohkem kui ühe eksemplari loomine mis tahes ühe sõlme. Samuti saate paluge teenuse struktuuri, määrates **-1** eksemplari loendamiseks iga sõlme alati eksemplari loomine. See tagab, et iga kord, kui lisate sõlmed välja klaster mastaapida, eksemplari kodakondsuseta teenust luuakse uus sõlmed. See väärtus on atribuut eksemplari, nii, et see on seatud teenuse eksemplari loomisel. PowerShelli kaudu saate seda teha.

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Seda saate teha ka siis, kui määratlete vaikimisi teenuse Visual Studio kodakondsuseta teenuse projektis:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Lisateavet rakenduste ja teenuste eksemplari loomise kohta leiate teemast [Deploy rakendus](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Järgmised sammud

[Silumine teenuse struktuuri rakenduse Visual Studio abil](service-fabric-debugging-your-application.md)
