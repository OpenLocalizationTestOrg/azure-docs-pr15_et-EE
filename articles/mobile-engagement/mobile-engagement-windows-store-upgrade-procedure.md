<properties 
    pageTitle="Universaalne rakendused Windows SDK täiendamine toimingute" 
    description="Universaalne rakendused Windows SDK täiendamine toimingute Azure mobiilsideseadmete kaasamine"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Universaalne rakendused Windows SDK täiendamine toimingute

Kui teil juba on integreeritud vanemat versiooni kaasamine rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Teil võib olla mitu toimingute jälgimiseks, kui teil on vastamata SDK mitut versiooni. Näiteks kui migreerite 0.10.1 0.11.0 tuleb esmalt kasutage toimingut ", et 0.10.1 0.9.0" siis korras ", et 0.11.0 0.10.1".

##<a name="from-330-to-340"></a>Et 3.4.0 3.3.0 kaudu

### <a name="test-logs"></a>Logide testimine

Praegu saab konsooli logid toodeti SDK lubatud/keelatud/filtreeritud. See kohandamiseks atribuudi värskendamiseks `EngagementAgent.Instance.TestLogEnabled` üks väärtus, mis on saadaval on `EngagementTestLogLevel` loendamine, näiteks:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Ressursid

REACHi ülekatet on täiustatud. See on osa SDK Nugeti pakett ressursid.

Kuigi versiooniks SDK uue versiooni saate valida, kas soovite hoida olemasolevaid faile oma ressursse ülekatet kausta või mitte.

* Kui eelmine ülekatet töötab teie eest või teil on integreerimine on `WebView` käsitsi seejärel saate otsustate väljumisel failide elemente, seda ikkagi tööd. 
* Kui soovite võtta kasutusele uue ülekatet, vaid asendada kogu `overlay` kausta oma ressursid uue SDK pakett (UWP rakendused: pärast versiooniuuendust, saate uue kausta ülekatet KASUTAJAPROFIILI %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Uue ülekatet abil kirjutab eelmise versiooni tehtud kohandused.

##<a name="from-320-to-330"></a>Et 3.3.0 3.2.0 kaudu

### <a name="resources"></a>Ressursid
Selles etapis tuleb puudutab ainult kohandatud ressursid. Kui teil on kohandatud SDK (html, pildid, ülekatet) vahendite siis tuleb need enne üleminekut varundus ja uuesti oma uuele versioonile üle viidud ressursid kohandamine.

##<a name="from-310-to-320"></a>3.1.0 3.2.0 kaudu

### <a name="resources"></a>Ressursid
Selles etapis tuleb puudutab ainult kohandatud ressursid. Kui teil on kohandatud SDK (html, pildid, ülekatet) vahendite siis tuleb need enne üleminekut varundus ja uuesti oma uuele versioonile üle viidud ressursid kohandamine.

### <a name="webview-integration"></a>WebView integreerimine
Selles versioonis võeti kasutusele mõne muu seadme vormi tegureid vastavaks täiustused. Veenduge, et, et teie integreerimine on webview vastavad järgmist:

Teie XAML lehe ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Kui ka failiga seostatud .cs:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Et 3.0.0 2.0.0 kaudu

### <a name="resources"></a>Ressursid
Selles etapis tuleb puudutab ainult kohandatud ressursid. Kui teil on kohandatud SDK (html, pildid, ülekatet) vahendite siis tuleb need enne üleminekut varundus ja uuesti oma uuele versioonile üle viidud ressursid kohandamine.

##<a name="from-111-to-200"></a>Et 2.0.0 1.1.1 kaudu

Järgmine kirjeldatakse, kuidas migreerida mõne SDK integreerimine Capptain teenus Capptain SAS tootja Azure Mobile kaasamine rakendusse. 

> [Azure.IMPORTANT] Capptain ja Mobile kaasamine pole sama teenuste ja allpool toodud juhiseid tõstetakse esile ainult kuidas saate migreerida kliendi rakendus. Migreerimine SDK rakenduse kuvatakse siirata andmete Capptain serverid serverid Mobile kaasamine

Kui migreerite varasemast versioonist, pöörduge Capptain veebisaidi migreerimine 1.1.1 esmalt siis järgmise toimingu rakendamine

### <a name="nuget-package"></a>Nugeti pakett

**Capptain.WindowsPhone** asendamiseks **MicrosoftAzure.MobileEngagement** Nugeti pakett.

### <a name="applying-mobile-engagement"></a>Rakendades mobiilsideseadmete kaasamine

SDK kasutab termin `Engagement`. Peate värskendama oma projekti selle muudatuse vastavaks.

Peate desinstallima oma praeguse Capptain Nugeti pakett. Võtke arvesse, et kõik muudatused Capptain ressursid kausta, eemaldatakse. Kui soovite need failid ja seejärel kopeerida need.

Pärast seda, installige uus Microsoft Azure'i Engagement Nugeti pakett projekti. Leiate otse veebisaidil [Nugeti]. ja siin indeks. See toiming asendab kõik ressursid failid, mis kasutavad kaasamine ja lisab uue kaasamine DLL oma projekti viited.

Teil on oma projekti viited puhastada kustutades Capptain DLL viited. Kui te ei tee seda, Capptain versiooni konflikte ja vigu juhtub.

Kui teil on kohandatud Capptain ressursse, kopeerige vana failide sisu ja kleepige need uue kaasamine failid. Pange tähele, et xaml-ja cs tuleb värskendada.

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

4. Lehe muudatuste ülekattega
    > [AZURE.IMPORTANT] Ülekatet seal muudatusi. Selle uue nimeruum on `Microsoft.Azure.Engagement.Overlay`. See on nii XAML-i ja cs-faile kasutada. Peale selle `CapptainGrid` on oma nime `EngagementGrid`, `capptain_notification_content` ja `capptain_announcement_content` on nimega `engagement_notification_content` ja `engagement_announcement_content`.
    
    Jaoks ülekatet:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    See muutub:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Capptain pildid ja HTML-failid nagu muud ressursid, Pange tähele, et nad ka on ümber nimetatud "Kaasamine" kasutama.

### <a name="project-declaration"></a>Projekti deklaratsioon

Klõpsake Package.appxmanifest `File Type Associations` on värskendatud:

 -   capptain\_saavutamiseks\_sisu kaasamine\_saavutamiseks\_sisu
 -   capptain\_log\_faili kaasamine\_log\_fail

### <a name="application-id--sdk-key"></a>Rakenduse ID / SDK võti

Kaasamine kasutab ühendusstringi. Teil määrata rakenduste ID-d ja mõne SDK vajutamisega Mobile kaasamine, tuleb ainult määrata ühendusstring. Saate häälestada selle oma EngagementConfiguration faili.

Kaasamine konfiguratsiooni saab määrata oma `Resources\EngagementConfiguration.xml` projekti fail.

Määrake faili redigeerimiseks tehke järgmist.

-   Rakenduse ühendusstringi siltide vahel `<connectionString>` ja `<\connectionString>`.

Kui soovite määrata selle käitusajal, saate helistada enne kaasamine agent lähtestamine järgmisel viisil:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

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

 
