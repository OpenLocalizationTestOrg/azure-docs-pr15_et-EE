<properties
    pageTitle="Kuidas kasutada .net-i teenuse siini relay | Microsoft Azure'i"
    description="Saate teada, kuidas ühendada kaks eri asukohtades olevate rakendused Azure'i teenus siini vahendusteenusesse abil."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Azure'i teenus siini vahendusteenusesse kasutamise kohta

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini vahendusteenusesse. Näidiste C# kirjutada ja laiendid komplekteerimine teenuse siini esitatud Windows Communication Foundation (WCF) API kasutamine. Teenuse siini relay kohta leiate lisateavet teemast [teenuse siini edastada, sõnumside](service-bus-relay-overview.md) ülevaade.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Mis on teenuse siini relay?

[Teenuse siini *relay* ](service-bus-relay-overview.md) teenuse võimaldab teil luua hübriid rakendusi, mis on Azure andmekeskuse nii oma kohapealse enterprise keskkonna käivitamine. Teenuse siini relay hõlbustab see võimaldab teil turvaliselt esitamist Windows Communication Foundation (WCF) teenuseid, mis asuvad ettevõtte ettevõtte võrgustikus avaliku pilveteenusesse, ilma avamata ühenduse tulemüüri või nõudva pealetükkivad muudatustest ettevõtte võrgu taristule.

![Relay mõisted](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Teenuse siini edastamine võimaldab host WCF teenused teie olemasoleva ettevõtte keskkonnas. Seejärel saate delegeerida listening sissetuleva meili jaoks seansid ja need WCF-i teenused töötavad Azure'is teenuse siini teenusesse taotluste. Võimaldab seada nende teenuste Azure'i rakenduse koodi või mobiilsideseadmete töötajate või Suhtevõrgu partneri keskkonnas. Teenuse siini võimaldab turvaliselt juurdepääsu reguleerimine nende teenuste Kohandatud tase. Tõhus ja turvaline võimalus nähtavaks tegemine rakenduse funktsionaalsust ja andmed teie ettevõtte olemasolevaid lahendusi ning kasutada seda pilvest pakub.

Selles artiklis näitab, kuidas luua WCF-i veebiteenus, avatud TCP kanali sidumine, mis rakendab kahe pooled turvalist vestluse abil teenuse siini relay abil.

## <a name="create-a-service-namespace"></a>Teenuse nimeruumi loomine

Azure'i teenus siini relay kasutamine alustamiseks peate esmalt looma nimeruumi. Nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks.

Teenuse nimeruumi loomiseks tehke järgmist.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Teenuse siini Nugeti paketi hankimine

[Teenuse siini Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus) on kõige lihtsam viis teenuse siini API ja kõik teenuse siini sõltuvused rakenduse konfigureerimiseks. Projekti Nugeti paketi installimiseks tehke järgmist.

1.  Solution Exploreris Paremklõpsake **Viited**ja seejärel klõpsake käsku **Halda NuGet-paketid**.
2.  Otsige "Teenuse siini" ja **Microsoft Azure'i teenus siini** üksuse valimine. **Installida** installimise lõpuleviimiseks nuppu ja seejärel sulgege dialoogiboks järgmist.

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Teenuse siini abil saate seada ja tarbimine SOAP veebiteenuse abil TCP

Jätke välise tarbimine olemasoleva WCF-i SOAP veebiteenuse, tuleb teil teha muudatusi teenuse sidumiste ja aadressid. See võib olla vaja konfiguratsiooni-failis tehtud muudatusi, või see võib nõua koodi muudatusi, olenevalt sellest, kuidas olete häälestamine ja konfigureerinud oma WCF-i teenused. Pange tähele, et WCF-i võimaldab teil on mitme võrgu lõpp-punktid üle sama teenust, nii, et samal ajal siini teenuse lõpp-punktid välise juurdepääsu lisamisel saate säilitada olemasoleva sisemise lõpp-punktid.

Selles ülesandes koostada lihtsaid WCF-teenuse ja teenuse siini kuulajale lisada. Selle ülesande eeldab, et mõned tundmine Visual Studio ja seetõttu tutvustavad kõik üksikasjad projekti loomine. Selle asemel kujundab kood.

Täitke need juhised enne keskkonna häälestamiseks järgmist.

1.  Visual Studio, luua kaks projekti, "Klient" ja "Teenus" lahendusse sisaldava konsooli rakendus.
2.  Microsoft Azure'i teenus siini Nugeti paketti lisada nii projektide. See pakett lisab kõik vajalikud komplekti viited oma projekte.

### <a name="how-to-create-the-service"></a>Kuidas luua teenuse

Esmalt Looge teenuse ise. WCF-teenus koosneb vähemalt kolme erinevate osade.

-   Leping, mis kirjeldab vahetatakse milliseid sõnumeid ja millised toimingud on kasutada määratlus.
-   Lepingu rakendamine.
-   Host, mis majutab WCF-teenuse ja seab mitme lõpp-punktid.

Selle jaotise näidetes koodi aadress iga järgmised komponendid.

Lepingu määratleb ühekordne, `AddNumbers`, mis lisab kahe arvu ja tagastab tulemi. Funktsiooni `IProblemSolverChannel` kasutajaliides võimaldab kliendi hõlpsam hallata puhverserveri eluiga. On parimaks peetakse sellise kasutajaliidese loomine. See on hea mõte panna selle lepingu määratluse eraldi faili, nii et saate otsida faili "Klient" ja "Teenus" projektide, kuid samuti saate kopeerida kood nii projektide.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Lepingu kohas, kus on triviaalne rakendamist.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Teenuse host programmiliselt konfigureerimine

Lepingu ja juurutamine kohas, saate nüüd majutada teenus. Hosting toimub hoolitseb haldamise teenuse eksemplare [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) objekti sees ja majutab lõpp-punktid, mida kuulata sõnumeid. Järgmine kood konfigureerib teenuse tavalise kohaliku lõpp-punkti nii illustreerimiseks ilme, kõrvuti, sise- ja lõpp-punktid, siini teenuse lõpp-punkti. Asendage stringi *nimeruum* oma nimeruumi nimi ja *yourKey* häälestamise eelmises etapis saadava SAS võti.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

Näiteks saate luua kaks lõpp-punktid, mis on sama lepingu rakendamise kohta. Üks on kohalik ja ühte prognooside teenuse siini kaudu. Peamised erinevused nende vahel on sidumiste; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) kohaliku vastu ja [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) siini teenuse lõpp-punkti ja aadressid. Kohalik lõpp-punkt on erinevate Port aadress kohalikus võrgus. Siini teenuse lõpp-punkti on koostatud stringi lõpp-punkti aadress `sb`, teie nimeruumi nime ja teed "Solveri." Selle tulemuseks URI `sb://[serviceNamespace].servicebus.windows.net/solver`, tuvastamise teenuse lõpp-punkti nimega teenuse siini TCP endpoint täielikult kvalifitseeritud välise DNS-i nimi. Kui te koodi, asendades kohatäidete sisse selle `Main` funktsioon **rakenduse,** on teil otstarbekas teenus. Kui soovite kuulata ainult teenuse siini teenust, eemaldage deklareerimise kohaliku lõpp-punkti.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Teenuse host App.config faili konfigureerimine

Samuti saate konfigureerida selle hosti App.config-faili abil. Teenuse kood majutusteenuse sel juhul kuvatakse järgmine näide.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Lõpp-punkti määratlused viimine App.config faili. Nugeti pakett on juba lisatud mitmesuguseid määratlused App.config faili, mis on nõutav konfiguratsioon laiendid teenuse siini jaoks. Järgmises näites on täpne vaste eelmise koodi, peaks kuvatama **system.serviceModel** elemendi all. See kood näide eeldab, et oma projekti C# nimeruum nimega **teenus**.
Kohatäidete asendamine oma teenuse siini teenuse nimeruumi ja SAS võti.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Pärast nende muudatuste teenus käivitub nagu enne, kuid kaks reaalajas lõpp-punktid: üks kohalik ja ühe listening pilveteenuses.

### <a name="create-the-client"></a>Kliendi loomine

#### <a name="configure-a-client-programmatically"></a>Kliendi programmiliselt konfigureerimine

Teenuse kasutamine, saate koostada WCF-i kliendi [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objekti abil. Teenuse siini kasutab luba põhinev turvalisus mudeli rakendada, kasutades SAS. Klassi [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) tähistab Turbeloa pakkujat sisseehitatud factory meetodid, mis tagastavad mõned tuntud Turbeloa pakkujad. Järgmises näites kasutatakse [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) meetodit käsitlema omandamise korral SAS luba. Nime ja klahvi on need, mis on saadud portaali eelmises jaotises kirjeldatud.

Esmalt viide või kopeerige soovitud `IProblemSolver` lepingu teenuse kood oma kliendi projekti.

Asendage kood on `Main` meetod kliendi, asendades uuesti kohatäitetekst oma teenuse siini nimeruum ja SAS võti.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Nüüd saate koostada kliendi ja teenust, neid (teenuse käivitamine esimest) ja kliendi teenuse helistab ja prinditakse **9**. Käivitada kliendi ja serveri erinevates arvutites, isegi mitmest võrgu ja teatise ikkagi tööd. Kliendi koodi saab käivitada ka pilveteenuses või kohalikult.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Kliendi App.config faili konfigureerimine

Järgmine kood näitab, kuidas konfigureerida kliendi App.config-faili abil.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Lõpp-punkti määratlused viimine App.config faili. Järgmine näide, mis on sama, mis eespool toodud kood, peaks kuvatama **system.serviceModel** elemendi all. Siia, nagu varem, peate asendama kohatäiteid oma teenuse siini nimeruum ja SAS võti.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete teenuse siini vahendusteenusesse põhitõdesid õppinud, järgige neid linke lisateabe.

- [Teenuse siini edastada, sõnumside ülevaade](service-bus-relay-overview.md)
- [Azure'i teenus siini arhitektuuri ülevaade](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Azure'i näidised][] teenuse siini näidised alla või [teenuse siini näidised ülevaade][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure'i näidised]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Teenuse siini näidised ülevaade]: ../service-bus-messaging/service-bus-samples.md