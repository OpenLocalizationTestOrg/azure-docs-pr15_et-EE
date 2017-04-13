<properties
    pageTitle="Klõpsake Azure'i rakendust Service mobiilirakendused Androidi rakenduse loomine | Microsoft Azure'i"
    description="Järgige selle õpetuse Androidi arengu Azure mobiilirakenduse taustaprogrammid kasutamise alustamine"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-android-app"></a>Androidi rakenduse loomine

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas lisada kirjutamata pilvepõhist teenust Mobile'i Androidi on Azure mobiilirakenduse taustväärtus abil.  Loote uue mobiilirakenduse kirjutamata nii lihtsa _Todo loendi_ Androidi rakendust, mis talletab rakenduse andmete Azure.

Selle õpetuse lõpuleviimine on nõutav muude Androidi õpetused teenuses Azure rakenduse funktsiooni Mobile'i rakenduste kasutamise kohta.

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks vajate järgmist:

* [Androidi arendaja tööriistu](https://developer.android.com/sdk/index.html), mis sisaldab Androidi Studio integreeritud keskkonnas ja uusim Android platvorm.
* Azure'i Mobile Androidi Tarkvaraarenduskomplektist, mis viitab automaatselt Kiirjuhend projekti, saate alla laadida.
* [Azure active konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Looge uus Azure mobiilirakenduse kirjutamata

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Project serveri konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Laadige alla ja Androidi rakenduse käivitamine

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
