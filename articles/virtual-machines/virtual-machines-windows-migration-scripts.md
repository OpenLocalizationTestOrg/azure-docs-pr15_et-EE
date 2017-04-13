<properties
    pageTitle="Kogukonna tööriistad Azure'i teenuse haldamiseks Azure'i ressursihaldur migreerimise abil"
    description="Selles artiklis kataloogid tööriistu, mis antud ühenduse abistamiseks migreerimise IaaS ressursside kaudu Azure'i Teenusehaldus Azure'i ressursihaldur virnas."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Kogukonna tööriistad Azure'i teenuse haldamiseks Azure'i ressursihaldur migreerimise abil

Selles artiklis kataloogid tööriistu, mis antud ühenduse abistamiseks migreerimise IaaS ressursside kaudu Azure'i Teenusehaldus Azure'i ressursihaldur virnas.

>[AZURE.NOTE]Microsoft Support nende tööriistade ametliku ei toeta. Seetõttu need on avatud hangitud github ja me nõus PRs paranduste või täiendavad stsenaariumid. Probleemist, kasutage funktsiooni Github probleemid.
>
> Nende tööriistade migreerimine põhjustada tööseisakute klassikaline Virtual arvuti. Kui otsite toetatud platvormi migreerimise jaoks, külastage 
>
>- [Toetatud platvormi migreerimise IaaS ressurssi klassikaline: Azure'i ressursihaldur virnas](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Tehnika Deep Dive platvormil toetatud migreerimise klassikaline Azure'i ressursihaldur](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Migreerida IaaS ressursid klassikaline Azure'i ressursihaldur Azure PowerShelli abil](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

See on PowerShelli skripti mooduli meiliteabe oma **ühe** virtuaalse masina (VM) Azure'i Teenusehaldus virnas Azure'i ressursihaldur virnas. 

[Tööriista dokumentatsiooni link](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz on üks täiendav valik migreerida Azure'i teenuse haldus IaaS ressursid komplekt Azure'i ressursihaldur IaaS ressurssidele. Migreerimise võivad tekkida ühe tellimuse või muu tellimuste ja tellimusi vahel (ex: CSP tellimused).

[Tööriista dokumentatsiooni link](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)