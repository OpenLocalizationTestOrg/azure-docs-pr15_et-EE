<properties
    pageTitle="Kuidas kasutada iOS SDK Azure'i mobiilirakenduste kohta"
    description="Kuidas kasutada iOS SDK Azure'i mobiilirakenduste kohta"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Kuidas kasutamine iOS-i kliendi teek Azure'i mobiilirakenduste kohta

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Sellest juhendist ütleb teile, et teha levinud stsenaariumi Viimane [SDK Azure'i mobiilirakenduste iOS-i]abil[1]. Kui teil on Azure Mobile'i rakendused esimese täieliku [Azure'i Mobile'i rakendused Kiirkäivituse] loomiseks on kirjutamata uue tabeli loomine ja valmismallide iOS-i Xcode projekti alla laadida. Sellest juhendist me keskenduda kliendipoolne iOS SDK. Serveripoolne SDK jaoks soovitud taustväärtus kohta leiate lisateavet teemast serveri SDK HOWTOs.

## <a name="reference-documentation"></a>Dokumentides

IOS-i klient SDK dokumentides asub siin: [Azure'i mobiilirakenduste iOS-i kliendi viide][2].

## <a name="supported-platforms"></a>Toetatud platvormid

IOS-i SDK toetab eesmärk-C projektid, Kiire 2.2 projektide ja kiire 2.3 projektide iOS 8.0 või uuemat versiooni.

"Serveri rahavoogude" autentimise kasutab WebView esitatud UI.  Kui seade ei saa esitada WebView UI, siis teise autentimise meetodit on nõutav on toote väljapoole.  
See SDK sobib seega ei Vaata-tüüpi või sarnaselt piiratud seadmetes.

##<a name="Setup"></a>Häälestamise ja eeltingimused

Sellest juhendist eeldab, et olete loonud mõne taustväärtus tabeli. Sellest juhendist eeldab, et tabelis on tabelid sama skeemiga, need õpetused. Sellest juhendist ka eeldab, et teie kood, saate viidata `MicrosoftAzureMobile.framework` ja importida `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Kuidas: kliendi loomine

On Azure mobiilirakenduste kirjutamata projektis juurdepääsemiseks luua ka `MSClient`. Asendage `AppUrl` rakenduse URL. Võib-olla jätate `gatewayURLString` ja `applicationKey` tühi. Kui häälestate lüüsi autentimine, asustades `gatewayURLString` lüüsi URL-iga.

**Eesmärk-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Kiire**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Kuidas: tabeli viite loomine

Accessi või Värskenda andmeid, viite taustväärtus tabeli loomine. Asendage `TodoItem` tabeli nimi

**Eesmärk-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Kiire**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Kuidas: päringu andmed

Andmebaasi päringu koostamiseks päringu soovitud `MSTable` objekti. Järgmine päring saab kõik üksused `TodoItem` ja logib iga kirje tekst.

**Eesmärk-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Kiire**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Kuidas: filtri tagastatud andmeid

Tulemite filtreerimiseks on saadaval palju võimalusi.

Filtreerimine predikaat, kasutage mõnda `NSPredicate` ja `readWithPredicate`. Järgmised filtrid tagastatakse andmed ainult lõpetamata Todo üksuste otsimiseks.

**Eesmärk-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Kiire**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Kuidas: rakendus MSQuery kasutamine

Teha keerukat päringut (sh sortimise ja Piipar), luua mõne `MSQuery` objekti otseselt või predikaat abil:

**Eesmärk-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Kiire**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`saate reguleerida mitme päringu käitumist.

* Määrake järjestuse tulemused
* Millised väljad tagastamiseks piiramine
* Kui palju kirjete tagastamiseks piiramine
* Määrata koguarv vastus
* Saate määrata kohandatud päringustringi parameetrite taotluse
* Täiendavate funktsioonide rakendamine

Käivitada mõne `MSQuery` päringu helistades `readWithCompletion` objekti.

## <a name="sorting"></a>Kuidas: rakendus MSQuery andmete sortimine

Tulemite sortimiseks vaatame näide. Välja "tekst" tõusvas järjestuses, siis 'Valmis' laskuvas järjestuses sortimiseks autonoomsest `MSQuery` näiteks nii:

**Eesmärk-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Kiire**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Kuidas: piirata väljad ja päringustringi parameetrite abil rakendus MSQuery laiendamine

Piirata välju, et päringu tagastatavad, määrake atribuudi **selectFields** väljade nimed. Selles näites tagastab teksti ja täidetud väljad:

**Eesmärk-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Kiire**:

```
query.selectFields = ["text", "complete"]
```

Täiendavad päringustringi parameetrite kaasamine serveri kutse (nt seetõttu, et kohandatud serveripoolse skripti kasutab neid), asustades `query.parameters` näiteks nii:

**Eesmärk-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Kiire**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Kuidas: leheformaadi konfigureerimine

Azure'i Mobile'i rakendustega leheformaadi juhtelemendid kirjed, mis on tõmmatakse korraga kirjutamata tabelite arvu. Kõne `pull` andmeid soovite seejärel partii üles andmeid, mis põhineb selle lehe suuruse, kuni pole veel kirjeid tõmmata.

On võimalik konfigureerida lehe suuruse **MSPullSettings** abil, nagu allpool näidatud. Lehekülje vaikesuurus on 50 ja näites muutub 3.

Saanud konfigureerida jõudluse suuruses. Kui teil on small andmekirjeid suure hulga, vähendab kõrge leheformaadi serveri vormimallikujundaja arvu. 

See säte määrab ainult leheformaadi kliendis. Kui kliendi küsib lehe suuremaks kui mobiilirakenduste kirjutamata toetab, lehe suuruse ülempiir on taustväärtus on konfigureeritud toetama maksimumväärtus. 

See säte on ka andmekirjeid, mitte _baitide_ _arvu_ .

Kui [teil peaks ka suurendamine lehe serveri](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size)klient lehe suuruse suurendamine

**Eesmärk-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Kiire**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Kuidas: andmete sisestamine

Tabelisse uue rea lisamiseks loomine on `NSDictionary` ja autonoomsest `table insert`. [Dünaamiliste skeemi] on lubatud, Azure'i rakendust Service mobiilsideseadmete kirjutamata loob automaatselt uue veeru alusel selle `NSDictionary`.

Kui `id` ei esitata, on kirjutamata loob automaatselt uue kordumatu ID. Sisestage oma `id` kasutada e-posti aadressid, kasutajanimed, või oma kohandatud väärtuste ID. Pakkuda oma ID võivad hõlbustada ühendused ja äriotstarbeliste andmebaasi loogika.

Funktsiooni `result` sisaldab lisatud uus üksus. Sõltuvalt oma serveri loogika, see võib olla täiendavad või muudetud andmete võrreldes mis serverisse edastati.

**Eesmärk-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Kiire**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Kuidas: andmete muutmine

Mõne olemasoleva rea värskendamiseks muuta mõne üksuse ja kõne `update`:

**Eesmärk-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Kiire**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Teise võimalusena esitama ID rida ja värskendatud välja:

**Eesmärk-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Kiire**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Vähemalt selle `id` atribuut peab olema seatud tegemisel värskendused.

##<a name="deleting"></a>Kuidas: andmete kustutamine

Üksuse kustutamiseks autonoomsest `delete` üksusega:

**Eesmärk-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Kiire**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Rea ID pakkudes võimalusena kustutamiseks tehke järgmist.

**Eesmärk-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Kiire**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Vähemalt selle `id` atribuut peab olema seatud kui tegemine kustutab.

##<a name="customapi"></a>Kuidas: kohandatud API kõne

Kohandatud API-ga, saate seada taustväärtus funktsioone. See ei pea vastendamiseks tabeli käivitamine. Mitte ainult sa saada rohkem kontrolli sõnumside, saate isegi lugemine/set päised ja keha vastuse vormingu muutmine. Saate teada, kuidas luua kohandatud API funktsiooni taustväärtus, vt [Kohandatud API -d](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Kohandatud API, helistage `MSClient.invokeAPI`. Koosolekukutsete ja vastuse sisu käsitletakse JSON. Muud tüüpi, kasutada [ülekoormuse, kasutage `invokeAPI` ] [ 5].  Veenduge, et on `GET` taotlemine asemel on `POST` taotlemine, määrata parameeter `HTTPMethod` abil `"GET"` ja parameeter `body` abil `nil` (kuna Saada kutsed ei ole sõnumi keha.) Kui teie kohandatud API toetab muude HTTP verbide, muuta `HTTPMethod` õigesti.

**Eesmärk-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Kiire**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Kuidas: Register tõuketeatised Mallid platvormidel teatiste saatmine

Liigu Mallid registreerimiseks oma **client.push registerDeviceToken** meetod kliendi rakenduse mallid.

**Eesmärk-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Kiire**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Mallide on tippige NSDictionary ning võib sisaldada mitut Mallid järgmises vormingus:

**Eesmärk-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Kiire**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Kõik sildid on eemaldatakse taotlus turvalisuse.  Installide või mallidele installide siltide lisamise kohta vt teemat [.NET taustväärtus serveri SDK Azure'i mobiilirakenduste töötamine][4].  Saada teatisi nende registreeritud mallide kasutamine, töötamine [Teatis jaoturi API -de][3].

##<a name="errors"></a>Kuidas: vigade

Mõne Azure'i rakendust Service mobiilsideseadmete kirjutamata helistamisel lõpetamise plokk sisaldab mõnda `NSError` parameeter. Tõrke ilmnemisel see parameeter on-null. Koodi, peaksite märkige see parameeter ja käsitlemiseks tõrke vastavalt vajadusele, nagu on näidatud eelmise Koodilõigud.

Faili [`<WindowsAzureMobileServices/MSError.h>`] [6] määratleb konstantide `MSErrorResponseKey`, `MSErrorRequestKey`, ja `MSErrorServerItemKey`. Et saada rohkem andmeid, mis on seotud tõrge:

**Eesmärk-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Kiire**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Lisaks faili määratleb konstantide iga tõrkekood:

**Eesmärk-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Kiire**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Kuidas: autentida Active Directory Authentication Library kasutajad

Active Directory autentimise Raamatukogu (ADAL) abil saate sisse logida kasutades Azure Active Directory rakenduse kasutajad. Kliendi meilivoo autentimist kasutades identiteedipakkuja SDK on parem kasutada funktsiooni `loginWithProvider:completion:` meetod.  Kliendi meilivoo autentimise pakub rohkem kohalikke Kasutuskogemuse olemus ja võimaldab täiendav kohandamine.

1. Konfigureerimine oma mobiilirakenduse kirjutamata jaoks AAD sisselogimine järgides, [Kuidas konfigureerida rakenduse teenus Active Directory login] [ 7] õpetuse. Veenduge, et kohustuslik toiming omakliendi rakenduse registreerimisel. IOS-i, soovitame selle ümbersuunamise URI on vormi `<app-scheme>://<bundle-id>`. Lisateavet leiate teemast [ADAL iOS-i Kiirjuhend][8].

2. Installige ADAL Cocoapods abil. Oma Podfile lisada järgmised määratlus, asendades **Teie PROJEKTIGA** Xcode projekti nime redigeerimiseks tehke järgmist.

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   Pod ja tehke järgmist.

        pod 'ADALiOS'

3. Käivitage Terminal, kasutades `pod install` kataloogist, mis sisaldab projekti ja seejärel avage loodud Xcode tööruumi (mitte projekt).

4. Lisage järgmine kood rakenduse vastavalt keelt, mida te kasutate. Iga, tehke need asendamine:

    * Asendage **Lisa-asutus – siin** rentniku, kus te ette valmistatud rakenduse nimi. Vormingu peaks olema https://login.windows.net/contoso.onmicrosoft.com. See väärtus saab kopeerida domeeni menüüs Azure Active Directory [Azure klassikaline portaalis].
    * Asendage **Lisa-RESSURSI ID siin** kliendi ID jaoks oma mobiilirakenduse taustväärtus. Kliendi ID saate hankida mõnest portaalis jaotises **Azure Active Directory sätted** vahekaarti **Täpsemalt** .
    * Asendage **Lisa-kliendi ID siin** omakliendi rakendusest kopeeritud kliendi ID-ga.
    * Asendage **Lisa-REDIRECT URI siin** oma saidi _/.auth/login/done_ lõpp-punkti, kasutades HTTPS värviskeemi. See väärtus peab olema _https://contoso.azurewebsites.net/.auth/login/done_.

**Eesmärk-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Kiire**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Kuidas: Facebooki SDK iOS-i kasutajate autentimiseks

Facebooki SDK iOS-i abil saate sisse logida kasutades Facebook rakenduse kasutajad.  Kliendi meilivoo autentimist kasutades on parem kasutada funktsiooni `loginWithProvider:completion:` meetod.  Kliendi meilivoo autentimise pakub rohkem kohalikke Kasutuskogemuse olemus ja võimaldab täiendav kohandamine.

1. Konfigureerimine oma mobiilirakenduse kirjutamata jaoks Facebooki sisse logida, [Kuidas konfigureerida rakenduse teenuse Facebook login] järgides[ 9] õpetuse.

2. Installige iOS-i Facebooki SDK [Facebooki SDK iOS - alustamine] [ 10] dokumentatsiooni. Loomise rakendus, asemel saate lisada olemasoleva registreerimise iOS platvorm. 

3. Facebook dokumentatsiooni sisaldab rakenduse volitatud esindaja mõned eesmärk C-kood. Kui kasutate **kiire**, saate järgmine tõlked AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Lisaks lisamise `FBSDKCoreKit.framework` projekti, lisada ka viide `FBSDKLoginKit.framework` samal viisil. 

4. Lisage järgmine kood rakenduse vastavalt keelt, mida te kasutate. 

**Eesmärk-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Kiire**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Kuidas: autentida Twitteri struktuuri iOS-i kasutajad

Saate rakenduse abil Twitteri kasutajate sisselogimiseks struktuuri iOS-i. Kliendi meilivoo autentimine on parem kasutada funktsiooni `loginWithProvider:completion:` meetod, sest see pakub rohkem kohalikke Kasutuskogemuse olemus ja võimaldab täiendav kohandamine.

1. [Rakenduse Twitteri sisselogimise teenuse konfigureerimine](app-service-mobile-how-to-configure-twitter-authentication.md) õpetuse järgides konfigureerimine oma mobiilirakenduse kirjutamata jaoks Twitteri Logi sisse.

2. Struktuuri lisamine projekti jälgimise [struktuuri iOS - alustamine] dokumentatsiooni ja häälestate TwitterKit.

    > [AZURE.NOTE] Vaikimisi struktuuri Twitter rakendus loob teie jaoks automaatselt. Saate luua rakenduse registreerimisel tarbija võti ja tarbija salajane varem abil järgmised Koodilõigud loodud vältida.  Teise võimalusena saate asendada tarbija võti ja tarbija salajane väärtused esitate rakenduse teenuse näete [Struktuuri armatuurlaua]väärtustega. Kui valite selle suvandi, kindlasti seatud tagasihelistamise URL-i kohatäite väärtus, näiteks `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Kui otsustate kasutada varem loodud saladusi, lisada rakenduse volitatud järgmine kood:
    
    **Eesmärk-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Kiire**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Lisage järgmine kood rakenduse vastavalt keelt, mida te kasutate. 

**Eesmärk-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Kiire**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Kuidas: kasutajate sisselogimist SDK Google iOS-i autentimiseks

Google sisselogimise SDK iOS-i abil saate kasutajad logige rakenduse Google'i kontot kasutades.  Google teada viimati oma OAuthi turbepoliitikate muudatustest.  Poliitika muudatustest vajavad tulevikus Google SDK kasutamine.

1. Konfigureerida [rakendus Google sisselogimise teenuse konfigureerimine](app-service-mobile-how-to-configure-google-authentication.md) õpetuse järgides oma mobiilirakenduse kirjutamata jaoks Google Logi sisse.

2. Installige Google SDK iOS-i [integreerimine käivitamine Google sisselogimist iOS -](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumendid. Võib jaotis "Autentimiseks ja ka kirjutamata Server" vahele jätta.

3. Lisage järgmine tekst oma volitatud esindaja `signIn:didSignInForUser:withError:` meetodiga vastavalt keelt, mida te kasutate.

**Eesmärk-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Kiire**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Veenduge, et lisate järgmist `application:didFinishLaunchingWithOptions:` rakenduses rakenduse delegaat, asendades "SERVER_CLIENT_ID" sama ID, mida kasutasite rakendust Service konfigureerimine juhises 1.

**Eesmärk-C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Kiire**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Järgmine kood lisamine rakenduse UIViewController, mis rakendab selle `GIDSignInUIDelegate` protokolli vastavalt keelt, mida te kasutate.  Olete välja logitud enne allkirjastamist uuesti, ja kuigi te ei vaja, sisestage oma kasutajanimi ja parool uuesti, kuvatakse nõusolekut dialoogiboksi.  Ainult kõne seda meetodit, kui seansiga luba on aegunud.
 
 **Eesmärk-C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Kiire**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure'i Mobile'i rakendused kiire algus]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dünaamiline skeem]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Struktuuri armatuurlaud]: https://www.fabric.io/home
[Struktuuri iOS - töötamise alustamine]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
