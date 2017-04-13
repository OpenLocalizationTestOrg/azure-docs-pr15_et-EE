<properties
    pageTitle="Azure'i mobiilsideseadmete kaasamine Androidi SDK aruandlus asukoht"
    description="Kirjeldab, kuidas konfigureerida asukoht Azure Mobile kaasamine Androidi SDK teatamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Azure'i mobiilsideseadmete kaasamine Androidi SDK aruandlus asukoht

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

Selles teemas kirjeldatakse, kuidas teha Androidi rakenduse aruannete kohta.

## <a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Asukoha teatamine

Kui soovite asukohad esitatakse, peate lisama paar rida konfigureerimine (vahel on `<application>` ja `</application>` sildid).

### <a name="lazy-area-location-reporting"></a>Rongile ala asukoha teatamine

Rongile ala asukoht aruandlus võimaldab aruandlus riik, piirkond ja seostatud seadmete asukoht. Seda tüüpi asukoha teatamise kasutab ainult võrgukohtades (vastavalt lahtri ID või WiFi-ühendus). Kõige kord seansi esitatakse seadmes alale. GPS ei kasutata ja seega seda tüüpi aruande asukoht on väike mõju kohta.

Teatatud alade kasutatakse kasutajad, seansid, sündmused ja vigu geograafilised statistikat arvutada. Ta saab kasutada ka kriteeriumi REACHi kampaaniat.

Aruandlus eelnevalt mainitud järgmist konfiguratsiooni rongile ala asukoha lubamine

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Samuti peate määrama asukoha õigus. Järgmine kood kasutab ``COARSE`` õigus:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Kui teie rakenduse jaoks on vaja seda, saate kasutada ``ACCESS_FINE_LOCATION`` asemel.

### <a name="real-time-location-reporting"></a>Reaalajas asukoha teatamine

Reaalajas asukoha teatamine võimaldab aruandlus laius- ja laiuskraadide seotud seadmed. Vaikimisi kasutab seda tüüpi asukoha teatamine ainult võrgukohtades lahtri ID või WiFi-ühendust. Aruandlus on ainult aktiivne, kui rakendus töötab esiplaanil (nt ajal seansi).

Reaalajas asukohad on *pole* kasutatud arvutada statistikat. Nende ainus eesmärk on luba kasutada reaalajas geo hoides \<REACHi-sihtrühma-geofencing\> kriteeriumi REACHi kampaaniat.

Reaalajas asukoha aruandluse lubamiseks lisada rida koodi kui seate kaasamine ühendusstringi tegevuse käivitit. Tulemus näeb välja umbes selline:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS põhineva aruandlus

Vaikimisi reaalajas asukoha teatamine kasutab ainult võrgupõhine asukohad. GPS-põhine kohad, mis on palju Täpsemad, kasutamise lubamine kasutada konfiguratsiooni objekti.

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Samuti peate lisama järgmised õigused, kui puuduvad:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Tausta teatamine

Vaikimisi reaalajas asukoha teatamine on aktiivne ainult kui rakendus töötab esiplaanil (nt ajal seansi). Luba taustal ka aruanded, kasutage selle konfiguratsiooni objekti.

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Kui rakendus töötab taustal, ainult võrgupõhine asukohad esitatakse ka siis, kui olete lubanud GPS.

Kui kasutaja taaskäivitamisega oma seade, aruande asukoht taust on peatatud. Selle käivitamisel automaatselt uuesti, lisage järgmine kood.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Samuti peate lisama järgmised õigused, kui puuduvad:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Androidi M õigused

Alustades Androidi M, mõned õigused hallatakse käitusajal ja vajate kasutaja kinnitamine.

Kui saate suunata Androidi API taseme 23, käitusaja õigused on vaikimisi välja lülitatud uue rakenduse installi. Muul juhul ta on sisse lülitatud vaikimisi.

Te saate lubada või keelata need õigused seadme menüü Sätted kaudu. Väljalülitamine süsteemi menüü õigused tapab taust protsessid taotluse, mis on süsteemi käitumine ja vastuvõtmiseks taustal võimalus ei mõjuta.

Mobile kaasamine asukoha teatamise kontekstis, on õigused, mis käitusajal kinnitamise nõudmine.

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Päringu standard süsteemi dialoogiboksis kasutaja õigused. Kui kasutaja kinnitab, kindlaks teha ``EngagementAgent`` selle muudatuse arvesse võtta reaalajas. Muul juhul muutus töötlemise järgmine kord, kui kasutaja käivitab rakenduse.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
