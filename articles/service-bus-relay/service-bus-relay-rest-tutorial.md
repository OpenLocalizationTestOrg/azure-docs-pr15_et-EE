<properties
    pageTitle="Teenuse siini ülejäänud kuueosalisest abil edastada, sõnumside | Microsoft Azure'i"
    description="Koostada lihtne teenuse siini relay host rakendus, mis pakub ülejäänud-põhine kasutajaliides."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Teenuse siini Relay ülejäänud õpetus

Selles õppetükis kirjeldatakse, kuidas luua lihtne teenuse siini host rakendus, mis pakub ülejäänud-põhine kasutajaliides. ÜLEJÄÄNUD võimaldab web klient, veebibrauser, nt juurdepääsu teenuse siini API-de kaudu HTTP päringuid.

Selle õpetuse kasutab Windows Communication Foundation (WCF) ülejäänud programmeerimisega mudeli ehitada ülejäänud teenuse teenuse siini. Lisateabe saamiseks vt [WCF-i ülejäänud programmeerimisega mudeli](https://msdn.microsoft.com/library/bb412169.aspx) ja [kujundamisel ja teenuste rakendamise](https://msdn.microsoft.com/library/ms729746.aspx) WCF-i dokumentatsiooni.

## <a name="step-1-create-a-service-namespace"></a>Samm 1: Looge teenuse nimeruum

Esimese asjana nimeruumi loomiseks ja saada ühiskasutusse Accessi allkirja (SAS) võti. Nimeruumi pakub iga rakenduse kaudu teenuse siini esitatud rakenduse soovitud äärist. SAS klahvi luuakse automaatselt süsteemi teenuse nimeruumi loomisel. Teenuse nimeruum ja SAS klahvi kombinatsiooni pakub teenuse siini rakenduse juurdepääsu autentimiseks mandaadi.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Samm 2: Määratle teenusleping ülejäänud vastavalt WCF-i teenuse siini kasutamiseks

Kui muude teenustega teenuse siini ülejäänud laadis teenuse loomisel peate määratlema lepingu. Lepingu saate määrata, milliseid toiminguid selle hosti toetab. Teenuse toimingut saate vaadelda kui veebiteenuse meetodit. Lepingute on loodud C++, C# või Visual Basic kasutajaliidese määratlemine. Iga meetodi liideses vastab kindla teenuse toiming. Atribuut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) tuleb teha iga kasutajaliidese ja [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribuut tuleb teha iga toimingu. Kui meetodi liides, mis on [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) ei ole [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), et meetod on avatud. Nende toimingute jaoks kasutatud on näidatud korras.

Põhilised teenuse siini leping ja ülejäänud laadis lepingu esmane erinevus on atribuut, millele [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx)lisaks: [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Atribuut võimaldab teil oma liides meetodi vastendamine meetodi teisele küljele liides. Sel juhul kasutame [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) HTTP GET meetodi linkida. See võimaldab teenuse siini tõlgendamine saadetud kasutajaliideses käsud ja täpselt alla laadida.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Teenuse siini lepingu liidesega loomiseks

1. Avage administraatorina Visual Studio: Paremklõpsake programmi menüüs **Start** ja seejärel klõpsake käsku **Käivita administraatorina**.

2. Luua uue konsooli rakendus projekti. Klõpsake menüüd **fail** ja valige **Uus**ja valige **projekti**. Dialoogiboksis **Uus projekt** klõpsake **Visual C#**, valige mall **Konsooli rakendus** ja **ImageListener**nime. Kasutage vaikeväärtust **asukoht**. Klõpsake nuppu **OK** , et luua projekt.

3. C# projekti Visual Studio loob mõne `Program.cs` faili. See tund sisaldab tühja `Main()` meetod, mis on vaja konsooli rakendus projekti koostamiseks õigesti.

4. Viited teenuse siini ja **System.ServiceModel.dll** lisada projekti teenuse siini Nugeti paketi installimisel. See pakett automaatselt lisab teenuse siini teekide, samuti WCF-i **System.ServiceModel**viited. Solution Exploreris Paremklõpsake **ImageListener** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**. Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus`. Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega.

5. Projekti peate lisama konkreetselt **System.ServiceModel.Web.dll** viide:

    lisamine. Solution Exploreris **Viited** jaotises projekti kaust kaust, paremklõpsake ja klõpsake **Lisada viide**.

    b. **Viite lisamine** dialoogiboksis vahekaarti **Framework** vasakus servas ja väljale **Otsing** , sisestage **System.ServiceModel.Web**. Märkige ruut **System.ServiceModel.Web** ja seejärel klõpsake nuppu **OK**.

6. Lisage järgmine `using` laused Program.cs faili ülaosas.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) on nimeruumi, mis võimaldab Programmeerimisjuurdepääs põhijooned WCF-i. Teenuse siini kasutab paljude objektide ja atribuutide WCF-i määratlemiseks teenuse lepingud. Kasutate selle nimeruumi teenuse siini relay rakenduste enamikus. Samuti [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) aitab määratleda kanal, mis on objekt, mille kaudu suhtlete teenuse ja kliendi veebibrauseri. Lõpuks [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) sisaldab tüübid, mis võimaldavad teil luua veebipõhised rakendused.

7. Ümber nimetada selle `ImageListener` **Microsoft.ServiceBus.Samples**nimeruumi.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Vahetult pärast avamine looksulg nimeruumi, deklaratsiooni määratlemine uue kasutajaliidese nimega **IImageContract** ja rakendada **ServiceContractAttribute** atribuudi väärtus on kasutajaliideses `http://samples.microsoft.com/ServiceModel/Relay/`. Nimeruumi väärtus erineb nimeruumi, mida saate kasutada kogu teie koodi ulatust. Nimeruumi väärtus kasutatakse selle lepingu kordumatu ja peaks olema versiooniteave. Lisateavet leiate teemast [Teenuse Versioonimine](http://go.microsoft.com/fwlink/?LinkID=180498). Nimeruumi otseselt määrata takistab nimeruum vaikeväärtus on lisatud lepingu nimi.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Sees on `IImageContract` kasutajaliides, deklareerida ühe toimingu meetod on `IImageContract` lepingu seab liideses ja rakendada selle `OperationContractAttribute` atribuut meetod, mida soovite seada teenuse siini lepingu raames.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Atribuut **OperationContract** , lisage **WebGet** väärtus.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Nii võimaldab teenuse siini marsruutimiseks HTTP GET taotluste `GetImage`, ja tõlkimiseks saatja väärtuste `GetImage` on HTTP GETRESPONSE vastusesse. Allpool olevat õpetuse, kasutage veebibrauseri avamiseks seda meetodit ja pildi kuvamiseks brauseris.

11. Vahetult pärast selle `IImageContract` määratlus, deklareerida kanal, mis pärib mõlemad on `IImageContract` ja `IClientChannel` liidesed.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanali on WCF-i objekt, mida teenuse ja kliendi läbida teavet omavahel. Hiljem loote kanali hosti rakenduse. Teenuse siini kasutab selle kanali edasi HTTP GET taotlusi brauseri kaudu oma **GetImage** rakendamist. Teenuse siini kasutab ka kanali **GetImage** tagastatav väärtus ja tõlkida mõne HTTP GETRESPONSE brauseri kliendi jaoks.

12. Menüü **koostamine** klõpsake **Lahenduse luua** oma töö täpsuse siiani kinnitamiseks.

### <a name="example"></a>Näide

Järgmine kood kuvatakse lihtne kasutajaliides, mis määratleb teenuse siini leping.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Samm 3: Rakendada teenusleping ülejäänud vastavalt WCF-i teenuse siini kasutamiseks

ÜLEJÄÄNUD laadis teenuse siini teenuse loomise jaoks on vaja luua esmalt leping, mis on määratletud kasutajaliidese abil. Järgmiseks on kasutajaliideses rakendada. See hõlmab klassi nimega **ImageService** , mis rakendab kasutaja määratletud **IImageContract** kasutajaliidese loomine. Pärast lepingu, siis konfigureerimine kasutajaliideses App.config faili abil. Konfiguratsioonifail sisaldab vajalikku teavet rakenduse, näiteks teenuse nimi, nimi ja lepingu protokoll, mida kasutatakse suhelda teenuse siini tüüp. Nende toimingute jaoks kasutatud on korras näites toodud.

Nagu eelmisi juhiseid, on väga vähe vahe ülejäänud laadis leping ja põhilised teenuse siini leping.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>ÜLEJÄÄNUD laadis teenuse siini lepingu rakendamiseks

1. Saate luua uue nimega **ImageService** vahetult pärast **IImageContract** kasutajaliidese määratluse klassi. Klassi **ImageService** rakendab **IImageContract** kasutajaliidese.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Sarnaselt muude rakenduste kasutajaliidese, saate rakendada määratluse eri faili. Selles õpetuses mõeldud rakendamisel kuvatakse aga sama nimega kasutajaliidese määratluse faili ja `Main()` meetod.

2. Rakendada [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribuut **IImageService** klassi tähistamiseks ainekursuse WCF-i lepingu rakendamist.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Nagu eelnevalt mainitud, on selle nimeruumi pole traditsiooniline nimeruum. Selle asemel on WCF-i arhitektuur, mille abil tuvastatakse lepingu osa. Lisateabe saamiseks teemat [Andmete lepingu nimed](https://msdn.microsoft.com/library/ms731045.aspx) WCF-i dokumentatsiooni.

3. Jpg-vormingus pildi lisamine projekti.  

    See on pilt, mis teenuse vastuvõtmise brauseris kuvatakse. Paremklõpsake oma projekti ja seejärel klõpsake nuppu **Lisa**. Klõpsake **Olemasolevat üksust**. Liikuge sirvides soovitud vastav jpg **Lisada olemasoleva üksuse** dialoogiboksi abil ja seejärel klõpsake nuppu **Lisa**.

    Kui lisate faili, veenduge, et **Kõik failid** on valitud kõrval rippmenüü loendis soovitud **faili nimi:** välja. Selle õpetuse ülejäänud eeldab, et pildi nimi on "image.jpg". Kui teil on mõni muu fail, on teil pildi ümber nimetada või muuta oma koodi kompenseeri.

4. Veenduge, et töötab teenus ei leia pildifaili, **Solution** Exploreris paremklõpsake soovitud pildifail ja klõpsake käsku **Atribuudid**. Paanil **Atribuudid** seatud **Kopeeri väljundi kataloogi** **kopeerimine kui uuemat versiooni**.

5. Viide **System.Drawing.dll** komplekti lisada projekti ja lisada ka järgmisi seotud `using` laused.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. **ImageService** tunni, lisage järgmised võistkond, mis laadib rasterpilt ja koostab saatmiseks Kliendi brauser.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Otse pärast eelmise koodi lisada järgmise meetodi **GetImage** **ImageService** tunni tagastamiseks HTTP sõnum, mis sisaldab pilti.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Selle rakendamist kasutab **MemoryStream** pildi alla laadida ja selle ettevalmistamine streaming brauser. Käivitab voo asukoha nullpunktist, kinnitab voo sisu JPEG ja andmevoogu teave.

8. Klõpsake menüü **koostamine** klõpsake **Lahenduse luua**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Määratleda töötab teenus siini veebiteenuse konfigureerimine

1. **Solution Exploreris**, topeltklõpsake selle avamiseks Visual Studio redaktoris **App.config** .

    **App.config** faili sarnaneb WCF-i konfiguratsioonifail ja sisaldab teenuse nimi, lõpp-punkti (st teenuse siini seab kliendid ja hakkab majutama omavahel suhelda asukoht) ja sidumine (protokolli, mida kasutatakse suhtlemiseks tüüp). Peamine erinevus on selles, et konfigureeritud teenuse lõpp-punkti viitab [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) sidumine, mis ei kuulu .NET Frameworki.

2. Funktsiooni `<system.serviceModel>` XML-element on WCF-i element, mis määratleb ühe või mitme teenuseid. Siin kasutatakse määratleda teenuse nimi ja lõpp-punkti. Allosas on `<system.serviceModel>` elemendi (, kuid endiselt kasutamine `<system.serviceModel>`), lisada mõne `<bindings>` element, mis sisaldab järgmist. See määratleb sidumiste, kasutada rakenduse. Saate määratleda mitme sidumiste, kuid selles õpetuses mõeldud on ainult üks määratlemine.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Selles etapis tuleb määratleb teenuse siini [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) sidumine koos **relayClientAuthenticationType** **puudub**. See säte näitab ei nõua lõpp abil sidumine kliendi mandaati.

3. Pärast selle `<bindings>` element, lisada mõne `<services>` element. Sarnaselt on sidumiste, saate määratleda mitmes teenuses ühe konfiguratsioonifailis. Siiski selles õppetükis saate määrata ainult üks.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Selles etapis tuleb konfigureerib teenus, mis kasutab eelnevalt määratletud vaikimisi **webHttpRelayBinding**. Samuti kasutab vaikimisi **sbTokenProvider**, mis on määratletud järgmise juhise juurde.

4. Pärast selle `<services>` element, luua mõne `<behaviors>` elemendi järgmise sisuga, asendades "SAS_KEY" varem saadud [Azure portaali][] *Ühiskasutusse Accessi allkirja* (SAS) võti.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Veel App.config, klõpsake selle `<appSettings>` element, Asenda kogu ühenduse string väärtus portaali varem saadud ühendusstring. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Klõpsake menüü **koostada** **Lahenduse luua** luua kogu lahenduse.

### <a name="example"></a>Näide

Järgmine kood kuvatakse ülejäänud põhinev teenus, mis töötab teenus siini abil **WebHttpRelayBinding** sidumine lepingu ja teenuse rakendamiseks.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Järgmises näites on kujutatud teenusega seotud App.config faili.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Samm 4: Majutada ülejäänud vastavalt WCF-teenuse kasutamiseks teenuse siini

Selles etapis tuleb kirjeldatakse, kuidas käivitada konsooli rakendus kasutamine teenuse siini veebiteenusest. Selles etapis tuleb kirjutada koodi täieliku loendi on esitatud näide korras.

### <a name="to-create-a-base-address-for-the-service"></a>Baasaadressi teenuse loomine

1. Klõpsake soovitud `Main()` funktsioon deklareerimise, luua muutuja nimeruumi teenuse siini projekti talletamiseks. Veenduge, et asendada `yourNamespace` varem loodud teenuse nimeruumi nimi.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Teenuse siini kasutab teie nimeruumi nimi kordumatu URI loomiseks.

2. Luua mõne `Uri` teenus, mis põhineb nimeruumi baasaadressi eksemplari.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Loomiseks ja konfigureerimiseks teenuse veebi

- Teenuse veebi, aadressiga URI varem loodud selles jaotises luua.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Teenuse host on WCF-i objekt, mille instantiates host rakendus. Selles näites edastab selle hosti loodava (mõne **ImageService**) ja jätke hosti rakenduse aadressi tüüp.

### <a name="to-run-the-web-service-host"></a>Veebi teenuse käivitamiseks

1. Avage teenus.

    ```
    host.Open();
    ```
    Teenuse töötab nüüd.

2. Kuvatakse teade, et teenus töötab ja kuidas teenuse peatada.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Kui olete lõpetanud, sulgege teenus host.

    ```
    host.Close();
    ```

## <a name="example"></a>Näide

Järgmises näites sisaldab õpetuse leping ja eelmisi toiminguid alates ja majutab teenuse konsooli rakendus. Koostada järgmine kood täitmisfaili nimega ImageListener.exe sisse.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Koodi Kompileerimine

Pärast hoone lahenduse, tehke selle rakenduse käivitamiseks järgmist.

1. Vajutage **klahvi F5**või otsige sirvides täitmisfail (ImageListener\bin\Debug\ImageListener.exe) teenuse käivitamiseks. Säilita rakendus töötab, kui see on nõutav järgmise juhise täitmiseks.

2. Kopeerige ja kleepige pilt kuvamiseks brauseris käsuviibal aadress.

3. Kui olete lõpetanud, vajutage sisestusklahvi **Enter** käsuviiba aken sulgeda rakendus.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete varem rakendus kasutab teenuse siini vahendusteenusesse, lugege järgmisi artikleid võrdõigusvõrku sõnumside kohta:

- [Azure'i teenus siini arhitektuuri ülevaade](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Kuidas kasutada teenuse siini Vahendusteenusesse](service-bus-dotnet-how-to-use-relay.md)

[Azure'i portaal]: https://portal.azure.com