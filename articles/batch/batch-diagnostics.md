<properties
   pageTitle="Azure'i paketi diagnostikalogimine | Microsoft Azure'i"
   description="Salvestamine ja analüüsida diagnostika sündmuste Azure'i paketi konto ressursid nagu kaustu ja tööülesandeid."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure'i paketi diagnostikalogimine

Sarnaselt palju Azure teenuseid, paketi teenuse eraldab sündmuste logi teatud ressursid ressursi elu jooksul. Saate lubada Azure'i paketi diagnostikalogid sisse üritused ressursid nagu kaustu ja tööülesanded ja seejärel kasutage logid diagnostika hindamise ja järelevalve. Saate luua sündmusi, näiteks pool, kausta kustutamine, toimingu käivitamine, tööülesande lõpule viidud ja teised kaasatud paketi diagnostikalogid.

>[AZURE.NOTE] Selles artiklis käsitletakse logimine sündmuste paketi konto ressursid ise, mitte töö ja tööülesande väljundi andmeid. Oma ülesannete ja väljundandmete talletamise kohta leiate teemast [püsivad Azure'i paketi töö ja tööülesannete väljundi](batch-task-output.md).

## <a name="prerequisites"></a>Eeltingimused

* [Azure'i paketi konto](batch-account-create-portal.md)

* [Azure'i salvestusruumi konto](../storage/storage-create-storage-account.md#create-a-storage-account)

  Paketi diagnostikalogide püsib, peate looma konto Azure Storage, kus Azure'i salvestab logid. Saate määrata selle konto salvestusruumi kui te [diagnostikalogimise lubamiseks](#enable-diagnostic-logging) paketi konto jaoks. Saate määrata, kui lubate Logi saidikogumi mäluruumi konto pole sama, mis on lingitud salvestusruumi konto [rakenduse paketid](batch-application-packages.md) ja [tööülesande väljundi püsimine](batch-task-output.md) artiklites.

  >[AZURE.WARNING] Olete **lisandu** Azure Storage konto talletatud andmed. See hõlmab diagnostikalogid, mida selles artiklis käsitletakse. Pidage meeles teie [log säilituspoliitika](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)kujundamisel.

## <a name="enable-diagnostic-logging"></a>Diagnostikalogimise lubamiseks

Diagnostikalogimise vaikimisi paketi konto jaoks pole lubatud. Selgesõnaliselt lubama diagnostikalogimine iga paketi konto, mida soovite jälgida:

[Kuidas lubada saidikogumi Diagnostikalogid.](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Soovitame teil lugeda kogu [Ülevaade Azure Diagnostikalogid](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikkel, et saada aru mitte ainult, kuidas lubada logimine, kuid log kategooriad toetatud erinevate Azure'i teenuste. Näiteks Azure'i paketi toetab praegu üks log kategooria: **Teenuste logid**.

## <a name="service-logs"></a>Teenuste logid

Azure'i paketi teenuste logid sisaldavad sündmuste kiiratava Azure'i paketi teenuse paketi ressursi näiteks pool või tööülesande kestuse ajal. Iga sündmuse kiiratava paketi talletatakse määratud salvestusruumi konto JSON-vormingus. See on näiteks keha valimi **rakenduskausta sündmuse loomine**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Iga sündmuse keha asub .json faili määratud Azure Storage konto. Kui soovite logid otse juurde pääseda, võiksite üle vaadata [skeemi Diagnostikalogid salvestusruumi konto](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Sündmuste logi teenuse

Paketi teenuse praegu eraldab teenuse Logi järgmised sündmused. Selles loendis ei pruugi olla täielik, kuna täiendavat sündmused on lisatud Kuna artiklit viimati värskendati.

| **Sündmuste logi teenuse** |
| ------------------ |
| [Kausta loomine][pool_create] |
| [Kausta kustutamine käivitamine][pool_delete_start] |
| [Kausta kustutamine lõpetamine][pool_delete_complete] |
| [Kausta suuruse käivitamine][pool_resize_start] |
| [Kausta suuruse muutmise lõpetamine][pool_resize_complete] |
| [Toimingu käivitamine][task_start] |
| [Tööülesande lõpetamine][task_complete] |
| [Tööülesande fail][task_fail] |

## <a name="next-steps"></a>Järgmised sammud

Lisaks talletamine diagnostika sündmuste Azure Storage konto, saate ka voona paketi teenuse sündmuste [Azure'i sündmuse jaoturi](../event-hubs/event-hubs-what-is-event-hubs.md), ja [Azure Log Analytics](../log-analytics/log-analytics-overview.md)saata.

* [Voona Azure'i Diagnostikalogide sündmuse jaoturi abil](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Paketi diagnostika sündmusi väga paindlik andmete sissepääsu teenuse sündmuse jaoturi voona. Sündmuse jaoturi saate neelata miljoneid sündmuste sekundis, mida saate siis muuta ja salvestada mis tahes reaalajas analytics pakkuja.

* [Azure'i diagnostikalogid abil Log Analytics analüüs](../log-analytics/log-analytics-azure-storage-json.md)

  Saatke oma diagnostikalogid Log Analytics, kus saate need toimingud halduse komplekti (OMS) portaalis analüüsida või eksportimiseks analüüsi Power BI või Excelis.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
