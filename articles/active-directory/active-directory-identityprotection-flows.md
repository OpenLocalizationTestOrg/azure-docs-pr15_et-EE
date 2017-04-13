<properties
    pageTitle="Sisselogimise kogemusi Azure AD identiteedi kaitse | Microsoft Azure'i"
    description="Annab ülevaate kasutaja võimalused, kui kaitse on leevendada või tervendama kasutaja või kui poliitika on nõutud mitmikautentimise."
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Sisselogimise kogemused Azure AD identiteedi kaitse

Azure Active Directory identiteedi kaitse, saate teha järgmist.

- mitmikautentimise registreeruma kasutajad

- täitepide riskantne sisselogimist ja rikutud kasutajad

Vastuse süsteemi järgmiste probleemide korral on kasutaja sisselogimine mõju, kuna lihtsalt otse logida sisse andes kasutajanime ja parooli ei ole võimalik enam. Järgmised toimingud on vaja saada kasutaja turvaline uuesti sisse business.

Selles teemas antakse ülevaade kasutaja sisselogimine kõigil juhtudel, mis võivad tekkida.

**Mitmikautentimise**

- Mitmikautentimise registreerimine



**Risk sisselogimine**

- Riskantne taastamine

- Riskantne sisselogimist blokeeritud

- Mitmikautentimise registreerimise käigus riskantne sisselogimine
 

**Kasutaja vastutusel**

- Rikutud konto taastamine

- Rikutud blokeeritud konto




## <a name="multi-factor-authentication-registration"></a>Mitmikautentimise registreerimine

Parima kasutuskogemuse nii rikutud konto taastamise vool ja riskantne sisselogimise voogu, on see, kui kasutaja saate ise taastada. Kui kasutaja on registreerunud mitmikautentimise, nad on juba saab edasi turvalisus toimetulekuks oma kontoga seotud telefoninumber. Pole abi laua- või administraator kaasamine on vaja konto kompromiss taastada. Seega on väga soovitatav kasutajate mitmikautentimise jaoks registreeritud. 

Administraatorid saavad:

- seadnud poliitika, mis nõuab kasutajad oma kontode kontrollimiseks suurema turvalisuse tagamiseks. 
- luba juhuks, kui kasutajad soovivad anda kasutajatel on ajapikenduse perioodi enne registreerumist vahelejätmine mitmikautentimise registreerimise kuni 30 päeva.

**Mitmikautentimise registreerimise on kolm toimingut.**

1. Esimese sammuna kasutaja saab seada üles mitmikautentimise konto nõue teatisi. 

    ![Parandamise] (./media/active-directory-identityprotection-flows/140.png "Parandamise")


2. Mitmikautentimise häälestamine, peate lasta süsteemil teada, kuidas soovite ühendust võtta.

    ![Parandamise] (./media/active-directory-identityprotection-flows/141.png "Parandamise")
 
3. Süsteemi väidab, et probleemiks ja te peate vastama.

    ![Parandamise] (./media/active-directory-identityprotection-flows/142.png "Parandamise")

 



## <a name="risky-sign-in-recovery"></a>Riskantne taastamine

Kui administraator on konfigureerinud sisselogimise riske poliitika, teavitatakse kasutajatel, kui kasutajad proovivad Logi sisse. 

**Riskantne meilivoo on kaks toimingut.** 

1. Kasutaja on teavitada, et midagi ebatavalised tuvastati kohta oma sisselogimist, nt uude asukohta, seadme või rakenduse kaudu sisselogimist. 

    ![Parandamise] (./media/active-directory-identityprotection-flows/120.png "Parandamise")

2. Kasutaja peab oma isikut turvalisus probleem lahendada. Kui kasutaja on registreerunud mitmikautentimise tuleb edasi-tagasi oma telefoninumber turbekood. Kuna see on lihtsalt riskantne Logi sisse ja pole rikutud konto, kasutaja ei pea see vool parooli muutmine. 

    ![Parandamise] (./media/active-directory-identityprotection-flows/121.png "Parandamise")



 
## <a name="risky-sign-in-blocked"></a>Riskantne sisselogimist blokeeritud
Administraatorid saate valida ka korral sisselogimine sõltuvalt risk Blokeeri kasutajate sisselogimist Risk poliitika määramine. Lõppkasutajad tuleb blokeering saamiseks ühendust võtta mõne administraatori või abi kasutajatoe või need võite proovida uue asukoha või muust seadmest sisselogimisel. Mitmikautentimise lahendada ise taastuv ei ole võimalus sel juhul.

![Parandamise] (./media/active-directory-identityprotection-flows/200.png "Parandamise")




## <a name="compromised-account-recovery"></a>Rikutud konto taastamine

Kasutaja risk turbepoliitika konfigureerimisel risk kasutajatele, kes vastavad kasutaja määratletud poliitika tase (ja seega eeldatakse rikutud) tuleb läbida kasutaja kompromiss taastamine voogu enne, kui nad saaksid sisse logida. 

**Kasutaja kompromiss taastamine voogu on kolm toimingut.**

1. Kasutaja on teavitada, et oma konto turvalisus on risk kahtlase tegevuse tõttu või lekkinud mandaat.

    ![Parandamise] (./media/active-directory-identityprotection-flows/101.png "Parandamise")

2.  Kasutaja peab oma isikut turvalisus probleem lahendada. Kui kasutaja on registreerunud mitmikautentimise neid saab taastada omas väärkasutus. Need on vaja edasi-tagasi oma telefoninumber turbekood. 

    ![Parandamise] (./media/active-directory-identityprotection-flows/110.png "Parandamise")


3.  Lõpuks kasutaja on sunnitud oma parooli muuta, kuna keegi teine oli juurdepääsu oma kontole. See kogemus kuvatõmmised on allpool.
 
    ![Parandamise] (./media/active-directory-identityprotection-flows/111.png "Parandamise")



## <a name="compromised-account-blocked"></a>Rikutud blokeeritud konto 

Kasutaja, mis blokeeriti kasutaja risk turbepoliitika blokeering saamiseks peab kasutaja pöörduge administraatori poole või klienditugi. Mitmikautentimise lahendada ise taastuv ei ole suvandi sel juhul.


![Parandamise] (./media/active-directory-identityprotection-flows/104.png "Parandamise")



 
## <a name="reset-password"></a>Parooli lähtestamine

Kui rikutud kasutajad on blokeeritud sisse logida, saab administraator luua ajutine parool neid. Kasutajad on oma parooli muuta ajal järgmise sisselogimine.

![Parandamise] (./media/active-directory-identityprotection-flows/160.png "Parandamise")


 




 

## <a name="see-also"></a>Vt ka

- [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md) 