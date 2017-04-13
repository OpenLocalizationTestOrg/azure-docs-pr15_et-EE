<properties
    pageTitle="Praeguse kasutaja jaoks Tõuketeatiste registreerimine Web API abil | Microsoft Azure'i"
    description="Siit saate teada, kuidas taotleda tõuketeatised teate registreerimine iOS-i rakenduse Azure'i teatis jaoturi kui registeration sooritab ASP.net-i veebi-API."
    services="notification-hubs"
    documentationCenter="ios"
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

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Praeguse kasutaja jaoks Tõuketeatiste registreerimine ASP.net-i abil

> [AZURE.SELECTOR]
- [iOS-i](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Ülevaade

See teema näitab, kuidas taotleda tõuketeatised teatise registreerimise Azure teatis jaoturi kui registreerimine sooritab ASP.net-i veebi-API. Selles teemas laiendab õpetuse [teatis jaoturi teata kasutajad]. Olema juba lõpetanud nõutud toimingute selles õpetuses autenditud mobiilsideteenuse loomiseks. Teata kasutajate stsenaarium kohta leiate lisateavet teemast [teatise jaoturi teata kasutajad].

##<a name="update-your-app"></a>Rakenduse värskendamine  

1. Lisage oma MainStoryboard_iPhone.storyboard objekti teegist järgmised komponendid:

    + **Sildi**: "Push kasutaja teatise jaoturi"
    + **Sildi**: "InstallationId"
    + **Sildi**: "Kasutaja"
    + **Tekstivälja**: "Kasutaja"
    + **Sildi**: "Parool"
    + **Tekstivälja**: "Parool"
    + **Nupp**: "Login"

    Selles etapis oma süžeeskeemil näeb välja järgmine:

    ![][0]

2. Sisselogimisabimehe redaktoris loomine väljundi kõik juhtelemendid, mis on sisse lülitatud ja helistada, tekstiväljad kasutajaga vaade kontrolleril (delegaat) ja luua **toimingu** nuppu **Logi sisse** .

    ![][1]

    Failis BreakingNewsViewController.h peaks nüüd sisaldama järgmine kood:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Luua nimega **DeviceInfo**klassi ja kopeerige järgmine kood DeviceInfo.h faili jaotises kasutajaliides.

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Kopeerige järgmine kood DeviceInfo.m faili jaotises rakendamist.

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. PushToUserAppDelegate.h, lisage järgmised atribuut singleton:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. PushToUserAppDelegate.m **didFinishLaunchingWithOptions** meetod, lisage järgmine kood:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Esirea lähtestab **DeviceInfo** singleton. Teisel real käivitatakse registreerimise push teatised, mis on juba esitamine on juba olete täitnud õpetusega [Alustamine teatis jaoturi] .

9. PushToUserAppDelegate.m, rakendada meetod **didRegisterForRemoteNotificationsWithDeviceToken** oma AppDelegate ja lisage järgmine kood:

        self.deviceInfo.deviceToken = deviceToken;

    Seda määrab taotluse seadme sõnet.

    > [AZURE.NOTE] Selles etapis ei tohiks muu kood selle meetodi. Kui teil on juba **registerNativeWithDeviceToken** meetod, mis lisati õpetusega [Alustamine teatis jaoturi](/manage/services/notification-hubs/get-started-notification-hubs-ios/) lõppedes kõne, peate kommentaari välja või Eemalda selle kõne.

10. Lisage PushToUserAppDelegate.m faili sündmuseohjuri järgmisel viisil:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     See meetod kuvab teatise UI, kui rakenduse saab teatisi, kui see töötab.

9. Avage fail PushToUserViewController.m ja tagastab klaviatuuri pärast rakendamist:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. Failis PushToUserViewController.m meetodi **viewDidLoad** lähtestab installationId sildi järgmiselt:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Kasutajaliidese PushToUserViewController.m lisage järgmised atribuudid.

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Seejärel lisage pärast rakendamist.

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopeerige järgmine kood loodud XCode **sisselogimise** sündmuseohjuri meetodit.

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Seda meetodit saab nii installi ID ja kanali tõuketeatised ja saadab, seadme tüüp, koos autenditud Web API meetod, mis loob registreerimise teatis jaoturi. Selle veebi-API on määratletud [teatis jaoturi teata kasutajad].

Nüüd, kui kliendi rakendus on värskendatud, [Teata kasutajad teatis jaoturi] naasmiseks ja mobiilsideteenuse teatised saata teate jaoturi abil värskendada.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Teavitage kasutajaid teatis jaoturi abil]: /manage/services/notification-hubs/notify-users-aspnet

[Teatis jaoturi kasutamise alustamine]: /manage/services/notification-hubs/get-started-notification-hubs-ios
