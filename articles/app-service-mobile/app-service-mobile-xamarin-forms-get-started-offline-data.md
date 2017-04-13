<properties
    pageTitle="Ühenduseta sünkroonimise lubamine Azure'i Mobile'i rakendus (Xamarin.Forms) | Microsoft Azure'i"
    description="Saate teada, kuidas rakenduse teenuse mobiilirakenduse abil Xamarin.Forms rakenduse vahemälu ja Sünkrooni ühenduseta andmed"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Teie Xamarin.Forms mobiilirakenduse ühenduseta sünkroonimise lubamine

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Ülevaade
Selle õpetuse tutvustab ühenduseta sünkroonimise funktsiooni Azure Mobile rakenduste Xamarin.Forms. Ühenduseta sünkroonimine võimaldab lõppkasutajal mobiilirakenduse--vaatamine, lisamine või muutmine andmed, isegi siis, kui seal on võrguühendusega suhelda. Muudatused on talletatud kohalik andmebaas. Kui seade on võrguühenduse taastamisel, sünkroonitakse remote teenuse muudatustest.

Selle õpetuse põhineb mobiilirakenduste, kui täidate õpetuse [Loo Xamarin iOS-i rakendus] loodud Xamarin.Forms Kiirjuhend lahendus. Kiirjuhend lahendus Xamarin.Forms sisaldab toetamiseks ühenduseta sünkroonimine, mille just peab olema lubatud. Selles õpetuses värskendate Kiirjuhend lahendus Azure'i mobiilirakenduste ühenduseta funktsioonid sisse lülitada. Me esile tõsta ühenduseta kohased koodi rakenduses. Kui te ei kasuta allalaaditud Kiirjuhend lahenduse, peate lisama oma projekti andmed Accessi laiend paketid. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine][1].

Ühenduseta sünkroonimise funktsiooni kohta lisateabe saamiseks vaadake teemat [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Ühenduseta sünkroonimise funktsiooni Kiirjuhend lahendus

Ühenduseta sünkroonimine kood on kaasatud projekti C# preprocessor suuniseid abil. Kui soovitud **ÜHENDUSETA\_sünkroonimine\_lubatud** sümbol on määratletud, need kood teed kaasatud koostamine. Windowsi rakendused, tuleb teil installida SQLite platvormi.

1. Paremklõpsake Visual Studio lahendus > **Haldamine Nugeti pakettide lahenduse...**, siis otsimine ja installige **Microsoft.Azure.Mobile.Client.SQLiteStore** Nugeti paketi kõik projektide lahendus.

2. Solution Exploreris TodoItemManager.cs faili avamine **Portable** projekti nimi, mis on kaasaskantav klassi Raamatukogu projekti, siis kommentaari järgmist preprocessor direktiivi:

        #define OFFLINE_SYNC_ENABLED

4. (Valikuline) Installige Windows seadmed toetavad, üks järgmistest SQLite käitusaja paketid.

    * **Windows 8.1 käitusaja:** Installige [Windows 8.1 SQLite][3].
    * **Windows Phone 8.1:** Installige [Windows Phone 8.1 jaoks SQLite][4].
    * **Universaalne Windowsi platvormi** Installige [SQLite Universal Windows Universal][5].

    Kuigi on Kiirjuhend ei sisalda Universal Windows projekti, toetatakse ühes kohas Windowsi platvormi Xamarin vormidega.

5. (Valikuline) Paremklõpsake iga Windowsi rakenduse project **Viited** > **Lisada viide**, laiendage kaust **Windowsi** > **laiendid**.
    Lubage vastav **SQLite for Windows** SDK **Visual C++ 2013 käitusaja for Windows** SDK koos.
    SQLite SDK nimed iga Windowsi platvormi veidi erineda.

## <a name="review-the-client-sync-code"></a>Vaadake üle kliendi sünkroonimine kood

Siin on lühiülevaade, mis juba sisaldab kuueosalisest koodi sees on `#if OFFLINE_SYNC_ENABLED` suuniseid. Ühenduseta sünkroonimise funktsiooni on kaasaskantav klassi Raamatukogu projekti TodoItemManager.cs projekti fail. Funktsiooni kontseptuaalne ülevaade artiklist [Ühenduseta andmete sünkroonimine Azure Mobile'i rakendused][2].

* Enne mis tahes tabeli toiminguid teha, tuleb lähtestada kohalikku salve. Kohalikku salve andmebaas on lähtestatud **TodoItemManager** klassi ehitaja abil järgmine kood:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Järgmine kood loob uue kohaliku SQLite andmebaasi **MobileServiceSQLiteStore** klassi abil.

    **DefineTable** meetod loob tabeli kohalikku salve, mis vastab esitatud tüüp väljad.  Kaasata kõik veerud, mis on kaugandmebaas ei ole tüüp. On võimalik veerud alamhulga talletamiseks.

* **TodoTable** välja **TodoItemManager** on **IMobileServiceSyncTable** tüüpi **IMobileServiceTable**asemel. See klassi kasutab kõigi kohaliku andmebaasi loomine, lugemine, värskendada ja kustutada (CRUD) tabeli toimingud. Saate ise otsustada, kui need muudatused on lükata kirjutamata mobiilirakenduse abil, mille **PushAsync** **IMobileServiceSyncContext**. Sünkroonimine kontekstis aitab säilitada tabeli seosed jälgimine ja lükates muudatused kõigis tabelites kliendi rakendus on muudetud **PushAsync** kutsumisel.

    Järgmised **SyncAsync** meetodit nimetatakse Sünkrooni mobiilirakenduse kirjutamata:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    See näide kasutab lihtsa tõrketöötluse vaikimisi sünkroonimine sündmuseohjuri abil. Päris rakenduse käsitleks erinevaid vigu, nt võrgu läbilaskevõime ja serveri konfliktide kohandatud **IMobileServiceSyncHandler** rakendamise abil.

## <a name="offline-sync-considerations"></a>Ühenduseta sünkroonimine kaalutlused

Valimis, nimetatakse **SyncAsync** meetodit ainult käivitamisel ja sünkroonimine on nõutav.  Androidi või iOS-i rakenduse sünkroonimise käivitamiseks rippmenüü loendis üksuste; Windowsi jaoks, kasutage nuppu **Sünkrooni** . Tegelike rakenduses, võite ka muuta sünkroonimine päästik kui võrgu olek muutub.

Kui tõmmake vastu tabel, milles on ootel kohaliku värskenduste jälitatud konteksti poolt, et tõmmata toiming automaatselt päästikute eelnev kontekstis tõuketeatised. Kui värskendamine, lisamine ja selle valimi üksuste lõpuleviimine, jätate konkreetsete **PushAsync** kõne.

Esitatud kood remote TodoItem tabeli kõik kirjed on esitatakse selle kohta päring, kuid samuti on võimalik, saates päringu id ja päringu **PushAsync**kirjete filtreerimiseks. Lisateabe saamiseks vt [Ühenduseta andmete sünkroonimine Azure Mobile'i rakendused] *Suureneva sünkroonimine* [2].

## <a name="run-the-client-app"></a>Käivitage rakendus klient

Ühenduseta sünkroonimine kohe lubatud, kus klientrakenduse vähemalt ühe korra käitanud iga platvormil asustamiseks andmebaasi kohalikku salve. Hiljem simuleerida ka ühenduseta stsenaarium ja kohalikku salve andmeid muuta, kui rakendus töötab ühenduseta.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Sünkroonimine toimimist kliendi rakenduse värskendamine

Selles jaotises muuta kliendi projekti simuleerida ka ühenduseta stsenaarium on sobimatu URL-i abil oma taustväärtus. Teise võimalusena saate välja lülitada võrguühenduste, kui teisaldate seadme "Lennukikujuline-režiimis.  Kui lisate või muuta andmete üksusi, on need muudatused olevad kohalikku salve, kuid sünkroonitud kirjutamata andmesalve kuni ühendus on taastatud.

1. Solution Exploreris **Portable** projekti Constants.cs projekti faili avada ja muuta väärtus `ApplicationURL` osutamiseks sobimatu URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. **Portable** projekti TodoItemManager.cs faili avamine ja seejärel lisada mõne **jaole juba** base **erandi** klassi **SyncAsync** **proovida... saagi** plokk. Selle **jaole juba** ploki kirjutab erandi teade konsooli, järgmiselt:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Luua ja käivitada kliendi rakendus.  Lisage mõned uued üksused. Pange tähele, et iga katse sünkroonimise funktsiooni taustväärtus konsool logitakse erand. Nende uute üksuste olemas ainult kohalikku salve seni, kuni ta saab lükata Mobiiliversiooni taustväärtus. Kliendi rakendus käitub nagu siis, kui see on ühendatud taustväärtus, toetab kõiki loomine, lugemine, värskendamine, kustutamise (CRUD) toimingud.

4. Sulgege rakendus ja taaskäivitage veenduda, et uued üksused, mille lõite on jätkunud kohalikku salve.

5. (Valikuline) Visual Studio abil saate vaadata oma Azure'i SQL-andmebaasi tabelist andmed tagaandmebaas ei muutunud.

    Visual Studio, avage **Server Explorer**. Liikuge oma andmebaasi **Azure**->**SQL andmebaase**. Andmebaasi paremklõpsake ja valige käsk **Ava Exploreris SQL serveri objekti**. Nüüd saate sirvida oma SQL-andmebaasi tabeli ja selle sisu.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Kliendi rakenduse taaskäivitamiseks oma mobiilse taustväärtus värskendamine

Selles jaotises uuesti mobiilsideseadmete kirjutamata, mis jäljendab jälle võrguühenduse rakenduse rakenduse. Värskenda žest täitmisel andmed on sünkroonitud oma mobiilse taustväärtus.

1. Avage uuesti Constants.cs. Vea soovitud `applicationURL` õige URL-i osutamiseks.

2. Taastada ja käivitada kliendi rakendus. Rakendus proovib sünkroonida mobiilirakenduse kirjutamata pärast käivitamist. Veenduge, et erandid on sisse logitud konsooli silumine.

3. (Valikuline) SQL serveri objekti Exploreri või REST tööriista nagu viiuldaja või [postiljon]värskendatud andmeid vaadata[6]. Pange tähele, et andmed on sünkroonitud tagaandmebaas ja kohalikku salve.

    Pange tähele, et andmed on sünkroonitud andmebaas ja kohalikku salve ja sisaldab üksusi, saate lisada oma rakenduse katkestatud.

## <a name="additional-resources"></a>Lisaressursid

* [Ühenduseta sünkroonimise Azure mobiilirakendused][2]
* [Azure'i mobiilirakendused .NET SDK juhendid][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md