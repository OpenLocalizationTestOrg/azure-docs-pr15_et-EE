<properties
    pageTitle="Teatis jaoturi lokaliseeritud katkestamine uudiste õpetuse iOS-i"
    description="Saate teada, kuidas Azure'i teenus siini teatis jaoturi abil saate saata lokaliseeritud server uudiste teatised (iOS)."
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
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>IOS-seadmete lokaliseeritud uudiseid saatmine teatis jaoturi abil

> [AZURE.SELECTOR]
- [Windowsi poe C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS-i](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Ülevaade

See teema näitab leviedastus server uudiste teatised, mis on lokaliseeritud keel ja seadmest [Mallid](notification-hubs-templates-cross-platform-push-messages.md) funktsioon Azure'i teatis jaoturi abil. Selles õpetuses alustatakse iOS-i rakenduse [Kasutamine teatis jaoturi saata uudiseid]loodud. Kui olete lõpetanud, on võimalik kasutajaks olete huvitatud kategooriad, määrata teateid ja vastu võtta ainult Tõuketeatiste valitud kategooria selles keeles keel.


On selle stsenaariumi koosneb kahest osast.

- iOS-i rakendus võimaldab kliendi seadmed, määrake sobiv keel ja tellimine eri server uudiste kategooriaid;

- funktsiooni tagaandmebaas edastab teated, **silt** ja **malli** feautres, Azure teatis jaoturi abil.



##<a name="prerequisites"></a>Eeltingimused

Olema juba lõpetanud õpetuse [Kasutamine teatis jaoturi saata uudiseid] ja on saadaval, koodi, sest selles õpetuses põhineb otse koodi.

Visual Studio 2012 või uuem versioon pole kohustuslik.



##<a name="template-concepts"></a>Malli mõisted

[Kasuta teatis jaoturi saata uudiseid] ehitatud rakenduse kasutatud **Sildid** tellida erineva kategooria teatised.
Paljudes rakendustes siiski suunata mitmest turgude ja nõuavad lokaliseerimine. See tähendab, et teadete, ise sisu on lokaliseeritud ja seadmete õige komplektile toimetatud kohale.
Selles teemas näitame hõlpsalt esitamisel lokaliseeritud server uudiste teatised teatis jaoturi funktsioon **malli** kasutamise kohta.

Märkus: loomine mitme versiooni igal sildil on üks võimalus saata lokaliseeritud teatised. Näiteks inglise, prantsuse ja mandariini toetamiseks peame kolme eri sildid maailma uudiseid: "world_en", "world_fr" ja "world_ch". Me oleks siis saata lokaliseeritud versiooni maailma uudised iga järgmisi silte. Selles teemas kasutame Mallid vältimiseks leviku sildid ja nõue mitme sõnumite saatmiseks.

Mallid on kõrge, nii, et määrata, kuidas konkreetse seadme peaksid saama teatise. Mall määrab täpse last viidates atribuudid, mis pole teie rakenduse tagaandmebaas saadetud sõnumile. Meie puhul saadame lokaadi diagnostika sõnum, mis sisaldab kõiki toetatud keeltes:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Siis me tagada, et seadmed registreerida Mall, mis viitab õige atribuut. Näiteks iOS-i rakendus, mis soovib registreerida Prantsuse uudiseid register järgmist:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Mallid on väga võimas funktsiooni kohta lisateavet meie [Mallid](notification-hubs-templates-cross-platform-push-messages.md) artikkel.

##<a name="the-app-user-interface"></a>Rakenduse kasutajaliides

Me nüüd muuta teemas [Kasutamine teatis jaoturi saata uudiseid] saata lokaliseeritud uudiseid mallide kasutamine loodud katkestamine News kasutamine.


Lisage oma MainStoryboard_iPhone.storyboard segmenditud juhtelement on kolm keelt, mida me ei toeta: inglise, prantsuse ja mandariini.

![][13]

Veenduge, et lisada ka IBOutlet oma ViewController.h, nagu allpool näidatud:

![][14]

##<a name="building-the-ios-app"></a>IOS-i rakendus ehitamine


1. Oma Notification.h *retrieveLocale* meetod, lisada ja muuta pood ja tellida meetodid, nagu allpool näidatud:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Muuta oma Notification.m *storeCategoriesAndSubscribe* meetodit, lisades lokaadi parameetri ja talletamine selle kasutaja vaikesätted:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Muutke *tellida* kaasata lokaadi meetodit:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Pange tähele, kuidas nüüd kasutame meetodit *registerTemplateWithDeviceToken*asemel *registerNativeWithDeviceToken*. Kui malli registreerib meil pakuvad json Mall ja ka malli nimi (nagu meie rakenduse võiksite registreerida erinevaid malle). Veenduge, et registreeruda oma kategooriad sildid, nagu soovime veendumaks, et saada nende uudiseid notifciations.

    Meetodi lokaadi toomiseks vaikesätete kasutaja lisamiseks tehke järgmist.

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Nüüd, kui me muutnud meie teatised ainekursuse, meil veenduge, et meie ViewController kasutab uue UISegmentControl. Lisage järgmine rida *viewDidLoad* veendumaks, et kuvada lokaadi praegu valitud meetodit.

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Teie *tellida* meetodi, muutke oma kõne *storeCategoriesAndSubscribe* järgmine:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Lõpetuseks tuleb värskendada oma AppDelegate.m *didRegisterForRemoteNotificationsWithDeviceToken* meetod, et saate värskendada registreerimise õigesti rakenduse käivitamisel. Oma kõne teatiste järgmise meetodiga *tellida* muutmiseks tehke järgmist.

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(valikuline) .Net-i konsooli rakenduse lokaliseeritud malli teatisi saada.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(valikuline) Seadme lokaliseeritud malli teatiste saatmine

Kui te ei pääseks juurde Visual Studio või soovite lihtsalt testida lokaliseeritud malli teatiste saatmine otse rakenduse seadme kaudu.  Saate lihtsa lokaliseeritud malli parameetrid lisage soovitud `SendNotificationRESTAPI` meetod, mis on määratletud eelmises õppeteemas.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Järgmised sammud

Mallide kasutamise kohta leiate lisateavet teemast:

- [Teavitage kasutajaid teatis jaoturi abil: ASP.net-i]
- [Teavitage kasutajaid teatis jaoturi abil: Mobile teenused]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Uudiseid saata teate jaoturi abil]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Teavitage kasutajaid teatis jaoturi abil: ASP.net-i]: /manage/services/notification-hubs/notify-users-aspnet
[Teavitage kasutajaid teatis jaoturi abil: Mobile teenused]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
