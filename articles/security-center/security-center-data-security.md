<properties
   pageTitle="Azure'i turbekeskus andmete turvalisus | Microsoft Azure'i"
   description="Selles dokumendis selgitatakse, kuidas andmed on hallatavad ja Azure Security Center kaitstud."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Azure'i turbekeskus andmete turvalisus
Kliendid vältida, tuvastada ja Azure turbekeskus kogub ja töötleb andmeid oma Azure ressursse, sh konfiguratsiooniteavet, metaandmete, sündmuselogide, ohtude, vastata krahhi gigabaiti ja palju muud. Teeme suured kohustused privaatsust ja turvet andmete kaitsmiseks. Microsoft järgib nõuetele vastavus ja turve range – kodeerimine teenuste kaudu. 

Selles artiklis selgitatakse, kuidas andmed on hallatavad ja Azure turbekeskus kaitstud.

## <a name="data-sources"></a>Andmeallikad

Azure'i turbekeskus analüüsib andmeid järgmistest allikatest:

- Azure'i teenused: Loeb teavet Azure'i teenuste konfigureerimine juurutanud suheldes selle service ressursi pakkuja.
- Võrguliiklust: Loeb valimisse võrgu liikluse metaandmete Microsofti infrastruktuuri, nagu allikas/sihtkoht IP/port paketi suurus ja võrgu Protocol (protokoll).
- Partnerite lahenduste: Kogub Turbeteatiste integreeritud partnerilahenduste, nt tulemüürid ja ründevaratõrje lahendusi. Azure'i turbekeskus salvestusruumi, praegu asub Ameerika Ühendriikide andmed on salvestatud.
- Teie Virtuaalmasinates: Azure'i turbekeskus saate konfiguratsiooniteavet ja turvalisuse sündmuste, nt Windowsi sündmuste kohta teavet koguda ja auditi logid, IIS-i logid Logi sõnumeid ja krahhi mälutõmmise failid oma virtuaalmasinates abil andmete kogumine agentide. "Haldamise andmete kogumine" allpool olevast jaotisest täiendavad üksikasjad.  

Lisaks Turbeteatiste, soovitused ja turvalisuse seisund oleku kohta teave on talletatud Azure'i turbekeskus salvestusruumi, praegu asub Ameerika Ühendriikides. See teave võib sisaldada seotud konfiguratsiooniteavet ja turvalisuse sündmuste kogutud oma virtuaalmasinates vastavalt vajadusele teile turbeteates, soovitus või turvalisus seisund olek.

## <a name="data-protection"></a>Andmekaitse

**Andmete eraldamine**: andmeid hoitakse eraldi loogiliselt iga komponendi kogu teenus. Kõik andmed on sildistatud organisatsiooni kohta. See sildistamine jätkub kogu andmete elutsükli ja see on jõustatud igal kiht teenuse. Lisaks teie virtuaalmasinates kogutud andmed on salvestatud teie salvestusruumi kontod.

**Juurdepääs andmetele**: soovituste turvalisus ja ohtude uurida, et Microsofti töötajad pääsevad Azure teenused, sh krahh gigabaiti analüüsida või kogutud teavet. Krahh gigabaiti ja protsessi loomine sündmused tahtmatult võivad sisaldada kliendiandmeid või oma virtuaalmasinates isikuandmeid. Järgime [Microsoft Online Services tingimused](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ja [Privaatsusavaldus](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), mis olek, et Microsoft ei kliendiandmete kasutamiseks või tuletada andmeid on reklaami või muu sarnase trükikojas eesmärkidel. Kasutatakse ainult kliendiandmete vastavalt vajadusele Azure teenuseid, sealhulgas nende teenuste osutamisega seotud eesmärgid pakkumiseks. Saate säilitada kliendiandmete kõik õigused.

**Andmete kasutamine**: Microsoft kasutab mustrite ja ohtu ärianalüüsi mitme rentniku vahel näha täiustamiseks meie vältimine ja tuvastamise võimalusi; me seda teha vastavalt privaatsustingimuste nõuetelevastavust, mis on kirjeldatud meie [Privaatsusavaldus](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Andmete asukoht**: iga piirkonna kohta, kus töötab virtuaalmasinates jaoks on määratud salvestusruumi konto. See võimaldab teil sama piirkonna virtuaalse masina kogutud andmed andmete talletamiseks. Andmed, sh krahh dump faile, salvestatakse teie salvestusruumi konto püsivalt. Teenuse talletatakse ka teavet Turbeteatiste, sh teatiste integreeritud partnerilahenduste, soovitusi ja turvalisuse seisund olek Azure'i turbekeskus salvestusruumi, praegu asub Ameerika Ühendriikides.

## <a name="managing-data-collection-from-virtual-machines"></a>Andmete kogumine virtuaalmasinates haldamine

Kui otsustate lubada Azure turbekeskus, on andmete kogumine sisse lülitatud iga tellimuste. Andmete kogumine Azure'i turvalisus armatuurlauale jaotises "Turbepoliitika" välja lülitada. Kui andmete kogumine on sisse lülitatud, Azure'i turbekeskuse sätted Azure jälgimise Agent kõikidele toetatud virtuaalmasinates ja uusi faile, mis on loodud. Azure'i turvalisus jälgimise laiend otsib erinevate turvalisus seotud konfiguratsioone ja sündmuste Jälgi seda [Windowsi sündmuse jälitamise](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Lisaks operatsioonisüsteemi tõstab sündmuste logi sündmuste käigus töötab seade. Nende andmete näited: operatsioonisüsteemi tüüp ja versioon, opsüsteem süsteemi logid (Windowsi sündmuselogide) töötavate protsesside, arvuti nimi, IP-aadressi sisseloginud kasutaja ja rentniku ID-ga. Azure'i jälgimise Agent loeb sündmuselogi kirjeid ja ETW jälgi ja kopeerib need kontole salvestusruumi analüüsi jaoks. 

Iga piirkonna, kus teil on virtuaalmasinates töötab, kui andmed kogutud kaudu virtuaalmasinates, mis on talletatud sama piirkonna jaoks määratud salvestusruumi konto. See muudab lihtne hoida andmed samas piirkonnas privaatsuse ja andmete suveräänsuse eesmärgil. Saate konfigureerida Azure turvalisus armatuurlauale jaotises "Turbepoliitika" salvestusruumi kontod iga piirkonna jaoks.

Azure'i jälgimise Agent kopeerib ka krahh gigabaiti salvestusruumi kontole.  Azure'i turbekeskus kogub efemeersed krahh dump failide koopiad ja analüüsib nende jaoks kasutada katsete ja eduka kompromisse.  Azure'i turbekeskus sooritab analüüsi samas geograafilised piirkonnas salvestusruumi konto ja kustutab efemeersed koopiaid kui analüüsi on lõpule viidud.

Andmete kogumine virtuaalmasinates igal ajal, mis eemaldab jälgimise agentide varem installitud Azure'i turbekeskus, saate keelata.


## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut andmete hallatavate ja Azure turbekeskus kaitstud. Azure'i turbekeskus kohta leiate lisateavet järgmistest teemadest.

- [Azure'i turvalisus Center plaanimine ja kasutusjuhend](security-center-planning-and-operations-guide.md) – saate teada, kuidas plaanimine ja kujundamine vastu võtta Azure'i turbekeskus mõista.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastamine
- [Partnerite lahenduste ja Azure turbekeskus jälgimine](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – korduma kippuvad küsimused teenuse kasutamise kohta otsimine
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Leia ajaveebi Azure Turve ja nõuetele vastavuse kohta
