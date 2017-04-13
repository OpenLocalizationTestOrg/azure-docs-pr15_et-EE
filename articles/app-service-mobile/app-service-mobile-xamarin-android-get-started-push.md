<properties
    pageTitle="Tõuketeatiste lisamine rakenduse Xamarin.Android | Azure'i rakendust Service"
    description="Saate teada, kuidas Azure'i rakendust Service ja Azure teatis jaoturi abil saata oma Xamarin.Android rakenduse tõuketeatised"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Tõuketeatiste Xamarin.Android rakenduse lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Ülevaade


Selles õpetuses lisate Tõuketeatiste projekti [Xamarin.Android kiire alustamine](app-service-mobile-windows-store-dotnet-get-started.md) , et tõuketeatised teatis saadetakse seade iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateavet leiate [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ Aktiivse Gmaili kontosse. Saate registreeruda Gmaili kontosse veebisaidil [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Google Cloud sõnumside kliendi komponent](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Teate jaoturi konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Luba Firebase'i Cloud sõnumside

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Azure'i tõuketeatised päringuid konfigureerimine

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Värskenduse saatmiseks tõuketeatisi projekti server

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Kliendi projekti jaoks Tõuketeatiste konfigureerimine

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Rakenduse push teatised koodi lisamine

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Rakenduse testi tõuketeatised

Saate rakenduse abil virtuaalne seadme emulaator testida. Järgnevalt on vajalik, kui emulaator konfigureerimise juhised.

1. Veenduge, et teil on kasutusele võtavad või silumine virtuaalse seadmes, mis sisaldab Google API-de määramine mis Sihtvaluuta, nagu on näidatud allpool Androidi virtuaalse seadme (AVD) manager.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Google konto lisamine Androidi seadme, klõpsates nuppu **rakendused** > **sätted** > **Lisa konto**, siis järgige kuvatavaid juhiseid.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Enne kui todolist rakenduse käivitamine ja todo uue üksuse lisamine. Sel ajal, kuvatakse olekuala ikoon olekualal. Saate avada märguandesahtel teatise täisteksti kuvamiseks.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
