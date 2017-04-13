<properties
    pageTitle="Liikumine Windowsiga VM | Microsoft Azure'i"
    description="Windows VM teisaldamine teise Azure tellimuse või ressursside rühma ressursihaldur juurutamise mudeli."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Windows VM teisaldamine teise Azure tellimuse või ressursside rühma 

Selles artiklis tutvustatakse teisaldamine Windows VM ressursi rühmade või tellimuste vahel. Tellimuste vahel olla päris asjalikud kui lõite VM algselt personal tellimuse ja nüüd soovite liikuda oma ettevõtte tellimust oma tööd jätkata.

> [AZURE.NOTE] Uue ressursi ID-d ei looda teisaldamist osana. Kui VM on teisaldatud, peate värskendama oma tööriistad ja skriptide kasutamine uue ressursi ID-d. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>PowerShelli kasutamine VM teisaldamine

Virtuaalse masina teisaldamiseks teise ressursirühm, peate veenduge, et teisaldate ka kõik sõltuvad ressursid. Teisalda-AzureRMResource cmdlet-käsu kasutamine peate ressursi nimi ja tüüp ressursi. Saate mõlema leidmine-AzureRMResource cmdlet-käsk.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Liikumiseks tuleb teisaldada mitu ressursid VM. Me lihtsalt eraldi muutujate iga ressursi loomine ja seejärel loendis. Selles näites sisaldab enamik lihtsa ressursse VM, kuid saate lisada rohkem vastavalt vajadusele.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Ressursside liikumiseks muud tellimust kaasata **- DestinationSubscriptionId** parameeter. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Teil palutakse kinnitada, et soovite teisaldada määratud ressursid. Tippige **Y** kinnitada, et soovite teisaldada ressursside.

  
## <a name="next-steps"></a>Järgmised sammud

Ressursi rühmad ja tellimuste vahel saate liikuda palju erinevaid ressursse. Lisateavet leiate teemast [teisaldamine ressursid uue ressursirühma või tellimuse](../resource-group-move-resources.md).    