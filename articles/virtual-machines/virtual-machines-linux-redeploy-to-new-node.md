<properties 
    pageTitle="Juurutage uuesti Linuxi | Microsoft Azure'i" 
    description="Selles artiklis kirjeldatakse ümberkorraldamine Linux virtuaalmasinates SSH ühenduvusega probleeme leevendada." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Uue Azure sõlme virtuaalse masina ümberkorraldamine

Kui teil on raskusi tõrkeotsingu SSH või rakenduse juurdepääsu ümbersuunamine VM Azure virtuaalse masina (VM), võib aidata. Kui te ümberkorraldamine VM, viib VM Azure infrastruktuuri uus sõlm ja seejärel uuesti sisse, säilitades konfiguratsiooni suvandid ja seotud ressursid volitused. Selles artiklis kirjeldatakse, kuidas ümberkorraldamine VM Azure CLI või Azure portaali kaudu.

> [AZURE.NOTE] Pärast seda, kui te ümberkorraldamine VM, ajutised ketas läheb kaotsi ja dünaamiline virtuaalse võrgu liidese seotud IP-aadressid on värskendatud. 


## <a name="using-azure-cli"></a>Azure'i CLI abil

Veenduge, et teil on teie arvutisse [Installitud uusim Azure CLI](../xplat-cli-install.md) ja olete ressursihaldur režiimis (`azure config mode arm`).

Azure'i CLI järgmine käsk abil ümberkorraldamine virtual arvuti:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Kui see läheb läbi ümberkorraldamine protsessi saate vaadata oleku muutmine VM. Funktsiooni `PowerState` VM läheb 'käivitumist abiteemast, siis 'alates' ja lõpuks 'töötab"kui see läheb läbi protsessi uue Host ümbersuunamine. Ressursirühm, mille jooksul VMs oleku kontrollimine

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Järgmised sammud
Kui teil on probleeme oma VM ühenduse, leiate üksikasjalikku abi [tõrkeotsingu SSH ühendused](virtual-machines-linux-troubleshoot-ssh-connection.md) või [üksikasjalik SSH tõrkeotsingujuhiseid](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Kui te ei pääse rakendus töötab teie VM, võite lugeda ka [rakenduse tõrkeotsingu probleeme](virtual-machines-linux-troubleshoot-app-connection.md).