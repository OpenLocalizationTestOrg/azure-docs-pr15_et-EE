<properties
    pageTitle="Azure'i AD Cordova alustamine | Microsoft Azure'i"
    description="Kuidas luua Cordova rakendus, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de kasutamine OAuthi."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integreerimine Azure AD Apache Cordova rakendus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova võimaldab töötada HTML5/JavaScript rakendusi, milles saate käivitada mobiilsideseadmetes täieõiguslik kohalikke rakendustena.
Azure AD, saate lisada Cordova rakenduste ärifunktsioonide hinde autentimist. Tänu Cordova lisandmoodul, mähkimine Azure AD kohalikke SDK-d iOS, Android, Windowsi poe ja Windows Phone, saate täiustamiseks rakenduse tugi Logi sisse oma AD kontode, pääseda juurde Office 365 ja Azure API ja isegi kaitsta oma kohandatud veebi-API kõned.

Selles õpetuses kasutame Apache Cordova lisandmooduli jaoks Active Directory autentimise Raamatukogu (ADAL) parandamiseks lihtsa rakenduse järgmised funktsioonid:

-   Vaid mõne koodiread AD kasutaja autentimiseks ja saada märgiks Azure AD Graph API helistamine.
-   Kasutage seda luba autonoomsest Graph API päringu selle kausta ja tulemite kuvamiseks  
-   ADAL Turbeloa vahemälu minimeerimine autentimisviibad kasutaja jaoks kasutada.

Selleks, peate:

2. Azure AD Rakenduse registreerimine
2. Teie taotlus sõned rakenduse koodi lisamine
3. Päringute Graph API luba kasutamiseks koodi lisamine ja tulemite kuvamiseks.
4. Kõik platvormid soovite suunata ja Cordova ADAL lisandmooduli Cordova juurutamise projekti loomiseks ja kontrollimiseks lahendus emulators.

## <a name="0--prerequisites"></a>*0. eeltingimused*

Lõpuleviimiseks peate õppeteema:

- Kui teil on rakenduse arengu õigustega konto Azure AD rentnikku
- Arenduskeskkond, mis on konfigureeritud kasutama Apache Cordova  

Kui teil on mõlemad juba loodud, jätkake otse samm 1.

Kui teil on Azure AD rentniku, leiate [juhised, kuidas saada üks siin](active-directory-howto-tenant.md).

Kui teil pole Apache Cordova häälestamine teie arvutis, installige järgmist:

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (NPM paketi halduri kaudu saab hõlpsasti installida: `npm install -g cordova`)

Pange tähele, et need töötavad arvutis ja selle Mac-arvutisse.

Iga suunata platvorm on erinevate eeltingimused.

- Luua ja käivitada Windowsi Tahvelarvuti või telefoni rakenduse versioon
    - [Visual Studio 2013 for Windows Update 2 või uuem](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Kiire või mõni muu versioon).
- Koostamine ja iOS-i käivitamine
    -   Xcode 5.x või suurem. Laadige see http://developer.apple.com/downloads või [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios-sim](https://www.npmjs.org/package/ios-sim) – võimaldab iOS-i rakendusi käivitada iOS simulaatorit käsurea kaudu (terminal kaudu saab hõlpsasti installida: `npm install -g ios-sim`)

- Luua ja käivitada rakendus Androidi jaoks
    - Installige [Java arengu Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) või uuem versioon. Veenduge, et `JAVA_HOME` (keskkonnas muutuv) on õigesti häälestatud vastavalt JDK installi tee (nt C:\Program Files\Java\jdk1.7.0_75).
    - Installimine [Androidi SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) ja lisada `<android-sdk-location>\tools` asukoht (nt C:\tools\Android\android-sdk\tools) oma `PATH` keskkonnas muutuv.
    - Avage Androidi SDK Manager (nt kaudu terminal: `android`) ja installida
    - *Android 5.0.1 (API 21)* platvorm SDK
    - *Android SDK koostamine-tööriistade* versioon 19.1.0 või uuem versioon
    - *Androidi tugi hoidla* (Lisad)

  Androidi sdk ei paku mis tahes vaikimisi emulaator eksemplari. Looge uus käivitades `android avd` terminal ja seejärel käsku *Loo...* , kui soovite Androidi rakendust käivitada emulaator kaudu. Soovitatav *Api tase* on 19 või uuem versioon, vt [AVD Manager] (http://developer.android.com/tools/help/avd-manager.html) Android emulaator ja loomise suvandite kohta lisateavet.


## <a name="1--register-an-application-with-azure-ad"></a>*1. rakenduse registreerimine Azure AD*

Märkus: selle __juhise täitmine on valikuline__. Õpetuse esitatud eelnevalt ettevalmistatud väärtused, mis võimaldab teil vaadata oma rentniku ettevalmistamine tegemata tegelikkuses valimi. Siiski on soovitatav, et seda juhist ja tutvuda protsessi, kuna see on vaja, kui loote oma rakendustes.

Azure AD annavad sõned ainult teadaolevad rakendused. Enne Azure AD saate kasutada rakenduste, peate oma rentniku jaoks selle kirje loomiseks.  Uus rakendus oma rentniku registreerimiseks

-   [Azure'i haldusportaal](https://manage.windowsazure.com) sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige rentniku, kuhu soovite rakenduse registreerida.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua uue **Kohalikke klientrakendusega** (hoolimata sellest, et Cordova rakendused on HTML-i-põhine, loome omakliendi rakenduse siin nii `Native Client Application` peab olema märgitud suvand; vastasel korral rakendus ei tööta).
    -   Rakenduse **nimi** kirjeldada oma rakenduse lõppkasutajad
    -   **Suunake URI** on kasutatud sõned naasmiseks rakenduse URI. Sisestage `http://MyDirectorySearcherApp`.

Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest: leiate **konfigureerimine** vahekaardil äsja loodud rakendust.

Selleks, et käivitada `DirSearchClient Sample`, äsja loodud rakenduse loa päringu _Azure AD Graph API_:
-   **Konfigureerimine** vahekaardil, otsige üles jaotis "Õigused soovite muud rakendused".  "Azure Active Directory" rakenduse, lisada **Accessi kataloogi sisselogitud kasutaja** õiguste jaotises **Delegeeritud õigused**.  See võimaldab rakenduse päringu Graph API kasutajate jaoks.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2 klooni valimi rakenduse hoidla juhend on nõutav*

Shell või käsurea, tippige järgmine käsk:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Cordova rakenduse loomine*

On mitu võimalust Cordova rakenduse loomiseks. Selles õpetuses kasutame funktsiooni Cordova käsurea liides (CLI).
Shell või käsurea, tippige järgmine käsk:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

See loob kausta struktuuri ja tellingud Cordova projekti, starter projekti sisu kopeerimine www alamkausta.
Uue DirSearchClient kausta teisaldada.

    cd .\DirSearchClient

Lisage nimekiri lisandmooduli vajalik kasutada Graph API.

     cordova plugin add cordova-plugin-whitelist

Järgmiseks kõik platvormid, mida soovite lisada. Selleks, et töötamise valimi, peate vähemalt ühte allpool käsud käivitada. Pange tähele, et te ei saa jäljendada iOS-i Windowsi või Mac Windows/Windows Phone

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Lisaks saate lisada projekti ADAL Cordova lisandmooduli.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. kasutajate autentimiseks ja saada sõned AAD koodi lisamine*

Selles õpetuses arendate rakendus annab tühjal luu kataloogi otsingufunktsiooni, kus lõppkasutaja tippige pseudonüüm igale kasutajale kataloogis ja visualiseerida mõne lihtsa atribuute.  Starteri projekt sisaldab lihtsa kasutajaliidese (www/index.html) rakendus ja tellingud, mida kasutaja kasutajaliidese sidumiste ja tulemite kuvamiseks (www/js/index.js) loogika üles lihtsa rakenduse sündmuse tsüklit, juhtmed määratlus. Identiteedi tööülesannete rakendamise loogika lisamine on ainus teie jaoks välja jäetud.

Väga Kõigepealt peate tegema on tutvustada koodi Protocol (protokoll) väärtusi, mida kasutatakse AAD rakenduse ja ressursse, saate suunata. Ehitada Turbeloa taotlusi hiljem kasutatakse neid väärtusi. Lisage koodilõigu all tipus index.js faili.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Funktsiooni `redirectUri` ja `clientId` väärtused peaksid olema samad väärtused, mis kirjeldab rakenduse AAD. Leiate need konfigureerimine menüü Azure'i portaalis, nagu on kirjeldatud selles õpetuses etappi 1.
Märkus: kui olete valinud ei registreerimisel oma rentniku uue rakenduse, saate lihtsalt kleepida ülaltoodud eelnevalt konfigureeritud väärtused on – mis võimaldab teil näha valimi töötab, kuigi tuleks alati loote oma kirje jaoks mõeldud tootmise rakenduste.

Järgmiseks tuleb lisada tegelik Turbeloa taotluse kood. Lisage järgmised koodilõigu vahel on `search `ja `renderdata `määratlusi.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
See funktsioon kaupa peamised kaheosaline uurime.
See näide on loodud töötama mis tahes rentnikuga olema seotud konkreetse üks erinevalt. Kasutab "/ levinud" lõpp-punkti, mis võimaldab kasutajal sisestada mis tahes konto autentimine ajal ja suunab taotluse rentniku see kuulub.
Selle meetodi esimene osa dokumendist kuvamiseks, kui seal on juba salvestatud märgiks - ja kui on, kasutab rentnikud, selle saatjaks jaoks uuesti lähtestamine ADAL ADAL vahemälu. See on vajalik, et vältida täiendavat viipasid, nagu "/ levinud" Kasuta alati tulemuseks palub kasutajal sisestada uus konto.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Teine osa meetodit teostab proper tokewn kutse.
Funktsiooni `acquireTokenSilentAsync` meetod soovib ADAL juurde naasmiseks ilma nähtaval mis tahes UX. märgiks määratud ressursi Mis võib juhtuda siis, kui vahemälu on juba märgiks sobiva Accessi salvestatud, või kui ei värskenda märgiks, saate kasutada, et saada uue juurdepääsu luba ilma shwoing küsimata.
Kui mis katse ebaõnnestub, saame langeb tagasi `acquireTokenAsync` – mis küsib nähtavalt kasutaja autentida.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nüüd kus oleme luba, saame lõpuks autonoomsest Graph API ja soovime otsingu päringu. Lisage järgmised koodilõigu all olevat `authenticate` määratlus.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Algus punkti failid saadaval on barebone Kasutuskogemuse sisestamise kasutaja pseudonüüm tekstivälja. See meetod kasutab seda väärtust päringu koostamiseks, kombineerida juurdepääsu luba, saatke see graafik ja sõeluda tulemused. RenderData meetod, mis on juba olemas käivitamine punkti faili, hoolitseb soovite tulemused visualiseerida.

## <a name="4-run"></a>*4. Käivita*
Rakendus on lõpuks käivitamiseks valmis! See on väga lihtne: kui rakendus käivitub, Sisestage tekstiväljale kasutaja, mida soovite otsida - ja seejärel klõpsake nuppu. Teil palutakse autentimist. Eduka autentimise ja eduka otsingu korral kuvatakse otsida kasutaja atribuudid. Hiljem jookseb täidab ilma küsimata tänu varem omandatud luba vahemälu olemasolu näitab otsing.
Juhised konkreetsete rakendus töötab erineda platvormi.

####<a name="windows-10"></a>Windows 10:

   Tahvelarvuti:`cordova run windows --archs=x64 -- --appx=uap`

   Mobiil (nõuab Windows10 mobiilsideseade Arvutiga ühendatud):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Märkus__: esimese käitamisel, võidakse teilt arendaja litsentsi sisse logima. Üksikasjalikumat teavet teemast [arendaja litsentsi](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1/Tahvelarvuti:

   `cordova run windows`

   __Märkus__: esimese käitamisel, võidakse teilt arendaja litsentsi sisse logima. Üksikasjalikumat teavet teemast [arendaja litsentsi](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Ühendatud seadmes käivitamiseks tehke järgmist.`cordova run windows --device -- --phone`

   Vaikimisi emulaator käivitamiseks tehke järgmist.`cordova emulate windows -- --phone`

   Kasutage `cordova run windows --list -- --phone` kuvamiseks saadaval kõigi sihtkohtade ja `cordova run windows --target=<target_name> -- --phone` konkreetse seadme või emulaator rakenduse käivitamiseks (nt `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Ühendatud seadmes käivitamiseks tehke järgmist.`cordova run android --device`

   Vaikimisi emulaator käivitamiseks tehke järgmist.`cordova emulate android`

   __Märkus__: Veenduge, et teie loodud *AVD* halduris, nagu see on *grammatikavigu* emulaator eksemplari.

   Kasutage `cordova run android --list` kuvamiseks saadaval kõigi sihtkohtade ja `cordova run android --target=<target_name>` konkreetse seadme või emulaator rakenduse käivitamiseks (nt `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Ühendatud seadmes käivitamiseks tehke järgmist.`cordova run ios --device`

   Vaikimisi emulaator käivitamiseks tehke järgmist.`cordova emulate ios`

   __Märkus__: Veenduge, et teil on `ios-sim` paketi käivitamiseks emulaator installitud. *Üksikasjalikumat teavet leiate teemast grammatikavigu* .

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Kasutage `cordova run --help` näha täiendavad koostamine ja käivitage suvandid.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Te saate nüüd liikuda täpsemate (ok, ja huvitavamaks) stsenaariumid.  Võite proovida:

[Turvaline Node.js Web API Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
