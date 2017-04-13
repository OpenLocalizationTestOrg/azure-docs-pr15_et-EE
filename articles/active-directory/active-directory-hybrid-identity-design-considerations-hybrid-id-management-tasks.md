<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine kaalutlused – määratleda hübriid identiteedi haldustoimingute | Microsoft Azure'i"
    description="Azure Active Directory kontrollib juurdepääsu kontrolli, saate valida, kui kasutaja autentimist ja enne Accessi rakendusse eritingimused. Kui nendest tingimustest on täidetud, autenditud kasutaja ja rakenduse juurdepääs lubatud."
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>Hübriid identiteedi elutsükli kavandamine 

Identiteedi on üks teie ettevõtte mobility ja rakenduse access strateegia aluseid. Kas logite oma mobiilsideseadme või SaaS rakendus, on teie identiteedi võti juurdepääsu kõik. Kõige kõrgemal tasemel lahenduse identiteedi haldus hõlmab ühendamine ja sünkroonite oma identiteedi hoidlate, mis sisaldab automatiseerimine ja koondamise protsessi ettevalmistamise ressursid. Identiteedi lahenduse tuleks üle kohapealse ja pilveteenuse tsentraliseeritud identiteedi ja kasutada ka mõned identiteedi ühendamine säilitada tsentraliseeritud autentimist ja turvaliselt jagada ja koostööd väliste kasutajate ja jaoks. Ressursside vahemikus operatsioonisüsteemide ja rakenduste olevatele inimestele või liitunud, ettevõtte. Organisatsiooni struktuur saab muuta majutada ettevalmistamise poliitika ja toiminguid.

See on oluline, et anda kasutajatele, andes neile iseteenindusliku kogemusi hoida neid tõhusamalt töötada suunatud identiteedi lahenduse. Oma identiteedi lahendus on tugevam, kui see võimaldab ühekordse sisselogimise kasutajate jaoks kõik vajalikud ressursid juurde kõik administraatorid üle tasemed kasutada standardne toimingute haldamise kasutajatunnust. Mõned tasemed halduse saab vähendada või kõrvaldada, olenevalt ettevalmistamise haldamise lahendus laius. Lisaks saate turvaliselt levitada halduse võimaluste, automaatselt või käsitsi eri organisatsioonide vahel. Näiteks domeeni administraator võib olla ainult selle isikud ja ressursid domeenis. Selle kasutaja haldus ja ettevalmistamise toiminguid saate teha, kuid ei ole lubatud teha konfigureerimistoimingud, näiteks luua töövood.


## <a name="determine-hybrid-identity-management-tasks"></a>Hübriid identiteedi haldamise toiminguid määratlemine
Levitamise haldustoiminguid ettevõtte parandab täpsust ja tõhusust haldus ja parandab ülejäänud ettevõtte töökoormus. Järgnevalt on Pivot-funktsioonid, mis määratlevad robustne identiteedihaldussüsteemi.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Määratleda hübriid identiteedi haldamise toiminguid, peate aru põhitunnused mõni ettevõte, mis saavad võtta hübriid identiteedi. See on oluline mõista praeguse hoidlate, kasutatakse identiteedi allikad. Teab core elementidele, on põhilisi nõuded ja põhjal, et peate täpsema küsimusi, mis viib teid paremini kujundus oma otsust oma identiteedi lahenduse.  

Ajal määratlemine nendele nõuetele, tagada, et vähemalt järgmist küsimustele

- Ettevalmistamise suvandid: 
 - Hübriid identiteedi lahenduse ei toeta robustne konto juurdepääsu juhtimine ja ettevalmistamise süsteemi?
 - Kuidas on kasutajate, rühmade ja paroole saab hallata?
 - On identiteedi elutsükli haldus reageeri? 
      - Kui kaua parooli värskenduste konto peatamine aega?
      
- Litsentsihalduse: 
 - Kas hübriid identiteedi lahenduse pidemete litsentsihalduse?
     - Kui jah, millised funktsioonid on saadaval?
- Kas lahendus toime rühma-põhiste litsentsihalduse? 
      – Kui jah, on võimalik turberühma määrata? 
       – Kui jah, kas pilveteenuse kataloogi automaatselt litsentside määramine rühma liikmete? 
        – Mis juhtub, kui kasutaja on hiljem lisada või rühmast eemaldatud, kuvatakse litsentsi automaatselt määratud või eemaldatakse vastavalt vajadusele? 

- Muude tootjate identiteedipakkujad integreerimine:
- Saate selle hübriid lahenduse integreeritud kolmanda osapoole identiteedipakkujad ühekordse sisselogimise rakendamiseks?
- On võimalik kõik erinevad identiteedipakkujad sidus identiteedi süsteemi ühendada?
- Kui jah, kuidas ja millised on nende ja millised funktsioonid on saadaval?

## <a name="synchronization-management"></a>Sünkroonimise haldamine
Üks eesmärke identiteedi juhataja tuua kõik identiteedipakkujad ja hoida neid saaksid sünkroonida. Hoiate sünkroonitud andmete põhjal autoriteetsete juhtslaidi identiteedipakkuja. Hübriid identiteedi stsenaariumi sünkroonitud juhtimine mudelit, kus kõik kasutaja ja seadme identiteedid kohapealse serveri haldamine ja kontod ja soovi korral võite pilveteenusesse paroolide sünkroonimine. Kasutaja sisestab sama parool asutusesisese, kui ta või ta ei pilves ja sign in, parool on kinnitatud identiteedi lahendus. Näites kasutatakse directory sünkroonimistööriist.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Proper kujundus sünkroonimise hübriid identiteedi lahenduse otsivad järgmised küsimustele: • mis on sünkroonimine lahenduste hübriid identiteedi lahenduse kättesaadavaks?
• Mis on ühekordse sisselogimise funktsioonid saadaval?
• Võimalused B2B ja B2C identiteedi jaoks?

## <a name="next-steps"></a>Järgmised sammud
[Hübriid identiteedi haldamine kasutuselevõtu strateegia määratlemine](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)

