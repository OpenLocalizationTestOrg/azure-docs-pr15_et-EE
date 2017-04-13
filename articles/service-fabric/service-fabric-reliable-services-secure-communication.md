<properties
   pageTitle="Turvaline side teenuste teenuse struktuuri aitab | Microsoft Azure'i"
   description="Ülevaade sellest, kuidas aidata turvaline side, usaldusväärseid teenuseid, mis on Azure teenuse struktuuri kobar töötavad jaoks."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Turvaline side teenuste Azure teenuse struktuuri spikker

Turvalisus on üks kõige olulisemaid aspekte teatise. Usaldusväärsed teenused rakenduse raamistik paar Varemkoostatud side virnadena ja tööriistu, mille abil saate parandada. Selles artiklis rääkida turvalisuse täiustamine, kui kasutate teenuse remoting ja suhtlus virnas Windows Communication Foundation (WCF) tehke järgmist.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Turvalisemaks teenuse teenuse remoting kasutamisel

Me kasutame olemasoleva [näide](service-fabric-reliable-services-communication-remoting.md) , mis selgitab, kuidas häälestada remoting, usaldusväärseid teenuste jaoks. Turvalisuse teenuse, kui kasutate teenuse remoting, tehke järgmist.

1. Luua liides, `IHelloWorldStateful`, mis määratleb meetodid, mis on kaugprotseduurikutse teie teenuse jaoks saadaval. Teenust kasutab `FabricTransportServiceRemotingListener`, mis on esitatud selle `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` nimeruum. See on ka `ICommunicationListener` rakendamist, mis pakub remoting.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Lisage kuulajale sätted ja turvalisus mandaat.

    Veenduge, et sert, mida soovite kasutada oma teenuse side turvalisemaks on installitud sõlme klaster. On kaks võimalust, et saate sisestada kuulajale sätted ja turvalisus mandaat.

    1. Anda need otse teenuse kood:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Neile [config paketi](service-fabric-application-model.md)abil:

        Lisage soovitud `TransportSettings` settings.xml faili jaotis.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        Selles näites on `CreateServiceReplicaListeners` meetod, mis näeb välja umbes järgmine:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Kui lisate mõne `TransportSettings` settings.xml faili ilma mis tahes eesliite jaotis `FabricTransportListenerSettings` laaditakse selles jaotises vaikimisi kõik sätted.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         Selles näites on `CreateServiceReplicaListeners` meetod, mis näeb välja umbes järgmine:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Kui helistate meetodite tagatud teenuse asemel remoting virnas, kasutades funktsiooni `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klassi teenuse puhverserveri luua, kasutada `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Liigu `FabricTransportSettings`, mis sisaldab `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Kui kliendi kood töötab teenus osana, saate laadida `FabricTransportSettings` settings.xml failist. Looge TransportSettings jaotis, mis sarnaneb teenuse kood, nagu on näidatud varem. Kliendi koodi teha järgmisi muudatusi:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Kui kliendi osana teenus ei tööta, saate luua client_name.settings.xml faili samasse asukohta, kus on soovitud client_name.exe. Looge TransportSettings jaotise faili.

    Sarnaselt teenuse, kui lisate mõne `TransportSettings` jaotis ilma kliendi settings.xml/client_name.settings.xml, mis tahes eesliite `FabricTransportSettings` laaditakse selles jaotises vaikimisi kõik sätted.

    Sel juhul varasemates kood on veelgi lihtsustatud:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Turvalisemaks teenuse WCF-põhise side virnas kasutamisel

Me kasutame olemasoleva [näide](service-fabric-reliable-services-communication-wcf.md) , mis selgitab, kuidas häälestada WCF-põhise side virnas usaldusväärne teenuste jaoks. Turvalisuse teenuse, kui kasutate WCF-põhise side virnas, tehke järgmist.

1. Teenuse, tuleb teil turvalisemaks WCF-i side kuulajale (`WcfCommunicationListener`) loodud. Selleks, muuta selle `CreateServiceReplicaListeners` meetod.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Klient, kuvatakse `WcfCommunicationClient` klassi, mis on loodud eelmises [näites](service-fabric-reliable-services-communication-wcf.md) ei muudeta. Teil on vaja alistada, kuid selle `CreateClientAsync` meetod `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Kasutage `SecureWcfCommunicationClientFactory` loomiseks WCF-i teatis client (`WcfCommunicationClient`). Kliendi abil autonoomsest teenuse võimalust.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Järgmised sammud

* [Veebi-API koos OWIN usaldusväärsed teenused](service-fabric-reliable-services-communication-webapi.md)
