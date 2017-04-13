<properties
   pageTitle="Azure'i AD-ühendus: Kujundada põhimõtet | Microsoft Azure'i"
   description="Selles teemas üksikasjad teatud rakendamist kujundus ala"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure'i AD-ühendus: Kujundada mõisted
Selles teemas eesmärki on valdkondi, mille ajal Azure'i AD-ühenduse kujunduse rakendamine tuleb läbi mõelda. See teema on teatud alade sügav sukelduda ja need lühikirjeldused ka muud teemad.

## <a name="sourceanchor"></a>sourceAnchor
SourceAnchor atribuut on määratletud *atribuudi püsiv kestel ning objekti*. See määratleb kordumatult sama objekti asutusesisese ja Azure AD objekti. Atribuut nimetatakse ka **immutableId** ja kahe nime on kasutatud vahetatavad.

Sõna püsiv, mis on "ei saa muuta", on oluline teema. Kuna selle atribuudi väärtus ei saa muuta, kui see on seatud, on oluline, valige kujundus, mis toetab teie stsenaariumi.

Atribuuti kasutatakse järgmistel juhtudel:

- Kui uus sünkroonimine engine server on ehitatud või ehitada pärast katastroofi taastamise stsenaarium, lingid atribuudi objektid objektide kohapealse Azure AD.
- Kui teisaldate vaid identiteedi sünkroonitud identiteedi mudeli, siis selle atribuudi võimaldab "raske vaste" objektid objektide Azure AD kohapealse objektidega.
- Kui kasutate federation, siis selle atribuudi **userPrincipalName** koos kasutatakse nõude kasutaja tuvastamiseks.

Selles teemas ainult räägib sourceAnchor, et kasutajad. Samu reegleid rakendada kõik objekti tüübid, kuid see on ainult kasutajatele see probleem on tavaliselt probleemiks.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Hea sourceAnchor atribuut valimine
Atribuudi väärtus peab tehke järgmised reeglid.

- Olla kuni 60 märki
    - Märgid, mis ei ole a – z, A – Z või 0 – 9 kodeeritud ja lugeda 3 märki
- Ei sisalda erimärgi: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ (< >) '; : , [ ] " @ _
- Peab olema kordumatu globaalselt
- Peab olema stringi, täisarv, või kahendarvuks
- Põhinema pole kasutaja nimi, need muudatused
- Peaks olema tõstutundlik ja vältida väärtused, mis võivad erineda juhul
- Määratakse objekti loomisel

Kui valitud sourceAnchor pole string-tüüpi, siis Azure'i AD-ühenduse Base64Encode atribuudi väärtus ei erimärgid tagamaks kuvatakse. Kui kasutate muu liiduserveri kui ADFS-i, veenduge, et teie serveri, saate ka Base64Encode atribuut.

SourceAnchor atribuut on tõstutundlikud. Väärtusest "JohnDoe" pole sama, mis "johndoe". Kuid te ei peaks kahte erinevat objekti vahe ainult juhul.

Kui teil on ühe mets on asutusesiseselt, siis kasutage atribuuti **FederatedIdentity**. See on ka Azure'i AD-ühenduse ja ka kasutatavaid DirSync atribuut kiire sätete kasutamisel kasutatav atribuut.

Kui teil on mitu kogumit ja teisaldada kasutajate metsade ja domeenid, siis **FederatedIdentity** on hea atribuut kasutada ka sel juhul.

Kui teisaldate kasutajate metsade ja domeenid, siis tuleb leida atribuudile, mis ei muutu või saab teisaldada kasutajatega ajal liikuda. Soovitatav lähenemine on sünteetiline atribuut. Atribuuti, mis võiks korraldada midagi, mis näeb välja nagu GUID oleks sobiv. Objekti loomise ajal luuakse uue GUID ja kasutajale märgitud. Kohandatud sünkroonimise reegli loomist sünkroonimine engine serveris selle väärtuse alusel **FederatedIdentity** loomine ja värskendamine lisatakse atribuut on valitud. Objekti teisaldamisel veenduge, et see väärtus sisu kopeerida.

Teine lahendus on valida olemasoleva atribuudi teate, et muuta. Tavaliselt kasutatakse atribuutide kaasata **Tabeliseose**. Kui teie arvates atribuut, mis sisaldab tähtede, veenduge, et on võimalus täheregistrit (suurtähed vs väiketähed suurtähtedeks) saate muuta selle atribuudi väärtust. Vigased atribuudid, mis ei tohi kasutada kaasata neid atribuute kasutaja nimi. Abielu või lahutamise, eeldatakse nime muutmiseks, mis pole selle atribuudi lubatud. See on ka üks põhjus, miks atribuute, nagu **userPrincipalName**, **e-posti**ja **targetAddress** ei ole võimalik valimiseks installiviisardis Azure'i AD-ühenduse. Neid atribuute sisaldada ka selle @-character, mis pole lubatud selle sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>SourceAnchor atribuut muutmine
SourceAnchor atribuudi väärtus ei saa muuta, kui objekt on loodud Azure AD ja ID sünkroonitakse.

Seetõttu rakenduvad järgmised piirangud Azure'i AD-ühenduse:

- SourceAnchor atribuut, saate määrata ainult algsel installimisel. Kui te uuesti installimise viisard, see suvand on kirjutuskaitstud. Kui teil on vaja seda sätet muuta, tuleb desinstallida ja uuesti installida.
- Kui installite teise Azure'i AD-ühenduse server, peate valima sama sourceAnchor atribuut nagu varem kasutanud. Kui teil on varem kasutanud DirSync ja liikuda Azure'i AD-ühenduse, siis peate kasutama **FederatedIdentity** kuna see on kasutatavad DirSync atribuuti.
- Kui sourceAnchor väärtus on muutunud pärast eksportimist objekti Azure AD, siis Azure'i AD-ühenduse sünkroonimine põhjustab ilmnes tõrge ja ei luba veel muudatused tagasi allika kataloogis muudetakse objekti enne probleem on lahendatud ja selle sourceAnchor.

## <a name="azure-ad-sign-in"></a>Azure AD sisselogimine
Ajal kohapealse kataloogi integreerimise Azure AD, on oluline mõista, kuidas sätete sünkroonimine võib mõjutada viis kasutaja autendib. Azure AD kasutab autentida userPrincipalName (UPN). Juhul, kui sünkroonite kasutajaid, peate valima atribuudi userPrincipalName väärtus kasutada hoolikalt.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Atribuuti userPrincipalName valimine
Kui valite atribuut osutamiseks UPN-i kasutatakse Azure ühe väärtuse tagama

- Atribuut väärtused vastavad UPN-i süntaks (RFC 822), mis peaks olema formaat onusername@domain
- Järelliite väärtused vastab mõnele kinnitatud kohandatud domeenide Azure AD

Kiire sätted on eeldatud valik atribuudi userPrincipalName. Kui atribuut userPrincipalName ei sisalda väärtust soovite kasutajatele Azure'i sisse logida, siis peate valima **Kohandatud installi**.

### <a name="custom-domain-state-and-upn"></a>Kohandatud domeeni olek ja UPN-i
See on oluline veenduge, et on kinnitatud Domeen UPN-i järelliite jaoks.

John on kasutaja contoso.com. Soovite kasutada kohapealse UPN-i John john@contoso.com Azure sisse logida, kui teil on sünkroonitud kasutajad oma Azure AD directory contoso.onmicrosoft.com. Selleks, peate lisada ja kinnitada kohandatud domeeni contoso.com Azure AD enne sünkroonimist kasutajad. Kui UPN-i järelliite John, nt contoso.com ei sobi Azure AD kinnitatud domeen, siis Azure AD asendab UPN-i järelliite contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Mitte-marsruuditavaid kohapealse domeenid ja UPN-i jaoks Azure AD
Mõni ettevõte on-marsruuditavaid domeene, nt contoso.local või lihtsa Üksik silt domeenide näiteks contoso. Te ei saa Azure AD-marsruuditavaid domeeni kinnitamiseks. Azure'i AD-ühendus saab sünkroonida ainult kinnitatud Domeen Azure AD. Mõne Azure AD kataloogi loomisel luuakse marsruuditavaid domeenis, mis muutub vaikedomeen oma Azure AD näiteks contoso.onmicrosoft.com. Seetõttu tekkima vajadus kontrollida sel juhul marsruuditavaid domeene juhuks, kui te ei soovi sünkroonimiseks vaikimisi onmicrosoft.com domeeni.

Lugege lisateavet lisamise ja domeenid kinnitatava [lisada oma kohandatud domeeni nime Azure Active Directory](active-directory-add-domain.md) .

Azure'i AD-ühendus tuvastab, kui kasutate versiooni-marsruuditavaid domeenikeskkonna ja oleks õigesti Hoiata saate kiirsätteid alustamine. Kui töötate-marsruuditavaid domeeni, siis on tõenäoline, et selle kasutaja UPN-marsruuditavaid sufiksid liiga. Näiteks kui kasutate jaotises contoso.local, Azure'i AD-ühenduse soovitab saate kasutada kohandatud sätteid, mitte kiire sätete abil. Kasutades kohandatud sätteid, mida on võimalik määrata atribuut, mida tuleks kasutada UPN-i Azure sisse logida, kui kasutajad on sünkroonitud Azure AD.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
