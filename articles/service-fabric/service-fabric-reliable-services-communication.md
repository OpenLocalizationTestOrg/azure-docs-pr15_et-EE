<properties
   pageTitle="Usaldusväärne teenuste side ülevaade | Microsoft Azure'i"
   description="Usaldusväärsed teenused side mudeli, sh avamine kuulajatele Services, lahendamine lõpp-punktid ja teenuste vaheliste suhtlemine ülevaade."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Usaldusväärsed teenused side API-de kasutamine

Azure'i teenus platvormi on täielikult agnostik suhtlemine teenuste kohta. Kõik protokollid ja virnadena on aktsepteeritav, UDP http kaudu. See on teenuse arendaja valida, kuidas teenuste peaks suhelda. Usaldusväärsed teenused rakenduse raamistik sisseehitatud side virnadena kui ka API-d, mille abil saate luua oma kohandatud side komponendid. 

## <a name="set-up-service-communication"></a>Teenuse suhtlemise häälestamine

Usaldusväärne Services API kasutab lihtsa kasutajaliidese teenuse suhtlus. Teenuse lõpp-punkt avamiseks lihtsalt rakendada selle kasutajaliidese:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Seejärel saate lisada oma side kuulajale rakendamist saates selle teenuse vastavalt klassi meetodi alistada.

Kodakondsuseta teenuste:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Stateful teenuste:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Mõlemal juhul naasete kuulajatele kogum. See võimaldab kuulata mitme lõpp-punktid potentsiaalselt kasutades erinevaid protokolle, mitme kuulajatele teenust. Näiteks võib olla mõni HTTP kuulajale ja eraldi WebSocket kuulajale. Iga kuulajale saab nimi ja tulemuseks kogum *nimi: aadress* paari on esitatud JSON objektina, kui klient taotleb teenuse eksemplari või sektsiooni kuulamise aadressid.

Kodakondsuseta teenusega on alistamine tagastab ServiceInstanceListeners kogum. Mõne ServiceInstanceListener sisaldab funktsiooni loomiseks on ICommunicationListener ja annab selle nime. Stateful teenuste, tagastab selle alistamine ServiceReplicaListeners kogum. See on veidi erineda selle kodakondsuseta töölauafunktsioonid kuna on ServiceReplicaListener on võimalus avada mõne ICommunicationListener kohta teisene koopiad. Mitte ainult saate kasutada mitut side kuulajatele mõnes muus teenuses, kuid samuti saate määrata, millised kuulajatele aktsepteerige klõpsake teisene koopiad ja millised kuulata ainult esmane koopiad.

Näiteks saate määrata ServiceRemotingListener, mis võtab RPC kõnede ainult esmane koopiad ja teiseks kohandatud kuulajale, mis viib loetuks taotleb klõpsake teisene koopiad http kaudu.

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Mitme kuulajatele teenuse loomisel **tuleb** iga kuulajale anda kordumatu nimi.

Lõpuks kirjeldada vajalike soovitud [näidata teenuse](service-fabric-application-model.md) lõpp-punktid jaotises teenuse lõpp-punktid.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Suhtlus kuulajale pääsete juurde selle kaudu eraldatud lõpp-punkti on `CodePackageActivationContext` klõpsake soovitud `ServiceContext`. Kuulajale saad listening taotluste, kui see on avatud.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Lõpp-punkti ressursid on levinud kogu teenuste paketti ja need on eraldatud teenuse struktuuri teenuse paketi aktiveerimisel. Mitme teenuse koopiad majutatud sama ServiceHost jagada sama port. See tähendab, et suhtlus kuulajale toetama pordi ühiskasutus. Soovitatav viis selleks on side kuulajale sektsiooni ID ja koopia/eksemplari kasutada, kui see loob kuulata aadress.

### <a name="service-address-registration"></a>Teenuse aadressi registreerimine

Süsteemi teenust nimega *Nimeteenus* töötab teenuse struktuuri kogumite. Nime andmise teenus on mõne registraatori teenuste ja nende aadressid, mida iga eksemplari või koopia teenus on kuulamise. Kui soovitud `OpenAsync` meetod on `ICommunicationListener` on lõpule jõudnud, selle tagasi väärtus saab registreeritud teenuses nimetamine. See tagastatav väärtus, mis saab avaldatud nime panemise teenus on väärtusega võib olla midagi kõigil string. Stringi väärtuseks kliendid näevad nad küsida aadressi teenuse nime andmise teenuse.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Teenuse struktuuri annab API-d, mis võimaldab kliendid ja muud teenused, siis selle aadressi küsida teenuse nimi. See on oluline, kuna teenuse aadress pole staatiline. Teenused on liigutada kobar ressursi tasakaalustamiseks ja kättesaadavus eesmärkidel. See on süsteem, mis võimaldab kuulata aadressi teenuse kliendid.

> [AZURE.NOTE] Täieliku walk-through, kuidas kirjutada jaoks on `ICommunicationListener`, lugege teemat [teenuse struktuuri Web API teenustega OWIN ise hosting](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Teenuse suhtlemine
Usaldusväärne Services API pakub järgmised teekide kirjutamiseks klientides teenuste suhelda.

### <a name="service-endpoint-resolution"></a>Teenuse lõpp-punkti eraldusvõime
Esimene samm suhtlemine teenus on sektsiooni või eksemplari soovite rääkida teenuse lõpp-punkti aadressi lahendamiseks. Funktsiooni `ServicePartitionResolver` kasuliku klassi on lihtne primitiivne, mis aitab määratleda käitusajal teenuse lõpp-punkti kliendid. Teenuse struktuuri terminoloogia, teenuse lõpp-punkti määramine on osutatud *teenuse lõpp-punkti eraldusvõime*.

Teenuste lisamine kobar ühenduse `ServicePartitionResolver` saab luua vaikesätted. See on soovitatav kasutus enamasti:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Ühenduse loomiseks muu kobar, teenused on `ServicePartitionResolver` saab luua kogumi kobar lüüsi lõpp-punktid. Pange tähele, et lüüsi lõpp-punktid on lihtsalt erinevad lõpp-punktid ühenduse sama kobar. Näiteks:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Teise võimalusena `ServicePartitionResolver` võib olla antud funktsiooni loomiseks on `FabricClient` ettevõttesiseselt kasutada: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`on objekti, mis on teenuse struktuuri kobar mitmesuguste toimingute klaster suhelda. See on kasulik, kui soovite rohkem kontrolli selle üle, kuidas `ServicePartitionResolver` klaster suhtleb. `FabricClient`sooritab ettevõttesiseselt vahemällu talletamine ja on üldiselt kallis loomiseks, seetõttu on oluline kasutada `FabricClient` võimalikult palju eksemplarid. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Lahenda meetodi kasutatakse aadressi või teenuse sektsiooni sektsioonitud teenuste jaoks.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Teenuse aadress saab hõlpsasti lahendada on `ServicePartitionResolver`, kuid rohkem tööd on vaja lahendatud aadressi saab kasutada õigesti. Oma kliendi tuleb tuvastada, kas ühenduse loomise katse nurjus siirdamiseks tõrke tõttu ja saate uuesti proovida (nt teenuse teisaldatud või pole ajutiselt saadaval) või püsiv tõrge (nt teenus on kustutatud või selle ressursi pole enam). Teenuse eksemplari või koopiad saate liikuda sõlm sõlm igal ajal mitu põhjust. Teenuse aadress, lahendada `ServicePartitionResolver` võib olla aegunud aeg oma kliendi kood proovib ühendust luua. Sel juhul uuesti kliendi tuleb uuesti aadressi lahendamiseks. Esitada eelmise `ResolvedServicePartition` näitab, et selle lahendaja peab proovige uuesti asemel lihtsalt too vahemällu talletatud aadressi.

Tavaliselt kliendi koodi vaja ei tööta soovitud `ServicePartitionResolver` otse. See on loodud ja suhtlus kliendi tehased usaldusväärne teenuste API edasi. Tehastes abil soovitud lahendaja ettevõttesiseselt kliendi objekti, kasutatavate teenuste suhelda.

### <a name="communication-clients-and-factories"></a>Suhtlus kliendid ja tehased

Suhtlus factory teegi rakendab tüüpilised viga käsitlemine uuesti muster, mis lihtsustab proovin uuesti ühenduste lahendatud teenuse lõpp-punktid. Teegi factory pakub uuesti süsteemi esitate käitleja tõrge.

`ICommunicationClientFactory`määratleb rakendatud side kliendi factory, mis toodab kliendid, mida saate rääkida teenuse struktuuri teenuse base kasutajaliides. Funktsiooni CommunicationClientFactory rakendamine sõltub side virnas kasutavad teenuse struktuuri teenuse, kus klient soovib suhelda. Usaldusväärne Services API annab vastuseks `CommunicationClientFactoryBase<TCommunicationClient>`. See pakub base rakendamine soovitud `ICommunicationClientFactory` kasutajaliides ja kogu suhtlus virnadena ühised sooritab. (Nende ülesannete hulka abil soovitud `ServicePartitionResolver` määratlemiseks teenuse lõpp-punkti). Kliendid tavaliselt rakendada abstraktsed CommunicationClientFactoryBase klassi käsitlema loogika, mis on seotud side virnas.

Teatis client lihtsalt saab aadressi ja kasutab seda teenusega ühenduse loomiseks. Klient võib kasutada mis tahes protokolli seda soovib.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Kliendi factory eelkõige vastutab side kliendid loomise kohta. Kliendid, kes ei säilita pidev ühendus, nt HTTP kliendi, peab funktsiooni factory ainult loomine ja kliendi naasmiseks. Muud protokoll säilitada pidev ühendus, nt mõned kahendarvu protokollid tuleks kinnitada ka factory määrata kas peab uuesti loodud ühendus.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Lõpuks on mõni erand sündmuseohjuri vastutavate erandi kaasnev toimingu määramise. Erandid on jaotatud **retriable** ja **mitte retriable**. 

 - **Mitte retriable** erandid lihtsalt uuesti visata naasmiseks helistaja. 
 - **Retriable** erandid põhjalikumaks liigitada **siirdamiseks** ja **- mööduv**.
  - **Siirdamiseks** erandid on need, mis lihtsalt proovitakse ilma uuesti kõrvaldamise teenuse lõpp-punkti aadress. Nendeks on ajutine võrguprobleemide või teenuse tõrge vastuste muudel, mis näitavad teenuse lõpp-punkti aadress pole olemas. 
  - **Mitte-mööduv** erandid on need, mis nõuavad teenuse lõpp-punkti aadress uuesti lahendada. Erineva sõlme teenus, mis näitab, on nendeks erandid, mis näitavad teenuse lõpp-punkti ei saa ühendust, teisaldada. 

Funktsiooni `TryHandleException` otsustab antud erandi kohta. Kui see **ei tea,** milline otsuseid teha erandi kohta, see peaks tagastama **false**. Kui see **teada,** mis otsus teha, peaks seada vastavalt tulem ja tagastab **väärtuse true**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Pannes kõik kokku
Koos on `ICommunicationClient`, `ICommunicationClientFactory`, ja `IExceptionHandler` ehitatud protokoll, on `ServicePartitionClient` on murtakse see kõik koos ja sisaldab viga käsitlemine ja teenus partition aadress eraldusvõime tsükkel ümber järgmised komponendid.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Järgmised sammud
 - Näide HTTP suhtlemine teenuste [valimi projekti github](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount)kuvamine

 - [Kaugprotseduurikutse kõned usaldusväärsed teenused remoting abil](service-fabric-reliable-services-communication-remoting.md)

 - [Veebi-API, mis kasutab OWIN usaldusväärsed teenused](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-i suhtlus, usaldusväärseid teenuseid kasutades](service-fabric-reliable-services-communication-wcf.md)
