<properties
    pageTitle="Azure Active Directory B2C: Kõne veebi-API iOS-i rakendus kolmanda osapoole teekide abil | Microsoft Azure'i"
    description="Selles artiklis näitab teile, kuidas luua iOS-i 'Ülesandeloend' rakendus, mis nõuab Node.js veebi-API abil OAuth 2.0 esitaja sõned kolmanda osapoole teegi kasutamine"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< sildid ms.service= "aktiivne-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "pilt artikkel"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure'i AD B2C: Kõne veebi-API kolmanda osapoole teegi kasutamine iOS-i rakendus

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Microsofti identity platvormi kasutab avatud standardeid nagu OAuth2 ja OpenID ühendamine. See võimaldab arendajatel kasutada mis tahes teegi nad soovivad meie teenuste integreerimine. Abi meie platvorm funktsiooniga teistes teekides arendajate oleme kirjutanud mõne juhendavad tutvustused umbes selline, et demonstate kolmanda osapoole teekide Microsofti identity platform ühenduse konfigureerimine. Enamik teekides rakendada [RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) saab ühendada Microsofti Identity platvormi.


Kui olete uus OAuth2 või OpenID Connect suure osa sellest valimi võib-olla ei loogiline teile. Soovitame vaadata lühike [Ülevaade oleme siin olete dokumenteerida protokolli](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Meie platvormi funktsioonid, mis on avaldise standardeid, nagu juurdepääsu ja Intune poliitika haldamine, nõuavad meie avatud allikas Microsoft Azure'i identiteedi teekide kasutamist. 
   
B2C platvorm ei toeta kõiki Azure Active Directory stsenaariumid ja funktsioonid.  Kindlaks teha, kui kasutate B2C platvorm, lugege [B2C piirangud](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Saada on Azure AD B2C kataloog

Enne kasutamist Azure AD B2C, saate luua või rentniku. Kataloogi on ümbris kõigi kasutajate, rakendused, rühmad ja muu. Kui teil pole seda juba, jätkake [luua B2C kataloogi](active-directory-b2c-get-started.md) enne.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate B2C kataloogis rakenduse loomine. See annab Azure AD teabest ja rakenduse. Rakenduse nii veebi-API esindavad ühe **Rakenduse ID** sel juhul, kuna need sisaldavad üks loogiline rakendus. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md). Kindlasti järgmist.

- Kaasake rakenduse **mobiilsideseadmesse** .
- Kopeerige **Rakenduse ID** , mis määratakse teie rakendus. On vaja seda hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

Azure'i AD B2C, iga kasutuskogemusega on määratletud [poliitika](active-directory-b2c-reference-policies.md). See rakendus sisaldab ühe identiteedi kasutusvõimalusi: on ühendatud sisse logida sisse logida. Peate looma iga tüüpi sellele seadnud [poliitika artiklis](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kirjeldatud. Kui loote poliitika, ärge unustage:

- Valige oma poliitika **kuvatav nimi** ja registreerumise atribuute.
- Valige **kuvatav nimi** ja **Objekti ID** rakenduse nõuded iga poliitika. Soovi korral saate kasutada ka muud nõuded.
- Kopeerige iga poliitika **nimi** pärast selle loomist. Sellel peab olema eesliite `b2c_1_`.  Poliitika nimi kuvatakse hiljem vaja.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Pärast seda, kui olete loonud oma poliitikad, olete valmis oma rakenduse.


## <a name="download-the-code"></a>Laadige alla kood

Selle õpetuse kood säilitatakse [github](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Saate jälgida, [laadige rakendus on zip nimega](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) või selle klooni:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Või allalaadimine on lõpule viidud kood ja kohe tööle asuda. 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Kolmanda osapoole teegi nxoauth2 alla laadida ja käivitada tööruumi

See selgituse kasutame OAuth2Client GitHub, on OAuth2 teegi jaoks Mac OS X ja iOS-i (kakao ja puudutage kakao) kaudu. See teek on alusel OAuth2 spec mustand 10. See rakendatakse kohalikus rakenduses profiili ja toetab lõppkasutaja autoriseerimine lõpp-punkti. Need on asjad, mida me vaja selleks, et integrat Microsoft identiteedi platvorm.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Projekti abil CocoaPods teegi lisamine

CocoaPods on sõltuvus manager Xcode projektide jaoks. Ülaltoodud installimisjuhiseid haldab automaatselt.

```
$ vi Podfile
```
See podfile järgmine lisamiseks tehke järgmist.

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Nüüd laadida podfile, kasutades cocoapods. See loob uue XCode tööruumi saate laaditakse.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Projekti struktuur

Meil on meie projekti skelett häälestamine järgmine struktuur:

* Tööpaani **Juhtslaidivaade**
* **Tööülesande vaate lisamine** valitud tööülesande kohta andmete jaoks
* **Login vaade** , mis võimaldab kasutajatel sisselogimine rakendusse.

Me ei liikuda erinevad faili lisamiseks autentimise projekt. Mujal kood, nt visual kood ei esitatud identiteedi ja pakutakse teile.

## <a name="create-the-settingsplist-file-for-your-application"></a>Looge selle `settings.plist` fail rakenduse

Oleks lihtsam, kui meil on meie konfiguratsiooni väärtuste keskse asukoha rakenduse konfigureerimiseks. See aitab teil mõista, mida iga säte ei oma rakenduse. Oleme viis anda need väärtused rakendusse kogusummaks *Atribuudi lisanud* .

* Luua Ava soovitud `settings.plist` jaotises faili `Supporting Files` oma rakenduse tööruumis

* Sisestage järgmised väärtused (me tuleb läbida üksikasjalikult tulekul)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Esmalt avage neid üksikasjalikult.


Jaoks `authURL`, `loginURL`, `bhh`, `tokenURL` märkate, mida on vaja rentniku nime. See on teie B2C rentniku teile määratud rentniku nimi. Näiteks `kidventusb2c.onmicrosoft.com`. Kui kasutate meie avatud allikas Microsoft Azure'i identiteedi teekide meil tuleks rippmenüü andmed kasutades meie metaandmete lõpp-punkti. Oleme teinud, saate need väärtused ekstraktimiseks tööd.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Funktsiooni `keychain` väärtus on ümbris kasutavate NXOAuth2Client teegi võtmekimbu, salvestada oma sõned loomiseks. Kui soovite saada SSO mitmeid rakendustes saate saate määrata sama võtmekimbu iga rakenduste samuti taotleda selle võtmekimbu kasutamine oma XCode entitements. See on kaetud Apple'i dokumentatsiooni kohta.

Funktsiooni `<policy name>` iga URL-i lõpus on kohad, kus soovite eelnevalt loodud poliitika. Rakendus on kõne need poliitikad sõltuvalt voogu.

Funktsiooni `taskAPI` on ülejäänud lõpp-punkti me kutsume oma B2C luba kas tööülesannete või olemasoleva päringu tööülesannete lisamine. See on häälestatud spetsiaalselt selle valimi. Saate ei pea seda muuta valimi töötamiseks.

Ülejäänud need väärtused on vaja kasutada teek ja lihtsalt kohti väärtused konteksti abil saate luua.

Nüüd, kui meil on `settings.plist` loodud faili läheb vaja seda lugeda kood.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Lugege meie sätted AppData ainekursuse häälestamine

Teeme lihtsa faili, mis lihtsalt sõelub meie `settngs.plist` faili me loodud kohale ja tehke nende sätete avaialble tulevikus mis tahes tunni. Kuna me ei soovi iga kord, kui ainekursuse küsib andmed uue koopia loomiseks, me Singleton mustri kasutamiseks ja ainult tagastada samas eksemplaris luua iga kord, kui taotletakse sätted

* Saate luua ka `AppData.h` faili:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Saate luua ka `AppData.m` faili:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Nüüd saate hõlpsalt saame meie andmed lihtsalt helistades `  AppData *data = [AppData getInstance];` mis tahes ajal meie tunnid kuvatakse allpool.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Teie AppDelegate NXOAuth2Client teegi häälestamine

NXOAuthClient teegi jaoks on vaja mõned väärtused häälestamiseks. Kui see on lõpule viidud saate luba, mis on omandatud helistamiseks REST API-ga. Kuna me teada, mis on `AppDelegate` nimeks saab igal ajal me laadige rakendus on mõistlik Panime meie konfiguratsiooni väärtuste faili.
* Avatud `AppDelegate.m` fail

* Mõne kasutame hiljem päise faili importida.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Lisage soovitud `setupOAuth2AccountStore` meetod on AppDelegate

Meil on vaja luua mõne AccountStore ja seejärel selle kanali me lihtsalt lugeda, andmeid selle `settings.plist` fail.

On mõningad asjad, mida peaks olema teadlik B2C teenuse sel hetkel, mis teevad selle koodi paremini mõistetav.


1. Azure'i AD B2C kasutab *poliitika* päringu parameetrite esitatud teenuse oma taotluse. See võimaldab Azure Active Directory tegutseda sõltumatu teenuse ainult rakenduse jaoks. Need lisatasu päringu parameetrite tuleb sisestada pakkumiseks selle `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` meetod meie kohandatud Poliitikasätte parameetritega. 

2. Azure'i AD B2C kasutab otsinguulatuste palju samamoodi OAuth2 muudes serverites. Kuid kuna B2C kasutamine on nii palju autentimist kasutaja juurdepääs ressursse peavad täiesti mõned otsinguulatuste, selleks, et meilivoo õigesti töötada. See on selle `openid` ulatust. Meie Microsofti identity SDK-d pakuvad automaatselt selle `openid` ulatus teile nii, et te ei näe, et meie SDK konfiguratsioon. Kuna me kasutame kolmanda osapoole teeki, aga läheb vaja selle ulatuse määramiseks.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Seejärel veenduge, et helistate AppDelegate jaotises `didFinishLaunchingWithOptions:` meetod. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Luua mõne `LoginViewController` ainekursus kasutame taotluste autentimise reageerimine

Kasutame webview konto Logi sisse. See võimaldab meil Küsi kasutajalt täiendavad tegurid, nagu SMS-sõnumi (kui see on konfigureeritud) või tõrketeated tagasi anda ja kasutajale. Siin määrame soovitud webview üles ja kirjutage siis hiljem käsitlema kontekstiatribuuti, mis juhtub WebView Microsofti Identity teenuse kood.

* Luua mõne `LoginViewController.h` klassi

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Loome eri viiside allpool.

> [AZURE.NOTE] 
    Veenduge, et seote selle `loginView` abil tegelik webview, mis on teie süžeeskeemi sees. Muul juhul ei pea te saate hüpikaknas, kui on aeg autentida webview.

* Luua mõne `LoginViewController.m` klassi

* Mõned muutujad läbi olekusse, mis me autentida lisamine

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Alistada WebView meetodid autentimise reageerimine

Läheb vaja teada saada, mis on webview käitumine, soovime, kui kasutaja on vaja sisse logida, nagu eespool. Saate lihtsalt lõikamisel ja kleepimisel allpool kood.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Tulemus OAuth2 taotluse käsitlema koodi kirjutamine

Vajame kood, mis tegeleb redirectURL, mis pärineb tagasi soovitud WebView. Kui see ei õnnestunud, proovime uuesti. Samal ajal annab teegi viga, et saaksite kuvada konsooli või toime asyncronously. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Saate häälestada tehased teatis.

Loome me ei sama meetod on `AppDelegate` kohal, kuid seekord lisame mõned `NSNotification`s meile, mis juhtub teenusele. Me loodud vaatleja, mis ütleb meile, kui midagi muutub luba. Kui me luba me tagasi kasutaja tagasi selle `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Mis tegeleb kasutaja iga kord, kui taotlus on algatanud Logi emakeelena koodi lisamine

Loome meetod, mida nimetatakse iga kord, kui meil taotluse autentimist. See on meetod, mis tegelikult loob webview

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Lõpuks vaatame kõne kõik need meetodid oleme kirjutanud kohal iga kord selle `LoginViewController` laadib. Teeme seda meetodit kasutades lisamisega meie `viewDidLoad` meetod Apple annab meile

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Nüüd olete teinud loomise peamist viisi me teil suhelda meie taotlus Logi sisse. Pärast seda, et olete sisse logitud, vajame meie sõned oleme saanud kasutada. Jaoks, mille loome mõned helper koodi, et helistavad koosolekule REST API-de meile selle teegi abil.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Luua mõne `GraphAPICaller` klassi meie taotluste REST API reageerimine

Meil on laaditakse iga kord, kui me laadimine meie rakenduse konfiguratsioon. Nüüd on vaja midagi teha, kui oleme märgiks. 

* Luua mõne `GraphAPICaller.h` fail

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Kuvatakse järgmine kood, et loome kaks võimalust: üks tööülesanded toomine API ja teine API ülesannete lisamine.

Nüüd saame olete häälestanud meie kasutajaliidese, lisada tegelikku rakendamist:

* Looge lisamine`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Käivitage rakendus näidis

Lõpetuseks, luua ja käivitada rakenduse Xcode. Registreerumine või rakendusse sisselogimine ja tööülesannete sisse logitud kasutajale loomine. Logige välja ja teise kasutajana logige uuesti sisse ja selle kasutaja tööülesannete loomine.

Märkate, et tööülesanded on talletatud kasutaja kohta API, kuna API ekstraktib juurdepääsu luba, et see saab kasutaja identiteet.


## <a name="next-steps"></a>Järgmised sammud

Nüüd saate teisaldada täpsemate B2C teemade peale. Võite proovida:

[Kõne Node.js veebi-API Node.js web Appist]()

[Rakenduse B2C Kasutuskogemuse kohandamiseks]()
