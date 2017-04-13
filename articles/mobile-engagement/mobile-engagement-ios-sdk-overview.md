<properties
    pageTitle="Azure'i Mobile kaasamine iOS SDK ülevaade | Microsoft Azure'i"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS-i SDK Azure Mobile kaasamine

Kõigi üksikasjade vaatamiseks kohta, kuidas ühendada Azure Mobile kaasamine rakendus iOS alustage siit. Kui soovite seda proovida esmalt, veenduge, et saate teha meie [õpetuse 15 minutit](mobile-engagement-ios-get-started.md).

Klõpsake soovitud [SDK sisu](mobile-engagement-ios-sdk-content.md) kuvamiseks

##<a name="integration-procedures"></a>Toimingute integreerimine
1. Alustage siit: [Kuidas integreerida oma iOS-i rakenduses Mobile kaasamine](mobile-engagement-ios-integrate-engagement.md)

2. Teatiste: [Kuidas integreerida iOS-i rakenduse REACHi (teatised)](mobile-engagement-ios-integrate-engagement-reach.md)

3. Lepingu täitmise sildistamine: [Täpsemalt Mobile kaasamine sildistamine API iOS-i rakenduse kasutamine](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Väljalaskemärkmed

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Fikseeritud teatis ei actioned iOS 10-seadmetes.
-   Taunimine XCode 7.

Varasema versiooni kohta leiate teemast [täielik väljalaskemärkmed](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Toimingute täiendamine

Kui teil juba on integreeritud vanemat versiooni kaasamine rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Teil võib olla mitu toimingute jälgimiseks, kui te vastamata mitut versiooni SDK teemast [Täiendamine toimingute](mobile-engagement-ios-upgrade-procedure.md)lõpule viidud.

Iga uue versiooni SDK peate esmalt asendamine (eemaldamine ja uuesti importida xcode) EngagementSDK ja EngagementReach kaustad.

###<a name="from-300-to-400"></a>Kui soovite 4.0.0 3.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 on kohustuslik alates SDK 4.0.0 versiooni.

> [AZURE.NOTE] Kui te tegelikult sõltuvad XCode 7 võib kasutada [iOS-i kaasamine SDK v3.2.4](https://aka.ms/r6oouh). On teadaolev viga REACHi moodul eelmise versiooni iOS 10-seadmetes käitamise ajal: süsteemi teatisi ei ole actioned. Vea parandamiseks on teil rakendada taunitud API `application:didReceiveRemoteNotification:` rakenduse delegaadi järgmiselt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Mis tahes eelseisvad (väike) iOS-i versiooni täiendamine **me ei soovita seda lahendust** , kui seda käitumist muuta, kuna see iOS-i API on aegunud. Mida peaks vahetada XCode 8 nii kiiresti kui võimalik.

#### <a name="usernotifications-framework"></a>UserNotifications raames
Peate lisama selle `UserNotifications` framework oma koostada etapist.

project Explorer, teie projekti paani avamine ja valige õige eesmärk. Seejärel avage vahekaart **"Koostada etapist"** ja **"Link kahendarvu koos teekide"** menüüs Lisa framework `UserNotifications.framework` -link nimega seadmine`Optional`

#### <a name="application-push-capability"></a>Rakenduse tõuketeatised võimalus
XCode 8 võib lähtestada oma rakenduse push võimalus, palun kontrolli selle selle `capability` teie valitud sihtrühma vahekaarti.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>IOS-i 10 teatise registreerimise koodi lisamine
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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Kui olete juba oma UNUserNotificationCenterDelegate rakendamine

SDK on oma UNUserNotificationCenterDelegate protokolli rakendamine. Seda kasutatakse SDK jälgida elutsükli kaasamine teatised seadmetes, kus töötab iOS, 10 või uuemat versiooni. Kui SDK tuvastab volitatud seda ei kasuta oma rakendamist kuna ei saa olla ainult üks UNUserNotificationCenter volitatud esindaja taotluse kohta. See tähendab, et teil on oma volitatud esindaja kaasamine loogika lisamine.

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