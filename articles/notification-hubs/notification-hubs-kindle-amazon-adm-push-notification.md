<properties
    pageTitle="Azure'i teatis jaoturi Kindle rakenduste kasutamise alustamine | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas saata Tõuketeatiste Kindle rakenduste Azure teatis jaoturi abil."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Teatis jaoturi Kindle rakenduste kasutamise alustamine

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas saata Tõuketeatiste Kindle rakenduste Azure teatis jaoturi abil.
Saate luua tühja Kindle rakendus, mis saab Tõuketeatiste Amazon seadme sõnumside (ADM) abil.

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ Saada <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Androidi saidi</a>Androidi SDK (me oletame, et kasutate Eclipse).
+ Järgige <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Säte üles oma arenduskeskkond</a> häälestada oma arenduskeskkond Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Uue rakenduse lisamine portaali arendaja

1. Esmalt looge rakenduse [Amazon arendaja portaalis].

    ![][0]

2. Kopeerige **kiirmenüü klahvi**.

    ![][1]

3. Portaalis, klõpsake rakenduse nime ja seejärel vahekaarti **Seadme sõnumside** .

    ![][2]

4. Klõpsake nuppu **Loo uus profiil turvalisus**ja seejärel looge uus turbeprofiili (nt **TestAdm turbeprofiili**). Klõpsake nuppu **Salvesta**.

    ![][3]

5. Valige äsja loodud turbeprofiili kuvamiseks **Turvalisus profiilid** . Kopeerige **Kliendi ID** ja **Kliendi salajane** väärtused hilisemaks kasutamiseks.

    ![][4]

## <a name="create-an-api-key"></a>Saate luua ka API võti

1. Avage käsuviip administraatori õigustega.
2. Liikuge kausta, Android SDK.
3. Sisestage järgmine käsk:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  **Võtmehoidjas** parooli, tippige **Androidi**.

5.  Kopeerige **MD5** sõrmejälje.
6.  Tagasi valige portaalis arendaja menüü **sõnumid** **Android/Kindle** ja sisestage oma rakenduse (nt **com.sample.notificationhubtest**) ja **MD5** väärtuse paketi nimi ja seejärel klõpsake nuppu **Loo API võti**.

## <a name="add-credentials-to-the-hub"></a>Jaoturi identimisteabe lisamine

Portaalis lisada kliendi salajane ja kliendi ID oma teate jaoturi vahekaarti **konfigureerimine** .

## <a name="set-up-your-application"></a>Rakenduse häälestamine

> [AZURE.NOTE] Rakenduse loomisel kasutada vähemalt API tase 17.

Projekti Eclipse ADM teekide lisamiseks tehke järgmist.

1. Saada ADM teegi [SDK alla laadida]. Ekstraktida SDK zip-fail.
2. Paremklõpsake oma projekti Eclipse, ja seejärel klõpsake käsku **Atribuudid**. Valige **Java koostada tee** vasakul ja valige **teekide **menüü ülaosas. Klõpsake nuppu **Lisa välise Jar**ja valige fail `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` kataloogist, kus ekstraktitud Amazon SDK.
3. Laadige alla NotificationHubs Android SDK (link).
4. Pakkige pakett ja seejärel lohistage fail `notification-hubs-sdk.jar` sisse selle `libs` Eclipse kausta.

Oma rakenduse manifesti toetamiseks ADM redigeerimiseks tehke järgmist.

1. Lisage Amazon nimeruumi manifest juurelemendis.


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Lisage jaotises manifest elemendi esimene element õigused. Paketi, mille lõite rakenduse asendada **[Teie paketi nimi]** .

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Sisestage järgmine element rakenduse elemendi esimene tütarelement. Ärge unustage asendada oma ADM teade sündmuseohjuri loomist (sh paketi) järgmises jaotises nimega **[Teie teenuse nimi]** ja **[Teie paketi nimi]** paketi nimi, mille lõite rakenduse asendamine.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Teie ADM teade sündmuseohjuri loomine

1. Saate luua uue ainekursuse, mis pärib `com.amazon.device.messaging.ADMMessageHandlerBase` ja pange sellele `MyADMMessageHandler`, nagu on näidatud järgmisel joonisel:

    ![][6]

2. Lisage järgmine `import` laused:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Lisage järgmine kood loodud tunni. Ärge unustage asendada jaoturi nimi ja ühenduse string (kuulata):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Lisage järgmine kood on `OnMessage()` meetod:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Lisage järgmine kood on `OnRegistered` meetod:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Lisage järgmine kood on `OnUnregistered` meetod:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. Klõpsake soovitud `MainActivity` meetod lisamine järgmine impordi-lause:

        import com.amazon.device.messaging.ADM;

8. Lisage järgmine kood lõpus olevat `OnCreate` meetod:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Rakenduse lisamiseks oma API võti

1. Eclipse, looge uus fail nimega **api_key.txt** projekti directory vara.
2. Avage fail ja kopeerige API võti teie loodud Amazon arendaja portaalis.

## <a name="run-the-app"></a>Käivitage rakendus

1. Käivitage emulaator.
2. Emulaator, nipsake ja klõpsake nuppu **sätted**, ja seejärel klõpsake valikut **minu konto** ja sobivat Amazon kontot registreerida.
3. Eclipse, käivitage rakendus.

> [AZURE.NOTE] Kui probleem esineb, märkige ruut emulaator (või seadme). Aeg väärtus peab olema täpne. Selleks, et muuta Kindle emulaator, saate Android SDK platform-tööriistade kataloogist käivitage järgmine käsk:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Sõnumi saatmine

Sõnumi saatmiseks .net-i abil:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon arendaja portaal]: https://developer.amazon.com/home.html
[SDK allalaadimine]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
