<properties
    pageTitle="Tõuketeatiste lisamine rakenduse Xamarin.iOS Azure rakenduse teenusega"
    description="Saate teada, kuidas Azure'i rakendust Service abil saate saata oma Xamarin.iOS rakenduse tõuketeatised"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Tõuketeatiste Xamarin.iOS rakenduse lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Ülevaade

Selles õpetuses lisate Tõuketeatiste projekti [Xamarin.iOS Kiire alustamine](app-service-mobile-xamarin-ios-get-started.md) , et tõuketeatised teatis saadetakse seade iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateavet leiate [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Eeltingimused

* Täitke [Xamarin.iOS Kiirjuhend](app-service-mobile-xamarin-ios-get-started.md) õpetuse.

* Füüsilise iOS-seadmes. Tõuketeatiste ei toeta iOS-i simulaatorit.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Tõuketeatiste Apple'i arendaja portaalis jaoks rakenduse registreerimine

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Oma Mobile'i rakendus saata Tõuketeatiste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Värskenduse saatmiseks tõuketeatisi projekti server

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Projekti Xamarin.iOS konfigureerimine

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Tõuketeatiste lisamiseks rakenduse

1. **QSTodoService**, lisage järgmised atribuudi nii, et **AppDelegate** saate hankida mobiiltelefoni kliendi:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Lisage järgmine `using` lause **AppDelegate.cs** fail üles.

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. Klõpsake **AppDelegate**, alistada **FinishedLaunching** sündmuse:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Sama faili alistada **RegisteredForRemoteNotifications** sündmus. Järgmine kood olete registreerimisel lihtne Mall teatise, mis saadetakse kõigi toetatud platvormid kogu server.

    Mallide põhjal, mille teatise jaoturi kohta leiate lisateavet teemast [Mallid](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Seejärel alistada **DidReceivedRemoteNotification** sündmuse:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Rakenduse on värskendatud Tõuketeatiste toetavad.

## <a name="test"></a>Rakenduse testi tõuketeatised

1. Projekti koostamine ja käivitage rakendus iOS-i toega seadme nuppu **Käivita** ja seejärel klõpsake nuppu **OK** , et aktsepteerida tõuketeatisi.

    > [AZURE.NOTE] Konkreetselt peab aktsepteerida tõuketeatisi, rakenduse kaudu. Taotluse ilmneb ainult esimene kord, kui rakendus töötab.

2. Tippige tööülesande rakendus, ja klõpsake plussmärki (**+**) ikoon.

3. Veenduge, et teatis on saanud, siis klõpsake nuppu **OK** , et teatise eiramine.

4. Korrake juhiseid 2 ja kohe rakenduse sulgeda, siis veenduge, et teatis kuvatakse.

Selles õpetuses on lõpule.

<!-- Images. -->

<!-- URLs. -->



