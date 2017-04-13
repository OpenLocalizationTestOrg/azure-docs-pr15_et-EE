<properties
    pageTitle="Ühenduseta sünkroonimise lubamine Azure'i mobiilirakenduse (Cordova) | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada rakenduse teenuse Mobile'i rakendus Cordova rakenduse vahemälu ja Sünkrooni ühenduseta andmed"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Teie Cordova mobiilirakenduse ühenduseta sünkroonimise lubamine

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Selle õpetuse tutvustab Cordova ühenduseta sünkroonimise funktsioon Azure'i mobiilirakenduste kohta. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust. Muudatused on talletatud kohalik andmebaas; Kui seade on võrguühenduse taastamisel, sünkroonitakse remote teenuse muudatustest.

Selle õpetuse põhineb mobiilirakenduste, kui täidate [Apache Cordova Kiire alustamine]õpetuse loodud Cordova Kiirjuhend lahendus. Selles õpetuses värskendate Kiirjuhend lahendus lisada Azure mobiilirakenduste ühenduseta funktsioone. Ühenduseta kohased koodi rakenduses kuvatakse ka esile tõsta.

Ühenduseta sünkroonimise funktsiooni kohta lisateabe saamiseks vaadake teemat [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste kohta]. API kasutamise kohta lisateabe saamiseks vt selle lisandmooduli [README] -faili.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Ühenduseta sünkroonimise Kiirjuhend lahenduse lisamine

Ühenduseta sünkroonimine koodi tuleb lisada rakendus. Ühenduseta sünkroonimine nõuab cordova-sqlite-salvestusruumi lisandmoodul, mis toob lisatakse automaatselt teie rakendus kui projekt sisaldab Azure'i mobiilirakenduste lisandmoodul. Kiirjuhend projekt sisaldab nii need lisandmoodulid.

1. Visual Studio Solution Exploreris avage index.js ja asendada järgmine kood

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    selle koodi:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Järgmiseks asendage järgmine kood:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    selle koodi:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Eelmise koodi lisamised kohalikku salve lähtestada ja määratleda kohaliku tabeli, mis vastab veeru väärtused, mida kasutatakse teie Azure tagasi end. (Te ei pea kaasata kõik veeruväärtuste järgmine kood.)

    Saate helistades **getSyncContext**viide sünkroonimine kontekstis. Sünkroonimine kontekstis aitab säilitada tabeli seosed jälgimine ja lükates muudatused kõigis tabelites kliendi rakendus on muudetud **tõuketeatised** kutsumisel.

3. Värskendage Mobile'i rakendus rakenduse URL-i rakenduse URL.

4. Järgmiseks asendage järgmine kood:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    selle koodi:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Eelmise koodi lähtestab sünkroonimine konteksti ja siis kõned getSyncTable (getTable) asemel saada kohaliku tabeli.

    See kood kasutab kõigi kohaliku andmebaasi loomine, lugemine, värskendamine ja kustutamine (CRUD) tabeli toimingud.

    See teostab lihtsa tõrketöötluse sünkroonimise konfliktide kohta. Päris rakenduse käsitleks erinevaid vigu, nt võrgu läbilaskevõime, serveri konfliktide ja teistele. Koodi näiteid leiate [ühenduseta sünkroonimine valimi].

5. Järgmisena lisage see funktsioon tegelik sünkroonimine sooritamiseks.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Saate otsustada, millal lükata muudatused mobiilirakenduse kirjutamata helistades **tõuketeatised** kliendi poolt kasutatud **syncContext** objekti. Näiteks saate lisada **syncBackend** kõne nupp sündmuseohjuri, nt nupp Sünkrooni rakenduse või saate lisada kõne **addItemHandler** funktsiooni sünkroonida iga kord, kui lisatakse uus üksus.

##<a name="offline-sync-considerations"></a>Ühenduseta sünkroonimine kaalutlused

Valimis, nimetatakse **lükketeatiste** meetodit **syncContext** ainult rakenduse käivitamisel sisselogimise funktsiooni tagasihelistamise.  Tegelike rakenduses, võite ka muuta selle sünkroonimise funktsiooni käivitada käsitsi või kui võrgu olek muutub.

Tõmmake täidetakse vastu tabel, mis on ootel kohaliku värskenduste jälitatud konteksti poolt, käivitab pull see toiming automaatselt eelmise kontekstis tõuketeatised. Värskendamisel, lisamine ja lõpuleviimine üksused selles näites saate jätate konkreetsete **tõuketeatised** kõne, kuna see võib olla üleliigsed (esimene kontroll [seletusfail] hetkeseisu funktsioonide jaoks).

Esitatud kood remote todoItem tabeli kõik kirjed on esitatakse selle kohta päring, kuid samuti on võimalik, saates päringu id ja päringu **tõuketeatised**kirjete filtreerimiseks. Lisateavet jaotisest *Suureneva sünkroonimine* on [Võrguühenduseta andmete sünkroonimine Azure Mobile'i rakendused].

## <a name="optional-disable-authentication"></a>(Valikuline) Autentimise keelamine

Kui te ei juba seadistanud autentimist ja ei soovi autentimise enne testimise ühenduseta sünkroonimise häälestamise ja tagasihelistamise funktsiooni sisselogimise kommentaar, kuid jätta koodi sisse uncommented tagasihelistamise funktsiooni.

Pärast kommenteerimise välja login read peaks välja nägema järgmine kood.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Rakenduse sünkroonitakse nüüd Azure taustväärtus asemel kui sisselogimist rakenduse käivitamisel.

## <a name="run-the-client-app"></a>Käivitage rakendus klient

Ühenduseta sünkroonimine kohe lubatud, kus kohe käivitada klientrakenduse vähemalt ühe korra iga platvormi asustamiseks andmebaasi kohalikku salve. Hiljem saate simuleerida ka ühenduseta stsenaarium ja kohalikku salve andmeid muuta, kui rakendus töötab ühenduseta.

## <a name="optional-test-the-sync-behavior"></a>(Valikuline) Testige sünkroonimine käitumine

Selles jaotises muudate kliendi projekti simuleerida ka ühenduseta stsenaarium on sobimatu URL-i abil oma taustväärtus. Kui lisate või muuta andmete üksusi, need muudatused on toimunud kohalikku salve, kuid sünkroonitud kirjutamata andmesalve kuni ühendus on taastatud.

1. Lahenduse index.js projekti faili avada ja muuta rakenduse URL-i osutamiseks sobimatu URL, näiteks järgmine:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Värskenduse index.html, on CSP `<meta>` sobimatu sama URL-iga element.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Luua ja käivitada kliendi rakendus ja konsooli, kui rakendus proovib sünkroonida selle taustväärtus pärast sisselogimist logitakse erandi teade. Saate lisada mis tahes uued üksused on olemas ainult kohalikku salve seni, kuni ta saab lükata Mobiiliversiooni taustväärtus. Kliendi rakendus käitub nagu siis, kui on ühendatud taustväärtus, toetab kõiki loomine, lugemine, värskendamine, kustutamise (CRUD) toimingud.

4. Sulgege rakendus ja taaskäivitage veenduda, et uued üksused, mille lõite on jätkunud kohalikku salve.

5. (Valikuline) Visual Studio abil saate vaadata oma Azure'i SQL-andmebaasi tabelist andmed tagaandmebaas ei muutunud.

    Visual Studio, avage **Server Explorer**. Liikuge oma andmebaasi **Azure**->**SQL andmebaase**. Andmebaasi paremklõpsake ja valige käsk **Ava Exploreris SQL serveri objekti**. Nüüd saate sirvida oma SQL-andmebaasi tabeli ja selle sisu.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Valikuline) Sisselülitamise abil oma mobiilse taustväärtus testimine

Selles jaotises kuvatakse uuesti rakenduse mobiilsideseadmete kirjutamata, mis jäljendab jälle võrguühenduse rakendus. Sisselogimisel, sünkroonitakse teie mobiilse taustväärtus andmed.

1. Avage uuesti index.js ja parandada rakenduse URL-i osutamiseks õige URL-i.

2. Uuesti index.html ja parandada rakenduse URL on CSP `<meta>` element.

3. Taastada ja käivitada kliendi rakendus. Rakendus proovib sünkroonida mobiilirakenduse kirjutamata pärast sisselogimist. Veenduge, et erandid on sisse logitud konsooli silumine.

4. (Valikuline) SQL serveri objekti Exploreri või REST tööriista nagu viiuldaja värskendatud andmeid vaadata. Pange tähele, et andmed on sünkroonitud tagaandmebaas ja kohalikku salve.

    Pange tähele, et andmed on sünkroonitud andmebaas ja kohalikku salve ja sisaldab üksusi, saate lisada oma rakenduse katkestatud.

## <a name="additional-resources"></a>Lisaressursid

* [Ühenduseta sünkroonimise Azure mobiilirakendused]

* [Cloud tiitelleht: Azure'i mobiilsideseadmete teenuste ühenduseta sünkroonimise] \(Märkus: video on Mobile Services, kuid ühenduseta sünkroonimine toimib sarnaselt Azure Mobile'i rakendused\)

* [Visual Studio tööriistad Apache Cordova jaoks]

## <a name="next-steps"></a>Järgmised sammud

* Vaadata ühenduseta sünkroonimine täpsemaid funktsioone, näiteks konfliktilahendus [ühenduseta sünkroonimine näidis]
* Vaadata ühenduseta sünkroonimine API viide [seletusfail]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova Lühijuhend]: app-service-mobile-cordova-get-started.md
[SELETUSFAIL]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[ühenduseta sünkroonimine näidis]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Ühenduseta sünkroonimise Azure mobiilirakendused]: app-service-mobile-offline-data-sync.md
[Pilveteenuse tiitelleht: Ühenduseta sünkroonimine Azure Mobile teenused]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio tööriistad Apache Cordova jaoks]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
