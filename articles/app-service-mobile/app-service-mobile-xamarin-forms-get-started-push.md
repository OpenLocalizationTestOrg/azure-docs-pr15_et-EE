<properties
    pageTitle="Tõuketeatiste lisamine rakenduse Xamarin.Forms | Microsoft Azure'i"
    description="Saate teada, kuidas saada mitme platvormi Tõuketeatiste Xamarin.Forms rakenduste Azure services abil."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Tõuketeatiste Xamarin.Forms rakenduse lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Ülevaade

Selles õpetuses lisamist Tõuketeatiste kõiki projekte põhjustas [Xamarin.Forms Kiire alustamine](app-service-mobile-xamarin-forms-get-started.md) , et tõuketeatised teavitus saadetakse kõigile platvormidel klientidele iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateavet leiate [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Eeltingimused

* IOS-i, peate mõne [Apple'i arendaja liikmelisus](https://developer.apple.com/programs/ios/) ja füüsilise iOS-i seadme Kuna [iOS-i simulaatorit ei toeta tõuketeatisi](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Teate jaoturi konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Värskenduse saatmiseks tõuketeatisi projekti server

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Valikuline) Konfigureerida ja käivitada Androidi projekt

Täitke selles jaotises lubamiseks Tõuketeatiste Xamarin.Forms Droid projekti Androidi jaoks.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Luba Firebase'i Cloud sõnumside (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Tõuketeatised päringuid abil FCM mobiilirakenduse kirjutamata konfigureerimine

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Tõuketeatiste lisada Androidi projekti

Taustväärtus, mis on konfigureeritud FCM, saame lisada komponendid koodide kliendile registreerima FCM, Registreeruge Tõuketeatiste Azure'i teatis jaoturi abil kirjutamata mobiilirakenduse kaudu ja teatisi.

1. **Droid** Projectis, paremklõpsake kausta **komponendid** nuppu **Saada lisateavet komponendid...**, **Google Cloud sõnumside kliendi** komponent otsimine ja lisamine projekti. See komponent toetab Tõuketeatiste Xamarin Androidi projekti.


2. Avage fail MainActivity.cs projekti ja lisage järgmine abil faili ülaosas lause:

        using Gcm.Client;

3. Pärast kõne **LoadApplication**lisada **OnCreate** meetod järgmine kood:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Lisage uus **CreateAndShowDialog** helper meetod järgmiselt:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Saate lisada **MainActivity** klassi järgmine kood:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    See seab **MainActivity** praeguse eksemplari, et saaksime täita peamise Kasutajaliidese teemat.

6. Lähtestada selle `instance`, muutuv alguses **OnCreate** meetod järgmiselt.

        // Set the current instance of MainActivity.
        instance = this;

2. Uue ainekursuse faili lisamine **Droid** projekti nimega `GcmService.cs`, ja veenduge, et faili ülaosas on **abil** järgmistest:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Lisage järgmised luba kutsed faili ülaosas pärast **abil** laused ja enne **nimeruumi** deklaratsioon.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Lisage järgmised klassi määratluse nimeruumi. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Asendage **< PROJECT_NUMBER >** oma projektinumber eespool märgitud.   

11. Järgmine kood, mis kasutab leviedastuse vastuvõtja uue tühja **GcmService** klassi asendamiseks tehke järgmist.

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Lisada järgmine kood **GcmService** klassi, alistab **OnRegistered** sündmuseohjuri ja rakendab **Register** meetod.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Järgmine kood, mis rakendab **OnMessage**lisamiseks tehke järgmist. 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    See tegeleb sissetulevate teadete ja saada teatis halduri kuvada.

14. **GcmServiceBase** nõuab rakendada **OnUnRegistered** ja **Tõrke_korral** sündmuseohjuri meetodid, mida saate teha järgmiselt:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Nüüd olete valmis testi Tõuketeatiste opsüsteemi Android-seadmes või emulaator rakenduses.

###<a name="test-push-notifications-in-your-android-app"></a>Androidi rakenduse testi tõuketeatised

Kahe esimese vajalike ainult siis, kui emulaator katsetamine.

1. Veenduge, et teil on kasutusele võtavad või silumine virtuaalse seadmes, mis sisaldab Google API-de määramine mis Sihtvaluuta, nagu on näidatud allpool Androidi virtuaalse seadme (AVD) manager.

2. Google konto lisamine Androidi seadmes **rakendused**klõpsates > **sätted** > **Lisa konto**ja seejärel järgige viipasid kasutada seadme Google konto lisamine uue konto loomiseks.

1. Visual Studio või Xamarin Studio, paremklõpsake **Droid** projekt ja klõpsake käsku **käivitus projekti seadmine**.

2. Nuppu **Käivita** projekti koostamine ja käivitage rakendus oma Androidi seadmes või emulaator.

3. Tippige tööülesande rakendus, ja klõpsake plussmärki (**+**) ikoon.

4. Veenduge, et teatis on saanud, kui üksus on lisatud.


##<a name="optional-configure-and-run-the-ios-project"></a>(Valikuline) Konfigureerida ja käivitada iOS-i projekt

See jaotis on mõeldud Xamarin iOS-i projekti iOS-seadmete jaoks. Selles jaotises saate vahele jätta, kui te ei tööta iOS-seadmete.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Teate jaoturi APNs-i konfigureerimine

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Järgmiseks tuleb konfigureerida iOS-i projekti säte Xamarin Studio või Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Tõuketeatiste oma iOS-i rakenduse lisamine

1. **IOS-i** projekt, avage AppDelegate.cs lause **abil** lisada koodi faili ülaosas.

        using Newtonsoft.Json.Linq;

4. Klassi **AppDelegate** on alistamine **RegisteredForRemoteNotifications** sündmuse registreeruma teatised lisamiseks tehke järgmist.

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. **AppDelegate**, lisage järgmised alistamine **DidReceivedRemoteNotification** sündmuseohjuri:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    See meetod tegeleb sissetulevate teadete, kui rakendus töötab.

2. Klassi **AppDelegate** lisamine **FinishedLaunching** meetod järgmine kood: 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    See võimaldab tugi remote teatised ja taotlused push registreerimise.

Rakenduse on värskendatud Tõuketeatiste toetavad.

####<a name="test-push-notifications-in-your-ios-app"></a>IOS-i rakenduse testi tõuketeatised

1. Paremklõpsake iOS-i projekti ja **StartPp projekti**saate määrata.

2. Vajutage **klahvi F5** ja nuppu **Käivita** Visual Studio projekti koostamine ja käivitage rakendus iOS-seadmes ja siis klõpsake nuppu **OK** , et aktsepteerida tõuketeatisi.

    > [AZURE.NOTE] Konkreetselt peab aktsepteerida tõuketeatisi, rakenduse kaudu. Taotluse ilmneb ainult esimene kord, kui rakendus töötab.

3. Tippige tööülesande rakendus, ja klõpsake plussmärki (**+**) ikoon.

4. Veenduge, et teatis on saanud, siis klõpsake nuppu **OK** , et teatise eiramine.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Valikuline) Konfigureerida ja käivitada Windows projektid

See jaotis on mõeldud Xamarin.Forms WinApp ja WinPhone81 projektide Windowsi seadmete jaoks. Need juhised toetavad ka Universal platvormil (UWP) projektid. Selles jaotises saate vahele jätta, kui te ei tööta koos Windowsi jaoks.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registreerige oma Windowsi rakenduse jaoks Tõuketeatiste WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Teate jaoturi WNS konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Tõuketeatiste teie Windowsi rakenduse lisamine

1. Visual Studio, avage Windows projekti **App.xaml.cs** ja lisage **abil** järgmistest.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Asendamine `<your_TodoItemManager_portable_class_namespace>` koos nimeruum portable projekti, mis sisaldab soovitud `TodoItemManager` klassi.
 

2. Lisage App.xaml.cs **InitNotificationsAsync** järgmisel viisil: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    See meetod saab tõuketeatised teatis kanali ja registreerib malli malli teatisi teie teatise keskuse kaudu. Malli teatis, mis toetab *messageParam* toimetatakse see klient.

3. App.xaml.cs, värskendada **OnLaunched** sündmuse sündmuseohjuri meetod määratlus, lisades selle `async` muutja, lisage järgmine rida koodi meetod lõpus: 

        await InitNotificationsAsync();

    See tagab, et tõuketeatised teate registreerimine on loodud või värskendada iga kord, kui rakendus on käivitatud. Oluline on seda teha selleks, et tagada WNS tõuketeatised kanali on alati aktiivne.  

4. Visual Studio Solution Exploreris **Package.appxmanifest** faili avada ja määrake väärtuseks **Jah** jaotises **teatised** **Töölauateatisega toega** .

5. Rakenduse koostamine ja veenduge, et teil on tõrkeid.  Kliendi rakendus peaks nüüd Registreeruge malli teatiste Mobile'i rakendus taustväärtus. Korrake iga Windowsi projekti teie lahendus selles jaotises.


####<a name="test-push-notifications-in-your-windows-app"></a>Windowsi rakenduse testi tõuketeatised

1. Visual Studio, paremklõpsake Windows projekti ja klõpsake käsku **käivitus projekti seadmine**.

2. Nuppu **Käivita** projekti koostamine ja käivitage rakendus.

3. Rakenduses, tippige uus todoitem nimi ja klõpsake plussmärki (**+**) ikooni, et lisada see.

4. Veenduge, et teatis on saanud, kui üksus on lisatud.

##<a name="next-steps"></a>Järgmised sammud

Lugege lisateavet Tõuketeatiste:

* [Tõuketeatised teatis probleemide](../notification-hubs/notification-hubs-push-notification-fixer.md)  
On erinevad põhjused, miks teatised võib saada lähevad või ei jõua seadmetes. Selles teemas näidatakse, kuidas analüüsida ja tõuketeatised teatis tõrkeid põhjuseks välja selgitada. 

On soovitatav jätkuva ühte järgmistest õpetused edasi.

* [Rakenduse lisamiseks autentimine](app-service-mobile-xamarin-forms-get-started-users.md)  
Saate teada, kuidas kasutajad oma rakenduse pakkujaga identiteedi kinnitamiseks.

* [Rakenduse ühenduseta sünkroonimise lubamine](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil ka Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

