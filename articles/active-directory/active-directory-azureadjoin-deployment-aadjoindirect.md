<properties
    pageTitle="Kasutamise stsenaariumid ja juurutamise kaalutluste kohta Azure AD liitumine | Microsoft Azure'i"
    description="Selgitatakse, kuidas administraatorid saavad häälestada Azure AD liitumine lõppkasutajatele (töötajad, õpilaste, teised kasutajad). See käsitletakse ka reaalseid stsenaariumit kasutamise Azure AD liitumine."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< sildid ms.service= "active directory" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Kasutamise stsenaariumid ja juurutamise kaalutluste kohta Azure AD liitumine

## <a name="usage-scenarios-for-azure-ad-join"></a>Azure'i AD liitumine jaoks kasutamise stsenaariumid
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Stsenaarium 1: Ettevõtetele suuresti pilveteenuses

Azure Active Directory liitumine (Azure'i AD liituda) saavad teile praegu Sky ja identiteedid haldamine pilveteenuses teie ettevõtte või pilveteenusesse kiiresti teisaldada. Konto, mille olete loonud Azure AD Windows 10 sisselogimiseks saate kasutada. [Esimese Käivita kogemus (FRX) abil](active-directory-azureadjoin-user-frx.md), või liitumise Azure AD [menüü sätted](active-directory-azureadjoin-user-upgrade.md)kaudu, saate kasutajate liitumine oma masinad Azure AD.  Kasutajad saavad nautida ühekordse sisselogimise (SSO) juurdepääsu cloud ressursse nagu Office 365, brauseri või Office'i rakenduste.

### <a name="scenario-2-educational-institutions"></a>Stsenaarium 2: Haridusasutustele

Haridusasutustes on tavaliselt kasutaja kahte tüüpi: õppejõududele ja õppuritele. Õppejõud peetakse organisatsiooni pikaajalise liikmed. Soovitatav on luua kohapealsete kontode neid. Kuid õpilased on organisatsiooni lühemaajalist liikmed ja nende kontod saab hallata Azure AD. See tähendab, et kataloogi skaala saate selle asemel et salvestatud kohapealse pilveteenusesse lükata. Ka tähendab see, et õpilased on võimalik Windows Azure AD oma kontoga sisselogimine ja Office 365 ressursside Office'i rakendustes saate.

### <a name="scenario-3-retail-businesses"></a>Stsenaarium 3: Jaemüügi ettevõtetele

Jaemüügi ettevõtetel on hooajalised töötajad ja pikaajalised töötajad. Tavaliselt kohapealsete kontode loomine ja domeeni ühendatud masinad pikaajalise täiskohaga töötajate jaoks kasutada. Kuid hooajaline töötajatel organisatsiooni lühemaajalist liikmed ja on soovitatav hallata oma kontot, kus kasutajalitsentside saab kergemini liigutada. Kasutajakonto loomisel pilveteenuses teenusekomplektiga Office 365 litsentside need kasutajad saavad eelised sisselogimine Windowsi ja Office'i rakenduste Azure AD konto puhul samal ajal, kui haldate rohkem paindlikkust koos oma litsentside pärast lahkumist.

### <a name="scenario-4-additional-scenarios"></a>Stsenaarium 4: Täiendavad stsenaariumid

Koos eespool kasu, mida on kasulik kasutajate liitumine tema seadmetes Azure AD tõttu on lihtsustatud liitumist kogemus, tõhusa seadmehalduse, automaatne mobiilsideseadme halduse registreerimine ja ühekordse sisselogimise Azure AD ja kohapealsete ressursid.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Juurutamise kaalutluste kohta Azure AD liitumine

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Lubada kasutajatel liituda ettevõttel omanik seade otse Azure AD


Suurettevõtete saate sisestada vaid kontod partnerid ja organisatsioonidele. Need partnerid hõlpsalt pääseb ettevõtte rakendused ja ühekordse sisselogimise ressursse. See stsenaarium kehtib kasutajatele, kellel on juurdepääs ressursid peamiselt pilves, nt Office 365 või SaaS Azure AD autentimiseks kasutavad rakendused.

### <a name="prerequisites"></a>Eeltingimused
**Ettevõtte tasemel (administraator)**

*   Azure'i tellimuse Azure Active Directory  

**Kasutaja tasemel**

*   Windows 10 (Professional väljaanded)

### <a name="administrator-tasks"></a>Administraatori ülesannete
* [Seadme registreerimine häälestamine](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Kasutaja ülesanded
* [Uue Windows 10 seadmesse Azure AD häälestamise ajal häälestamine](active-directory-azureadjoin-user-frx.md)
* [Menüü Sätted kaudu Windows 10 seadmesse Azure AD häälestamine](active-directory-azureadjoin-user-upgrade.md)
* [Liitumine teie organisatsioonile isikliku Windows 10 seadmesse](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>Luba BYOD ettevõtte Windows 10 jaoks
Saate häälestada oma kasutajate ja töötajate oma isikliku Windowsi seadmed (BYOD) juurdepääsu ettevõtte rakendused ja ressursse. Kasutajaid saate lisada oma Azure AD kontod (töö- või koolikonto kontod) isikliku Windowsi seade juurdepääs ressurssidele turvaline ja nõuetele.

### <a name="prerequisites"></a>Eeltingimused
**Ettevõtte tasemel (administraator)**

*   Azure'i AD tellimus

**Kasutaja tasemel**

*   Windows 10 (Professional väljaanded)


### <a name="administrator-tasks"></a>Administraatori ülesannete

* [Seadme registreerimine häälestamine](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Kasutaja ülesanded
* [Liitumine ettevõttesse isikliku Windows 10 seadmesse](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Parooli Microsoft Passporti kaudu ilma autentimine](active-directory-azureadjoin-passport.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
