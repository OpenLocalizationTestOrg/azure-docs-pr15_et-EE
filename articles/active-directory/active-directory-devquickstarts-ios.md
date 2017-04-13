<properties
    pageTitle="Azure'i AD iOS-i alustamine | Microsoft Azure'i"
    description="Kuidas luua rakendus iOS-i, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de kasutamine OAuthi."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Azure AD integreerida iOS rakendus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD pakub Active Directory Authentication Library või ADAL, iOS-i klientidele, kes vajavad juurdepääsu kaitstud ressursse.  ADAL on ainus eesmärk elu on hõlpsalt oma rakenduse access sõned saada.  Näidata, kui lihtne on siin me tuleb koostada eesmärk C Ülesandeloend rakenduse mis:

-   Saab kasutada sõned Azure AD Graph API [OAuth 2.0 autentimine protokolli](https://msdn.microsoft.com/library/azure/dn645545.aspx)abil helistamine.
-   Otsib kataloog kasutajatele, kellel on antud alias (pseudonüüm).

Täieliku töötamine rakenduse loomiseks peate:

2. Rakenduse registreerima Azure AD.
3. Installimine ja konfigureerimine ADAL.
5. ADAL abil saate sõned Azure AD.

Alustamiseks, [laadige rakendus luukere](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) või [allalaadimine on lõpule viidud valimi](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Peate Azure AD rentniku, kus saate luua kasutajaid ja rakenduse registreerimine.  Kui teil pole veel rentniku, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

> [AZURE.TIP] Proovige meie uut [arendaja portaali](https://identity.microsoft.com/Docs/iOS) , mis aitab teil saada tööle Azure Active Directory vaid paari minutiga eelvaadet!  Arendaja portaali juhendab teid rakenduse registreerimisel ja integreerimine Azure AD oma kood.  Kui olete lõpetanud, on teil lihtne rakendus, mida saate kasutajate oma rentniku ja on kirjutamata, mida saate aktsepteerida sõned ning teha valideerimine autentimiseks. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. kindlaks teha, mida teie ümber suunata URI saab iOS-i*

Turvaline teatud stsenaariumide SSO rakenduste käivitamiseks peavad **Ümber suunata URI** loomist kindlas vormingus. Suunake URI kasutatakse tagamaks, et selle sõned tagasi õige rakendus, mis neid küsida.

Suunake URI iOS-i vorming on:

```
<app-scheme>://<bundle-id>
```

-   **aap-värviskeemi** – see on registreeritud XCode projektis. Kuidas muude rakenduste kõne on. Saate otsida see jaotises Info.plist -> URL-i andmetüübid -> URL-i identifikaator. Üks tuleks luua, kui teil pole veel üks või mitu konfigureeritud.
-   **komplekt-id** - see on komplekt identifikaator "identiteet" leiate un XCode oma projekti sätted.

Näiteks koodi Kiirjuhend: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. register DirectorySearcher rakendus*
Saada sõned rakenduse lubamiseks esmalt peate selle oma Azure AD rentniku registreerida ja selle juurdepääsu lubamine Azure AD Graph API.

-   Azure'i haldusportaal sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige, kuhu rakenduse registreerida rentniku.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua uue **Kohalikke klientrakendusega**.
    -   Rakenduse **nimi** kirjeldab rakenduse lõppkasutajatele
    -   **Suunata Uri** on värviskeemi ja stringi kasutavate Azure AD Turbeloa vastuste tagastamiseks.  Sisestage teatud väärtus ülaltoodud andmete põhjal rakenduse.
-   Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü **konfigureerimine** .
- Ka **konfigureerimine** vahekaardil, otsige üles jaotis "Õigused soovite muud rakendused".  "Azure Active Directory" rakenduse, lisada **juurdepääs teie ettevõtte kataloogi** luba jaotises **Delegeeritud õigused**.  See võimaldab rakenduse päringu Graph API kasutajate jaoks.

## <a name="3-install--configure-adal"></a>*3. installida ja konfigureerida ADAL*
Nüüd, kui teil on rakenduse Azure AD, saate installida ADAL ja kirjutage oma identiteedi seotud kood.  Selleks ADAL saama Azure AD suhelda, peate esitama registreerimise rakenduse kohta leiate teavet.
-   Alustage, lisades ADAL DirectorySearcher projekti Cocapods abil.

```
$ vi Podfile
```
See podfile järgmine lisamiseks tehke järgmist.

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Nüüd laadida podfile, kasutades cocoapods. See loob uue XCode tööruumi saate laaditakse.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Projectis Kiirjuhend plist faili avamiseks `settings.plist`.  Asendage elementide kajastamiseks Azure portaali sisestatud väärtused jaotises väärtused.  Iga kord, kui seda kasutatakse ADAL viitab teie koodi need väärtused.
    -   Funktsiooni `tenant` on Azure AD rentniku, nt contoso.onmicrosoft.com Domeen.
    -   Funktsiooni `clientId` on rakenduse portaalist kopeerisite clientId.
    -   Funktsiooni `redirectUri` on uuesti registreeritud portaali url.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. saada sõned AAD ADAL kasutamine*
On põhimõte ADAL taha, et iga kord, kui teie rakendus peab juurdepääsu sümboolse, seda lihtsalt kõned on completionBlock `+(void) getToken : `, ja ADAL teeb ülejäänud.  

-   Klõpsake soovitud `QuickStart` projekti avamine `GraphAPICaller.m` ja otsige üles soovitud `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` kommentaari ülaservas.  See on, kus te kaotate ADAL kaudu on CompletionBlock Azure AD suhelda ja juhendada selle vahemälu sõned koordinaate.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Nüüd tuleb kasutada seda luba kasutajate Graphi otsimine. Otsige soovitud `// TODO: implement SearchUsersList` kommenteerige meetodiga GET-päringu Azure AD Graph API päringusse kasutajatele, kelle UPN-i algab antud otsingutermin.  Selleks, et päringu Graph API, tuleb kaasata ka access_token sisse, kuid selle `Authorization` päis kutse – see on koht, kus on ADAL.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Kui teie rakendus taotleb märgiks helistades `getToken(...)`, ADAL proovib juurde naasmiseks märgiks küsimata kasutaja mandaat.  Kui ADAL määratleb, et saada märgiks sisselogimiseks peab kasutaja, see on login väljalogimisel koguda kasutaja mandaat ja tagastada märgiks kinnitamisel eduka.  Kui ADAL ei saa tagastada märgiks mingil põhjusel, see põhjustada mõne `AdalException`.
- Pange tähele, et selle `AuthenticationResult` objekt sisaldab on `tokenCacheStoreItem` objekt, mida saab kasutada rakenduse vajalikud teabe kogumiseks.  Klõpsake soovitud Kiirjuhend `tokenCacheStoreItem` kasutatakse, et kindlaks teha, kui authenitcation on juba toimunud.


## <a name="step-5-build-and-run-the-application"></a>Juhis 5: Koostamine ja käivitage rakendus



Palju õnne! Nüüd on töötamine iOS-i rakendus, mis on võimalus kasutajate autentimiseks, helistage turvaline Web API-de kasutamine OAuth 2.0 ja saada põhiteavet kasutaja.  Kui te pole seda veel teinud, siis nüüd on aeg asustamiseks oma rentniku mõned kasutajatega.  Kiirjuhend rakenduse käivitamine ja logige sisse ühe nende kasutajate.  Otsige teised kasutajad saavad UPN-i põhjal.  Sulgege rakendus ja käivitage see uuesti.  Pange tähele, kuidas kasutaja seansi jääb samaks.

ADAL lihtne lisada kõik need levinud identiteedi funktsioonid rakenduse.  See hoolitseb musta töö teile - vahemälu halduse, OAuthi protokolli tugi, esinemise kasutaja kasutajanimega UI, värskendamine aegunud sõned ja palju muud.  Kõik, mida peaksite teadma on ühe API kõne, `getToken`.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Täiendavad stsenaariumid
Te saate nüüd liikuda täiendavad stsenaariumid.  Võite proovida:

- [Turvaline Node.JS Web API Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
- Saate teada, [Kuidas lubada rist-rakenduse SSO iOS ADAL abil](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
