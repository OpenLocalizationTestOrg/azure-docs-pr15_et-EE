<properties
    pageTitle="Tõuketeatiste saata iOS-is Azure'i teatis jaoturi | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas saata Tõuketeatiste iOS-i rakendus Azure'i teatis jaoturi abil."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="tõuketeatised teatis Tõuketeatiste, iOS-i tõuketeatised"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>IOS-is Azure'i teatis jaoturi Tõuketeatiste saatmine

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Selle õpetuse näidatakse, kuidas saata Tõuketeatiste iOS-i rakendus Azure'i teatis jaoturi abil. Saate luua tühja iOS-i rakendus, mis saab Tõuketeatiste [Apple'i Lükketeatiste teenus (APNs-i)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)kasutades. 

Kui olete lõpetanud, saate küll leviedastus Tõuketeatiste töötavad rakenduse seadmed teie teate jaoturi abil.

## <a name="before-you-begin"></a>Enne alustamist

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Selle õpetuse lõplikus kood leiate [github](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ [SDK versioon 1.2.4 Mobile teenuste iOS-i]
+ [Xcode] uusim versioon
+ IOS-i 8 (või uuem versioon)-toega seadme
+ [Apple arendaja programmi](https://developer.apple.com/programs/) liikmeks.

   > [AZURE.NOTE] Tõuketeatiste konfigureerimine nõuded, kuna peab juurutada ja testida Tõuketeatiste füüsilise iOS-i seadmes (iPhone või iPad) asemel iOS-i simulaatorit.

Selle õpetuse lõpuleviimine on nõutav kõik teatis jaoturi õpetused iOS-i rakendused.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigureerige oma teate jaoturi for iOS-i tõuketeatised

Selles jaotises juhatab teid läbi uus teatis keskuse loomine ja koos APNs-i abil loodud **.p12** tõuketeatised serdi autentimise konfigureerimine. Kui soovite kasutada teate jaoturi, mille olete juba loonud, võite jätkata juhisega 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p><b>Teatis teenused</b> nuppu tera <b>sätted</b> ja seejärel valige <b>Apple (APNs-i)</b>. Klõpsake <b>Serdi üleslaadimine</b> ja valige eksporditud varem <b>.p12</b> -fail. Veenduge, et määrata ka õige parooli.</p>
<p>Veenduge, et valida <b>liivakastirežiimi</b> , kuna see on arengu. Kui soovite saata kasutajatele, kes ostsid poest rakenduse Tõuketeatiste kasutada ainult <b>tootmise</b> .</p>
</li>
</ol>
&emsp;&emsp;![Azure'i portaalis APNs-i konfigureerimine](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Azure'i portaalis sertimine APNs-i konfigureerimine](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Teie teate jaoturi on nüüd konfigureeritud töötamiseks APNs-i ja teil on rakenduse registreerida ja saata Tõuketeatiste stringid ühendus.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Ühenduse loomine oma iOS-i rakenduse teatise jaoturi

1. Xcode, iOS-i uue projekti loomine ja valige **Üks kuvamine rakenduse** mall.

    ![Xcode - ühte vaatesse rakendus][8]

2. Kui seate oma uue projekti suvandid, veenduge, et kasutada sama **Tootenimi** ja **Ettevõtte ID** , kui varem määratud kogumi ID Apple arendaja portaali.

    ![Xcode - projekti suvandid][11]

3. Jaotises **sihtkohtade**, klõpsake oma projekti nime, vahekaarti **Sätted koostamine** ja laiendamine **Koodi allkirjastamise identiteedi**ja määrake jaotises **silumine**, teie identiteedi koodi allkirjastamiseks. Tavalise **Kõik** **tasemed** alates **lihtsamatest** ja määrata **Ettevalmistamise profiil** varem loodud ettevalmistamise profiilile.

    Kui te ei näe ettevalmistamise uue profiili Xcode loodud, proovige oma allkirja identiteedile profiilid värskendamine. **Xcode** klõpsake menüü, klõpsake käsku **eelistused**, klõpsake vahekaarti **konto** , klõpsake nuppu **Kuva üksikasjad** , klõpsake oma allkirja identiteedi ja klõpsake paremas allnurgas nuppu Värskenda.

    ![Xcode - ettevalmistamise profiil][9]

4. Laadige alla [SDK versioon 1.2.4 Mobile teenuste iOS-i] ja pakkige see lahti faili. Xcode, paremklõpsake projekti ja **Failide lisamine** suvandit Xcode projekti **WindowsAzureMessaging.framework** kausta lisada. Valige **vajadusel üksuste kopeerimine**ja seejärel klõpsake nuppu **Lisa**.

    >[AZURE.NOTE] Teatis jaoturiga SDK ei toeta praegu bitcode Xcode 7.  Projekti peate määrama **Koostada suvandid** **ei** **Luba Bitcode** .

    ![Azure'i SDK pakkige see lahti][10]

5. Uue faili päise lisamine projekti nimega `HubInfo.h`. Selle faili hoiab teie teate jaoturi on konstandid.  Lisage järgmised määratlused ja stringi sõnasõnaline kohatäidete asendamine oma *keskuse nimi* ja *DefaultListenSharedAccessSignature* eespool märgitud.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Avage oma `AppDelegate.h` fail lisada järgmised impordi suuniseid:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. Klõpsake oma `AppDelegate.m file`, lisage järgmine kood on `didFinishLaunchingWithOptions` meetod, mis põhineb oma iOS-i versioon. Järgmine kood registreerib oma seadme pidet APNs-i:

    IOS-i 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    IOS-i versioonid enne 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Sama faili, lisage järgmistest meetoditest. Järgmine kood loob ühenduse teate jaoturi abil määratud HubInfo.h ühendusteabe. See siis annab seadme luba teate jaoturi nii, et teate jaoturi saate saata teatised:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Sama faili lisamine on **UIAlert** kuvamiseks, kui teate kättesaamise rakendus on aktiivne järgmisel viisil:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Koostamine ja käivitage rakendus oma seadmes pole ebaõnnestumisi kinnitamiseks.

## <a name="send-test-push-notifications"></a>Test Tõuketeatiste saatmine


Saate testida rakenduse teatiste saatmisega Tõuketeatiste [Azure portaali] kaudu **tõrkeotsingu** lõik jaoturi tera (kasutage suvandi *Testida saata* ).

![Azure'i portaal - testi saatmine][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Valikuline) Tõuketeatiste saatmine rakendusest

>[AZURE.IMPORTANT] See näide teatiste saatmine kliendi rakendus on mõeldud õppe eesmärgil. Kuna see nõuab selle `DefaultFullSharedAccessSignature` võib kliendi rakendus, seab selle oma teate jaoturi, et kasutaja võib saada saata oma klientidele teatised volitamata juurdepääsu riski.

Kui soovite saata Tõuketeatiste kaudu oma rakendusest, selles jaotises antakse näide sellest, kuidas seda teha ülejäänud liidest kasutades.

1. Avage Xcode, `Main.storyboard` ja lisage järgmised komponendid UI võimaldab kasutajal saata Tõuketeatiste rakenduse teegist objekti:

    - Sildi sildi teksti. Seda kasutatakse aruandes vigu teatiste saatmine. Atribuudi **read** peaks väärtuseks **0** , et see automaatselt maht piiratud paremale ja vasakule veerised ja vaate ülaosas.
    - ** **Sisestage teavitussõnumi**määratud kohatäitetekstiga** tekstiväli. Piirata vaid sildi all välja nagu allpool näidatud. Määrake vaate kontrolleril delegaadina väljund.
    - Nupu **Saada teatis** pealkirjaga piiratud tekstivälja all ja horisontaalne keskele.

    Vaade peaks välja nägema järgmine:

    ![Xcode designer][32]


2. Siltide ja välja [lisamine väljundi](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) ühendatud teie arvates ja värskendage oma `interface` määratlus toetamiseks `UITextFieldDelegate` ja `NSXMLParserDelegate`. Lisage kolme atribuuti andmed aidata kõne REST API-ja sõelumine vastus allpool.

    Failis ViewController.h peaks välja nägema järgmine:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Avatud `HubInfo.h` ja lisage järgmine konstandid, mida kasutatakse teie jaoturi teatiste saatmiseks. Asendage kohatäide stringi sõnasõnaline tegelik *DefaultFullSharedAccessSignature* ühendusstring.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Lisage järgmine `#import` lauseid oma `ViewController.h` fail.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. Klõpsake `ViewController.m` lisada järgmine kood kasutajaliidese rakendamist. Järgmine kood on sõeluda oma *DefaultFullSharedAccessSignature* ühendusstring. Nagu [REST API viide](http://msdn.microsoft.com/library/azure/dn495627.aspx), kasutatakse selle sõelutud teabe SaS luba **Luba** taotluse päise loomiseks.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. Klõpsake `ViewController.m`, värskendada selle `viewDidLoad` meetod sõeluda ühendusstringi vaate laadimise ajal. Lisada ka kasuliku meetodid, allpool kasutajaliidese rakendamist.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. Klõpsake `ViewController.m`, lisage järgmine kood kasutajaliidese rakendamiseks luua SaS autoriseerimine luba, mis antakse **Luba** päises mainitud [REST API viide](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + **Teatise saatmine** lohistage nupp `ViewController.m` nimega **SendNotificationMessage** **Puudutage alla** sündmuse toimingu lisamiseks. Järgmine kood saata teate REST API abil värskendada meetod.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. Klõpsake `ViewController.m`, lisada järgmised volitatud esindaja meetod toetamiseks sulgemist klaviatuuri tekstiväli. Lohistage teksti välja kasutajaliidese designer seadmiseks vaade kontrolleril delegaadina väljund vaade kontrolleril ikooni (CTRL).

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. Klõpsake `ViewController.m`, lisada volitatud esindaja toetamiseks sõelumine vastus, kasutades järgmist `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Projekti koostamine ja veenduge, et tõrkeid.


> [AZURE.NOTE] Kui teil ilmneb koostamine tõrge Xcode7 bitcode tugi, muudate **Sätteid koostamine** > Xcode **ei** **Luba Bitcode (ENABLE_BITCODE)** . Teatis jaoturi SDK ei toeta praegu bitcode. 

Apple'i [kohalik Push teatis programmeerimisega juhendi]leiate kõigi võimalike teatis lõhkelaengu.


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Kontrollimine, kui saate rakenduse tõuketeatised

Tõuketeatiste iOS testimiseks peab juurutama füüsilise iOS-i seadmes rakendus. Te ei saa saata Apple'i lükketeatiste, kasutades iOS-i simulaatorit.

1. Käivitage rakendus ning veenduge, et registreerimise õnnestub, ja seejärel klõpsake nuppu **OK**.

    ![iOS-i rakenduse Push teatise registreerimise Test][33]

2. Saate saata teavitus testi tõuketeatised [Azure portaali]ülalpool kirjeldatud. Kui lisasite saatmiseks tõuketeatisi rakenduse kood, puudutage tekstivälja teksti välja sisestage vastav teade. Vajutage klaviatuuri või saata teavitussõnum vaate nupp **Teatise saatmine** nuppu **saada** .

    ![iOS-i rakenduse Push teatise saatmine Test][34]

3. Tõuketeatised teatis saadetakse kõigis seadmetes, mis on juba registreeritud teatisi kindla teatise keskuse kaudu.

    ![iOS-i rakenduse Push teatise vastuvõtmine Test][35]


##<a name="next-steps"></a>Järgmised sammud

Selles näites lihtsa eetris Tõuketeatiste registreeritud iOS-i kõikides seadmetes. Soovitame oma õ, mis pidage [Azure'i teatis jaoturi teavitamise kasutajate iOS-is .NET kirjutamata] õppeteema, mis juhendab teid loomise taustväärtus, saata siltide kasutamine Tõuketeatiste järgmise sammuna. 

Kui soovite segmendi kasutajate poolt huvirühmade, saate lisaks liikuda [Kasutamine teatis jaoturi saata uudiseid] õpetuse. 

Üldist teavet teatise jaoturi kohta, vt [Teatis jaoturi juhiseid].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[SDK versioon 1.2.4 Mobile teenuste iOS-i]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Teatis jaoturi juhised]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure'i teatis jaoturi teavitamise kasutajate .NET taustväärtus iOS-is]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Uudiseid saata teate jaoturi abil]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Kohaliku ja lükake teatis programmeerimise juhend]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure'i portaal]: https://portal.azure.com