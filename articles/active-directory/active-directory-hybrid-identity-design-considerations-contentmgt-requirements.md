<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda sisuhaldus nõuded | Microsoft Azure'i"
    description="Antakse ülevaade sellest, kuidas määrata oma ettevõtte sisuhaldus nõuetele. Tavaliselt juhul, kui kasutaja on oma seadme ta võib olla ka mitu identimisteave, mis kuvatakse vahelduvad vastavalt selle ta kasutab. See on oluline eristada, millist tüüpi sisu on loodud ja need ettevõtte mandaadi abil loodud isikliku mandaadi abil. Oma identiteedi lahenduse peaks oskama suhelda cloud services tõrgeteta esitada lõppkasutaja ajal privaatsuse tagamine ja suurendamine vastu, andmete kaitse."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Sisuhaldus nõuded lahendusse hübriid identiteedi määratlemine

Ettevõtte sisuhaldus nõuete mõistmine võib mõjutada otse oma otsustamine, millised hübriid identiteedi lahendus kasutada. Mitmes seadmes leviku ja võimalus kasutajate tuua oma seadmed ([BYOD](http://aka.ms/byodcg)), ettevõtte peab kaitsta oma andmeid, aga need ka peab kasutaja Privaatsus samaks. Tavaliselt juhul, kui kasutaja on oma seadme ta võib olla ka mitu identimisteave, mis kuvatakse vahelduvad vastavalt selle ta kasutab. See on oluline eristada, millist tüüpi sisu on loodud ja need ettevõtte mandaadi abil loodud isikliku mandaadi abil. Oma identiteedi lahenduse peaks oskama suhelda cloud services tõrgeteta esitada lõppkasutaja ajal privaatsuse tagamine ja suurendamine vastu, andmete kaitse. 

Oma identiteedi lahendus on kasutada erinevaid tehnilise juhtelemente pakkumiseks sisuhaldus, nagu on näidatud alloleval joonisel:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Turbemeetmed, mille ei saa kasutamine oma identiteedihaldussüsteemi**

Üldiselt sisuhaldus nõuete kogusummaks oma identiteedihaldussüsteemi järgmisi teemasid:

- Privaatsus: tuvastamise ressursi ja rakendamise asjakohase kontrolli, et säilitada usaldusväärsus kasutaja.
- Andmete liigitamine: tuvastada kasutaja või rühma ja objekti vastavalt oma liigitamine tase. 
- Andmekaitse lekke: turbemeetmed eest kaitsmise andmete lekke vältimiseks tuleb suhelda identiteedi süsteem kasutaja identiteedi kinnitamiseks. See on oluline, et rada eesmärki auditeerimine.

>[AZURE.NOTE]
Lugege heade tavade kohta lisateavet [andmete liigitus pilveteenuses valmiduse](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ja andmete liigitamine juhised.

Kui teie hübriid identiteedi lahenduse plaanimine tagada vastavalt oma ettevõtte vajadustele järgmised küsimustele:

- Kas teie ettevõttel on turbemeetmed kohas jõustamiseks andmete privaatsust?
 - Kui jah, kas need turbemeetmed on võimalik integreerida hübriid identiteedi lahenduse, mida te ei kavatse vastu?
- Kas teie ettevõte kasutab andmete liigitamine?
 - Kui jah, on praegune lahendus võimalik integreerida hübriid identiteedi lahenduse, mida te ei kavatse vastu?
- Kas teie ettevõte praegu on mis tahes lahendus andmeleket? 
 - Kui jah, on praegune lahendus võimalik integreerida hübriid identiteedi lahenduse, mida te ei kavatse vastu?
- Kas teie ettevõte on vaja auditeeritavad juurdepääsu ressursid
 - Kui jah, millist tüüpi ressursse?
 - Kui jah, millised andmed on vajalik?
 - Kui jah, kus auditilogi peab asuma? Kohapealse või pilves?
- Kas teie ettevõte on vaja krüptimiseks e-kirju, mis sisaldavad tundlikku teavet (SSNs, krediitkaardinumbrid jne)
- Ettevõtte vajab kõigi dokumentide/sisu ühiskasutusse välise äripartneritega krüptimiseks?
- Kas teie ettevõte on vaja jõustada ettevõttepoliitikaid teatud tüüpi e-kirju (kas ei vasta kõigile, edasisaatmine)?
 
>[AZURE.NOTE]
Veenduge, et iga vastus märkmete tegemine ja vastus vaja mõista. [Andmete kaitsmise strateegia määratlemine](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) läheb üle saadaolevad suvandid ja kummagi variandi eelised ja puudused.  Millel vastused neile küsimustele valite, millise variandi parima sobib teie ettevõtte vajab.


## <a name="next-steps"></a>Järgmised sammud
[Juurdepääsu kontrollimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
