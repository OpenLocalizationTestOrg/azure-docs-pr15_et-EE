<properties
    pageTitle="Azure'i mobiilsideseadmete jaoks Xamarin.iOS kaasamine kasutamise alustamine"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Analytics ja tõuketeatised Xamarin.iOS rakendused."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Azure'i mobiilsideseadmete kaasamine Xamarin.iOS rakenduste kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas näidatakse, kuidas kasutada Azure Mobile kaasamine mõista teie rakenduse kasutamise ja saata Tõuketeatiste segmenditud kasutajatele Xamarin.iOS rakenduses.
Selles õpetuses loote tühja Xamarin.iOS rakendus, mis kogub lähteandmete ja võtab vastu Tõuketeatiste Apple Push teatise süsteemi (APNs-i) kasutades.

Selle õpetuse kasutamiseks on vaja järgmist:

+ [Xamarin Studio](http://xamarin.com/studio). Visual Studio saate kasutada ka koos Xamarin, kuid selles õpetuses kasutab Xamarin Studio. Installimise juhiste saamiseks vaadake teemat [Setup ja installige Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobiilse kaasamine Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Mobile kaasamine oma iOS-i rakenduse häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse tutvustab "lihtsa ühendamine", mis on minimaalne vajalik andmete kogumise ja tõuketeatised teatise saatmine.

Loome lihtsa rakenduse Xamarin näidata integreerimine:

###<a name="create-a-new-xamarinios-project"></a>Xamarin.iOS uue projekti loomine

1. Käivitage Xamarin Studio. Avage **fail** -> **uue** -> **lahendus** 

    ![][1]

2. Valige **Ühe vaate rakendus**, veenduge, et valitud keeles on **C#** ja seejärel klõpsake nuppu **edasi**.

    ![][2]

3. Sisestage **Rakenduse nimi** ja **Ettevõtte ID** ja seejärel klõpsake nuppu **edasi**. 

    ![][3]

    > [AZURE.IMPORTANT] Veenduge, et publishing profiili lõpuks abil saate juurutada oma iOS-i rakendus kasutab rakenduse ID, mis vastab täpselt identifikaatoriga kogumi on siin. 

4. Värskendage **Projekti nime**, **Lahenduse nime** ja **asukoha** vajadusel ja klõpsake nuppu **Loo**.

    ![][4]
 
Xamarin Studio loob demo rakenduse, kus oleme integreerida Mobile allikaid. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Ühenduse loomine rakenduse Mobile kaasamine taustväärtus

1. Paremklõpsake **pakettide** lahenduse Windowsi kausta ja valige **Lisa pakettide**

    ![][5]

2. **Microsoft Azure'i Mobile kaasamine Xamarin SDK** otsida ja lisada selle oma lahendus.  

    ![][6]
   
3. Avage **AppDelegate.cs** ja lisage järgmine kasutamine lause:

        using Microsoft.Azure.Engagement.Xamarin;

4. Lisage **FinishedLaunching** meetod lähtestamine Mobile kaasamine taustväärtus ühenduse loomiseks järgmist. Veenduge, et lisada oma **ConnectionString**. Järgmine kood kasutab ka fiktiivsete **NotificationIcon** , mis on lisatud Mobile kaasamine SDK, mida soovite asendada. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Reaalajas jälgimise lubamine

Andmete saatmine ja tagada, et kasutajad on aktiivne käivitamiseks peate saatma vähemalt ühe ekraanikuva Mobile kaasamine taustväärtus.

1. Avage **ViewController.cs** ja lisage järgmine kasutamine lause:

        using Microsoft.Azure.Engagement.Xamarin;

2. Asendage ainekursust, millest `ViewController` pärib `UIViewController` et `EngagementViewController`. 

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil suhelda oma kasutajate ja jõuda tõuketeatised ja rakenduse sõnumside kampaaniat kontekstis. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmistes jaotistes häälestada rakenduse neid vastu võtta.

### <a name="modify-your-application-delegate"></a>Rakenduse volitatud muutmine

1. Avage **AppDelegate.cs** ja lisage järgmine kasutamine lause:

        using System; 

2. Nüüd sees on `FinishedLaunching` meetod, lisage järgmine registreerimiseks tõuketeatised sõnumeid pärast`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Lõpetuseks - värskendamine või lisamine järgmistest viisidest:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Lahendus **Info.plist** failis veenduge, et **Komplekt identifikaator** vastab ettevalmistamise profiili sisse Apple'i Arenduskeskus on **Rakenduse ID** -ga. 

    ![][7]

5. **Info.plist** sama faili veenduge, et teie märgitud **Luba taustal režiimi** ja **Remote teatised**. 

    ![][8]

6. Käivitage rakendus teil on avaldamise profiiliga seotud seadmes. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
