<properties
    pageTitle="Azure'i teatis jaoturi RTF-vormingus tõuketeatised"
    description="Saate teada, kuidas saatmiseks rakendus iOS-i rikkaliku Tõuketeatiste Azure. Kirjutatud eesmärk-C ja C# koodi näidised."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure'i teatis jaoturi RTF-vormingus tõuketeatised


##<a name="overview"></a>Ülevaade

Selleks, et kasutajad kiirsõnumite rikkaliku sisu, võiksite rakenduse push Lisaks Lihttekst. Need teatised edendada kasutaja tegevuste ja esitada nagu URL-id, helid, pildid ja kuponge ja muud sisu. Selle õpetuse koostab [Teavitage kasutajaid](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) teema ja näidatakse, kuidas saata Tõuketeatiste, mis sisaldavad lõhkelaengu (nt pilt).


Selle õpetuse ühildub iOS 7 ja 8.

  ![][IOS1]

Kõrge:

1. Rakenduse taustväärtus:
    - Talletab rikkaliku last (sel juhul piltide) laos kirjutamata kohalik andmebaas
    - Saadab ID rikkaliku teate seade
2. Rakenduse seadme:
    - Kontaktide kirjutamata, paluda rikkaliku last saab ID-ga
    - Saadetakse kasutajate teatised seadmesse, kui andmete toomine on lõpule jõudnud, ja kuvab selle last kohe, kui kasutajad koputage rohkem


## <a name="webapi-project"></a>WebAPI projekti

1. Avage Visual Studios, [Teavitage kasutajaid](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) õppeteemas loodud **AppBackend** projekt.
2. Teavitage kasutajaid koos ja asetage **img** kaustas kataloogis projekti soovite pilti saada.
3. Klõpsake nuppu **Kuva kõik failid** Solution Exploreris ja paremklõpsake kausta **kaasata**projekti.
4. Valitud pildiga muuta **Manustatud**ressursi selle koostada toimingu aken Atribuudid.

    ![][IOS2]

5. **Notifications.cs**, lisage järgmine lause abil:

        using System.Reflection;

6. Värskendage kogu **teatised** klassi järgmine kood. Ärge unustage kohatäidete asendamine oma teatise keskuse mandaati ja pildi faili nimi.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (valikuline) Vaadake lisateavet selle kohta, kuidas lisada ja projekti ressursid hankimine [manustamise kohta ja Accessi ressursid, kasutades Visual C#](http://support.microsoft.com/kb/319292) .

7. **NotificationsController.cs**, klõpsake uuesti **NotificationsController** koos järgmised pikad. See saadab seadme algse vaikne rikkaliku teatis ID-d ja võimaldab kliendipoolne otsinguga pilt:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Nüüd uuesti kasutada me see rakendus oma veebisaidile Azure'i selleks, et hõlbustamine kõigist seadmetest. Paremklõpsake **AppBackend** projekti ja valige käsk **Avalda**.

9. Valige oma avalda target Azure veebisaidi. Azure'i kontosse sisse logida ja valige olemasolevad või uued veebisait ja kirjutage **sihtkoha URL-i** atribuut vahekaardil **ühendus** . Me viitab see URL oma *taustväärtus lõpp-punkti* allpool olevat selles õpetuses. Klõpsake nuppu **Avalda**.

## <a name="modify-the-ios-project"></a>IOS-i projekti muutmine

Nüüd, kui olete muutnud oma rakenduse taustväärtus saatmine ainult *id* teavitus, muudate oma iOS-i rakenduse toime selle id ja rikkaliku sõnumi toomiseks teie taustväärtus.

1. Avage oma iOS-i projekti ja remote teatised lubama oma peamist rakenduse sihtrakenduse **sihtkohtade** jaotises minnes.

2. Klõpsake **võimalusi**, **Taustal režiimi**sisselülitamine ja märkeruudu **Remote teatised** .

    ![][IOS3]

3. Minge **Main.storyboard**ja veenduge, et vaade kontrolleril (nimetatud nimega Avaleht vaade kontrolleril selles õpetuses) [Teavitamise kasutaja](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) õpetuse kaudu.

4. **Navigeerimise kontrolleril** lisada oma süžeeskeemi ja juhtelemendi lohistamisega Home vaate kontrolleril teha **root vaade** navigeerimine. Veenduge, et **On algse vaate kontrolleril** atribuute inspektor on valitud navigeerimise kontrolleril ainult.

5. Lisage **Vaate kontrolleril** süžeeskeemi ja lisada ka **Pildi kuvamine**. See on lehe kasutajad näevad, kui nad valida, lugege lisateavet klõpsates soovitud notifiication. Teie süžeeskeemile peaks välja nägema järgmine:

    ![][IOS4]

6. Klõpsake **Avaleht vaade kontrolleril** süžeeskeemi ja veenduge, et see on **homeViewController** oma **Kohandatud klassi** ja **Süžeeskeemi ID** jaotises identiteedi inspektor.

7. Tehke sama pildi kuvamine kontrolleril **imageViewController**nimega.

8. Looge uue vaate kontrolleril klassi pealkirjaga **imageViewController** UI äsja loodud tegelema.

9. Lisage **imageViewController.h**, selle domeenikontrolleri kasutajaliidese määrust järgmist. Veenduge, et juhtelemendi-lohistage pilt ajaskaalavaatele atribuutidest kahe link.

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. Lisage **imageViewController.m**, **viewDidload**lõpus järgmist:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. **AppDelegate.m**, importida pilt kontrolleril, mis on loodud:

        #import "imageViewController.h"

12. Lisage mõni järgmised deklaratsioon with jaotises kasutajaliides.

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. **AppDelegate**, veenduge, et teie rakendus registreerib vaikne teatised **rakendus: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Süžeeskeemi tegema järgmised rakendamisel jaoks **rakenduse: didRegisterForRemoteNotificationsWithDeviceToken** subsitute UI muudab kontole:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Seejärel lisage **AppDelegate.m** laadida pildi oma lõpp-punkti ja saata kohaliku teatise, kui allalaadimine on lõpetatud järgmistest meetoditest. Veenduge, et Fondiasendus kohatäite `{backend endpoint}` koos oma taustväärtus lõpp-punkt:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Kohaliku teate kohal toime avades pildi kuvamine domeenikontrolleri sisse **AppDelegate.m** järgmistest viisidest:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Käivitage rakendus

1. XCode, käivitage rakendus iOS-seadmes on füüsilise (push teatised ei tööta soovitud simulaatorit).

2. IOS-i rakenduse UI, sisestage kasutajanimi ja parool on sama väärtusega autentimist ja klõpsake nuppu **Logi sisse**.

3. Klõpsake käsku **saada tõuketeatised** ja te peaksite nägema teatise rakendus. Kui klõpsate käsku **rohkem**, viiakse kaasamiseks oma rakenduse taustväärtus valitud pilt.

4. Saate klõpsata nuppu **saada tõuketeatised** ja kohe oma seadme nuppu Avaleht. Mõne hetke aega, saate tõuketeatised teatis. Kui seda koputage või klõpsake nuppu rohkem, viiakse rakenduse ja rikkaliku pildi sisu.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
