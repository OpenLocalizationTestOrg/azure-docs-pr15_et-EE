<properties
   pageTitle="Toimingute haldus komplekti (OMS) ülevaade | Microsoft Azure'i"
   description="Microsoft toimingute komplekti (OMS) on Microsofti pilvepõhise IT lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu.  Selles artiklis tuvastab kaasatud OMS erinevad teenused ja nende üksikasjalik sisu lingid."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Mis on toimingud halduse komplekti (OMS)?

Microsoft toimingute komplekti (OMS) on Microsofti pilvepõhise IT lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu.  Kuna OMS on pilvepõhist teenust, saate määrata selle tööle kiiresti, mille minimaalne taristu teenused.  Uued funktsioonid edastatakse automaatselt salvestamist saate poolelioleva hooldus ja maksab versioonitäiendus.

Lisaks väärtuslik teenuste oma, saate OMS integreerida System Center komponendid, nt System Center Operations Manager laiendada oma olemasoleva halduse investeeringud pilve.  System Center ja OMS saavad anda täielikku hübriid halduse kogemuse koos töötada.

Järgmistes jaotistes on erinev väärtus alade OMS ja teenuseid, mis neid rakendada kõrge taseme kirjeldus.  Viidata saate ülevaate erinevate osade OMS OMS arhitektuur enne üle vaadata iga üksikasjalikud dokumendid.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Ülevaate ja analüüsi](media/operations-management-suite-overview/icon-insight-analytics.png) Ülevaate ja analüüsi

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) aitab teil koguda, oleksid, otsida ja Logi ja jõudluse andmeid, mida operatsioonisüsteemide ja rakenduste kohta. Siit leiate reaalajas töö ülevaated hõlpsasti analüüsida miljonid kirjed kõigis oma töökoormus ja serverid sõltumata nende füüsilise asukoha integreeritud otsing ja kohandatud armatuurlaudade abil.

Saate lisada hõlpsalt lahenduste Log Kasutusanalüüsi, mis määratlevad andmete kogumine ja analüüsi loogika.  Lahendused võivad sisaldada täiendavaid funktsioone, mis saadetakse automaatselt agentide koos minimaalsete või pole konfigureerimine.  Lisaks üksikute lahenduste analüüsi tööriista abil saate teha kohandatud otsinguid kogu andmekomplekti üle selleks, et oleksid andmete süsteemide ja rakenduste vahel.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automaatika ja juhtimine](media/operations-management-suite-overview/icon-automation-control.png) Automaatika ja juhtimine

Azure'i automaatika automatiseerib haldus protsesside [tegevusraamatud](../automation/automation-runbook-types.md) , mis põhinevad PowerShelli ja käivitada Azure pilveteenuses.  Tegevusraamatud pääsevad toode või teenus, mida saab hallata PowerShelliga, sh ressursse teiste pilved nagu Amazon Web Services (AWS).  Tegevusraamatud saab teostada ka teie kohaliku andmekeskuse serveri haldamiseks kohalikke ressursse.

Azure'i automaatika pakub konfiguratsiooni haldus – [PowerShelli DSC](../automation/automation-dsc-overview.md).  Saate luua ja hallata DSC ressursse, mis on majutatud Azure ja cloud ja kohapealsete süsteemide määratlemine ja nende konfiguratsiooni automaatselt Jõusta rakendamine.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Kaitse ja taastamine](media/operations-management-suite-overview/icon-protection-recovery.png) Kaitse ja katastroofiabi

[Azure varukoopia](http://azure.microsoft.com/documentation/services/backup) kaitseb teie rakenduse andmeid ja säilitab aastat ilma mis tahes investeeringud ja minimaalsete kulud.  See saab varundada andmeid füüsilise ja virtuaalse Windows Serveri Lisaks rakenduse töökoormus nt SQL serveri ja SharePoint.  Seda saab kasutada ka System Center andmete kaitse Manager (DPM) kaitstud andmed kopeerida Azure koondamise ja pikaajaline salvestusruumi.

[Azure'i saidi taastamine](http://azure.microsoft.com/documentation/services/site-recovery) aitab teie süsteemi järjepidevuse ja suurõnnetuste taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja asutusesisese Hyper-V virtuaalmasinates, VMware virtuaalmasinates ja füüsilise Windows/Linux serveri. Saate ise masinad teisene andmekeskuse või pikendada oma andmekeskuse imitatsiooniga need Azure. Saidi taastamine pakub ka lihtsa Tõrkesiirde ja taastamise töökoormus. See katastroof taastamine menetlustele, nt SQL serveri AlwaysOn integreerub ja pakub taastamise lihtne Tõrkesiirde töökoormus, mis on mitmetasandilise mitme arvutites.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS Turve ja nõuetele vastavus](media/operations-management-suite-overview/icon-security-compliance.png) Turve ja nõuetele vastavus
Turve ja nõuetele vastavus aitab teil tuvastada, hinnata ja oma infrastruktuuri turvalisuse riske leevendada.  Nende funktsioonide OMS rakendatakse mitme lahenduste Log Analytics, mis analüüsida Logi andmed ja konfiguratsiooni agent Systemsi aidata teil keskkonna poolelioleva turvalisuse tagamiseks.

- [Turvalisus ja Audit lahenduse](oms-security-getting-started.md ) kogub ja analüüsib turvalisus üritused operatsioonisüsteemides hallatavate kahtlase tegevuse tuvastamiseks.
- [Ründevaratõrje lahenduse](log-analytics-malware.md ) aruandeid ründevaratõrje kaitse operatsioonisüsteemides hallatavate olekut.  
- Süsteemi uuendused lahenduse sooritab analüüsi turvalisuse värskendused ja muu värskendamist operatsioonisüsteemides oma hallatavate nii, et te kerge vaevaga tuvastada süsteemide nõudva lappimine.


## <a name="next-steps"></a>Järgmised sammud
- Lisateavet [Log Kasutusanalüüsi](http://azure.microsoft.com/documentation/services/log-analytics).
- Lisateavet [Azure automatiseerimine](../automation/automation-intro.md).
- Lisateavet [Azure'i varukoopiad](http://azure.microsoft.com/documentation/services/backup).
- Lisateavet [Azure'i saidi taastamine](http://azure.microsoft.com/documentation/services/site-recovery).
