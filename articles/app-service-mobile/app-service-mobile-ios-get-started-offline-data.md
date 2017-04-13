<properties
    pageTitle="Ühenduseta sünkroonimise lubamine Azure'i Mobile'i rakendus (iOS)"
    description="Saate teada, kuidas kasutada rakenduse teenuse mobiilirakenduste vahemälu ja sünkroonimine oma iOS-i rakenduse võrguühenduseta andmete"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Oma iOS-i mobiilirakenduse ühenduseta sünkroonimise lubamine

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Ülevaade

Selles õpetuses käsitletakse ühenduseta sünkroonimisfunktsiooni Azure'i mobiilirakenduste iOS-i. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust. Muudatused on talletatud kohalik andmebaas; Kui seade on võrguühenduse taastamisel, on need muudatused sünkroonida remote taustväärtus.

Kui see on teie esimene kogemus Azure'i mobiilirakenduste kohta, tuleb esmalt täita õpetuse [iOS rakenduse loomine]. Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate lisama oma projekti andmed Accessi laiend paketid. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Ühenduseta sünkroonimise funktsiooni kohta lisateabe saamiseks vaadake teemat [Andmete ühenduseta sünkroonimise Azure'i mobiilirakenduste kohta].

## <a name="review-sync"></a>Vaadake üle kliendi sünkroonimine kood

Kliendi projekti allalaaditud [loomine iOS rakenduse] juhend on juba sisaldab koodi, mis toetavad ühenduseta sünkroonimise kasutamine kohaliku andmebaasi Core andmete põhjal. Selles jaotises on juba kaasatava kuueosalisest koodi kokkuvõte. Funktsiooni kontseptuaalne ülevaade artiklist [Ühenduseta andmete sünkroonimine Azure Mobile'i rakendused].

Azure'i mobiilirakenduste ühenduseta sünkroonimine sünkroonimisfunktsiooni võimaldab kasutajal suhelda kohaliku andmebaasi, kui võrguühendus puudub juurdepääs. Nende funktsioonide kasutamiseks rakenduse käivitage sünkroonimine kontekstis `MSClient` ja kohalikku salve. Seejärel viitavad tabeli kaudu soovitud `MSSyncTable` kasutajaliidese.

1. **QSTodoService.m** (eesmärk-C) või **ToDoTableViewController.swift** (kiire), Pange tähele liige tüüpi `syncTable` on `MSSyncTable`. Ühenduseta sünkroonimine kasutab seda sünkroonimine tabeli kasutajaliidese asemel `MSTable`. Sünkroonimine tabeli kasutamisel kõik toimingud minge kohalikku salve ja sünkroonitakse ainult remote taustväärtus konkreetsete ja toimingutega.

    Viide sünkroonimine tabeli saamiseks kasutage meetodit `syncTableWithName` kohta `MSClient`. Ühenduseta sünkroonimise funktsiooni eemaldamiseks kasutage `tableWithName` asemel.

2. Enne mis tahes tabeli toiminguid teha, tuleb lähtestada kohalikku salve. Siin on oluline kood. 
    
    **Eesmärk-C**:
    
    Klõpsake soovitud `QSTodoService.init` meetod:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Kiire**:
    
    Klõpsake soovitud `ToDoTableViewController.viewDidLoad` meetod:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    See loob kohaliku poe liidest kasutades `MSCoreDataStore`, mis on esitatud SDK Mobile'i rakendused. Saate selle asemel mõne muu kohalikku salve Sisesta rakendades on `MSSyncContextDataSource` Protocol (protokoll). 
    
    Lisaks esimene parameeter `MSSyncContext` konflikti sündmuseohjuri määrata. Kuna on möödas `nil`, saame vaikimisi konflikti sündmuseohjuri, mida ei suuda konfliktide lahendamine.
    
3. Nüüd loome tegelik sünkroonimine toimingu ja andmete toomine remote taustväärtus.

    **Eesmärk-C**:
    
    `syncData`esmalt sunnib muudatusi, siis helistab `pullData` andmete toomiseks remote taustväärtus. Omakorda meetodit `pullData` uued andmed, mis vastab teie päring saab:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Kiire**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    Eesmärk-C versioon, klõpsake `syncData`, esmalt kutsume `pushWithCompletion` sünkroonimine kontekstist. See meetod on liige `MSSyncContext` (asemel sünkroonimine tabeli Items) Kuna see push muudatused üle kõik tabelid. Serveriga, saadetakse ainult need kirjed, mida on muudetud kuidagi kohalikult (CUD toimingute kaudu). Seejärel helper `pullData` nimetatakse, mis nõuab `MSSyncTable.pullWithQuery` remote andmete toomiseks ja kohaliku andmebaasi salvestada.
    
    Kiire versioon on pole kõne `pushWithCompletion`. Selle põhjuseks on tõuketeatised toiming polnud tingimata vajalik. Kui sünkroonimine kontekstis tabel, mis teeb tõuketeatised toiming on ootel muudatusi, tõmmata alati probleemid push esmalt. Kui teil on rohkem kui ühe sünkroonimine tabeli, see aga kõige paremini selgesõnaliselt kõne tõuketeatised tagamaks, et kõik on ühesugune kõikides seotud tabelitest.
    
    Nii eesmärk-C ja kiire versioonides meetodit `pullWithQuery` võimaldab teil määrata päringu kirjete filtreerimiseks soovite tuua. Selles näites toob päringu lihtsalt kõik kirjed serveri `TodoItem` tabel.
    
    Teine parameeter `pullWithQuery` on päringu ID, mida on kasutatud *astmeline sünkroonimine*. Astmeline sünkroonimine laadib ainult need kirjed, mis on pärast viimast sünkroonimist, kasutades kirje muudetud `UpdatedAt` ajatempli (nimetatakse `updatedAt` sisse kohalikku salve.) Päringu ID peaks olema kirjeldav string, mis on kordumatu iga loogilise päringu rakenduse. Loobuda suureneva sünkroonimine, et edastada `nil` päringu ID Pange tähele, et see võib olla potentsiaalselt ebaefektiivne, kuna see toob kõik kirjed iga pull tegevuse.

5. Eesmärk-C rakenduse sünkroonib kui me muuta või lisada andmed, kasutaja sooritab värskendamise žest, ja käivitada. Kiire rakenduse sünkroonib, kui kasutaja sooritab värskendamise žest ja käivitada. 

Kuna rakenduse sünkroonib iga kord, kui andmed on muudetud (eesmärk-C) või rakenduse käivitamisel (eesmärk-C ja kiire), rakenduse eeldab, et kasutajal on võrguühendus. Mõne muu jaotises uuendame rakenduse, et kasutajad saavad redigeerida, isegi siis, kui nad on ühenduseta.

## <a name="review-core-data"></a>Vaadake üle Core andmemudel

Core ühenduseta andmesalve kasutamisel peate määratlema oma andmemudelis kindla tabelid ja väljad. Valimi rakenduse juba sisaldab andmemudelit õiges vormingus. Selles jaotises kuvatakse tutvustame need tabelid ja nende kasutamise.

- Avage **QSDataModel.xcdatamodeld**. On määratletud--nelja tabeleid, kolme, mida kasutatakse Tarkvaraarenduskomplektist, ja ühe tabeli jaoks soovitud todo üksused ise:     *MS_TableOperations: jälgimise üksused, mida tuleb serveriga sünkroonida    * MS_TableOperationErrors: jälgimise vigu, mis tehakse ühenduseta sünkroonimise ajal     *MS_TableConfig: jälgimise viimati värskendatud viimase toimingu Sünkrooni kõik pull toimingute jaoks aja    * TodoItem : Todo üksuste talletamiseks. Süsteemi veergude **createdAt**, **updatedAt**ja **versioon** on valikuline Süsteemiatribuudid.

>[AZURE.NOTE] Azure'i Mobile'i rakendused SDK jätab veergude nimed on koos "**``**". Ärge kasutage seda eesliite midagi muud kui süsteemi veerud, muidu teie veerunimede remote taustväärtus kasutamisel muudetakse.

- Ühenduseta sünkroonimise funktsiooni kasutamisel tuleb määratleda süsteemi tabelid, nagu allpool näidatud.

    ### <a name="system-tables"></a>Süsteemi tabelid

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Atribuut  |    Tüüp     |
  	|----------- |   ------    |
  	| ID         | Täisarv 64  |
  	| itemId     | String      |
  	| atribuudid | Binaarandmeid |
  	| Tabel      | String      |
  	| tableKind  | Täisarv 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Atribuut  |    Tüüp     |
  	|----------- |   ------    |
  	| ID         | String      |
  	| operationId | Täisarv 64 |
  	| atribuudid | Binaarandmeid |
  	| tableKind  | Täisarv 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Atribuut  |    Tüüp     |
  	|----------- |   ------    |
  	| ID         | String      |
  	| klahv        | String      |
  	| keyType    | Täisarv 64  |
  	| Tabel      | String      |
  	| väärtus      | String      |

    ### <a name="data-table"></a>Andmetabel

    **TodoItem**

  	| Atribuut    |  Tüüp   | Märkus                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID           | String, mis on tähistatud nõutav  | primaarvõtme remote poes                            |
  	| lõpuleviimine     | Kahendmuutuja | Todo üksuseväli                                        |
  	| teksti         | String  | Todo üksuseväli                                        |
  	| createdAt | Kuupäev    | (valikuline) kaardid createdAt süsteemi atribuut         |
  	| updatedAt | Kuupäev    | (valikuline) kaardid updatedAt süsteemi atribuut         |
  	| versioon   | String  | (valikuline) kasutatakse konfliktide, kaardid versiooni tuvastamiseks |


## <a name="setup-sync"></a>Rakenduse sünkroonimine käitumise muutmine

Selles jaotises saate muuta rakenduse nii, et see ei sünkrooni rakenduse avakuval, või kui lisamise ja värskendamise üksusi, kuid ainult siis, kui toimub žest nuppu Värskenda.

**Eesmärk-C**:

1. **QSTodoListViewController.m**, muutke **viewDidLoad** meetodi abil eemaldada kõne `[self refresh]` meetod lõpus. Nüüd andmed pole sünkroonitakse serveriga rakenduse avakuval, kuid selle asemel on sisu kohalikku salve.

2. Määratlemise muuta **QSTodoService.m**, `addItem` nii, et see ei sünkroonita üksuse järele. Eemaldamine on `self syncData` blokeerida ja asendage järgmist:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Määratlemise muuta `completeItem` nagu eespool; Eemaldage puhul `self syncData` ja asendage järgmist:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Kiire**:

1. Klõpsake `viewDidLoad` **ToDoTableViewController.swift**, klõpsake välja need kaks joont rakenduse avakuval sünkroonimise peatamiseks kommentaar. Selle artikli kirjutamise ajal kiire Todo rakenduse värskendamine teenuse kui keegi lisab või üksuse rakenduse avakuval on lõpule jõudnud.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Rakenduse testimine

Selles jaotises saate ühenduse ka ühenduseta stsenaarium jäljendamiseks sobimatu URL. Andmete üksuste lisamisel nad on toimus põhiandmed kohalikku salve, kuid mobiilsideseadmete kirjutamata sünkroonitud.

1. Muuta Mobile'i rakendus URL **QSTodoService.m** sobimatu URL ja käivitage rakendus uuesti.

    **Eesmärk-C** QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Kiire** ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Mõned todo üksusi lisada. Funktsiooni simulaatorit sulgemine (või sunniviisiliselt sulgege rakendus) ja taaskäivitage. Veenduge, et teie muudatused on kestnud.

3. Remote TodoItem tabeli sisu vaatamiseks tehke järgmist.

    + Node.js kirjutamata, valige [Azure portaali](https://portal.azure.com/)ja rakenduses Mobile taustväärtus klõpsake **Lihtne tabelite** > **TodoItem** sisu kuvamiseks on `TodoItem` tabel.
    + .Net-i kirjutamata, saate vaadata SQL-i tööriist, näiteks SQL Server Management Studio või REST klient, nt viiuldaja või postiljon tabeli sisu.

    Veenduge, et uued üksused *on sünkroonitud serveriga* :

4. Muuta URL-i kohta naasmiseks õigeid **QSTodoService.m** ja käivitage rakendus uuesti. Sooritada värskendamise žest vahelt üksuste loendit. Kuvatakse edenemise liitboksi.

5. TodoItem andmeid uuesti vaadata. Uute ja muudetud TodoItems peaks nüüd kuvatama.

## <a name="summary"></a>Kokkuvõte

Ühenduseta sünkroonimise funktsiooni toetamiseks kasutatakse funktsiooni `MSSyncTable` kasutajaliides ja lähtestada `MSClient.syncContext` koos kohalikku salve. Sel juhul kohalikku salve oli Core andmete põhjal andmebaasi.

Core andmete kohalikku salve kasutamisel tuleb määratleda [õige Süsteemiatribuudid](#review-core-data)mitu tabelit.

Tavaline CRUD toimingute Azure'i mobiilirakenduste töötada, kui rakendus on endiselt ühendatud, kuid kõik need toimingud tekkida vastu kohalikku salve.

Kui soovime kalendriüksusi kohalikku salve sünkroonimine serveriga, kasutatakse selle `MSSyncTable.pullWithQuery`meetod.


## <a name="additional-resources"></a>Lisaressursid

* [Ühenduseta sünkroonimise Azure mobiilirakendused]

* [Cloud tiitelleht: Azure'i mobiilsideseadmete teenuste ühenduseta sünkroonimise] \(Märkus: video on Mobile Services, kuid ühenduseta sünkroonimine toimib sarnaselt Azure Mobile'i rakendused\)

<!-- URLs. -->


[IOS rakenduse loomine]: app-service-mobile-ios-get-started.md
[Ühenduseta sünkroonimise Azure mobiilirakendused]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Pilveteenuse tiitelleht: Ühenduseta sünkroonimine Azure Mobile teenused]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
