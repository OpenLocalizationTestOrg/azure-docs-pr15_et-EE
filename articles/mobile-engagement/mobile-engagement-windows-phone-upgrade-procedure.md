<properties 
    pageTitle="Windows Phone Silverlighti SDK versioonitäienduse toimingute" 
    description="Windows Phone Silverlighti SDK versioonitäienduse toimingute Azure mobiilsideseadmete kaasamine"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlighti SDK versioonitäienduse toimingute

Kui teil juba on integreeritud vanemat versiooni meie SDK rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Peate järgima mitu korda, kui teil on vastamata SDK erinevates versioonides. Näiteks kui migreerite 0.10.1 0.11.0 tuleb esmalt kasutage toimingut ", et 0.10.1 0.9.0" siis korras ", et 0.11.0 0.10.1".

##<a name="from-200-to-330"></a>Et 3.3.0 2.0.0 kaudu

### <a name="test-logs"></a>Logide testimine

Praegu saab konsooli logid toodeti SDK lubatud/keelatud/filtreeritud. See kohandamiseks atribuudi värskendamiseks `EngagementAgent.Instance.TestLogEnabled` üks väärtus, mis on saadaval on `EngagementTestLogLevel` loendamine, näiteks:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Et 2.0.0 1.1.1 kaudu

Järgmine kirjeldatakse, kuidas migreerida mõne SDK integreerimine Capptain teenus Capptain SAS tootja Azure Mobile kaasamine rakendusse. 

> [Azure.IMPORTANT] Capptain ja Mobile kaasamine pole sama teenuste ja allpool toodud juhiseid tõstetakse esile ainult kuidas saate migreerida kliendi rakendus. Migreerimine SDK rakenduse kuvatakse siirata andmete Capptain serverid serveriga Mobile kaasamine

Kui migreerite varasemast versioonist, pöörduge Capptain veebisaidi migreerimine 1.1.1 esmalt siis järgmise toimingu rakendamine

### <a name="nuget-package"></a>Nugeti pakett

**Capptain.WindowsPhone** asendamiseks **MicrosoftAzure.MobileEngagement** Nugeti pakett.

### <a name="applying-mobile-engagement"></a>Rakendades mobiilsideseadmete kaasamine

SDK kasutab termin `Engagement`. Peate värskendama oma projekti selle muudatuse vastavaks.

Peate desinstallima oma praeguse Capptain Nugeti pakett. Võtke arvesse, et kõik muudatused Capptain ressursid kausta, eemaldatakse. Kui soovite need failid ja seejärel kopeerida need.

Pärast seda, installige uus Microsoft Azure'i Engagement Nugeti pakett projekti. Leiate otse [Nugeti](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement)kohta. See toiming asendab kõik ressursid failid, mis kasutavad kaasamine ja lisab uue kaasamine DLL oma projekti viidetes.

Teil on oma projekti viited puhastamine kustutades Capptain DLL viited. Kui te ei tee seda, Capptain versiooni konflikte ja vigu juhtub.

Kui teil on kohandatud Capptain ressursid, kopeerige vana failide sisu ja kleepige need uue kaasamine failid. Pange tähele, et xaml-ja cs tuleb värskendada.

Kui nende juhiste täitmist tuleb ainult asendage vana Capptain viited viidetega uue allikaid.

1. Kõik Capptain nimeruumid tuleb värskendada.

    Enne migreerimise:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Pärast migreerimist:
    
        using Microsoft.Azure.Engagement;

2. Kõik Capptain tunnid, mis sisaldavad "Capptain" peaks sisaldama "Kaasamine".

    Enne migreerimise:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Pärast migreerimist:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml failide Capptain nimeruum ja atribuute muuta.

    Enne migreerimise:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Pärast migreerimist:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Nagu Capptain piltide muud ressursid, Pange tähele, et nad ka on ümber nimetatud "Kaasamine" kasutama.

### <a name="application-id--sdk-key"></a>Rakenduse ID / SDK võti

Kaasamine kasutab ühendusstringi. Teil määrata rakenduste ID-d ja mõne SDK vajutamisega Mobile kaasamine, tuleb ainult määrata ühendusstring. Saate häälestada selle oma EngagementConfiguration faili.

Kaasamine konfiguratsiooni saab määrata oma `Resources\EngagementConfiguration.xml` projekti fail.

Määrake faili redigeerimiseks tehke järgmist.

-   Rakenduse ühendusstringi siltide vahel `<connectionString>` ja `<\connectionString>`.

Kui soovite määrata selle käitusajal, saate helistada enne kaasamine agent lähtestamine järgmisel viisil:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Ühendusstringi rakenduse jaoks kuvatakse Azure'i klassikaline portaal.

### <a name="items-name-change"></a>Üksuste nime muutmine

Kõigi üksuste nimega *capptain* on nimega *allikaid*. Sarnaselt jaoks *Capptain* *allikaid*.

Levinud Capptain üksuste näited:

-   Nüüd nimega EngagementConfiguration CapptainConfiguration
-   Nüüd nimega EngagementAgent CapptainAgent
-   Nüüd nimega EngagementReach CapptainReach
-   Nüüd nimega EngagementHttpConfig CapptainHttpConfig
-   Nüüd nimega GetEngagementPageName GetCapptainPageName

Pange tähele, et ka ümbernimetamine mõjutab alistada meetoditest.



 
