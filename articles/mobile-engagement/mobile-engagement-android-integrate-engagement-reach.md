<properties
    pageTitle="Azure'i mobiilsideseadmete kaasamine Android SDK integreerimine"
    description="Uusimad värskendused ja toiminguid Android SDK Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Kuidas integreerida kaasamine REACHi Androidi seadmes

> [AZURE.IMPORTANT] Peate järgima integreerimine toimingut, mis on kirjeldatud, kuidas integreerida kaasamine Androidi dokumendiga enne selle juhendi.

##<a name="standard-integration"></a>Standardse integreerimine

Saavutamiseks SDK nõuab **Androidi tugi teek (v4)**.

Kiireim viis oma projekti **Eclipse** teeki lisamiseks on `Right click on your project -> Android Tools -> Add Support Library...`.

Kui te ei kasuta Eclipse, saate lugeda juhiseid [siin].

Kopeerige REACHi ressurss faile projekti SDK.

-   Kopeerige failid on `res/layout` kausta, mis on esitatud SDK sisse selle `res/layout` kausta rakenduse.
-   Kopeerige failid on `res/drawable` kausta, mis on esitatud SDK sisse selle `res/drawable` kausta rakenduse.

Redigeeri oma `AndroidManifest.xml` faili:

-   Lisage järgmisest jaotisest (vahel on `<application>` ja `</application>` sildid):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Teil on vaja see õigus kordus süsteemi teatisi, mis pole veel klõpsatud käivitamisel (vastasel juhul säilitatakse kettal, kuid ei kuvata enam, peate seda).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Saate määrata teatiste (nii rakenduste ja süsteemi neist) kasutavad kopeerite ja redigeerite järgmisest jaotisest ikooni (vahel on `<application>` ja `</application>` sildid):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Selles jaotises on **kohustuslik** , kui kavatsete kasutada süsteemi teatisi REACHi kampaaniat loomisel. Androidi takistab süsteemi teatisi ilma ikoonid on kuvatud. Nii kui jätate see jaotis, kui teie lõppkasutajad ei saa neid vastu võtta.

-   Kui loote kampaaniat süsteemi teatiste abil üldpildiga, peate lisama järgmisi õigusi (pärast selle `</application>` sildi) kui puuduvad:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Androidi M ja kui rakenduse eesmärgid Androidi API tase 23 või suurem, ``WRITE_EXTERNAL_STORAGE`` õigus nõutud kasutaja kinnitamine. Lugege [järgmise jaotise](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Süsteemi teatiste saate määrata ka REACHi kampaanias kui seade peaks ring ja/või Värin. See toimib, peate veenduge, et teil järgmised õigused deklareeritud (pärast selle `</application>` sildi):

            <uses-permission android:name="android.permission.VIBRATE" />

    See õigus ilma Androidi takistab süsteemi teatisi ei kuvatakse, kui märgitud Helista või värin suvand halduri turunduskampaania saavutamiseks.

-   Kui olete rakenduse abil **ProGuard** koostamine ja teil Androidi toe teegi või kaasamine jar seotud tõrgete, lisada järgmised read, et teie `proguard.cfg` faili:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Kohalikke tõuketeatised

Nüüd, kui olete konfigureerinud REACHi mooduli, peate saama kampaaniat seadme kohalikke PTT konfigureerimine.

Toetame kaks teenused Android:

  - Google Play seadmed: kasutamine [Google Cloud sõnumside] [kohta integreerida GCM kaasamine juhend](mobile-engagement-android-gcm-integrate.md) juhend järgides.
  - Amazon seadmed: kasutamine [Amazon seadme sõnumside] [kohta integreerida ADM kaasamine juhend](mobile-engagement-android-adm-integrate.md) juhend järgides.

Kui soovite suunata nii Amazon ja Google Play seadmed, on võimalik on kõik 1 AndroidManifest.xml/APK arengu sees. Kuid Amazon esitamisel need kõnest rakenduse kui ta ei leia GCM kood.

Sel juhul tuleks kasutada mitut APKs.

**Teie rakendus on nüüd valmis vastu võtta ja kuvada REACHi kampaaniat!**

##<a name="how-to-handle-data-push"></a>Andmete tõuketeatised reageerimine

### <a name="integration"></a>Integreerimine

Kui soovite oma rakenduse REACHi andmete sunnib vastu võtta, peate looma sub klassi `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` ja see on `AndroidManifest.xml` faili (vahel on `<application>` ja/või `</application>` sildid):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Seejärel saate alistada, et `onDataPushStringReceived` ja `onDataPushBase64Received` kontekstiatribuuti. Siin on näide:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategooria

Kategooria parameeter on valikuline, kui loote andmete Push turunduskampaania ja võimaldab teil andmeid filtreerida sunnib. See on kasulik, kui teil on mitu leviedastuse vastuvõtjad töötlemise eri tüüpi andmete sunnib, või kui soovite push erinevaid, `Base64` andmed ja soovite tuvastada sõelumine need enne nende tüüp.

### <a name="callbacks-return-parameter"></a>Kontekstiatribuuti saatja parameeter.

Siin on mõned juhised saatja parameetri õigesti käsitlema `onDataPushStringReceived` ja `onDataPushBase64Received`:

-   Leviedastuse vastuvõtja peaks tagastama `null` tagasihelistamise, kui see ei tea, kuidas hallata andmete tõuketeatised sisse. Kasutage kategooria määratlemaks, kas teie leviedastuse vastuvõtja peaks hakkama andmete tõuketeatised või mitte.
-   Üks leviedastuse vastuvõtja peaks tagastama `true` tagasihelistamise, kui see aktsepteerib andmete tõuketeatised sisse.
-   Üks leviedastuse vastuvõtja peaks tagastama `false` tagasihelistamise, kui see tuvastab andmete tõuketeatised, kuid kustutab selle mingil põhjusel sisse. Näiteks tagastada `false` kui vastuvõetud andmed ei sobi.
-   Kui üks leviedastus vastuvõtja tagastab `true` , samas teise ühte tagastab `false` sama andmete vastavat käitumist on määramata, ei tohi kunagi teha.

Tagastuse tüüp kasutatakse ainult REACHi statistikud on:

-   `Replied`kui üks leviedastuse vastuvõtjad tagastatud ühte muudeta `true` või `false`.
-   `Actioned`suurendatakse ainult juhul, kui üks leviedastuse vastuvõtjad tagasi `true`.

##<a name="how-to-customize-campaigns"></a>Kuidas kohandada kampaaniat

Kampaaniat kohandamiseks saate muuta lähtutud saavutamiseks SDK paigutus.

Teie peaks kõik identifikaatorid, mida kasutatakse paigutused ja hoida vaateid, mida kasutada identifikaatori, eriti tekst, vaadete ja pilt vaadete tüübid. Mõned vaated kasutatakse peitmine või kuvamine alad nii, et nende tüüpi võimalik muuta. Kontrollige lähtekoodi, kui soovite muuta esitatud paigutustes vaate tüüp.

### <a name="notifications"></a>Teatised

On kahte tüüpi teatised: süsteem ja rakenduste teatised, mis erinevate paigutus failide kasutamine.

#### <a name="system-notifications"></a>Süsteemi teated

Kohandada süsteemi teatisi, peate kasutama **Kategooriad**. Saate liikuda [Kategooriad](#categories).

#### <a name="in-app-notifications"></a>Klõpsake rakenduse teatised

Teatis – rakendus on vaikimisi vaade, mis lisatakse dünaamiliselt praeguse tegevuse kasutajaliidese tänu Androidi meetodit `addContentView()`. Seda nimetatakse teatis ülekatet. Teatis ülekatete on suur kiire integreerimine, kuna nad ei vaja, saate oma rakenduse mis tahes paigutuse muutmine.

Teie teatis ülekatete ilme muutmiseks saate muuta lihtsalt faili `engagement_notification_area.xml` oma vajadustele.

> [AZURE.NOTE] Faili `engagement_notification_overlay.xml` on üks, mis saab luua teatise ülekatet, see sisaldab faili `engagement_notification_area.xml`. Saate kohandada seda vastavalt oma vajadustele (nt paigutuse olekualale ülekatte sees).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Kaasa teatis paigutus osana ka tegevuste paigutus

Ülekatete on suurepärane kiire integreerimine, kuid saate olla ebamugav või pool võib mõjutada teatud juhtudel. Ülekatet süsteemi saab kohandada tegevuse tasemel, hõlbustades küljel mõju teisiti tegevuste jaoks.

Saate otsustada, kas kaasata oma olemasolevat paigutust tänu Androidi **kaasata** lause meie teatis paigutus. Järgmine on näide modifitseeritud `ListActivity` paigutus, mis sisaldab ainult mõne `ListView`.

**Enne kaasamine integreerimine:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Pärast kaasamine integreerimine:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

Selles näites me lisada ema container, kuna algne paigutus kasutada loendivaate ülataseme element. Lisasime ka `android:layout_weight="1"` oleks võimalik allpool konfigureeritud loendivaate vaate lisamine `android:layout_height="fill_parent"`.

Kaasamine saavutamiseks SDK automaatselt tuvastab, et teatis paigutuse kaasatakse see tegevus ja selle tegevuse ülekatet ei lisa.

> [AZURE.TIP] Kui kasutate oma rakenduse lisamine ListActivity, nähtav REACHi ülekatet takistavad teil reageeri loendivaate üksusi enam klõpsamisel. See on teadaolev probleem. Selle probleemi lahendamiseks soovitame manustamine teatis paigutus oma loendi tegevuste paigutuse nagu eelmises valimis.

##### <a name="disabling-application-notification-per-activity"></a>Rakenduse teatise kohta tegevuste keelamine

Kui te ei soovi lisada oma tegevuse ülekatet ja ei kaasata oma paigutuse teatis paigutus, saate keelata ülekatet tegevuse jaoks soovitud `AndroidManifest.xml` , lisades on `meta-data` jaotis nagu järgmises näites:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategooriad

Kui muudate esitatud paigutused, muudate ilme kõik teie teatised. Kategooriate abil saate määratleda erinevate suunatud ilmed (võib-olla käitumise) teatiste saamiseks. Kui loote REACHi turunduskampaania saab määrata kategooria. Pidage meeles, et kategooriad ka võimaldavad teil kohandada teadaanded ja küsitlused, mis on kirjeldatud hiljem selles dokumendis.

Kategooria sündmuseohjuri oma teatiste registreerida, peate lisama kõne, kui rakendus on lähtestatud.

> [AZURE.IMPORTANT] Lugege android: protsessi atribuut hoiatust \<Androidi sdk-kaasamine protsessi\> integreerimiseks kaasamine enne jätkamist Androidi teemal, kuidas.

Järgmises näites eeldab tunnistada eelmise hoiatus ning te kasutate sub klassi `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Funktsiooni `MyNotifier` objekt on teatis kategooria sündmuseohjuri rakendamine. Rakendamist, on selle `EngagementNotifier` kasutajaliidese ülem- või rakendamise vaikimisi: `EngagementDefaultNotifier`.

Pange tähele, et teatise esitaja võite hakkama mitu kategooriat, võite need registreerida umbes järgmine:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Vaikimisi kategooria rakendamine asendamiseks saate registreerida oma rakendamist, nagu järgmises näites:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Kasutusel on sündmuseohjuri praeguse kategooria on möödas parameetrina enamiku meetodite saate alistada, klõpsake `EngagementDefaultNotifier`.

See on möödunud, kas on `String` parameeter või kaudselt mõne `EngagementReachContent` objekti, mis on mõne `getCategory()` meetod.

Saate muuta enamik teatis loomisprotsessi uueks meetodite kohta `EngagementDefaultNotifier`, täpsemate kohandamine julgelt dokumentatsioon ja lähtekoodi vaadata.

##### <a name="in-app-notifications"></a>Klõpsake rakenduse teatised

Kui soovite lihtsalt kasutada alternatiivse paigutuste konkreetse kategooria, saate seda rakendada, nagu järgmises näites:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Näide `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Nagu näete, on standardne erinevas ülekatet vaade identifikaator. On oluline, et iga paigutuse kasutamiseks ülekatete kordumatut tunnust.

**Näide `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Nagu näete, on standardne erinevas teatis ala vaate identifikaator. On oluline, et iga paigutus kasutab kordumatu teatis alad.

Lihtne näide selle kategooria muudab rakenduse (või rakendus) teatised, kuvatakse kuva ülaosas. Me ei muutunud kasutatud ise olekualal tuvastatud.

Kui soovite seda muuta, peate uuesti selle `EngagementDefaultNotifier.prepareInAppArea` meetod. Soovitatav on vaadata tehnilise dokumentatsiooni ja lähtekoodi `EngagementNotifier` ja `EngagementDefaultNotifier` kui soovite tase täpsem kohandamine.

##### <a name="system-notifications"></a>Süsteemi teated

Pikendades `EngagementDefaultNotifier`, saate alistada `onNotificationPrepared` Muuda teatise, mille on koostanud vaikimisi rakendamine.

Näiteks:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Selles näites muudab süsteem teatise nimega "poolelioleva" kategooria kasutamisel pidev korral kuvatakse sisu.

Kui soovite luua selle `Notification` objekti algusest peale ise luua, võite tagastada `false` meetodi ja kõne `notify` enda kohta on `NotificationManager`. Sel juhul on oluline, et hoiate on `contentIntent`, on `deleteIntent` ja teatis identifikaator, mis kasutavad `EngagementReachReceiver`.

Siin on õige näide on rakendamise:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Teatis ainult teadaannete

Klõpsake ainult teadaanne saab kohandada alistamine teatise haldamine `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` muutmiseks ettevalmistatud `Intent`. Selle meetodi abil saate lipud hõlpsasti häälestada.

Näiteks lisada soovitud `SINGLE_TOP` lipp:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Pärand lingid kasutajatele, Pange tähele, et süsteemi teatisi ilma toimingu URL-i nüüd käivitab rakenduse kui see on tausta, et seda meetodit saate kutsuda teate ilma toimingu URL-i. Mida peaksite kaaluma kohandamisel soovidele.

Saate rakendada ka `EngagementNotifier.executeNotifAnnouncementAction` algusest peale.

##### <a name="notification-life-cycle"></a>Teatis elutsükkel

Vaikimisi kategooria kasutamisel mõned elutsükli meetodid kutsunud selle `EngagementReachInteractiveContent` objekti aruande statistika ja värskendada turunduskampaania olek:

-   Kui teate kuvamisel rakenduses või olekuribal, pange selle `displayNotification` meetodit nimetatakse (mis aruannete statistika), `EngagementReachAgent` kui `handleNotification` tagastab `true`.
-   Kui teate jätta, kuvatakse `exitNotification` meetodit nimetatakse, statistiline teatatud ja nüüd saab töödelda järgmise kampaaniat.
-   Teate klõpsamisel `actionNotification` on helistanud, on teatatud statistiline ja seotud soovidele käivitatakse.

Kui teie rakendamine `EngagementNotifier` mille liiklus möödub vaikekäitumise, teil helistada ise elutsükli järgmised võimalused. Järgmised näited illustreerivad mõnel juhul, kui vaikekäitumine on mööduda:

-   Te ei laiendada `EngagementDefaultNotifier`, nt saate rakendada kategooria käsitsemise algusest peale ise luua.
-   Süsteemi teatistes, overrode on `onNotificationPrepared` ja muudetud `contentIntent` või `deleteIntent` klõpsake soovitud `Notification` objekti.
-   Rakendusesisese teatistes, overrode `prepareInAppArea`, veenduge, et kaart vähemalt `actionNotification` ühele oma U.I juhtelemendid.

> [AZURE.NOTE] Kui `handleNotification` viskab erandi sisu kustutatakse ja `dropContent` nimetatakse. See on esitatud statistika ja nüüd saab töödelda järgmise kampaaniat.

### <a name="announcements-and-polls"></a>Teadaanded ja küsitluste

#### <a name="layouts"></a>Paigutused

Saate muuta selle `engagement_text_announcement.xml`, `engagement_web_announcement.xml` ja `engagement_poll.xml` kohandada teksti teadaandeid, web teadaanded ja küsitluste failid.

Need failid jagada kaks levinud skeemid tiitli ala ja nupp. Tiitli paigutuse on `engagement_content_title.xml` ja kasutab joonestatav samanimelise faili tausta. Toimingu ja välju nuppude paigutus on `engagement_button_bar.xml` ja kasutab joonestatav samanimelise faili tausta.

Küsitlus, küsimus paigutust ja nende Valikud on dünaamiliselt täidetud abil mitu korda on `engagement_question.xml` paigutuse faili küsimuste ja `engagement_choice.xml` faili Valikud.

#### <a name="categories"></a>Kategooriad

##### <a name="alternate-layouts"></a>Alternatiivsete paigutused

Teatised, nt saab olema alternatiivse paigutuste teadaanded ja küsitluste soovitud turunduskampaania kategooria.

Näiteks teksti teadaanne kategooria loomiseks saate laiendada `EngagementTextAnnouncementActivity` ja see on `AndroidManifest.xml` faili:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Pange tähele, et selle kategooria soovidele filtreerida saab teha tegevusega vaikimisi teadaanne vahe.

Saavutamiseks SDK kasutab tahtlikult süsteemi lahendamiseks õige tegevuse konkreetse kategooria ja see langeb tagasi vaikimisi kategooria kui eraldusvõime nurjus.

Siis teil rakendada `MyCustomTextAnnouncementActivity`, kui soovite lihtsalt paigutuse muutmine (kuid säilitada sama vaade identifikaatorite), ainult peate määratlema klassi nagu järgmises näites:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Vaikimisi kategooria teadaannete teksti asendamiseks lihtsalt asendada `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` oma rakendades.

Web teadaanded ja küsitluste saab kohandada sarnasel viisil.

Web teadaannete laiendamine `EngagementWebAnnouncementActivity` ja deklareerida oma tegevusest selle `AndroidManifest.xml` nagu järgmises näites:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Küsitlused laiendamine `EngagementPollActivity` ja deklareerida oma rakenduses selle `AndroidManifest.xml` nagu järgmises näites:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Rakendamise algusest peale

Saate rakendada kategooriad oma teadaanne (ja küsitluse) tegevuste jaoks ühte laiendamata soovitud `Engagement*Activity` saavutamiseks SDK esitatud tunnid. See on kasu näiteks kui soovite määratleda paigutus, mis ei kasuta sama vaadete standardne paigutus.

Nagu täpsemalt teatis kohandamine, on soovitatav vaadata lähtekoodi standard rakendamist.

Siin on mõned asjad, mida meeles pidada: REACHi käivitab pluss täiendav parameeter, mis on sisu identifikaator tegevuse teatud tahtlikult, (vastav tahtlikult filtreerimine).

Sisu objekti, mis sisaldavad välju, määratud veebisaidil kampaaniat loomisel saate teha seda.

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Statistika, teatage sisu kuvatakse soovitud `onResume` sündmuse:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Seejärel, ärge unustage kõne ühte `actionContent(this)` või `exitContent(this)` sisu objekti enne tegevuse aktiveerub tausta.

Kui te pole kas kõne `actionContent` või `exitContent`, statistika ei saadeta (st pole analytics kampaaniat kohta) ja veelgi olulisem järgmise kampaaniat ei teavitata enne rakenduse protsessi taaskäivitamist.

Suund või muude konfiguratsioonimuudatuste saate teha ka kood keeruline kindlaks teha, kas tegevuse aktiveerub tausta või mitte, standard rakendamist saab kontrollida, kas sisu on teatatud väljunud, kui kasutaja lahkub tegevuse (klahvikombinatsiooni `HOME` või `BACK`), kuid kui paigutus muutub.

Siin on rakendamist huvitav osa:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Nagu näete, kui helistanud `actionContent(this)` hiljem valmis tegevuse, `exitContent(this)` ilma mingit mõju turvaline kutsuda.

[Siin]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud sõnumside]:http://developer.android.com/guide/google/gcm/index.html
[Amazon seadme sõnumside]:https://developer.amazon.com/sdk/adm.html
