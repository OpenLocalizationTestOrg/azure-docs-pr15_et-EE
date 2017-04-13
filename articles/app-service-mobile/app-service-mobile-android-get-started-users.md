<properties
    pageTitle="Lisada autentimise Androidi Mobile'i rakendustega | Azure'i rakendust Service"
    description="Saate teada, kuidas kasutada autentida kasutajad Androidi rakenduse kaudu identiteedipakkujad, sh Google, Facebooki, Twitteri ja Microsoft Azure'i rakendust Service mobiilirakenduste kohta."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Androidi rakenduse lisamine autentimine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Kokkuvõte

Selles õppetükis saate lisada autentimine todolist Kiirjuhend projekti toetatud identiteedipakkuja Android kasutamise kohta. Selle õpetuse põhineb [Alustamine mobiilirakenduste] õpetuse, mille peate.

##<a name="register"></a>Registreerimist rakenduse autentimiseks ja rakenduse teenuse konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Androidi Studios, avage plaanitud teie lõpetatud projekti õpetusega [Alustamine mobiilirakenduste kohta]. **Käivitage** menüüs nuppu **Käivita rakendus** ja veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub.

     Seda erandit ei toimu, kuna rakendus proovib juurde pääseda kirjutamata autentimata kasutajana, kuid tabeli _TodoItem_ nüüd nõuab autentimist.

Järgmiseks värskendate rakenduse kasutajate autentimiseks enne taotluse ressursid kirjutamata mobiilirakenduse kaudu.

## <a name="add-authentication-to-the-app"></a>Autentimise lisamine rakendusse

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Vahemälu autentimine märkide klient

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui te täitnud õppeteema elementaarautentimine, kaaluge jätkuva ühte järgmistest õpetused edasi.

+ [Kui soovite oma Androidi lisamine tõuketeatised](app-service-mobile-android-get-started-push.md) Saate teada, kuidas konfigureerida oma Mobile'i rakendus kirjutamata saatmiseks tõuketeatisi Azure'i teatis jaoturi abil.

+ [Teie Androidi jaoks mõeldud ühenduseta sünkroonimise lubamine](app-service-mobile-android-get-started-offline-data.md) Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajal kasutamine mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Mobile'i rakenduste kasutamise alustamine]: app-service-mobile-android-get-started.md
