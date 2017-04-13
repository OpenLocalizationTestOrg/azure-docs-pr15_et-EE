<properties
    pageTitle="Tõuketeatiste lisamine Apache Cordova rakenduse Azure mobiilirakendused | Azure'i rakendust Service"
    description="Saate teada, kuidas Azure'i mobiilirakenduste saatmine Tõuketeatiste Apache Cordova rakenduse abil."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Tõuketeatiste Apache Cordova rakenduse lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Ülevaade

Selles õpetuses lisate Tõuketeatiste projekti [Apache Cordova Kiire alustamine] , et tõuketeatised teatis saadetakse seade iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateavet leiate [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse hõlmab Apache Cordova rakenduse Visual Studio 2015, mis töötab Google Android emulaator, Android-seadmes, Windowsi seade ja iOS-seadmes välja töötatud.

Selle õpetuse tegemiseks vajate:

* Arvuti [Visual Studio ühenduse 2015] või uuem versioon.
* [Visual Studio tööriistad Apache Cordova jaoks].
* [Azure active konto](https://azure.microsoft.com/pricing/free-trial/).
* Lõpetatud [Apache Cordova Lühijuhend] projekti.
* (Android) [Google'i konto] kontrollitud e-posti aadress.
* (iOS) Mõne Apple'i arendaja programmi liikmete ja iOS-seadmes (iOS-i simulaatorit ei toeta tõuketeatised).
* (Windows) Windowsi poe arendaja konto ja Windows 10 seadmesse.

##<a name="configure-hub"></a>Teate jaoturi konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Vaadake videot, mis näitab selle jaotise juhised](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Värskenduse saatmiseks tõuketeatisi projekti server

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Tõuketeatiste vastuvõtmiseks oma Cordova rakenduse muutmine

Teil peab veenduge, et Apache Cordova rakenduse projekti on valmis tegelema Tõuketeatiste installides Cordova tõuketeatised lisandmooduli pluss platvormi kohased tõuketeatised teenused.

#### <a name="update-the-cordova-version-in-your-project"></a>Värskendage projektis Cordova versioon.

Soovitame, et värskendate kliendi projekti Cordova 6.1.1 kui projekti on konfigureeritud kasutamine vanem versioon. Projekti värskendamiseks paremklõpsake config.xml avamiseks konfiguratsiooni designer. Valige vahekaart platvormid ja valige 6.1.1 **Cordova CLI** tekstiväljale.

Valige **koostamine**ja seejärel **Lahenduse luua** projekti värskendamiseks.

#### <a name="install-the-push-plugin"></a>Tõuketeatised lisandmooduli installimine

Apache Cordova rakenduste hakkama algupäraselt seadme või võrgu võimalusi.  Neid võimalusi pakub lisandmoodulid, mis on avaldatud [npm](https://www.npmjs.com/) või github.  Funktsiooni `phonegap-plugin-push` lisandmoodul on kasutatud käsitlema võrgu tõuketeatisi.

Tõuketeatised lisandmooduli saate installida ühel järgmistest viisidest.

**Käsuviip:**

Käivitage järgmine käsk:

    cordova plugin add phonegap-plugin-push

**Kaudu jooksul Visual Studio:**

1.  Solution Exploreris avage soovitud `config.xml` fail, klõpsake nuppu **lisandmoodulid** > **kohandatud**, valige Installi allikana **Git** , seejärel sisestage `https://github.com/phonegap/phonegap-plugin-push` allikana.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klõpsake installimise andmeallika kõrval olevat noolt.

3. Klõpsake **SENDER_ID**, kui teil on juba arvuline projekti ID-d Google arendaja konsooli projekti, saate lisada selle siin. Muul juhul sisestage kohatäite väärtus, nt 777777, ja kui olete suunatud Androidi saate selle väärtuse config.xml hiljem värskendada.

4. Klõpsake nuppu **Lisa**.

Tõuketeatised lisandmoodul on installitud.

####<a name="install-the-device-plugin"></a>Seadme lisandmooduli installimine

Kasutage sama toimingut, mida kasutasite tõuketeatised lisandmooduli installimiseks, kuid te leiate seadme lisandmooduli Core lisandmoodulid loendis (klõpsake nuppu **lisandmoodulid** > **Core** seda üles leida). Peate selle lisandmooduli hankimiseks platvormi nimi (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Tõuketeatised käivitamisel seadme registreerimine

Esialgu me lisada mõned minimaalsete koodi Androidi jaoks. Hiljem teeme mõned small muudatused käivitamiseks iOS-i või Windows 10.

1. **RegisterForPushNotifications** kõne ajal tagasihelistamise sisselogimise protsessi või **onDeviceReady** meetodit allosas lisamine

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Selles näites kuvatakse helistaja **registerForPushNotifications** pärast autentimist õnnestub, mis on soovitatav rakenduse Tõuketeatiste nii autentimist kasutades.

2. Lisage uus **registerForPushNotifications** meetod järgmiselt:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Ülaltoodud kood, Asendage `Your_Project_ID` abil soovitud arv projekti ID oma rakenduse [Google arendaja konsooli].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Valikuline) Konfigureerige ja käivitage rakendus Androidi seadmes

Täitke selles jaotises lubamiseks Tõuketeatiste Androidi jaoks.

####<a name="enable-gcm"></a>Luba Firebase'i Cloud sõnumside

Kuna oleme suunatud Google Android platvormi algselt, tuleb lubada Firebase'i Cloud sõnumside. Samamoodi kui olid suunatud Microsoft Windowsi seadmetes, saate lubada WNS tugi.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Tõuketeatised päringuid abil FCM mobiilirakenduse kirjutamata konfigureerimine

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Androidi rakenduse Cordova konfigureerimine

Rakenduses Cordova avada config.xml ja asendada `Your_Project_ID` abil soovitud arv projekti ID oma rakenduse [Google arendaja konsooli].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Avage index.js ja värskendada koodi kasutada oma arvuline projekti.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Androidi seadme jaoks USB silumine konfigureerimine

Oma Android-seadmes rakenduse juurutamiseks peate lubamine USB silumine.  Androidi telefonis tehke järgmist.

1. Avage **sätted** > **telefoni kohta**ja seejärel puudutage soovitud **arv koostada** kuni arendaja režiimis on lubatud (umbes 7 korda).

2. Olles tagasi kohas **sätted** > **Arendaja Valikud** lubamine **USB silumine**ja seejärel oma Androidi telefonis oma arengu arvuti USB-kaabli abil.

Oleme testinud, kasutades Google seos 5 X seadet, kus töötab Android 6.0 (vahukommi).  Siiski tehnika on levinud kogu tänapäevane Androidi vabanemise.

#### <a name="install-google-play-services"></a>Installige Google Play teenused

Tõuketeatised lisandmooduli tugineb Androidi Google Play teenused tõuketeatisi.  

1.  **Visual Studio**, klõpsake nuppu **Tööriistad** > **Androidi** > **Android SDK Manager**, laiendage kausta **lisad** ja märkige ruut veenduge, et iga järgmine SDK-d on installitud.
    * Android 2.3 või uuem versioon
    * Google hoidla paranduse 27 või uuem versioon
    * Google Play teenused 9.0.2 või uuem versioon

2.  Klõpsake **Installida pakettide** ja oodata installimise lõpuleviimiseks.

Praegune nõutav teegid on loetletud [phonegap-lisandmoodul-push installi dokumentatsiooni].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Androidi rakenduse testi tõuketeatised

Saate nüüd testi Tõuketeatiste rakendus töötab ja lisades TodoItem tabelis. Selleks samasse seadmesse või mõnda teise seadmesse, kui kasutate sama taustväärtus. Testige Cordova rakenduse Androidi platvormi ühel järgmistest viisidest:

- **Füüsilise seadmes:**  
Kinnitage oma Androidi seadmes arengu arvuti USB-kaabli abil.  Asemel **Google Android emulaator**, valige **seade**. Visual Studio on rakenduse seade ja käivitage see.  Seejärel saate suhelda seadmes rakendus.  
Arengu kasutusvõimalusi parandada.  Kuva ühiskasutuse rakendusi nagu [Mobizen] saate aitab teil Androidi rakenduse poolt Androidi ekraani veebibrauseris sisse oma arvutis.

- **Android emulaator:**  
Järgnevalt on vajalik, kui emulaator konfigureerimise juhised.

    Veenduge, et teil on kasutusele võtavad või silumine virtuaalse seadmes, mis sisaldab Google API-de määramine mis Sihtvaluuta, nagu on näidatud allpool Androidi virtuaalse seadme (AVD) manager.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Kui soovite kasutada kiirem x86 emulaator, [HAXM draiveri installida](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) ja konfigureerida emulaator seda kasutada.

    Google konto lisamine Androidi seadmes **rakendused**klõpsates > **sätted** > **Lisa konto**ja seejärel järgige viipasid lisamiseks mõne olemasoleva Google'i konto seadmele (soovitatav konto kasutamise asemel luua uue eksemplari).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Enne kui todolist rakenduse käivitamine ja todo uue üksuse lisamine. Sel ajal, kuvatakse olekuala ikoon olekualal. Saate avada märguandesahtel teatise täisteksti kuvamiseks.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Valikuline) Konfigureerida ja käivitada iOS-i

See jaotis on mõeldud Cordova projekti iOS-seadmetes töötavad. Selles jaotises saate vahele jätta, kui te ei tööta iOS-seadmete.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Installimine ja käivitamine iOS-i remotebuild agent Mac-arvutisse või pilvepõhises teenuses

Enne iOS Visual Studio abil saate käivitada Cordova rakendus, avage [iOS-i häälestusjuhend](http://taco.visualstudio.com/en-us/docs/ios-guide/) installida ja käitada remotebuild agent toodud juhiseid.

Veenduge, et saate koostada rakendus iOS-i. Visual Studio iOS-i koostamiseks on vaja juhiseid häälestamise juhend. Kui teil on Mac-arvutis, saate koostada iOS-i teenus nagu MacInCloud remotebuild agent kasutamine. Kohta leiate teemast [käivitada oma iOS-i rakenduse pilveteenuses](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] Tõuketeatised lisandmooduli kasutamine iOS-i läheb vaja XCode 7 või uuemat.

####<a name="find-the-id-to-use-as-your-app-id"></a>Kui rakenduse ID kasutada ID leidmine

Enne, kui teie minirakendus Tõuketeatiste, avatud config.xml rakenduses Cordova registreerida leidmiseks on `id` väärtustes vidina elementi ja kopeerige see hilisemaks kasutamiseks. Järgmine XML, ID on `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Hiljem, kasutage selle identifikaator Apple'i arendaja portaalis ID rakenduse loomisel. (Kui loote erinevat ID rakenduse arendaja portaalis ja soovite kasutada, kas peate teha paar lisatoimingut hiljem muuta selle ID config.xml selle õpetuse. Vidina elementi ID peab vastama rakenduse ID arendaja portaalis.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Tõuketeatiste Apple'i arendaja portaalis jaoks rakenduse registreerimine

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Vaadake videot nähtaval sarnased toimingud](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Azure'i saata Tõuketeatiste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Veenduge, et teie rakenduse ID vastab teie Cordova rakendus

Kui rakendus Apple arendaja konto juba loodud ID vastab config.xml vidina elemendi ID-d, saate selle sammu vahele jätta. Juhul, kui ID-d ei ühti, tehke järgmist:

1. Projekti platvormide jaoks loodud kausta kustutada.

2. Projekti lisandmoodulid kausta kustutada.

3. Projekti node_modules kausta kustutada.

4. Värskendage atribuudi id vidina elemendi config.xml kasutada konto Apple arendaja loodud rakenduse ID-d.

5. Taastada oma projekti.

#####<a name="test-push-notifications-in-your-ios-app"></a>IOS-i rakenduse testi tõuketeatised

1. Visual Studios, veenduge, et selle **iOS-i** juurutamise sihtkoht on valitud, ja valige **seade** ühendatud iOS-seadmes käivitada.

    Saate käivitada teie Arvutiga ühendatud iOS-seadmes iTunes abil. IOS-i simulaatorit ei toeta tõuketeatisi.

2. Vajutage **klahvi F5** ja nuppu **Käivita** Visual Studio projekti koostamine ja käivitage rakendus iOS-seadmes ja siis klõpsake nuppu **OK** , et aktsepteerida tõuketeatisi.

    >[AZURE.NOTE] Konkreetselt peab aktsepteerida tõuketeatisi, rakenduse kaudu. Taotluse ilmneb ainult esimene kord, kui rakendus töötab.

3. Tippige tööülesande rakendus, ja klõpsake plussmärki (+) ikoon.

4. Veenduge, et teatise saamist ja seejärel nuppu OK hülgamiseks teate.

##<a name="optional-configure-and-run-on-windows"></a>(Valikuline) Konfigureerida ja käivitada Windows

See jaotis on mõeldud Apache Cordova rakenduse project opsüsteemi Windows 10 seadmes (PhoneGap tõuketeatised lisandmoodul on toetatud Windows 10). Selles jaotises saate vahele jätta, kui te ei tööta koos Windowsi jaoks.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registreerige oma Windowsi rakenduse jaoks Tõuketeatiste WNS

Visual Studio poe suvandite kasutamiseks loendist Windows target lahenduse platvormide jaoks loodud, nt **Windowsi-x64** või **Windowsi-x86** (vältida Tõuketeatiste jaoks **Windowsi-AnyCPU** ).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Vaadake videot nähtaval sarnased toimingud](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Teate jaoturi WNS konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Teie Cordova rakendus toetab Windows Tõuketeatiste konfigureerimine

Avage konfiguratsiooni designer (Paremklõpsake config.xml ja valige **Kuvakujundaja**), valige **Windowsi** menüü ja valige jaotises **Windowsi Target versiooni** **Windows 10** .

>[AZURE.NOTE] Kui kasutate Cordova versiooni enne Cordova 5.1.1 (soovitatav 6.1.1), peate määrama argumendi Töölauateatisega toega lipu config.xml on tõene.

Tõuketeatised toetamiseks oma vaikimisi (silumine) teatised koostab, avatud build.json fail. Kopeerige "release" konfiguratsiooni konfiguratsioonist silumine.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Pärast värskendamist, eelmise koodi peaks välja nägema järgmine.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Rakenduse koostamine ja veenduge, et teil on tõrkeid. Kliendi rakendus peaks nüüd Registreeruge teatiste Mobile'i rakendus taustväärtus. Korrake iga Windowsi projekti teie lahendus selles jaotises.

####<a name="test-push-notifications-in-your-windows-app"></a>Windowsi rakenduse testi tõuketeatised

Visual Studio, veenduge, et Windowsi platvormi on valitud juurutamise target, nt **Windowsi-x64** või **Windowsi-x86**. Käivitage rakendus Windows 10 arvutis majutusteenuse Visual Studios, valige **Kohalikus arvutis**.

Projekti koostamine ja käivitage rakendus nuppu Käivita.

Rakenduses, tippige uus todoitem nimi ja klõpsake plussmärki (+) ikooni, et lisada see.

Veenduge, et teatis on saanud, kui üksus on lisatud.

##<a name="next-steps"></a>Järgmised sammud

* Lugege [Teatis jaoturi] Tõuketeatiste kohta.
* Kui te pole seda juba teinud, jätkake oma Apache Cordova rakendusse [Lisada] autentimise õpetuse.

Siit saate teada, kuidas kasutada funktsiooni SDK-d.

* [Apache Cordova SDK]
* [ASP.net-i serveri SDK]
* [Node.js serveri SDK]

<!-- URLs -->
[Lisades autentimine]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova Lühijuhend]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Gmaili kontosse]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google arendaja konsooli]: https://console.developers.google.com/home/dashboard
[phonegap-lisandmoodul-push installi dokumentatsioon]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio ühenduse 2015]: http://www.visualstudio.com/
[Visual Studio tööriistad Apache Cordova jaoks]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Teatis jaoturi]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.net-i serveri SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js serveri SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
