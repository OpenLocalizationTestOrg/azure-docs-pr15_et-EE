<properties
    pageTitle="Azure'i mobiilirakendused Apache Cordova lisandmooduli kasutamise kohta"
    description="Azure'i mobiilirakendused Apache Cordova lisandmooduli kasutamise kohta"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Kuidas kasutada Azure mobiilirakendused Apache Cordova kliendi teek

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Sellest juhendist ütleb teile, et teha levinud stsenaariumi, kasutades uusima [Apache Cordova lisandmooduli Azure'i mobiilirakenduste kohta]. Kui teil on Azure Mobile'i rakendused esimese täieliku [Azure'i Mobile'i rakendused Kiirkäivituse] loomiseks on kirjutamata uue tabeli loomine ja valmismallide Apache Cordova projekti alla laadida. Sellest juhendist me keskenduda kliendipoolne Apache Cordova lisandmoodul.

## <a name="supported-platforms"></a>Toetatud platvormid

See SDK toetab Apache Cordova v6.0.0 ja hiljem iOS-i, Androidi ja Windowsi seadmed.  Platvormi tugi on järgmine:

* Androidi API 19-24 (KitKat pähklimass kaudu)
* iOS 8.0 ja uuemad versioonid.
* Windows Phone 8.0
* Windows Phone 8.1
* Universaalne Windowsi platvormi

##<a name="Setup"></a>Häälestamise ja eeltingimused

Sellest juhendist eeldab, et olete loonud mõne taustväärtus tabeli. Sellest juhendist eeldab, et tabelis on tabelid sama skeemiga, need õpetused. Sellest juhendist eeldab, et olete lisanud oma koodi Apache Cordova lisandmoodul.  Kui te olete teinud, võib Apache Cordova lisandmooduli lisamine projekti käsureal:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

[Esimene Apache Cordova rakenduse]loomise kohta leiate lisateavet teemast oma dokumendid.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Kuidas: kasutajate autentimiseks

Azure'i rakendust Service toetab autentimist ja lubatakse rakenduse kasutajad abil erinevad väliste identiteedipakkujad: Facebook, Google, Microsoft Account ja Twitteri. Õigusi saab seada tabelites piirata juurdepääsu teatud toimingute ainult autenditud kasutajad. Autenditud kasutaja ID abil rakendada serveri skriptide autoriseerimine reeglid. Lisateavet leiate teemast õpetusega [Alustamine autentimist] .

Autentimise Apache Cordova rakenduse kasutamisel tuleb järgmine Cordova lisandmoodulid saadaval:

* [Cordova-lisandmoodul-seade]
* [inappbrowser-Cordova-lisandmoodul]

Kahe autentimise puhul on toetatud: serveri kulgemist ja kliendi kulgemist.  Serveri voogu pakub lihtsaim autentimise versioon, kuna see põhineb pakkuja web autentimine liides. Kliendi voogu võimaldab põhjalikult integreerimine seadme võimaluste näiteks ühekordse sisselogimise kuna see põhineb teenusepakkuja kohased seadme SDK-d.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Kuidas: teie mobiiliteenuse rakenduse konfigureerimine välise ümber suunata URL.

Mitut tüüpi Apache Cordova rakenduste abil loopback võimalus toime OAuthi UI puhul.  OAuthi UI puhul localhost põhjustada probleeme, kuna autentimisteenus ainult teab, kuidas kasutada teenust vaikimisi.  Probleemne OAuthi UI puhul näited.

- Sulin emulaator.
- Reaalajas koos Ionic Laadi uuesti.
- Mobiilse taustväärtus kohalikult töötab
- Töötab mobiilsideseadmete kirjutamata erinevate Azure'i rakendust Service kui ühe andev autentimist.

Kohalike sätete konfiguratsiooni lisamiseks tehke järgmist

1. [Azure'i portaali] sisse logida
2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu Mobile rakenduse nimi.
3. Klõpsake **menüü Tööriistad**
4. **Ressursi Exploreri** järgima menüü, klõpsake käsku **Valige**.  Uus aken või tab avab.
5. Laiendage **config**, **authsettings** sõlmed saidi Vasakpoolsel navigeerimispaanil.
6. Klõpsake nuppu **Redigeeri**
7. Otsige elemendi "allowedExternalRedirectUrls".  On seada tühi või massiivi väärtustest.  Muutke väärtuseks järgmine väärtus:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Teenuse URL-id, URL-ide asendada.  Näiteks "http://localhost:3000" (teenuse Node.js valimi) või "http://localhost:4400" (teenuse sulin).  Kuid idest on näited - olukord, sh näidetes nimetatud teenuste võivad olla erinevad.
8. Kuva paremas ülanurgas nuppu **Lugemis-ja kirjutamisõigusega** .
9. Klõpsake rohelise **sellele** nupule.

Selles etapis salvestatakse sätted.  Sulgege eelvaatega brauseriaken, kuni lõpetanud sätete salvestamine.
Rakenduse teenust CORS sätete lisada ka loopback idest:

1. [Azure'i portaali] sisse logida
2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu Mobile rakenduse nimi.
3. Sätete tera avatakse automaatselt.  Kui seda ei juhtu, klõpsake käsku **Kõik sätted**.
4. Klõpsake menüüs API **CORS** .
5. Esitatud URL, mida soovite lisada, väljale ja vajutage sisestusklahvi ENTER.
6. Sisestage täiendavad URL-ide vastavalt vajadusele.
7. Klõpsake nuppu **Salvesta** sätted salvestada.

Kulub muudatuse jõustumiseks uute sätete ligikaudu 10 – 15 minutit.

##<a name="register-for-push"></a>Kuidas: Tõuketeatiste Register

Installige [phonegap-lisandmoodul-push] Tõuketeatiste käsitlema.  Selle lisandmooduli saate hõlpsasti lisada abil soovitud `cordova plugin add` käsk käsureal või kaudu Git lisandmooduli installer Visual Studio sees.  Järgmine kood Apache Cordova rakenduse registreerib seadme jaoks lükketeatiste:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Tõuketeatiste saatmine serverist teatis jaoturi SDK abil.  Ära saada kunagi Tõuketeatiste otse kliendid. Nii saab käivitada soovitud Teenustõkkerünnak teatis jaoturi või PNS.  PNS võiks keelata liiklust tulemusena sellise eest.

<!-- URLs. -->
[Azure'i portaal]: https://portal.azure.com
[Azure'i Mobile'i rakendused kiire algus]: app-service-mobile-cordova-get-started.md
[Alustamine autentimine]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova lisandmooduli Azure mobiilirakendused]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[oma esimese Apache Cordova rakenduse]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[tõuketeatised-phonegap-lisandmoodul]: https://www.npmjs.com/package/phonegap-plugin-push
[Cordova-lisandmoodul-seade]: https://www.npmjs.com/package/cordova-plugin-device
[inappbrowser-Cordova-lisandmoodul]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
