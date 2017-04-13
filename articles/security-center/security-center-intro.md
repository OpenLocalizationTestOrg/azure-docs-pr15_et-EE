<properties
   pageTitle="Sissejuhatus Azure'i turbekeskus | Microsoft Azure'i"
   description="Lisateavet Azure turbekeskus, selle võtme võimaluste ja kuidas see toimib."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Azure'i turbekeskus tutvustus

Lisateavet Azure turbekeskus, selle võtme võimaluste ja kuidas see toimib.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.

## <a name="what-is-azure-security-center"></a>Mis on Azure turbekeskus?
 Turbekeskus aitab vältida, avastada ja vastata ohtude suurendatud nähtavuse ja juhtida turvalisus oma Azure ressursse. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

##  <a name="key-capabilities"></a>Võtme võimalused
 Turbekeskus annab lihtne kasutada ja tõhus ohtu vastuse ennetamine ja tuvastamise võimalusi, mis on ehitatud Azure. Võtme funktsioonid on:

| | |
|----- |-----|
| Vältida | Jälgib oma Azure ressursse turvalisus |
| | Määratleb poliitikate Azure'i tellimused ja ressursside rühmad vastavalt oma ettevõtte turbenõuetele, rakenduste kasutamise ja andmete tundlikkust tüübid |
| | Poliitika põhinev kasutab turvalisus soovitused juhend teenuse omanikud läbi vastava protsessi rakendamiseks nõutavad juhtelemendid |
| | Kiiresti kasutab turvalisust ja seadmete Microsofti ja partnerite |
| Tuvastamine |Automaatselt kogub ja analüüsib turvalisus andmeid oma Azure ressursse, võrgu ja partnerilahenduste nagu ründevaratõrje programmid ja tulemüürid |
| | Globaalne tasakaalustab ohtu ärianalüüsi kaudu Microsofti toodete ja teenuste, Microsoft digitaalse kuritegude üksus (DCU), Microsoft turvalisus vastuse Center (MSRC) ja välise kanalid |
| | Rakendab täiustatud analüüsi, sh masina õppimine ja käitumise analüüs |
| Vasta | Pakub tähtsuse juhtumite/Turbeteatiste |
| | Pakub teadmisi allikas ja rünnak kannatavas ressursid |
| | Soovitusi, kuidas lõpetada praegune rünnak ja vältida tulevaste eest |

## <a name="introductory-walkthrough"></a>Sissejuhatav kiirtutvustus
 Pääsete turbekeskus [Azure portaali](https://azure.microsoft.com/features/azure-portal/). [Portaali sisse logida](https://portal.azure.com), valige **Sirvi**, ja seejärel leidke **Turbekeskus** valik või paani **Turbekeskus** , mis varem kinnitatud portaali armatuurlaud.

![Turvalisus paani Azure'i portaalis][1]

Turbekeskus, saate määrata turbepoliitikate, jälgida turvalisus konfiguratsioone ja Turbeteatiste kuvamine.

### <a name="security-policies"></a>Turbepoliitikate

Saate määratleda poliitikate Azure'i tellimused ja ressursside rühma vastavalt oma ettevõtte turbenõuetele. Te saate ka kohandada neid tüüpi rakendusi kasutate või andmete tundlikkust iga tellimus. Näiteks ressurssidega või testimine võib olla erinevad turbenõuetele kasutatavad tootmise rakendused. Samuti rakendusi nagu PII reguleeritud andmetega võib olla vaja kõrgema taseme turvalisus.

> [AZURE.NOTE] Turbepoliitika, tellimuse tasemel või ressursside töörühma tasandil muutmiseks peate olema tellimuse omanik või selle kaasautori.

Enne **Turbekeskus** , valige oma tellimuste ja ressursside rühmade loendi paan **poliitika** .   

![Turbekeskus blade][2]

Valige **turbepoliitika** enne tellimuse poliitika üksikasju soovite vaadata.

![Turvalisus poliitika blade tellimus][3]

**Andmete kogumine** (vt eespool) võimaldab andmete kogumine turvalisuse poliitika. Lubada pakub.

- Igapäevane kontrollimine kõigi toetatud virtuaalse masinad turvalisus jälgimine ja soovitused.
- Saidikogumi turvalisus sündmuste jaoks analüüsi ja ohtu tuvastamine.

**Valige salvestusruumi konto müüginäitajaid piirkonna kohta** (vt ülal) saate valida iga piirkonna, kus teil on opsüsteemi, virtuaalmasinates salvestusruumi konto nende virtuaalmasinates kogutud andmete talletuskoht. Kui valite salvestusruumi konto iga piirkonna, luuakse see teie eest. Kogutud andmed on loogiliselt eraldatud teiste klientide andmete turvalisuse põhjustel.

> [AZURE.NOTE] Andmete kogumine ja valides salvestusruumi konto müüginäitajaid piirkonna kohta on konfigureeritud tellimuse tasemel.

**Vältimise poliitika** tera avamiseks valige **vältimise poliitika** (vt eespool). **Kuvada soovitusi** saate valida turbemeetmed, mida soovite jälgida ja soovitame vastavalt vajadustele turvalisus tellimuse ressursse.

Seejärel valige ressursirühma poliitika üksikasjade kuvamiseks.

![Poliitika blade ressursi turberühm][4]

**Pärimine** (vt ülal) abil saate määratleda ressursirühma nimega.

- Päritud (vaikeväärtus) mis tähendab kõik turbepoliitikate jaoks selle ressursirühma päritud tellimuse tase.
- Kordumatu mis tähendab, et ressursirühma on kohandatud turbepoliitika. Peate muutmiseks klõpsake jaotises **Kuva soovitused**.

> [AZURE.NOTE] Tellimuse poliitika ja ressursside rühmapoliitika taseme korral ressursside taseme rühmapoliitika ülimuslik.

### <a name="security-recommendations"></a>Turvalisus soovitused

 Turbekeskus analüüsib turvalisus olek võimalikud turvahaavatavusi tuvastamiseks oma Azure ressursse. Soovituste loendi juhendab teid protsessi konfigureerimine vajalik juhtelement. Näited:

- Ründevaratõrje tuvastamiseks ja eemaldada ründetarkvara ettevalmistamine
- Võrgu turberühmad ja reegleid juhtelement liiklust virtuaalmasinates konfigureerimine
- Ettevalmistamise web rakenduse tulemüürid aidata suunatud eest kaitsta oma veebirakenduste
- Rakendades puuduvad süsteemi värskendused
- Adressaatide valimiseks OS konfiguratsioone, mis vastavad soovitatav võrdlusandmeid?

Klõpsake paani **soovituste** loendi soovitusi. Klõpsake iga soovitus täiendavat teavet või toimingu probleemi lahendamiseks ette võtta.

![Azure'i turbekeskus turvalisus soovitused][5]

### <a name="resource-health"></a>Ressursi seisund

Paani **ressursside turvalisus seisund** kuvatakse üldine turvalisus asendi keskkonna ressursi tüüp, sh virtuaalmasinates, veebirakenduste ja muud ressursid.   

Valige rohkem teavet, sealhulgas tähtsusega kõik võimalikud tuvastatud loendi kuvamiseks **ressursside turvalisus seisund** paani ressursi tüüp. (**Virtuaalmasinates** valitakse järgmises näites).

![Ressursside seisund paan][6]

### <a name="security-alerts"></a>Turbeteatiste

 Turbekeskus automaatselt kogub, analüüsib ja ühendab Logi oma Azure ressursse, võrgu ja partnerilahenduste nagu ründevaratõrje programmid ja tulemüürid andmeid. Ohtude tuvastamisel luuakse turvalisus teatis. Näited tuvastamise.

- Rikutud virtuaalmasinates suhtlemine teadaolevad pahatahtlik IP-aadressid
- Täpsem abil Windowsi tõrketeavituse tuvastatud ründevara
- Jõuvõtete vastu virtuaalmasinates
- Integreeritud ründevaratõrje programmid ja tulemüürid turvaviipu

**Turbeteatiste** paani kuvatakse loendi tähtsuse teatised.

![Turbeteatiste][7]

Valige teatise kuvatakse rünnak ja soovitused migreerimisel ning probleemide lahendamisel selle kohta lisateavet.

![Turvalisus teatiste üksikasjad][8]

### <a name="partner-solutions"></a>Partnerite lahenduste

**Partnerite lahenduste** paani võimaldab teil jälgida lühiülevaade seisund olekut oma partnerite lahenduste integreeritud Azure tellimuse. Turbekeskus kuvatakse teatised, mis on pärit lahendused.

Valige paan **partnerilahenduste** . Tera avaneb kõigi ühendatud partnerite lahenduste loendit.

![Partnerite lahenduste][9]

## <a name="get-started"></a>Alustamine
Alustamine turbekeskus, peate Microsoft Azure'i tellimust. Azure'i tellimus on lubatud turbekeskus. Kui olete tellinud, te saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

 Pääsete turbekeskus [Azure portaali](https://azure.microsoft.com/features/azure-portal/). Lugege lisateavet [portaali dokumentatsiooni](https://azure.microsoft.com/documentation/services/azure-portal/) .

[Alustamine: Azure'i turbekeskus](security-center-get-started.md) kiiresti juhendab teil kontrollides turbe- ja turbekeskus poliitika-halduse komponendid.

## <a name="see-also"></a>Vt ka
Selles dokumendis võeti kasutusele turbekeskus, selle võtme võimaluste ja kuidas alustada. Lisateabe saamiseks vaadake järgmist:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus soovitusi Azure'i turbekeskus haldamine](security-center-recommendations.md) – siit saate teada, kuidas soovitusi aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastata.
- [Partnerite lahenduste ja Azure turbekeskus jälgimine](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
