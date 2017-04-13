<properties
    pageTitle="Azure'i AD v2.0 iOS-i rakendus | Microsoft Azure'i"
    description="Kuidas koostada rakendus iOS-i, et kasutajad nii isikliku Microsofti kontoga ja töö-või koolikonto abil kolmanda osapoole teeke märke."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Rakendus iOS-i abil muu teegi kasutades v2.0 lõpp-punkti Graph API-ga sisselogimiseks lisamine

Microsofti identity platvormi kasutab avatud standardeid nagu OAuth2 ja OpenID ühendamine. Arendajad saate kasutada mis tahes teegi kasutajad soovivad meie teenuste integreerimine. Et arendajad meie platvormi kasutada muude teekidega, oleme kirjutanud mõne juhendavad tutvustused umbes selline näidata, kuidas konfigureerida ühenduse Microsofti identity platvormi kolmanda osapoole teeke. Enamik teekide [RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) , et saate luua ühenduse Microsofti identity platvormi.

Rakendus, mis loob juhendis kasutajad saavad logige sisse oma organisatsiooni ja seejärel otsige teiste oma asutuse Graph API abil.

Kui olete uus OAuth2 või OpenID ühendus, suure osa sellest valimi ei oleks teile. Soovitame teil lugeda [v2.0 Protokollid - OAuthi 2.0 autoriseerimine kood Flow](active-directory-v2-protocols-oauth-code.md) tausta jaoks.


> [AZURE.NOTE]
    Meie platvormi funktsioonid, mis on avaldise OAuth2 või OpenID Connect standardeid, näiteks juurdepääsu ja Intune poliitika haldus nõuavad meie avatud allikas Microsoft Azure'i identiteedi teekide kasutamist.

V2.0 lõpp-punkti ei toeta kõik Azure Active Directory stsenaariumid ja funktsioonid.

> [AZURE.NOTE]
    Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Koodi saate alla laadida GitHub
Selle õpetuse kood säilitatakse [github](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) - või klooni skelett:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Saate alla laadida ka lihtsalt valimi ja kohe tööle asuda.

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Rakenduse registreerimine
Luua uue rakenduse [rakenduse registreerimise portaali](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige üksikasjalikud juhised, kuidas [registreerida app v2.0 lõpp-punkti](active-directory-v2-app-registration.md).  Veenduge, et:

- **Rakenduse Id** , mis on määratud rakenduse, kuna peate kiiresti kopeerida.
- Lisage oma rakenduse **Mobile** platvormi.
- Kopeerige portaali **URI ümber suunata** . Kasutage vaikeväärtust, milleks on `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Muude tootjate NXOAuth2 teegi alla laadida ja tööruumi loomine

See selgituse kasutate OAuth2Client GitHub, mis on OAuth2 Raamatukogu Mac OS X ja iOS (kakao ja puudutage kakao) kaudu. See teek on alusel OAuth2 spec mustand 10. See rakendatakse kohalikus rakenduses profiili ja toetab autoriseerimine lõpp-punkti Kasutaja. Need on asjad, mida peate Microsofti identity platvormi integreerida.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Teegi lisamine projekti CocoaPods abil

CocoaPods on sõltuvus manager Xcode projektide jaoks. Eelmise installimisjuhiseid haldab automaatselt.

```
$ vi Podfile
```
1. See podfile järgmine lisamiseks tehke järgmist.

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Laadimine on podfile CocoaPods abil. See loob uue Xcode tööruum, mis on teil laadida.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Projekti struktuuri uurimine

Meie projekti skelett on häälestatud järgmine struktuur:

- Juhtslaidi vaade UPN-i otsimine
- A Detail View andmete jaoks valitud kasutaja kohta
- Login vaade, kus kasutaja saab sisse logida rakendusse Graphi päringud

Me liigub skelett mitmesuguste failide lisamine autentimist. Koodi, visual kood, nt mujal ei hõlma identiteedi, kuid teile pakutakse.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Settings.plst faili teegis häälestamine

-   Kiirjuhend projekt, avage soovitud `settings.plist` faili. Jaotises väärtused, mida kasutasite Azure'i portaalis kajastamiseks elementide väärtuste asendamine. Iga kord, kui seda kasutatakse Active Directory Authentication Library viitab teie koodi need väärtused.
    -   Funktsiooni `clientId` on rakenduse portaalist kopeeritud kliendi ID-d.
    -   Funktsiooni `redirectUri` on portaalis esitatud ümbersuunamise URL-i.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Teie LoginViewController NXOAuth2Client teegi häälestamine

NXOAuth2Client teegi jaoks on vaja mõned väärtused häälestamiseks. Pärast selle toimingu lõpuleviimist saate helistada Graph API omandatud luba. Kuna `LoginView` on nimega igal ajal läheb vaja autentida, on mõistlik panna konfiguratsiooni väärtuste faili.

- Lisame mõned väärtused on `LoginViewController.m` faili määramiseks kontekst autentimise ja luba. Väärtused üksikasjade järgige kood.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Vaatame üksikasjade kood.

Esimese stringi on `scopes`.  Funktsiooni `User.Read` väärtus võimaldab lugeda põhiprofiil kehtib kasutaja.

Saate lisateavet kõik saadaolevad otsinguulatuste veebisaidil [Microsoft Graphi õiguste otsinguulatuste](https://graph.microsoft.io/docs/authorization/permission_scopes).

Jaoks `authURL`, `loginURL`, `bhh`, ja `tokenURL`, kasutage varem esitatud väärtused. Kui kasutate Microsoft Azure identiteedi teekide avatud allikas, me rippmenüü andmed teie eest, kasutades meie metaandmete lõpp-punkti. Oleme teinud, saate need väärtused ekstraktimiseks tööd.

Funktsiooni `keychain` väärtus on ümbris kasutavate NXOAuth2Client Raamatukogu võtmekimbu, salvestada oma sõned loomiseks. Kui soovite saada ühekordse sisselogimise (SSO) mitmeid rakendustes, saate määrata sama võtmekimbu iga rakenduste ja taotleda selle võtmekimbu kasutada oma Xcode maksud. See on selgitatud Apple'i dokumentatsiooni kohta.

Ülejäänud need väärtused on vaja kasutada teek ja kohti väärtused konteksti abil saate luua.

### <a name="create-a-url-cache"></a>URL-i vahemälu luua

Sees `(void)viewDidLoad()`, mis on alati nimega pärast vaate laaditakse, järgmine kood primes vahemälu meie kasutada.

Lisage järgmine kood:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Sisselogimise WebView loomine

WebView saate Küsi kasutajalt täiendavad tegurid, nagu SMS-sõnumi (kui see on konfigureeritud) või tagasi tõrketeated kasutajale. Siin saate määrata selle WebView üles ja kirjutage siis hiljem käsitlema kontekstiatribuuti, mis juhtub WebView identiteedi teenuste kood.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Alistada WebView meetodid autentimise reageerimine

Funktsiooni WebView öelda, mis juhtub, kui kasutaja on vaja sisse logida, nagu varem, saate kleepida järgmine kood.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Tulemus OAuth2 taotluse käsitlema koodi kirjutamine

Järgmine kood tegeleb redirectURL, mis tagastab selle WebView. Kui autentimine ei õnnestunud, proovib koodi uuesti. Samal ajal, teeki annab viga, mida näete konsooli või toime asünkroonselt.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>OAuthi konteksti (ehk konto poe) häälestamine

Siin saate helistada `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` jagatud poes soovitud rakenduse saama kasutada iga teenuse jaoks. Konto tüüp on string, mida kasutatakse identifikaatori teatud teenuse. Kuna Graph API avate koodi viitab seda `"myGraphService"`. Seejärel saate seadistada vaatleja, mis ütleb teile, kui midagi muutub luba. Pärast saate luba, saate saatja kasutaja tagasi selle `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Juhtslaidivaade, otsida ja kuvada Graph API kasutajate häälestamine

Põhi-vaade – kontrolleril (MVC) rakendus, mis kuvatakse tagastatud andmed ruudustiku juhendis väljub ja palju õppematerjalid selgitatakse, kuidas koostada ühte. Kõik järgmine kood on skelett faili. Siiski peate tegelema paari asja selles MVC rakenduses:

* Kui kasutaja tipib midagi otsinguväljale Intercept
* Pakkuda andmete objekti tagasi selle MasterView nii, et see tulemite kuvamiseks ruudustikus

Me need allpool.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Lisage sisse, et näha, kui olete sisse logitud

Rakendus ei vähe kui kasutaja on pole sisse logitud, nii et see on tark, et kontrollida, kas on juba märgiks vahemälu. Kui ei, suunamine LoginView kasutaja, sisse logida. Kui te ei mäleta, on parim viis vaate laadimise ajal toimingute tegemiseks kasutada funktsiooni `viewDidLoad()` meetod, mis Apple pakub meile.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Tabeli vaate värskendada, kui andmed on saabunud

Kui Graph API tagastab andmed, peate andmete kuvamiseks. Lihtsa, siis siin on kõik tabeli värskendamiseks koodi. Kleepimiseks võite lihtsalt MVC trafaretset koodi õige väärtused.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Võimalda Graph API kõne, kui kasutaja tipib otsinguväljale

Kui kasutaja tipib väljale Otsing, peate pista mis Graph API. Funktsiooni `GraphAPICaller` klassi, mis on koostate järgmine kood, eraldab funktsiooni lookup esitlus. Nüüd, vaatame kirjutage kanalite otsingu ekraanile Graph API tähist. Me seda teha, pakkudes meetodit nimetatakse `lookupInGraph`, mis võtab stringi, mida me soovite otsida.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Kirjutage Graph API juurdepääsu ainekursuse Helper.

See on meie taotlus. Samas ülejäänud on vaikimisi MVC mustri Apple lisamise kood, siia kirjutada koodi, kui kasutaja tipib Graphi päringud ja seejärel andmeid naasmiseks. Siin on kood ja üksikasjalik selgitus see järgib.

### <a name="create-a-new-objective-c-header-file"></a>Eesmärk C päise uue faili loomine

Pange failile nimi `GraphAPICaller.h`, ja lisage järgmine kood.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Siin näete, et määratud meetodit võtab stringi ja annab vastuseks completionBlock. See completionBlock, nagu võib olla arvasid, värskendatakse tabeli andes objekti täidetud andmete reaalajas kasutaja otsingud.


### <a name="create-a-new-objective-c-file"></a>Eesmärk C uue faili loomine

Pange failile nimi `GraphAPICaller.m`, ja lisage järgmisel viisil.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Vaatame seda meetodit üksikasjalikult.

Järgmine kood keskmes on selle `NXOAuth2Request`, meetod, mis võtab parameetrid, mille olete juba määratletud settings.plist faili.

Kõigepealt tuleb koostada õige Graph API kõne. Kuna helistate `/users`, saate määrata mis lisamine Graph API ressursi koos versioon. On mõistlik panna need välise sätteid failis, sest need võivad muutuda, nagu API areneb.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Järgmiseks peate määrama parameetrid, mis annavad Graph API kõne. On *väga oluline* , et te ei pane parameetrid ressursi lõpp-punkti kuna mis on scrubbed kõik-URI mittevastavate märgid käitusajal. Kõik päringu koodi tuleb parameetrid.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Võite märgata on vaja mõnda `convertParamsToDictionary` meetod, et te pole seda veel kirjutada. Vaatame tehke nüüd faili lõpus.

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Järgmiseks kasutame funktsiooni `NXOAuth2Request` meetod andmete naasmiseks API JSON-vormingus.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Lõpuks vaatame kuidas taastada andmed on MasterViewController. Andmete Tagastab veaväärtuse, kuna seeriasertide ja tuleb deserialized ja objekti, mida saab kasutada funktsiooni MainViewController laaditud. Selleks on luukere on `User.m/h` faili, mis loob kasutaja objekti. Saate asustada selle objekt graafik teabega.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Valimi käivitamine

Kui olete kasutanud luukere või jälgitavad koos selle kiirtutvustus rakenduse peaks nüüd käivitada. Käivitage selle simulaatorit ja klõpsake rakenduse kasutamiseks **logige sisse** .

## <a name="get-security-updates-for-our-product"></a>Meie toote jaoks turbevärskenduste saamine

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [Turbe Tehnokeskus](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
