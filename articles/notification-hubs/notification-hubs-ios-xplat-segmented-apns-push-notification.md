<properties
    pageTitle="Teatis jaoturi server uudiste Õppeteema – iOS-i"
    description="Saate teada, kuidas saata server uudiste teatised iOS-seadmete Azure'i teenus siini teatis jaoturi abil."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Teatis jaoturi abil saate saata uudiseid

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Ülevaade

Selles teemas näidatakse, kuidas leviedastus Server Uudised teateid rakendus iOS-i Azure teatis jaoturi abil. Kui olete lõpetanud, kuvatakse Server Uudised kategooriad olete huvitatud registreerida ja vastu võtta ainult Tõuketeatiste kategooriate. Selle stsenaariumi on levinud mustri palju rakendused, kus on teatised saadetakse rühmad kasutajate, mis on varem huvi nende, nt RSS-lugeja, rakendused muusika ventilaatoreid jne.

Leviedastuse stsenaariumid on lubatud, sh ühe või mitme _siltide_ loomisel registreerimise teate jaoturi. Kui sildi, saadetakse teatised, kuvatakse kõigis seadmetes, mis on registreeritud sildi jaoks teate. Kuna sildid on lihtsalt stringid, ei pea neid ette valmistada ette. Siltide kohta lisateabe saamiseks vaadake [teatis jaoturi marsruutimist ja sildi avaldised](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Eeltingimused

Selles teemas koostab loodud [teatis jaoturi alustamine]rakenduse[get-started]. Selles õpetuses enne olema juba lõpetanud [Alustamine teatis jaoturi][get-started].

##<a name="add-category-selection-to-the-app"></a>Kategooria valik lisamine rakendusse

Esimene samm on Kasutajaliidese elementidele lisada oma olemasoleva süžeeskeemil, mis võimaldavad kasutajal valida kategooriate registreerida. Seadmes on talletatud kasutaja poolt valitud kategooriad. Rakenduse käivitamisel luuakse seadme registreerimine oma teatise keskuses koos valitud kategooria sildid.

1. Lisage oma MainStoryboard_iPhone.storyboard objekti teegist järgmised komponendid:
    + Sildi "Katkestamine News" tekstiga,
    + Märgib kategooria tekstiga "Maailma", "Poliitika", "Ettevõtetele", "Tehnoloogia", "Teadus", "Sport"
    + Kuus parameetrid, üks kategooria kohta määrata iga vahetamise **oleku** vaikimisi välja **lülitatud** .
    + Ühe nuppu sildiga "Telli"

    Teie süžeeskeemile peaks välja nägema järgmine:

    ![][3]

2. Abimehe redaktoris luua kõik parameetrid müügivõimaluste ja helistada "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Saate luua toimingu nupp nimega "Telli". Teie ViewController.h sisaldab järgmist:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Luua uue **Kakao puudutage klassi** nimetatakse `Notifications`. Kopeerige järgmine kood faili Notifications.h kasutajaliidese osas.

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Notifications.m järgmist impordi direktiivi lisamiseks tehke järgmist.

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopeerige järgmine kood rakendamist jaotises faili Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    See tund kasutab kohalikku ja selle seadme saadud Uudiste kategooriad. See sisaldab ka, meetodi nende kategooriate [malli](notification-hubs-templates-cross-platform-push-messages.md) registreerimise abil registreerida.

7. Failis AppDelegate.h lisamine lauset impordi Notifications.h ja lisada atribuudi teatised klassi eksemplar:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. AppDelegate.m **didFinishLaunchingWithOptions** meetod, lisage koodi lähtestada teatised eksemplari alguses meetodit.  
 
    `HUBNAME`ja `HUBLISTENACCESS` (määratletud hubinfo.h) peaks juba selle `<hub name>` ja `<connection string with listen access>` kohatäidete asendada oma teatise jaoturi nimi ja ühendusstringi *DefaultListenSharedAccessSignature* , mida te saada varasemas versioonis

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Kuna identimisteavet, mida levitatakse kliendi rakendus pole üldjuhul turvaline, peaks oma kliendi rakendusega jaotada kuulata juurdepääsu võti. Rakenduse teatised, kuid olemasolevate registreerimise registreerida ei saa muuta access võimaldab kuulata ja teatised ei saa saata. Täielik juurdepääs kasutatakse turvalise kirjutamata teenuses saatmise teatised ja muuta olemasolevaid registreerimise.


9. AppDelegate.m **didRegisterForRemoteNotificationsWithDeviceToken** meetod, asendage kood meetodi järgmine kood seadme luba edastamiseks klassi teatised. Teatiste klassi täidab teatiste kategooriatega registreerimisel. Kui kasutaja muudab kategooria Valikud, me kutsume selle `subscribeWithCategories` meetod vastuseks nende värskendamiseks nuppu **Telli** .

    > [AZURE.NOTE] Kuna seadme luba määratud, Apple Push teatise teenuse (APNS) saate igal ajal võimalus, tuleks Registreeruge teatised tihti, et vältida tõrkeid teatis. Selles näites registreerib teavitamise iga kord, kui rakendus käivitub. Sageli töötavad rakendused, rohkem kui üks kord päevas, saate tõenäoliselt vahele jätta registreerimise säilitada läbilaskevõime kui vähem kui päev on möödunud eelmise registreerimise.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Pange tähele, et sel hetkel on koodi pole **didRegisterForRemoteNotificationsWithDeviceToken** meetod.

10. Järgmistest meetoditest peaks olema juba olemas AppDelegate.m lõpuleviimist [Alustamine teatis jaoturi] [ get-started] õpetuse.  Kui ei, siis neid lisada.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    See meetod tegeleb saadud kui rakendus töötab, kuvades lihtsa **UIAlert**teatised.

11. ViewController.m, lisada impordi lause AppDelegate.h ja kopeerige järgmine kood XCode loodud **tellida** meetod. Järgmine kood update teate registreerimine uue kategooria siltide kasutaja on valitud kasutajaliidese kasutamiseks.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    See meetod **NSMutableArray** , kategooriate loob ja kasutab **teatised** klassi talletamiseks kohalikku talletamist loendi ja registreerib sildid vastavad teie teate jaoturi. Kategooriate muutmisel uuesti registreerimise luua uute kategooriate.

12. ViewController.m, lisage järgmine kood **viewDidLoad** meetodi abil kasutajaliides, mis on varem salvestatud kategooriate määramine.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Rakenduse nüüd saate salvestada seadme registreerimiseks teate jaoturi rakenduse käivitamisel kasutatav kohalik salvestusruum kategooria.  Kasutaja valikut kategooriad käitusajal muuta, ja klõpsake nuppu **Telli** meetodi abil värskendada seadme registreerimine. Järgmiseks värskendate rakenduse Server Uudised teatiste saatmine otse rakenduse ise.


##<a name="optional-sending-tagged-notifications"></a>(valikuline) Sildistatud teatiste saatmine

Kui teil pole juurdepääsu Visual Studio, saate järgmise jaotise juurde ja teatiste saatmine rakendusest ise. Lisaks saate saata teate proper malli [Azure klassikaline portaali] silumine menüü teie teate jaoturi abil. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(valikuline) Saada teatisi seadme

Tavaliselt teatised saadetaks, taustväärtus teenus, kuid saate saata server Uudised teatised otse rakenduse kaudu. Selleks uuendame selle `SendNotificationRESTAPI` meetod, mis me määratletud [Alustamine teatis jaoturi] [ get-started] õpetuse.


1. ViewController.m värskendus on `SendNotificationRESTAPI` samasse järgib, et see parameeter on kategooria sildi aktsepteerib ja saadab proper [malli](notification-hubs-templates-cross-platform-push-messages.md) teate.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

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



2. Värskendage ViewController.m **Teatise saatmine** toimingu, nagu on näidatud koodi järgneva. Nii, et see saadab abil igal sildil eraldi teatisi ja saada mitme kaudu.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Projekti taastada ja veenduge, et teil on tõrkeid koostamine.


##<a name="run-the-app-and-generate-notifications"></a>Käivitage rakendus ja luua teatised

1. Projekti koostamine ja käivitage rakendus nuppu Käivita. Valige mõni server Uudiste tellimine ja vajutage nuppu **Telli** . Peaksite nägema dialoog, mis näitab, teatised tellinud.

    ![][1]

    **Telli**valimisel app teisendab valitud kategooria sildid ja taotleb uue seadme registreerimine jaoks siltidega teatise keskuse kaudu.

2. Sisestage sõnum saata uudiseid ja seejärel klõpsake nuppu **Saada teatis** . Teise võimalusena käivitada .net-i konsooli rakenduse loomiseks teatised.

    ![][2]


3. Iga seadme tellinud katkestamine Uudised saavad saadetud ainult server Uudised teatised.



## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis me õppinud, kuidas edastada uudiseid kategooria järgi. Kaaluge lõpuleviimine ühte järgmised Õppematerjalid, mis täpsemalt teatis jaoturi stsenaariumide esile tõsta.

+ **[Teatis jaoturi abil lokaliseeritud uudiseid leviedastus]**

    Saate teada, kuidas laiendada Server Uudised rakenduse lubamiseks saatmise lokaliseeritud teatised.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Teatis jaoturi abil lokaliseeritud uudiseid leviedastus]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure'i klassikaline portaal]: https://manage.windowsazure.com
