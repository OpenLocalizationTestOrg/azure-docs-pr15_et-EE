<properties
    pageTitle="Lisada autentimise Apache Cordova Mobile'i rakendustega | Azure'i rakendust Service"
    description="Saate teada, kuidas kasutada mobiilirakenduste autentida kasutajate Apache Cordova rakenduse kaudu identiteedipakkujad, sh Google, Facebooki, Twitteri ja Microsoft Azure'i rakendust Service."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Rakenduse Apache Cordova autentimise lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Kokkuvõte

Selles õppetükis saate lisada autentimine todolist Kiirjuhend projekti Apache Cordova toetatud identiteedipakkuja abil. Selle õpetuse põhineb [Alustamine mobiilirakenduste] õpetuse, mille peate.

##<a name="register"></a>Registreerimist rakenduse autentimiseks ja rakenduse teenuse konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Vaadake videot nähtaval sarnased toimingud](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nüüd saate kontrollida oma kirjutamata anonüümse juurdepääsu keelamist. Visual Studios, avage projekt lõpetatud õpetusega [Alustamine mobiilirakenduste], loodud rakenduse käivitada **Google Android emulaator** ja kontrollige ühenduse ootamatu tõrge kuvatakse pärast rakenduse käivitab.

Järgmiseks värskendate rakenduse kasutajate autentimiseks enne taotluse ressursid kirjutamata mobiilirakenduse kaudu.

##<a name="add-authentication"></a>Autentimise lisamine rakendusse

1. Avage oma projekti **Visual**Studio ja seejärel soovitud `www/index.html` faili redigeerimiseks.

2. Otsige üles soovitud `Content-Security-Policy` metasilt pea jaotises.  Peate OAuthi host lubatud allikate loendisse lisamiseks.

  	| Pakkuja               | SDK pakkuja nimi | OAuthi Host                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | AAD               | https://login.Windows.net   |
  	| Facebooki               | Facebooki          | https://www.Facebook.com    |
  	| Google                 | Google            | https://accounts.google.com |
  	| Microsoft              | microsoftaccount  | https://login.live.com      |
  	| Twitteri                | Twitteri           | https://API.twitter.com     |

    Näide sisu--turbepoliitika (rakendatud Azure Active Directory jaoks) on järgmine:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Mida tuleks asendada `https://login.windows.net` OAuthi Host ülaltoodud tabelist.  See metasilt lisateabe saamiseks lugege [sisu turbepoliitika dokumentatsiooni] .

    Pange tähele, et mõned autentimisteenuse pakkujate ei nõua sisu turbepoliitika muutub vastav mobiilsideseadmetes kasutamisel.  Näiteks muutusteta sisu turbepoliitika vajalike Google autentimist kasutades Androidi seadmes.

3. Avatud on `www/js/index.js` faili redigeerimiseks, otsige üles soovitud `onDeviceReady()` meetod ja jaotises kliendi loomine koodi lisamine järgmiselt:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Pange tähele, et järgmine kood Asenda olemasolev kood, mis loob tabeli viide ja värskendab UI.

    Login() meetod algab pakkuja autentimist. Login() meetod on asünkroonse funktsioon, mis tagastab lubadus JavaScripti.  Ülejäänud lähtestamine on paigutada lubadus vastuse nii, et see on täide, kuni login() meetod on lõpule viidud.

4. Asendage kood, mille just lisasite `SDK_Provider_Name` sisselogimise pakkuja nimi. Näiteks Azure Active Directory, kasutada `client.login('aad')`.

4. Käivitage oma projekti.  Kui projekt on lõpule jõudnud, käivitamine, kuvatakse rakenduse OAuthi sisselogimislehe pakkuja valitud autentimist.

##<a name="next-steps"></a>Järgmised sammud

* Lugege rohkem Azure'i rakendust Service [Autentimise kohta] .
* Jätkake õpetuse Apache Cordova rakenduse [Tõuketeatised] lisamisega.

Siit saate teada, kuidas kasutada funktsiooni SDK-d.

* [Apache Cordova SDK]
* [ASP.net-i serveri SDK]
* [Node.js serveri SDK]

<!-- URLs. -->
[Mobile'i rakenduste kasutamise alustamine]: app-service-mobile-cordova-get-started.md
[Sisu turbepoliitika dokumentatsioon]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Tõuketeatised]: app-service-mobile-cordova-get-started-push.md
[Autentimise kohta]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.net-i serveri SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js serveri SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
