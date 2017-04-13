<properties
    pageTitle="Uue seadme häälestamise ajal Azure AD häälestamine | Microsoft Azure'i"
    description="Teema, mis selgitab, kuidas kasutajad saavad häälestada Azure AD liitumine Käivita esmakasutuse ajal."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Uue seadme häälestamise ajal Azure AD häälestamine

Opsüsteemis Windows 10 kasutajad kutse tema seadmetes, et Azure Active Directory (Azure AD) esmakasutuse (FRX). See võimaldab asutustel levitada termokahaneva seadmete töötajatele või õpilaste või temaga valige oma seadmed (CYOD).
Kui seadmes on installitud nii Windows 10 Professional või Windows 10 Enterprise väljaanded, kogemusi vaikimisi seadmetes ettevõtte jaoks häälestamise protsess.

## <a name="to-join-a-device-to-azure-ad"></a>Liituda mõne seadme Azure AD


1. Uude seadmesse sisse lülitada ja häälestamise alustamiseks, peaksite nägema **Valmis saada** sõnum. Järgige viipasid oma seadme häälestamiseks.
2. Alustuseks kohandada oma piirkond ja keel. Aktsepteerige Microsofti tarkvara litsentsitingimused.
![Teie piirkonnas kohandamine](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Valige võrk, mida soovite kasutada Interneti-ühenduse loomiseks.
4. Valige, kas kasutate isiklikku seadet või seadme ettevõttel omanik. Kui see on ettevõttel omanik, klõpsake **seade kuulub minu asutust**. Käivitub Azure AD liitumise võimalusi. Järgmine on ekraan, et näete, kui kasutate Windows 10 Professional.
<center>
![Kellele see arvuti ekraan](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Sisestage mandaat, mis on teie asutuses.
<center>
![Sisselogimiskuva](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Kui olete sisestanud oma kasutajanimi, asub kattuvad rentniku Azure AD. Kui olete ühendatud domeenis, suunatakse teie kohapealse Secure Turbeloa teenus (STS) server--näiteks Active Directory Federation Services (AD FS).
7. Kui olete kasutaja ühendamata domeenis, sisestage oma kasutajanimi ja parool otse lehel Azure'i AD-majutatud. Kui ettevõtte sümboolika on konfigureeritud, saate ka vaadata oma ettevõtte logo ja toeta teksti.
8.  Teil palutakse probleemiks mitmikautentimise jaoks. See probleem on konfigureeritav IT-administraator.
9.  Azure AD kontrollib, kas kasutaja/eeldab kuulumine mobiilsideseadmete haldus.
10. Windowsi registrite seadme ettevõtte nimistust Azure AD ja enrolls see mobiilsideseadmete haldus, vajaduse korral.
11. Kui olete hallatava kasutaja, Windows suunab teid läbi automaatse protsessi töölaua.
12. Kui olete ühendatud kasutaja, suunatakse teid sisestama oma mandaadi Windows sisselogimiskuval.

> [AZURE.NOTE] Liitumine Windows out box kogemus kohapealse Windows Server Active Directory domeenis ei toetata. Seetõttu, kui plaanite liituda arvuti domeeniga, peaksite valima link **Windowsi kohalik konto häälestamine** selle asemel. Seejärel saate liituda domeeni sätted teie arvutis, kui olete teinud.

## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Parooli Microsoft Passporti kaudu ilma autentimine](active-directory-azureadjoin-passport.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
