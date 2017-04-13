<properties
    pageTitle="Alustamine Azure Mobile kaasamine iOS-i eesmärk c | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Kasutusanalüüsi ja push teatised iOS-i rakendused."
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
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Azure'i Mobile kaasamine iOS-i rakenduste eesmärk c kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas näidatakse, kuidas kasutada Azure Mobile kaasamine mõista teie rakenduse kasutamise ja saata Tõuketeatiste segmenditud kasutajad iOS-i rakendus.
Selles õpetuses loote tühja iOS-i rakenduse, mis kogub lähteandmete ja võtab vastu Tõuketeatiste Apple Push teatise süsteemi (APNs-i) kasutades.

Selle õpetuse kasutamiseks on vaja järgmist:

+ XCode 8, mille saate installida MAC App Store'ist
+ [SDK Mobile kaasamine iOS-i]

Selle õpetuse lõpuleviimine on nõutav kõik Mobile kaasamine õpetused iOS-i rakendused.

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Mobile kaasamine oma iOS-i rakenduse häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse tutvustab "lihtsa integratsioon", mis on nõutav jõudlusandmeid koguda ja tõuketeatised teatise saatmine minimaalsete määramine. Täielik integratsioon dokumentatsiooni kohta leiate [Mobile kaasamine iOS-i SDK integreerimine](mobile-engagement-ios-sdk-overview.md)

Loome lihtsa rakenduse XCode integreerimine näitamiseks.

###<a name="create-a-new-ios-project"></a>IOS-i uue projekti loomine

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

1. Laadige alla [SDK Mobile kaasamine iOS-i].
2. Ekstraktida soovitud. tar.gz fail teie arvuti kausta.
3. Paremklõpsake projekti ja seejärel valige **Lisa failid**.

    ![][1]

4. Liikuge kausta, kuhu ekstraktitud Tarkvaraarenduskomplektist, valige soovitud `EngagementSDK` kaust, seejärel vajutage klahvi **OK**.

    ![][2]

5. Avage vahekaart **Koostada etapist** ja menüüs **Link kahendarvu koos teekide** , lisage soovitud raamistiku nagu allpool näidatud:

    ![][3]

6. Minge tagasi oma rakenduse lehel **Ühenduse teave** Azure portaali ja kopeerige ühendusstring.

    ![][4]

7. Lisage järgmine rida koodi **AppDelegate.m** failis.

        #import "EngagementAgent.h"

8. Ühendusstringi kleepida selle `didFinishLaunchingWithOptions` volitatud esindaja.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`on valikuline lause, mis võimaldab SDK logid teie küsimuste jaoks. 

##<a id="monitor"></a>Reaalajas jälgimise lubamine

Andmete saatmine ja tagada, et kasutajad on aktiivne käivitamiseks peate saatma Mobile kaasamine taustväärtus vähemalt ühe ekraanikuva (tegevus).

1. Avage fail **ViewController.h** ja **EngagementViewController.h**importida.

    `# import "EngagementViewController.h"`

2. Nüüd asendada selle samasse klassi **ViewController** liides, `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil suhelda oma kasutajate ja jõuda tõuketeatisi ja rakenduse sõnumside kampaaniat kontekstis. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmistes jaotistes häälestada rakenduse neid vastu võtta.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Lubage oma rakenduse saada vaikne tõuketeatised

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>REACHi teegi lisamine projekti

1. Paremklõpsake oma projekti.
2. Valige **Lisa faili**.
3. Liikuge kausta, kuhu ekstraktitud SDK.
4. Valige soovitud `EngagementReach` kausta.
5. Klõpsake nuppu **Lisa**.

### <a name="modify-your-application-delegate"></a>Rakenduse volitatud muutmine

1. Tagasi **AppDeletegate.m** faili importida mooduli kaasamine saavutamiseks.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Sees on `application:didFinishLaunchingWithOptions` meetod REACHi mooduli loomine ja laadige see oma olemasoleva kaasamine lähtestamine joon:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Rakenduse saada APNS tõuketeatised lubamine

1. Lisage järgmine rida on `application:didFinishLaunchingWithOptions` meetod:

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

2. Lisage soovitud `application:didRegisterForRemoteNotificationsWithDeviceToken` meetod järgmiselt:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Lisage soovitud `didFailToRegisterForRemoteNotificationsWithError` meetod järgmiselt:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Lisage soovitud `didReceiveRemoteNotification:fetchCompletionHandler` meetod järgmiselt:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[SDK Mobile kaasamine iOS-i]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

