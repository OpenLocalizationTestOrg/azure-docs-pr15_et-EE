<properties 
    pageTitle="Teenuse siini Relay õpetuse | Microsoft Azure'i"
    description="Koostada teenuse siini kliendi rakenduste ja teenuste kohta teenuse siini Relay abil."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Teenuse siini Relay õpetus

Selles õppetükis kirjeldatakse, kuidas luua lihtsa teenuse siini klientrakendusega ja teenuse teenuse siini "relay" võimaluste kasutamine. Vastavate õpetuse, mis kasutab teenuse siini [vahendatud sõnumside](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)leiate teemast [Teenuse siini vahendajaks sõnumside .net-i õpetus](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Töö kaudu selles õpetuses annab teile ülevaate toiminguid, mis on vaja teenuse siini klient ja teenuse rakenduse loomine. WCF-i oma kolleegidega, nagu on teenuse ehitada, mis pakub ühe või mitme lõpp-punktid, mis pakub ühe või mitme teenuse käigus. Teenuse lõpp-punkti aadress teenuse avastamisel, sidumine, mis sisaldab teavet, mis klient ja teenuse lepingu, mis määratleb funktsioonid teenuse oma klientidele saate määrata. WCF-i ja teenuse siini teenuse peamine erinevus on, et pilveteenuses asemel kohalik teie arvutis on avatud lõpp-punkti.

Kui te töötate selle teemaloendis selles õpetuses, on teil töötab teenus ja klient, mida saate kasutada toimingute teenuse. Esimene teema kirjeldatakse, kuidas luua konto. Järgmised sammud kirjeldavad määratlemine teenus, mis kasutab leping, kuidas rakendada teenuse ja konfigureerimise teenuse kood. Ka on kirjeldatud, kuidas majutada ja teenuse käivitamine. Teenus, mis on loodud on ise hostitud ja klient ja teenuse käivitamine samas arvutis. Saate konfigureerida teenuse kood või konfiguratsiooni faili abil.

Lõplik kolm toimingut on kirjeldatud, kuidas luua kliendi rakendus, klientrakendus, konfigureerimine ja luua ja kasutada klient, mida saab hosti seda funktsiooni kasutada.

Kõik selle jaotise teemad Oletame, et kasutate Visual Studio on arenduskeskkond.

## <a name="sign-up-for-an-account"></a>Konto kasutajaks registreerumine

Esimese asjana nimeruumi loomiseks ja saada ühiskasutusse Accessi allkirja (SAS) võti. Nimeruumi pakub iga rakenduse kaudu teenuse siini esitatud rakenduse soovitud äärist. SAS klahvi luuakse automaatselt süsteemi teenuse nimeruumi loomisel. Teenuse nimeruum ja SAS klahvi kombinatsiooni pakub teenuse siini rakenduse juurdepääsu autentimiseks mandaadi.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>WCF-i teenusleping kasutamiseks koos teenuse siini määratlemine

Teenuse lepingu saate määrata, milliseid toiminguid (Web teenuse terminid meetodite või funktsioonide) teenuse toetab. Lepingute on loodud C++, C# või Visual Basic kasutajaliidese määratlemine. Iga meetodi liideses vastab kindla teenuse toiming. Iga kasutajaliidese peab olema rakendatud [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atribuut ja iga toimingu peab olema rakendatud [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribuuti. Kui liides, mis on [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atribuudi meetodi ei ole [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribuut, et meetod on avatud. Koodi nende toimingute jaoks on esitatud näide korras. Lepingute ja teenuste suuremat arutelu, vt [kujundamisel ja teenuste rakendamise](https://msdn.microsoft.com/library/ms729746.aspx) WCF-i dokumentatsiooni.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Teenuse siini lepingu liidesega loomiseks

1. Kui administraator Visual Studio avamiseks paremklõpsake programmi menüüs **Start** ja valige **Käivita administraatorina**.

2. Luua uue konsooli rakendus projekti. Klõpsake menüüd **fail** ja valige **Uus**ja seejärel klõpsake nuppu **projekti**. Dialoogiboksis **Uus projekt** klõpsake **Visual C#** (kui **Visual C#** ei kuvata, otsige jaotises **Muud keeled**). Klõpsake malli **Konsooli rakendus** ja nime **EchoService**. Klõpsake nuppu **OK** , et luua projekt.

    ![][2]

3. Installige teenuse siini Nugeti pakett. See pakett automaatselt lisab teenuse siini teekide, samuti WCF-i **System.ServiceModel**viited. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) on nimeruumi, mis võimaldab teil programmiliselt juurde põhijooned WCF-i. Teenuse siini kasutab paljude objektide ja atribuutide WCF-i määratlemiseks teenuse lepingud.

    Solution Exploreris paremklõpsake lahendus ja seejärel klõpsake nuppu **Halda Nugeti pakettide lahenduse**. Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus`. Veenduge, et projekti nimi on valitud, klõpsake väljal **versioonid** . Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega.

    ![][3]

3. Topeltklõpsake Solution Exploreris Program.cs faili avamiseks redaktoris, kui see pole juba avatud.

4. Lisage järgmine laused ülaosas faili abil:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Selle vaikenimi **EchoService** **Microsoft.ServiceBus.Samples**nimeruumi nime muutmine.

    >[AZURE.IMPORTANT] Selle õpetuse kasutab C# nimeruumi **Microsoft.ServiceBus.Samples**, mis on lepingu hallatavate liiki, mida kasutatakse konfiguratsioonifailis [konfigureerimine WCF-i kliendi](#configure-the-wcf-client) etapis nimeruumi. Saate määrata mis tahes nimeruum soovite kui koostate selle näidis; siiski õpetuse ei tööta, kui siis nimeruumid lepingu muutmist ja teenuse vastavalt sellele, rakenduse konfiguratsioonifailis. Nimeruumi määratud App.config faili peab olema sama mis failides C# määratud nimeruumi.

1. Vahetult pärast selle `Microsoft.ServiceBus.Samples` nimeruumi deklaratsioon, kuid sees nimeruumi, määratlemine uue kasutajaliidese nimega `IEchoContract` ja rakendada selle `ServiceContractAttribute` kasutajaliidese **http://samples.microsoft.com/ServiceModel/Relay/**nimeruum väärtusega atribuut. Nimeruumi väärtus erineb nimeruumi, mida saate kasutada kogu teie koodi ulatust. Selle asemel nimeruum väärtust kasutatakse kordumatu selle lepingu kohta. Nimeruumi otseselt määrata takistab nimeruum vaikeväärtus on lisatud lepingu nimi.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Tavaliselt sisaldab teenuse lepingu nimeruumi nimetamise süsteemi, mis sisaldab versiooniteave. Sh versiooniteabe teenuse lepingu nimeruumi võimaldab teenuste eristamiseks peamistest muudatustest, määratledes teenuse uue lepingu uue nimeruum ja asetades selle uue lõpp-punkti kohta. Sel viisil kliendid edasi kasutada vana lepingu ilma värskendada. Versiooniteabe võib koosneda kuupäeva või järgu number. Lisateavet leiate teemast [Teenuse Versioonimine](http://go.microsoft.com/fwlink/?LinkID=180498). Selleks et selles õpetuses, ei sisalda nimetamise süsteemi teenuse lepingu nimeruumi versiooniteabe.

1. Sees on `IEchoContract` kasutajaliides, deklareerida ühe toimingu meetod on `IEchoContract` lepingu seab liideses ja rakendada selle `OperationContractAttribute` atribuut meetod, mida soovite seada teenuse siini lepingu raames.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Vahetult pärast selle `IEchoContract` määratlus kasutajaliides, deklareerida pärib mõlemad kanali `IEchoContract` ja ka on `IClientChannel` kasutajaliides, nagu järgmisel joonisel:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanali on WCF-i objekt, mida host ja kliendi läbida teavet omavahel. Hiljem kirjutate koodi kanali kaja kaks taotlust teabe suhtes.

1. Klõpsake menüü **koostamine** klõpsake **Lahenduse luua** või vajutage **Klahvikombinatsiooni Ctrl + Shift + B** õigsust töö seni.

### <a name="example"></a>Näide

Järgmine kood kuvatakse lihtne kasutajaliides, mis määratleb teenuse siini leping.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nüüd, kui kasutajaliideses on loodud, saate rakendada kasutajaliides.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>WCF-i lepingu kasutada teenuse siini rakendada

Teenuse siini relay loomise jaoks on vaja luua esmalt leping, mis on määratletud kasutajaliidese abil. Liidese loomise kohta leiate lisateavet teemast eelmist toimingut. Järgmiseks on kasutajaliideses rakendada. See hõlmab loomise klassi nimega `EchoService` mis rakendab kasutaja määratletud `IEchoContract` kasutajaliidese. Pärast seda, kui rakendate kasutajaliides, siis konfigureerimine kasutajaliideses App.config konfiguratsiooni faili abil. Konfiguratsioonifail sisaldab vajalikku teavet rakenduse, näiteks teenuse nimi, nimi ja lepingu protokoll, mida kasutatakse suhelda teenuse siini tüüp. Nende toimingute jaoks kasutatud on korras näites toodud. Üldine arutelu kohta, kuidas rakendada teenusleping, vt [Rakendamise teenuse lepingute](https://msdn.microsoft.com/library/ms733764.aspx) WCF-i dokumentatsiooni.

1. Saate luua uue nimega klassi `EchoService` vahetult pärast määratlus on `IEchoContract` kasutajaliidese. Funktsiooni `EchoService` klassi rakendab selle `IEchoContract` kasutajaliidese. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Sarnaselt muude rakenduste kasutajaliidese, saate rakendada määratluse eri faili. Siiski selles õpetuses mõeldud rakendamist asub sama faili nimega kasutajaliidese määratluse ja `Main` meetod.

1. [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribuudi rakendada soovitud `IEchoContract` kasutajaliidese. Atribuut määrab teenuse nime ja nimeruumi. Pärast seda, on `EchoService` klassi kuvatakse järgmiselt:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Rakendada selle `Echo` meetod, mis on määratletud funktsiooni `IEchoContract` kasutajaliides, klõpsake selle `EchoService` klassi. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. **Koostamiseks**klõpsake **Lahenduse luua** oma töö õigsust.

### <a name="to-define-the-configuration-for-the-service-host"></a>Määratleda konfiguratsiooni teenuse pakkuja

1. Konfiguratsioonifail on väga sarnane WCF-i konfigureerimise faili. See sisaldab teenuse nimi, lõpp-punkti (st teenuse siini seab kliendid ja hakkab majutama omavahel suhelda asukoht) ja sidumine (protokolli, mida kasutatakse suhtlemiseks tüüp). Peamine erinevus on, et see konfigureeritud teenuse lõpp-punkti viitab [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) sidumine, mis ei kuulu .NET Frameworki. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) on üks sidumiste, mis on määratletud teenuse siini.

1. Topeltklõpsake **Solution Exploreris**App.config faili avamiseks Visual Studio redaktoris.

2. Klõpsake soovitud `<appSettings>` element, kohatäidete nimi oma teenuse nimeruumi ja muude võti, et mõne varasema etapis kopeeritud Asenda. 

1. Sees on `<system.serviceModel>` sildid, lisage soovitud `<services>` element. Mitme teenuse siini rakendusi saate määratleda ühe konfiguratsioonifailis. Selle õpetuse määratleb siiski ainult üks.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Sees on `<services>` element, lisada mõne `<service>` elemendi teenuse nime määratlus.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Sees on `<service>` element, määrata asukoha lõpp-punkti lepingu ja sidumine lõpp-punkti tüüp.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Lõpp-punkti määratleb, kus kliendi näeb hosti rakenduse. Hiljem õpetuse kasutab seda toimingut URI, mis täielikult seab host teenuse siini kaudu luua. Köitmine teatab, et kasutame TCP protokoll teenuse siini suhelda.

1. Klõpsake menüü **koostamine** **Lahenduse luua** oma töö täpsuse kinnitamiseks.

### <a name="example"></a>Näide

Järgmine kood kuvatakse lepingu rakendamist.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Järgmine kood kuvatakse põhivorming seotud teenuse host App.config faili.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Hosti ja käivitada lihtsa veebiteenuse registreeruda teenuse siini

Selles etapis tuleb kirjeldatakse, kuidas käivitada lihtsa teenuse siini teenus.

### <a name="to-create-the-service-bus-credentials"></a>Teenuse siini mandaadi loomiseks

1. Klõpsake `Main()`, luua kahte muutujat, kus soovite talletada nimeruumi ja muude võti, mis on lugeda konsooli aknast.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    SAS klahvi kasutatakse hiljem juurdepääsu teenuse siini projekti. Nimeruumi on möödas parameetrina `CreateServiceUri` teenuse URI.

4. [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) objekti kasutamisel deklareerida, et kasutate SAS klahvi tüübiks mandaati. Lisage järgmine kood vahetult pärast viimases etapis lisatud kood.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Baasaadressi teenuse loomine

1. Pärast koodi lisasite viimases etapis loomine on `Uri` teenuse baasaadressi eksemplari. Selle URI määrab teenuse siini värviskeemi, nimeruumi ja teenuse kasutajaliidese tee.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" on lühend teenuse siini värviskeemi ja näitab, et me kasutame TCP protokoll. See on ka eelnevalt märkida konfiguratsioonifailis, kui [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) on määratud sidumine.
    
    Selles õpetuses URI on `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Loomiseks ja konfigureerimiseks teenuse pakkuja

1. Määratud ühenduvuse režiimi `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Ühenduvus režiimi kirjeldab teenuse kasutab suhelda teenuse siini; HTTP või TCP. Vaikesätteid `AutoDetect`, teenuse proovib ühendust luua teenuse siini üle TCP, kui see on saadaval ja HTTP kui TCP pole saadaval. Teate, et see erineb protokoll teenuse määrab klient teatis. Köitmine kasutatud määratakse protokolli. Näiteks saate teenuse [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) sidumine, mis määratleb selle lõpp-punkti (klõpsake teenuse siini esitatud) suhtleb klientide http kaudu. Sama teenuse määrata, et teenus suhtleb teenuse siini TCP **ConnectivityMode.AutoDetect** .

1. Majutusteenus, kasutades URI varem loodud selles jaotises luua.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Teenuse host on WCF-i objekt, mille instantiates teenus. Siin, andke seda tüüpi teenuse loodava (mõne `EchoService` tüüp), ja ka aadress, millele soovite seada teenuse.

1. Lisage Program.cs faili ülaosas viited [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) ja [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Olles tagasi kohas `Main()`, konfigureerimine lõpp-punkti avaliku juurdepääsu lubamiseks.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Selles etapis tuleb saada rakenduse leitav avalikult uurides teenuse siini ATOMI teenuse siini kanali projekti. Kui seate **DiscoveryType** **Privaatne**, klient ikkagi teenuse juurde pääseda. Siiski teenus oleks ei kuvata, kui see otsib teenuse siini nimeruumi. Selle asemel oleks kliendi lõpp-punkti tee eelnevalt teada.

1. Teenuse lõpp-punktid määratletud App.config faili teenuse identimisteabe rakendamiseks tehke järgmist.

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Nagu eelmises etapis, mida oleks deklareeritud mitme teenuseid ja konfiguratsioonifailis lõpp-punktid. Kui oleksite, järgmine kood tuleks läbida konfiguratsioonifail ja otsige iga lõpp-punkti, millele rakendada oma kasutajanimi ja parool. Kuid selles õpetuses konfiguratsioonifail on ainult üks lõpp-punkti.

### <a name="to-open-the-service-host"></a>Teenuse pakkuja avamine

1. Avage teenus.

    ```
    host.Open();
    ```

1. Kasutaja teatavad, et teenus töötab ning selgitatakse, kuidas sulgeda teenus.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Kui olete lõpetanud, sulgege teenus host.

    ```
    host.Close();
    ```

1. Vajutage **Klahvikombinatsiooni Ctrl + Shift + B** projekti koostamiseks.

### <a name="example"></a>Näide

Järgmises näites sisaldab õpetuse leping ja eelmisi toiminguid alates ja majutab teenuse konsooli rakendus. Kompileerida täitmisfaili nimega EchoService.exe arvesse järgmist.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>WCF-i kliendi teenuse lepingu loomine

Järgmiseks on lihtne teenuse siini klientrakendusega loomine ja selle teenuse lepingu otsustate hiljem toimingutes. Teate, et neid juhiseid paljude sarnanevad teenuse loomiseks kasutatud juhiseid: määratlemine leping, redigeerimiseks mõnda App.config faili mandaadiga ühenduse teenuse siini, jne. Nende toimingute jaoks kasutatud on korras näites toodud.

1. Looge uus projekt praeguse Visual Studio lahenduse kliendi jaoks, tehes järgmist:
    1. Solution Exploreris sama lahenduse, mis sisaldab teenuse, paremklõpsake praegune lahendus (mitte projekt) ja klõpsake nuppu **Lisa**. Klõpsake nuppu **Uus projekt**.
    2. Klõpsake dialoogiboksis **Lisamine uue projekti** **Visual C#** (kui **Visual C#** ei kuvata, otsige jaotises **Muud keeled**), valige mall **Konsooli rakendus** ja pange sellele **EchoClient**.
    3. Klõpsake nuppu **OK**.
<br />

1. Topeltklõpsake Solution Exploreris Program.cs faili avamiseks redaktoris Projectis **EchoClient** , kui see pole juba avatud.

1. Nimeruumi nimi selle vaikenime muutmine `EchoClient` et `Microsoft.ServiceBus.Samples`.

1. Installige [teenuse siini Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Solution Exploreris Paremklõpsake **EchoClient** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**. Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus`. Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega.

    ![][3]

1. Lisage soovitud `using` [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) nimeruumi Program.cs faili, mis sisaldab. 

    ```
    using System.ServiceModel;
    ```

1. Lisage teenuse lepingu määratluse nimeruumi, nagu on näidatud järgmises näites. Pange tähele, et see määratlus on identne kasutatud **teenuse** project. Lisage järgmine kood ülaosas olevat `Microsoft.ServiceBus.Samples` nimeruum.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Vajutage **Klahvikombinatsiooni Ctrl + Shift + B** kliendi koostamiseks.

### <a name="example"></a>Näide

Järgmine kood kuvatakse praeguse oleku Program.cs faili EchoClient projekt.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>WCF-i kliendi konfigureerimine

Selles etapis tuleb teil luua App.config faili lihtsa klientrakendusega, mis kasutab teenust varem loodud selles õpetuses mõeldud. Selle App.config faili määratleb lepingu, sidumine ja lõpp-punkti nimi. Nende toimingute jaoks kasutatud on korras näites toodud.

1. Topeltklõpsake Solution Exploreris Projectis **EchoClient** **App.config** faili avamiseks Visual Studio redaktoris.

2. Klõpsake soovitud `<appSettings>` element, kohatäidete nimi oma teenuse nimeruumi ja muude võti, et mõne varasema etapis kopeeritud Asenda.

1. Jooksul system.serviceModel element, lisage soovitud `<client>` element.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    See toiming kinnitab, et määratlemisel WCF-laadi kliendi rakendus.

1. Sees on `client` element, Määratle nimi, lepingu ja sidumine lõpp-punkti tüüp.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Selles etapis tuleb määratleb lõpp-punkti lepingu teenuse ja asjaolu, et klientrakendus kasutab TCP suhelda teenuse siini määratletud nimi. Lõpp-punkti nimi on kasutusel järgmise juhise juurde linkida selle lõpp-punkti konfiguratsiooni teenuse URI.

1. Klõpsake nuppu **fail**ja seejärel **Salvesta kõik**.

## <a name="example"></a>Näide

Järgmine kood kuvatakse App.config faili kaja kliendi jaoks.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>WCF-i kliendi teenuse siini helistamiseks rakendada.

Selles etapis tuleb teil rakendada lihtsa klientrakendusega, mis kasutab teenust varem loodud selles õpetuses. Sarnane teenuse kliendi sooritab paljude juurdepääsu teenuse siini samu toiminguid.

1. Ühenduvus režiimiks.
1. Loob URI, mis otsib hostiteenus.
1. Määratleb turvalisus mandaat.
1. Kehtib identimisteabe ühendus.
1. Avaneb ühendus.
1. Täidab konkreetse rakenduse ülesanded.
1. Suleb ühendus.

Ühte Peamised erinevused siiski, et kliendi rakendus kasutab kanali ühenduse teenuse siin, mis tuleks teenuse kasutab **ServiceHost**kõne. Nende toimingute jaoks kasutatud on korras näites toodud.

### <a name="to-implement-a-client-application"></a>Rakendada kliendi rakendus

1. Seadke ühenduvuse režiimi **Automaatne tuvastamine**. Lisage järgmine kood sees on `Main()` **EchoClient** -meetodit.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Määratleda muutujate ootelepanekuks väärtusi teenuse nimeruumi ja SAS võti on lugeda konsooli.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. URI, mis määratleb selle hosti asukoha teenuse siini projekti loomine

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Mandaadi objekti teenuse nimeruumi lõpp-punkti loomine

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Kanali factory, mis laadib konfiguratsiooni kirjeldatud App.config faili loomine.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanali factory on WCF-i objekti, mis loob kanali kaudu suhtlemine teenuse ja kliendi rakendused.

1. Kehtivad teenuse siini mandaat.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Luua ja avada kanali teenusega.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Kirjutage lihtsa kasutajaliidese ja kaja funktsioonid.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Pange tähele, et kood kasutab kanali objekti eksemplarile proxy teenuse kohta.

1. Kanali Sule ja sulgege soovitud factory.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Selle rakenduse käivitamiseks

1. Vajutage **Klahvikombinatsiooni Ctrl + Shift + B** koostamiseks lahendus. See loob kliendi projekti nii teenuse projekt, et eelmiste juhiste järgi loodud.

2. Enne käivitamist klientrakendus, peate veenduma, teenuserakenduse töötab. Visual Studio Solution Exploreris Paremklõpsake **EchoService** lahendus ja seejärel klõpsake käsku **Atribuudid**.

3. Lahendus dialoogiboksi Atribuudid, klõpsake **Käivitus projekti**seejärel nuppu **käivitus mitmed projektid** . Veenduge, et **EchoService** kuvatakse loendis esimesena. 

4. Määrake **toiming** väljale soovitud **EchoService** ja **EchoClient** projektide **käivitamiseks**.

    ![][5]

5. Klõpsake nuppu **projekti sõltuvused**. Valige väljal **projektide** **EchoClient**. Klõpsake väljal **sõltub** veenduge, et **EchoService** oleks märgitud.

    ![][6]

6. Klõpsake nuppu **OK** , et jätta dialoogiboks **Atribuudid** .

7. Vajutage klahvi **F5** nii projektide käivitamiseks.

8. Nii konsooli windows avamine ja küsimine nimeruumi nimi. Teenuse peab käivitamine esimest nii **EchoService** konsooli aken, sisestage nimeruumi ja vajutage sisestusklahvi **Enter**.

9. Järgmiseks küsitakse SAS tootevõtit. Sisestage SAS klahvi ja vajutage sisestusklahvi ENTER.

    Siin on näide väljund konsooli aknast. Pange tähele, et väärtused esitatud siin on näide ainult.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Otsinguteenuse rakenduse prinditakse konsooli akna aadress see on kuulamine nagu näha Järgnevas näites.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Sisestage aknas **EchoClient** konsooli sisestatud varem teenuserakenduse sama teavet. Järgige eelmisi juhiseid sisestada klientrakenduse sama teenuse nimeruumi ja SAS väärtused.

11. Pärast nende väärtuste sisestamise kliendi avab kanali teenuse ja teil palutakse sisestada teksti nagu näha Järgnevas näites konsooli väljundi.

    `Enter text to echo (or [Enter] to exit):` 

    Sisestage tekst teenuserakenduse saata, ja vajutage sisestusklahvi Enter. See tekst teenuse kaja teenuse kaudu saadetud ja kuvatakse nagu järgmises näites väljundi aknas teenuse konsooli.

    `Echoing: My sample text`

    Klientrakenduse saab tagastatav väärtus on `Echo` toiming, mis lähteteksti ja prinditakse selle konsooli aknas. Järgmises näites väljundi aknas kliendi konsooli.

    `Server echoed: My sample text`

12. Saate jätkata tekstsõnumite saatmine kliendi teenusega sel viisil. Kui olete lõpetanud, lõppu sisestusklahvi Enter klient ja teenuse konsooli Windows mõlemad rakendused.

## <a name="example"></a>Näide

Järgmises näites on kujutatud, kuidas luua kliendi rakendus, kuidas kutsuda toimingute teenuse ja sulgemisest klient, kui kõne toiming on lõpule jõudnud.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Järgmised sammud

Selle õpetuse ilmnes, kuidas luua teenuse siini kliendi rakenduste ja teenuste kohta teenuse siini "relay" võimaluste kasutamine. Sarnaselt õpetuse, mis kasutab teenuse siini [sõnumside](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)leiate teemast [Teenuse siini vahendajaks sõnumside .net-i õpetus](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Lisateavet teenuse siini, leiate järgmistest teemadest.

- [Teenuse siini sõnumside ülevaade](../service-bus-messaging/service-bus-messaging-overview.md)
- [Teenuse siini põhialused](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Teenuse siini ülesehitus](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
