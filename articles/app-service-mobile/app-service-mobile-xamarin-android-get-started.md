<properties
    pageTitle="Azure'i mobiilirakenduste Xamarin.Android rakenduste kasutamise alustamine"
    description="Järgige selle õpetuse Xamarin Androidi arengu Azure'i Mobile'i rakenduste kasutamise alustamine"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Xamarin.Android rakenduse loomine

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas lisada kirjutamata pilvepõhist teenust Xamarin.Android rakendus. Lisateabe saamiseks vaadake teemat [mis on mobiilirakenduste kohta](app-service-mobile-value-prop.md).

Kuvatõmmis lõplikus rakendusest on all.

![][0]

Selle õpetuse lõpuleviimine on nõutav kõik mobiilirakenduste õpetused Xamarin.Android rakendused.

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks peate järgmine kohustuslik tarkvara.

* Aktiivne Azure'i konto. Kui teil pole kontot, on Azure prooviversiooni kasutajaks ja saada kuni 10 tasuta mobiilirakenduste kohta. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio abil Xamarin. Vaadake [installiprogramm ja installige Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) olevaid juhiseid.

>[AZURE.NOTE]Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](https://tryappservice.azure.com/?appServiceName=mobile).  Saate luua kohe lühiajaline starter Mobile'i rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

## <a name="create-an-azure-mobile-app-backend"></a>Azure'i Mobile'i rakendus on taustväärtus loomine

Järgmiste juhiste abil luua mobiilirakenduse taustväärtus.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nüüd on ette valmistatud Azure'i Mobile'i rakendus taustväärtus, mida saate kasutada rakenduste mobiilikliendi. Järgmiseks allalaadimine server projekti jaoks lihtne "todo list" kirjutamata ja Azure avaldada.

## <a name="configure-the-server-project"></a>Project serveri konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Laadige alla ja käivitage rakendus Xamarin.Android

1. Jaotises **alla laadida ja käivitada Xamarin.Android projekti**, nuppu **Laadi alla** .

    Salvestage fail tihendatud projekti kohalikus arvutis ja kirjutage, kuhu see salvestada.

2. Projekti koostamine ja käivitage rakendus klahvi **F5** .

3. Rakenduses tippige tähendusrikas teksti, nt _lõpuleviimine õpetuse_ ja seejärel klõpsake nuppu **Lisa** .

    ![][10]

    Taotluse andmed lisatakse tabelisse TodoItem. Üksused, mis on talletatud tabelis on tagastatud mobiilirakenduse kirjutamata ja andmed kuvatakse loendis.

    > [AZURE.NOTE] Saate vaadata koodi, mis kasutab teie mobiilirakenduse kirjutamata päringu ja andmed, mille leiate ToDoActivity.cs C# faili lisamiseks.

##<a name="next-steps"></a>Järgmised sammud

* [Ühenduseta sünkroonimise lisamiseks rakenduse](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Rakenduse lisamiseks autentimine](app-service-mobile-xamarin-android-get-started-users.md)
* [Tõuketeatiste Xamarin.Android rakenduse lisamine](app-service-mobile-xamarin-android-get-started-push.md)
* [Kuidas kasutada hallatavate kliendi Azure'i mobiilirakenduste kohta](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
