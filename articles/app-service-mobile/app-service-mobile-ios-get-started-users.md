<properties
    pageTitle="IOS-is Azure'i mobiilirakenduste autentimist lisamine"
    description="Saate teada, kuidas kasutada autentida kasutajad oma iOS-i rakenduse kaudu identiteedipakkujad, sh AAD, Google, Facebooki, Twitteri ja Microsoft Azure'i mobiilirakenduste kohta."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Oma iOS-i rakenduse lisamine autentimine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Selles õpetuses saate lisada autentimist kasutades toetatud identiteedipakkuja [iOS-i Kiire alustamine] projekti. Selle õpetuse aluseks on [iOS-i Kiire alustamine] õpetuse, mille peate.

##<a name="register"></a>Registreerimist rakenduse autentimiseks ja rakenduse teenuse konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Vajutage Xcode, käivitage rakendus **käivitada** . Erandi tõstetakse, sest rakendus proovib juurde pääseda kirjutamata autentimata kasutajana, kuid _TodoItem_ tabeli nüüd nõuab autentimist.

##<a name="add-authentication"></a>Autentimise lisamine rakendusse

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS-i Lühijuhend]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
