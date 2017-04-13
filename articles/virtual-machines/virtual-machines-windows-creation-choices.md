<properties
    pageTitle="Võimalust luua Windows VM | Microsoft Azure'i"
    description="Loendite loomine Windowsi virtuaalse masina koos ressursihaldur võimalust."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Võimalust luua Windowsi virtuaalse masina ressursihaldur abil

Azure'i pakub võimalust luua virtuaalse masina, kuna virtuaalmasinates sobivad erinevatele kasutajatele ja eesmärgil. See tähendab, et peate tegema järgmisi võimalusi virtuaalse masina ja kuidas luua selle kohta. See artikkel annab ülevaate järgmisi valikuid ja lingid juhiseid.

## <a name="azure-portal"></a>Azure'i portaal

Azure'i portaalis on lihtne viis proovida virtuaalse masina, eriti siis, kui te olete just tööd alustamas Azure. 

[Töötab Windows portaali loomine](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Mall

Virtuaalmasinates nõuavad ressursse (nt mõne kättesaadavus komplekti ja salvestusruumi kontod). Selle asemel juurutamine ja hallata eraldi iga ressursi, saate luua Azure'i ressursihaldur malli, mis kasutab ja kõik ressursid ühe koordineeritud kasutusel sätted.

- [Windowsi virtuaalse masina ressursihaldur malli loomine](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure'i PowerShelli

Kui eelistate käsk shell töötab, saate kasutada Azure PowerShelli.

- [Luua VM Windows PowerShelli abil](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Visual Studio abil saate luua, hallata ja juurutamine Visual Studio ja Azure SDK VMs Azure'i tööriistu.

[Azure'i Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

