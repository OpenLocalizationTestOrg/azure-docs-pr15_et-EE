<properties
    pageTitle="Azure'i AD v2.0 AngularJS alustamine | Microsoft Azure'i"
    description="Kuidas koostada nurga JS ühelt leheküljelt rakendus, milles on nii isikliku Microsofti kontoga kasutajad ning töö-või koolikonto."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>AngularJS ühelt leheküljelt rakenduse - .NET sisselogimise lisamine

Selles artiklis lisame mida Microsofti kontole sisselogimine rakendusse AngularJS kasutades Azure Active Directory v2.0 lõpp-punkti.  V2.0 lõpp-punkti võimaldab teha ühe integreerimine rakenduse ja isiklik ja töö/kool kontode autentimiseks.

See valim on lihtne Ülesandeloend ühelt leheküljelt rakenduse talletab tööülesanded on kirjutamata REST API-ga, kirjutada, kasutades .NET 4.5 MVC raamistikku ja turvatud OAuthi esitaja sõnet Azure AD abil.  AngularJS rakendus kasutab meie avatud allikas JavaScripti autentimine teegi [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toime kogu protsessi sisselogimine ja hankida sõned helistamine REST API-ga.  Sama muster saate rakendada muid REST API-d, nagu [Microsoft Graphi](https://graph.microsoft.com)autentimiseks.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="download"></a>Laadi alla

Enne alustamist peate allalaadimine ja installimine Visual Studio.  Seejärel saate saate klooni või [allalaadimine](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) skelett rakendus:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

Skelett rakendus sisaldab trafaretset koodi lihtsa AngularJS rakenduse, kuid pole kõik identiteedi seotud andmeühikuga.  Kui te ei soovi, et jälgida, saate selle asemel klooni või [allalaadimine](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) lõpule viidud proovi.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Rakenduse registreerimine

Esmalt rakenduse [App registreerimise portaali](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)loomine või järgige neid [üksikasjalikke juhiseid](active-directory-v2-app-registration.md).  Veenduge, et:

- Lisage oma rakenduse **Web** platvormi.
- Sisestage õige **URI ümber suunata**. Vaikimisi on see valim on `https://localhost:44326/`.
- Jätke märkeruut **Luba peidetud Flow** lubatud. 

Koopia alla **Rakenduse ID** , mis on seotud rakenduse, peate selle varsti. 

## <a name="install-adaljs"></a>Installige adal.js
Alustamiseks liikuge projekti, saate alla laadida ja installida adal.js.  Kui teil on [bower](http://bower.io/) installitud, saate selle käsu lihtsalt käivitada.  Mis tahes sõltuvus versioon vastuolude puhul valige lihtsalt uuem versioon.
```
bower install adal-angular#experimental
```

Teise võimalusena saate käsitsi alla [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) ja [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Nii faile lisada soovitud `app/lib/adal-angular-experimental/dist` kataloog on `TodoSPA` projekti.

Nüüd avage projekt Visual Studio ja laadimine adal.js peamine lehe sisu lõpus.

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>REST API häälestamine

Kuigi me loomise asju, vaatame saada kirjutamata REST API töötamine.  Avage projekt juures `web.config` ja asendada selle `audience` väärtus.  REST API on seda väärtust kasutama sõned AJAXI taotluste nurga rakendusest saab kinnitada.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>
    
...
```

Mis on kogu aeg me ei kavatse veedavad arutada, kuidas toimib REST API-ga.  Julgelt Otsi koodi, kuid kui soovite lisateavet turvaliseks web API-de Azure AD, vaadake [käesoleva artikli](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>Logi kasutajad
Mõned identiteedi koodi kirjutamiseks aega.  Te olete juba märganud selle adal.js sisaldab AngularJS pakkuja, mis esitatakse kenasti, millel on nurga marsruutimine.  Alustuseks lisage adal mooduli rakendus:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Te saate nüüd lähtestada selle `adalProvider` oma rakenduste ID-ga:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Hästi, nüüd adal.js on kogu teavet, tuleb tagada teie rakendus ja kasutajad sisse logida.  Konkreetse protsessi rakenduse Logi sisse, et kõik kulub on üks rida koodi:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Nüüd, kui kasutaja klõpsab soovitud `TodoList` linki adal.js kuvatakse automaatselt ümber suunata Azure AD sisselogimise vajaduse korral.  Sisselogimine ja väljunud taotlusi saab saata eraldi ka adal.js sisse oma kontrollerid reeglid.

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Kuva Kasutajateave
Nüüd, kui kasutaja on sisse logitud, peate võib-olla sisselogitud kasutaja autentimise andmete oma rakenduse avamiseks.  ADAL.js seab see teave teile selle `userInfo` objekti.  Selle objekti vaates juurdepääsu esmalt lisama adal.js juurkausta ulatuse vastavate kontrolleril:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Seejärel võite käsitleda otse soovitud `userInfo` objekti vaates: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Saate kasutada ka funktsiooni `userInfo` objekti kindlaks teha, kui kasutaja on sisse logitud, või mitte.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Kõne REST API-ga
Lõpuks on aeg saada mõned sõned ja kõne REST API loomine, lugemine, värskendamine ja kustutamine tööülesanded.  Hästi arvan, mida?  Te ei pea *midagi*tegema.  ADAL.js hoolitseb automaatselt saada, vahemällu talletamine ja sõned värskendamine.  See hoolitseb kinnitamise nende sõned väljamineva REST API saadetavate AJAXI päringutele.  

Kuidas see toimib? See on kõik tänu maagiline [AngularJS telefonide](https://docs.angularjs.org/api/ng/service/$http), mis võimaldab transformeerida http sissetulevate ja väljaminevate sõnumite adal.js.  Lisaks eeldab adal.js taotlused saata sama domeeni, nagu akna tuleks kasutada sõned mõeldud sama rakenduse ID AngularJS rakendusena.  Sellepärast, et kasutasime sama rakenduste ID-d nii nurga rakendus ja NodeJS REST API-ga.  Muidugi saate alistada seda käitumist ja adal.js sõned saamiseks muude REST API-de vajadusel - kindlaks teha, kuid selle lihtsa stsenaariumi jaoks vaikesätteid tuleb teha.

Siin on koodilõigu, mis näitab, kui lihtne on päringuid koos esitaja märkide: Azure'i AD.

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Palju õnne!  Oma Azure AD integreeritud ühelt leheküljelt rakendus on nüüd valmis.  Jätkata, kummarda.  See saab kasutajate autentimiseks, turvaliselt kõne oma kirjutamata REST API kasutamine OpenID ühendamine ja saada põhiteavet kasutaja.  Välja kasti, see toetab iga kasutaja isikliku Microsofti Account või töö/kool konto kaudu Azure AD.  Käivitage rakendus ja liikuge brauseris `https://localhost:44326/`.  Logige sisse Microsofti konto või töö/kool konto.  Tööülesannete lisamine kasutaja Ülesandeloend ja logige välja.  Proovige sisse logida konto tüüp. Kui teil on vaja mõnda Azure AD rentniku kasutajate töö/kool, loomiseks [saate teada, kuidas saada üks siin](active-directory-howto-tenant.md) (see on tasuta).

Rohkem õppematerjale v2.0 lõpp-punkti kohta, et Mine tagasi [v2.0 arendaja juhend](active-directory-appmodel-v2-overview.md).  Täiendavad ressursid, vaadake:

- [Azure'i-näidised github >>](https://github.com/Azure-Samples)
- [Azure AD virnas ületäitumisel >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure'i AD dokumentatsioon [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
