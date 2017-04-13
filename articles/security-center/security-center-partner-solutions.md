<properties
   pageTitle="Azure'i Security Center partnerite lahenduste haldamine | Microsoft Azure'i"
   description="Selle dokumendi juhendab teid kuidas Azure'i turbekeskus võimaldab teil jälgida lühiülevaade seisund olekut oma partnerite lahenduste integreeritud Azure tellimuse."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Partnerite lahenduste ja Azure turbekeskus jälgimine

Selle dokumendi juhendab teid oma partnerite lahenduste Azure'i turbekeskus jälgida seisundi kohta.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil. See ei ole samm-sammult juhendi.

## <a name="monitoring-partner-solutions"></a>Partnerite lahenduste jälgimine

**Partnerite lahenduste** paanil **Turbekeskus** tera võimaldab teil jälgida lühiülevaade seisund olekut oma partnerite lahenduste integreeritud Azure tellimuse.
![Partnerite lahenduste paan][1]

**Partnerite lahenduste** paan kuvab arvu partnerite lahenduste ja nende lahenduste Kokkuvõte olek.

Partnerite lahenduste **olek** võib olla:

- Kaitstud (roheline) – ei ole seisundi küsimus.
- Vigane (punane) – on seisund küsimus, mis nõuab viivitamatut tähelepanu.
- Peatatud (oranž) aruandlus – lahendus on lõpetanud oma seisundi teatamine.
- Tundmatu kaitse olek (oranž) – lahendus seisundi pole teada nurjunud protsessi lisada uue ressursi olemasoleva lahenduse tõttu.
- Teatatud (Hall) – lahendus on aru midagi, ent lahenduse seisund võib olla teatamata, kui see on ainult ühendatud ja on endiselt juurutamine.

Kui puuduvad lahendused integreeritud tellimuse märgitakse paani on olemas lahendusi. **Partnerite lahenduste** paani valimine võimaldab teil **soovitused** tera juurutamiseks partneri lahendustele avamiseks.
![Partnerite lahenduste pole][2]

Partnerite lahenduste seisundi vaatamiseks tehke järgmist.

1. Valige paan **partnerilahenduste** . Tera avatakse teie ühendatud turbekeskus partnerite lahenduste loendit.
![Partnerite lahenduste][3]

2. Valige partneri lahenduse. Selles näites võimaldab valida **F5-WAF2** lahendus.  Tera avaneb on kuvatud partnerite lahenduste ja lahenduse oleku seotud ressursid. Valige **lahenduse konsooli** avamiseks partnerite halduse kogemus see lahendus.
![Partnerite lahenduste üksikasjad][4]

3. Minge tagasi **F5-WAF2** tera ja valige **Link rakendus**. **Link rakenduste** tera avaneb. Siin saate luua ühenduse ressursid partneri lahendus.
![Partnerite lahenduste ressursse link][5]

## <a name="see-also"></a>Vt ka
Selles dokumendis võeti kasutusele **Partnerite lahenduste** paanile Security Center. Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus soovitusi Azure'i turbekeskus haldamine](security-center-recommendations.md) – siit saate teada, kuidas soovitusi aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
