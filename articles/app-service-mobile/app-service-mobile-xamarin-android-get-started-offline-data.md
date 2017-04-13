<properties
    pageTitle="Ühenduseta sünkroonimise lubamine Azure'i Mobile'i rakendus (Xamarin Android)"
    description="Saate teada, kuidas rakenduse teenuse mobiilirakenduse abil Xamarin Androidi rakenduse vahemälu ja Sünkrooni ühenduseta andmed"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Teie Xamarin.Android mobiilirakenduse ühenduseta sünkroonimise lubamine

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Ülevaade

Selle õpetuse tutvustab Xamarin.Android ühenduseta sünkroonimise funktsioon Azure'i mobiilirakenduste kohta. Ühenduseta sünkroonimine võimaldab lõppkasutajal mobiilirakenduse--vaatamine, lisamine või muutmine andmed, isegi siis, kui seal on võrguühendusega suhelda. Muudatused on talletatud kohalik andmebaas.
Kui seade on võrguühenduse taastamisel, sünkroonitakse remote teenuse muudatustest.

Selles õpetuses värskendate kliendi projekti kuueosalisest [Xamarin Androidi rakenduse loomine] toetamiseks Azure'i mobiilirakenduste ühenduseta funktsioone. Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate lisama oma projekti andmed Accessi laiend paketid. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Ühenduseta sünkroonimise funktsiooni kohta lisateabe saamiseks vaadake teemat [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste kohta].

## <a name="update-the-client-app-to-support-offline-features"></a>Värskendage kliendi rakenduse toetamiseks ühenduseta funktsioonid

Azure'i mobiilirakenduse ühenduseta funktsioonid võimaldavad teil suhelda kohaliku andmebaasi, kui olete ühenduseta stsenaariumi puhul. Nende funktsioonide kasutamiseks rakenduse, saate lähtestada [SyncContext] kohalikku salve. Seejärel viitavad tabeli [IMobileServiceSyncTable] [IMobileServiceSyncTable] kasutajaliidese kaudu. SQLite kasutatakse seadme kohalikku salve.

1. Avage Visual Studios, Projectis, mida te täitnud [Xamarin Androidi rakenduse loomine] õpetuses Nugeti package manager.  Otsida ja installida **Microsoft.Azure.Mobile.Client.SQLiteStore** Nugeti pakett.

2. Avage fail ToDoActivity.cs ja kommenteerige välja soovitud `#define OFFLINE_SYNC_ENABLED` määratlus.

3. Visual Studios, vajutage **klahvi F5** taastada ja käivitada kliendi rakendus. Rakendus töötab sama, nagu enne märkisite ühenduseta sünkroonimine. Kohalik andmebaas nüüd sisustatakse andmetega, mida saab kasutada ühenduseta stsenaariumi puhul.

## <a name="update-sync"></a>Värskendage rakendus on kirjutamata ühenduse katkestamine

Selles jaotises katkestada ühendus oma mobiilirakenduse kirjutamata simuleerida ka ühenduseta olukord. Andmete üksuste lisamisel oma sündmuseohjuri erandi teade, et rakendus töötab ühenduseta režiimis. Olekus on uute üksuste kohalikku salve lisatud ja sünkroonitakse mobiilirakenduse kirjutamata kui push ühendatud.

1. Ühiskasutuses projekti ToDoActivity.cs redigeerida. **ApplicationURL** osutamiseks sobimatu URL muutmiseks tehke järgmist.

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Wi-Fi- ja seadme mobiilsidevõrgu keelamine või kasutades lennukikujuline režiimis saate tõendada ka ühenduseta käitumine.

2. Vajutage **klahvi F5** koostamine ja käivitage rakendus. Pange tähele oma sünkroonimine rakenduse käivitamisel värskendamine nurjus.

3. Sisestage uued üksused ja pange tähele, et tõuketeatised nurjub [CancelledByNetworkError] oleku iga kord, kui klõpsate nuppu **Salvesta**. Kuid uute todo üksuste olemas kohalikku salve seni, kuni ta saab lükata mobiilirakenduse taustväärtus.  Tootmise rakenduses, kui kommentaarisilte järgmised erandid kliendi rakenduse käitub nagu siis, kui see on endiselt ühendatud mobiilirakenduse taustväärtus.

4. Sulgege rakendus ja taaskäivitage veenduda, et uued üksused, mille lõite on jätkunud kohalikku salve.

5. (Valikuline) Visual Studio, avage **Server Explorer**. Liikuge oma andmebaasi **Azure**->**SQL andmebaase**. Andmebaasi paremklõpsake ja valige käsk **Ava Exploreris SQL serveri objekti**. Nüüd saate sirvida oma SQL-andmebaasi tabeli ja selle sisu. Veenduge, et andmed tagaandmebaas ei muutunud.

6. (Valikuline) Nagu viiuldaja või postiljon ülejäänud tööriista abil päring oma mobiilsideseadmete kirjutamata, GET päringu kasutamine vormi `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Rakenduse taaskäivitamiseks oma mobiilirakenduse kirjutamata värskendamine

Selles jaotises uuesti rakenduse mobiilirakenduse taustväärtus. Rakenduse esmakordsel käivitamisel kuvatakse `OnCreate` sündmuse sündmuseohjuri kõned `OnRefreshItemsSelected`. See nõuab `SyncAsync` sünkroonida oma kohalikku salve koos tagaandmebaas.

1. Avage ToDoActivity.cs ühiskasutuses projekti ja **applicationURL** atribuudi muudatuse taastada.

2. Vajutage klahvi **F5** taastada ja käivitage rakendus. Rakenduse kohaliku muudatuste sünkroonib Azure'i Mobile'i rakendus taustväärtus, kasutades ja toimingute kui selle `OnRefreshItemsSelected` meetod, mis käivitab.

3. (Valikuline) SQL serveri objekti Exploreri või REST tööriista nagu viiuldaja värskendatud andmeid vaadata. Teate andmed on sünkroonitud Azure mobiilirakenduse tagaandmebaas ja kohalikku salve.

4. Klõpsake rakenduses mõne üksuste lõpetada nende kohalikku salve kõrval asuv ruut.

  `CheckItem`kõnede `SyncAsync` sünkroonimine iga lõpetatud üksusega Mobile'i rakendus taustväärtus. `SyncAsync`kõned nii ja. **Iga kord, kui täidate tõmmake vastu kliendi tehtud muudatustest, tabeli push käivitatakse alati automaatselt**. See tagab kõigi tabelite kohalikku salve koos seosed jäävad ühtsete. Selline käitumine võib kaasa tuua mõne ootamatu tõuketeatised. Selline käitumine kohta leiate lisateavet teemast [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste kohta].

## <a name="review-the-client-sync-code"></a>Vaadake üle kliendi sünkroonimine kood

Xamarin kliendi projekti [Xamarin Androidi rakenduse loomine] õpetuse juba lõpetatud allalaaditud sisaldab koodi, mis toetavad ühenduseta sünkroonimise kasutamine SQLite kohaliku andmebaasi. Siin on lühiülevaade, mis juba sisaldab selle kuueosalisest ilt. Funktsiooni kontseptuaalne Ülevaade teemast [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste kohta].

* Enne mis tahes tabeli toiminguid teha, tuleb lähtestada kohalikku salve. Kohalikku salve andmebaas on lähtestatud, kui `ToDoActivity.OnCreate()` aktiveeritakse `ToDoActivity.InitLocalStoreAsync()`. See meetod loob kohaliku SQLite andmebaasi abil soovitud `MobileServiceSQLiteStore` klassi SDK Azure'i mobiilirakenduste kliendi poolt.

    Funktsiooni `DefineTable` meetod loob tabeli kohalikku salve, mis vastab esitatud tüüp väljad `ToDoItem` sel juhul. Kaasata kõik veerud, mis on kaugandmebaas ei ole tüüp. On võimalik ainult alamkomplekt veergude talletamiseks.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* Funktsiooni `toDoTable` liige `ToDoActivity` on selle `IMobileServiceSyncTable` asemel tippige `IMobileServiceTable`. Funktsiooni IMobileServiceSyncTable suunab kõik loomine, lugemine, värskendamine ja kustutamine (CRUD) tabeli toimingud andmebaasi kohalikku salve.

    Saate otsustada, kui muudatused on lükata kirjutamata Azure'i mobiilirakenduse abil helistades `IMobileServiceSyncContext.PushAsync()`. Sünkroonimine kontekstis aitab säilitada tabeli seosed jälgimine ja lükates muudatused kõigis tabelites kliendi rakendus on muudetud, millal `PushAsync` nimetatakse.

    Esitatud koodi kõnesid `ToDoActivity.SyncAsync()` sünkroonimiseks iga kord, kui todoitem loendisse värskendatakse või mõne todoitem lisamisel või lõpetatud. Pärast iga muudatust ei kohaliku sünkroonib kood.

    Esitatud kood kõigi kirjete remote `TodoItem` tabel on esitatakse selle kohta päring, kuid samuti on võimalik, saates päringu id kirjete filtreerimine ja päringu abil `PushAsync`. Lisateavet jaotisest [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste] *Astmeline sünkroonimine* .

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Lisaressursid

* [Ühenduseta sünkroonimise Azure mobiilirakendused]
* [Azure'i mobiilirakendused .NET SDK juhendid][8]

<!-- URLs. -->
[Xamarin Androidi rakenduse loomine]: ../app-service-mobile-xamarin-android-get-started.md
[Ühenduseta sünkroonimise Azure mobiilirakendused]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Androidi rakenduse loomine]: app-service-mobile-xamarin-android-get-started.md
[Ühenduseta sünkroonimise Azure mobiilirakendused]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
