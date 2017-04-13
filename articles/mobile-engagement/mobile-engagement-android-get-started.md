<properties
    pageTitle="Alustamine Androidi rakenduste Azure Mobile kaasamine"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Kasutusanalüüsi ja push teatised rakendused Androidi jaoks."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Azure'i Mobile kaasamine Androidi rakenduste kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas kirjeldatakse, mida kasutamise Azure Mobile kaasamine mõista teie rakenduse kasutamise kohta ja kuidas saata Tõuketeatiste segmenditud kasutajad Androidi rakenduse.
Selle õpetuse näitab lihtsa leviedastuse stsenaariumi, kasutades Mobile allikaid. Saate luua tühja Androidi rakendust, mis kogub lähteandmete ja võtab vastu Tõuketeatiste Google Cloud sõnumside (GCM) abil.

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse lõpuleviimine nõuab [Androidi arendaja tööriistu](https://developer.android.com/sdk/index.html), mis sisaldab Androidi Studio integreeritud keskkonnas ja uusim Android platvorm.

Jaoks on vaja [Mobile kaasamine Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Selle õpetuse tegemiseks peate Azure active konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Teie Androidi jaoks mõeldud Mobile kaasamine häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse tutvustab "lihtsa integratsioon", mis on nõutav jõudlusandmeid koguda ja tõuketeatised teatise saatmine minimaalsete määramine. Saate luua lihtsa rakenduse Androidi Studio integreerimine näitamiseks.

Täielik integratsioon dokumentatsiooni kohta leiate [Mobile kaasamine Android SDK integreerimine](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Luua Androidi projekti

1. Käivitage **Androidi Studio**ja hüpikaknas nuppu **Alusta uut Androidi Studio projekti**.

    ![][1]

2. Sisestage rakenduse nimi ja ettevõtte domeeni. Kirjutage, mis on täita, kuna seda hiljem vaja. Klõpsake nuppu **edasi**.

    ![][2]

3. Valige target vormi tegur ja API taset, ja klõpsake nuppu **edasi**.

    >[AZURE.NOTE] Mobile kaasamine nõuab API tase 10 minimaalne (Android 2.3.3).

    ![][3]

4. Valige **Tühi tegevuse** siin, mis on selle rakenduse Kuva ainult ja klõpsake nuppu **edasi**.

    ![][4]

5. Lõpetuseks, jätke vaikesätted ja klõpsake nuppu **valmis**.

    ![][5]

Nüüd loob Androidi Studio demo rakenduse kuhu me integreerida Mobile allikaid.

### <a name="include-the-sdk-library-in-your-project"></a>Projekti SDK teegi kaasamine

1. Laadige alla [mobiilne kaasamine Android SDK](https://aka.ms/vq9mfn).
2. Väljavõte arhiivi faili teie arvuti kausta.
3. Laiendid teegi jaoks selle SDK praeguse versiooni tuvastamine ja kopeerige see lõikelauale.

      ![][6]

4. Liikuge jaotisse **projekti** (1) ja kleepida selle laiendid kena tegevust jälle kausta (2).

      ![][7]

5. Teegi laadimiseks sünkroonida projekt.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Ühenduse loomine rakenduse Mobile kaasamine kirjutamata ühendusstringi abil

1. Kopeerige järgmised koodiread tegevuse loomine (peab tegema vaid ühes kohas rakenduse tavaliselt tegevuse peamine). Valimi minirakenduse-MainActivity src jaotises Ava > põhi -> java kaust ja lisage järgmine tekst:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Tõrke viiteid, vajutades klahvikombinatsiooni Alt + Enter või lisamise impordi järgmistest:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Minge tagasi oma rakenduse lehel **Ühenduse teave** Azure klassikaline portaali ja kopeerige **Ühendusstring**.

      ![][9]

4. Kleepige see on `setConnectionString` parameeter, asendades kogu string, mis on kuvatud järgmine kood:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Õigused ja deklaratsioon teenuse lisamine

1. Vahetult enne või pärast nende õiguste lisamine projekti Manifest.xml soovitud `<application>` silt:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Deklareerida teenuse agent, lisage järgmine kood vahel on `<application>` ja `</application>` Sildid:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Asendage kleebitud koodis `"<Your application name>"` silti, mis kuvatakse menüü **sätted** , kus näete seadme teenuseid. Saate lisada sõna "Teenus" mis märgivad näiteks.

### <a name="send-a-screen-to-mobile-engagement"></a>Saada ekraani Mobile kaasamine

Käivitage andmete saatmine ja veenduge, et kasutajad on aktiivne, peate saatma Mobile kaasamine kirjutamata vähemalt ühe ekraanikuva (tegevuste).

Minge **MainActivity.java** ja lisage järgmine base klassi **MainActivity** **EngagementActivity**soovite asendada.

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Kui alus tunni pole *tegevust*, konsulteerige [Täpsemad Androidi aruannete](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) kohta, kuidas erinevate tunnid pärivad.


Kommentaar välja rida selle lihtsa valimi stsenaariumi:

    // setSupportActionBar(toolbar);

Kui soovite säilitada selle `ActionBar` oma rakenduse kuva [Täpsemad Androidi teatamine](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Ajal kampaaniat, Mobile kaasamine võimaldab teil suhelda ja saavutamiseks kasutajate tõuketeatised ja sõnumside rakendus. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmises jaotises häälestab teie rakendus neid vastu võtta.

### <a name="copy-sdk-resources-in-your-project"></a>Kopeerige oma projekti SDK ressursid

1. Liikuge tagasi SDK allalaadimine sisu ja kopeerige **res** kausta.

    ![][10]

2. Minge tagasi Androidi Studio, valige **peamine** kataloog projekti failide ja seejärel kleepige see ressursside lisamine projekti.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Järgmised sammud

Avage [Androidi SDK](mobile-engagement-android-sdk-overview.md) saada üksikasjalikke teadmisi SDK integreerimise kohta.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
