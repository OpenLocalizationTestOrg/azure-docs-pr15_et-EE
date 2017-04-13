<properties
    pageTitle="Häälestamine Windows 10 seadmesse Azure AD sätted | Microsoft Azure'i"
    description="Selgitatakse, kuidas kasutajad saavad liituda Azure AD menüü Sätted kaudu."
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Häälestamine Windows 10 seadmesse Azure AD sätted
Kui kasutate juba Windows 7 või Windows 8 ja oma arvutis või seadmes on täiendatud versioonile Windows 10, võite menüü Sätted kaudu liituda Azure Active Directory (Azure AD).

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Azure AD menüü Sätted kaudu liituda


1. Klõpsake menüü **Start** kaudu tuumnupu **sätted** .
2. Valige menüü **sätted**, valige **süsteem**->**kohta**->**Azure AD liituda**.
<center>
![Azure AD menüü Sätted kaudu liituda](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Klõpsake nuppu **Jätka** sõnumiaknas Azure AD liituda.
<center>
![Azure'i AD-ühendus sõnumiaknas](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Sisestage oma mandaat. See sisselogimine sisaldab täieliku autentimisega vajalike juhiseid. Kui teil on väline rentniku osa, annab teie administraator teile majutatud ettevõttes kogemusega federation.
<center>
![Sisestage sisselogimise identimisteave](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Kui teie asutus on konfigureeritud Azure Mitmikautentimise liitumist Azure AD, sisestage teine enne jätkamist.
6. **Luba selle seadme hallatakse** kuval nuppu **Aktsepteeri** .
7. Peaksite nägema sõnum "seadme on nüüd ühendatud ettevõtte Azure AD".


## <a name="additional-information"></a>Lisateave
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
* [Parooli Microsoft Passporti kaudu ilma autentimine](active-directory-azureadjoin-passport.md)
