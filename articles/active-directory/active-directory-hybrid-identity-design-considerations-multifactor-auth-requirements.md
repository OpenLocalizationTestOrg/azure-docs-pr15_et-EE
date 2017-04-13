<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda mitmikautentimise nõuded"
    description="Azure Active Directory kontrollib juurdepääsu kontrolli, saate valida, kui kasutaja autentimist ja enne Accessi rakendusse eritingimused. Kui nendest tingimustest on täidetud, autenditud kasutaja ja rakenduse juurdepääs lubatud."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Mitmikautentimise nõuded lahendusse hübriid identiteedi määratlemine

Selles maailmas mobility kasutajatega andmete ja rakenduste pilves ja mis tahes seadmest juurde pääsemiseks turvaliseks see teave on muutunud väga tähtsaks.  Iga päev on uus pealkiri rikkumisega turvalisus.  Kuigi ei ole kindel, nt rikkumise, mitmikautentimise, pakub väärtpaberi vältimiseks nende seotud rikkumiste eest.
Alustuseks hindamisel ettevõtted nõuete mitmikautentimise. Mida organisatsioon püüab secure.  Selle hindamiseks on oluline määratleda tehnilised nõuded häälestamise ja lubada ettevõtted kasutajate mitmikautentimise jaoks.

>[AZURE.NOTE]
Kui olete tuttav MFA ja mida see tähendab, on soovitatav lugeda artiklist [mis on Azure Mitmikautentimise?](../multi-factor-authentication/multi-factor-authentication.md) eelneva jätkamiseks lugemine see jaotis.

Veenduge, et vastata järgmist:

- Ettevõtte proovib secure Microsoft rakendusi? 
- Kuidas need rakendused on avaldatud?
- Teie ettevõte osutab Kaugpöördusteenuse võimaldab töötajatel kohapealse rakenduste juurdepääsu?

Kui jah, millist tüüpi Kaugpöördusteenuse? Samuti vajate hindamaks, kuhu kasutajad, kellel on juurdepääs need rakendused paigutatakse. Selle hindamiseks on oluline samm proper mitmikautentimise strateegia määratlemine. Veenduge, et vastavad järgmistele küsimustele.

- Kui kasutajad hakkavad olema asub?
- Ta saab leida suvalist kohta?
- Kas teie ettevõtte soovite piirata vastavalt kasutaja asukoha?

Kui teil mõista nendele nõuetele, on oluline hindamaks, kui ka kasutajanõuded mitmikautentimise. See hindamine on oluline, kuna see määratleb nõuded jooksvalt läbi mitmikautentimise. Veenduge, et vastavad järgmistele küsimustele.

- Kas kasutajad kursis mitmikautentimise?
- Kuvatakse mõned kasutab esitama täiendavat autentimist?  
 - Kui jah, kogu aeg, kui väliseid võrgustikke või juurdepääsu teatud rakenduste või muude tingimustel?
- Kasutajad vajavad koolituse kohta, kuidas häälestada ja rakendada mitmikautentimise?
- Mis on loetletud tähtsaimad stsenaariumid, mis teie ettevõtte soovib lubamiseks mitmikautentimise oma kasutajate jaoks?

Pärast eelmine küsimustele, on võimalik mõista, kui seal on juba rakendatud mitmikautentimise kohapealse. Hindamisel on oluline määratleda häälestamise ja lubada ettevõtted kasutajate jaoks mitmikautentimise tehnilised nõuded. Veenduge, et vastavad järgmistele küsimustele.

- Ettevõtte vajab õigustega kontodel MFA kaitsta?
- Kas teie ettevõte on vaja lubamiseks MFA teatud taotlus nõuetele vastavuse tagamiseks
- Kas teie ettevõte on vaja lubamiseks MFA õigus kõigi kasutajate neid rakenduse või ainult administraatorid
- Kas teil on vaja on alati lubatud MFA või kui kasutajad on sisse logitud ainult väljaspool teie ettevõtte võrguga?


## <a name="next-steps"></a>Järgmised sammud
[Hübriid identiteedi kasutuselevõtu strateegia määratlemine](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
