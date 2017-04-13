<properties
   pageTitle="Toimingute haldus komplekti turvalisus ja Audit lahenduse andmete turvalisus | Microsoft Azure'i"
   description="Selles dokumendis selgitatakse, kuidas andmed on hallatavad ja kaitstud toiminguid haldus komplekti turvalisus ja auditi lahendus."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Toimingute haldus komplekti turvalisus ja Audit lahenduse andmete turvalisus

Selleks, et kliendid vältida, avastada ja ohtude vastata, [toimingud halduse komplekti (OMS) turbe- ja auditilogi lahenduse](operations-management-suite-overview.md) kogub ja töötleb oma ressursse, mis sisaldab andmeid:

- Turvalisuse sündmuste logi
- Sündmuse jälgimine for Windows (ETW) sündmused
- AppLockeri Auditeerimispoliitika sündmused
- Windowsi tulemüüri log
- Täiustatud ohtu Kasutusanalüüsi sündmused
- Alusjoone hindamise tulemusi
- Ründevaratõrje hindamise tulemusi
- Tulemuste/värskenduspaik.
- Syslogs voole selgesõnaliselt lubatud agent kohta

Teeme suured kohustused privaatsust ja turvet andmete kaitsmiseks. Microsoft järgib nõuetele vastavus ja turve range – kodeerimine teenuste kaudu.
Selles artiklis selgitatakse, kuidas andmed on hallatavad ja kaitstud OMS turvalisus ja Audit lahendus.

## <a name="data-sources"></a>Andmeallikad

OMS turvalisus ja Audit lahenduse analüüside Virtuaalmasinates ja füüsilise arvutites, kuhu on installitud OMS Agent. OMS turvalisus ja Audit lahenduse saate koguda konfiguratsiooniteavet turvalisus sündmuste, nt Windowsi sündmuste, auditilogid, IIS-i logid ja Logi sõnumite kohta. Nende andmete näited: operatsioonisüsteemi tüüp ja versioon töötab protsessid, arvuti nimi, IP-aadressi sisseloginud kasutaja ja rentniku ID-ga.  

## <a name="data-protection"></a>Andmekaitse

**Andmete eraldamine**: andmeid hoitakse eraldi loogiliselt iga komponendi kogu teenus. Kõik andmed on sildistatud organisatsiooni kohta. See sildistamine jätkub kogu andmete elutsükli ja see on jõustatud igal kiht teenuse. 

**Juurdepääs andmetele**: Microsofti töötajad pääsevad soovituste turvalisus ja ohtude uurida, analüüsitud teenuste või kogutud teavet. Järgime [Microsoft Online Services tingimused](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ja [Privaatsusavaldus](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), mis olek, et Microsoft ei kliendiandmete kasutamiseks või tuletada andmeid on reklaami või muu sarnase trükikojas eesmärkidel. Microsofti töötajad pääsevad turvalisus soovitusi ja ohtude uurida, analüüsitud teenuste või kogutud teavet. Kasutatakse ainult kliendiandmete vastavalt vajadusele Azure teenuseid, sealhulgas nende teenuste osutamisega seotud eesmärgid pakkumiseks. Saate säilitada oma andmeid kõik õigused.

**Andmete kasutamine**: Microsoft kasutab mustrite ja ohtu ärianalüüsi mitme rentniku vahel näha täiustamiseks meie vältimine ja tuvastamise võimalusi; me seda teha vastavalt privaatsustingimuste nõuetelevastavust, mis on kirjeldatud meie [Privaatsusavaldus](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Andmete asukoht on konfigureeritud tasemel OMS tööruum tööruumi loomisel, mis on algse OMS turvalisus ja Audit konfigureerimise käigus.

## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut andmete hallatavate ja OMS kaitstud. OMS turvalisus ja Audit lahenduse kohta leiate lisateavet järgmistest teemadest.

- [Toimingute haldus komplekti (OMS) ülevaade](operations-management-suite-overview.md)
- [Jälgimine ja toimingute haldus komplekti turvalisus ja Audit lahenduse Turbeteatiste reageerimine](oms-security-responding-alerts.md)
- [Toimingute haldus komplekti turvalisus ja Audit lahenduse jälgimisega seotud ressursid](oms-security-monitoring-resources.md)

