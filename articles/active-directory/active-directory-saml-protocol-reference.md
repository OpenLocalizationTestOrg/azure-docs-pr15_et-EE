<properties
    pageTitle="Azure'i AD SAML protokolli viide | Microsoft Azure'i"
    description="Selles artiklis antakse ülevaade Azure Active Directory ühekordse sisselogimise ja ühe Sign-Out SAML profiilid."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Kuidas kasutab Azure Active Directory SAML Protocol (protokoll)

Azure Active Directory (Azure AD) kasutab SAML 2.0 protokolli lubamiseks rakenduste kasutajatele ühekordse sisselogimise teenuse pakkumiseks. [Ühekordse sisselogimise](active-directory-single-sign-on-protocol-reference.md) ja [ühe väljunud](active-directory-single-sign-out-protocol-reference.md) SAML profiilid Azure AD selgitavad, kuidas kasutada SAML kinnitused, protokollid ja sidumiste identiteedi pakkuja teenus.

Identiteedipakkuja (Azure AD) ja teenuse pakkuja (rakendus) enda kohta teabe vahetamiseks on vaja SAML Protocol (protokoll).

Rakenduse registreerimisel Azure AD registreerib rakenduse arendaja Azure AD federation seotud teavet. See hõlmab **URI ümber suunata** ja **Metaandmete URI** taotluse.

Azure AD kasutab **Metaandmete URI** pilveteenusesse, toomiseks allkirjastamiseks võti ja välja logimine URI pilveteenusesse. Kui rakendus ei toeta mõnda metaandmete URI, arendaja peab pöörduge Microsofti klienditoe poole esitada URI välju ja klahvi sisselogimine.

Azure Active Directory seab rentniku kohased ja levinud (rentniku sõltumatul) ühekordse sisselogimise ja ühe väljunud lõpp-punktid. Idest tähistada adresseeritavad asukohad--ei ole lihtsalt identifikaatorid – nii saate minna lõpp-punkti lugeda metaandmeid.

 - Rentniku kohased lõpp-punkti asub `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Funktsiooni <TenantDomainName> kohatäide TenantID GUID Azure AD rentniku või registreeritud domeeninimi. Näiteks contoso.com tenant metaandmete Domeeniliit on: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Rentniku sõltumatul lõpp-punkti asub `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Lõpp-punkti aadressis **levinud** , asemel kuvatakse rentniku domeeninime või ID-ga.

Federation metaandmete dokumendid, mis avaldab Azure AD kohta leiate teavet teemast [Federation metaandmete](active-directory-federation-metadata.md).
