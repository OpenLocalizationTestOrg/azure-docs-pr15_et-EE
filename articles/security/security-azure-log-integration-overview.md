<properties
   pageTitle="Sissejuhatus Microsoft Azure'i log integreerimine | Microsoft Azure'i"
   description="Teavet integreerimine Azure Logi selle võtme võimaluste ja kuidas see toimib."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Sissejuhatus Microsoft Azure'i log integreerimine (eelvaade)

Teavet integreerimine Azure Logi selle võtme võimaluste ja kuidas see toimib.

## <a name="overview"></a>Ülevaade

Service (PaaS) ja infrastruktuuri teenus (IaaS) majutatud Azure platvormi loovad turvalisus logid suurt hulka andmeid. Need logid sisaldavad olulist teavet, mis pakub ärianalüüsi ja võimas teadmisi poliitika rikkumised, sise- ja ohtude, nõuetele vastavuse probleemid ja võrgu, host ja kasutajale tegevuse kõrvalekaldeid.

Azure'i log integreerimine võimaldab teil integreerida kohapealse Turbeteave ja sündmuse Management (SIEM) süsteemid töötlemata logid oma Azure ressursse. Integreerimine Azure log kogub *(WAD)* oma Windows Azure'i diagnostika virtuaalmasinates, samuti diagnostika partnerilahenduste näiteks on Web rakenduse tulemüüri (WAF) kaudu. Sellise integratsiooni annab ühendatud armatuurlaua oma varasid, kohapealse või pilves, nii, et saate liitmine, oleksid, analüüsida ja Teavita turvalisus sündmuste jaoks.

![Azure'i log integreerimine][1]

## <a name="what-logs-can-i-integrate"></a>Millised logid saate integreerida?

Azure'i toodab olulisel logimine iga Azure'i teenus. Need logid on kaks peamist tüüpi kategoriseeritud:

- **Järelevalve logid**, mis aitavad muuta paremini nähtavaks tegevusega Azure'i ressursihaldur loomine, värskendamine ja kustutamine. Azure'i auditilogid on kujutatud seda tüüpi log.
- **Andmete lennuk logid**, mis on Azure ressurss kasutamist osana astmes sündmuste nähtavus. Näited seda tüüpi log on Windowsi sündmuste süsteemi, Turve ja rakenduse logib virtuaalse masina.

Azure'i log integreerimine toetab praegu integreerimine Azure auditilogid, virtuaalse masina logid ja Azure turbekeskus teatised.

Kui teil on küsimusi integreerimine Azure Log, saatke meile e-posti [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Järgmised sammud

Selles dokumendis võeti kasutusele integreerimine Azure log. Azure'i log integratsioon ja logid toetatud tüüpi kohta leiate lisateavet järgmistest teemadest järgmist.

- [Microsoft Azure'i Log integreerimine Azure logide (eelvaade)](https://www.microsoft.com/download/details.aspx?id=53324) – allalaadimiskeskus üksikasjad, süsteeminõuded ja installi juhised integreerimine Azure Logi sisse.
- [Azure'i log integreerimine kasutamise alustamine](security-azure-log-integration-get-started.md) – selles õpetuses juhendab teid installi ja Azure log integreerimine integreerimine Azure storage, Azure'i auditilogid ja turbekeskuse teatiste logid.
- [Partneri konfigureerimistoimingute](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – sellest ajaveebipostitusest näitab, kuidas konfigureerida Azure log integreerimine partnerite lahenduste Splunk, HP ArcSight ja IBM QRadari töötamiseks.
- [Azure'i log integreerimine korduma kippuvad küsimused (KKK)](security-azure-log-integration-faq.md) – see KKK vastuseid küsimustele integreerimine Azure log.
- [Azure Integrated Security Center teatiste sisse logida integreerimine](../security-center/security-center-integrating-alerts-with-log-integration.md) – selle dokumendi näitab, kuidas sünkroonida turbekeskus teatised, virtuaalse masina turvalisus sündmuste logi analytics või SIEM lahenduse Azure'i diagnostika- ja Azure auditilogid, kogutud koos.
- [Uued funktsioonid Azure diagnostika- ja Azure auditilogid](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – sellest ajaveebipostitusest tutvustab teile Azure'i auditilogid ja muud funktsioonid, mis aitavad teil saada teadmisi oma Azure ressursse toimimise.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
