<properties
    pageTitle="Kuidas rakendus teenuste rakenduse Twitteri autentimise konfigureerimine"
    description="Saate teada, kuidas rakenduse teenuste rakenduse Twitteri autentimise konfigureerimine."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Twitteri login

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

See teema näitab, kuidas konfigureerida Azure rakenduse kasutamiseks Twitteri autentimisteenuse pakkuja.

Selles teemas antud lõpuleviimiseks peab teil olema Twitteri konto, mis on kinnitatud e-posti meiliaadressi ja telefoninumbri sisestamine. Minge <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>Twitteri uue konto loomiseks.

## <a name="register"> </a>Twitteri rakenduse registreerima


1. [Azure'i portaali]sisse logida ja liikuge rakenduse. Kopeerige oma **URL-i**. Selle abil kuvatakse rakenduse Twitteri konfigureerimine.

2. Avage veebisait [Arendajate Twitteri] , logige sisse oma Twitteri konto identimisteave ja klõpsake nuppu **Loo uus rakendus**.

3. Tippige **nimi** ja **Kirjeldus** uus rakendus. Kleepige oma rakenduse väärtus **veebisaidi** **URL-i** . **Tagasihelistamise URL**, kleepige varem kopeeritud **Tagasihelistamise URL-i** . See on lisatud teega, _/.auth/login/twitter/callback_lüüsi Mobile'i rakendus. Näiteks `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Veenduge, et kasutate HTTPS värviskeemi.

3.  Klõpsake lehe allosas lugege läbi ja nõustuge nendega. Klõpsake nuppu **Loo Twitteri rakenduse**. See registreerib rakendus kuvatakse rakenduse üksikasjad.

4. Klõpsake vahekaarti **sätted** , märkige **see rakendus Twitteri sisselogimiseks kasutada**ja seejärel klõpsake nuppu **Värskenda sätted**.

5. Valige vahekaart **Kiirklahvid ja juurdepääs märgid** . Kirjutage väärtused **Tarbija klahv (API võti)** ja **tarbija salajane (API salajane)**.

    > [AZURE.NOTE] Salajane tarbija on mõni oluline turvalisuse mandaati. Selle salajase teistega või levitada oma rakendusega.


## <a name="secrets"> </a>Rakenduse teabe lisamine Twitteri

13. Tagasi [Azure portaali], otsige üles rakenduse. Klõpsake nuppu **sätted**ja seejärel **autentimine / luba**.

14. Kui autentimine / autoriseerimine funktsioon pole lubatud, lülitage **sisse**aktiveerimine.

15. Klõpsake **Twitteri**. Kleepige rakenduse ID ja rakenduse salajane väärtusi, mida te saada varem. Klõpsake nuppu **OK**.

    ![][1]

    Vaikimisi rakenduse teenus pakub autentimist, kuid ei piirata volitatud juurdepääsu oma saidi sisu ja API-d. Teil tuleb lubada kasutajate rakenduse koodi.

17. (Valikuline) Ainult autenditud Twitter kasutajatele saidile juurdepääsu piiramiseks, seatud **toiming taotluse pole autenditud** **Twitteri**. Selleks, et kõik kutsed autentida ja kõik autentimata taotlused suunatakse Twitteri autentimist.

17. Klõpsake nuppu **Salvesta**.

Nüüd olete valmis kasutama Twitteri autentimise rakenduse.

## <a name="related-content"> </a>Seotud sisu

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitteri arendajad]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure'i portaal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
