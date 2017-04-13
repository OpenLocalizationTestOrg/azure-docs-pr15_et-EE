<properties 
    pageTitle="Ümberkorraldamine Windowsi virtuaalmasinates | Microsoft Azure'i" 
    description="Selles artiklis kirjeldatakse ümberkorraldamine Windowsi virtuaalmasinates RDP ühenduvusega probleeme leevendada." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Uue Azure sõlme virtuaalse masina ümberkorraldamine

Kui teil on ees probleeme tõrkeotsing kaugtöölaua (RDP) ühendus või rakenduste juurdepääsu Windowsi-põhise Azure arvutiga (VM) ümbersuunamine VM võib aidata. Kui te ümberkorraldamine VM, viib VM Azure infrastruktuuri uus sõlm ja seejärel uuesti sisse, säilitades konfiguratsiooni suvandid ja seotud ressursid volitused. Selles artiklis kirjeldatakse, kuidas ümberkorraldamine Azure PowerShelli või Azure portaali VM.

> [AZURE.NOTE] Pärast seda, kui te ümberkorraldamine VM, ajutised ketas läheb kaotsi ja dünaamiline virtuaalse võrgu liidese seotud IP-aadressid on värskendatud. 

## <a name="using-azure-powershell"></a>Azure'i PowerShelli abil

Veenduge, et teil on uusim Azure PowerShelli 1.x teie arvutisse installitud. Lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

Selle Azure PowerShelli käsu abil saate oma virtuaalse masina ümberkorraldamine:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Järgmised sammud
Kui teil on probleeme oma VM ühenduse, leiate üksikasjalikku abi [tõrkeotsingu RDP ühendused](virtual-machines-windows-troubleshoot-rdp-connection.md) või [üksikasjalik RDP tõrkeotsingujuhiseid](virtual-machines-windows-detailed-troubleshoot-rdp.md). Kui te ei pääse rakendus töötab teie VM, võite lugeda ka [rakenduse tõrkeotsingu probleeme](virtual-machines-windows-troubleshoot-app-connection.md).