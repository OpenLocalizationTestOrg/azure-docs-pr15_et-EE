<properties
    pageTitle="Kuidas konfigureerida rakenduse teenuste rakenduse Microsoft Account autentimine"
    description="Saate teada, kuidas rakenduse teenuste rakenduse Microsoft Account autentimise konfigureerimine."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Microsoft Account sisselogimine

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

See teema näitab, kuidas konfigureerida Azure rakenduse kasutamiseks Microsofti Account autentimisteenuse pakkuja. 

## <a name="register-microsoft-account"> </a>Rakenduse Microsoft Account registreerimine

1. [Azure'i portaali]sisse logida ja liikuge rakenduse. Kopeerige oma **URL-i**, mis hiljem kasutate Microsofti Account rakenduse konfigureerimiseks.

2. Avage Microsofti Account Developer Center lehele [Minu rakendused] ja vajadusel logige oma Microsofti kontoga.

3. Klõpsake nuppu **Lisa rakendus**, seejärel tippige rakenduse nimi ja klõpsake nuppu **Loo rakenduse**.

4. Kirjutage **Rakenduse ID**, kui teil on vaja hiljem. 

5. Jaotises "Platvormid", klõpsake nuppu **Lisa platvormi** ja valige "Web".

6. Jaotises "Ümber suunata URI" esitama rakenduse lõpp-punkti ja seejärel klõpsake nuppu **Salvesta**. 
 
    >[AZURE.NOTE]Teie redirect URI on lisatud teega, _/.auth/login/microsoftaccount/callback_rakenduse URL. Näiteks `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Veenduge, et kasutate HTTPS värviskeemi.

7. Jaotises "Rakenduse saladused," nuppu **Loo uus parool**. Märkige üles väärtuse, mis kuvatakse. Kui lahkute lehelt, seda ei kuvata uuesti.


    > [AZURE.IMPORTANT] Parool on mõni oluline turvalisuse mandaati. Parooli teistega või levitada kliendi rakenduses.

## <a name="secrets"> </a>Oma rakenduse teenuserakenduse teabe Microsofti konto lisamine

1. Tagasi [Azure portaali], avage rakenduse, klõpsake nuppu **sätted** > **autentimine / luba**.

2. Kui autentimine / autoriseerimine funktsioon pole lubatud, lülitage see **sisse**.

3. Klõpsake linki **Microsofti konto**. Rakenduse ID ja parooliga väärtused, mida te ei saanud varem Kleebi ja soovi korral lubada mis tahes otsinguulatuste rakenduse jaoks on vaja. Klõpsake nuppu **OK**.

    ![][1]

    Vaikimisi rakenduse teenus pakub autentimist, kuid ei piirata volitatud juurdepääsu oma saidi sisu ja API-d. Teil tuleb lubada kasutajate rakenduse koodi.

4. (Valikuline) Ainult need kasutajad, autenditud Microsofti konto saidile juurdepääsu piiramiseks, määrake **toiming taotluse pole autenditud** **Microsofti**kontoga. Selleks on vaja autentida kõik kutsed ja kõik autentimata taotlused suunatakse teid Microsofti konto autentimine.

5. Klõpsake nuppu **Salvesta**.

Nüüd olete valmis kasutama Microsofti Account autentimise rakenduse.

## <a name="related-content"> </a>Seotud sisu

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Minu rakendused]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure'i portaal]: https://portal.azure.com/
