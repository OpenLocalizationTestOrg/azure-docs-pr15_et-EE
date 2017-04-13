<properties
    pageTitle="Oma rakenduse teenuste rakendus Google autentimise konfigureerimine"
    description="Saate teada, kuidas oma rakenduse teenuste rakendus Google autentimise konfigureerimine."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Google sisselogimine

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

See teema näitab, kuidas konfigureerida Azure rakenduse kasutamiseks Google autentimisteenuse pakkuja.

Selles teemas antud lõpuleviimiseks peab teil olema Google'i konto, mis on kinnitatud meiliaadress. Google uue konto loomiseks avage [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Google rakenduse registreerima

1. [Azure'i portaali]sisse logida ja liikuge rakenduse. Kopeerige **URL**, mille abil saate hiljem oma Google'i rakenduse konfigureerimiseks.

2. Avage veebisait [Google API-d](http://go.microsoft.com/fwlink/p/?LinkId=268303) , logige sisse oma Google'i konto identimisteave, klõpsake nuppu **Loo projekt**, **projekti nime**ja seejärel käsku **Loo**.

3. Klõpsake jaotises **Social API -de** **Google + API** ja seejärel **lubada**.

4. Vasakpoolsel paanil **identimisteabe** > **OAuthi nõusoleku Kuva**, siis valige oma **e-posti aadressi**, sisestage **Toote nimi**ja klõpsake nuppu **Salvesta**.

5. **Mandaadi** menüüs nuppu **Loo identimisteavet** > **OAuthi kliendi ID**ja seejärel valige **veebirakendus**.

6. Kleepige varem kopeeritud **Luba JavaScript päritolu**rakenduse teenuse **URL-i** ja seejärel kleepige oma redirect URI **Volitatud ümber suunata URI**. Ümbersuunamise URI on lisatud teega, _/.auth/login/google/callback_rakenduse URL. Näiteks `https://contoso.azurewebsites.net/.auth/login/google/callback`. Veenduge, et kasutate HTTPS värviskeemi. Klõpsake nuppu **Loo**.

7. Klõpsake järgmisel kuval, kirjutage kliendi ID ja kliendi salajane väärtused.


    > [AZURE.IMPORTANT]
    Kliendi salajane on mõni oluline turvalisuse mandaati. Selle salajase teistega või levitada kliendi rakenduses.


## <a name="secrets"> </a>Google lisada teavet rakenduse

8. Tagasi [Azure portaali], otsige üles rakenduse. Klõpsake nuppu **sätted**ja seejärel **autentimine / luba**.

9. Kui autentimine / autoriseerimine funktsioon pole lubatud, lülitage **sisse**aktiveerimine.

10. Klõpsake **Google'i**. Kleepige rakenduse ID-d ja rakenduse salajane väärtused, mis te varem saadud ja soovi korral lubada mis tahes otsinguulatuste rakenduse jaoks on vaja. Klõpsake nuppu **OK**.

    ![][1]

    Vaikimisi rakenduse teenus pakub autentimist, kuid ei piirata volitatud juurdepääsu oma saidi sisu ja API-d. Teil tuleb lubada kasutajate rakenduse koodi.

17. (Valikuline) Määra juurdepääsu piiramiseks ainult Google autenditud kasutajad saidile **Google** **toiming taotluse pole autenditud** . Selleks, et kõik kutsed autentida ja kõik autentimata taotlused suunatakse Google autentimist.

12. Klõpsake nuppu **Salvesta**.

Nüüd olete valmis kasutama Google autentimise rakenduse.

## <a name="related-content"> </a>Seotud sisu

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure'i portaal]: https://portal.azure.com/

