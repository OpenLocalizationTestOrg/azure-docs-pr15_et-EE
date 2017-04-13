<properties
    pageTitle="Azure'i SDK saavutamiseks integratsiooni Mobile kaasamine iOS-i | Microsoft Azure'i"
    description="Uusimate värskenduste ja toimingute iOS-i SDK Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Kuidas jõuda integreerida kaasamine iOS

Teil tuleb toimige integreerimine kirjeldatud [integreerida kaasamine iOS-i dokumendiga kohta](mobile-engagement-ios-integrate-engagement.md) enne sellest juhendist.

Dokumentide jaoks on vaja XCode 8. Kui te tegelikult sõltuvad XCode 7 võib kasutada [iOS-i kaasamine SDK v3.2.4](https://aka.ms/r6oouh). On teadaolev viga selle eelmise versiooni iOS 10-seadmetes käitamise ajal: süsteemi teatisi ei ole actioned. Vea parandamiseks on teil rakendada taunitud API `application:didReceiveRemoteNotification:` rakenduse delegaadi järgmiselt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Mis tahes eelseisvad (väike) iOS-i versiooni täiendamine **me ei soovita seda lahendust** , kui seda käitumist muuta, kuna see iOS-i API on aegunud. Mida peaks vahetada XCode 8 nii kiiresti kui võimalik.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Lubage oma rakenduse saada vaikne tõuketeatised

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Juhised integreerimine

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>IOS-i projekti kaasamine saavutamiseks SDK embed

-   Lisage REACHi sdk Xcode projekti. Minge Xcode, **projekti \> lisamine projekti** ja valige soovitud `EngagementReach` kausta.

### <a name="modify-your-application-delegate"></a>Rakenduse volitatud muutmine

-   Rakendamine faili ülaosas importida mooduli kaasamine saavutamiseks.

        [...]
        #import "AEReachModule.h"

-   Meetodiga `applicationDidFinishLaunching:` või `application:didFinishLaunchingWithOptions:`, luua REACHi mooduli ja andke seda oma olemasoleva kaasamine lähtestamine joon:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Saate muuta pildi nimi, mida soovite oma teavitusikooni nimega **'icon.png'** string.
-   Kui soovite kasutada suvandit *Värskenda märgi väärtus* REACHi kampaaniat või kui soovite kasutada kohalikke tõuketeatised \<SaaS/REACHi API/turunduskampaania vorming/Native tõuketeatised\> kampaaniat, laske REACHi, mooduli haldamine märgi ikooni ise (see automaatselt tühjendage rakenduse märgi ja ka lähtestamine talletatud kaasamine iga kord, kui rakendus pole alustatud või foregrounded väärtus). Seda tehakse, lisades järgmine rida pärast REACHi mooduli lähtestamine:

        [reach setAutoBadgeEnabled:YES];

-   Kui soovite käsitleda REACHi andmete tõuketeatised, laske rakenduse volitatud vasta selle `AEReachDataPushDelegate` Protocol (protokoll). Lisage järgmine rida pärast REACHi mooduli lähtestamine.

        [reach setDataPushDelegate:self];

-   Seejärel saate rakendada meetodid `onDataPushStringReceived:` ja `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` sisse oma rakenduse volitatud esindaja:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategooria

Kategooria parameeter on valikuline, kui loote andmete Push turunduskampaania ja võimaldab teil andmeid filtreerida sunnib. See on kasulik, kui soovite push erinevaid, `Base64` andmed ja soovite tuvastada sõelumine need enne nende tüüp.

**Teie rakendus on nüüd valmis vastu võtta ja kuvada REACHi sisu!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Kuidas saada teadaanded ja küsitluste igal ajal

Kaasamine saate saata REACHi teatised teie lõppkasutajad igal ajal Apple Push teavitusteenuse abil.

Selle funktsiooni lubamiseks peate teie taotlus Apple'i lükketeatiste ettevalmistamine ja muuta oma rakenduse volitatud esindaja.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Teie taotlus Apple'i lükketeatiste ettevalmistamine

Järgige juhend: [Kuidas valmistada teie taotlus Apple Push teatised](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Vajalikud kliendi koodi lisamine

*Selles etapis rakenduse peaks olema registreeritud Apple push serdi frontend allikaid.*

Kui see pole veel teinud, peate rakenduse saada Tõuketeatiste registreerimiseks.

* Importimine on `User Notification` raames:

        #import <UserNotifications/UserNotifications.h>

* Lisage rakenduse käivitamisel (tavaliselt sisse `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Seejärel peate seadme luba tagastatud Apple'i serverid kaasamine pakkumiseks. Seda tehakse meetodit nimega `application:didRegisterForRemoteNotificationsWithDeviceToken:` sisse oma rakenduse volitatud esindaja:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Lõpetuseks tuleb rakenduse remote teate kaasamine SDK teavitada. Selleks, et kõne meetodit `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` sisse oma rakenduse volitatud esindaja:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Eeltoodud meetodi kasutusele iOS 7. Kui olete suunatud iOS-i < 7, veenduge, et rakendada meetodit `application:didReceiveRemoteNotification:` rakenduse volitatud esindaja ja kõne `applicationDidReceiveRemoteNotification` EngagementAgent null asemel sooritab kohta on `handler` argumenti:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Vaikimisi määrab kaasamine saavutamiseks on completionHandler. Kui soovite käsitsi vastamine on `handler` blokeerida koodi, siis saate liigu null on `handler` argumendi ja juhtelemendi lõpetamist ise blokeerida. Lugege teemat funktsiooni `UIBackgroundFetchResult` võimalike väärtuste loendi tüüp.


### <a name="full-example"></a>Täielik näide

Siin on täielik näide integreerimine.

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Kui teil on oma UNUserNotificationCenterDelegate rakendamine

SDK on ka oma UNUserNotificationCenterDelegate protokolli rakendamine. Seda kasutatakse SDK jälgida elutsükli kaasamine teatised seadmetes, kus töötab iOS, 10 või uuemat versiooni. Kui SDK tuvastab volitatud seda ei kasuta oma rakendamist kuna ei saa olla ainult üks UNUserNotificationCenter volitatud esindaja taotluse kohta. See tähendab, et teil on oma volitatud esindaja kaasamine loogika lisamine.

On selleks kaks võimalust.

Lihtsalt saates volitatud kõned SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Või pärimist soovitud `AEUserNotificationHandler` klassi

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Saate määrata, kas teatis on pärit kaasamine või sooritab pole selle `userInfo` sõnastiku agent `isEngagementPushPayload:` klassi meetod.

##<a name="how-to-customize-campaigns"></a>Kuidas kohandada kampaaniat

### <a name="notifications"></a>Teatised

On kahte tüüpi teatised: süsteemi ja rakenduste teatised.

Süsteemi teatisi iOS-i hallatakse ja ei saa kohandada.

Vaate, mis lisatakse dünaamiliselt praeguse rakenduse akna tehakse-rakenduste teatised. Seda nimetatakse teatis ülekatet. Teatis ülekatete on suur kiiresti integreerimine, sest need ei vaja, saate muuta oma rakenduse mis tahes vaates.

#### <a name="layout"></a>Paigutus

Teie-rakenduste teatised ilme muutmiseks saate muuta lihtsalt faili `AENotificationView.xib` teie vajadustele, kui hoiate sildi väärtusi ja olemasolevaid alamvaated tüüpi.

Vaikimisi on esitatud-rakenduste teatised ekraani allosas. Kui eelistate neid ekraani ülaosas kuvada, redigeerida on esitatud `AENotificationView.xib` ja muutke soovitud `AutoSizing` atribuut põhikuval nii, et see saab hoida oma superview ülaosas.

#### <a name="categories"></a>Kategooriad

Kui esitatud paigutuse muutmiseks saate muuta kõigi teie teatised ilmet. Kategooriate abil saate määratleda erinevate suunatud ilmed (võib-olla käitumise) teatiste saamiseks. Kui loote REACHi turunduskampaania saab määrata kategooria. Pidage meeles, et kategooriad ka võimaldavad teil kohandada teadaanded ja küsitlused, mis on kirjeldatud hiljem selles dokumendis.

Kategooria sündmuseohjuri oma teatiste registreerida, peate lisama kõne, kui REACHi moodul on lähtestatud.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`peab olema näiteks objekti, mis vastab protokolli `AENotifier`.

Saate rakendada protokolli meetodite ise või saate valida reimplement klassi `AEDefaultNotifier` mis kaduma juba töö.

Näiteks, kui soovite uuesti teatise vaate konkreetse kategooria, saate järgige selles näites:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Lihtne näide selle kategooria endale fail nimega `MyNotificationView.xib` oma rakenduse kogumis. Kui meetod ei ole võimalik leida vastava `.xib`, teate ei kuvata ja allikaid väljund sõnumi konsooli.

Esitatud nib faili tuleks järgida järgmisi reegleid:

-   See peaks sisaldama ainult ühes vaates.
-   Alamvaated peaks olema sama tüüpi kui need sees esitatud nib fail nimega`AENotificationView.xib`
-   Alamvaated peaks olema sama sildid, kui need on esitatud sees nib fail nimega`AENotificationView.xib`

> [AZURE.TIP] Lihtsalt kopeerige esitatud nib faili nimega `AENotificationView.xib`, ja seal Alustame. Kuid olge ettevaatlik, vaate sees nib fail on seostatud ainekursuse `AENotificationView`. See tund uuesti meetodit `layoutSubViews` teisaldamine ja suuruse muutmine oma alamvaated kontekstist olenevalt. Soovite asendada, kus on `UIView` või kohandatud vaate klassi.

Kui teil on vaja põhjalikult kohandamine teie teatised (kui soovite näiteks laadimiseks vaate otse kood), on soovitatav Heitke pilk andmeallika esitatud koodi ja klassi dokumendid `Protocol ReferencesDefaultNotifier` ja `AENotifier`.

Pange tähele, et saate teatise esitaja jaoks mitu kategooriat.

Samuti saate uuesti vaikimisi taotleja umbes järgmine:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Teatis töötlemine

Vaikimisi kategooria kasutamisel mõned elutsükli meetodid kutsunud selle `AEReachContent` objekti aruande statistika ja värskendada turunduskampaania olek:

-   Teate kuvamisel rakenduses, kuvatakse `displayNotification` meetodit nimetatakse (mis aruannete statistika), `AEReachModule` kui `handleNotification:` tagastab `YES`.
-   Kui teate jätta, kuvatakse `exitNotification` meetodit nimetatakse, statistiline teatatud ja nüüd saab töödelda järgmise kampaaniat.
-   Teate klõpsamisel `actionNotification` on helistanud, on teatatud statistiline ja seotud toiming.

Kui teie rakendamine `AENotifier` mille liiklus möödub vaikekäitumise, teil helistada ise elutsükli järgmised võimalused. Järgmised näited illustreerivad mõnel juhul, kui vaikekäitumine on mööduda:

-   Te ei laiendada `AEDefaultNotifier`, nt saate rakendada kategooria käsitsemise algusest peale ise luua.
-   Saate overrode `prepareNotificationView:forContent:`, veenduge, et kaart vähemalt `onNotificationActioned` või `onNotificationExited` ühele oma U.I juhtelemendid.

> [AZURE.WARNING] Kui `handleNotification:` viskab erandi sisu kustutatakse ja `drop` on kutsutud, see on aruandes statistika ja nüüd saab töödelda järgmise kampaaniat.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Kaasa teatise osana olemasolev vaade

Ülekatete on suurepärane kiire integreerimiseks, kuid võib mõnikord pole mugav või võib olla soovimatuid.

Kui te ei ole rahul ülekatet süsteemi osa oma vaateid, saate seda kohandada neid vaateid.

Saate otsustada, kas kaasata meie teatis paigutus oma olemasoleva vaated. Selleks on kaks rakendamist laade.

1.  Lisage teatis vaadet kasutajaliidese avaldisekoosturi abil

    -   Avatud *Builder kasutajaliides*
    -   Viige 320 x 60 (või kui teil on iPadi 768 x 60) `UIView` , kus soovite kuvada teatis
    -   Seatakse see vaade sildi väärtus: **36822491**

2.  Teatise vaate programmiliselt lisada. Kui teie arvates on lähtestatud, lisage järgmine kood:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Makro leiate `AEDefaultNotifier.h`.

> [AZURE.NOTE] Vaikimisi taotleja automaatselt tuvastab, et teatis paigutus selles vaates sisaldab ja ei lisa ülekatte seda.

### <a name="announcements-and-polls"></a>Teadaanded ja küsitluste

#### <a name="layouts"></a>Paigutused

Saate muuta failid `AEDefaultAnnouncementView.xib` ja `AEDefaultPollView.xib` kui hoiate sildi väärtusi ja olemasolevaid alamvaated tüüpi.

#### <a name="categories"></a>Kategooriad

##### <a name="alternate-layouts"></a>Alternatiivsete paigutused

Teatised, nt saab olema alternatiivse paigutuste teadaanded ja küsitluste soovitud turunduskampaania kategooria.

Teate kategooria loomiseks peab laiendamine **AEAnnouncementViewController** ja registreerida pärast mooduli REACHi on lähtestatud.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Iga kord, kui kasutaja on klõpsake teate kategooriaga teade "minu\_kategooria", registreeritud vaate kontrolleril (sel juhul `MyCustomAnnouncementViewController`), helistades meetodit lähtestatakse `initWithAnnouncement:` ja vaate praeguse rakenduse akna lisatakse.

Teie rakendamiseks soovitud `AEAnnouncementViewController` on teil lugeda klassi `announcement` lähtestada oma alamvaated. Kaaluge järgmises näites, kus kaks märgib on lähtestatud, kasutades `title` ja `body` atribuudid on `AEReachAnnouncement` klassi:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Kui te ei soovi oma vaadete laadimiseks ise, kuid soovite lihtsalt teadaanne vaade vaikepaigutus uuesti kasutada, saate lihtsalt kohandatud vaate kontrolleril laiendab esitatud klassi `AEDefaultAnnouncementViewController`. Sel juhul dubleerimine nib fail `AEDefaultAnnouncementView.xib` ja nimetage see ümber nii, et see saab laadida oma kohandatud vaate kontrolleril (jaoks mõne selle domeenikontrolleri nimega `CustomAnnouncementViewController`, peaksite helistama nib failis `CustomAnnouncementView.xib`).

Vaikimisi kategooria teadaannete asendamiseks registreerida kohandatud vaate kontrolleril määratletud kategooria `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Küsitluste saab kohandada samal viisil:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Sel ajal, on esitatud `MyCustomPollViewController` peab laiendamine `AEPollViewController`. Saate valida laiendatakse vaikimisi domeenikontrolleri või: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Ärge unustage kõne ühte `action` (`submitAnswers:` kohandatud küsitluse vaade kontrollerid) või `exit` meetod enne vaate kontrolleril jätta. Muul juhul statistika ei saadeta (st pole analytics kampaaniat kohta) ja veel olulisem järgmise kampaaniat ei teavitata enne rakenduse protsessi taaskäivitamist.

##### <a name="implementation-example"></a>Rakendamise näide

Selle rakendamiseks kohandatud teadaanne vaade on laaditud välise xib failist.

Nagu täpsemalt teatis kohandamine, on soovitatav vaadata lähtekoodi standard rakendamist.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
