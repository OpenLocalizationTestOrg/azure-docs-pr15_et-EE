<properties
    pageTitle="iOS-i tõuketeatised teatis jaoturi abil Xamarin rakenduste | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas saata Tõuketeatiste Xamarin iOS-i rakendus Azure'i teatis jaoturi abil."
    services="notification-hubs"
    keywords="iOS-i tõuketeatised tõuketeatised sõnumite tõuketeatised, vajutage sõnumi"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS-i tõuketeatised teatis jaoturi abil Xamarin rakendused

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade
> [AZURE.IMPORTANT] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Selle õpetuse näidatakse, kuidas saata Tõuketeatiste iOS-i rakendus Azure'i teatis jaoturi abil.
Saate luua tühja Xamarin.iOS rakendus, mis saab Tõuketeatiste [Apple'i Lükketeatiste teenus (APNs-i)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)abil. Kui olete lõpetanud, saate küll leviedastus Tõuketeatiste töötavad rakenduse seadmed teie teate jaoturi abil. Viimistletud kood on saadaval [NotificationHubs rakenduse] [ GitHub] valimi.

Selle õpetuse näitab lihtsa tõuketeatised sõnumi leviedastuse stsenaariumi teatis jaoturi abil.

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ [Xcode 6.0][Install Xcode]
+ IOS-i 7.0 (või uuem versioon) seadmega
+ iOS-i arendaja liikmelisus
+ [Xamarin Studio]

   > [AZURE.NOTE] Konfiguratsiooni nõuded tõttu for iOS-i tõuketeatised, peate juurutada ja test valimi taotluse füüsilise iOS-i seadmes (iPhone või iPad) asemel kuvatakse simulaatorit.

Selle õpetuse lõpuleviimine on nõutav kõik teatis jaoturi õpetused Xamarin iOS-i rakendused.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Teie teate jaoturi konfigureerimine

Selles jaotises juhatab teid läbi uus teatis keskuse loomine ja APNs-i abil loodud **.p12** tõuketeatised serdi abil autentimise konfigureerimine. Kui soovite kasutada teate jaoturi, mille olete juba loonud, võite jätkata juhisega 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Kui soovime konfigureeriks APNs-i, Azure'i portaalis, teate jaoturi sätete avamine, ande klõpsake <b>Teate teenuste</b>ja klõpsake loendis üksust <b>Apple'i (APNs-i)</b> . Kui lõpetanud, klõpsake <b>Serdi üleslaadimine</b> ja valige eksporditud varasemas versioonis, samuti parooli serdi <b>.p12</b> sert.</p>
<p>Veenduge, et valida <b>liivakastirežiimi</b> , kuna saadate sõnumeid tõuketeatised arengu keskkonnas. Kui soovite saata kasutajatele, kes juba ostnud poest rakenduse Tõuketeatiste kasutada ainult <b>tootmise</b> säte.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Teie teate jaoturi on nüüd konfigureeritud töötamiseks APNs-i ja teil on rakenduse registreerida ja saata Tõuketeatiste stringid ühendus.


##<a name="connect-your-app-to-the-notification-hub"></a>Ühenduse loomine rakenduse teate jaoturi

#### <a name="create-a-new-project"></a>Uue projekti loomine

1. Xamarin Studios, iOS-i uue projekti loomine ja valige **Ühendatud API** > **Ühe kuvamine rakenduse** mall.

    ![Xamarin Studio - tüübi valimine][31]

2. Lisage komponent Azure'i sõnumside viide. Lahendus vaates paremklõpsake oma projekti **komponendid** kausta ja valige **Saada lisateavet komponendid**. **Azure'i sõnumside** komponent otsida ja lisada projekti osa.

3. **AppDelegate.cs**, lisage järgmine lause abil:

        using WindowsAzure.Messaging;

4. Deklareerida **SBNotificationHub**näiteks:

        private SBNotificationHub Hub { get; set; }

5. Järgmiste muutujate **Constants.cs** klassi luua.

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. Värskenduse **AppDelegate.cs**, **FinishedLaunching()** , et need vastaksid järgmist:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Alistada **AppDelegate.cs** **RegisteredForRemoteNotifications()** meetod:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Alistada **AppDelegate.cs** **ReceivedRemoteNotification()** meetod:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Looge **AppDelegate.cs** **ProcessNotification()** järgmisel viisil:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Saate valida alistada **FailedToRegisterForRemoteNotifications()** reageerimine olukordades, nt puudub võrguühendus. See on eriti oluline, kui kasutaja võib rakenduse käivitamine ühenduseta režiimis (nt Lennukikujuline) ja soovite käsitleda tõuketeatised stsenaariumid, mis on seotud rakenduse sõnumside.


10. Käivitage rakendus oma seadmes.


## <a name="sending-push-notifications"></a>Tõuketeatiste saatmine


Saate testida vastuvõtmine Tõuketeatiste rakenduse saatmisega teatised [Azure portaali] kaudu ning **tõrkeotsingu** tööriistakomplekt võimalust **Testida saata** teate jaoturi lehel paremale, nagu on näidatud allpool ekraani.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Tõuketeatiste tavapäraselt tagaandmebaas teenuse nagu Mobile teenuseid või ASP.net-i abil ühilduvad teegi kaudu. Samuti saate REST API otse tõuketeatised sõnumite saatmine, kui teegis pole saadaval teie stsenaariumi. 

Selles õpetuses me Hoidke sõnum lihtne ja lihtsalt näidata testimine oma kliendi rakenduse saatmisega .NET SDK kasutamise teatis jaoturi konsooli rakendus asemel kirjutamata teenuse teatised. Soovitame [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) õpetuse järgmise sammuna teatiste saatmine on ASP.net-i taustväärtus. Järgmistest võimalustest saab saatmise teatised:

* **Ülejäänud kasutajaliidese**: saate toetada tõuketeatised teatis mis tahes kirjutamata platvormi [ülejäänud kasutajaliidese](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)abil.

* **Microsoft Azure'i teatis jaoturi .NET SDK**: the Nugeti pakett halduris Visual Studio, käivitage [Installi pakett Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [teatis jaoturi kaudu Node.js kasutamise kohta](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobiilirakenduste kohta**: Kuidas saata teatised Azure'i rakenduse teenuse mobiilirakenduste kirjutamata, mis on integreeritud teatis jaoturi näide leiate [Lisa Tõuketeatiste oma mobiilirakenduse abil](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: näide sellest, kuidas saata Tõuketeatiste REST API-de abil kohta teavet teemast "kasutada teatis jaoturi kaudu Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Valikuline) Tõuketeatiste saatmine .net-i konsooli rakenduse kaudu

Selles jaotises saadame Tõuketeatiste lihtsa .net-i konsooli rakenduse kasutamine. Selles näites eesmärgil me aktiveerige Windows arenduskeskkond, mis on juba installitud Visual Studio.

1. Visual Studio, luuakse uus konsool rakendus Visual C#:

    ![Visual Studio - Loo uus konsool rakendus][213]

2. Visual Studios, klõpsake nuppu **Tööriistad**, klõpsake **Nugeti Package Manager**ja klõpsake **Package Manager konsooli**.

    Package manager konsooli peaks kuvatama dokitud Visual Studio tööruumi allosas.

3. Paketi Manager konsooli aknas määrata **vaikimisi projekti** uue projekti konsooli rakendus ja seejärel konsooli aken, käivitage järgmine käsk:

        Install-Package Microsoft.Azure.NotificationHubs

    Sellega lisatakse Azure'i teatis jaoturi SDK abil <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification jaoturi Nugeti pakett</a>viide.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Avatud on `Program.cs` faili ja lisada järgmine `using` tagada, et kasutame Azure tunnid ja funktsioonide peamised tunni jooksul lause:

        using Microsoft.Azure.NotificationHubs;

3. Klõpsake oma `Program` klassi, lisada järgmisel viisil (Ärge unustage asendamiseks **ühendusstringi** ja **keskuse nimi**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Lisage järgmised read oma `Main` meetod:

         SendNotificationAsync();
         Console.ReadLine();

5. Rakenduse käivitamiseks vajutage klahvi F5. Sekunditega, peaksite nägema tõuketeatised teatis kuvatakse teie seadmes. Kas kasutate WiFi-ühendust või mõne mobiilsidevõrgu, veenduge, et seadmes on Interneti-ühendust.

Apple'i [kohalik Push teatis programmeerimisega juhendi]leiate kõigi võimalike lõhkelaengu.

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Valikuline) Teatiste saatmine mobiilsideteenuse ülevaade

Selles jaotises saadame Tõuketeatiste abil mobiiliteenuse sõlm skripti kaudu.

Teatise saatmine mobiiliteenuse abil, järgige [Alustamine Mobile teenused], ja seejärel:

1. [Azure'i klassikaline portaali]sisse ja valige oma mobiilsideteenuse.

2. Valige vahekaart **Scheduler** peal.

    ![Azure'i klassikaline portaali - ajasti][215]

3. Looge uus ajastatud töö nime ja valige **nõudmisel**.

    ![Azure'i klassikaline portaali - uue töö loomine][216]

4. Kui töö on loodud, klõpsake töö nime. Klõpsake vahekaarti **skripti** ülaribal.

5. Sisestage järgmine skript oma ajasti funktsiooni sees. Veenduge, et kohatäidete asendamine oma teatise jaoturi nimi ja ühendusstringi *DefaultFullSharedAccessSignature* , mida te saada varasemas versioonis. Klõpsake nuppu **Salvesta**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Klõpsake allosas oleval ribal nuppu **Käivita üks kord** . Te peate saama teatise oma seadmes.

##<a name="next-steps"></a>Järgmised sammud

Selles näites lihtsa eetris Tõuketeatiste oma iOS seadmed. Selleks, et suunata kindlatele kasutajatele, vaadake õpetuse [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele]. Kui soovite segmendi kasutajate poolt huvirühmade, saate lugeda [Kasutamine teatis jaoturi uudiseid saata]. Lisateavet teate jaoturi [Teatis jaoturi juhised] ja selle [Teate jaoturi juhiseid iOS-i]kohta.


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Mobile teenuste kasutamise alustamine]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
[Teatis jaoturi juhised]: http://msdn.microsoft.com/library/jj927170.aspx
[Teatis jaoturi juhiseid iOS-i]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Tõuketeatiste kasutajatele teatis jaoturi abil]: /manage/services/notification-hubs/notify-users-aspnet
[Teatis jaoturi abil saate saata uudiseid]: /manage/services/notification-hubs/breaking-news-dotnet

[Kohaliku ja lükake teatis programmeerimise juhend]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure'i portaal]: https://portal.azure.com
