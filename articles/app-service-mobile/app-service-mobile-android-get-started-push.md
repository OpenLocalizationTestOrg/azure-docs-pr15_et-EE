<properties
    pageTitle="Tõuketeatiste Azure mobiilirakendused Androidi rakenduse lisamine"
    description="Saate teada, kuidas Azure'i mobiilirakenduste saatmine Tõuketeatiste Androidi rakenduse abil."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Tõuketeatised Androidi rakenduse lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Ülevaade
Selles õpetuses lisate Tõuketeatiste [Androidi Lühijuhend] projekt, et tõuketeatised teatis saadetakse seade iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateabe saamiseks vt [taustväärtus .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Eeltingimused

Teil on vaja järgmist:

* Sõltuvalt oma projekti taustväärtus on IDE:

    * Kui see rakendus on Node.js kirjutamata [Androidi Studio](https://developer.android.com/sdk/index.html) .

    * [Visual Studio ühenduse 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) või uuem versioon, kui see rakendus on .net taustväärtus.

* Android 2.3 või uuemat versiooni, Google hoidla paranduse 27 või uuem versioon ja Google Play teenuste 9.0.2 või uuem versioon Firebase'i Cloud sõnumside.

* Täitke [Androidi Lühijuhend].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Projekti, mis toetab Firebase'i Cloud sõnumside loomine

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Teate jaoturi konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure'i saata Tõuketeatiste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Project serveri Tõuketeatiste lubamine

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Tõuketeatiste lisamiseks rakenduse

Selles jaotises saate värskendada oma kliendi Androidi käsitlema tõuketeatisi.

### <a name="verify-android-sdk-version"></a>Veenduge, et Android SDK versioon

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Teie järgmise sammuna tuleb installida Google Play teenused. Google Cloud sõnumside on mõned API taseme miinimumnõuded katsetamine, mille manifestis **minSdkVersion** atribuut peab vastama.

Kui on testimine vanemat seadet, siis konsulteerige [Seadmine Google Play teenuste SDK üles] kuidas madal määratlemiseks Määrake selle väärtuseks ja õigesti seatud.

### <a name="add-google-play-services-to-the-project"></a>Google Play teenuseid lisamine projekti

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Koodi lisamine

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Rakenduse proovimine avaldatud mobiilsideteenuse testimine

Rakenduse saate testida, lisades otse USB-kaabli abil Androidi telefonis või emulaator virtuaalne seadme abil.

## <a name="more"></a>Lisateave

<!-- URLs -->
[Androidi Lühijuhend]: app-service-mobile-android-get-started.md

[Google Play teenused SDK häälestamine]:https://developers.google.com/android/guides/setup
