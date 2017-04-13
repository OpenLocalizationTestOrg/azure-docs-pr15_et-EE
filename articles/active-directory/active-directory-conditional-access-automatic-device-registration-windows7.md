<properties
    pageTitle="# Automaatse seadme registreerimine Windows 7 ühendatud domeeni seadmete jaoks konfigureerida | Microsoft Azure'i"
    description="Windows 7 domeeni konfigureerimine juhiseid ühendatud seadmete automaatselt registreerimiseks Azure AD. ja juhiseid seadme registreerimine tarkvarapaketi juurutamiseks domeeni Windows 7 ühendatud seadmete tarkvara jaotuse süsteemi nagu System Center Configuration Manager abil."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Automaatse seadme registreerimine Windows 7 ühendatud domeeni seadmete jaoks konfigureerimine

Kui IT-administraator, saate konfigureerida seadmetes Windows 7 ühendatud domeeni automaatselt registreerimiseks Azure AD. Seda teha, peab seadme registreerimine tarkvarapaketi juurutamiseks domeeni Windows 7 ühendatud seadmete tarkvara jaotuse süsteemi nagu System Center Configuration Manager abil. Kindlasti lugege läbi ja täitke automaatne seadme registreerimine Azure Active Directory jaoks Windowsi domeeni-ühendatud seadmete loetletud eeltingimused.

>[AZURE.NOTE]
 Uusima juhised, kuidas häälestada automaatse seadme registreerimine teemadest, [Kuidas seadistada automaatse registreerimise Windowsi domeeni liidetud seadmete Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Seadme registreerimise tarkvarapaketi installimine Windows 7 domeeni ühendatud seadmete

Seadme registreerimine Windows 7 on [allalaaditav MSI pakett](https://connect.microsoft.com/site1164). Paketi peab olema installitud Windows 7 arvutites, mis on ühendatud Active Directory domeeni. Juurutamist tuleks kasutada tarkvara jaotuse süsteemi nagu System Center Configuration Manager. MSI pakett toetab standard vaikne installisuvandid, kasutades selle Quiet parameeter.
Tarkvarapaketi on saadaval [veebisaidil Microsoft Connect](https://connect.microsoft.com/site1164)alla. Siin saate valida ja seejärel alla laadida töökoha liitumine Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Töökoha Liitu Azure Active Directory
Windows 7 ühendatud domeeni seadmete jaoks seadme registreerimine ei nõua või sisalda kasutajaliidest. Kui arvutisse on installitud, mis tahes domeeni kasutaja, et arvutisse sisse logib automaatselt ja vaikselt registreeritakse seadme objekti Azure AD. Seal on üks seadme objekti Azure AD iga seadme registreeritud kasutaja.

Installiprogrammi loob ajastatud ülesanne süsteemi, mis töötab kasutaja konteksti ja käivitab kasutaja sisselogimine kohta. Tööülesande vaikselt registreerib kasutaja ja Azure AD pärast kasutaja märgid sisse seade on lõpule viidud.
Ajastatud ülesanne leiate selle Toiminguajasti teek jaotises **Microsoft** > **Töökoha liituda**.
Tööülesande kestab ja registreerida kõik Active Directory kasutajad, mis on arvutisse sisse logima.
Järgmisel joonisel on loetletud automaatse seadme registreerimine üksikasjalik protsess.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Windows 7 klientarvutisse Active Directory domeeni mandaadiga logib kasutaja (töötaja teave).
1. Töökoha liitumine Ajastatud ülesanne on täidetud.
1. Kasutaja autenditakse vaikselt AD FS integreeritud Windowsi autentimist kasutades.
1. Windows 7 Arvutis on registreeritud kasutajale Azure AD.
1. Seadme objekti ja serdi luuakse Azure AD. Objekti tähistab selle user@device.
1. Töökoha liitumine sert on talletatud arvutis.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Windows 7 domeeni registreerimise tühistamine liitunud seadmed

Võite valida unregister oma domeeni liitunud Windows 7 seadmed, tehes järgmist: desinstallida töökoha liitumine tarkvarapaketi Windows 7 domeenist liitunud tarkvara jaotuse süsteemi nagu System Center Configuration Manager kasutavates seadmetes.

Seejärel avage Windows 7 arvutis Käsuviip ja käivitage järgmine käsk seadme registreerimise tühistamine.

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>See käsk peate käivitama iga kasutaja domeeni, mis on allkirjastatud kontekstis kohale.
Sündmusevaatur ja vigade Windows 7 domeeni ühendatud seadmete.

Windowsi sündmuselogi Windows 7 arvutis kuvatakse sõnumid, mis on seotud töökoha liituda. Sõnumite leiate eduka nii õnnestu töökoha liitumine sündmuste jaoks. Sündmuste logi võib leida sündmuse vaatur jaotises rakenduste ja teenuste logid > Microsoft-töökoha liituda.

## <a name="additional-topics"></a>Täiendavad Teemad

- [Azure Active Directory seadme registreerimine ülevaade](active-directory-conditional-access-device-registration-overview.md)
- [Automaatse seadme registreerimine Azure Active Directory for Windows Domain-Joined seadmete](active-directory-conditional-access-automatic-device-registration.md)
- [Automaatse seadme registreerimine Windows 8.1 domeeni ühendatud seadmete jaoks konfigureerimine](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automaatse seadme registreerimine Azure Active Directory Windows 10 jaoks domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
