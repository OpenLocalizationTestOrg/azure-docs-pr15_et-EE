<properties 
    pageTitle="Windowsi ühes kohas kaasamine SDK integreerimine" 
    description="Kuidas integreerida universaalne rakendused Windows Azure'i mobiilsideseadmete kaasamine"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windowsi ühes kohas kaasamine SDK integreerimine

> [AZURE.SELECTOR] 
- [Universaalne Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS-i](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Siin kirjeldatakse lihtsaim viis suhete 's analüüsi- ja jälgimise funktsioone Windows universaalne rakenduse aktiveerimiseks.

Järgmised toimingud on piisavalt arvutada statistikat kõik kasutajad, seansid, tegevused, jookseb ja Technicals vaja logid aruande aktiveerimiseks. Aruande arvutada teiste statistikute reguleerimist sündmusi, tõrgete ja tööd, nagu on vaja logid tuleb teha käsitsi kaasamine API abil (vt [Täpsemalt Mobile kaasamine sildistamine API Windows universaalne rakenduse kasutamise kohta](mobile-engagement-windows-store-use-engagement-api.md) , kuna see statistika on sõltuva.

## <a name="supported-versions"></a>Toetatud versioonid

Mobile kaasamine SDK jaoks Windows universaalne rakendused saab ainult integreerida Windows kestus ja universaalne Windowsi platvormi rakenduste suunamise:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (töölaua- ja peredele)

> [AZURE.NOTE] Kui suunatud Windows Phone Silverlighti, siis [Windows Phone Silverlighti integratsiooni kord](mobile-engagement-windows-phone-integrate-engagement.md)viidata.

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Mobiilsideseadmete kaasamine universaalne rakendused SDK installimine

### <a name="all-platforms"></a>Kõik platvormid

Mobile kaasamine SDK jaoks universaalne rakendus Windows on saadaval Nugeti paketina, mida nimetatakse *MicrosoftAzure.MobileEngagement*. Saate installida selle kaudu Visual Studio Nugeti Package Manager.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x ja Windows Phone 8.1

Nugeti kasutab automaatselt SDK ressursside kohta on `Resources` rakenduse projekti juurkausta.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 Universal Windowsi platvormi rakendused

Nugeti pole automaatselt juurutada SDK ressursse rakenduse UWP veel. Teil teha seda käsitsi, kuni juurutamise ressursid on uuesti Nugeti.

1.  Avage oma File Explorer.
2.  Liikumine järgmisesse asukohta (**x.x.x** on Office'i versiooni installimise Engagement): *kausta %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Pukseerige **ressursse** kaust file Explorer Visual Studio projekti juurkausta.
4.  Visual Studio valimine projekti ja aktiveerimine ikooni **Kuva kõik failid** peale **Solution Exploreris**.
5.  Teatud faile ei kuulu projekt. Importida neid korraga paremklõps **välistamine projekti** **ressursid** kausta, siis teise paremklõps kausta **ressursse** , **kaasa projekti** uuesti lisada kogu kausta. Kõik failid kaustast **ressursid** on nüüd kaasatud projekti.

## <a name="add-the-capabilities"></a>Võimaluste lisamine

Kaasamine SDK peab mõned funktsioonid Windows SDK korralikult töötamiseks.

Avage oma `Package.appxmanifest` failide ja veenduge, et deklareeritud järgmisi funktsioone:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Kaasamine SDK lähtestamine

### <a name="engagement-configuration"></a>Kaasamine konfigureerimine

Kaasamine konfigureerimine on tsentraliseeritud soovitud `Resources\EngagementConfiguration.xml` projekti fail.

Määrata, et selle faili redigeerimiseks tehke järgmist.

-   Rakenduse ühendusstringi siltide vahel `<connectionString>` ja `<\connectionString>`.

Kui soovite määrata selle käitusajal, saate helistada enne kaasamine agent lähtestamine järgmisel viisil:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Ühendusstringi rakenduse jaoks kuvatakse Azure'i klassikaline portaal.

### <a name="engagement-initialization"></a>Kaasamine lähtestamine

Kui loote uue projekti, on `App.xaml.cs` fail on loodud. See tund pärib `Application` ja see sisaldab palju olulised meetodid. Seda kasutatakse kaasamine SDK lähtestada.

Muutke soovitud `App.xaml.cs`:

-   Lisada oma `using` laused:

        using Microsoft.Azure.Engagement;

-   Meetodi kaasamine lähtestamine üks kord kõik kõnede jaoks ühiskasutusse anda määratlemine

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Kõne `InitEngagement` sisse selle `OnLaunched` meetod:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Kui teie taotlus on käivitatud kohandatud värviskeemi, mõnda muusse rakendusse või käsurea kaudu ja seejärel soovitud `OnActivated` meetodit nimetatakse. Samuti vajate kaasamine SDK käivitamiseks, kui teie rakendus on aktiveeritud. Selleks alistada `OnActivated` meetod:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Soovitame tungivalt takistada saate lisada teise kohta rakenduse kaasamine lähtestamine.

## <a name="basic-reporting"></a>Tavaline teatamine

### <a name="recommended-method-overload-your-page-classes"></a>Soovitatav meetod: ülekoormuse oma `Page` tunnid

Selleks, et aktiveerida kõik logid nõutud kaasamine arvutada kasutajaid, seansid, tegevused, jookseb ja tehniline statistika aruande, vaid saate kõik teie `Page` sub tunnid pärivad selle `EngagementPage` tunnid.

Siin on näide sellest, kuidas seda teha rakenduse lehe. Mida saab teha sama rakenduse kõik leheküljed.

#### <a name="c-source-file"></a>C# lähtefail

Muutke oma lehe `.xaml.cs` faili:

-   Lisada oma `using` laused:

        using Microsoft.Azure.Engagement;

-   Asendage `Page` koos `EngagementPage`:

**Ilma kaasamine:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Kus lingid:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Kui teie lehe alistab selle `OnNavigatedTo` meetod, veenduge, et kõne `base.OnNavigatedTo(e)`. Vastasel korral ei saa esitada tegevuse (selle `EngagementPage` kõned `StartActivity` sees selle `OnNavigatedTo` meetod).

#### <a name="xaml-file"></a>XAML-i fail

Muutke oma lehe `.xaml` faili:

-   Lisage oma nimeruumid deklaratsiooni.

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Asendage `Page` koos `engagement:EngagementPage`:

**Ilma kaasamine:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Kus lingid:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Vaikimisi sisse lülitatud alistamine

Vaikimisi on selle tunni nime lehe staatusega tegevuse nime, mille ilma täiendava. Kui ainekursuse kasutab järelliide "Leht", kaasamine ka eemaldada see.

Kui soovite tühistamine kellelt vaikimisi sisse lülitatud, lihtsalt lisada järgmine kood:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Kui soovite teatada mõned täiendavat informations koos oma tegevust, saate lisada järgmine kood:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Nende meetodite nimetatakse rakenduses selle `OnNavigatedTo` lehe meetod.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatiivne meetod: kõne `StartActivity()` käsitsi

Kui te ei saa või ei soovi ülekoormuse oma `Page` tunnid, selle asemel saate alustada oma tegevuste helistades `EngagementAgent` meetodid otse.

Soovitame helistada `StartActivity` sees oma `OnNavigatedTo` lehe meetod.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Veenduge, et teie seansi lõpetamine õigesti.
> 
> Universaalne Windows SDK kõnede automaatselt selle `EndActivity` meetod, kui rakendus on suletud. Seega on **väga** soovitatav helistamiseks on `StartActivity` iga kord, kui tegevuse kasutaja muutmine ja **mitte kunagi** kõne meetod on `EndActivity` meetod seda meetodit saadab kaasamine server, et praeguse kasutaja on rakenduse jätta, see mõjutab kõiki logid.

## <a name="advanced-reporting"></a>Täpsem aruandlus

Teise võimalusena võite aruande rakenduse kindla sündmused, tõrgete ja tööd, selleks kasutada teiste meetodite leitud soovitud `EngagementAgent` klassi. Kaasamine API võimaldab kasutada kõiki suhete 's täiustatud võimalused.

Lisateabe saamiseks vt [Täpsemalt Mobile kaasamine sildistamine API Windows universaalne rakenduse kasutamise kohta](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Täiustatud konfigureerimine

### <a name="disable-automatic-crash-reporting"></a>Keela automaatne krahh teatamine

Saate keelata automaatse krahhi aruandluse funktsioon allikaid. Seejärel, kui toimub töötlemata erandi, kaasamine ei tee midagi.

> [AZURE.WARNING] Kui kavatsete seda funktsiooni, pidage meeles, et kui töötlemata krahhi võivad ilmneda rakenduse, kaasamine ei saada krahh **ja** ei sulgu seanss ja -tööde haldamine.

Automaatse krahhi aruandluse keelamiseks lihtsalt kohandada oma konfiguratsioon, sõltuvalt sellest, saate selle deklareeritud.

#### <a name="from-engagementconfigurationxml-file"></a>Kaudu `EngagementConfiguration.xml` fail

Aruande krahh määramine `false` vahel `<reportCrash>` ja `</reportCrash>` sildid.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Kaudu `EngagementConfiguration` objekti käitusajal

Aruande krahh väärtuseks false, mis on teie EngagementConfiguration objekti abil.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Sarivõte

Vaikimisi logib reaalajas kaasamine teenuse aruandeid. Kui rakenduse aruannete logid väga sageli, on parem puhvri logid ja aruandluseks need kõik korraga aeg base (seda nimetatakse "Sarivõte").

Selleks helistada meetodit.

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument on väärtus **millisekundites**. Igal ajal, kui soovite uuesti reaalajas logimine, helistage meetodit ilma mis tahes parameeter, või väärtuse 0.

Selle Sarivõte veidi suurendada aku, kuid mõjutab kaasamine kuvari: kõik seansid ja -tööde haldamine kestus ümardatakse lõhkemist lävi (seega seansid ja -tööde haldamine lühem kui lõhkemist lävi ei pruugi olla nähtavad). Soovitatav on kasutada lõhkemist piirmäära enam kui 30000 (30s). Peate olema teadlik salvestatud logid on piiratud 300 üksust. Kui saatmine on liiga pikk kaotate mõned logid.

> [AZURE.WARNING] Burst lävi ei saa konfigureerida väiksem 1s punkt. Kui proovite seda teha, SDK näitab jälgi tõrke ja vaikeväärtus, st 0s lähtestab automaatselt. See käivitab SDK logid reaalajas teatada.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
