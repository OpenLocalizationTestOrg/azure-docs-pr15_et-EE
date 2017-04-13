<properties
    pageTitle="Log Analytics ülevaade Azure storage andmete kogumine | Microsoft Azure'i"
    description="Azure'i ressursid saate kirjutada logid ja mõõdikute Azure storage konto sageli Azure'i diagnostika abil. Log Analytics indekseerida andmed ja veenduge, et see otsitavate."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Azure'i salvestusruumi andmete kogumise Log Analytics ülevaade

Mitme Azure'i ressursid on võimalik kirjutada logid ja mõõdikute Azure storage konto. Log Analytics saate kasutada neid andmeid ja oleks lihtsam jälgida oma Azure ressursse.

Azure'i salvestusruumi kirjutamise ressursi võib Azure diagnostika või teil on oma viis andmete kirjutamiseks. Andmed võib kirjutada eri vormingutes ühte järgmisest kohast:

+ Azure'i tabeli
+ Azure'i bloobimälu
+ EventHub

Log Analytics toetab Azure teenuseid, mis on [Azure diagnostikalogid](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)andmete kirjutamiseks. Lisaks toetab Log Analytics muud teenused, mida väljund logid ja mõõdikute erineval kujul ning asukohad.  

>[AZURE.NOTE] Tavaline Azure andmete määra salvestusruumi ja tehingud diagnostika saatmisel salvestusruumi konto ning kui Log Analytics loeb andmeid salvestusruumi kontolt teilt tasu.

![Azure'i salvestusruumi skeem](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Toetatud Azure ressursid

Log Analytics saate koguda andmeid Azure järgmistest allikatest:

| Ressursi tüüp | Logide (diagnostika kategooriad) | Log Analytics lahendus |
| --------------------------------------- | -------------------------------- | --------------- |
| Rakenduse ülevaated | Kättesaadavus <br> Kohandatud sündmused <br> Erandid <br> Taotlused <br> | Rakenduse ülevaated (eelvaade) |
| API haldus | | *ükski* (Eelvaade) |
| Automatiseerimine <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (eelvaade) |
| Võtme Vault <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (eelvaade) |
| Rakenduse lüüsi <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (eelvaade) |
| Võrgu turberühm <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (eelvaade) |
| Teenuse struktuuri                          | ETWEvent <br> Funktsionaalseid sündmuse <br> Usaldusväärne näitleja sündmuse <br> Usaldusväärne teenus sündmuse| ServiceFabric (eelvaade) |
| Virtuaalmasinates | Linux Logi <br> Windowsi sündmuste <br> IIS-i Logi <br> Windowsi ETWEvent | *ükski* |
| Web rollid <br> Töötaja rollid | Linux Logi <br> Windowsi sündmuste <br> IIS-i Logi <br> Windowsi ETWEvent | *ükski* |

>[AZURE.NOTE] Jälgimine Azure'i virtuaalmasinates (Linux ja Windows), soovitame [Log Analytics VM laiend](log-analytics-azure-vm-extension.md)installimise. Agent pakub teie virtuaalmasinates põhjalikult teadmisi, kui kasutate diagnostika kirjutada mäluruumi kui.

Aita tähtsuse täiendavad logisid OMS analüüsimiseks, hääletamine meie [leht Tagasiside](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy).


- Vt lisateavet selle kohta, kuidas Log Analytics saate lugeda logid Azure teenused, mis toetavad [Azure diagnostikalogid](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) [analüüsida Azure'i diagnostikalogid Log Analytics abil](log-analytics-azure-storage-json.md)
  - Azure'i võtme Vault
  - Azure'i automatiseerimine
  - Rakenduse lüüsi
  - Võrgu turberühmad
- [Kasutage bloobimälu IIS ja tabelimälu sündmuste jaoks.](log-analytics-azure-storage-iis-table.md) Lisateavet selle kohta, kuidas Log Analytics lugeda logid teenuste Azure'i tabelimälu või IIS-i logid Bloobivahemälu salvestusruumi, kirjutada selle kirjutamine diagnostika sh:
  - Teenuse struktuuri
  - Web rollid
  - Töötaja rollid
  - Virtuaalmasinates


Rakenduse ülevaateid on privaatne eelvaade ja seda kasutab pidev ekspordi Bloobivahemälu salvestusruumi. Liituda privaatse eelvaade, võtke ühendust oma Microsofti Account meeskonnatöö või viidata [tagasiside saidi](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights)üksikasjad.

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i analüüsida diagnostikalogid Log Analytics abil](log-analytics-azure-storage-json.md) lugemise logid Azure'i teenuste selle kirjutamine diagnostika bloobimälu JSON-vormingus.
- [Kasutamine bloobimälu IIS ja tabelimälu sündmuste](log-analytics-azure-storage-iis-table.md) logide lugemiseks Azure teenuste selle kirjutamine diagnostika tabelimälu või IIS-i logid Bloobivahemälu salvestusruumi kirjutada.
- [Luba lahenduste](log-analytics-add-solutions.md) annab ülevaate andmed.
- [Otsingu päringute kasutamine](log-analytics-log-searches.md) andmete analüüsimiseks.
