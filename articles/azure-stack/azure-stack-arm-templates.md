<properties
    pageTitle="Azure'i ressursihaldur mallide kasutamine Azure virnas (rentniku arendajad) | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure ressursihaldur malle Azure'i virnas juurutada ja kõik ressursse rakenduse ühe koordineeritud kasutusel ettevalmistamine."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure'i virnas Azure'i ressursihaldur mallide kasutamine

Azure'i ressursihaldur Mallid juurutada ja ettevalmistamise kõik ressursse rakenduse ühe ja koordineeritud toiming. Saate määratleda ressursid ja kuidas seda kasutada.  Te saate ka ümberkorraldamine Mallid ressursside kohta ressursirühma muudatuste tegemiseks.

Microsoft Azure'i virnas portaali, PowerShelli, käsurea ja Visual Studio saab kasutada neid malle.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Järgmised Mallid on saadaval [github](http://aka.ms/azurestackgithub).

## <a name="deploy-sharepoint-non-high-availability"></a>Juurutamine SharePointi (-kõrge-saadavus)

PowerShelli DSC laiend abil saate luua SharePoint 2013 serveripargis, mis sisaldab järgmist:

-   Virtuaalne võrgus

-   Kolm salvestusruumi kontod

-   Kahe välise koormus soolise

-   Ühe VM konfigureeritud domeenikontrolleri uus mets ühe domeeni jaoks

-   Ühe VM konfigureeritud SQL Server 2014 eraldiseisev server

-   Ühe arvuti SharePoint 2013 serveripargis konfigureeritud ühele VM

## <a name="deploy-ad-non-high-availability"></a>AD (-kõrge-saadavus) juurutamine

PowerShelli DSC laiend abil saate luua AD domeeni domeenikontrolleri server, mis sisaldab järgmist:

-   Virtuaalne võrgus

-   Ühe salvestusruumi konto

-   Ühe välise koormuse koormusetasakaalustusteenuse

-   Ühe VM konfigureeritud domeenikontrolleri uus mets ühe domeeni jaoks

## <a name="deploy-adsql-non-high-availability"></a>AD/SQL-i (-kõrge-saadavus) juurutamine

PowerShelli DSC laiend abil saate luua SQL Server 2014 eraldiseisev server, mis sisaldab järgmist:

-   Virtuaalne võrgus

-   Kahe salvestusruumi kontod

-   Ühe välise koormuse koormusetasakaalustusteenuse

-   Ühe VM konfigureeritud domeenikontrolleri uus mets ühe domeeni jaoks

-   Ühe VM konfigureeritud SQL Server 2014 eraldiseisev server

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

PowerShelli DSC laiend abil saate konfigureerida olemasoleva virtuaalse masina kohaliku Configuration Manager (LCM) ja registreerima Azure automatiseerimine konto DSC tõmmata serveriga.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Luua virtuaalse masina kasutaja pildilt

Luua kohandatud kasutaja pildilt virtuaalse masina. Selle malli kasutab ka (DNS) virtuaalse võrgu, avaliku IP-aadress ja võrgu liidese.

## <a name="simple-vm"></a>Lihtne VM

Lihtne Windows VM, mis sisaldab (DNS) virtuaalse võrgu, avaliku IP-aadress ja võrgu liidese juurutamine.

## <a name="cancel-a-running-template-deployment"></a>Töötava vormimalli juurutamine tühistamine

Töötava vormimalli juurutamine tühistamiseks kasutage funktsiooni `Stop-AzureRmResourceGroupDeployment` PowerShelli cmdlet-käsk.


## <a name="next-steps"></a>Järgmised sammud

[Mallide portaalis juurutamine](azure-stack-deploy-template-portal.md)

[Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)

