<properties
 pageTitle="Virtuaalse masina laiendid ja funktsioonide | Microsoft Azure'i"
 description="Siit saate teada, millised laiendid on saadaval Azure'i virtuaalmasinates, mida nad pakuvad või parandada Rühmitusalus."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Virtuaalse masina laiendid ja funktsioonide kohta

## <a name="azure-vm-extensions"></a>Azure'i VM laiendid

Azure virtuaalse masina laiendid on väikesed rakendused, mis pakuvad postituse juurutamise konfiguratsiooni ja automaatika tööülesande Azure'i Virtuaalmasinates. Näiteks kui virtuaalse masina nõuab tarkvara on installitud, viirusetõrje kaitse või keskmise suurusega konfiguratsioon, VM laiend saab kasutada järgmiste toimingute tegemiseks. Azure'i VM laiendid saab käivitada kasutades Azure CLI, PowerShelli, ressursside haldamine Mallid ja Azure portaali. Laiendid saate koos uue virtuaalse masina juurutamise või mis tahes olemasolevat süsteemi vastu.

Selles dokumendis on toodud eeltingimuste Azure virtuaalse masina laiendi ja juhised selle kohta, kuidas tuvastada saadaval VM laiendid. 

## <a name="azure-vm-agent"></a>Azure'i VM Agent

Azure'i VM Agent haldab on Azure virtuaalse masina ja Azure struktuuri kontrolleril vahelist suhtlust. VM agent on palju funktsionaalne aspekte juurutamine ja haldamise Azure'i Virtuaalmasinates, sh töötab VM laiendid. Azure'i VM Agent Azure Galerii piltide eelinstallitud ja saab installida toetatud operatsioonisüsteemid. 

Toetatud opsüsteemid ja installimisjuhised kohta lisateabe saamiseks leiate [Azure'i Linux Agent](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Tutvumine VM laiendid

Paljude erinevate VM laiendid on Azure'i Virtuaalmasinates kasutamiseks saadaval. Täieliku loendi kuvamiseks käivitage järgmine käsk Azure'i CLI, asendades asukohta valitud asukohta.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Levinud VM laiendid

|Laiend nimi   |Kirjeldus   |Lisateave   |
|---|---|---|
|Kohandatud skript laiend Linuxi jaoks  | Skriptide käitamiseks vastu ka Azure virtuaalse masina  |[Kohandatud skript laiend Linuxi jaoks](./virtual-machines-linux-extensions-customscript.md)   |
|Keskmise suurusega laiend |Installib keskmise suurusega daemon toetamiseks serveri keskmise suurusega käske.  | [Keskmise suurusega VM laiend](./virtual-machines-linux-dockerextension.md)  |
|VM Accessi laiend | Azure virtuaalse masina juurdepääsu taastamiseks  |[VM Accessi laiend](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Azure'i diagnostika laiend |Azure'i diagnostika haldamine |[Azure'i diagnostika laiend](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

