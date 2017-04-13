<properties
    pageTitle="Azure'i rakenduse teenuse mobiilirakenduste Xamarin.iOS rakenduste kasutamise alustamine | Microsoft Azure'i"
    description="Järgige selle õpetuse Xamarin.iOS arengu Mobile'i rakenduste kasutamise alustamine."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Xamarin.iOS rakenduse loomine

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas lisada kirjutamata pilvepõhist teenust Xamarin.iOS mobiilirakenduse abil on Azure mobiilirakenduse taustväärtus.  Saate luua uue mobiilirakenduse kirjutamata nii lihtsa _Todo loendi_ Xamarin.iOS rakendus, mis talletab rakenduse andmete Azure.

Selle õpetuse lõpuleviimine on nõutav kõik Xamarin.iOS õpetused teenuses Azure rakenduse funktsiooni Mobile'i rakenduste kasutamise kohta.

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks peate järgmine kohustuslik tarkvara.

* Aktiivne Azure'i konto. Kui teil pole kontot, on Azure prooviversiooni kasutajaks ja saada kuni 10 tasuta mobiilirakendused, mis saab edasi kasutada, isegi pärast prooviperioodi lõppu. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio abil Xamarin. Vaadake [installiprogramm ja installige Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) olevaid juhiseid.

* Mac-arvutisse või uuema versiooniga Xcode v7.0 ja Xamarin Studio ühenduse installitud. Vt [häälestamine ja Visual Studio ja Xamarin installimine](https://msdn.microsoft.com/library/mt613162.aspx) ja [häälestamine, installimist ja kontrolle Maci kasutajad](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](https://tryappservice.azure.com/?appServiceName=mobile). Lühiajaline starter mobiilirakenduse kaudu saate luua kohe rakendust Service – pole vaja krediitkaarti ja kohustusi.

## <a name="create-an-azure-mobile-app-backend"></a>Azure'i Mobile'i rakendus on taustväärtus loomine

Järgmiste juhiste abil luua mobiilirakenduse taustväärtus.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Project serveri konfigureerimine

Nüüd on ette valmistatud Azure'i Mobile'i rakendus taustväärtus, mida saate kasutada rakenduste mobiilikliendi. Järgmiseks allalaadimine server projekti jaoks lihtne "todo loend" kirjutamata ja Azure avaldada.

Serveri projekti kasutada Node.js või .NET taustväärtus konfigureerimiseks järgmised juhised.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Laadige alla ja käivitage rakendus Xamarin.iOS

1. Avage [Azure'i portaalis] brauseriaknas.

2. Oma Mobile rakenduse enne sätted, klõpsake nuppu **Alustada** > **Xamarin.iOS**. Jaotises Samm 3, klõpsake nuppu **Loo uus rakendus** , kui see pole juba valitud.  Järgmiseks klõpsake nuppu **Laadi alla** .

    Kliendi rakendus, mis loob ühenduse oma mobiilse taustväärtus laaditakse. Salvestage fail tihendatud projekti kohalikus arvutis ja kirjutage, kuhu see salvestada.

3. Ekstraktida allalaaditud projekti ja avage see Xamarin Studio (või Visual Studio).

    ![][9]

    ![][8]

4. Projekti koostamine ja käivitage rakendus iPhone emulaator klahvi F5.

5. Rakenduse tippige tähendusrikas teksti, nt _Xamarin saate teada_, ja klõpsake seejärel soovitud **+** nupp.

    ![][10]

    Taotluse andmed lisatakse tabelisse TodoItem. Üksused, mis on talletatud tabelis on tagastatud mobiilirakenduse kirjutamata ja andmed kuvatakse loendis.

>[AZURE.NOTE]Saate vaadata, mis kasutab teie mobiilirakenduse kirjutamata päringu ja lisada andmete faili QSTodoService.cs C# koodi.

##<a name="next-steps"></a>Järgmised sammud

* [Ühenduseta sünkroonimise lisamiseks rakenduse](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Rakenduse lisamiseks autentimine](app-service-mobile-xamarin-ios-get-started-users.md)
* [Tõuketeatiste Xamarin.Android rakenduse lisamine](app-service-mobile-xamarin-ios-get-started-push.md)
* [Kuidas kasutada hallatavate kliendi Azure'i mobiilirakenduste kohta](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure'i portaal]: https://portal.azure.com/
