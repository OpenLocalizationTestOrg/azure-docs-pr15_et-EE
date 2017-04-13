<properties
    pageTitle="Tõuketeatised iOS-i mobiilirakenduste Azure'i rakenduse lisamine"
    description="Saate teada, kuidas Azure'i Mobile'i rakenduste abil saate saata oma iOS-i rakenduse tõuketeatisi."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Tõuketeatised lisamine oma iOS-i rakendus

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Ülevaade
Selles õpetuses lisate Tõuketeatiste projekti [iOS-i Kiire alustamine] , et tõuketeatised teatis saadetakse seade iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateavet leiate [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

Funktsiooni [iOS-i simulaatorit ei toeta tõuketeatisi](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Peate seadme füüsilised iOS-i ja mõne [Apple'i arendaja liikmelisus](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Teate jaoturi konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Rakenduse Tõuketeatiste registreerimine

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure'i saata Tõuketeatiste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Värskendage kirjutamata Tõuketeatiste saatmine

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Tõuketeatiste lisamine rakendusse

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Tõuketeatiste test

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Lisateave

* Mallide abil paindlikkust platvormidel sunnib ja lokaliseeritud sunnib saata. [Kuidas kliendi teek Azure'i mobiilirakenduste kasutamine iOS-i](app-service-mobile-ios-how-to-use-client-library.md#templates) näidatakse, kuidas registreerida mallid.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS-i Lühijuhend]: app-service-mobile-ios-get-started.md
