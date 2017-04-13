<properties
    pageTitle="Azure'i teatis jaoturi turvaline tõuketeatised"
    description="Saate teada, kuidas saatmiseks rakendus iOS-i secure Tõuketeatiste Azure. Kirjutatud eesmärk-C ja C# koodi näidised."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure'i teatis jaoturi turvaline tõuketeatised

> [AZURE.SELECTOR]
- [Windowsi universaalne](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS-i](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Ülevaade

Tõuketeatised teatis tugi Microsoft Azure võimaldab teil lihtne kasutada, paljudele, mastaabitud kontorist tõuketeatised taristu, mis lihtsustab rakendamise Tõuketeatiste tarbija ja ettevõtte puhul mobiilside kaudu juurdepääsu.

Tõttu nõuetele või turvalisus piiranguid, mõnikord rakenduse võib soovite kaasata midagi teate, et ei saa edastada standard tõuketeatised teatis taristu kaudu. Selles õppetükis kirjeldatakse, kuidas saavutada sama kogemus saatmisega tundliku teabe turvaline, autenditud kliendi seade ja rakenduse taustväärtus vahelise ühenduse kaudu.

Kõrge, voo on järgmine:

1. Rakenduse tagaandmebaas:
    - Salvestab turvaline last Tagaandmebaasi.
    - Saadab teate ID (secure teavet saadetakse) seade.
2. Rakendus töötab seadme taustal, kui nad saavad teate:
    - Seadme kontaktide paluda turvaline last tagaandmebaas.
    - Rakenduse saate kuvada soovitud last kujul seadmesse teatis.

See on oluline märkida, et eelmise voogu (ja selle õpetuse) me oletame, et seade talletab mõne autentimise luba kohalike salvestusruumi, pärast kasutaja logib sisse. See tagab täielikult tõrgeteta, kui seadme saate tuua teate turvaline last selle märgiks abil. Kui rakenduse salvestada seadme autentimine märkide või kui nende tähiste saate aegunud, seadme rakendus, teate saamisel peaks olema kuvatud üldise teatise viipa kasutaja käivitage rakendus. Rakenduse siis autendib kasutaja ja kuvab teate last.

Selle õpetuse Secure tõuketeatised näitab, kuidas turvaliselt tõuketeatised teatise saatmine. [Teavitage kasutajaid](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) õpetuse, et teil tuleb täita juhised selle õpetuse esmalt õpetus tugineb.

> [AZURE.NOTE] Selle õpetuse eeldab, et olete loonud ja oma teate jaoturi konfigureeritud, nagu on kirjeldatud [Alustamine teatis jaoturi (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>IOS-i projekti muutmine

Nüüd, kui saate muuta oma rakenduse tagaandmebaas lihtsalt *id* teatise saata, tuleb muuta oma iOS-i rakenduse teatise ja kõne tagasi oma tagaandmebaas kuvada turvalist sõnumit alla laadida.

Selle eesmärgi saavutamiseks meil kirjutada loogika turvalise sisu toomiseks rakenduse tagaandmebaas.

1. **AppDelegate.m**, veenduge, et rakendus registrite vaikne teatised, et töötleb teatis id saadetud soovitud taustväärtus. Lisage **UIRemoteNotificationTypeNewsstandContentAvailability** suvand didFinishLaunchingWithOptions.

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. Lisage oma **AppDelegate.m** järgmised deklaratsioon with ülaosas kuvatakse rakendamist jaotis:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Lisage rakendamine jaotises järgmine kood, asendades kohatäite `{back-end endpoint}` lõpp-punkti jaoks oma tagaandmebaas varem saadud abil:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Nüüd on sissetulevate teatis ja eeltoodud meetodi abil saate tuua sisu kuvamiseks. Esmalt meil Luba taustal, kui tõuketeatised teate oma iOS-i rakendus. **XCode**, valige vasakul paanil projekti rakendus ja seejärel klõpsake oma peamist rakenduse siht keskse paanil jaotises **sihtkohtade** .

5. Klõpsake oma **võimaluste** vahekaarti oma keskse paani ülaosas ja märkeruudu **Remote teatised** .

    ![][IOS1]


6. Lisage **AppDelegate.m** Tõuketeatiste käsitlema järgmisel viisil:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Pange tähele, et eelistada toime tagasi-lõpuks puuduvad autentimise päise atribuudi või hülgamiseks. Teatud juhtudel käsitsemise sõltuvad enamasti oma target kasutusvõimalused. Üheks võimaluseks on teatis koos üldise viip kasutaja autentida tuua tegeliku teate kuvamiseks.

## <a name="run-the-application"></a>Käivitage rakendus

Rakenduse käivitamiseks tehke järgmist.

1. XCode, käivitage rakendus iOS-seadmes on füüsilise (push teatised ei tööta soovitud simulaatorit).

2. IOS-i rakenduse UI, sisestage kasutajanimi ja parool. Need võivad olla mis tahes stringi, kuid need peavad olema sama väärtuse.

3. IOS-i rakenduse UI, klõpsake nuppu **Logi sisse**. Klõpsake nuppu **saada tõuketeatised**. Peaksite nägema turvaline teate kuvamist teatis keskuses.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
