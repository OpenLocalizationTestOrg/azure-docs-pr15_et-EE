<properties
    pageTitle="Azure'i kuvari autoscaling levinud mõõdikute. | Microsoft Azure'i"
    description="Siit saate teada, millised mõõdikute kasutatakse tavaliselt autoscaling oma pilveteenustega, Virtuaalmasinates ja veebirakenduste."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure'i kuvari autoscaling levinud mõõdikud

Azure'i kuvari autoscaling võimaldab mastaapimiseks töötava eksemplaride arv üles või alla, telemeetria andmete (mõõdikute). Selles dokumendis kirjeldatakse levinud mõõdikute, mida võiksite kasutada. Azure'i portaalis pilveteenustega ja serveri parkides, saate valida meetermõõdustik ressursi skaala järgi. Siiski saate mis tahes meetermõõdustik eri ressursi mastaapida ka valida.

Siin on andmed kohta, kuidas leida ja soovite mastaapimiseks mõõdikud loendis. Järgmine kehtib skaleerimist virtuaalse masina skaala komplektid ka.

## <a name="compute-metrics"></a>Arvutage mõõdikud
Vaikimisi Azure VM v2 kaasas diagnostika laiend, mis on konfigureeritud ja neil on järgmised mõõdikud sisse lülitatud.

- [Külalisena mõõdikute Windows VM v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Külalisena mõõdikute Linux VM v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Saate kasutada funktsiooni `Get MetricDefinitions` CLI/API/PoSH kuvamiseks saadaval oma VMSS ressursi mõõdikute. 

Kui kasutate komplekti VM skaala ja mõõdiku kindla loendis ei kuvata, siis on tõenäoliselt *keelatud* diagnostika laiendiga.

Kui kindla mõõdiku ei ole valimisse või soovite sagedusega üle, saate värskendada diagnostika konfigureerimine.

Kui mõlemal juhul ülaltoodud on true, siis lugege läbi lubamiseks mõõdiku [PowerShelli kasutamine lubamiseks klõpsake töötab Windows Azure'i diagnostika](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) PowerShelli konfigureerimine ja värskendage oma Azure'i VM diagnostika pikendamise kohta. See artikkel sisaldab ka näidisfaili diagnostika konfigureerimine.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Arvutage mõõdikute Windows VM v2 külalisena OS

Kui loote uue VM (v2) Azure, diagnostika on lubatud diagnostika laiendamise abil.

PowerShelli järgmise käsu abil saate luua loendi mõõdikud.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Saate luua järgmised mõõdikute teatise.

|Argumendil nimi|   Üksuse|
|---|---|
|\Processor(_Total)\% protsessori aeg    |%|
|\Processor(_Total)\% au aeg   |%|
|\Processor(_Total)\% kasutaja aeg |%|
|\Processor teave (_Total) \Processor sagedus |Count|
|\System\Processes| Count|
|\Process (_Total) \Thread arv| Count|
|\Process (_Total) \Handle arv  |Count|
|\Memory\% kinnitatud baiti kasutamine   |%|
|\Memory\Available baiti|   Baiti|
|\Memory\Committed baiti    |Baiti|
|\Memory\Commit limiit|  Baiti|
|\Memory\Pool leheküljed baiti|  Baiti|
|\Memory\Pool saalimata baiti|   Baiti|
|\PhysicalDisk(_Total)\% kettale aeg| %|
|\PhysicalDisk(_Total)\% kettalt lugemise ajal|    %|
|\PhysicalDisk(_Total)\% kettale salvestamise aeg|   %|
|\PhysicalDisk (_Total) \Disk edastamine sekundis   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk loeb sekundis   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk kirjutab sekundis  |CountPerSecond|
|\PhysicalDisk (_Total) \Disk baiti sekundis   |BytesPerSecond|
|\PhysicalDisk (_Total) \Disk lugeda baiti sekundis| BytesPerSecond|
|\PhysicalDisk (_Total) \Disk kirjutamine baiti sekundis |BytesPerSecond|
|\Avg \PhysicalDisk (_Total). Ketas järjekorra pikkus|  Count|
|\Avg \PhysicalDisk (_Total). Ketas lugemine järjekorra pikkus| Count|
|\Avg \PhysicalDisk (_Total). Ketta täitmine järjekorda pikkus |Count|
|\LogicalDisk(_Total)\% vaba ruumi| %|
|\LogicalDisk (_Total) \Free MB|   Count|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Arvutage mõõdikute Linux VM v2 külalisena OS

Kui loote uue VM (v2) Azure, diagnostika on vaikimisi diagnostika laiendamise abil.

PowerShelli järgmise käsu abil saate luua loendi mõõdikud.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Saate luua järgmised mõõdikute teatise.

|Argumendil nimi                            |Ühiku|
|---|---|
|\Memory\AvailableMemory                |Baiti|
|\Memory\PercentAvailableMemory         |%|
|\Memory\UsedMemory                     |Baiti|
|\Memory\PercentUsedMemory              |%|
|\Memory\PercentUsedByCache             |%|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Baiti|
|\Memory\PercentAvailableSwap           |%|
|\Memory\UsedSwap                       |Baiti|
|\Memory\PercentUsedSwap                |%|
|\Processor\PercentIdleTime             |%|
|\Processor\PercentUserTime             |%|
|\Processor\PercentNiceTime             |%|
|\Processor\PercentPrivilegedTime       |%|
|\Processor\PercentInterruptTime        |%|
|\Processor\PercentDPCTime              |%|
|\Processor\PercentProcessorTime        |%|
|\Processor\PercentIOWaitTime           |%|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Minutit|
|\PhysicalDisk\AverageWriteTime         |Sekundid|
|\PhysicalDisk\AverageTransferTime      |Sekundid|
|\PhysicalDisk\AverageDiskQueueLength   |Count|
|\NetworkInterface\BytesTransmitted     |Baiti|
|\NetworkInterface\BytesReceived        |Baiti|
|\NetworkInterface\PacketsTransmitted   |Count|
|\NetworkInterface\PacketsReceived      |Count|
|\NetworkInterface\BytesTotal           |Baiti|
|\NetworkInterface\TotalRxErrors        |Count|
|\NetworkInterface\TotalTxErrors        |Count|
|\NetworkInterface\TotalCollisions      |Count|




## <a name="commonly-used-web-server-farm-metrics"></a>Levinud Web (serveripargi) mõõdikud

Saate teha ka autoscale, nt Http järjekorda pikkus levinud web serveri mõõdikute põhjal. Argumendil nimi on **HttpQueueLength**.  Järgmises jaotises on loetletud mõõdikute saadaval serveri serveripargi (Web Apps).

### <a name="web-apps-metrics"></a>Web Appsi mõõdikud

PowerShelli järgmise käsu abil saate luua veebirakenduste mõõdikute loend.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Hoiata või mastaapimiseks saate nende mõõdikute järgi.

|Argumendil nimi        |Üksuse|
|---                |---|
|CpuPercentage      |%|
|MemoryPercentage   |%|
|DiskQueueLength    |Count|
|HttpQueueLength    |Count|
|BytesReceived      |Baiti|
|BytesSent          |Baiti|


## <a name="commonly-used-storage-metrics"></a>Sageli kasutatud salvestusruumi mõõdikud
Kui muudate, salvestusruumi järjekorda pikkus, mis on sõnumite salvestusruumi järjekorda. Salvestusruumi järjekorra pikkus on teisiti mõõdiku ja rakendatud piirmäär on sõnumite kohta eksemplari. See tähendab, kui kahel juhul kui 100 on määratud piirmäära, see mastaapimiseks juhul, kui järjekorda sõnumite arv 200. Näiteks 100 sõnumi eksemplari kohta.

Saate konfigureerida see on **sätete** tera Azure'i portaalis. VM skaala komplektide saate värskendada Autoscale säte ARM malli kasutada *metricName* *ApproximateMessageCount* ja edastama nimega *metricResourceUri*salvestusruumi kuhjuda ID-d.

Näiteks klassikaline salvestusruumi kontoga autoscale säte metricTrigger umbes järgmist:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Salvestusruumi (mitte klassikaline) konto on metricTrigger hõlmaks:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Tihti kasutatava teenuse siini mõõdikud

Kui muudate, teenuse siini järjekorda pikkus, mis on teenuse siini järjekorda teadete arv. Teenuse siini järjekorra pikkus on teisiti mõõdiku ja määratud piirmäära rakendatud saab sõnumite kohta eksemplari. See tähendab, kui kahel juhul kui 100 on määratud piirmäära, see mastaapimiseks juhul, kui järjekorda sõnumite arv 200. Näiteks 100 sõnumi eksemplari kohta.

VM skaala komplektide saate värskendada Autoscale säte ARM malli kasutada *metricName* *ApproximateMessageCount* ja edastama nimega *metricResourceUri*salvestusruumi kuhjuda ID-d.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Teenuse siini, ressursside rühma mõistet pole olemas, kuid Azure'i ressursihaldur luuakse vaikimisi ressursirühm müüginäitajaid piirkonna kohta. Ressursirühma on tavaliselt "Vaike - ServiceBus-[piirkond]"-vormingus. Näiteks "Vaike-ServiceBus-EastUS", "Vaike-ServiceBus-WestUS", "vaike-ServiceBus-AustraliaEast' jne.
