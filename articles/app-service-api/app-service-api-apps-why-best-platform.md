<properties 
    pageTitle="Sissejuhatus API rakendused | Microsoft Azure'i" 
    description="Siit saate teada, kuidas Azure'i rakendust Service abil saate töötada, pakkuja ja tarbimine rahulik API-d." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>API rakenduste ülevaade

Azure'i rakendust Service rakenduste API pakkuda funktsioone, mis oleks lihtsam töötada, pakkuja ja tarbimine API-de kohapealse ja pilveteenuse. API rakendusi saate ettevõtte tasemel turvalisus, lihtsa juurdepääsu reguleerimine, hübriid ühenduvust, SDK automaatseks ja sujuv integratsioon [Loogika rakendused](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Azure'i rakenduse teenus](../app-service/app-service-value-prop-what-is.md) on täielikult hallatavate platvorm mobiilse, veebi ja integreerimise stsenaariumid. API rakendused on üks neljast rakenduse pakutud [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md).

![Azure'i rakendust Service rakenduse tüüpi](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Miks kasutada API rakendusi?

Siin on mõned põhijooned API rakendused.

- **Tuua oma olemasoleva API-on** – teil pole vaja muuta oma olemasoleva API-de ära API rakenduste--lihtsalt juurutada oma kood API rakenduse koodi. Oma API saate kasutada mis tahes keel või framework rakenduse teenus, sh ASP.net-i ja C#, Java, PHP, Node.js ja Python ei toeta.

- **Lihtne tarbimis** - integreeritud tugi [ärplema API metaandmete](http://swagger.io/) muudab oma API-de hõlpsalt tarbitavad mitmesuguseid kliendid.  Automaatselt genereerida kliendi koodi oma erinevate keelte, sh C#, Java ja JavaScripti API-d. Hõlpsalt konfigureerimiseks [CORS](app-service-api-cors-consume-javascript.md) oma koodi muutmata. Lisateabe saamiseks vt [rakenduse API rakendused metaandmete API discovery ja koodi loomise](app-service-api-metadata.md) ja [tarbida API rakenduse abil CORS JavaScript](app-service-api-cors-consume-javascript.md). 

- **Lihtsa juurdepääsu reguleerimine** - API rakenduse kaitsta oma koodi autentimata juurdepääsuõiguste muudatusi. Sisseehitatud autentimisteenuste secure API-d juurdepääsu tähistav kasutajate klientidele või muude teenustega. Toetatud identiteedipakkujad kaasata Azure Active Directory, Facebooki, Twitteri, Google ja Microsofti Account. Kliendid saavad kasutada Active Directory autentimise Raamatukogu (ADAL) või Mobile'i rakendused SDK. Lisateavet leiate teemast [autentimise ja luba API rakendused teenuses Azure rakenduse](app-service-api-authentication.md).

- **Visual Studio integreerimine** – asjakohast tööriistad Visual Studio tõhustamine töö loomine ja juurutamine, tarbimine, silumine API rakenduste haldamine. Lisateabe saamiseks lugege teemat [väljakuulutamist Azure'i SDK 2.8.1 .net-i jaoks](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Loogika rakendustega integreerimise** - API rakendused, mille loote saate tarbitud [rakenduse teenuse loogika](../app-service-logic/app-service-logic-what-are-logic-apps.md)rakendused.  Lisateabe saamiseks vaadake [oma kohandatud API majutatud rakenduse teenuse loogika rakendustega](../app-service-logic/app-service-logic-custom-hosted-api.md) ja [uue skeemi versioon 2015-08-01-eelvaade](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Lisaks API rakendus võib kuluda [Veebirakenduste](../app-service-web/app-service-web-overview.md) ja [Mobiilirakenduste](../app-service-mobile/app-service-mobile-value-prop.md)pakutavaid võimalusi. Kehtib ka vastupidi: kui kasutate veebirakenduse või mobiilirakenduses majutada API, võib kuluda API rakenduste funktsioone nagu ärplema metaandmete klient koodi loomine ja CORS domeenide brauseri juurdepääsu. Ainult rakenduse kolme tüüpi (API, web, mobiil) vahe on nime ja kasutada neid Azure'i portaalis ikooni.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Mis vahe on API rakendused ja Azure API haldus?

API rakendused ja [Azure API haldus](../api-management/api-management-key-concepts.md) on täiendavad teenused.

* API haldamine on haldamise API-de kohta. Lõpetada API halduse ees API jälgimine ja ahendamise kasutamine, töödelda sisestus- ja väljundi, konsolideerida mitu API-de ühe lõpp-punkti ja nii edasi. Selle hallatava API olema majutatud suvalist kohta.
* API rakendused on majutusteenuse API-de kohta. Teenus sisaldab funktsioone, mis hõlbustavad arendamise ja nõudvate API-d, kuid see ei tee jälgimine, pidurdamise, käsitsemiseks liiki või konsolideerimisele selle API haldus ei. Kui te ei vaja API haldusfunktsioonid, saate majutada API-de API rakendustes kasutamata API haldus.

Siin on diagramm, mis kujutab API halduse API apps kui ka mujal majutatud API-de jaoks kasutada.

![Azure'i API haldus ja API rakendused](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Mõned funktsioonid API haldus ja API rakendused on sarnaseid funktsioone.  Näiteks saate nii automatiseerida CORS tugi. Kahe teenuseid koos kasutamisel saate kasutada API halduse CORS kuna see toimib esiosa oma API rakendusi. 

## <a name="getting-started"></a>Alustamine

Alustamiseks API rakendustega kasutades proovi kood ühele, lugege teemat õpetuse sellest, millist Framework, saate soovi korral:

* [ASP.NET-I](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Küsimuste API rakenduste kohta, et alustada teemat [API Appside Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 
