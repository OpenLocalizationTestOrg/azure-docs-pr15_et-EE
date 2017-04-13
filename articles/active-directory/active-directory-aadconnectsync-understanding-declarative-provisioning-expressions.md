<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: mõistmine deklaratiivseid ettevalmistamise avaldiste | Microsoft Azure'i"
    description="Selgitatakse deklaratiivseid ettevalmistamise avaldised."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure'i AD-ühendus sünkroonimine: mõistmine deklaratiivseid ettevalmistamise avaldised
Azure'i AD-ühendus sünkroonimine põhineb deklaratiivseid ettevalmistamise esmakordselt Forefront Identity Manager 2010. See võimaldab teil rakendada oma täieliku identiteedi integreerimine äriloogika kompileeritud koodi kirjutamiseks vajamata.

Oluline osa deklaratiivseid ettevalmistamise on atribuut liikumine kasutatav avaldis keel. Keelt, mida kasutatakse on alamhulk Microsoft® Visual Basic® for Applications (VBA). See keel on kasutusel Microsoft Office ja kasutajate VBScript kogemusega ka tuvasta seda. Deklaratiivseid ettevalmistamise avaldis keel on ainult funktsioonide kasutamise ja ei ole struktureeritud keel. Puuduvad meetodite või laused. Selle asemel on pesastada kiire programmi liikumise.

Lisateavet leiate teemast [Tere tulemast Visual Basic for Applications keele teatmematerjalid Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Tungivalt tipitakse atribuute. Funktsiooni aktsepteerib ainult õiget tüüpi atribuudid. Samuti on tõstutundlikud. Nii funktsiooninimed ja atribuutide nimed peavad olema proper kest või tõrge Expression.error.

## <a name="language-definitions-and-identifiers"></a>Keele määratlused ja identifikaatorid

- Funktsioonide on nimi, millele järgneb argumendid sulgudes: funktsiooninime (argument 1, argumendi N).
- Atribuudid on tähistatud nurksulgudega: [attributeName]
- Parameetrite on tähistatud protsendimäärana märgid: ParameterName %
- Stringi konstandid on ümbritsetud jutumärkidega: näiteks "Contoso" (Märkus: tuleb kasutada Sirgjutumärgid "" ja ei nutikate jutumärkidega "")
- Arvväärtused on väljendatud ilma jutumärkideta ja peaks olema kümnendkohti. Kuueteistkümnendarvu väärtused on eesliide & H. Näiteks 98052 & HFF
- Kahendmuutujaga väärtused on väljendatud konstantidega: True, False.
- Sisseehitatud konstandid ja literaalide on väljendatud ainult oma nimi: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Funktsioonid
Deklaratiivseid ettevalmistamise kasutab paljude funktsioonide lubamine võimalusega atribuudi väärtust muuta. Need funktsioonid võivad olla pesastatud nii ühe funktsiooni tulem on edasi teise funktsiooni.

`Function1(Function2(Function3()))`

Funktsioonide täieliku loendi leiate [funktsiooni viide](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parameetrid
Parameeter on määratletud, konnektori või administraator PowerShelli kaudu. Parameetrite tavaliselt sisaldavad väärtusi, mis erinevad süsteemist teise, näiteks asub kasutaja domeeni nimi. Atribuut puhul saab kasutada järgmiste parameetrite.

Active Directory konnektor mõeldud sünkroonimise sissetulevad reeglid järgmiste parameetrite abil.

| Parameetri nimi | Kommentaari |
| --- | --- |
| Domain.Netbios | Domeeni praegu imporditud, näiteks FABRIKAMSALES NetBIOS vorming |
| Domain.FQDN | Domeeni praegu imporditud, näiteks sales.fabrikam.com FQDN vorming |
| Domain.LDAP | LDAP vorming domeeni praegu imporditud, näiteks AV = müük, näiteks Põhiliselt fabrikam AV = = com |
| Forest.Netbios | Mets nime praegu imporditud, näiteks FABRIKAMCORP NetBIOS vormingu |
| Forest.FQDN | Mets nime praegu imporditud, näiteks fabrikam.com FQDN vormingu |
| Forest.LDAP | LDAP vorming mets nime praegu imporditud, näiteks AV = fabrikam AV = com |

Süsteemi pakub järgmist parameeter, mis kasutatakse saada praegu töötavate konnektor ainuidentifikaator.  
`Connector.ID`

Siin on näide, mis kuvab metaversumi atribuut domeeni netbios domeeni, kus asub kasutaja nimi:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Tehtemärgid
Saab kasutada järgmisi tehtemärke.

- **Võrdlus**: <, < =, <>, =, >, > =
- **Matemaatika**: +, -, \*, -
- **Stringi**: & (concatenate)
- **Loogika**: & & (ja) || (või)
- **Tutvumist võimaldava järjestuses**:)

Tehtemärkide hinnatakse vasakule, paremale ja on sama hindamise prioriteet. Mis on selle \* (mitmekordne) ei väärtustata enne - (lahutamine). 2\*(5 + 3) pole sama, mis 2\*5 + 3. Kui vasakult paremale hindamise tellimuse pole sobiv hindamise järjestuse muutmiseks kasutatakse sulud ().

## <a name="multi-valued-attributes"></a>Mitme väärtusega atribuudid
Funktsioonide saate töötada nii ühe väärtusega ja mitme väärtusega atribuute. Mitme väärtusega atribuute, funktsioon töötab üle iga väärtuse ja funktsiooni sama kehtib iga väärtuse.

Näiteks:  
`Trim([proxyAddresses])`Tehke Trim Kasutajaatribuut atribuut iga väärtuse.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Iga väärtuse koos mõne @-sign, domeeni koos @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Otsige SIP-aadress ja eemaldamine väärtused.

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet konfiguratsiooni mudeli [Mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning.md)kohta.
- Vaadake, kuidas deklaratiivseid ettevalmistamine on kasutatud out-of-box mõista [vaikimisi konfiguratsiooni](active-directory-aadconnectsync-understanding-default-configuration.md).
- Vaadake, kuidas abil, kuidas [muuta vaikimisi konfiguratsiooni](active-directory-aadconnectsync-change-the-configuration.md)deklaratiivseid ettevalmistamise praktiline muudatuste tegemine.

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)

**Viide Teemad**

- [Azure'i AD-ühendus sünkroonimine: funktsioonide juhend](active-directory-aadconnectsync-functions-reference.md)
