<properties
    pageTitle="Azure'i AD-ühenduse ja Federation | Microsoft Azure'i"
    description="See leht on keskne koht kõik dokumendid AD FS-i toimingute Azure'i AD-ühenduse kasutamise kohta"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure'i AD-ühendus ja federation

Azure'i AD-ühendus võimaldab teil koos asutusesisese ühinemise konfigureerimine AD FS-i ja Azure AD. Federation Logi sisse, saate lubada kasutajatel sisse logida vastavalt Azure AD Services kohapealse paroolide ja samal ajal ettevõtte võrgus, ilma et peaksite oma parooli uuesti. AD FS liiduserveri suvandi võimaldab kasutuselevõtuks uue või olemasoleva AD FS Windows Server 2012 R2 serveripargi määrata.

See teema on teavet Federation Avaleht seotud funktsioonid Azure'i AD-ühenduse ja loendite linkide kõik muud teemad, mis on sellega seotud. Azure'i AD-ühenduse lingid, leiate oma kohapealse identiteetide integreerimine Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure'i AD-ühendus - federation Teemad

| Teema: | Mida see hõlmab ja millal lugemine |
|:------|:-----------|
| **Azure'i AD-ühendus kasutaja sisselogimise suvandid** ||
| [Kasutaja sisselogimise suvandid mõistmine](active-directory-aadconnect-user-signin.md) | Erinevate kasutajate sisselogimist suvandite kirjeldusi ja kuidas need mõjutavad Azure kasutaja kogemus |
| **AD FS-i installimise Azure'i AD-ühenduse kaudu**||
| [Eeltingimused](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Et AD FS-i installi õnnestumise Azure'i AD-ühenduse kaudu|
| [Serveripargi AD FS-i konfigureerimine](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Kuidas installida uue AD FS-i pargi Azure'i AD-ühenduse kaudu |
| **AD FS-i konfiguratsiooni** | |
| [Usalda parandamine](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Kuidas parandada praeguse usalda vahel kohapealse AD FS-i ja Office 365 ja Azure |
| [Lisades uue AD FS server](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | AD FS-i serveriparki täiendavad AD FS server postituse algse installi laiendamine |
| [Lisades uue AD FS WAP server](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | AD FS-i serveripargi täiendavad WAP serveri postituse algsel installimisel laiendamine |
| [Uue välise domeeni lisamine](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Lisada mõne muu domeeni olema ühendatud Azure AD |
|**Postituse installi tööülesanded**||
| [Kohandatud ettevõtte logo lisamine / illustratsioon](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Kohandatud logo, mis kuvatakse sisselogimise lehel AD FS-i määramisega soovitud sisselogimine muutmine |
| [Sisselogimise kirjelduse lisamine](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Sisselogimise kirjeldus sisselogimise lehel AD FS-i muutmine | 
| [Muutmine AD FS-i taotlemine reeglid](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Muuta / lisada nõude reeglid AD FS vastav Azure'i AD-ühenduse sünkroonimise konfigureerimine |


## <a name="additional-resources"></a>Lisaressursid

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
* [Azure'i AD FS-i juurutamine](active-directory-aadconnect-azure-adfs.md)
* [Kõrge-saadavus-geograafiliste AD FS-i juurutamist Azure Azure'i liikluse haldur](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


