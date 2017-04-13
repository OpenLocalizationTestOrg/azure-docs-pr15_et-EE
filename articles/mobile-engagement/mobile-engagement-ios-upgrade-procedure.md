<properties
    pageTitle="Azure'i Mobile kaasamine iOS SDK täiendamine protseduuri | Microsoft Azure'i"
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

#<a name="upgrade-procedures"></a>Toimingute täiendamine

Kui teil juba on integreeritud vanemat versiooni kaasamine rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Iga uue versiooni SDK peate esmalt asendamine (eemaldamine ja uuesti importida xcode) EngagementSDK ja EngagementReach kaustad.

##<a name="from-300-to-400"></a>Kui soovite 4.0.0 3.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 on kohustuslik alates SDK 4.0.0 versiooni.

> [AZURE.NOTE] Kui te tegelikult sõltuvad XCode 7 võib kasutada [iOS-i kaasamine SDK v3.2.4](https://aka.ms/r6oouh). On teadaolev viga REACHi moodul eelmise versiooni iOS 10-seadmetes käitamise ajal: süsteemi teatisi ei ole actioned. Vea parandamiseks on teil rakendada taunitud API `application:didReceiveRemoteNotification:` rakenduse delegaadi järgmiselt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Mis tahes eelseisvad (väike) iOS-i versiooni täiendamine **me ei soovita seda lahendust** , kui seda käitumist muuta, kuna see iOS-i API on aegunud. Mida peaks vahetada XCode 8 nii kiiresti kui võimalik.

### <a name="usernotifications-framework"></a>UserNotifications raames
Peate lisama selle `UserNotifications` framework oma koostada etapist.

project Explorer, teie projekti paani avamine ja valige õige eesmärk. Seejärel avage vahekaart **"Koostada etapist"** ja **"Link kahendarvu koos teekide"** menüüs Lisa framework `UserNotifications.framework` -link nimega seadmine`Optional`

### <a name="application-push-capability"></a>Rakenduse tõuketeatised võimalus
XCode 8 võib lähtestada oma rakenduse push võimalus, palun kontrolli selle selle `capability` teie valitud sihtrühma vahekaarti.

### <a name="add-the-new-ios-10-notification-registration-code"></a>IOS-i 10 teatise registreerimise koodi lisamine
Rakenduse teatised registreerimiseks vanemate koodilõigu endiselt töötab, kuid kasutab API töötab iOS-i 10.

Importimine on `User Notification` raames:

        #import <UserNotifications/UserNotifications.h> 

Klõpsake rakenduse volitatud `application:didFinishLaunchingWithOptions` meetod asenda:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

alusel:

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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Kui olete juba oma UNUserNotificationCenterDelegate rakendamine

SDK on ka oma UNUserNotificationCenterDelegate protokolli rakendamine. Seda kasutatakse SDK jälgida elutsükli kaasamine teatised seadmetes, kus töötab iOS 10 või uuemat versiooni. Kui SDK tuvastab volitatud seda ei kasuta oma rakendamist kuna ei saa olla ainult üks UNUserNotificationCenter volitatud esindaja taotluse kohta. See tähendab, et teil on oma volitatud esindaja kaasamine loogika lisamine.

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

##<a name="from-200-to-300"></a>Et 3.0.0 2.0.0 kaudu
IOS-i tugi lähevad 4.X. Alates selle versiooni juurutamise target rakenduse peab olema vähemalt iOS 6.

Kui kasutate oma rakenduse REACHi, peate lisama `remote-notification` väärtuseks on `UIBackgroundModes` massiiv Info.plist faili remote teatiste saamiseks.

Meetodit `application:didReceiveRemoteNotification:` tuleb asendada `application:didReceiveRemoteNotification:fetchCompletionHandler:` rakenduse volitatud sisse.

"AEPushDelegate.h" on aegunud kasutajaliidese- ja teil on vaja kogu viiteid eemaldada. See hõlmab eemaldamise `[[EngagementAgent shared] setPushDelegate:self]` ja volitatud esindaja meetodite volitatud rakenduse kaudu:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Et 2.0.0 1.16.0 kaudu
Järgmine kirjeldatakse, kuidas migreerida mõne SDK integreerimine Capptain teenus Capptain SAS tootja Azure Mobile kaasamine rakendusse.
Kui migreerite varasemast versioonist, pöörduge veebisaidi Capptain siirduda esmalt 1.16 seejärel rakendada järgmist.

>[AZURE.IMPORTANT] Capptain ja Mobile kaasamine pole sama teenuste ja allpool toodud juhiseid tõstetakse esile ainult kuidas saate migreerida kliendi rakendus. Migreerimine SDK rakenduse kuvatakse siirata andmete Capptain serverid serveriga Mobile kaasamine

### <a name="agent"></a>Agent

Meetodit `registerApp:` on asendatud uue meetodi `init:`. Rakenduse volitatud tuleb värskendada vastavalt ja ühendusstringi.

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

SmartAd jälitus on eemaldatud lihtsalt peate eemaldama kõik eksemplarid SDK `AETrackModule` klassi

### <a name="class-name-changes"></a>Klassi nime muudatused

Osana selle rebranding on paar klassifail/nimed, mida soovite muuta.

Kõik tunnid eesliide "LP" on ümber nimetatud eesliitega "Au".

Näide:

-   `CPModule.h`Kui soovite ümber `AEModule.h`.

Kõik tunnid eesliide "Capptain" ümber eesliitega "Kaasamine".

Näited:

-   Klassi `CapptainAgent` on uueks nimeks `EngagementAgent`.
-   Klassi `CapptainTableViewController` on uueks nimeks `EngagementTableViewController`.
-   Klassi `CapptainUtils` on uueks nimeks `EngagementUtils`.
-   Klassi `CapptainViewController` on uueks nimeks `EngagementViewController`.
