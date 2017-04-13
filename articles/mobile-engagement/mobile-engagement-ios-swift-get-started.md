<properties
    pageTitle="IOS-i sisse Kiire alustamine Azure Mobile kaasamine | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Analytics ja tõuketeatised iOS-i rakendusi."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>IOS-i rakenduste Kiire alustamine Azure Mobile kaasamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas näidatakse, kuidas kasutada Azure Mobile kaasamine mõista teie rakenduse kasutamise ja saata Tõuketeatiste segmenditud saajatele iOS-i rakendus.
Selles õpetuses loote tühja iOS-i rakendus, mis kogub lähteandmete ja võtab vastu Tõuketeatiste Apple Push teatise süsteemi (APNs-i) kasutades.

Selle õpetuse kasutamiseks on vaja järgmist:

+ XCode 8, mille saate installida MAC App Store'ist
+ [SDK Mobile kaasamine iOS-i]
+ Tõuketeatised teatis sert (.p12), mida saab teie Apple'i Arenduskeskus

> [AZURE.NOTE] Selle õpetuse kasutab kiire versiooni 3.0. 

Selle õpetuse lõpuleviimine on nõutav kõik Mobile kaasamine õpetused iOS-i rakendused.

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Mobile kaasamine oma iOS-i rakenduse häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse esitab "lihtsa integratsioon", mis on nõutav andmete kogumine ja tõuketeatised teatise saatmine minimaalsete määramine. Täielik integratsioon dokumentatsiooni kohta leiate [Mobile kaasamine iOS-i SDK integreerimine](mobile-engagement-ios-sdk-overview.md)

Loome lihtsa rakenduse XCode näidata integreerimine:

###<a name="create-a-new-ios-project"></a>IOS-i uue projekti loomine

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Ühenduse loomine rakenduse Mobile kaasamine taustväärtus

1. [SDK Mobile kaasamine iOS-i] allalaadimine
2. Ekstraktida soovitud. tar.gz fail teie arvuti kausta
3. Projekti paremklõps ja valige "Faile lisada..."

    ![][1]

4. Liikuge kausta, kuhu ekstraktitud SDK ja valige soovitud `EngagementSDK` kausta ja seejärel vajutage nuppu OK.

    ![][2]

5. Avatud on `Build Phases` menüüd ja selle `Link Binary With Libraries` menüü lisamine soovitud raamistiku, nagu allpool näidatud:

    ![][3]

8. Looge Sotsiaalplaanis päis saama kasutada – SDK eesmärk C API-d, valides Fail > uus > fail > iOS-i > allikas > päise faili.

    ![][4]

9. Jätke Mobile kaasamine eesmärk C-kood oma kiire koodi, lisage järgmised impordi nädalavahetusele päise faili redigeerimine:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Klõpsake jaotises sätted koostada veenduge, et eesmärk-C teisendamine päise koostamine kiire koostaja - koodi loomine jaotises säte on see päis tee. Siin on näide tee: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (olenevalt tee)**

    ![][6]

11. Minge tagasi oma rakenduse lehel *Ühenduse teave* Azure portaali ja kopeerige ühendusstring

    ![][5]

12. Ühendusstringi kleepida selle `didFinishLaunchingWithOptions` volitatud esindaja

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Reaalajas jälgimise lubamine

Andmete saatmine ja tagada, et kasutajad on aktiivne käivitamiseks peate saatma Mobile kaasamine taustväärtus vähemalt ühe ekraanikuva (tegevus).

1. Avage fail **ViewController.swift** ja asendada base klassi **ViewController** **EngagementViewController**olema:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil suhelda ja jõuda kasutajate tõuketeatised ja-rakenduse sõnumside kampaaniat kontekstis. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmistes jaotistes on häälestamine rakenduse neid vastu võtta.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Lubage oma rakenduse saada vaikne tõuketeatised

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>REACHi teegi lisamine projekti

1. Paremklõpsake oma projekti
2. Valige`Add file to ...`
3. Liikuge kausta, kuhu ekstraktitud SDK
4. Valige soovitud `EngagementReach` kaust
5. Klõpsake nuppu Lisa
6. Jätke Mobile kaasamine C-eesmärgi saavutamiseks päised ja lisada järgmised impordi nädalavahetusele päise faili redigeerimine:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Rakenduse volitatud muutmine

1. Sees on `didFinishLaunchingWithOptions` – REACHi mooduli loomine ja laadige see oma olemasoleva kaasamine lähtestamine joon:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Lubage oma rakenduse APNS tõuketeatised vastuvõtmiseks
1. Lisage järgmine rida on `didFinishLaunchingWithOptions` meetod:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Lisage soovitud `didRegisterForRemoteNotificationsWithDeviceToken` meetod järgmiselt:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Lisage soovitud `didReceiveRemoteNotification:fetchCompletionHandler:` meetod järgmiselt:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[SDK Mobile kaasamine iOS-i]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
