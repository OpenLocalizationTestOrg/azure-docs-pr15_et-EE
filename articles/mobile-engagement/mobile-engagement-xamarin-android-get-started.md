<properties
    pageTitle="Azure'i mobiilsideseadmete jaoks Xamarin.Android kaasamine kasutamise alustamine"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Analytics ja tõuketeatised Xamarin.Android rakendused."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Azure'i mobiilsideseadmete kaasamine Xamarin.Android rakenduste kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas kirjeldatakse, mida kasutamise Azure Mobile kaasamine mõista teie rakenduse kasutamise kohta ja kuidas saata Tõuketeatiste segmenditud kasutajad Xamarin.Android rakenduse.
Selle õpetuse näitab lihtsa leviedastuse stsenaariumi, kasutades Mobile allikaid. Saate luua tühja Xamarin.Android rakendus, mis kogub lähteandmete ja tõuketeatised Google Cloud sõnumside (GCM) abil saab.

Selle õpetuse kasutamiseks on vaja järgmist:

+ [Xamarin Studio](http://xamarin.com/studio). Visual Studio saate kasutada ka koos Xamarin, kuid selles õpetuses kasutab Xamarin Studio. Installimise juhiste saamiseks vaadake teemat [Setup ja installige Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobiilse kaasamine Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Teie Androidi jaoks mõeldud Mobile kaasamine häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse esitab "lihtsa integratsioon", mis on nõutav andmete kogumine ja tõuketeatised teatise saatmine minimaalsete määramine. 

Loome lihtsa rakenduse Xamarin Studio näidata integreerimine.

###<a name="create-a-new-xamarinandroid-project"></a>Xamarin.Android uue projekti loomine

1. Avage **fail**käivitada **Xamarin Studio**  -> **uue** -> **lahendus** 

    ![][1]

2. Valige **Androidi rakendust** ja seejärel veenduge, et valitud keeles on **C#** ja klõpsake nuppu **edasi**.

    ![][2]

3. Sisestage **Rakenduse nimi** ja **ettevõtte identifikaator**. Veenduge, et märkida **Google Play teenuseid** ja seejärel klõpsake nuppu **edasi**. 

    ![][3]
    
4. Värskendage **Projekti nime**, **Lahenduse nime** ja **asukoha** vajadusel ja klõpsake nuppu **Loo**.

    ![][4]
 
Xamarin Studio loob rakendus meil Mobile kaasamine integreeritakse. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Ühenduse loomine rakenduse Mobile kaasamine taustväärtus

1. Paremklõpsake **pakettide** lahenduse Windowsi kausta ja valige **Lisa pakettide**

    ![][5]

2. **Microsoft Azure'i Mobile kaasamine Xamarin SDK** otsida ja lisada selle oma lahendus.  

    ![][6]
   
3. Avage **MainActivity.cs** ja lisage järgmine laused abil:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. Rakenduses selle `OnCreate` meetod, lisage järgmine lähtestamine Mobile kaasamine kirjutamata seos. Veenduge, et lisada oma **ConnectionString**. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Õigused ja deklaratsioon teenuse lisamine

1. Kausta atribuudid jaotises **Manifest.xml** faili avada. Valige allikas menüü nii, et värskendada otse XML-allikas.
 
2. Vahetult enne või pärast nende õiguste lisamine Manifest.xml (mis leiate kausta **Atribuudid** ) projekti soovitud `<application>` silt:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Lisage järgmine vahel on `<application>` ja `</application>` siltide deklareerida agent teenuse:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Lihtsalt kleebitud koodis asendamine `"<Your application name>"` sildile. See on kuvatud menüü **sätted** , kus kasutajad saavad vaadata teenuste seade töötab. Saate lisada sõna "Teenus" mis märgivad näiteks.

###<a name="send-a-screen-to-mobile-engagement"></a>Saada ekraani Mobile kaasamine

Selleks, et alustada andmete saatmine ja tagada, et kasutajad on aktiivne, peate saatma vähemalt ühe ekraanikuva kirjutamata Mobile allikaid. Selle tegemiseks – tagada, et selle `MainActivity` pärib `EngagementActivity` asemel `Activity`.

    public class MainActivity : EngagementActivity
    
Teine võimalus, kui te ei saa pärivad `EngagementActivity` siis peate lisama `.StartActivity` ja `.EndActivity` meetodite `OnResume` ja `OnPause` vastavalt.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil suhelda ja saavutamiseks kasutajate tõuketeatised ja rakenduse sõnumside kampaaniat kontekstis. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmistes jaotistes häälestab teie rakendus neid vastu võtta.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
