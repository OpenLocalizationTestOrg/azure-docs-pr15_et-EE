<properties
    pageTitle="Azure Active Directory identiteedi kaitse – kuidas blokeeringu kasutajad | Microsoft Azure'i"
    description="Siit saate teada, kuidas blokeeringu blokeeritud Azure Active Directory identiteedi kaitse poliitika kasutajatelt."
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, blokeeringu tühistada kasutaja"
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
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory identiteedi kaitse – kuidas blokeeringu kasutajad

Azure Active Directory identiteedi kaitse abil saate konfigureerida poliitika, et blokeerida kasutajad, kui konfigureeritud tingimused on täidetud. Tavaliselt kasutaja blokeeritud kontaktid infoliini saada blokeering. Selles artiklis selgitatakse saate teha blokeeringu blokeeritud kasutaja juhiseid.


## <a name="determine-the-reason-for-blocking"></a>Määratleda blokeerimise põhjus

Esimese sammuna blokeeringu tühistada kasutaja peate tuvastamiseks poliitika, mis on blokeerinud kasutaja, kuna see on sõltuvalt järgmised toimingud. Azure Active Directory identiteedi kaitse, mille kasutaja saab kas blokeerida sisselogimise risk poliitika või kasutaja risk poliitika. 

Saate oma poliitika, mis on blokeerinud pealkiri dialoogiboksi, mis on esitatud kasutaja ajal sisselogimise katsel kasutaja tüüp.

|Poliitika | Kasutaja dialoogiboks|
|--- | --- |
|Risk sisselogimine | ![Blokeeritud sisselogimine](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Kasutaja risk | ![Blokeeritud konto](./media/active-directory-identityprotection-unblock-howto/104.png) |


Kasutaja, mis on blokeeritud:

- Sisselogimise risk poliitika on ka kahtlaste sisselogimine
- Kasutaja risk poliitika on ka konto risk

 
## <a name="unblocking-suspicious-sign-ins"></a>Blokeeringu tühistamiseks kahtlaste sisselogimist

Blokeeringu kahtlaste sisselogimine, on teil järgmised valikud.

1. **Sisselogimine tuttavad asukohta või muust seadmest** - blokeeritud kahtlaste sisselogimist levinud põhjused on sisselogimise katsete võõras asukohad või seadmed. Kasutajaid saate kiiresti kindlaks teha, kas see on blokeerimise põhjus püüdes sisselogimine tuttavad asukohta või muust seadmest.


3. **Poliitika välistamine** – kui arvate, et teie poliitika konfiguratsioonile põhjustab probleeme teatud kasutajate jaoks, saate välja jätta kasutajad seda. [Risk sisselogimise poliitika](active-directory-identityprotection.md#sign-in-risk-policy) üksikasjalikumat teavet teemast.
 
4. **Keela poliitika** - kui arvate, et teie poliitika konfiguratsiooni põhjustab kõigi kasutajate jaoks probleeme, saate keelata poliitika. Lugege teemat [sisselogimise risk poliitika](active-directory-identityprotection.md#sign-in-risk-policy) rohkem üksikasju.


## <a name="unblocking-accounts-at-risk"></a>Risk blokeeringu tühistamiseks kontod

Blokeeringu konto risk, on teil järgmised valikud.

1. **Parooli lähtestamine** – saate lähtestada kasutaja parooli. Vt lisateavet [käsitsi turvalise parooli lähtestamine](active-directory-identityprotection.md#manual-secure-password-reset) .

2. **Eiramine kõik risk sündmused** – kasutaja risk poliitika blokeerib kasutaja on konfigureeritud kasutaja taseme juurdepääsu blokeerida jõudnud. Saate vähendada kasutaja taseme käsitsi sulgedes teatatud risk sündmused. Leiate lisateavet teemast [sulgemist risk sündmuste käsitsi](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Poliitika välistamine** – kui arvate, et teie poliitika konfiguratsioonile põhjustab probleeme teatud kasutajate jaoks, saate välja jätta kasutajad seda. Vaadake lisateavet [kasutaja risk poliitika](active-directory-identityprotection.md#user-risk-policy) .
 
4. **Keela poliitika** - kui arvate, et teie poliitika konfiguratsiooni põhjustab kõigi kasutajate jaoks probleeme, saate keelata poliitika. Vaadake lisateavet [kasutaja risk poliitika](active-directory-identityprotection.md#user-risk-policy) .




## <a name="next-steps"></a>Järgmised sammud

 Kas soovite rohkem teada Azure AD identiteedi kaitse? Lugege teemat [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md).
 

