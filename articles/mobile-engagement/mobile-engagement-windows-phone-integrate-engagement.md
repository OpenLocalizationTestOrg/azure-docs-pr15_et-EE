<properties 
    pageTitle="Windows Phone Silverlighti kaasamine SDK integreerimine" 
    description="Kuidas Windows Phone Silverlighti rakendused integreerimine Azure mobiilsideseadmete kaasamine"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlighti kaasamine SDK integreerimine

> [AZURE.SELECTOR] 
- [Windowsi universaalne](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS-i](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Siin kirjeldatakse lihtsaim viis Azure Mobile kaasamine analüüsi- ja jälgida oma Windows Phone Silverlighti rakenduse funktsioonide aktiveerimine.

Järgmised toimingud on piisavalt arvutada statistikat kõik kasutajad, seansid, tegevused, jookseb ja Technicals vaja logid aruande aktiveerimiseks. Aruande arvutada teiste statistikute reguleerimist sündmusi, tõrgete ja tööd, nagu on vaja logid tuleb teha käsitsi kaasamine API abil (vt [Täpsemalt Mobile kaasamine sildistamine API Windows Phone Silverlighti rakenduse kasutamise kohta](mobile-engagement-windows-phone-use-engagement-api.md) ) Kuna statistika on rakenduse sõltuv.

##<a name="supported-versions"></a>Toetatud versioonid

Mobile kaasamine SDK jaoks Windows Silverlighti saab integreerida ainult rakenduste suunamise:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Kui olete suunatud Windows Phone 8.1 (mitte Silverlight) seejärel viidata [Windows universaalne integreerimine toimingut](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Mobiilse kaasamine Silverlighti SDK installimine

Mobile kaasamine SDK jaoks Windows Silverlighti on saadaval Nugeti paketina, mida nimetatakse *MicrosoftAzure.MobileEngagement*. Saate installida selle kaudu Visual Studio Nugeti Package Manager. 

##<a name="add-the-capabilities"></a>Võimaluste lisamine

Kaasamine SDK peab mõned funktsioonid Windows Phone Silverlighti SDK õigesti töötamiseks.

Avage oma `WMAppManifest.xml` fail ja veenduge, et järgmisi võimalusi on deklareeritud on `Capabilities` paneel:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Kaasamine SDK lähtestamine

### <a name="engagement-configuration"></a>Kaasamine konfigureerimine

Kaasamine konfigureerimine on tsentraliseeritud soovitud `Resources\EngagementConfiguration.xml` projekti fail.

Määrake faili redigeerimiseks tehke järgmist.

-   Rakenduse ühendusstringi siltide vahel `<connectionString>` ja `<\connectionString>`.

Kui soovite määrata selle käitusajal, saate helistada enne kaasamine agent lähtestamine järgmisel viisil:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Ühendusstringi rakenduse jaoks kuvatakse Azure'i klassikaline portaal.

### <a name="engagement-initialization"></a>Kaasamine lähtestamine

Kui loote uue projekti, on `App.xaml.cs` fail on loodud. See tund pärib `Application` ja see sisaldab palju olulised viise. Seda kasutatakse kaasamine SDK lähtestada.

Muutke soovitud `App.xaml.cs`:

-   Lisada oma `using` laused:

        using Microsoft.Azure.Engagement;

-   Lisa `EngagementAgent.Instance.Init` sisse selle `Application_Launching` meetod:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Lisa `EngagementAgent.Instance.OnActivated` sisse selle `Application_Activated` meetod:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Soovitame tungivalt takistada saate lisada teise kohta rakenduse kaasamine lähtestamine. Arvestage, et selle `EngagementAgent.Instance.Init` meetod töötab asjakohast teemat, mitte UI lõime.

##<a name="basic-reporting"></a>Tavaline teatamine

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Soovitatav meetod: ülekoormuse oma `PhoneApplicationPage` tunnid

Selleks, et aktiveerida kõik logid nõutud kaasamine arvutada kasutajaid, seansid, tegevused, jookseb ja tehniline statistika aruande, vaid saate kõik teie `PhoneApplicationPage` pärivad alamtäpi klassid on `EngagementPage` tunnid.

Siin on näide sellest, kuidas seda teha rakenduse lehe. Mida saab teha sama rakenduse kõik leheküljed.

#### <a name="c-source-file"></a>C# lähtefail

Muutke oma lehe `.xaml.cs` faili:

-   Lisada oma `using` laused:

        using Microsoft.Azure.Engagement;

-   Asendage `PhoneApplicationPage` koos `EngagementPage` :

**Ilma kaasamine:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Kus lingid:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Kui teie lehe pärib selle `OnNavigatedTo` meetod, olge ettevaatlik, et lasta selle `base.OnNavigatedTo(e)` kõne. Vastasel korral ei saa esitada tegevuse. Tõepoolest on `EngagementPage` helistab `StartActivity` sees on `OnNavigatedTo` meetod.

#### <a name="xaml-file"></a>XAML-i fail

Muutke oma lehe `.xaml` faili:

-   Lisage oma nimeruumid deklaratsiooni.

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Asendage `phone:PhoneApplicationPage` koos `engagement:EngagementPage` :

**Ilma kaasamine:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Kus lingid:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Alistada vaikekäitumise

Vaikimisi on selle tunni nime lehe staatusega tegevuse nime, mille ilma täiendava. Kui ainekursuse kasutab järelliide "Leht", kaasamine ka eemaldada see.

Kui soovite alistada vaikekäitumise nimi, lihtsalt lisage see oma koodi.

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Kui soovite teatada täiendavat teavet oma tegevust, saate lisada järgmine kood:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Nende meetodite nimetatakse rakenduses selle `OnNavigatedTo` lehe meetod.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatiivne meetod: kõne `StartActivity()` käsitsi

Kui te ei saa või ei soovi ülekoormuse oma `PhoneApplicationPage` tunnid, selle asemel saate alustada oma tegevuste helistades `EngagementAgent` meetodite otse.

Soovitame helistaja `StartActivity` sees oma `OnNavigatedTo` oma PhoneApplicationPage meetodit.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Veenduge, et teie seansi lõpetamine õigesti.
>
> SDK kõnede automaatselt selle `EndActivity` meetod, kui rakendus on suletud. Seega on **väga** soovitatav helistamiseks on `StartActivity` iga kord, kui tegevuse kasutaja muutmine ja **mitte kunagi** kõne meetod on `EndActivity` meetod. See meetod saadab sõnumi kaasamine server, et praeguse kasutaja on jätnud rakendus ja see mõjutab kõiki logid.

##<a name="advanced-reporting"></a>Täpsem aruandlus

Teise võimalusena võite aruande rakenduse teatud sündmused, tõrgete ja tööd, selleks kasutada teiste meetodite leitud soovitud `EngagementAgent` klassi. Kaasamine API võimaldab kasutada kõiki suhete 's täiustatud võimalused.

Lisateabe saamiseks vt [Täpsemalt Mobile kaasamine sildistamine API Windows Phone Silverlighti rakenduse kasutamise kohta](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Täiustatud konfigureerimine

### <a name="disable-automatic-crash-reporting"></a>Keela automaatne krahh teatamine

Saate keelata automaatse krahhi aruandluse funktsioon allikaid. Seejärel, kui toimub töötlemata erandi, kaasamine ei tee midagi.

> [AZURE.WARNING] Kui kavatsete seda funktsiooni, pidage meeles, et kui töötlemata krahhi võivad ilmneda rakenduse, kaasamine ei saada krahh **ja** see ei sulgu seanss ja -tööde haldamine.

Automaatse krahhi aruandluse keelamiseks lihtsalt kohandada oma konfiguratsioon, sõltuvalt sellest, saate selle deklareeritud.

#### <a name="from-engagementconfigurationxml-file"></a>Kaudu `EngagementConfiguration.xml` fail

Aruande krahh määramine `false` vahel `<reportCrash>` ja `</reportCrash>` sildid.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Kaudu `EngagementConfiguration` objekti käitusajal

Aruande krahh väärtuseks false, mis on teie EngagementConfiguration objekti abil.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Sarivõte

Vaikimisi logib reaalajas kaasamine teenuse aruandeid. Kui rakenduse aruannete logid väga sageli, on parem puhvri logid ja aruandluseks need kõik korraga aeg base (seda nimetatakse "Sarivõte").

Selleks helistada meetodit.

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument on väärtus **millisekundites**. Igal ajal, kui soovite uuesti reaalajas logimine, helistage meetodit ilma mis tahes parameeter, või väärtuse 0.

Selle Sarivõte veidi suurendada aku, kuid mõjutab kaasamine kuvari: kõik seansid ja -tööde haldamine kestus ümardatakse lõhkemist lävi (seega seansid ja -tööde haldamine lühem kui lõhkemist lävi ei pruugi olla nähtavad). Soovitatav on kasutada lõhkemist piirmäära enam kui 30000 (30s). Peate olema teadlik salvestatud logid on piiratud 300 üksust. Kui saatmine on liiga pikk kaotate mõned logid.

> [AZURE.WARNING] Burst lävi ei saa konfigureerida väiksema ajavahemikul kui sekund. Kui proovite seda teha, SDK kuvatakse jälitusteave viga ja saavad lähtestada vaikeväärtus, mis on null sekundit. See käivitab SDK logid reaalajas teatada.
 
