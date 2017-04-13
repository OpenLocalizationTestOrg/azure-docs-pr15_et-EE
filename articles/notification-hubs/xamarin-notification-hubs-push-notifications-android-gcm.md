<properties
    pageTitle="Alustamine teatis jaoturi Xamarin.Android rakenduste | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas saata Tõuketeatiste Xamarin Androidi rakenduse Azure'i teatis jaoturi abil."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Teatis jaoturi abil Xamarin Android kasutamise alustamine

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas saata Tõuketeatiste Xamarin.Android rakenduse Azure'i teatis jaoturi abil.
Saate luua tühja Xamarin.Android rakendus, mis saab Tõuketeatiste Google Cloud sõnumside (GCM) abil. Kui olete lõpetanud, saate küll leviedastus Tõuketeatiste töötavad rakenduse seadmed teie teate jaoturi abil. Viimistletud kood on saadaval [NotificationHubs rakenduse] [ GitHub] valimi.

Selle õpetuse näitab lihtsa leviedastuse stsenaarium teatis jaoturi abil sisse.


## <a name="before-you-begin"></a>Enne alustamist

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Selles õpetuses lõplikus kood github leiate [siit](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ Visual Studio abil Xamarin Windows või Xamarin Studio on Mac OS x täieliku installimisjuhised on [häälestamise](https://msdn.microsoft.com/library/mt613162.aspx)ja installige Visual Studio ja Xamarin.
+ Aktiivse Google konto
+ [Azure'i sõnumside komponent]
+ [Google Cloud sõnumside kliendi komponent]

Selle õpetuse lõpuleviimine on nõutav kõik teatis jaoturi õpetused Xamarin.Android rakendused.

> [AZURE.IMPORTANT] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Google Cloud sõnumside lubamine

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Teie teate jaoturi konfigureerimine

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Ülaosas vahekaarti <b>konfigureerimine</b> , sisestage hankisite eelmises jaotises <b>API võti</b> väärtus ja klõpsake siis nuppu <b>Salvesta</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Teie teate jaoturi on nüüd konfigureeritud GCM töötamiseks ja teil on ühendusstringi nii rakenduse teadete vastuvõtmiseks ja saatmiseks tõuketeatisi registreerida.

##<a name="connect-your-app-to-the-notification-hub"></a>Teate jaoturi rakenduse ühendamine

###<a name="create-a-new-project"></a>Uue projekti loomine

1. Xamarin Studios, klõpsake nuppu **Uus lahendus**nuppu **Androidi rakendust**, ja klõpsake nuppu **edasi**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Sisestage oma **Rakenduse nimi** ja **identifikaator**. Klõpsake **Target Plaforms** soovite toetavad ja seejärel klõpsake nuppu **edasi** ja **loomine**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    See loob uue Androidi projekti.

2. Projekti atribuutide avamiseks paremklõpsake lahenduse vaates uue projekti ja klõpsake käsku **Suvandid**. **Androidi rakenduse** üksuse valimine jaotises **koostamine** .

    Veenduge, et teie **paketi nime** algustäht on väiketähtedeks.

    > [AZURE.IMPORTANT] Esimene täht paketi nimi peab olema väiketähtedeks. Muul juhul saate rakenduse ilmsetele kui registreerute oma **BroadcastReceiver** ja **IntentFilter** Tõuketeatiste allpool.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Soovi korral saate määrata **miinimum Androidi versioon** tasandile API.

4. Soovi korral võite määratud **Target Androidi versioon** API mõnda muud versiooni, mida soovite suunata (peab olema API tase 8 või uuem versioon).

Klõpsake nuppu **OK** ja sulgege dialoogiboks projekti suvandid.


###<a name="add-the-required-components-to-your-project"></a>Nõutavad komponendid lisamine projekti

Google Cloud sõnumside kliendi saadaval Xamarin komponendi poes toetada Tõuketeatiste Xamarin.Android saiti.

1. Paremklõpsake Xamarin.Android rakenduses komponendid kausta ja valige **Saada lisateavet komponendid**.

2. **Azure'i sõnumside** komponendi otsimine ja lisamine projekti.

3. **Google Cloud sõnumside kliendi** komponent otsimine ja lisamine projekti.


###<a name="set-up-notification-hubs-in-your-project"></a>Teatis jaoturi projekti häälestamine

1. Otsige oma Androidi rakenduse ja teavitusi jaoturi järgmist teavet:

    - **GoogleProjectNumber**: selle projektinumber väärtuse toomine teie rakendus Google arendaja portaalis ülevaade. Kui lõite rakenduse portaalis tehtud märkme selle väärtuse varasemas versioonis.
    - **Kuulake ühendusstringi**: [Azure'i klassikaline portaali]armatuurlaual nuppu **Kuva ühendusstringi**. Kopeerige ühendusstring *DefaultListenSharedAccessSignature* selle väärtuse jaoks.
    - **Jaoturi nimi**: see on teie jaoturi [Azure klassikaline portaali]nimi. Näiteks *mynotificationhub2*.

    Luua oma Xamarin projekti **Constants.cs** klassi ja määratleb ainekursuse konstandi järgmised väärtused. Kohatäidete asendamine oma väärtustega.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Lisage järgmine lauseid **MainActivity.cs**abil:

        using Android.Util;
        using Gcm.Client;

3. Lisage eksemplari muutujat soovitud `MainActivity` ainekursuse, kui rakendus töötab ka teatiste dialoogiboks kuvamiseks kasutatud:

        public static MainActivity instance;


3. Luua **MainActivity** tunni järgmisel viisil:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. Klõpsake soovitud `OnCreate` meetodit **MainActivity.cs**, lähtestamine on `instance` muutuja ja kõne lisamine `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Saate luua uue ainekursuse, **MyBroadcastReceiver**.

    > [AZURE.NOTE] Me juhendab **BroadcastReceiver** klassi loomine algusest peale allpool. Kiirülevaate asemel **MyBroadcastReceiver.cs** käsitsi loomiseks vaadake leitud proovi Xamarin.Android projekti kaasas [NotificationHubs näidised] **GcmService.cs** faili on siiski[GitHub]. Dubleerida **GcmService.cs** ja muuta ainekursuse nime saab alustada ka hea koht.

5. Lisage järgmine abil lauseid **MyBroadcastReceiver.cs** (viidates osa ja komplekti, mis varem lisatud):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. **MyBroadcastReceiver.cs**, lisage järgmised luba kutsed **abil** ja **nimeruumi** deklaratsioon:

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. **MyBroadcastReceiver.cs**, muutke **MyBroadcastReceiver** klassi vastavaks järgmist:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Lisage teise klassi **MyBroadcastReceiver.cs** nimega **PushHandlerService**, mis tuleneb **GcmServiceBase**. Veenduge, et rakendada **soovitud atribuuti** ainekursust.

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** rakendab **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**ja **OnError()**. Meie **PushHandlerService** rakendamist klassi peab alistada nende meetodite ja suheldes teate jaoturi vastuseks tule järgmised võimalused.


9. **OnRegistered()** meetod **PushHandlerService** alistada, kasutades järgmine kood:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] Ülaltoodud **OnRegistered()** kood tuleks Märkus võimalus määrata siltide teatud sõnumside kanalite registreerida.

10. **OnMessage** meetod **PushHandlerService** alistada, kasutades järgmine kood:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Teavitades kasutajaid, kui saab teate lisamine **PushHandlerService** **createNotification** ja **dialogNotify** järgmistest meetoditest.

    >[AZURE.NOTE] Teatise kujundus Androidi versioon 5.0 või uuem versioon tähistab, et varasemate versioonide märkimisväärne erinevus. Kui te seda kontrollida Android 5.0 või uuem versioon, rakendus on vaja teavitada töötama. Lisateavet leiate teemast [Androidi teatised](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Abstraktsed liikmete **OnUnRegistered()**ja **OnRecoverableError()** **OnError()** alistada, nii, et teie kood kogub:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Käivitage rakendus emulaator

Kui käivitate selle rakenduse emulaator, veenduge, et kasutada mõne Androidi virtuaalse seadme (AVD), mis toetab Google API-d.

> [AZURE.IMPORTANT] Selleks et saada Tõuketeatiste, peab häälestamine Google'i konto virtuaalse Androidi seadmesse. (Emulaator, liikuge **sätted** ja klõpsake nuppu **Konto lisamine**.) Samuti veenduge, et emulaator on ühendatud Interneti-ühendus.

>[AZURE.NOTE] Teatise kujundus Androidi versioon 5.0 või uuem versioon tähistab, et varasemate versioonide märkimisväärne erinevus. Lisateavet leiate teemast [Androidi teatised](http://go.microsoft.com/fwlink/?LinkId=615880).


1. **Tööriistad**, klõpsake nuppu **Ava Android emulaator halduri**, valige oma seade ja seejärel klõpsake nuppu **Redigeeri**.

    ![][18]

2. Valige **Target** **Google API -d** ja seejärel klõpsake nuppu **OK**.

    ![][19]

3. Klõpsake ülemisel tööriistaribal nuppu **Käivita**ja seejärel valige oma rakendus. See käivitab emulaator ja rakendus töötab.

  Rakenduse toob *atribuutide registrationId* GCM ja registreerib teate jaoturi.

##<a name="send-notifications-from-your-backend"></a>Teie taustväärtus teatiste saatmine


Saate testida teatiste rakenduse saatmisega teatised [Azure klassikaline portaali] kaudu teatis keskuses silumine menüü nagu on näidatud allpool ekraani.

![][30]


Tõuketeatiste saadetakse tavaliselt kirjutamata teenuse nt Mobile teenuste või ASP.net-i ühilduvad teegi kaudu. Saate REST API otse saatmiseks teated, kui teegis pole saadaval teie taustväärtus.

Siit leiate loendi mõne muu õpetusi, mis võib-olla soovite läbi vaadata saatmise teatised:

- ASP.net-i: Vt [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele].
- Azure'i teatis jaoturi Java SDK: vaadake [teatis jaoturi kaudu Java kasutamise kohta](notification-hubs-java-push-notification-tutorial.md) teatiste saatmine Java. See on testitud Eclipse Androidi arengu.
- PHP: Vaadake, [Kuidas kasutada teatis jaoturi kaudu PHP](notification-hubs-php-push-notification-tutorial.md).


Õpetuse järgmise lõikes saadetud teatiste kasutades .net-i konsooli rakenduse ja mobiilsideteenuse ülevaade sõlm skripti kaudu abil.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Valikuline) Saada teatisi .net-i rakenduse kasutamine

Selles jaotises saadame teatised .net-i konsooli rakenduse kasutamine

1. Loo uus konsool rakendus Visual C#:

    ![][20]

2. Visual Studios, klõpsake nuppu **Tööriistad**, klõpsake **Nugeti Package Manager**ja klõpsake **Package Manager konsooli**.

    Visual Studio kuvatakse Package Manager konsooli.

3. Paketi Manager konsooli aknas määrata **vaikimisi projekti** uue projekti konsooli rakendus ja seejärel konsooli aken, käivitage järgmine käsk:

        Install-Package Microsoft.Azure.NotificationHubs

    Sellega lisatakse Azure'i teatis jaoturi SDK abil <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification jaoturi Nugeti pakett</a>viide.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Avage fail Program.cs ja lisage järgmine `using` lause:

        using Microsoft.Azure.NotificationHubs;

5. Klõpsake oma `Program` klassi, lisada järgmisel viisil. Värskendage kohatäitetekst oma *DefaultFullSharedAccessSignature* ühenduse string ja keskuse nimi [Azure klassikaline portaali].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Lisage järgmised read oma **Main** meetodit.

         SendNotificationAsync();
         Console.ReadLine();

7. Rakenduse käivitamiseks vajutage klahvi F5. Rakenduse saama teatise.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Valikuline) Saada teatisi abil mobiilsideteenuse ülevaade

1. Järgige [Alustamine Mobile teenused].

1. [Azure'i klassikaline portaali]sisse ja valige oma mobiilsideteenuse.

2. Valige vahekaart **ajasti** peal.

    ![][22]

3. Looge uus ajastatud töö nime ja valige **nõudmisel**.

    ![][23]

4. Kui töö on loodud, klõpsake töö nime. Seejärel klõpsake vahekaarti **skripti** ülariba.

5. Sisestage järgmine skript oma Scheduleri funktsiooni sees. Veenduge, et kohatäidete asendamine oma teatise jaoturi nimi ja ühendusstringi *DefaultFullSharedAccessSignature* , mida te saada varasemas versioonis. Klõpsake nuppu **Salvesta**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Klõpsake allosas oleval ribal nuppu **Käivita üks kord** . Peaksite nägema töölauateatis teatis.

##<a name="next-steps"></a>Järgmised sammud

Selles näites lihtsa eetris teatised ja Androidi seadmed. Selleks, et suunata kindlatele kasutajatele, vaadake õpetuse [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele]. Kui soovite segmendi kasutajate poolt huvirühmade, saate lugeda [Kasutamine teatis jaoturi uudiseid saata]. Lisateave kasutamise teatis jaoturi [Teatis jaoturi juhised] ja selle [Teate jaoturi juhiseid Androidi jaoks].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Mobile teenuste kasutamise alustamine]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Teatis jaoturi juhised]: http://msdn.microsoft.com/library/jj927170.aspx
[Teatis jaoturi õpetused Androidi jaoks]: http://msdn.microsoft.com/library/dn282661.aspx

[Tõuketeatiste kasutajatele teatis jaoturi abil]: /manage/services/notification-hubs/notify-users-aspnet
[Uudiseid saata teate jaoturi abil]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud sõnumside kliendi komponent]: http://components.xamarin.com/view/GCMClient/
[Azure'i sõnumside komponent]: http://components.xamarin.com/view/azure-messaging
