<properties
    pageTitle="Azure'i mobiilsideseadmete kaasamine Android SDK integreerimine"
    description="Uusimad värskendused ja toiminguid Android SDK Azure Mobile kaasamine"
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
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Kuidas integreerida GCM mobiilsideseadmete kaasamine

> [AZURE.IMPORTANT] Peate järgima integreerimine toimingut, mis on kirjeldatud, kuidas integreerida kaasamine Androidi dokumendiga enne selle juhendi.
>
> Selles dokumendis on kasulik ainult siis, kui teil juba integreeritud REACHi mooduli ja lepingu push Google Play seadmed. Sujuv REACHi kampaaniat oma rakenduse, lugege esmalt kuidas jõuda integreerida kaasamine Android.

##<a name="introduction"></a>Sissejuhatus

GCM integreerimise rakendus, mis võimaldab teie lükata.

GCM lõhkelaengu lükata SDK alati sisaldavad soovitud `azme` olulisi andmeid objekt. Seega kui kasutate GCM muul eesmärgil rakenduse, saate filtreerida sunnib põhinevad et võti.

> [AZURE.IMPORTANT] Ainult seadmetes, kus töötab Android 2.2 või kohal, kas teil on installitud ning teil on Google Google Play tausta ühenduse lubatud saate lükata abil GCM; Siiski saate integreerida selle koodi turvaline toetamata seadmetes (ainult kasutab kavatsused).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>API võti Google Cloud sõnumside projekti loomine

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK integreerimine

### <a name="managing-device-registrations"></a>Seadme registreerimise haldamine

Iga seadme peate saatma registreerimise käsu Google serverid, muidu ei jõudnud.

Seadme saate ka unregister GCM teatised (seade on automaatselt registreerimata rakenduse desinstallimisel) kaudu.

Kui te ei kasuta [Google Play SDK] või pole juba saadate registreerimise soovidele ise, saate kaasamine registreerida seade automaatselt teie eest.

Selle lisatakse järgmised oma `AndroidManifest.xml` faili sees on `<application/>` silt:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registreerimise id teenusesse kaasamine Push suhelda ja saada teatisi

Suhelda kaasamine Push teenuse seadme registreerimise ID-d ja selle teatisi, et lisada järgmised oma `AndroidManifest.xml` faili sees on `<application/>` sildistamine (isegi juhul, kui haldate seadme registreerimine ise):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Veenduge, et teil on järgmised õigused oma `AndroidManifest.xml` (pärast selle `</application>` silt).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Kaasamine mobiilse juurdepääsu lubamine oma GCM API võti

Järgige [selle juhendi](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile kaasamine juurdepääsu lubamine oma GCM API võti.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
