<properties
   pageTitle="Usaldusväärne teenuste WCF-i side virnas | Microsoft Azure'i"
   description="Sisseehitatud WCF-i side virnas teenuse struktuuri hõlmab kliendi teenuse WCF-i side usaldusväärne teenuste jaoks."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-põhise side virnas usaldusväärne teenuste jaoks
Usaldusväärsed teenused raames võimaldab valida side virnas, millele nad soovivad oma teenust kasutada teenuse autorid. Ta saab ühendage side virnas enda valitud **ICommunicationListener** tagastatud [CreateServiceReplicaListeners või CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) meetodite kaudu. Sisaldab põhjal Windows Communication Foundation (WCF) teenuse autorid, kes soovivad kasutada WCF-põhise side side virnas rakendamist.

## <a name="wcf-communication-listener"></a>WCF-i side kuulajale
WCF-i kohased rakendamise **ICommunicationListener** pakub **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** klassi.

Muidu Oletagem, et meil tüüpi teenuse leping`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

WCF-i side kuulajale saame luua teenuses järgmisel viisil.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>WCF-i side virnas klientidele kirjutamine
Kirjutamiseks suhelda teenuseid, kasutades WCF-i kliendid, sisaldab **WcfClientCommunicationFactory**, mis on [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md)WCF-i kohased rakendamine.

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF-i side kanali pääseb juurde **WcfCommunicationClient** loodud **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Kliendi koodi saate kasutada **WcfCommunicationClientFactory** koos **WcfCommunicationClient** , mis rakendab **ServicePartitionClient** määratleda teenuse lõpp-punkti ja teenuse edastada.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Vaikimisi ServicePartitionResolver eeldab, et kliendi töötab sama nimega teenuse kobar. Kui see on ei ole, ServicePartitionResolver objekti loomine ja edastada kobar ühenduse lõpp-punktid.

## <a name="next-steps"></a>Järgmised sammud
* [Kaugprotseduurikutse, usaldusväärseid teenuseid remoting koos](service-fabric-reliable-services-communication-remoting.md)

* [Veebi-API koos OWIN usaldusväärsed teenused](service-fabric-reliable-services-communication-webapi.md)

* [Suhtlus, usaldusväärseid teenuste turvamine](service-fabric-reliable-services-secure-communication.md)
