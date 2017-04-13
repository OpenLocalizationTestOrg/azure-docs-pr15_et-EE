<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda andmekaitse nõudeid | Microsoft Azure'i"
    description="Kui plaanite oma hübriid identiteedi lahenduse, tuvastada oma äri ja millised suvandid on saadaval kõige tingimustele vastavad andmed vastavus."
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

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Kavandamine tugev identiteedi lahenduse andmete turvalisuse täiustamine

Esimene samm andmete kaitsmiseks on tuvastamine, kellel on juurdepääs andmeid ja selle käigus peab teil olema identiteedi integreerub lahenduse, mis võib teie süsteemi autentimise ja luba võimalusi pakkuda. Autentimise ja luba sageli segi üksteisega ja oma rolli valesti. Tegelikult nad on üsna erinevad, nagu on näidatud alloleval joonisel:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Mobiilsideseadme halduse elutsükli etappide**

Teie hübriid identiteedi lahendus kavandamisel peab mõistate andmekaitse nõudeid oma äri ja millised suvandid on saadaval kõige tingimustele vastavad.
 
>[AZURE.NOTE]
Kui olete lõpetanud, andmete turvalisuse kavandamise, lugege läbi [määratleda mitmikautentimise nõuded](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) tagamaks, et soovitud valikud puudutav mitmikautentimise nõuded ei mõjuta otsuseid selles jaotises.

## <a name="determine-data-protection-requirements"></a>Andmekaitse nõudeid määratlemine
Enamik ettevõtteid mobility vanus, on levinud eesmärk: saavad kasutajad saaksid veel tõhusamalt töötada kõikjal mobiilsideseadmetes ajal kohapealse või eemalt tootlikkuse suurendamiseks. Kuigi see võib olla ühine eesmärk, ettevõtted, kes on see on ka muret ohtude, mida tuleb leevendada ettevõtte andmete turvalisuse ja privaatsuse kasutaja säilitamiseks summa. Iga ettevõte võib olla erinevad nõuded erinevate vastavuse reeglid, mis on erinev milliseid tööstusharu vastavalt ettevõtte toimib viib erinevate kujundus otsuseid. 

Kuid on mõned turvalisuse aspekte, mis tuleks uurida ja valideeritud valdkonna, olenemata sellest, mida selgitatakse järgmises jaotises.

## <a name="data-protection-paths"></a>Andmete kaitsmine teed

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Andmete kaitsmine teed**

Ülaltoodud joonisel on identiteedi komponent esimene enne, kui andmed on juurdepääs. Andmed saate siiski eri riikides ajal see oli juurde. Iga arvu selle skeemi tähistab teed, kus andmeid saab mingil hetkel leida aega. Need numbrid on järgmiselt:

1. Andmekaitse seadme tasemel.
2. Andmekaitse liikudes.
3. Andmekaitse samal ajal kohapealse ülejäänud.
4. Andmekaitse samal ajal ülejäänud pilveteenuses.

Kuigi tehniline juhtelemendid, mis võimaldab kaitsta andmed ise need etapid sellise ei pakuta otse hübriid identiteedi lahenduse, tuleb et hübriid identiteedi lahenduse suudab kasutamine nii kohapealse ja pilveteenuse identiteedi haldamine ressursid tuvastamiseks enne kasutajale anda juurdepääsu andmetele. Kui teie hübriid identiteedi lahenduse plaanimine tagada vastavalt oma ettevõtte vajadustele järgmised küsimustele:

## <a name="data-protection-at-rest"></a>Ülejäänud andmekaitse
Sõltumata sellest, kus andmed on ülejäänud (seadme, cloud või kohapealse), on oluline, kui hindamine mõistmiseks sellega ettevõtte vajadustele. Selles jaotises tagama järgmised küsimused:

- Ettevõtte vajab ülejäänud andmete kaitsmiseks?
 - Kui jah, on hübriid identiteedi lahendus saavad teie praegune kohapealse taristu integreerida?
 - Kui jah, on hübriid identiteedi lahendus võimalik integreerida oma töökoormus pilveteenuses asub?
- Pilveteenuse identiteetide haldamisviis suudab kaitsta kasutaja mandaat ja muude andmete pilveteenuses?

## <a name="data-protection-in-transit"></a>Andmekaitse teel
Kasutatav seade ja andmekeskuse seade ja pilveteenuse vahel või töödeldavate andmete peavad olema kaitstud. Siiski-teel olles ei tähenda suhtlus protsessi komponent väljaspool teie pilveteenuses; Andmevormis viib ettevõttesiseselt, samuti nagu kahe virtuaalse võrgu. Selles jaotises tagama järgmised küsimused:

- Ettevõtte vajab töödeldavate andmete kaitsmiseks?
 - Kui jah, on hübriid identiteedi lahendus võimalik turvaline juhtelemendid, nt SSL/TLS integreerida?
- Pilveteenuse identiteetide haldus ei hoida liikluse ja sees directory poe (ja nende vahel andmekeskuste) sisse loginud?


## <a name="compliance"></a>Nõuetele vastavus
Määruste, seaduste ja nõuetele vastavuse nõuded sõltub teie ettevõtte kuuluva valdkonna. Ettevõtete kõrge reguleeritud valdkondades peab tegelema identiteedi haldamise nõuetele vastavus probleemid seotud probleeme. Identiteedi ja juurdepääsu on väga ranged määruste nagu Sarbanes Oxley (SOX), kindlustamiseks teisaldamist ja vastutuse Act (HIPAA), Gramm-Leach-Bliley seadus (GLBA) ja selle makse kaart Industry Data Security Standard (PCIDSS). Hübriid identiteedi lahenduse, mis võtab ettevõtte peab olema põhilised funktsioonid, mis on üks või mitu nende määruste nõuete täitmiseks. Selles jaotises tagama järgmised küsimused:

- On hübriid identiteedi lahendus regulatiivsetele nõuetele, oma ettevõtte jaoks?
- Kas hübriid identiteedi lahenduse on sisseehitatud funktsioonid, mis võimaldab teie ettevõte olla nõuetele vastavuse regulatiivsetele nõuetele? 
 
>[AZURE.NOTE]
Veenduge, et iga vastus märkmete tegemine ja vastus vaja mõista. [Andmete kaitsmise strateegia määratlemine](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) läheb üle saadaolevad suvandid ja kummagi variandi eelised ja puudused.  Millel vastused neile küsimustele valite, millise variandi parima sobib teie ettevõtte vajab.

## <a name="next-steps"></a>Järgmised sammud
 [Sisuhaldus nõuete määratlemine](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
