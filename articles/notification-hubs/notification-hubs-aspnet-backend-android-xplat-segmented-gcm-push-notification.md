<properties
    pageTitle="Teatis jaoturi katkestamine Kuueosalisest - Uudised Android"
    description="Saate teada, kuidas saata server uudiste teatised Androidi seadmete Azure'i teenus siini teatis jaoturi abil."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Uudiseid saata teate jaoturi abil

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Ülevaade

Selles teemas näidatakse, kuidas Azure'i teatis jaoturi abil leviedastuse server uudiste teatiste Androidi rakenduse abil. Kui olete lõpetanud, kuvatakse server Uudiste kategooriad olete huvitatud registreerida ja vastu võtta ainult Tõuketeatiste kategooriate. Selle stsenaariumi on levinud mustri palju rakendused, kus on teatised saadetakse rühmad kasutajate, mis on varem huvi nende, nt RSS-lugeja, rakendused muusika ventilaatoreid jne.

Leviedastuse stsenaariumid on lubatud, sh ühe või mitme _siltide_ loomisel registreerimise teate jaoturi. Kui sildi, saadetakse teatised, kuvatakse kõigis seadmetes, mis on registreeritud sildi jaoks teatist. Kuna sildid on lihtsalt stringid, ei pea neid ette valmistada ette. Siltide kohta lisateabe saamiseks vaadake [teatis jaoturi marsruutimist ja sildi avaldised](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Eeltingimused

Selles teemas koostab loodud [teatis jaoturi alustamine]rakenduse[get-started]. Selles õpetuses enne olema juba lõpetanud [Alustamine teatis jaoturi][get-started].

##<a name="add-category-selection-to-the-app"></a>Kategooria valik lisamine rakendusse

Esimene samm on Kasutajaliidese elementidele lisada oma olemasoleva peamised tegevusele, mis võimaldavad kasutajal valida kategooriate registreerida. Seadmes on talletatud kasutaja poolt valitud kategooriad. Rakenduse käivitamisel luuakse seadme registreerimine oma teatise keskuses koos valitud kategooria sildid.

1. Avage res/layout/activity_main.xml fail ja asendada sisu järgmist:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Avage fail res/values/strings.xml ja lisada järgmised read:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Teie main_activity.xml graafiline paigutus peaks olema nüüd selline:

    ![][A1]

3. Nüüd luua sama nimega **MainActivity** tunni pakkimine ainekursuse **teatised** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    See tund kasutab kohalik salvestusruum uudiseid, mis on selle seadme kategooriates talletamiseks. See sisaldab ka viise nende kategooriate registreerida.


4. **MainActivity** tunni eemaldamine oma isiklike väljade **NotificationHub** ja **GoogleCloudMessaging**ja **teatiste**välja lisamine:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Seejärel **onCreate** meetodit, eemaldage lähtestamine **jaoturi** välja ja **registerWithNotificationHubs** meetod. Lisage järgmised read, mille lähtestada **teatised** klassi eksemplar. 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`ja `HubListenConnectionString` juba seadma koos selle `<hub name>` ja `<connection string with listen access>` oma teatise jaoturi nimi ja ühendusstringi *DefaultListenSharedAccessSignature* varem saadava jaoks kohatäited.

    > [AZURE.NOTE] Kuna identimisteavet, mida levitatakse kliendi rakendus pole üldjuhul turvaline, peaks oma kliendi rakendusega jaotada kuulata juurdepääsu võti. Rakenduse teatised, kuid olemasolevate registreerimise registreerida ei saa muuta access võimaldab kuulata ja teatised ei saa saata. Täielik juurdepääs kasutatakse turvalise kirjutamata teenuses saatmise teatised ja muuta olemasolevaid registreerimise.


6. Lisage järgmised impordi ja `subscribe` meetod nuppu Telli käsitlema klõpsake sündmuse:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    See meetod loob kategooriate loendit ja kasutab **teatised** klassi kohalik salvestusruum loendi salvestada ja teie teate jaoturi vastavate siltide registreerida. Kategooriate muutmisel uuesti registreerimise luua uusi kategooriaid.

Teie rakendus on nüüd võimalik salvestada kategooria kohalik salvestamine seade ja registreerida jaoturi teatis iga kord, kui kasutaja muudab valikut kategooriad.

##<a name="register-for-notifications"></a>Teatiste register

Teate jaoturi käivitamisel on talletatud kohalik salvestusruumi kategooriate kasutamist registreerida järgmist.

> [AZURE.NOTE] Kuna atribuutide, registrationId määratud, Google Cloud sõnumside (GCM) saate igal ajal muuta, peaksite Registreeruge teatised tihti, et vältida tõrkeid teatis. Selles näites registreerib teavitamise iga kord, kui rakendus käivitub. Sageli töötavad rakendused, rohkem kui üks kord päevas, saate ilmselt vahele jätta registreerimise säilitada läbilaskevõime kui vähem kui päev on möödunud eelmise registreerimise.


1. Lisage järgmine kood **onCreate** meetodit **MainActivity** tunni lõpus.

        notifications.subscribeToCategories(notifications.retrieveCategories());

    See muudab veenduge, et iga kord, kui rakendus käivitub see toob kategooriad kohalikku ja taotleb registeration, nende kategooriate. 

2. Seejärel värskendage selle `onStart()` meetod on `MainActivity` klassi järgmiselt:

    @Overridekaitstud kehtetu onStart() {super.onStart();      isVisible = true;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    See värskendab peamine tegevus varem salvestatud kategooriaid oleku põhjal.

Rakendus on nüüd valmis ja saate talletada kategooria seadme registreerimiseks jaoturi teatis iga kord, kui kasutaja muudab valikut kategooriad kasutatav kohalik salvestusruum. Järgmiseks me määratleb taustväärtus, mis saavad saata selle rakenduse kategooria teatised.

##<a name="sending-tagged-notifications"></a>Sildistatud teatiste saatmine

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Käivitage rakendus ja luua teatised

1. Androidi Studios, koostada rakendus ja käivitage see seadme või emulaator.

    Teate, et rakenduse kasutajaliides annab lülitab kogum, mille abil saate valida tellida kategooriad.

2. Ühe või mitme kategooriad lülitab lubamine ja seejärel klõpsake nuppu **Telli**.

    Rakenduse valitud kategooria teisendab sildid ja taotleb uue seadme registreerimine jaoks siltidega teatise keskuse kaudu. Registreeritud kategooriad on tagastatud ning kuvatakse töölauateatis teatis.

4. Saatke uus teatis .net-i konsooli rakendus töötab.  Teise võimalusena saate saata sildistatud malli teatised, kasutades teie teate jaoturi silumine vahekaarti [Azure klassikaline portaali].

    Teatiste valitud kategooria kuvatakse töölauateatisega teatised.

##<a name="next-steps"></a>Järgmised sammud

Selles õppetükis me õppinud, kuidas edastada uudiseid kategooria järgi. Võtke arvesse, täites ühte järgmised õppetükid, mis täpsemalt teatis jaoturi stsenaariumide esile tõsta.

+ [Teatis jaoturi abil lokaliseeritud uudiseid leviedastus]

    Saate teada, kuidas laiendada Server Uudised rakenduse lubamiseks saatmise lokaliseeritud teatised.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Teatis jaoturi abil lokaliseeritud uudiseid leviedastus]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure'i klassikaline portaal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
