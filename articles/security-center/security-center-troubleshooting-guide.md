<properties
   pageTitle="Azure'i turbekeskus Tõrkeotsingujuhendi | Microsoft Azure'i"
   description="Selle dokumendi aitab Azure'i turbekeskus probleemide tõrkeotsing."
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
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Azure'i turbekeskus tõrkeotsingu juhend
Sellest juhendist leiavad infotehnoloogia (IT) asjatundjate, teabe turvalisus ärianalüütikud ja pilvepõhise administraatoritele, kelle ettevõtted kasutate Azure turbekeskus ja vaja turbekeskus seotud probleemid.

## <a name="troubleshooting-guide"></a>Tõrkeotsing juhend
Sellest juhendist selgitatakse, kuidas tõrkeotsing turbekeskus seotud probleemid. Enamik tehtud turbekeskus tõrkeotsingu toimub vaadates esimese nurjunud komponendi [Auditilogi](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) kirjed. Auditilogide, kaudu saate määrata:

- Milliseid toiminguid ei toimunud
- Algataja toiming
- Millal ilmnes selle toimingu
- Toimingu olek
- Muud atribuute, mis võivad aidata teil uurimine toiming

Auditilogi sisaldab kõigi kirjutamine toimingutest (panna, postituse, DELETE) oma ressursse, aga ei sisalda toimingud (saada).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Jälgimisega seotud agent installimine Windowsi tõrkeotsing

Andmete kogumine teostamiseks kasutatakse turbekeskus, agent jälgimine. Pärast andmete kogumine on lubatud ja agent on õigesti target arvutisse installitud, tuleks need protsessid täitmine:

- ASMAgentLauncher.exe - Azure'i Agent jälgimine 
- ASMMonitoringAgent.exe - laiend Azure turvalisus jälgimine
- ASMSoftwareScanner.exe – Azure skannimine haldur

Azure'i turvalisus jälgimise laiend otsib erinevate turvalisuse oluline konfiguratsiooni ja kogub turvalisus logid virtuaalse masina. Paikade skannerina kasutatakse skannimine ülemus.

Kui installimine on edukalt näha peaks olema umbes üks auditilogide sihtrühmade VM all kirje:

![Auditilogide](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Saate hankida Lisateavet installitoimingut lugedes agent logid, asub *%systemdrive%\windowsazure\logs* (näide: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Kui Azure turvalisus Center Agent ei tööta, peate taaskäivitage target VM, kuna lõpetada ja alustada agent käsku pole.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Linux jälgimisega seotud agenti installi tõrkeotsing
Tõrkeotsingul VM agenti installi Linuxi veenduge laiendamine on/var/teegi/waagent / alla laaditud. Saate kasutada käsu alla, et kontrollida, kas see on installitud.

`cat /var/log/waagent.log` 

Muud logi faile, mis võimaldab teil läbi vaadata tõrkeotsingu otstarve on: 

- /var/log/mdsd.ERR
- / var/log/Azure'i /

Töö süsteem peaksite mdsd protsessi ühenduse TCP 29130. See on Logi, suhtlemine mdsd protsess. Allpool käsu abil saate kinnitada seda käitumist:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Microsofti toe poole pöördumist

Mõned probleemid, saate kindlaks teha, kasutades suuniste antud selle artikli teistele leiate ka dokumenteerida turbekeskus avalik [Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Juhul kui vajate veel tõrkeotsing, saate avada uue tugiteenuse taotluse Azure portaali kaudu, nagu allpool näidatud: 

![Microsofti tugiteenuste](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut Azure'i turbekeskus turbepoliitikate konfigureerimise kohta. Azure'i turbekeskus kohta leiate lisateavet teemast järgmist:

- [Azure'i turvalisus Center plaanimine ja kasutusjuhend](security-center-planning-and-operations-guide.md) – saate teada, kuidas plaanimine ja kujundamine vastu võtta Azure'i turbekeskus mõista.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastamine
- [Partnerite lahenduste ja Azure turbekeskus jälgimine](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – korduma kippuvad küsimused teenuse kasutamise kohta otsimine
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Leia ajaveebi Azure Turve ja nõuetele vastavuse kohta
