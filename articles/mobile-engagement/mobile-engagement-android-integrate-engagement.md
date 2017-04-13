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

#<a name="how-to-integrate-engagement-on-android"></a>Kuidas integreerida kaasamine Androidi seadmes

> [AZURE.SELECTOR]
- [Windowsi universaalne](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Siin kirjeldatakse lihtsaim viis suhete 's analüüsi- ja jälgida oma Androidi rakenduse funktsioonide aktiveerimine.

> [AZURE.IMPORTANT] Teie Androidi SDK API tuleb vähemalt 10 või uuem versioon (Android 2.3.3 või uuem versioon).

Järgmised toimingud on piisavalt aktiveerib arvutada statistikat kõik kasutajad, seansid, tegevused, jookseb ja Technicals vaja logid aruanne. Aruande arvutada teiste statistikute reguleerimist sündmusi, tõrgete ja tööd, nagu on vaja logid tuleb teha käsitsi kaasamine API abil (vt [Täpsemalt Mobile kaasamine sildistamine API oma Android kasutamise kohta](mobile-engagement-android-use-engagement-api.md) , kuna statistika on sõltuva.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Androidi projekti kaasamine SDK ja teenuse embed

Saate alla laadida Android SDK [siin](https://aka.ms/vq9mfn) toomine `mobile-engagement-VERSION.jar` ja paigutada need on `libs` kausta Androidi projekti (kena tegevust jälle kausta loomine, kui see pole veel olemas).

> [AZURE.IMPORTANT]
> Kui koostate oma rakendusepaketi koos ProGuard, peate säilitada mõned tunnid. Saate teha järgmist konfiguratsiooni koodilõigu.
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Järgmise meetodi helistades käivitit tegevuse oma kaasamine ühendusstringi määramiseks tehke järgmist.

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Ühendusstringi rakenduse jaoks kuvatakse Azure portaali.

-   Kui puudu, lisage järgmised Androidi õigused (enne selle `<application>` sildi):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Lisage järgmisest jaotisest (vahel on `<application>` ja `</application>` sildid):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Muuda `<Your application name>` rakenduse nime järgi.

> [AZURE.TIP] Funktsiooni `android:label` atribuut võimaldab teil valida kaasamine teenuse nimi, nagu see kuvatakse lõppkasutajatele "Töötab teenuste" kuval oma telefoni. Soovitatav on seatud atribuut `"<Your application name>Service"` (nt `"AcmeFunGameService"`).

Määrab selle `android:process` atribuut tagab, et kaasamine teenuse kestab eraldi protsessi (töötab kaasamine samasugune toiming nagu rakenduse muuta oma põhi-/ UI lõime vähem potentsiaalselt tundlik).

> [AZURE.NOTE] Mis tahes koodi asetate `Application.onCreate()` ja muude rakenduste kontekstiatribuuti käivitavad teie taotlus protsesside, sh kaasamine teenuse jaoks. See võib olla tagajärgi (nt mittevajaliku mälu jaotus ja selle kaasamine protsessi dubleeritud leviedastuse vastuvõtjad või teenuste Teemad).

Kui te alistada `Application.onCreate()`, on soovitatav lisada järgmine koodilõigu alguses oma `Application.onCreate()` funktsioon:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Sama saate teha `Application.onTerminate()`, `Application.onLowMemory()` ja `Application.onConfigurationChanged(...)`.

Saate laiendada `EngagementApplication` laiendamise asemel `Application`: selle tagasihelistamise `Application.onCreate()` ei kontrolli protsess ja kõnede `Application.onApplicationProcessCreate()` ainult juhul, kui praeguse pole see majutusteenuses kaasamine, samu reegleid rakendamine muude kontekstiatribuuti.

##<a name="basic-reporting"></a>Tavaline teatamine

### <a name="recommended-method-overload-your-activity-classes"></a>Soovitatav meetod: ülekoormuse oma `Activity` tunnid

Selleks, et aktiveerida kõik logid nõutud kaasamine arvutada kasutajaid, seansid, tegevused, jookseb ja tehniline statistika aruande, vaid peate tegema kõik teie `*Activity` alamkaustadeks tunnid pärivad vastava `Engagement*Activity` tunnid (nt kui teie pärand ulatub `ListActivity`, tehke seda laiendab `EngagementListActivity`).

**Ilma kaasamine:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Kus lingid:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Kasutades `EngagementListActivity` või `EngagementExpandableListActivity`, veenduge, et mõni kõne `requestWindowFeature(...);` enne kõne tehakse `super.onCreate(...);`, muidu juhtub krahhi.

Saate otsida neid tunde on `src` kausta ja kopeerige need oma projekti. Klassid on ka **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatiivne meetod: kõne `startActivity()` ja `endActivity()` käsitsi

Kui te ei saa või ei soovi ülekoormuse oma `Activity` tunnid, saate selle asemel käivitamine ja lõppkuupäeva oma tegevuste helistades `EngagementAgent`'s meetodid otse.

> [AZURE.IMPORTANT] Android SDK kunagi helistab selle `endActivity()` meetod, isegi siis, kui rakendus on suletud (Android, rakendused on tegelikult kunagi suletud). Seega on *väga* soovitatav helistamiseks on `startActivity()` meetod on `onResume` tagasihelistamise *Kõik* oma tegevuste ja `endActivity()` meetod on `onPause()` tagasihelistamise *Kõik* oma tegevuste. See on ainus võimalus kindlasti, et seansid ei lekkinud. Kui lekkinud on seanss, kaasamine teenuse katkestab kunagi kaasamine kirjutamata (kuna teenuse jääb ühendatud kui seanss on ootel).

Siin on näide:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Selles näites on väga sarnane on `EngagementActivity` klassi ja selle variandid, mille lähtekood on esitatud selle `src` kausta.

##<a name="test"></a>Test

Nüüd kontrollige oma integreerimine töötab teie mobiilirakenduse emulaator või seadme ja kinnitatava registreerib see seansi vahekaardil kuvar.

Järgmised jaotised on valikuline.

##<a name="location-reporting"></a>Asukoha teatamine

Kui soovite asukohad esitatakse, peate lisama paar rida konfigureerimine (vahel on `<application>` ja `</application>` siltide).

### <a name="lazy-area-location-reporting"></a>Rongile ala asukoha teatamine

Rongile ala asukoht aruandlus võimaldab aruande riik, piirkond ja seotud seadmete asukoht. Seda tüüpi asukoha teatamise kasutab ainult võrgukohtades (vastavalt lahtri ID või WiFi-ühendus). Kõige kord seansi esitatakse seadmes alale. GPS ei kasutata ja seega seda tüüpi aruande asukoht on väga vähe (ei tähenda, et ei) mõju kohta.

Teatatud alade kasutatakse kasutajaid, seansid, sündmused ja vigu geograafilised statistikat arvutada. Ta saab kasutada ka kriteeriumi REACHi kampaaniat.

Lubada rongile ala asukoha teatamine, saate selle konfiguratsiooni eelnevalt mainitud selle toimingu abil teha.

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Samuti vajate lisada järgmised õigused, kui puuduvad:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Või saate kasutada ``ACCESS_FINE_LOCATION`` kui kasutate juba selle rakenduse.

### <a name="real-time-location-reporting"></a>Reaalajas asukoha teatamine

Reaalajas asukoha teatamine võimaldab aruande laiuskraad ja pikkuskraad seotud seadmed. Vaikimisi seda tüüpi asukoha teatamise kasutab ainult võrgukohtades (vastavalt lahtri ID või WiFi-ühendus) ja aruandluse ainult on aktiivne, kui rakendus töötab esiplaanil (nt käigus seansi).

Reaalajas asukohad on *pole* kasutatud arvutada statistikat. Nende ainus eesmärk on luba kasutada reaalajas geo hoides \<REACHi-sihtrühma-geofencing\> kriteeriumi REACHi kampaaniat.

Reaalajas asukoht aruandluse lubamiseks saate selle konfiguratsiooni eelnevalt mainitud selle toimingu abil teha.

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Samuti peate lisama järgmised õigused, kui puuduvad:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Või saate kasutada ``ACCESS_FINE_LOCATION`` kui kasutate juba selle rakenduse.

#### <a name="gps-based-reporting"></a>GPS põhineva aruandlus

Vaikimisi reaalajas asukoha teatamise kasutab ainult vastavalt võrgukohtades. GPS kasutamise lubamine alusel asukohtades (mis on palju täpsemad), kasutage konfiguratsiooni objekti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Samuti peate lisama järgmised õigused, kui puuduvad:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Tausta teatamine

Vaikimisi reaalajas asukoha teatamine on aktiivne ainult kui rakendus töötab esiplaanil (nt käigus seansi). Luba taustal ka aruanded, kasutage konfiguratsiooni objekti.

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Kui rakendus töötab taustal, ainult võrgus vastavalt asukohad esitatakse ka siis, kui olete lubanud GPS.

Aruande taustal asukoht peatub, kui kasutaja taaskäivitamisega oma seadme, saate seda muuta käivitamisel automaatselt uuesti lisada.

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Samuti peate lisama järgmised õigused, kui puuduvad:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Androidi M õigused

Alustades Androidi M, mõned õigused hallatakse käitusajal ja peab kasutaja kinnitamine.

Käitusaja õiguste lülitatakse välja uue rakenduse installi vaikimisi, kui saate suunata Androidi API tase 23. Muul juhul see lülitatakse vaikimisi.

Kasutaja saab lubada või keelata need õigused seadme menüü Sätted kaudu. Väljalülitamine süsteemi menüü õigused tapab taust protsessid rakenduse, see on süsteemi käitumine ja vastuvõtmiseks taustal võimalus ei mõjuta.

Mobile kaasamine kontekstis, on õigused, mis käitusajal kinnitamise nõudmine.

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(kui suunamise selle ühe Androidi API tase 23)

Välise salvestusruumi kasutatakse ainult REACHi üldpildiga funktsiooni. Kui te ei leia küsib kasutajad üleminek see õigus, saate selle eemaldada kui kasutasite ainult Mobile kaasamine, kuid hinnaga üldpildiga funktsiooni keelamine.

Asukoha funktsioone, peate hankima standard süsteemi dialoogiboksis kasutaja õigused. Kui kasutaja kinnitab, peate rääkima ``EngagementAgent`` selle muudatuse arvesse võtta reaalajas (vastasel juhul muutmine töödeldakse järgmine kord, kui kasutaja käivitab rakenduse).

Siin on näide koodi koosolekukutse õiguste ja edasisaatmine tulemi, kui on positiivne tegevus rakenduse abil ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Täpsem aruandlus

Soovi korral võite kui soovite teatada rakenduse teatud sündmused, tõrgete ja -tööde haldamine, peate kasutama kaasamine API meetodid kaudu soovitud `EngagementAgent` klassi. Selle rühma objekti saab retreived helistades on `EngagementAgent.getInstance()` staatiline meetod.

Kaasamine API võimaldab kasutada kõiki suhete 's täpsemalt võimaluste ja on üksikasjalikult kirjeldatud juhised kaasamine API kasutamine Androidi (samuti dokumentatsioon, on `EngagementAgent` klassi).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Täpsem (AndroidManifest.xml) konfigureerimine

### <a name="wake-locks"></a>Pärast lukud

Kui soovite olla kindel, et statistika saadetakse reaalajas kasutades Wi-Fi- või kui ekraan on välja lülitatud, lisage järgmised valikuline õigused.

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Aruande ootamatult sulguda

Kui soovite keelata krahh aruanded, lisage see (vahel on `<application>` ja `</application>` sildid):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst lävi

Vaikimisi logib reaalajas kaasamine teenuse aruandeid. Kui rakenduse aruannete logid väga sageli, on parem puhvri logid ja aruandluseks need kõik korraga aeg base (seda nimetatakse "Sarivõte"). Võimalusi, et lisada see (vahel on `<application>` ja `</application>` sildid):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Selle Sarivõte veidi aku tõsta, kuid mõjutab kaasamine jälgimine: kõik seansid ja -tööde haldamine kestus ümardatakse lõhkemist lävi (seega seansid ja -tööde haldamine lühem kui lõhkemist lävi ei pruugi olla nähtavad). Soovitatav on kasutada lõhkemist piirmäära enam kui 30000 (30s).

### <a name="session-timeout"></a>Seansi ajalõpp

Vaikimisi seanss on lõppenud 10s pärast viimase tegevuse (mis juhtub tavaliselt Home või tagasi vajutamisega, seades telefoni jõude või hüpates mõnda muusse rakendusse). See on vältimiseks tükeldamine iga kord, kui kasutaja välju seanss ja naaske rakenduse väga kiiresti (mis juhtub kui ta valimine üles soovitud pilt, vaadata teatise jne.). Kui soovite muuta see parameeter. Võimalusi, et lisada see (vahel on `<application>` ja `</application>` sildid):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Keelake log teatamine

### <a name="using-a-method-call"></a>Kõne meetodi abil

Kui soovite kaasamine peatamiseks logid saata, saate helistada.

            EngagementAgent.getInstance(context).setEnabled(false);

See kõne on püsiv: kasutab faili ühiskasutusega eelistused.

Kui kaasamine on aktiivne, kui helistate seda funktsiooni, võib kuluda 1 minuti teenuse peatada. Kuid see ei käivitada teenuse üldse järgmisel käivitamisel rakendus.

Log teatamise uuesti sama funktsiooni abil saate lubada `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Oma integreerimise`PreferenceActivity`

Asemel nõuab seda funktsiooni, saate ka integreerida see säte sisse otse oma praegust `PreferenceActivity`.

Saate konfigureerida kaasamine kasutada oma eelistusi faili (koos soovitud režiim) soovitud `AndroidManifest.xml` faili `application meta-data`:

-   Funktsiooni `engagement:agent:settings:name` võtit kasutatakse määratleda eelistused ühiskasutusega faili nimi.
-   Funktsiooni `engagement:agent:settings:mode` vajutamise korral määratleda režiimi eelistusi ühiskasutusega faili, kasutage sama režiimi nimega oma `PreferenceActivity`. Režiimi siseneda arvuna: kui kasutate koodi kombinatsiooni Konstandi lipud, märkige ruut koguväärtus.

Kaasamine alati kasutada funktsiooni `engagement:key` kahendmuutujaga klahvi haldamise selle sätte eelistused failis.

Järgmises näites `AndroidManifest.xml` kuvatakse vaikimisi väärtused:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Seejärel saate lisada mõne `CheckBoxPreference` eelistused paigutuses, nagu üks järgmistest:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
