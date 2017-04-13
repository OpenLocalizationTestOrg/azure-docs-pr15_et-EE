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


#<a name="how-to-integrate-adm-with-engagement"></a>Kuidas integreerida ADM kaasamine

> [AZURE.IMPORTANT] Peate järgima integreerimine toimingut, mis on kirjeldatud, kuidas integreerida kaasamine Androidi dokumendiga enne selle juhendi.
>
> Selles dokumendis on kasulik ainult siis, kui teil juba integreeritud REACHi mooduli ja lepingu push Amazon seadmed. Sujuv REACHi kampaaniat oma rakenduse, lugege esmalt kuidas jõuda integreerida kaasamine Android.

##<a name="introduction"></a>Sissejuhatus

Integreerimine ADM võimaldab lükata kui suunamise Amazon Androidi seadmete rakenduse.

ADM lõhkelaengu lükata SDK alati sisaldavad soovitud `azme` olulisi andmeid objekt. Seega kui kasutate ADM muul eesmärgil rakenduse, saate filtreerida sunnib põhinevad et võti.

> [AZURE.IMPORTANT] Ainult Amazon Kindle seadmetes, kus töötab Android 4.0.3 või kohal toetavad Amazon seadme sõnumside; Siiski saate integreerida selle koodi turvaline teistes seadmetes.

##<a name="sign-up-to-adm"></a>Logi sisse, et ADM

Kui see pole juba tehtud, tuleb lubada ADM Amazon kontol.

Protseduur on üksikasjalikult veebisaidil: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Protseduur vormistamisel saate teha järgmist:

-   OAuthi mandaat (kliendi ID ja kliendi salajane) kaasamine saama push seadmetes.
-   API võti, mis tuleb integreerida rakenduse.

##<a name="sdk-integration"></a>SDK integreerimine

### <a name="managing-device-registrations"></a>Seadme registreerimine haldamine

Iga seadme registreerimise käsu peate saatma ADM serverid, muidu ei jõudnud.

Kui kasutate juba [ADM kliendi teek]ja juba [integreeritud ADM] saate otse minna sdk Androidi adm vastu.

Kui teil on integreeritud ADM veel, on Engagement lihtsam viis oma rakenduse lubamiseks.

Redigeeri oma `AndroidManifest.xml` faili:

-   Amazon nimeruumi, lisage faili peaks algama umbes järgmine:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Sees on `<application/>` tag, lisage see jaotis:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Pärast lisamist amazon silti, võib olla ehitada tõrge, kui teie projekti koostada eesmärk on all Android 2.1. Peate kasutama mõne **Android 2.1 +** Koosta target (Ärge muretsege, saate veel mõne `minSdkVersion` seatud 4).
-   Integreerida ADM API võti vara järgmised [seda toimingut].

Seejärel järgige jaotistest.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registreerimise id teenusesse kaasamine Push suhelda ja saada teatisi

Suhelda kaasamine Push teenuse seadme registreerimise ID-d ja selle teatisi, et lisada järgmised oma `AndroidManifest.xml` faili sees on `<application/>` sildistamine (isegi kui kasutate ADM ilma Engagement):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Veenduge, et teil on järgmised õigused oma `AndroidManifest.xml` (enne selle `</application>` silt).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Anda kaasamine OAuthi identimisteave

Esitage mandaat OAuthi (kliendi ID ja kliendi salajane) kaasamine portaalis.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM kliendi teek]:https://developer.amazon.com/sdk/adm/setup.html
[integreeritud ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[See toiming]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
