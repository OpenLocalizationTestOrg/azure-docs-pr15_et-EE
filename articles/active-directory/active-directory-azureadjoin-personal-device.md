

<properties
    pageTitle="Liitumine teie organisatsioonile isiklik seade | Microsoft Azure'i"
    description="Selgitatakse, kuidas saab kasutajaid registreerida isikliku Windows 10 seadmeid oma ettevõtte võrku ja juurutamise juhised BYOD stsenaariumi."
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

# <a name="join-a-personal-device-to-your-organization"></a>Isiklik seade teie organisatsioonile liitumine

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Windows 10 seadmesse teie organisatsioonile liituda

1.  Menüü **Start** käsk **sätted**.
2.  Valige **kontod**ja seejärel klõpsake nuppu **konto**.
3.  Klõpsake **lisamine töö või kooli konto**ja seejärel tippige ettevõtte konto.
4.  Lehel sisselogimine oma ettevõtte jaoks, sisestage oma kasutajanimi ja parool ja seejärel klõpsake nuppu **OK**.
5.  Teil palutakse mitmikautentimise ülesanne. (Selle probleemi lahendamiseks on konfigureeritav IT-administraator).
6.  Azure Active Directory (Azure AD) kontrollib, kas seade nõuab mobiilsideseadme halduse kuulumist.
7.  Windowsi registrite seadme ettevõtte nimistust Azure AD ja enrolls see mobiilsideseadmete haldus, vajaduse korral.
8.  Kui olete hallatava kasutaja, Windows suunab teid töölaua kaudu automaatne Logi sisse.
9.  Kui olete ühendatud kasutaja, saate võetakse kuvale Windowsi sisselogimiseks sisestage oma kasutajanimi ja parool.

## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Parooli Microsoft Passporti kaudu ilma autentimine](active-directory-azureadjoin-passport.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
