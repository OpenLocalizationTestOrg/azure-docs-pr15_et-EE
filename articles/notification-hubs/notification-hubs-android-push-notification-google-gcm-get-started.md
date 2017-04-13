<properties
    pageTitle="Tõuketeatiste saata Azure'i teatis jaoturi Android | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas Azure'i teatis jaoturi abil tõuketeatised ja Androidi seadmed."
    services="notification-hubs"
    documentationCenter="android"
    keywords="tõuketeatised, tõuketeatised teatis, android tõuketeatised teatis"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Azure'i teatis jaoturi Android Tõuketeatiste saatmine

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

> [AZURE.IMPORTANT] See teema näitab Tõuketeatiste koos Google Cloud sõnumside (GCM). Kui kasutate Google'i Firebase'i Cloud sõnumside (FCM), lugege teemat [saatmine Tõuketeatiste Android FCM ja Azure teatis koos](notification-hubs-android-push-notification-google-fcm-get-started.md).

Selle õpetuse näidatakse, kuidas saata Tõuketeatiste Androidi rakenduse Azure'i teatis jaoturi abil.
Saate luua tühja Androidi rakendust, mis saab Tõuketeatiste Google Cloud sõnumside (GCM) abil.

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Selle õpetuse lõplikus kood saab alla laadida GitHub [siin](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Eeltingimused

> [AZURE.IMPORTANT] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

Ülalnimetatud aktiivne Azure'i konto, lisaks selles õpetuses ainult nõuab [Androidi Studio](http://go.microsoft.com/fwlink/?LinkId=389797)uusima versiooni.

Selles õpetuses lõpuleviimine on kõik teatis jaoturi õpetused rakendused Androidi jaoks nõutav.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Projekti loomine toetab Google Cloud sõnumside

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Uus teatis jaoturi konfigureerimine


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. tera **sätted** , valige **Teatise teenuste** ja seejärel **Google (GCM)**. Sisestage API võti ja klõpsake nuppu **Salvesta**.

&emsp;&emsp;![Azure'i teatis jaoturi - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Teie teate jaoturi on nüüd konfigureeritud GCM töötamiseks ja teil on ühendusstringi nii registreerida oma rakenduse Tõuketeatiste saatmiseks ja vastuvõtmiseks.

##<a id="connecting-app"></a>Ühenduse loomine rakenduse teate jaoturi

###<a name="create-a-new-android-project"></a>Androidi uue projekti loomine

1. Androidi Studios, käivitage Androidi Studio uue projekti.

    ![Androidi Studio - uue projekti][13]

2. Valige **telefoni ja tahvelarvuti** vormi tegur ja **Vähemalt SDK** toetab soovitud. Klõpsake nuppu **edasi**.

    ![Androidi Studio - projektitöövoos loomine][14]

3. Valige **Tühi tegevuse** peamine tegevuste, klõpsake nuppu **Järgmine**ja seejärel klõpsake nuppu **valmis**.

###<a name="add-google-play-services-to-the-project"></a>Google Play teenuste lisamine projekti

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Azure'i teatis jaoturi teekide lisamine


1. Klõpsake soovitud `Build.Gradle` fail **rakenduse**, lisada järgmised read jaotises **sõltuvused** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Lisage järgmised hoidla pärast **sõltuvused** jaotist.

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Funktsiooni AndroidManifest.xml värskendamine.


1. GCM toetamiseks me rakendama eksemplari ID kuulajale teenuse meie [saada registreerimise sõned](https://developers.google.com/cloud-messaging/android/client#sample-register) abil [Google'i eksemplari ID API](https://developers.google.com/instance-id/)kasutatava koodi. Selles õppetükis me ainekursuse nime `MyInstanceIDService`. 
 
    Lisage järgmised teenuse määratlus AndroidManifest.xml fail, sees on `<application>` silt. Asendada selle `<your package>` kohatäite koos selle oma ülaosas kuvatud tegelik paketi nimi on `AndroidManifest.xml` faili.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Kui oleme saanud meie GCM registreerimise luba eksemplari ID API, kasutame [Azure'i teate jaoturi](notification-hubs-push-notification-registration-management.md)registreerida. Selle registreerimise toetame tausta kasutamisel on `IntentService` nimega `RegistrationIntentService`. See teenus vastutab ka värskendamise [meie GCM registreerimise luba](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
 
    Lisada järgmised teenuse määratluse faili AndroidManifest.xml sees on `<application>` silt. Asendada selle `<your package>` kohatäite koos selle oma ülaosas kuvatud tegelik paketi nimi on `AndroidManifest.xml` faili. 

        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>



3. Me määratleda ka vastuvõtja, kui soovite saada teatisi. Lisada järgmised vastuvõtja määratluse faili AndroidManifest.xml sees on `<application>` silt. Asendada selle `<your package>` kohatäite koos selle oma ülaosas kuvatud tegelik paketi nimi on `AndroidManifest.xml` faili.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Lisage järgmine vajalikud GCM seotud õiguste allpool olevat `</application>` silt. Veenduge, et asendada `<your package>` paketi nimi kuvatakse ülaosas olevat `AndroidManifest.xml` faili.

    Nende õiguste kohta leiate lisateavet teemast [GCM kliendi app for Android häälestamine](https://developers.google.com/cloud-messaging/android/client#manifest).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Koodi lisamine


1. Laiendage projekti vaates **rakenduse** > **src** > **peamine** > **java**. Paremklõpsake kausta paketi **java**all, klõpsake nuppu **Uus**ja klõpsake **Java klassi**. Uue nimega klassi `NotificationSettings`. 

    ![Androidi Studio - uutel Java][6]

    Veenduge, et need kolm värskendamiseks kohatäidete järgmine kood on `NotificationSettings` klassi:
    * **SenderId**: hankisite eelnevalt kirjeldatud [Google Cloud konsooli](http://cloud.google.com/console)projekti number.
    * **HubListenConnectionString**: jaoks oma jaoturi **DefaultListenAccessSignature** ühendusstring. Saate kopeerida selle ühendusstring, klõpsates **Juurdepääsupoliitikaid** **sätted** enne oma keskuse [Azure portaali].
    * **HubName**: Kasutage oma teate jaoturi jaoturi tera [Azure portaali]kuvatava nime.

    `NotificationSettings`kood:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Kasutades eeltoodud juhiseid lisada teise uutel nimega `MyInstanceIDService`. See on meie eksemplari ID kuulajale teenuse rakendamiseks.

    See tund kood helistavad koosolekule meie `IntentService` [värskendamine GCM luba](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) taustal.

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Lisada mõne muu uuest ainekursuse nime, projekti `RegistrationIntentService`. Sellest saab rakendada meie `IntentService` mis tegeleb [värskendamine GCM luba](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ja [teate jaoturi registreerumist](notification-hubs-push-notification-registration-management.md).

    Kasutage järgmine kood see tund.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {      
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. Klõpsake oma `MainActivity` klassi, lisage järgmine `import` laused klassi deklaratsiooni kohal.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Lisage järgmised era liikmed ainekursuse ülaosas. Kasutame neid [Google Play teenuste soovitatud Google olemasolu kontrollida](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. Klõpsake oma `MainActivity` klassi, Google Play teenuste lisamine järgmisel viisil. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. Klõpsake oma `MainActivity` klassi, lisage järgmine kood, mis kontrollib Google Play teenuseid enne helistaja oma `IntentService` saada registreerimise GCM Turbeloa ja Registreerige oma teate jaoturi.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. Klõpsake soovitud `OnCreate` meetod on `MainActivity` klassi, lisage järgmine kood registreerimise alustamiseks tegevuse loomisel.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Lisage need täiendavad meetodid, et selle `MainActivity` rakenduse oleku ja aruande olek rakenduse kinnitamiseks.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. Funktsiooni `ToastNotify` meetod kasutab *"Tere, maailm"* `TextView` juhtelement aruande olek ja püsivalt rakenduses teatised. Lisage oma activity_main.xml paigutus juhtelemendi järgmist id.

        android:id="@+id/text_hello"

11. Järgmine lisame meie vastuvõtja me määratletud funktsiooni AndroidManifest.xml alamklass. Lisada mõne muu uutel nimega projekti `MyHandler`.

12. Import järgmistest lisamine ülaosas `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Lisage järgmine kood on `MyHandler` klassi alamklass, muutes `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Järgmine kood alistab selle `OnReceive` meetod, et selle sündmuseohjuri aruande teatised, mille olete saanud. Selle sündmuseohjuri saadab ka tõuketeatised teate juhatajale Androidi teatis, kasutades funktsiooni `sendNotification()` meetod. Funktsiooni `sendNotification()` meetod käivitatakse rakendus ei tööta ja saab teate.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Androidi Studio menüüribal nuppu **koostada** > **Taasloomine projekti** veendumaks, et tõrkeid esinevad koodi.

##<a name="sending-push-notifications"></a>Tõuketeatiste saatmine

Saate testida vastuvõtmine Tõuketeatiste rakenduse saates neile [Azure portaali] – vaadake **tõrkeotsingu** jaotisele labale jaoturi kaudu, nagu allpool näidatud.

![Azure'i teatis jaoturi - testi saatmine](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Valikuline) Tõuketeatiste saatmine otse rakenduse kaudu

Tavaliselt soovite saada teatised kirjutamata serveri kasutamine. Mõnel juhul võite saata Tõuketeatiste otse klientrakendust. Selles jaotises selgitatakse, kuidas saata teateid kliendi [Azure'i teatis jaoturi REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx)abil.

1. Androidi Studio Projecti vaade, laiendage **rakenduse** > **src** > **peamine** > **res** > **paigutus**. Avage soovitud `activity_main.xml` paigutus fail ja klõpsake nuppu **teksti** tabeldusklahv (tab) faili sisu teksti värskendamine. Värskendage seda koodiga allpool, mis lisab uusi `Button` ja `EditText` tõuketeatised teatis sõnumeid saata teate jaoturi juhtelemente. Lisage see kood alt, just enne `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Androidi Studio projekti vaade, laiendage **rakenduse** > **src** > **peamine** > **res** > **väärtused**. Avage soovitud `strings.xml` faili ja lisada stringi väärtuse, millele viidatakse uue `Button` ja `EditText` juhtelemendid. Lisage selle faili allosas just enne `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. Klõpsake oma `NotificationSetting.java` faili, lisage järgmised sätte, et selle `NotificationSettings` klassi.

    Värskenduse `HubFullAccess` **DefaultFullSharedAccessSignature** ühendusstringi jaoks oma jaoturi abil. See ühendusstringi saab kopeerida [Azure portaali] , klõpsates **Juurdepääsupoliitikaid** **sätted** enne oma teate jaoturi jaoks.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

4. Klõpsake oma `MainActivity.java` faili, lisage järgmine `import` laused ülaltoodud soovitud `MainActivity` klassi.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. Klõpsake oma `MainActivity.java` faili järgmised liikmete lisamine ülaosas, on `MainActivity` klassi.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Peate looma autentida postituse taotluse sõnumeid saata oma teate jaoturi märgiks tarkvara juurdepääsu allkirja (SaS). Selleks sõelumine ühendusstringi olulisi andmeid ja seejärel luua SaS luba, nimetatud [Levinud põhimõtet](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API viide. Järgmine kood on näide rakendamist.

    Rakenduses `MainActivity.java`, lisage järgmised meetodi abil soovitud `MainActivity` klassi sõeluda oma ühendusstring.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. Rakenduses `MainActivity.java`, lisage järgmised meetodi abil soovitud `MainActivity` klassi luua SaS autentimise luba.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. Rakenduses `MainActivity.java`, lisage järgmised meetodi abil soovitud `MainActivity` ainekursuse, klõpsake nuppu **Saada teatis** ja saata tõuketeatised teavitussõnumi jaoturi abil sisseehitatud REST API-ga.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Rakenduse testimine

####<a name="push-notifications-in-the-emulator"></a>Emulaator tõuketeatised

Kui soovite testida Tõuketeatiste emulaator sees, veenduge, et emulaator pilt toetab Google API taset, mille valisite oma rakenduse. Kui pilti ei toeta kohalikke Google API-d, saate lõpuks on **teenuse\_ei\_saadaval** erand.

Lisaks veenduge, et olete lisanud Google'i konto **sätted**jaotises teie töötava emulaator > **kontod**. Muul juhul oma katsete registreerima GCM võib kaasa tuua soovitud **autentimine\_NURJUNUD** erand.

####<a name="running-the-application"></a>Rakendus töötab

1. Käivitage rakendus ja pange tähele, et registreerimise ID teatatakse edukaks registreerimist.

    ![Android - kanali registreerimise testimine][18]

2. Sisestage teade, mis saadetakse kõigile Android seadmeid, mis on registreeritud jaoturi.

    ![Android - sõnumi saatmist testimine][19]

3. Vajutage **teatise saatmine**. Kuvatakse kõik seadmed on rakendus töötab ka `AlertDialog` tõuketeatised teavitussõnumi eksemplariga. Seadmetes, mis pole rakendus töötab, kuid varem registreeritud Tõuketeatiste saadetakse teile teatis Androidi teatis haldur. Need saab vaadata nipsates allapoole ülemise vasakpoolse nurga.

    ![Android - teatised testimine][21]

##<a name="next-steps"></a>Järgmised sammud

Soovitame [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele] õpetuse järgmise juhise juurde. See näitab teile, kuidas saata teatised ASP.net-i taustväärtus, mis siltide abil saate suunata kindlatele kasutajatele.

Kui soovite segmendi kasutajate poolt huvirühmade, lugege teemat [Kasutamine teatis jaoturi saata uudiseid] õpetuse.

Üldine lisateave teatis jaoturi leiate teemast meie [Teatis jaoturi juhised].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Teatis jaoturi juhised]: http://msdn.microsoft.com/library/jj927170.aspx
[Tõuketeatiste kasutajatele teatis jaoturi abil]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Teatis jaoturi abil saate saata uudiseid]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure'i portaal]: https://portal.azure.com
