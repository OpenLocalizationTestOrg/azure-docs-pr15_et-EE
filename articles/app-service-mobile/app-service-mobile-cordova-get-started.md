<properties
    pageTitle="Azure'i rakendust Service mobiilirakenduste kohta Cordova rakenduse loomine | Microsoft Azure'i"
    description="Järgige selle õpetuse Apache Cordova arengu Azure mobiilirakenduse taustaprogrammid kasutamise alustamine"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="Cordova, javascript, mobiilikliendi" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Luua Apache Cordova rakendus

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Ülevaade

Selle õpetuse näidatakse, kuidas lisada kirjutamata pilvepõhist teenust Apache Cordova Mobile'i rakendus on Azure mobiilirakenduse taustväärtus abil.  Loote uue mobiilirakenduse kirjutamata nii lihtsa _Todo loendi_ Apache Cordova rakendus, mis talletab rakenduse andmete Azure.

Selle õpetuse lõpuleviimine on nõutav kõik Apache Cordova õpetused teenuses Azure rakenduse funktsiooni Mobile'i rakenduste kasutamise kohta.

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks vajate järgmist:

* Arvuti abil [Visual Studio ühenduse 2015] või uuemat versiooni.
* [Visual Studio tööriistad Apache Cordova jaoks].
* [Azure active konto](https://azure.microsoft.com/pricing/free-trial/).

Võite ka mööduda Visual Studio ja kasutage Apache Cordova käsurea otse.  See on kasulik, kui lõpuleviimine õpetuse Mac-arvutis.  Selles õpetuses ei hõlma koostamise käsureal Apache Cordova klientrakendustes.

## <a name="create-a-new-azure-mobile-app-backend"></a>Looge uus Azure mobiilirakenduse kirjutamata

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Vaadake videot nähtaval sarnased toimingud](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Project serveri konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Laadige alla ja käivitage rakendus Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui te täitnud õppeteema Lühijuhend, liikuda ühte järgmistest õpetused:

* Apache Cordova rakenduse [Lisamine autentimist] .
* [Tõuketeatised lisamine] rakenduse Apache Cordova.

Lugege lisateavet võtme põhimõtet Azure'i rakenduse teenusega.

* [Autentimine]
* [Tõuketeatised]

Siit saate teada, kuidas kasutada funktsiooni SDK-d.

* [Apache Cordova SDK]
* [ASP.net-i serveri SDK]
* [Node.js serveri SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio ühenduse 2015]: http://www.visualstudio.com/
[Visual Studio tööriistad Apache Cordova jaoks]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Lisage autentimine]: app-service-mobile-cordova-get-started-users.md
[Tõuketeatiste lisamine]: app-service-mobile-cordova-get-started-push.md
[Autentimine]: app-service-mobile-auth.md
[Tõuketeatised]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.net-i serveri SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js serveri SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
