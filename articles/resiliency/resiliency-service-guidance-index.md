<properties
   pageTitle="Teenuse paindlikkust juhised | Microsoft Azure'i"
   description="Lingid katastroofiabi ja Microsoft Azure'i teenuste aktiivne paindlikkust ja -saadavus juhised."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Microsoft Azure'i teenuse paindlikkust juhised
Microsoft Azure'i on mõeldud teile ressursside vajate, kui neid vajate. Osana hea kujundus ja funktsionaalseid tavade, peaksite teadma, kuidas arhitekt saavutamiseks kõrge-saadavus Azure'i teenuste kasutamisega nii kui ka mida teha, kui teie taotlus mõjutab teenuse häirete. Abi teile selle protsessi, see dokument sisaldab linke katastroofi taastamise juhised kui ka mitmesuguste Azure kujundus juhised.

##<a name="disaster-recovery-guidance"></a>Juhised katastroofi taastamine
Linkidest on katastroofi taastamise juhised annavad teile andmed, mida vajate, mis aitavad teil saada rakenduse taastamisel kiiresti, kui teil on Azure teenuse häirete mõjutab. Neid linke, mis on loodud aitavad vastata, "Ma olen on mõjutab ka Azure teenuse häirete, mida teha?"

##<a name="design-guidance"></a>Kujundus juhised
Linkidest kujundus juhised on kujundus ja arhitektuuri juhiseid, mis on loodud selleks, et aidata teil mõista, kuidas kõige paremini iga Azure'i teenust kasutada nii, et maksimeerib rakenduse sees. Neid linke, mis on loodud aitavad vastata küsimusele "Kuidas kontrollida, kas viga, riistvara tõrke, teenuse häirete või muude tõrge ei mõju minu rakenduste üldine kättesaadavus?" Kui pole praegu otsite teenuse juhiseid, [kõrge kättesaadavus Microsoft Azure loodud](./resiliency-high-availability-azure-applications.md) artiklis võib olla täiendavat teavet, mis aitavad teil oma kujundus. 

##<a name="service-guidance"></a>Teenuse juhised
| Teenus  | Juhised katastroofi taastamine | Kujundus juhised |
|:---------|:--------------------------:|:------------------:|
| [Pilveteenused] (https://azure.microsoft.com/services/cloud-services/ "Azure'i pilveteenused")       | [link] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure'i pilveteenustega katastroofi taastamise juhised")   | Pole saadaval |
| [Võtme Vault] (https://azure.microsoft.com/services/key-vault/ "Azure'i võtme Vault")                      | [link] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure'i klahvi Vault katastroofi taastamise juhised")        | Pole saadaval |
| [Salvestusruumi] (https://azure.microsoft.com/services/storage/ "Azure'i salvestusruum")                            | [link] (../storage/storage-disaster-recovery-guidance.md "Azure Storage katastroofi taastamise juhised")          | Pole saadaval |
| [SQL-i andmebaasid] (https://azure.microsoft.com/services/sql-database/ "SQL Azure'i andmebaasid")           | [link] (../sql-database/sql-database-disaster-recovery.md  "Azure'i SQL-andmebaasi katastroofi taastamise juhised")    | [link] (../sql-database/sql-database-business-continuity.md "Ettevõtte järjepidevus Azure'i SQL-andmebaasi ülevaade") |
| [Virtuaalmasinates] (https://azure.microsoft.com/services/virtual-machines/ "Azure'i Virtuaalmasinates") | [link] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure'i Virtuaalmasinates katastroofi taastamise juhised") | Pole saadaval |
| [Virtuaalse võrgu] (https://azure.microsoft.com/services/virtual-network/ "Azure virtuaalse võrgu")    | [link] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure'i virtuaalse võrgu katastroofi taastamise juhised")  | Pole saadaval |

##<a name="next-steps"></a>Järgmised sammud
Kui otsite juhiseid, mis keskendub laiemalt süsteemid ja lahendusi, lugege [ja Microsoft Azure loodud kõrge kättesaadavus](https://aka.ms/drtechguide).
