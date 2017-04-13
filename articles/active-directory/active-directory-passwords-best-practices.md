<properties
    pageTitle="Head tavad: Azure'i AD parooli haldus | Microsoft Azure'i"
    description="Juurutamise ja kasutamise head tavad, valimi lõppkasutaja dokumentatsiooni ja koolitus juhendite hankimiseks Azure Active Directory parooli haldus."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Halduse parooli juurutamisega ja koolitus kasutajad seda kasutada?

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

Pärast lubamine parooli lähtestamiseks peate tegema järgmiseks on teenuse kasutamise oma asutuse kasutajate saada. Selle tegemiseks peate veenduge, et teie kasutajad on konfigureeritud õigesti ja ka teenuse kasutamiseks peavad kasutajad koolitus, nad peavad olema edukalt oma paroolide haldamine. See artikkel selgitab teile järgmist põhimõtet:

* [**Kuidas saada kasutajate konfigureeritud parooli haldus**](#how-to-get-users-configured-for-password-reset)
  * [Milline konto on konfigureeritud parooli lähtestamine](#what-makes-an-account-configured)
  * [Viise andmete asustamiseks](#ways-to-populate-authentication-data)
* [**Parimaid viise hakkama parooli lähtestamiseks oma organisatsiooni**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [E-postipõhise plaanida + valimi e-posti suhtluse](#email-based-rollout)
  * [Luua oma kohandatud parooli haldusportaali kasutajate jaoks](#creating-your-own-password-portal)
  * [Kuidas kasutada jõustatud registreerimise kasutajatelt logige sisse veebisaidil register](#using-enforced-registration)
  * [Kasutajakonto andmete üleslaadimine](#uploading-data-yourself)
* [**Valimi kasutaja ja abimaterjalide koolitus (tulekul!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Kuidas saada kasutajate konfigureeritud parooli lähtestamine
Selles jaotises kirjeldatakse teile erinevad meetodid, mille alusel saate tagada teie asutuses iga kasutaja saab kasutada Iseteeninduslik parooli lähtestamine tõhus juhuks, kui nad oma parooli unustanud.

### <a name="what-makes-an-account-configured"></a>Milline konto on konfigureeritud
Enne, kui kasutaja parooli lähtestamine kasutamiseks **Kõik** järgmised tingimused peavad olema täidetud:

1.  Parooli lähtestamine peab olema lubatud kataloogis.  Saate teada, kuidas lubada parooli lähtestamise lugedes [võimaldavad kasutajatel Azure AD paroolide lähtestamiseks](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) või [võimaldavad kasutajatel lähtestamine või AD parooli muutmine](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
2.  Kasutaja peab olema litsentsitud.
 - Pilveteenuste kasutajad, peab kasutaja on **mis tahes tasulist Office 365 litsentsi**, või **AAD tavaline** või **AAD Premium litsentsi** määratud.
 - Kasutajatele kohapeal (ühendatud või räsi sünkroonitud), **peab olema AAD Premium litsentsi, määratud**kasutajale.
3.  Kasutaja peab olema **minimaalsed autentimise andmete määratletud** vastavalt praeguse parooli lähtestamine poliitika.
 - Andmete käsitletakse siis, kui vastav väli kataloogis sisaldab regulaarne andmed.
 - Andmete minimaalsed on määratletud nagu **vähemalt üks** lubatud autentimise suvandid, kui üks gate poliitika on konfigureeritud või **vähemalt kaks** lubatud autentimise suvandite kui kaks gate poliitika on konfigureeritud.
4.  Kui kasutaja on kasutusel asutusesisese konto, siis [Parooli tagasikirjutusega](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) peab olema lubatud ja sisse lülitanud.

### <a name="ways-to-populate-authentication-data"></a>Autentimise andmetega asustada viisid
Teil on mitu võimalust kohta, kuidas määrata oma asutuse kasutajate jaoks andmete kasutamiseks parooli lähtestamine.

- [Azure'i haldusportaal](https://manage.windowsazure.com) või [Office 365 halduskeskuse](https://portal.microsoftonline.com) kasutajate redigeerimine
- Azure AD sünkroonimine abil saate sünkroonida Azure AD kohapealse Active Directory domeeni atribuudid.
- Windows PowerShelli abil saate [siin juhiseid](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)järgides kasutaja atribuutide redigeerimine.
- Kasutajatel oma andmete registreerimisel juhendada neid registreerimise portaali aadressil [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Kasutajad registreeruma parooli lähtestada, kui nad saavad Azure AD kontosse sisselogimine, seades selle [**Kasutajad registreerida sisselogimisel?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) konfiguratsiooni valik **Jah**.

Kasutajad ei vaja registreerida parooli lähtestamise süsteemi töötamiseks.  Näiteks kui teil on olemasolevaid mobiiltelefon või Office'i telefoninumbrite kohaliku kataloogis, saate sünkroonida Azure AD ja kasutame neid parooli lähtestamise automaatselt.

Samuti võite lugeda Lisateavet [andmete kasutamise parooli lähtestamine](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) ja [kuidas saate asustada autentimise üksikute väljade PowerShelliga](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Mis on parim viis hakkama parooli lähtestamine kasutajate jaoks?
Järgmised juhised parooli lähtestamist üldine levitamise kohta.

1.  Luba parooli lähtestamise kataloogis **konfigureerimine** vahekaardil [Azure'i haldusportaal](https://manage.windowsazure.com) ja valides **Jah** suvandi **Kasutajatele lubada parool lähtestada** .
2.  Iga kasutaja nimi, kellele soovite parooli lähtestada pakkuda sobiv litsentside määramine selle [Azure'i haldusportaal](https://manage.windowsazure.com)vahekaardil **litsentside** alusel.
3.  Soovi korral piirata parooli lähtestamise rühmale kasutajad hakkama funktsiooni aeglaselt aja jooksul seada **Parooli lähtestamiseks juurdepääsu piiramise** tumblernupu **Jah** ja valides turberühma parooli lähtestamiseks (lisa need kasutajad peavad kõik olema määratud litsentside) lubamiseks.
4.  Paluge parool lähtestada, kas saata neile neid registreerida, juhendamine meili kasutajatele lubada jõustatud Accessi paneeli või nende kasutajate jaoks sobiv autentimise andmete üleslaadimisel ise DirSync, PowerShelli või [Azure'i haldusportaal](https://manage.windowsazure.com)kaudu.  Lisateavet selle kohta, kui see on esitatud allpool.
5.  Aja jooksul, lugege läbi kasutajate registreerida vahekaardile liikumine ja [**Parooli lähtestamiseks registreerimise tegevuse**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) aruande kuvamisel.
6.  Kui olete registreerunud on hea kasutajate arv, vaadake neid kasutada parooli lähtestamise vahekaardile liikumine ja [**Parooli lähtestamiseks tegevuse**](active-directory-passwords-get-insights.md#view-password-reset-activity) aruande kuvamisel.

On mitu võimalust registreeruda ja kasutada parool lähtestada oma asutuse kasutajate teatada.  Need on allpool.

### <a name="email-based-rollout"></a>E-postipõhise levitamise kohta.
Võib-olla lihtsaim lähenemine Teavitage kasutajaid sellest Registreeruge või kasutage parooli lähtestamiseks on neid selleks juhendamine meili saatmisega.  Allpool on malli abil saate seda teha.  Julgelt asendada värvide / logod need oma valimise seda vastavalt oma vajadustele kohandada.

  ![][001]

Saate [alla laadida meilisõnumi malli siin](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Oma parooli portaali loomine
Üks strateegia, mis toimib hästi suurte klientide haldusvõimalusi parooli juurutamisega on luua "parooli portal", mida kasutajad abil saate hallata kõike seotud paroolide ühes kohas.  

Paljud suurim klientide soovite luua juur DNS-i kirje, nagu https://passwords.contoso.com koos linkidega Azure AD parooli lähtestamiseks portaali, parooli lähtestamise registreerimise portaali ja parooli muutmine lehti.  Sellisel viisil, mis tahes e-posti suhtluse või fliers saadate, saate lisada ühekordne, meeldejääv, URL, mida kasutajad saate minna, kui nad on teine teenuse kasutamist alustada.

Alustamine siin, et olete loonud lihtsa lehte, mis kasutab Viimane reageeri Kasutajaliidese kujundus paradigmad, ja töötavad kõik brauserid ja mobiilseadmete jaoks.

  ![][007]

Saate [alla laadida veebisaidi malli siin](https://github.com/kenhoff/password-reset-page).  Soovitame, logo ja värvide teie ettevõtte vajadustele kohandada.

### <a name="using-enforced-registration"></a>Jõustatud registreerimise abil
Kui soovite kasutajate parooli lähtestamise ise registreerida, saate jõustada ka need registreerida, kui juurdepääsu paneeli [http://myapps.microsoft.com](http://myapps.microsoft.com)sisselogimist.  See suvand, kui teie kataloogi **konfigureerimine** menüü võimaldab **Register Accessi paneeli sisselogimisel kasutajate nõua** suvandi lubamine.  

Soovi korral ka saate määratleda, kas teil palutakse, muutes **päevade enne kasutajad peavad kinnitage oma kontaktandmed** võimalus on nullist erinev väärtus konfigureeritav aja jooksul pärast uuesti registreerida. Lisateabe saamiseks vaadake [Kohandamine kasutaja parooli juhtimise käitumist](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Kui olete lubanud see suvand, kui kasutajad kasutada paneeli sisse logida, näevad nad popup, mis teavitab neid oma administraator peab neid kontrollida oma kontaktteavet. Need saate kasutada oma parooli lähtestamiseks, kui nad ei pääse oma kontole.

  ![][003]

Klõpsates **Kinnitamine nüüd** toob need on **parooli lähtestamise registreerimise portaali** [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) ja jaoks on vaja need registreerida.  See meetod kaudu saate lükata, klõpsates nuppu **Tühista** või sulgege aken, kuid meeles iga kord, kui nad sisselogimine, kui nad ei kuvata.

  ![][004]

### <a name="uploading-data-yourself"></a>Andmete üleslaadimine
Kui soovite andmete üleslaadimiseks, siis kasutajad on vaja Registreeri parooli lähtestamise enne paroolide lähtestamiseks.  Kui kasutajad on autentimise andmete sisse oma konto, mis vastab parooli lähtestamine olete määratlenud poliitika, siis need kasutajad saaksid oma parooli lähtestada.

Siit saate teada, millised atribuudid AAD ühenduse või Windows PowerShelli kaudu saate määrata, leiate [andmed, mida kasutatakse parooli lähtestamine](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Autentimise andmete [Azure'i haldusportaal](https://manage.windowsazure.com) kaudu saate üles laadida, järgides allolevaid juhiseid:

1.  Liikuge kataloogi **Active Directory laiend** [Azure'i haldusportaal](https://manage.windowsazure.com).
2.  Klõpsake vahekaardil **Kasutajad** .
3.  Valige kasutaja, olete huvitatud loendist.
4.  Klõpsake esimese menüü leiate **Alternatiivne meiliaadress**, mida saab kasutada atribuudi Luba parooli lähtestamine.

    ![][005]

5.  Klõpsake vahekaardil **Teave töö** .
6.  Sellelt lehelt leiate **Office telefon**, **mobiiltelefon**, **Autentimine telefon**ja **Autentimine e-posti**.  Neid atribuute saab määrata ka lubada kasutajal tema enda parool lähtestada.

    ![][006]

Näha, [millised andmed on kasutatavad parooli lähtestamine](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) näha, kuidas saab kasutada iga neid atribuute.

Vaadake, [Kuidas pääseda juurde parooli lähtestamine kasutajate PowerShelli andmete](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) näha, kuidas saate lugeda ja seadke selle PowerShelliga.

## <a name="sample-training-materials"></a>Valimi koolitusmaterjalid
Me tegeleme valimi koolitusmaterjalid, mille abil saate kiiresti ettevõtte IT ja kasutajate Täiskiirusele juurutada ja kasutada parooli lähtestamise kohta.  Hoia!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Linkide parooli lähtestamiseks dokumentatsioon
Allpool on lingid kõik lehed Azure AD parooli lähtestamist dokumentatsiooni:

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
* [**Kuidas see toimib**](active-directory-passwords-how-it-works.md) - teave kuus erinevate osade teenuste, mida iga ei
* [**Alustamine**](active-directory-passwords-getting-started.md) – saate teada, kuidas võimaldavad kasutajatel lähtestamine ja pilveteenuse või kohapealse parooli muutmine
* [**Kohanda**](active-directory-passwords-customize.md) – siit saate teada, kuidas kohandada ilme ja olemus ja käitumine teenuse teie asutuse vajadustest
* [**Saada ülevaadet**](active-directory-passwords-get-insights.md) - Lisateavet meie integreeritud aruandlusvõimalused
* [**FAQ**](active-directory-passwords-faq.md) – siin on vastused korduma kippuvatele küsimustele
* [**Tõrkeotsing**](active-directory-passwords-troubleshoot.md) – saate teada, kuidas kiiresti teenuse probleemide tõrkeotsing
* [**Lisateavet leiate teemast**](active-directory-passwords-learn-more.md) -valige suure tehnilise üksikasjadesse teenuse tööpõhimõtted



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
