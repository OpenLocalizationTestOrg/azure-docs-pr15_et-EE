<properties
   pageTitle="Azure Active Directory abil välise identiteedid haldamise võimalusi võrdlus | Microsoft Azure'i"
   description="Võrdleb Azure Active Directory B2B koostöö, B2C ja mitme rentniku rakenduse toetamiseks autentimise ja luba välise identiteedid"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Azure Active Directory abil välise identiteedid haldamise võimalusi võrdlus

Töötajate mobiilse juurdepääsu SaaS rakenduste haldamine, Lisaks aitab Azure Active Directory (Azure AD) ettevõtte äripartneritega ressursside jagamiseks ja pakkuda rakenduste ettevõtetele ja tarbijatele.

## <a name="developing-applications-for-businesses"></a>Ettevõtete jaoks rakenduste arendamise

Kas esitate teenuse või rakenduse, nt palgad teenus, ettevõtetele? Azure AD pakub identiteedi platvormi, mis võimaldab teil luua rakendusi, et sujuvalt integreerida miljoneid ettevõtted, mis on juba konfigureeritud Azure AD juurutamisel Office 365 või ettevõtte muude teenustega.

**Näide:** Pharmaceutical distributor pakub meditsiiniline tarvikute ja et tervishoiuteenuste valdkonna. Need on vaja välja analytics rakenduse meditsiini tavasid ja soovitud klientidele oma identiteedi haldamine. Selle ettevõtte valitud Azure AD identiteedi platvorm oma tava halduse rakenduse, pakkudes Azure AD identiteedid oma klientidele märgiga kuni vajaduse korral. Lisateavet leiate teemast [Azure Active Directory arendaja juhend](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Partneri juurdepääsu oma ettevõtte ressursse lubamine

Kas teil on väljaspool ettevõtet, kellel on vaja juurdepääsu oma ettevõtte ressursid, nt SharePointi saidi või ERP-süsteemis äripartneritega või teised? Azure AD võimaldab administraatorid andmiseks väliste kasutajate (võib-olla või ei eksisteeri Azure AD) ühe sisselogimise ettevõtte rakendustele juurdepääsemiseks. See parandab turvalisus kasutajate kaotada juurdepääsu, kui nad lahkuvad partneri organisatsiooni, samal ajal juhite juurdepääsupoliitikaid oma ettevõttes. See ka lihtsustab haldamine, kui te ei pea välisele partnerile kataloogi või kohta partneri federation seoste haldamine.

**Näide:** Kujutise ettevõte pakub jaemüüjad foto Imagingi teenuste ja suurim jaemüügi võrgu printimise kioskis toimib. Need valitud Azure AD lubamiseks tuhandete kasutajate jaemüügi äripartnerite oma mandaadi abil saate alla laadida uusima partneri turundusmaterjalide ja foto töötlemise tarvikute ettevõtte tarnija Suhtevõrgu järjestuse muutmine. Lisateavet leiate teemast [Azure AD B2B koostöö](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Tarbijate jaoks rakenduste arendamise

Kas vajate turvaliselt ja efektiivselt avaldamine veebis rakendused, näiteks jaemüügi poe ees, miljonid tarbijad? Azure AD annab platvormi lubamine social samuti kasutajanime ja parooliga sisse logida, firmastiilis iseteenindusliku registreeruda ja Iseteeninduslik parooli lähtestamise rakenduse tarbijate jaoks. Suurendab mugavuse tarbijatele vähendades oma arendajate koormus.

**Näide:** Funktsiooni \#1 Sport frantsiisi vaja suhelda otse oma 450 miljonit ventilaatoreid maailmas. Selleks need ehitatud mobiilirakenduse kaudu, kasutades Azure AD kasutaja autentimise ja profiili Storage. Saada lihtsustatud registreerimise ja sisselogimist nagu Facebooki suhtlusvõrgu kontod abil või nad saavad kasutada traditsiooniline kasutajanime ja parooli tõrgeteta iOS-i, Androidi ja Windowsi telefonides. Mis on loodud Azure AD platvormi märgatavalt vähendada kohandatud koodi ajal annab õiguse hääletada kohandatud margikujundus ja turvalisuse, andmete seotud rikkumiste eest ja skaleeritavus leevendada. Lisateavet leiate teemast [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Azure AD võimaluste võrdlus

Iga stsenaariumid, mis on juba kirjeldatud selles artiklis käsitletakse võimaluste Azure AD sees. Selles tabelis aitavad täpsustada, millised võimalused on kõige asjakohasemad:

| **Kaaluge selle toote...**       | [Azure AD rentniku mitme SaaS rakendus](active-directory-developers-guide.md)    | [Azure'i AD B2B koostöö](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure'i AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Kui mul on vaja sisestada...** | teenuse ettevõtetele | partnerite juurdepääsuga minu rakendused  | teenuse tarbijatele |
| **Ja ma olen sarnane...**  | Pharma distributor      | Ettevõtte Imagingi            | Spordialad frantsiis       |
| **Minirakenduse juurutamine...**  | Harjutamine haldus     | Suhtevõrgu tarnija          | Võistlus ventilaatoreid            |
| **Suunamise...**        | Arsti büroode        | Kinnitatud äripartneritega | Kõik, kellel on e-posti      |
| **Puuetega inimestele juurdepääsetavate siis, kui...**      | Kliendi administraator nõusolekute | Minu administraator kutsed           | Funktsiooni registreerub      |
