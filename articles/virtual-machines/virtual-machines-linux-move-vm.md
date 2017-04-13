<properties
    pageTitle="Liikumine Linux VM | Microsoft Azure'i"
    description="Mõne muu Azure tellimuse või ressursside rühma ressursihaldur juurutamise mudeli Linux VM liikuda."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Linux VM teisaldamine teise tellimuse või ressursside rühma

Selles artiklis tutvustatakse teisaldamise Linux VM ressursi rühmade või tellimuste vahel. Liikumine tellimuste VM saab kasulik, kui olete loonud VM personal tellimuse ja nüüd soovite oma ettevõtte tellimust liikumiseks.

> [AZURE.NOTE] Teisaldamist osana on loodud uue ressursi ID-d. Kui VM on teisaldatud, peate värskendama oma tööriistad ja skriptide kasutamine uue ressursi ID-d. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Azure'i CLI VM liikumiseks kasutage 

VM edukalt liikumiseks peate teisaldamine VM ja kõik täiendavad ressursid. **Azure'i jaotises Kuva** käsu abil saate loendi kõigi ressursside kohta ressursirühma ja nende ID-d. See aitab see käsk väljundi toru faili nii, et saate kopeerida ja kleepida hiljem käskude ID-d.

    azure group show <resourceGroupName>

VM ja selle ressursside teisaldamiseks teise ressursirühm käsu **azure ressursi teisaldamine** CLI. Järgmises näites kujutatakse VM ja see nõuab kõige levinum ressursside teisaldamine. Kasutame **-i** parameetri ja edastama jaoks ressursside liikumiseks ID-d (ilma tühikuteta) komaga eraldatud loendi.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Kui soovite teisaldada mõnda muud tellimust VM ja oma ressursse, lisage soovitud **--sihtkoha-subscriptionId & #60; destinationSubscriptionID & #62;** parameetri määrama sihtkoha tellimus.

Kui te töötate Windowsi arvuti käsuviibal, peate lisama mõne **$** ette muutujate nimed, kui te neid deklareerida. See pole vajalik Linux.

Teil palutakse kinnitada, et soovite teisaldada määratud ressursi. Tippige **Y** kinnitada, et soovite teisaldada ressursside.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Järgmised sammud

Ressursi rühmad ja tellimuste vahel saate liikuda palju erinevaid ressursse. Lisateavet leiate teemast [teisaldamine ressursid uue ressursirühma või tellimuse](../resource-group-move-resources.md).    