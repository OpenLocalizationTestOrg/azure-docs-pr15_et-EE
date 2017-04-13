<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine – ülevaade | Microsoft Azure'i"
    description="Ülevaade ja sisu kaart hübriid identiteedi kujundus kaalutlused juhend"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory hübriid identiteedi kujundamine

Tarbija-põhiste seadmete levivad ettevõtte maailma ja pilvepõhise tarkvara-kui-a-service (SaaS) rakendused on lihtne vastu võtta. Seetõttu on raske säilitades kontrolli kasutajate rakenduse access sisemise andmekeskuste ja pilve keskkondades.  Microsofti identity lahendused hõlmavad kohapealse ja pilvepõhiseid funktsioone, ühe kasutaja identiteedi jaoks autentimise ja luba kõigile ressurssidele, olenemata asukohast loomine. Me kutsume selle hübriid identiteedi. On erinevate kujundus ja konfiguratsiooni suvandite abil Microsofti lahenduste hübriid identiteedi ja mõnel juhul võib olla keeruline kindlaks teha, milline on parim ettevõtte vajadustele. See hübriid identiteedi kujundus kaalutlused juhend aitab teil mõista, kuidas kujundada hübriid identiteedi lahenduse mis sobib kõige paremini ettevõtet ja tehnoloogia peab teie ettevõtte jaoks.  Sellest juhendist üksikasjalikult sarja juhiseid ja tööülesandeid, mida saab jälgida, et aidata teil kujundada hübriid identiteedi lahenduse, mis vastab teie ettevõtte vajadustele. Kogu juhiseid ja tööülesandeid, esitab juhend oluline tehnoloogiad ja funktsioonide võimalusi, mida ettevõtted otstarbekas koosolekud ja teenuste kvaliteedi (nt kättesaadavus, skaleeritavus, jõudluse, hallatavust ja turvalisus) taseme nõuetele. Täpsemalt hübriid identiteedi kujundus kaalutluste juhend eesmärgid on järgmised küsimused: 

- Küsimused, mis on vaja juhtida hübriid identiteedi kohased kujundus tehnoloogia või probleem domeeni, et kõige paremini vastab oma küsimusele?
- Millist järjestust tegevuste tuleb täita domeeni tehnoloogia või probleemi lahenduse hübriid identiteedi kujundamisel? 
- Mis hübriid identiteedi tehnoloogia ja konfiguratsiooni võimalusi on abiks minu nõuetele? Millised on nende suvandite vahel kompromisse nii, et valin on parim valik oma ettevõtte jaoks?


## <a name="who-is-this-guide-intended-for"></a>Kes juhend mõeldud?
 CIO, CITO, juht identiteedi arhitektid, Enterprise arhitektidele ja IT arhitektid vastutab kujundamise hübriid identiteedi lahenduse suur või keskmise suurusega ettevõtete jaoks.

## <a name="how-can-this-guide-help-you"></a>Kuidas see juhend teid aidata? 
Selle juhendi abil saate aru saada, kuidas kujundada hübriid identiteedi lahenduse, mis on võimalik pilvepõhine identiteedihaldussüsteemi integreerida teie identiteedi praegune kohapealse lahenduse. Järgmisel pildi näha näiteks hübriid identiteedi lahenduse, mis võimaldab seda hallata oma praeguse Windows Server Active Directory lahenduse integreerida administraatorid asub kohapealse koos Microsoft Azure Active Directory, et kasutajad saaksid kasutada kõigis rakendustes, mis asuvad kohapealse ja pilveteenuse ühekordse sisselogimise (SSO).

![](./media/hybrid-id-design-considerations/hybridID-example.png)


Ülaltoodud joonisel on kujutatud hübriid identiteedi lahenduse, mis on kasutamine pilveteenused integreerida kohapealse võimaluste pakkumiseks üks kogemus lõppkasutaja autentimise protsessi ja hõlbustada nende ressursside haldamine. Kuigi see võib olla väga levinud stsenaariumi, iga asutuse hübriid identiteedi kujundus on tõenäoliselt erinevas näide, mis on näidatud joonisel 1 tõttu erinevad nõuded. Sellest juhendist leiate sarja juhiseid ja tööülesandeid, mida võite järgida kujundamisel hübriid identiteedi lahenduse, mis vastab teie ettevõtte vajadustele. Kogu järgmised etapid ja toimingud on juhend tutvustab asjakohaseid tehnoloogiaid ja funktsioon saadaolevad suvandid teile otstarbekas ja teenuste kvaliteedi taseme nõuetele oma ettevõtte jaoks.

**Oletused**: teil on Windows Server, Active Directory domeeniteenused ja Azure Active Directory kogemusi. Selles dokumendis me oletame, et otsite kuidas vastavad nende lahendused teie ettevõtte vajadustele või integreeritud lahendust.

## <a name="design-considerations-overview"></a>Kujundus kaalutlused ülevaade
Selles dokumendis on toodud juhiseid ja tööülesandeid, mida saate jälgida kujundamise hübriid identiteedi lahenduse, mis vastab teie vajadustele kõige paremini. Järjestatud jada on esitatud juhiseid. Saate teada, hiljem toimingutes kujundamine võib olla vajalik muuta otsuseid tehtud varasema juhiseid, aga tõttu vastuoluliste kujunduse Valikud. Iga katse märku võimalike kujundus konfliktide kogu dokumendis. 

Saate jõudma kujundus, et kõige paremini vastab teie vajadustele alles pärast iterating nii mitu korda nii, et lisada kõik kaalutlused dokumendis olevaid juhiseid. 

| Hübriid identiteedi etapp.                                             | Teema loend                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Määratleda identiteedi nõuded                                   | [Ärivajaduste määratlemine](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Directory sünkroonimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Mitmikautentimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Hübriid identiteedi kasutuselevõtu strateegia määratlemine](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Kavandamine tugev identiteedi lahenduse andmete turvalisuse täiustamine | [Andmekaitse nõudeid määratlemine](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Sisuhaldus nõuete määratlemine](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Juurdepääsu kontrollimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Määratleda langeva vastuse nõuded](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Andmete kaitsmise strateegia määratlemine](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Hübriid identiteedi elutsükli kavandamine                                | [Hübriid identiteedi haldustoimingute määratlemine](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Sünkroonimise haldamine](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hübriid identiteedi haldamine kasutuselevõtu strateegia määratlemine](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Selle juhendi allalaadimiseks
PDF-i juhend hübriid identiteedi kujundamine versiooni saate alla laadida [TechNeti Galerii](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
