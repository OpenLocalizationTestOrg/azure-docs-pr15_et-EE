<properties
    pageTitle="Azure'i AD liitumine seadistamine kasutajate jaoks | Microsoft Azure'i"
    description="Selgitatakse, kuidas administraatorid saavad häälestada Azure AD liitumine kohapealse kataloogi ja seadme registreerimine."
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
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Azure'i AD liitumine häälestamine teie asutuse

Enne häälestamist Azure Active Directory liitumine (Azure'i AD liituda), peate kas sünkroonida kasutajate pilveteenusesse kohapealse kataloogi puhastama või käsitsi luua hallatavate kontode Azure AD.

Üksikasjalikud juhised sünkroonimise kohapealse kasutajatele Azure AD käsitletakse [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).


Käsitsi luua ja hallata kasutajate Azure AD, vaadake [kasutajahalduse Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Seadme registreerimine häälestamine
1. Logige administraatorina sisse Azure portaali.
2. Klõpsake vasakpoolsel paanil nuppu **Active Directory**.
3. Valige vahekaardil **Directory** kataloogi.
4. Valige vahekaart **konfigureerimine** .
5. Avage jaotis **seadmed** .
6. Klõpsake vahekaarti **seadmed** määrata järgmist.  
   * **Suurim lubatud arv, seadmed kasutaja kohta**: Valige kasutaja võib olla Azure AD seadmete maksimaalne arv.  Kui kasutaja jõuab selle piirmäära, nad ei saa lisada täiendavad seadmed kuni ühe või mitme oma olemasolevaid seadmeid eemaldatakse.
   * **NÕUA mitme TEGURI AUTH LIITUMISE seadmete**: määrata, kas kasutajad peavad autentimise teine oma seadme ühendamiseks Azure AD esitama. Azure'i Mitmikautentimise kohta leiate lisateavet teemast [Azure Mitmikautentimise pilveteenuses töötamise alustamine](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Kasutajad võivad AZURE'I AD-ühendus seadmed**: valige kasutajad ja rühmad, seadmete ühendamiseks Azure AD lubatud.
   * **Täiendavad administraatorid sees AZURE AD ühendatud seadmete**: Azure'i AD Premium või ettevõtte mobiilsus komplekti (EMS), saate valida, millistel kasutajatel on antud seadme kohaliku administraatori õigused. Globaalse administraatorid ja seadme omanikud antakse vaikimisi kohaliku administraatori õigusi.

<center>![Seadme regisration häälestamine](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Kui olete häälestanud Azure AD liitumine kasutajate jaoks, nad saavad ühenduse Azure AD oma ettevõtte või personali seadmete kaudu.

Järgnevalt on kõigi kolme stsenaariumi puhul, saate kasutajad saaksid häälestada Azure AD liituda.

- Kasutajate liituma ettevõttel omanik seade otse Azure AD.
- Kasutaja domeeni-Liitu kohapealse Active Directory ettevõttel omanik seade ja seejärel hõlmavad Azure AD seade.
- Kasutajate lisamine töö-või koolikonto Windowsi isiklik seade

## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
