<properties
 pageTitle="Virtuaalse masina laiendid ja funktsioonide | Microsoft Azure'i"
 description="Siit saate teada, millised laiendid on saadaval Azure'i virtuaalmasinates, mida nad pakuvad või parandada Rühmitusalus."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Virtuaalse masina laiendid ja funktsioonide kohta

## <a name="azure-vm-extensions"></a>Azure'i VM laiendid

Azure virtuaalse masina laiendid on väikesed rakendused, mis pakuvad postitus juurutamise konfiguratsiooni ja automaatika tööülesande Azure'i Virtuaalmasinates. Näiteks kui virtuaalse masina nõuab tarkvara on installitud, Anti-Virus kaitse või keskmise suurusega konfiguratsioon, VM laiend saab kasutada järgmiste toimingute tegemiseks. Azure'i VM laiendid saab käivitada kasutades Azure CLI, PowerShelli, ressursside haldamine Mallid ja Azure portaali. Laiendid saate koos uue virtuaalse masina juurutamise või mis tahes olemasolevat süsteemi vastu.

Selle dokumendi annab saadaval VM laiendid tuvastamiseks eeltingimused Azure virtuaalse masina laiendi ja juhiseid. 

## <a name="azure-vm-agent"></a>Azure'i VM Agent

Azure'i VM Agent haldab on Azure virtuaalse masina ja Azure struktuuri kontrolleril vahelist suhtlust. VM agent on palju otstarbekas aspekte juurutamine ja haldamise Azure'i Virtuaalmasinates, sh töötab VM laiendid. Azure'i VM Agent Azure Galerii piltide eelinstallitud ja saab installida toetatud operatsioonisüsteemid. 

Toetatud opsüsteemid ja installimisjuhised kohta leiate teavet teemast [Azure virtuaalse masina Agent](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Tutvumine VM laiendid

Paljude erinevate VM laiendid on Azure'i Virtuaalmasinates kasutamiseks saadaval. Täieliku loendi kuvamiseks käivitage järgmine käsk Azure'i CLI, asendades asukohta valitud asukohta.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Levinud VM laiendid

|Laiend nimi   |Kirjeldus   |Lisateave   |
|---|---|---|
|Kohandatud skript laiend for Windows  | Skriptide käitamiseks vastu ka Azure virtuaalse masina  |[Kohandatud skript laiend for Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC laiend for Windows | PowerShelli DSC (soovitud maakond konfiguratsiooni) laiendamine.  | [Keskmise suurusega VM laiend](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Azure'i diagnostika laiend | Azure'i diagnostika haldamine |[Azure'i diagnostika laiend](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
