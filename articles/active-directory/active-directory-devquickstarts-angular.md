<properties
    pageTitle="Azure'i AD AngularJS alustamine | Microsoft Azure'i"
    description="Kuidas luua nurga JS ühelt leheküljelt rakendus, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de kasutamine OAuthi."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>AngularJS ühe lehe rakenduste Azure AD turvamine

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD muudab lihtne ja arusaadav lisamine Logi sisse, Logi välja ja secure OAuthi API kõned ühelt leheküljelt rakenduste jaoks.  See võimaldab rakenduse kasutajad saavad Active Directory kontod autentimiseks ja kasutada mis tahes veebi-API, mis on kaitstud Azure AD, nt Office 365 API-d või Azure API-ga.

JavaScripti rakenduste töötamine brauseris, pakub Azure AD Active Directory Authentication Library või adal.js.  Elu ADAL.js on ainus eesmärk on saada juurdepääsu sõned rakenduse hõlpsalt.  Näidata, kui lihtne on siin me tuleb koostada Tehke loendis AngularJS rakenduse mis:

- Kasutades Azure AD identiteedipakkuja rakendusse logib kasutaja.
- Mõne kasutaja teabe kuvamine.
- Turvaline nõuab rakenduse abil teha loendi API esitaja sõnet AAD abil.
- Kasutaja välja rakenduse märke.

Täieliku töötamine rakenduse loomiseks peate:

2. Rakenduse registreerima Azure AD.
3. ADAL installida ja konfigureerida SPA.
5. Turvaline lehtede spaas ADAL abil.

Alustamiseks, [laadige rakendus luukere](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) või [allalaadimine on lõpule viidud valimi](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Peate Azure AD rentniku, kus saate luua kasutajaid ja rakenduse registreerimine.  Kui teil pole veel rentniku, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. register DirectorySearcher rakendus*
Rakenduse kasutajate autentimiseks ja saada sõned lubamiseks esmalt peate oma Azure AD rentniku registreerida.

-   [Azure'i haldusportaal](https://manage.windowsazure.com) sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige, kuhu rakenduse registreerida rentniku.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua mõne uue **veebirakenduse ja/või WebAPI**.
    -   Rakenduse lõppkasutajatele kirjeldada rakenduse **nimi** .
    -   **Suunake Uri** on AAD tagastab märkide asukoht.  On vaikeasukohaks see näidis`https://localhost:44326/`
-   Kui olete registreerimine, AAD määrata rakenduse kordumatu **Kliendi ID**.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü **konfigureerimine** .
- ADAL.js kasutab OAuthi peidetud voogu Azure AD suhelda.  Peate lubama peidetud voogu rakenduse järgi:
    - Laadige rakenduse manifesti, klõpsates nuppu **Haldamine näidata**.
    - Avage manifest ja otsige üles soovitud `oauth2AllowImplicitFlow` atribuut. Määrake selle väärtuseks `true`.
    - Salvestamine ja üles laadida rakenduse manifesti, klõpsates **Halda näidata** taas.

## <a name="2-install-adal--configure-the-spa"></a>*2 installi ADAL & SPA konfigureerimine*
Nüüd, kui teil on rakenduse Azure AD, saate installida adal.js ja kirjutage oma identiteedi seotud kood.

-   Alustamiseks, lisades adal.js TodoSPA projekti Package Manager konsooli abil.
  - [Adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) alla laadida ja lisada selle funktsiooni `App/Scripts/` register.
  - [Adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) alla laadida ja lisada selle funktsiooni `App/Scripts/` register.
  - Laadi iga skripti enne selle `</body>` sisse `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   SPAA taustväärtus abil teha loendi API aktsepteerimiseks sõned brauseri kaudu, peab funktsiooni kirjutamata konfiguratsiooni rakenduse registreerimise teave. Avage TodoSPA projekti `web.config`.  Elementide väärtuste asendamine soovitud `<appSettings>` jaotis kajastamiseks Azure portaali sisestatud väärtused.  Iga kord, kui seda kasutatakse ADAL viitab teie koodi need väärtused.
    -   Funktsiooni `ida:Tenant` on Azure AD rentniku, nt contoso.onmicrosoft.com Domeen.
    -   Funktsiooni `ida:Audience` peab olema **Kliendi ID** rakenduse kopeeritud portaalist.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. Kasutage ADAL secure spaas lehed*
ADAL.js on ehitatud integreerida AngularJS marsruutimiseks ja http pakkujad, mis võimaldab teil kaitsta oma Spa üksikud vaated.

- Klõpsake `App/Scripts/app.js`, tuua adal.js mooduli:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Te saate nüüd lähtestada selle `adalProvider` rakenduse registreerimise väärtustega konfiguratsiooni ka `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Tagamiseks on `TodoList` vaadata rakenduse ainult üks rida koodi on vaja - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Nüüd on teil turvaline ühelt leheküljelt rakenduse kasutajad sisse logida ja anda selle kirjutamata API esitaja Turbeloa kaitstud taotlusi võimaluse.  Kui kasutaja klõpsab soovitud `TodoList` linki adal.js kuvatakse automaatselt ümber suunata Azure AD jaoks Logi sisse vajadusel.  Lisaks automaatselt adal.js mõne access_token lisada Ajaxi taotlused, mis saadetakse rakenduse taustväärtus.  Eespool on tühi adal.js - SPAAD koostamiseks vajalik, kuid on mitmeid funktsioone, mis on kasulikud SPAs:

- Konkreetselt probleemi Logi sisse ja logige välja kutsed saate määratleda oma domeenikontrollerid autonoomsest adal.js funktsioone.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Samuti võite rakenduse UI kasutaja teabe esitamiseks.  Adal teenus on juba lisatud on `userDataCtrl` kontrolleril, et pääseksite selle `userInfo` objekt seotud vaates `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Seal on palju võimalusi, kus te soovite teada, kui kasutaja on sisse logitud või mitte.  Saate kasutada ka funktsiooni `userInfo` objekti sellest teabe kogumine.  Näiteks `index.html` saate kuvada "Login" või "Välju" nuppu autentimise oleku põhjal:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Palju õnne! Oma Azure AD integreeritud ühe lehe rakendus on nüüd valmis.  See saab kasutajate autentimiseks, turvaliselt oma kirjutamata OAuthi 2.0 abil Helistamine ja saada põhiteavet kasutaja.  Kui te pole seda veel teinud, siis nüüd on aeg asustamiseks oma rentniku mõned kasutajatega.  Käivitage oma abil teha loendi SPA ja logige sisse ühe nende kasutajate.  Tööülesannete lisamine kasutajate loend, logige välja ja uuesti sisse.

ADAL.js lihtne lisada kõik need levinud identiteedi funktsioonid rakenduse.  See hoolitseb musta töö teile - vahemälu halduse, OAuthi protokolli tugi, esinemise kasutaja kasutajanimega UI, värskendamine aegunud sõned ja palju muud.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Te saate nüüd liikuda täiendavad stsenaariumid.  Võite proovida:

[Kõne CORS veebi-API on >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
