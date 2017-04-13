<properties
    pageTitle="Azure'i mobiilsideseadmete jaoks Cordova/Phonegap kaasamine kasutamise alustamine"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Analytics ja tõuketeatised Cordova/Phonegap rakendused."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Azure'i mobiilsideseadmete jaoks Cordova/Phonegap kaasamine kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas näidatakse, kuidas kasutada Azure Mobile kaasamine mõista oma rakenduse kasutamine ja saata Tõuketeatiste segmenditud kasutajate jaoks välja töötatud Cordova mobiilirakendust.

Selles õpetuses me luua tühja Cordova rakenduse abil Mac-arvutisse ja seejärel integreerida Mobile kaasamine SDK. Selle lihtsa analytics andmete kogub ja võtab vastu Tõuketeatiste for Android kasutamise Apple Push teatise süsteemi (APNS) iOS-i ja Google Cloud sõnumside (GCM). Me kasutada seda, iOS või Android-seadmes testimiseks. 

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Selle õpetuse kasutamiseks on vaja järgmist:

+ XCode, mille saate installida Mac App Store'ist (for iOS-i juurutamist)
+ [Android SDK ja emulaator](http://developer.android.com/sdk/installing/index.html) (for Android juurutamine)
+ Tõuketeatised teatis sert (.p12), mida saate hankida mõnest Arenduskeskus Apple'i APNs-i jaoks
+ Saate hankida oma Google'i arendaja konsooli GCM GCM projektinumber
+ [Mobiilse kaasamine Cordova lisandmoodul](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Võite leida lähtekoodi ja ReadMe Cordova lisandmooduli [github](https://github.com/Azure/azure-mobile-engagement-cordova)

##<a id="setup-azme"></a>Mobile kaasamine oma Cordova rakenduse häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse tutvustab "lihtsa ühendamine", mis on minimaalne vajalik jõudlusandmeid koguda ja tõuketeatised teatise saatmine. 

Loome lihtsa rakenduse Cordova näidata integreerimine:

###<a name="create-a-new-cordova-project"></a>Cordova uue projekti loomine

1. Käivitage *Terminal* aknas oma Mac-arvutisse ja tippige järgmine, mis loob uue projekti Cordova vaikemalli kaudu. Veenduge, et publishing profiili lõpuks abil saate kasutada oma iOS-i app kasutab 'com.mycompany.myapp' rakenduse ID 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Käivita projekti **iOS-i** konfigureerimine ja käivitage see iOS-i simulaatorit järgmist:

        $ cordova platform add ios 
        $ cordova run ios

3. Järgnevad projekti konfigureerimine **Androidi** ja käivitage see Android emulaator käivitada. Veenduge, et et oma Android SDK emulaator sätted on oma nimega Google API (Google Inc.) CPU / nimega Google API-de ARM ABI.  

        $ cordova platform add android
        $ cordova run android

4.  Lisandmooduli Cordova konsooli. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Ühenduse loomine rakenduse Mobile kaasamine taustväärtus

1. Installige Azure'i Mobile kaasamine Cordova lisandmooduli pakkudes selle lisandmooduli konfigureerimiseks muutujaga.

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Androidi saavutamiseks ikoon* : peab olema ilma laiend ega joonestatav eesliite ressursi nimi (ex: mynotificationicon), ja faili ikooni tuleb kopeerida oma Androidi project (platvormid/android/res/joonestatav)

*iOS-i saavutamiseks ikoon* : peab olema laiend ressursi nimi (ex: mynotificationicon.png), ja faili ikooni tuleb lisada oma iOS-i projekti XCode (failide lisamine menüü kaudu)

##<a id="monitor"></a>Reaalajas jälgimise lubamine

1. Redigeerige Cordova projekti - **www/js/index.js** lisada Mobile kaasamine deklareerimine uue korra *deviceReady* sündmuse kõne on saanud.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Käivitage rakendus.
        
    - **IOS-i**
    
        Klõpsake `Terminal` akna käivitada rakenduse uus simulaatorit eksemplari, käitades järgmine:

            cordova run ios

    - **Androidi jaoks**
        
        Klõpsake `Terminal` akna käivitada rakenduse uus emulaator eksemplari, käitades järgmine:

            cordova run android

3. Näete järgmist konsooli logid:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil oma kasutajatega tõuketeatised ja rakenduse sõnumside kampaaniat kontekstis abil. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmistes jaotistes on häälestamine rakenduse neid vastu võtta.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Tõuketeatised mandaadi konfigureerimine Mobile kaasamine

Luba Mobile kaasamine saata tõuketeatised teie nimel, peate anda juurdepääsu oma Apple iOS-i serdi või GCM serveri API võti. 
    
1. Liikuge oma Mobile kaasamine portaali. Veenduge, et olete rakenduse meil selle projekti jaoks kasutate, ja seejärel klõpsake allosas nuppu **osaleda** :
    
    ![][1]
    
2. Saate oma kaasamine portaalis lehel maa. Olemas, klõpsake nuppu **Kohalikke Push** jaotises:
    
    ![][2]

3. IOS-i serdi/GCM serveri API võti konfigureerimine

    **[iOS-i]**

    lisamine. Valige oma .p12, laadige see ja tippige oma parool.
    
    ![][3]

    **[Android]**

    lisamine. Klõpsake ikooni redigeerimine ette **API võti** GCM sätted jaotises ja hüpikaknas, mis näitab kleepida GCM serveri võti ja klõpsake nuppu **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Tõuketeatiste Cordova rakenduse lubamine

Redigeerimiseks **www/js/index.js** kõne Mobile kaasamine tõuketeatised ja deklareerida on sündmuseohjuri lisamiseks tehke järgmist.

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Käivitage rakendus

**[iOS-i]**

1. Me kasutame XCode luua ja juurutada katse Tõuketeatiste, kuna iOS võimaldab ainult Tõuketeatiste tegelik seadmesse seadmes rakendus. Minge asukohta, kus luuakse Cordova projekti ja liikuge **...\platforms\ios** asukoht. XCode kohalikke .xcodeproj faili avada. 
    
2. Koostage ja Juurutage Cordova rakendus iOS-seadmes kontoga, mis on ettevalmistamise profiili sisaldavate sert, mille just üles laadida Mobile kaasamine portaali ja rakenduse Id mis vasteid üks andsite Cordova rakenduse loomisel. Saate vaadata *kogumi identifikaator* oma **ressursid\*-info.plist** XCode sobitada seda faili. 

3. Kuvatakse hüpikaknas standard iOS-i seadmes, mis ütleb, et rakendus taotleb õigus saada teatisi. Anda õiguse. 

**[Android]**

Saate lihtsalt emulaator nagu GCM teatised on toetatud Android emulaator Androidi rakenduse käivitamiseks. 

    cordova run android

##<a id="send"></a>Rakenduse teatise saatmine

Nüüd loome lihtsa tõuketeatised teatis turunduskampaania, mis push saata oma rakendus töötab seadme:

1. Liikuge oma Mobile kaasamine portaalis menüü **saavutamiseks.**

2. Klõpsake nuppu **Uus teadaanne** oma tõuketeatised turunduskampaania loomine

    ![][6]

3. Sisestage sisendina teie **[Android]** turunduskampaania loomine
    
    - Sisestage oma turunduskampaania jaoks **nimi** . 
    - Valige *süsteem teatise* *lihtne* **Kohaletoimetamise tüüp**
    - Valige **kohaletoimetamise aeg** *"igal ajal"*
    - Sisestage oma teatise, mis on esimene rida push **tiitel** .
    - Teie teate, mis toimib sõnumi kehasse **sõnum** ette. 

    ![][11]

4. Sisestage sisendina luua oma turunduskampaania **[iOS-i]**

    - Sisestage oma turunduskampaania jaoks **nimi** . 
    - Valige **kohaletoimetamise kellaaeg** *"välja ainult rakenduse"*
    - Sisestage oma teatise, mis on esimene rida push **tiitel** .
    - Teie teate, mis toimib sõnumi kehasse **sõnum** ette. 
 
    ![][12]

5. Liikuge allapoole ja sisu jaotises Valige **ainult teatis**

    ![][8]

6. [Valikulised] Samuti saate sisestada toimingu URL-i. Veenduge, et see kasutab URL-i kava samal ajal soovitud lisandmooduli konfigureerimise **AZME\_ümber SUUNATA\_URL-i** muutuja nt *myapp://test*.  

7. Olete valmis seadmine kõige olulisemaid turunduskampaania võimalik. Nüüd kerige uuesti ja klõpsake nuppu **Loo** salvestada oma turunduskampaania.

8. Lõpuks **Aktiveeri** oma turunduskampaania
    
    ![][10]

9. Nüüd peaks tõuketeatised teatise nähtaval osana kampaanias seadmes või emulaator. 

##<a id="next-steps"></a>Järgmised sammud
[Kõik meetodid saadaval Cordova Mobile kaasamine SDK ülevaade](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

