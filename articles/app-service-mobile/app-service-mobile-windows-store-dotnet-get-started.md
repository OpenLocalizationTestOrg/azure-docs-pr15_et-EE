<properties
    pageTitle="Luua mõne Universal platvormil (UWP) kasutavas Mobile'i rakendused | Microsoft Azure'i"
    description="Järgige selle õpetuse alustada Universal platvormil (UWP) rakenduste arendamiseni C#, Visual Basic või JavaScript Azure mobiilirakenduse taustaprogrammid kasutamise."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Windowsi rakenduse loomine

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas lisada kirjutamata pilvepõhist teenust Universal platvormil (UWP) rakendus. Lisateabe saamiseks vaadake teemat [mis on mobiilirakenduste kohta](app-service-mobile-value-prop.md). Järgnevalt on lõpule viidud rakendusest ekraanipildid.

![Lõpetatud töölauarakenduses](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Töölaua töötab. 

![Lõplikus telefonirakenduse](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Töötab mobiiltelefonis

Selle õpetuse lõpuleviimine on nõutav kõik mobiilirakenduse õpetused UWP rakendused. 

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks vajate järgmist:

* Aktiivne Azure'i konto. Kui teil pole kontot, saate mõne Azure prooviversiooni kasutajaks ja saada kuni 10 tasuta mobiilirakendused, mis saab edasi kasutada, isegi pärast prooviperioodi lõppu. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio ühenduse 2015] või uuem versioon.

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](https://tryappservice.azure.com/?appServiceName=mobile). Olemas, saate kohe luua lühiajaline starter mobiilirakenduse rakendust Service – pole vaja krediitkaarti ja kohustusi.

##<a name="create-a-new-azure-mobile-app-backend"></a>Looge uus Azure'i Mobile'i rakendus taustväärtus

Järgmiste juhiste abil saate luua uue Mobile'i rakendus taustväärtus.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nüüd on ette valmistatud Azure'i Mobile'i rakendus taustväärtus, mida saate kasutada rakenduste mobiilikliendi. Järgmiseks laadite serveri projekti jaoks lihtne "todo list" kirjutamata ja Azure avaldada.

## <a name="configure-the-server-project"></a>Project serveri konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Alla laadida ja käivitada kliendi projekti

Kui olete konfigureerinud oma mobiilirakenduse kirjutamata, saate luua uue klientrakenduse rakenduse või olemasoleva rakenduse Azure'i ühenduse muutmine. Selles jaotises saate alla laadida UWP rakenduse malli projekti, mis on kohandatud ühenduse loomiseks oma Mobile'i rakendus taustväärtus.

1. Tagasi oma mobiilirakenduse kirjutamata **Lühijuhend** labale nuppu **Loo uus rakendus** > **Laadige alla**ja seejärel ekstrakti tihendatud projekti faile kohalikku arvutisse.

    ![Windowsi Kiirjuhend projekti allalaadimine](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Valikuline) Project serveri nimega sama lahendust UWP rakenduse project lisada. See on hõlpsam silumine ja testimine nii rakendust kui ka selle kirjutamata sama lahenduse Visual Studios, kui te ei soovi seda teha. Lahendus projekti UWP rakenduse lisamiseks peate kasutama Visual Studio 2015 või uuem versioon.

4. Rakendusega UWP käivitus projekti klahvi F5 juurutada ja käivitage rakendus.

5. Rakenduse selgitav tekst, nt *lõpuleviimine õpetuse*, tippige tekstiväljale **soovitud TodoItem lisada** ja klõpsake siis nuppu **Salvesta**.

    ![Windowsi Kiirjuhend täieliku töölaud](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    See saadab POST taotlus uue mobiilirakenduse kirjutamata, mis on majutatud Azure.

6. (Valikuline) Rakenduse peatada ja taaskäivitage see mobiilsideseadmete emulaator või muust seadmest.

    ![Windowsi Kiirjuhend täieliku telefon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Pange tähele, et andmed on salvestatud eelmises juhises laaditakse Azure'i pärast UWP rakenduse käivitamisel. 

##<a name="next-steps"></a>Järgmised sammud

* [Rakenduse lisamiseks autentimine](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Saate teada, kuidas kasutajad oma rakenduse pakkujaga identiteedi kinnitamiseks.

* [Tõuketeatiste lisamiseks rakenduse](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Saate teada, kuidas lisada push teatised rakenduse tugi ja konfigureerida oma Mobile'i rakendus kirjutamata saatmiseks tõuketeatisi Azure'i teatis jaoturi abil.

* [Rakenduse ühenduseta sünkroonimise lubamine](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil ka Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio ühenduse 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
