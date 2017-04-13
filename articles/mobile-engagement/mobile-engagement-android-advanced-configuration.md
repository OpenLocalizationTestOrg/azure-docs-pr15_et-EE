<properties
    pageTitle="Azure'i Mobile kaasamine Android SDK täpsemalt konfigureerimine"
    description="Kirjeldab täpsemalt konfiguratsiooni suvandeid, sh Androidi näidata Azure Mobile kaasamine Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Azure'i Mobile kaasamine Android SDK täpsemalt konfigureerimine

> [AZURE.SELECTOR]
- [Universaalne Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

See toiming kirjeldab erinevaid võimalusi konfiguratsiooni Azure Mobile kaasamine Androidi rakenduste konfigureerimine.

## <a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Õiguste nõuded
Mõne suvandi nõua kindlaid õigusi, mis on loetletud siin viide ja-line teatud funktsioon. Vahetult enne või pärast nende õiguste lisamine projekti AndroidManifest.xml on `<application>` silt.

Õiguste koodi peab välja nägema järgmine, kus täidate vastav õigus järgneva tabelist.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Õiguste | Kui seda kasutatakse |
| ---------- | --------- |
| INTERNET | Nõutav. Tavaline aruannete jaoks |
| ACCESS_NETWORK_STATE | Nõutav. Tavaline aruannete jaoks |
| RECEIVE_BOOT_COMPLETED | Nõutav. Pärast seadet taaskäivitage arvuti Centeri teatiste kuvamiseks |
| WAKE_LOCK | Soovitatav. Lubab andmete kogumise, kui kasutate Wi-Fi- või kui ekraan on välja lülitatud |
| VÄRIN | Valikuline. Lubab vibratsiooni kui saadetud teatised |
| DOWNLOAD_WITHOUT_NOTIFICATION | Valikuline. Lubab Androidi Üldpildiga teatis |
| WRITE_EXTERNAL_STORAGE | Valikuline. Lubab Androidi Üldpildiga teatis |
| ACCESS_COARSE_LOCATION | Valikuline. Võimaldab reaalajas asukoha teatamine |
| ACCESS_FINE_LOCATION | Valikuline. Lubab GPS-põhiste asukoha teatamine |

Alustades Androidi M, [teatud õiguste hallatakse käitusajal](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Kui kasutate juba ``ACCESS_FINE_LOCATION``, siis ei pea te kasutada ka ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Androidi Manifest konfiguratsiooni suvandid

### <a name="crash-report"></a>Aruande ootamatult sulguda

Krahh aruannete keelamiseks lisage järgmine kood vahel on `<application>` ja `</application>` Sildid:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst lävi

Vaikimisi logib reaalajas kaasamine teenuse aruandeid. Kui teie aruande logid erinevad sageli, on parem puhvri logid ja teatada neid kõiki korraga base aeg (nn "lõhkemise režiimi"). Selleks, lisage järgmine kood vahel on `<application>` ja `</application>` Sildid:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Sarivõte veidi suureneb aku, kuid mõjutab kaasamine kuvari: kõik seansid ja -tööde haldamine kestus ümardatakse lõhkemist lävi (seega seansid ja -tööde haldamine lühem kui lõhkemist lävi ei pruugi olla nähtavad). Teie lõhkemist läve ei tohi olla pikem kui 30000 (30s).

### <a name="session-timeout"></a>Seansi ajalõpp

 Saate registreeruda tegevuse, vajutades klahvi **Home** või **tagasi** telefoni jõude või mõnda muusse rakendusse hüpped lõpetada. Vaikimisi seanss lõpetatakse kümme minutit pärast viimase tegevuse. See aitab vältida seansi tükeldatud iga kord kasutaja väljub ja tagastab rakendusse kiiresti, mis võib juhtuda kui kasutaja valib üles pilt, kontrollib teatis jne. Kui soovite muuta see parameeter. Selleks, lisage järgmine kood vahel on `<application>` ja `</application>` Sildid:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Keelake log teatamine

### <a name="using-a-method-call"></a>Kõne meetodi abil

Kui soovite kaasamine peatamiseks logid saata, saate helistada.

    EngagementAgent.getInstance(context).setEnabled(false);

See kõne on püsiv: kasutab faili ühiskasutusega eelistused.

Kui kaasamine on aktiivne, kui helistate seda funktsiooni, võib kuluda ühe minuti teenuse peatada. Kuid see ei käivitada teenuse üldse järgmisel käivitamisel rakendus.

Log teatamise uuesti sama funktsiooni abil saate lubada `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Oma integreerimise`PreferenceActivity`

Asemel nõuab seda funktsiooni, saate ka integreerida see säte sisse otse oma praegust `PreferenceActivity`.

Saate konfigureerida kaasamine kasutada oma eelistusi faili (koos soovitud režiim) soovitud `AndroidManifest.xml` faili `application meta-data`:

-   Funktsiooni `engagement:agent:settings:name` võtit kasutatakse määratleda eelistused ühiskasutusega faili nimi.
-   Funktsiooni `engagement:agent:settings:mode` võtit kasutatakse määratleda eelistused ühiskasutusega faili režiimi. Kasutage sama režiimi nimega oma `PreferenceActivity`. Režiimi siseneda arvuna: kui kasutate koodi kombinatsiooni Konstandi lipud, märkige ruut koguväärtus.

Kaasamine alati kasutab funktsiooni `engagement:key` kahendmuutujaga klahvi haldamise selle sätte eelistused failis.

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
