<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda identiteedi nõuded | Microsoft Azure'i"
    description="Määratlege ettevõtte ärivajaduste, mis viib teid määratleda nõuded hübriid identiteedi kujundus."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Identiteedi nõuded lahendusse hübriid identiteedi määratlemine
Esimene samm kujundamise hübriid identiteedi lahenduse on business ettevõte, mis ei saa kasutamine see lahendus nõuded kontrollida.  Hübriid identiteedi algab (see toetab kõiki pilve lahendusi autentimine) roll ja läheb kohta pakkuda uut ja huvitavat funktsioone, mille avamine kasutajate jaoks uue töökoormus.  Nende töökoormus või teenused, mida soovite kasutajate jaoks vastu ei näe nõuded hübriid identiteedi kujundus.  Nende teenuste ja töökoormus tuleb kasutada hübriid identiteedi asutusesiseselt ja pilves.  

Peate minema üle järgmisi olulisi äritegevuse mõista, mis see on nüüd nõue ja mida ettevõte tulevikus. Kui teil nähtavuse pikaajaline strateegia hübriid identiteedi kujundus, on tõenäoline, et teie lahendus ei ole scalable ettevõtte vajadused kasvada ja muuta.   T ta diagramm näitab näide hübriid identiteedi arhitektuur ja töökoormus, mis on avada kasutajate jaoks. See on lihtsalt näide kõiki uusi võimalusi, mida saab lukustamata ja ühtlase hübriid identiteedi strateegia kohale toimetatud. 
 
Teatud komponendid, mis on osa hübriid identiteedi arhitektuur![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Ärivajaduste määratlemine
Iga ettevõte on erinevad nõuded, isegi juhul, kui need ettevõtted on osa sellest sama tegevusala, reaal ettevõtte nõuded võivad erineda. Saate siiski kasutada head tavad tööstusharu, kuid lõpuks on ettevõtte ärivajaduste, mis viib teid määratleda nõuded hübriid identiteedi kujundus. 

Veenduge, et teie ettevõtte vajadustele tuvastamiseks järgmised küsimused.

- Kas teie ettevõte otsib lõigata funktsionaalseid maksumus?
- Kas teie ettevõte otsib secure cloud varad (SaaS rakendused, taristu)?
- Kas teie ettevõte otsib teie IT ajakohastada?
  - On kasutajate rohkem mobile ja nõuavad seda, et luua oma DMZ erinevat tüüpi liikluse erinevate ressursside juurdepääsu lubamiseks erandid?
  - Kas teie ettevõttel on pärand rakendused, millel on vaja need tänapäevane kasutajad avaldada, kuid pole lihtne kirjutada?
  - Kas teie ettevõte on vaja täita kõiki neid ülesandeid ja tuua kontrolli all samal ajal
- Kas teie ettevõte otsib secure kasutajate identiteetide ja tuues uusi tööriistu, mida kasutada Microsoft Azure'i turvalisus teadmiste kohapealse teadmisi vähendab?
- Kas teie ettevõtte üritab vabaneda kartsin "välise" kontod kohapealne ja neid teisaldada pilve, kui nad ei ole enam soikus ohtu sees kohapealse keskkonna?

## <a name="analyze-on-premises-identity-infrastructure"></a>Kohapealse identiteedi taristu analüüsimine
Nüüd, kui teil on oma ettevõtte business nõuete kohta aimu, peate oma kohapealse identiteedi taristu hinnata. Hindamisel on oluline tehnilised nõuded integreerida oma praeguse identiteedi lahendus cloud identiteedihaldussüsteemi määratlemine. Veenduge, et vastavad järgmistele küsimustele.

- Millist autentimise ja luba lahenduse, kas teie ettevõte asutusesisese kasutada? 
- Kas teie ettevõte praegu on mis tahes kohapealse sünkroonimisteenused?
- Kas teie ettevõte kasutab mõni muu Identiteedipakkujad (IdP)?

Peate olema teadlik pilveteenustega, mis võib teie ettevõte olla. Hinnang mõista teie keskkonnas SaaS, IaaS või PaaS mudelite praeguse integreerimine läbimiseks on väga oluline. Veenduge, et see hindamisel vastavad järgmistele küsimustele.
- Kas teie ettevõttel on mis tahes integreerimine pilve teenusepakkuja?
- Kui jah, milliseid teenuseid kasutatakse?
- Selle integreerimine on praegu valmistamisel või on katseprojekti?


>[AZURE.NOTE]
Kui teil pole on on täpne vastenduse kõikide rakenduste ja pilveteenused, saate kasutada pilveteenuse rakenduse Discovery tööriist. See tööriist pakkuda nähtavus oma ettevõtte business ja tarbija pilve rakendused oma IT-osakonna poole. Mis muudab lihtsamaks kui kunagi leida varju IT ettevõtte andmetega kasutus mustrite ja kõik kasutajad, kes kasutavad teie pilv rakendusi. Juurdepääsuks avage see tööriist [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Identiteedi integreerimine nõuete hindamine
Järgmiseks tuleb teil hinnata identiteedi integreerimine nõuetele. See hindamine on oluline määratleda, kuidas kasutajad saavad autentida, ettevõtte kohaloleku ilme pilves, kuidas organisatsiooni võimaldab autoriseerimine ja mida kasutuskogemuse saab olema tehnilised nõuded. Veenduge, et vastavad järgmistele küsimustele.

- Kas teie asutuses kasutada federation, standard autentimist või mõlemad?
- On Domeeniliit?  Kuna järgmist:
 - Kerberos-põhiste SSO-d
 - Teie ettevõttel on kohapealse rakenduste (kas ehitatud ettevõttesiseseid või 3 tootja) kasutava SAML ja sarnase federation võimalused.
 - MFA kiipkaardi kaudu. RSA SecurID jne.
 - Kliendi juurdepääsu reeglid, mis järgmistele küsimustele:
     1. Saate blokeerida kõik välise juurdepääsu teenusekomplektile Office 365, mis põhineb kliendi IP-aadress?
     1. Saate blokeerida kõik välise juurdepääsu teenusekomplektile Office 365, välja arvatud Exchange ActiveSync?
     1. Saate blokeerida kõik välise juurdepääsu teenusekomplektile Office 365, välja arvatud brauseripõhiste rakenduste (OWA, SPO)
     1. Saate blokeerida kõik välise juurdepääsu teenusekomplektile Office 365 jaoks määratud AD rühma liikmete
- Turvalisus ja auditeerimine probleemid
- Juba olemasolevate investeeringute ühendatud autentimine
- Nimi, mida kasutatakse meie ettevõtte meie domeeni pilveteenuses?
- Kas ettevõttel on kohandatud domeeni?
    1. Avalik ja hõlpsasti kontrollitav DNS-i kaudu, on selle domeeni?
    1. Kui pole, siis kas teil on avalik domeen, mida saab kasutada mõne alternatiivse UPN-i registreerimiseks AD?
- Vastavad kasutaja identifikaatorite cloud kujutamiseks? 
- Kas organisatsioon on rakendusi, mis nõuavad integreerimine pilveteenustega?
- Kas organisatsioon on mitu domeeni ja need kõik kasutatakse standard- või ühendatud autentimine?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Rakendusi, mis töötavad teie keskkonnas hindamine
Nüüd, kui on idee kohta oma kohapealse ja pilveteenuse taristu, peate rakendusi, mis töötavad nendes keskkondades. Hindamisel on oluline määratleda tehnilised nõuded integreerida cloud identiteedihaldussüsteemi need rakendused. Veenduge, et vastavad järgmistele küsimustele.

- Kus meie rakenduste elavad?
- Saab kasutajatele juurdepääsu kohapealse rakendusi?  Pilveteenuses? Või mõlemad?
- Kas on kavas võtta olemasoleva rakenduse töökoormus ja pilveteenusesse teisaldada?
- Kas arendada uue rakendusi, mis asuvad kas kohapeal või pilves, mida kasutatakse pilvepõhises lepingute autentimist?

## <a name="evaluate-user-requirements"></a>Kasutajanõuded hindamine
Teil on ka hindamaks, kui kasutaja nõuetele. Hindamisel on oluline määratleda toiminguid, mis on vaja alustamine ja aidata kasutajaid, nagu need sujuv pilveteenusesse. Veenduge, et vastavad järgmistele küsimustele.

- Saab kasutajatele juurdepääsu asutusesisese rakendusi?
- Saab kasutajatele juurdepääsu taotluste pilveteenuses?
- Kuidas kasutajad tavaliselt oma kohapealse keskkonna sisse logida?
- Kuidas saavad kasutajad sisse logida pilveteenusesse?

>[AZURE.NOTE]
Veenduge, et iga vastus märkmete tegemine ja vastus vaja mõista. [Määratlemine langeva vastuse nõuded](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) lähevad üle saadaolevad suvandid ja spetsialistidele/miinuste kummagi variandi.  On vastused neile küsimustele valite, millise variandi parima sobib teie ettevõtte peab.

## <a name="next-steps"></a>Järgmised sammud
[Directory sünkroonimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
