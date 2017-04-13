<properties
    pageTitle="Facebooki rakenduse teenuste rakenduse autentimise konfigureerimine"
    description="Saate teada, kuidas rakenduse teenuste rakenduse Facebooki autentimise konfigureerimine."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Facebook login

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

See teema näitab, kuidas konfigureerida Azure rakenduse kasutamiseks Facebooki autentimisteenuse pakkuja.

Selles teemas antud lõpuleviimiseks peab teil olema Facebooki konto, mis on kinnitatud meiliaadressi ja mobiiltelefoninumbri. Facebooki uue konto loomiseks avage [facebook.com].

## <a name="register"> </a>Facebooki rakenduse registreerima

1. [Azure'i portaali]sisse logida ja liikuge rakenduse. Kopeerige oma **URL-i**. Seda kasutatakse teie Facebooki rakenduse konfigureerimiseks.

2. Mõnes muus brauseriaknas, avage veebisait [Facebook arendajad] ja logige sisse oma Facebooki konto identimisteave.

3. (Valikuline) Kui te pole juba registreerunud, klõpsake **rakenduste** > **arendaja registreerida**, siis nõustuge poliitika ja järgige registreerimise juhiseid.

4. Valige **minu rakendused** > **lisada uue rakenduse** > **veebisaidi** > **vahele jätta ja rakenduse ID loomine**. 

5. **Kuvatav nimi**, sisestage oma rakenduse kordumatu nimi, tippige **Kontakti e-posti**, valige **kategooria** oma rakenduse ja seejärel klõpsake **rakenduse loomine ID** ja täitke turvalisus sisse. See viib teid oma uue Facebooki rakenduse arendaja armatuurlaud.

6. Klõpsake jaotises "Facebooki Logi sisse," **Alustamine**. Rakenduse **Ümber suunata URI** lisamine **lubatud OAuthi ümber suunata URI-d**ja seejärel klõpsake käsku **Salvesta muudatused**. 

    > [AZURE.NOTE] Teie redirect URI on lisatud teega, _/.auth/login/facebook/callback_rakenduse URL. Näiteks `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Veenduge, et kasutate HTTPS värviskeemi.

6. Klõpsake vasakpoolsel navigeerimispaanil nuppu **sätted**. **Rakenduse salajane** välja kuvamiseks märkige ruut **Kuva**, sisestage oma parool nõutav, siis märkige **Rakenduse ID** ja **Rakenduse salajane**väärtused. Saate neid hiljem rakenduse Azure konfigureerimine.

    > [AZURE.IMPORTANT] Salajane rakendus on mõni oluline turvalisuse mandaati. Selle salajase teistega või levitada kliendi rakenduses.

7. Facebooki konto, mida kasutati registreerida rakenduse on rakenduse administraator. Selles etapis vaid administraatorid saate sisse logida selle rakenduse. Muude Facebooki kontode kinnitamiseks valige **Rakendus läbivaatus** ja lubamine **avalikusta < teie-rakenduse nimi >** üldist avaliku juurdepääsu Facebooki autentimist kasutades.

## <a name="secrets"> </a>Facebooki lisada teavet rakenduse

1. Tagasi [Azure portaali], otsige üles rakenduse. Klõpsake nuppu **sätted** > **autentimine / luba**, ja veenduge, et **Rakendus autentimise** on **sisse lülitatud**.

2. Klõpsake **Facebooki**, kleepige rakenduse ID-d ja rakenduse salajane väärtusi, mille te varem saadud, võite lubada mis tahes otsinguulatuste rakenduse jaoks vajalik, seejärel klõpsake nuppu **OK**.

    ![][0]

    Vaikimisi rakenduse teenus pakub autentimist, kuid ei piirata volitatud juurdepääsu oma saidi sisu ja API-d. Teil tuleb lubada kasutajate rakenduse koodi.

3. (Valikuline) Ainult autenditud Facebook kasutajatele saidile juurdepääsu piiramiseks, määrake **toiming taotluse pole autenditud** **Facebooki**. Selleks, et kõik kutsed autentida ja kõik autentimata taotlused suunatakse Facebooki autentimist.

4. Kui autentimise konfigureerimine lõpetanud, klõpsake nuppu **Salvesta**.

Nüüd olete valmis kasutama Facebooki rakenduse autentimiseks.

## <a name="related-content"> </a>Seotud sisu

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook arendajad]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure'i portaal]: https://portal.azure.com/
