<properties
    pageTitle="Ühenduseta sünkroonimise lubamine Azure'i Mobile'i rakendus (Android)"
    description="Saate teada, kuidas kasutada rakenduse Mobile'i rakendused oma Androidi rakenduse vahemälu ja Sünkrooni ühenduseta andmete"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Teie Androidi mobiilirakenduse ühenduseta sünkroonimise lubamine

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Ülevaade

Selles õpetuses hõlmab ühenduseta sünkroonimise funktsioon Azure'i Mobile'i rakendused Androidi jaoks. Ühenduseta sünkroonimine võimaldab lõppkasutajal kasutamine mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust. Muudatused on talletatud kohalik andmebaas. Kui seade on võrguühenduse taastamisel, on need muudatused sünkroonida remote taustväärtus.

Kui see on teie esimene kogemus Azure'i mobiilirakenduste, tuleb esmalt täita õpetuse [Androidi rakenduse loomine]. Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate lisama oma projekti andmed Accessi laiend paketid. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Ühenduseta sünkroonimise funktsiooni kohta lisateabe saamiseks vaadake teemat [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste kohta].

## <a name="update-the-app-to-support-offline-sync"></a>Värskendage rakenduse toetamiseks ühenduseta sünkroonimine

Ühenduseta sünkroonimise, lugeda ja kirjutada *sünkroonimine tabeli* ( *IMobileServiceSyncTable* liidest kasutades), mis on osa **SQLite** andmebaasi oma seadmes.

Tõuketeatised ja tõmmake muutuste vahel seadmest ja Azure Mobile teenused, kasutage *sünkroonimine kontekstis* (*MobileServiceClient.SyncContext*), mille saate lähtestada koos kohalikult andmete talletamiseks kohalikku andmebaasi.

1. Klõpsake `TodoActivity.java`, kommentaari välja mõiste `mToDoTable` ja kommenteerige välja sünkroonimine tabeli versioon:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. Rakenduses selle `onCreate` meetod kommentaari välja olemasoleva lähtestamine `mToDoTable` ja selle määratluse kommentaari:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. Rakenduses `refreshItemsFromTable` kommentaari välja määratlus `results` ja selle määratluse kommentaari:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Kommentaari välja määratlus `refreshItemsFromMobileServiceTable`.

5. Kommenteerige välja määratlus `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Kommenteerige välja määratlus `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Rakenduse testimine

Selles jaotises proovida käitumine WiFi-ühendust ja seejärel lülitage WiFi-ühendus luua mõne ühenduseta stsenaarium.

Kui lisate andmed üksusi, nad on olevad SQLite kohalikku salve, kuid ei sünkroonita mobiilsideteenuse ülevaade enne seda, kui vajutate nuppu **Värskenda** . Muud rakendused võivad olla erinevad nõuded kui andmeid on vaja sünkroonida, kuid selles õpetuses on demo eesmärgil kasutaja selgesõnaliselt seda taotleda.

Kui vajutate seda nuppu, käivitab uue ülesande tausta. See esmalt sunnib kõik muudatused sünkroonimise konteksti, siis tõmbab kõigi muudetud andmetega Azure'i kohaliku tabeli abil kohalikku salve.

### <a name="offline-testing"></a>Ühenduseta testimine

1. Viige seadme või simulaatorit *Lennukikujuline*režiimis. See loob ka ühenduseta stsenaarium.

2. Mõned *ToDo* üksuste lisamine või mõne üksuse lõpetatuks märkida. Sulgege seadme või simulaatorit (või sunniviisiliselt sulgege rakendus) ja taaskäivitage. Veenduge, et teie muudatused on antud kestnud seadme kuna neid hoitakse kohaliku SQLite salvestada.

3. Kas SQL vahend, nt *SQL Server Management Studio*või REST klient, nt *viiuldaja* või *postiljon*Azure *TodoItem* tabeli sisu kuvamine. Veenduge, et uued üksused _on sünkroonitud serveriga_

    + Node.js kirjutamata, valige [Azure portaali](https://portal.azure.com/)ja rakenduses Mobile taustväärtus klõpsake **Lihtne tabelite** > **TodoItem** sisu kuvamiseks on `TodoItem` tabel.
    + .Net-i kirjutamata, saate vaadata SQL-i tööriist, näiteks *SQL Server Management Studio*või REST klient, nt *viiuldaja* või *postiljon*tabeli sisu.

4. Lülitage sisse Wi-Fi-seadme või simulaatorit. Järgmine, klõpsake nuppu **Värskenda** .

5. TodoItem andmeid vaadata uuesti Azure'i portaalis. Uute ja muudetud TodoItems peaks nüüd kuvatama.

## <a name="additional-resources"></a>Lisaressursid

* [Ühenduseta sünkroonimise Azure mobiilirakendused]

* [Cloud tiitelleht: Azure'i mobiilsideseadmete teenuste ühenduseta sünkroonimise] \(Märkus: video on Mobile Services, kuid ühenduseta sünkroonimine toimib sarnaselt Azure Mobile'i rakendused\)


<!-- URLs. -->

[Ühenduseta sünkroonimise Azure mobiilirakendused]: app-service-mobile-offline-data-sync.md

[Androidi rakenduse loomine]: app-service-mobile-android-get-started.md

[Pilveteenuse tiitelleht: Ühenduseta sünkroonimine Azure Mobile teenused]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

