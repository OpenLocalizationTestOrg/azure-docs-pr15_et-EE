<properties
    pageTitle="Rakenduse teenuste rakenduse Azure Active Directory autentimise konfigureerimine"
    description="Saate teada, kuidas rakenduse teenuste rakenduse Azure Active Directory autentimise konfigureerimine."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Azure Active Directory sisselogimine

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

See teema näitab, kuidas konfigureerida Azure rakenduse teenuste kasutamiseks Azure Active Directory autentimisteenuse pakkuja.

## <a name="express"> </a>Konfigureerida Azure Active Directory kiire sätete abil

13. [Azure'i portaali], avage rakenduse. Klõpsake nuppu **sätted**ja seejärel valige **Autentimise/autoriseerimine**.

14. Kui autentimine / autoriseerimine funktsioon pole lubatud, lülitage **sisse**aktiveerimine.

15. Klõpsake **Azure Active Directory**ja klõpsake nuppu **Express** **Juhtimist**.

16. Klõpsake nuppu **OK** , et registreerida rakenduse Azure Active Directory. See loob uue registreerimise. Kui soovite valida mõne olemasoleva registreerimise selle asemel, klõpsake nuppu **Vali olemasolev rakendus** ja otsige üles varem loodud registreerimise jooksul oma rentniku nimi.
Klõpsake nuppu registreerimine, valige see ja klõpsake nuppu **OK**. Enne Azure Active Directory sätted, klõpsake nuppu **OK** .

    ![][0]

    Vaikimisi rakenduse teenus pakub autentimist, kuid ei piirata volitatud juurdepääsu oma saidi sisu ja API-d. Teil tuleb lubada kasutajate rakenduse koodi.

17. (Valikuline) Seadmine juurdepääsu piiramiseks ainult Azure Active Directory autenditud kasutajad saidile sisse **logima Azure Active Directory** **toiming taotluse pole autenditud** . Selleks on vaja autentida kõik kutsed ja kõik autentimata taotlused suunatakse Azure Active Directory autentimine.

17. Klõpsake nuppu **Salvesta**.

Nüüd olete valmis kasutama Azure Active Directory autentimise rakenduse.

## <a name="advanced"> </a>(Alternatiivne meetod) Azure Active Directory ja täpsemate sätete käsitsi konfigureerimine
Võite ka sisestada konfiguratsioonisätted käsitsi. Kui soovite kasutada AAD rentniku erineb rentniku, mille abil logite Azure on see eelistatud lahendus. Konfigureerimise lõpuleviimiseks peate esmalt looma registreerimine Azure Active Directory ja seejärel peab võimaldama mõned üksikasjade rakenduse teenusega.

### <a name="register"> </a>Rakenduse registreerida Azure Active Directory

1. [Azure'i portaali]sisse logida ja liikuge rakenduse. Kopeerige oma **URL-i**. Selle abil kuvatakse rakenduse Azure Active Directory konfigureerimine.

3. [Azure'i klassikaline portaali] sisse logida ja liikuge **Active Directory**.

    ![][2]

4. Valige oma kausta ja valige **rakendused** vahekaardil ülaosas. Luua uue rakenduse registreerimine allosas nuppu **ADD** .

5. Klõpsake nuppu **Lisa rakenduse areneb minu asutust**.

6. Rakenduse viisardit, sisestage oma rakenduse **nimi** ja klõpsake nuppu **Rakendus ja/või Web Veebiteenuste** tüüp. Seejärel klõpsake nuppu edasi.

7. Kleepige väljale **SISSELOGIMISE-ON URL-i** rakenduse varem kopeeritud URL. Sisestage väljale **Rakenduse ID URI** selle sama URL-i. Seejärel klõpsake nuppu edasi.

8. Kui rakendus on lisatud, klõpsake vahekaarti **konfigureerimine** . Redigeerige **Vastus URL-i** jaotises **ühekordse sisselogimise** tuleb teed, _/.auth/login/aad/callback_on lisatud sünkroonimistõrke rakenduse URL. Näiteks `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Veenduge, et kasutate HTTPS värviskeemi.

    ![][3]

9. Klõpsake nuppu **Salvesta**. Seejärel kopeerige **Kliendiid** rakenduse. Konfigureerige rakenduse seda edaspidi kasutada.

10. Allosas oleval ribal käsku, klõpsake nuppu **Kuva lõpp-punktid**ja **Federation metaandmete dokumendi** URL-i kopeerimine ja see dokument alla laadida või liikuge seda brauseris.

11. Sees root **EntityDescriptor** element, peaks olema vormi atribuudi **entityID** `https://sts.windows.net/` , millele järgneb teatud GUID oma rentniku (nn "rentniku ID"). Kopeerige see väärtus – see toimib oma **Väljaandja URL-i**. Konfigureerige rakenduse seda edaspidi kasutada.

### <a name="secrets"> </a>Lisada Azure Active Directory teavet rakenduse

13. Tagasi [Azure portaali], otsige üles rakenduse. Klõpsake nuppu **sätted**ja seejärel valige **Autentimise/autoriseerimine**.

14. Kui autentimine/autoriseerimine funktsioon pole lubatud, lülitage **sisse**aktiveerimine.

15. Klõpsake **Azure Active Directory**ja seejärel kategooriat **Täpsemalt** jaotises **Juhtimist**. Kleepige varem hankisite kliendi ID ja väljaandja URL-i väärtust. Klõpsake nuppu **OK**.

    ![][1]

    Vaikimisi rakenduse teenus pakub autentimist, kuid ei piirata volitatud juurdepääsu oma saidi sisu ja API-d. Teil tuleb lubada kasutajate rakenduse koodi.

17. (Valikuline) Seadmine juurdepääsu piiramiseks ainult Azure Active Directory autenditud kasutajad saidile sisse **logima Azure Active Directory** **toiming taotluse pole autenditud** . Selleks on vaja autentida kõik kutsed ja kõik autentimata taotlused suunatakse Azure Active Directory autentimine.

17. Klõpsake nuppu **Salvesta**.

Nüüd olete valmis kasutama Azure Active Directory autentimise rakenduse.

## <a name="optional-configure-a-native-client-application"></a>(Valikuline) Omakliendi rakenduse konfigureerimine

Azure Active Directory võimaldab teil registreerida kohalikke kliendid, mis pakub vastendamise õiguste üle suuremat kontrolli. Peate seda, kui soovite teha sisselogimise, nt **Active Directory Authentication Library**teegi kasutamine.

1. Liikuge **Active Directory** [Azure klassikaline portaalis].

2. Valige oma kausta ja valige **rakendused** vahekaardil ülaosas. Luua uue rakenduse registreerimine allosas nuppu **ADD** .

3. Klõpsake nuppu **Lisa rakenduse areneb minu asutust**.

4. Rakenduse viisardit, sisestage oma rakenduse **nimi** ja klõpsake nuppu **Kohalikke klientrakendusega** tüüp. Seejärel klõpsake nuppu edasi.

5. Sisestage väljale **URI suunake** oma saidi _/.auth/login/done_ lõpp-punkti, kasutades HTTPS värviskeemi. See väärtus peab olema _https://contoso.azurewebsites.net/.auth/login/done_. Kui loomine Windowsi rakendus, kasutage selle asemel [paketi SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) URI.

6. Kui rakendus on lisatud, klõpsake vahekaarti **konfigureerimine** . **Kliendi ID** leidmine ja märkige üles selle väärtuse.

7. Liikuge kerides jaotiseni **muudes rakendustes õiguste** lehel ja klõpsake käsku **Lisa rakendus**.

8. Otsige veebirakendus, mis varem registreeritud ja klõpsake plussmärgiikooni. Klõpsake dialoogiboksi sulgemiseks sisse. Kui veebirakenduse ei saa leitud, liikuge registreerimist ja lisage uus vastus URL (nt praeguse URL-i HTTP versioon), klõpsake nuppu Salvesta ja seejärel korrake neid juhiseid - rakendus peaks näidata loendis.

9. Äsja lisatud uus kirje avamine **Delegeeritud õiguste** rippmenüü ja valige **Access (rakendusenimi)**. Klõpsake nuppu **Salvesta**.

Nüüd olete konfigureerinud kohalikke klientrakendusega, millele pääsete juurde oma rakenduse teenuserakenduse.

## <a name="related-content"> </a>Seotud sisu

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure'i portaal]: https://portal.azure.com/
[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
[alternative method]:#advanced
