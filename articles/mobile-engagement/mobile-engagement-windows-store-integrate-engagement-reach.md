<properties 
    pageTitle="Universaalne rakendused Windows SDK integreerimise saavutamiseks." 
    description="Kuidas ühes kohas rakendused Windows Azure'i mobiilsideseadmete kaasamine REACHi integreerimine"
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

# <a name="windows-universal-apps-reach-sdk-integration"></a>Universaalne rakendused Windows SDK integreerimise saavutamiseks.

Peate järgima kirjeldatud [Universaalne kaasamine Windows SDK integreerimine](mobile-engagement-windows-store-integrate-engagement.md) enne järgmise juhendi integreerimine korras.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Windowsi universaalne projekti kaasamine saavutamiseks SDK embed

Te ei ole midagi lisada. `EngagementReach`viited ja ressursid on juba projektis.

> [AZURE.TIP] Saate kohandada pilte, mis asub selle `Resources` kausta projekti, eriti kaubamärgi ikooni (seda vaikekäitumist ikooni Engagement). Universaalne rakendused saate teisaldada ka selle `Resources` kaustas jagatud projekti vahel rakendused, kuid saate selle sisu ühiskasutusse andmiseks on hoida funktsiooni `Resources\EngagementConfiguration.xml` faili vaikeasukoht, et see on platvormi sõltuvad.

## <a name="enable-the-windows-notification-service"></a>Windowsi teavitusteenuse lubamine

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x ja Windows Phone 8.1 ainult

Selleks, et kasutada **Windows teavitusteenuse** (nimetatakse WNS) oma `Package.appxmanifest` faili `Application UI` klõpsake `All Image Assets` väljale vasakul robot. Paremas servas kasti `Notifications`, muuta `toast capable` kaudu `(not set)` et `(Yes)`.

### <a name="all-platforms"></a>Kõik platvormid

Peate sünkroonima rakenduse oma Microsofti kontoga ja allikaid Platformi. Seda peate Looge konto või logige [windows Arenduskeskus](https://dev.windows.com). Pärast, et luua uue rakenduse ja otsige SID ja salajane võti. Kaasamine frontend, valige oma rakenduse säte sisse `native push` ja kleepige oma kasutajanimi ja parool. Pärast seda, paremklõpsake valimine projekti `store` ja `Associate App with the Store...`. Peate lihtsalt valige rakendus, saate luua enne selle sünkroonida.

## <a name="initialize-the-engagement-reach-sdk"></a>Kaasamine REACHi SDK lähtestamine

Muutke soovitud `App.xaml.cs`:

-   Lisa `EngagementReach.Instance.Init` kohe pärast `EngagementAgent.Instance.Init` sisse oma `InitEngagement` meetod:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    Funktsiooni `EngagementReach.Instance.Init` sihtotstarbeline teemas töötab. Teil pole seda ise teha.

> [AZURE.NOTE] Kui kasutate Tõuketeatiste mujal oma rakenduse siis tuleb [jagada oma tõuketeatised kanali](#push-channel-sharing) kaasamine saavutamiseks.

## <a name="integration"></a>Integreerimine

Kaasamine pakub REACHi-rakenduse plakatid ja interstitsiaalne vaated teadaanded ja küsitluste rakenduse lisamiseks kaks võimalust: ülekatet integratsioon ja web vaadete käsitsi integreerimine. Teil peaks omavahel kombineerida nii lähenemine samal lehel.

Valida kahe integreerimine kokkuvõetavad sellisel viisil:

-   Võite valida ülekatet integreerimine kui lehtede pärib juba agendi `EngagementPage`, vaid paari asendamisel on `EngagementPage` , `EngagementPageOverlay` ja `xmlns:engagement="using:Microsoft.Azure.Engagement"` , `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` oma lehtedel.
-   Võite web vaadete käsitsi integreerimine kui soovite paigutada täpselt saavutamiseks UI sees lehtede või kui te ei soovi pärimise teise taseme lehtedele lisada. 

### <a name="overlay-integration"></a>Ülekatet integreerimine

Kaasamine ülekatet lisab dünaamiliselt Kasutajaliidese elemendid, kuvatakse teie lehel REACHi kampaaniat. Kui ülekatte ei sobi teie paigutuse kaaluge web vaadete käsitsi integreerimise asemel.

Oma .xaml faili muutmine toodud `EngagementPage` viide`EngagementPageOverlay`

-   Lisage oma nimeruumid deklaratsiooni.

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Asendage `engagement:EngagementPage` koos `engagement:EngagementPageOverlay`:

**Koos EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Koos EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Seejärel .cs faili sildistamine oma lehe `EngagementPageOverlay` asemel `EngagementPage` ja importida `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Asendage `EngagementPage` koos `EngagementPageOverlay`:

**Koos EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Koos EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Kaasamine ülekate lisab on `Grid` peal oma lehele element, mis koosneb oma paigutuse ja kahe `WebView` elemendid ühe sildi ja teine interstitsiaalne vaate jaoks.

Saate kohandada ülekatet elementide otse soovitud `EngagementPageOverlay.cs` faili.

### <a name="web-views-manual-integration"></a>Web vaadete käsitsi integreerimine

REACHi otsivad lehtede kahe `WebView` elementide eest banner ja interstitsiaalne vaate kuvamine. Ainus, mida on vaja teha on lisada need kaks `WebView` elementide kuskil oma lehtedel siin on näide:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


Selles näites on `WebView` elemendid on tõmmatud vastavalt nende container, mis automaatselt uuesti mahud need korral Kuva pööre või akna suuruse muutmine.

> [AZURE.WARNING] See on oluline hoida sama nimetamise `engagement_notification_content` ja `engagement_announcement_content` jaoks soovitud `WebView` elemente. REACHi tuvastamise nende nime järgi. 

## <a name="handle-datapush-optional"></a>Toime datapush (valikuline)

Kui soovite oma rakenduse REACHi andmete sunnib vastu võtta, teil rakendada EngagementReach klassi sündmustest.

Klõpsake App.xaml.cs App() ehitaja lisamiseks tehke järgmist.

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Näete, et iga meetodi tagasihelistamise tagastas on kahendväärtus. Kaasamine saadab tagasiside andmiseks oma tagaandmebaas pärast andmete tõuketeatised. Kui soovitud tagasihelistamise tagastab väärtuse false, on `exit` on saada tagasisidet. Muul juhul oleks `action`. Kui pole tagasihelistamise on seatud sündmusi, on `drop` tagasiside tagastatakse allikaid.

> [AZURE.WARNING] Kaasamine ei ole võimalik saada hulgidiagrammid tagasiside eest andmete tõuketeatised. Kui soovite määrata mitu käitleja sündmuse, võtke arvesse, et tagasiside vastavad viimasele üks saadetakse. Sel juhul soovitatav alati tagastab sama väärtuse, klõpsake selle ees vältida segadust tagasiside.

## <a name="customize-ui-optional"></a>Kohandamine Kasutajaliidese (valikuline)

### <a name="first-step"></a>Esimene samm

Me võimaldavad kohandada REACHi UI.

Selleks on vaja luua alamklass, on `EngagementReachHandler` klassi.

**Proovi kood:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Määrake sisu on `EngagementReach.Instance.Handler` välja koos oma kohandatud objekti oma `App.xaml.cs` klassi sees on `App()` meetod.

**Proovi kood:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Vaikimisi kasutab kaasamine enda rakendamine `EngagementReachHandler`.
> Teil pole vaja luua oma ja kui teete, pole teil alistada iga meetod. Vaikekäitumine on lingid base objekti valimiseks.

### <a name="web-view"></a>Veebivaade

Vaikimisi kasutab REACHi manustatud ressursse dll teatised ja lehtede kuvamiseks.

Anda täielik kohandamine kasutame veebivaade võimalusi. Kui soovite kohandada paigutused, alistada otse ressursid failide `EngagementAnnouncement.html` ja `EngagementNotification.html`. Kaasamine tuleb kõik kood `<body></body>` õigeks töötamiseks. Saate lisada sildi väline, kuid `engagement_webview_area`.

Siiski saate otsustada, kas kasutada oma ressursse.

Saate alistada `EngagementReachHandler` meetodite kaasamine paigutuste kasutamine, kuid olge ettevaatlik, et teile öelda teie alamklassi manustatud kaasamine süsteem:

**Proovi kood:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Vaikimisi on AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. See tähistab tõuketeatised sõnumi (teksti teadaanne, Web anoucement ja küsitluse teadaanne) sisu kujundada HTML-faili. AnnouncementName on `engagement_announcement_content`. See on webview kujundus xaml lehel oma nimi.

NotfificationHTML on `ms-appx-web:///Resources/EngagementNotification.html`. See tähistab kujundamine teate tõuketeatised sõnumi HTML-faili. NotfificationName on `engagement_notification_content`. See on webview kujundus xaml lehel oma nimi.

### <a name="customization"></a>Kohandamine

Saate kohandada teatis ja teadaanne veebivaade on te soovite siis, kui saate säilitada kaasamine objekti. Olge ettevaatlik, et webview objekt on kirjeldatud kolm korda - oma xaml esimest korda, teist korda "setwebview()" meetod .cs failis ja kolmanda aja HTML-faili.

-   Teie xaml saate kirjeldada praeguse graafilise paigutuse webview komponent.
-   .Cs faili saate määratleda "setwebview()", kus saate määrata mõõde kaks webview (teatis, teadaanne). See on väga tõhus, kui rakendus on suurust muuta.
-   Kaasamine HTML-faili kirjeldame webview sisu, kujundus ja elementide positsioonide omavahel.

### <a name="launch-message"></a>Käivitage sõnum

Kui kasutaja klõpsab süsteemi teatis (töölauateatis), kaasamine käivitab rakenduse, laadimine tõuketeatised sõnumite sisu ja kuvada vastava turunduskampaania leht.

Rakenduse käivitamine ja kuvatava lehe (olenevalt teie võrgu kiiruse) vahel on viivitus.

Tähistamiseks kasutajale midagi on laadimise, peate esitama visuaalse teabe, nt edenemise riba või edenemise näidik. Kaasamine ei oska, et ise, kuid pakub mõne käitleja teie eest.

Funktsiooni tagasihelistamise kasutusele App.xaml.cs sisse "Avaliku App() {}" lisada:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Saate määrata selle tagasihelistamise oma "Avaliku App() {}" meetodit oma `App.xaml.cs` parim enne faili selle `EngagementReach.Instance.Init()` kõne.

> [AZURE.TIP] Iga sündmuseohjuri nimetatakse UI lõime. Te ei pea muretsema, MessageBox või midagi seotud Kasutajaliidese kasutamisel.

##<a id="push-channel-sharing"></a>Tõuketeatised kanali ühiskasutus

Kui kasutate Tõuketeatiste muul eesmärgil oma rakenduse siis tuleb kasutada tõuketeatised kanali ühiskasutusfunktsiooni kaasamine SDK. See on vastamata tõuketeatised vältimiseks.

- Saate sisestada oma tõuketeatised kanali kaasamine saavutamiseks lähtestamine. SDK, kasutatakse selle asemel, et paluda uus.

Värskendada oma tõuketeatised kanali kaasamine saavutamiseks lähtestamine selle `InitEngagement` meetod on `App.xaml.cs` faili:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Teine võimalus, kui soovite lihtsalt tarbimine tõuketeatised kanali pärast REACHi lähtestamine, siis saate seada pakkumist kaasamine saavutamiseks saada tõuketeatised kanali üks kord see on loodud SDK.

Määrata oma tagasihelistamise veebisaidil mis tahes kohas **pärast** REACHi lähtestamine.

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Kohandatud värviskeemi Näpunäide.

Pakume kohandatud värviskeemi kasutamine. Saate saata erinevat tüüpi URI kaasamine frontend kaasamine rakenduse kasutamiseks. Vaikimisi skeemi nagu `http, ftp, ...` on haldamine Windows akna saavad viip kui nad pole vaikimisi määratud rakenduse seadmesse installitud. Samuti saate luua oma kohandatud värviskeemi rakenduse.

Lihtne viis rakenduse kohandatud värviskeemi määramiseks on avada oma `Package.appxmanifest` minna `Declarations` paani. Valige `Protocol` kerige väljal saadaolevad andmed ja lisage see. Redigeerimine on `Name` välja oma uue protokolli soovitud nimi.

Nüüd kasutama protokolli muuta oma `App.xaml.cs` koos selle `OnActivated` meetod ja ärge unustage lähtestamine kaasamine siin ka:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
