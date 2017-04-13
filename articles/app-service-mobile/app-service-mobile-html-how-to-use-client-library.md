<properties
    pageTitle="Kuidas kasutada JavaScripti SDK Azure mobiilirakendused"
    description="Kuidas kasutada v Azure'i mobiilirakenduste kohta"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Kuidas kasutada Azure mobiilirakendused JavaScripti kliendi teek

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Sellest juhendist ütleb teile, et teha levinud stsenaariumi uusima [JavaScripti SDK Azure'i mobiilirakenduste]abil. Kui olete kasutanud Azure'i mobiilirakenduste, täitke esmalt [Azure'i Mobile'i rakendused Kiirkäivituse] loomiseks on kirjutamata ja tabeli loomine. Sellest juhendist me keskenduda mobiilsideseadmete kirjutamata HTML-i ja JavaScripti veebirakenduste abil.

## <a name="supported-platforms"></a>Toetatud platvormid

Piiratud Brauseritugi suurte brauserite praeguse ja viimase versioonidega: Google Chrome'i, Microsoft Edge, Microsoft Internet Explorer ja Mozilla Firefox.  Loodame SDK funktsiooni, mis tahes üsna tänapäevane brauseriga.

Paketi levitatakse ühes kohas JavaScripti mooduli, nii, et see toetab globaalsed AMD CommonJS vormingud.

##<a name="Setup"></a>Häälestamise ja eeltingimused

Sellest juhendist eeldab, et olete loonud mõne taustväärtus tabeli. Sellest juhendist eeldab, et tabelis on tabelid sama skeemiga, need õpetused.

Azure'i Mobile'i rakendused JavaScripti SDK installimisel saate teha kaudu soovitud `npm` käsk:

```
npm install azure-mobile-apps-client --save
```

Pärast installimist teek asub `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Kopeerige see fail teie web ala.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Teegi saate kasutada ka ES2015 mooduli, CommonJS keskkonnas, nt Browserify ja Webpack ja kui ka AMD Raamatukogu sees.  Näiteks:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Kuidas: kasutajate autentimiseks

Azure'i rakendust Service toetab autentimist ja lubatakse rakenduse kasutajad abil erinevad väliste identiteedipakkujad: Facebook, Google, Microsoft Account ja Twitteri. Õigusi saab seada tabelites piirata juurdepääsu teatud toimingute ainult autenditud kasutajad. Autenditud kasutaja ID abil rakendada serveri skriptide autoriseerimine reeglid. Lisateavet leiate teemast õpetusega [Alustamine autentimist] .

Kahe autentimise puhul on toetatud: serveri kulgemist ja kliendi kulgemist.  Serveri voogu pakub lihtsaim autentimise versioon, kuna see põhineb pakkuja web autentimine liides. Kliendi voogu võimaldab põhjalikult integreerimine seadme võimaluste näiteks ühekordse sisselogimise kuna see põhineb teenusepakkuja kohased SDK-d.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Kuidas: teie mobiiliteenuse rakenduse konfigureerimine välise ümber suunata URL.

Mitut tüüpi JavaScripti rakenduste abil loopback võimalus toime OAuthi UI puhul.  Need funktsioonid on järgmised.

* Kohalik ei tööta teie teenus
* Reaalajas uuestilaadimine funktsiooniga ära raames
* Ümbersuunamine rakenduse teenusesse autentimist. 

Kohalik töötab võib põhjustada probleeme, kuna see vaikimisi konfigureeritud rakenduse autentimise ainult oma kirjutamata mobiilirakenduse kaudu juurdepääsu lubamiseks. Järgmiste juhiste abil saate muuta rakenduse Teenusesätted, et lubada autentimine, kui kohalik töötab server:

1. [Azure'i portaali] sisse logida
2. Liikuge oma Mobile'i rakendus taustväärtus.
3. Valige **Ressursi Exploreri** menüü **Tööriistad arengu** .
4. Klõpsake uuel vahekaardil või aknas resource explorer jaoks oma mobiilirakenduse kirjutamata avamiseks **avage** .
5. Laiendage **config** > **authsettings** sõlm oma rakenduse.
6. Klõpsake nuppu **Redigeeri** ressursi redigeerimise lubamiseks.
7. Otsige **allowedExternalRedirectUrls** element, mis peaks olema null. Lisage oma URL-ide massiivi.

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Asendage URL-ide massiivis URL-id oma teenus, mis on selles näites on `http://localhost:3000` kohaliku Node.js valimi teenuse kohta. Võite kasutada ka `http://localhost:4400` sulin teenuse või mõne muu URL-i, olenevalt sellest, kuidas teie rakendus on konfigureeritud.

8. Klõpsake lehe ülaosas **Lugemis-ja kirjutamisõigusega**klõpsake **sellele** värskenduste salvestamiseks.

Samuti peate lisama sama loopback URL-ide CORS nimekiri sätted:

1. Liikuge tagasi [Azure portaali].
2. Liikuge oma Mobile'i rakendus taustväärtus.
3. Klõpsake menüüs **API** **CORS** .
4. Sisestage tekstiväljale tühja **Lubatud päritolu** iga URL-i.  Luuakse uus tekstiväli.
5. Klõpsake nuppu **Salvesta**
    
Pärast soovitud kirjutamata värskendab, on võimalik kasutada uut loopback URL-id oma rakenduses.

<!-- URLs. -->
[Azure'i Mobile'i rakendused kiire algus]: app-service-mobile-cordova-get-started.md
[Alustamine autentimine]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure'i portaal]: https://portal.azure.com/
[JavaScripti SDK Azure mobiilirakendused]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

