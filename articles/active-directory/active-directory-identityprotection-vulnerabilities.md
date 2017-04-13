<properties
    pageTitle="Nõrkade kohtade avastada Azure Active Directory identiteedi kaitse | Microsoft Azure'i"
    description="Ülevaade kohtade, avastada Azure Active Directory identiteedi kaitse."
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, pilveteenuse rakenduse discovery, rakendused, Turve, risk, taseme, haavatavuse, turbepoliitika haldamine"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Nõrkade kohtade avastada Azure Active Directory identiteedi kaitse 

Nõrkade kohtade on teie keskkonnas, kus saate ära ründaja nõrkuste. Soovitame järgmiste nõrkade kohtade parandamiseks turvalisus asendi teie ettevõtte aadress ja vältida ründajad nende kasutamiseks.
<br><br>
![nõrkade kohtade](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

Järgmistes jaotistes teile soovitud kohtade identiteedi kaitse esitatud ülevaade.

## <a name="multi-factor-authentication-registration-not-configured"></a>Mitmikautentimise registreerimise pole konfigureeritud 

See probleem abil saate reguleerida Azure'i Mitmikautentimise kasutuselevõtt teie asutuses. 

Azure'i mitmikautentimise pakub teine kiht kasutaja autentimisega turvalisus. See aitab tagada juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal. See pakub tugeva autentimise kaudu mitmesuguseid võimalusi lihtne kinnitamine – telefonikõne, tekstsõnumi või mobiilirakenduses teatist või kinnitamine koodi ja 3. tootja VANDE sõned.

Soovitatav on nõuda Azure'i Mitmikautentimise kasutaja sisselogimist. Mitmikautentimise on võtme roll juurdepääsu riski põhjal poliitikate identiteedi kaitse kaudu.

Lisateavet leiate teemast [mis on Azure Mitmikautentimise?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Mittehallatavate pilve rakendused

See probleem aitab tuvastada mittehallatava pilve rakendused teie asutuses.
 
Tänapäevane ettevõtetes IT-osakondade sageli teadlikud cloud kõik rakendused, mida kasutate oma asutuse kasutajate oma tööd teha. On lihtne mõista, miks administraatorid oleks muret volitamata juurdepääsu ettevõtte andmeid, võimalike andmeleket ja muud ohtu turvalisusele. 

Soovitame, et teie ettevõttes juurutada pilveteenuse rakenduse Discovery avastada mittehallatava pilv rakendusi ja Azure Active Directory rakenduste haldamiseks.

Leiate üksikasjalikumat teavet teemast [otsimise mittehallatava cloud rakenduste pilveteenuse rakenduse Discovery](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Turvaviipu õigustega identiteetide haldus

See probleem aitab tuvastada ja lahendada õigustega identiteedid ettevõtte kohta teatiste saamiseks.  

Kasutajate õigustega toiminguid lubamiseks peavad organisatsioonid kasutajate püsivate või ajutiste õigustega juurdepääsu andmiseks Azure AD, Azure'i või Office 365 ressursside või muud SaaS rakendused. Kõik need õigustega kasutajad suurendab rünnak pind teie asutuses. See probleem aitab tuvastada kasutajate mittevajalike õigustega juurdepääsu, ja võtma asjakohaseid vähendada või kõrvaldada seotud ohtu. 

Soovitame, et teie asutus kasutab Azure AD õigustega identiteetide haldamisviis haldamiseks valige juhtelement, ja kuvari au identiteedid ja nende ressursside Azure AD juurdepääsu kui ka muude Microsofti veebiteenuste nagu Office 365 või Microsoft Intune'i.

Lisateavet leiate teemast [Azure AD õigustega identiteetide haldus](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Vt ka

 - [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md)
