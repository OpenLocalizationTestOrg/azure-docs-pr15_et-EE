<properties
    pageTitle="Looge oma Azure'i Linux VM koopia | Microsoft Azure'i"
    description="Saate teada, kuidas luua koopia arvuti Azure'i Linux virtual ressursihaldur juurutamise mudeli"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Azure'i töötab Linux virtuaalse masina koopia loomine


Selles artiklis kirjeldatakse, kuidas Azure virtuaalne arvuti (VM) töötab Linux ressursihaldur juurutamise näidise koopia loomiseks. Esmalt kopeerige operatsioonisüsteem ja andmete ketast, et uus ümbris, siis ressursside võrgu häälestamine ja luua uue virtuaalse masina.

Samuti saate [üles laadida ja luua kohandatud plaat kaudu VM](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et enne alustamist juhiseid vastama järgmistele tingimustele:

- Teil on Azure CLI (… / xplat-cli-install.md) alla ja installitakse teie arvutisse. 

- Samuti vajate oma olemasoleva Azure Linux VM kohta leiate teavet:

| Andmeallika VM teave | Kust seda |
|------------|-----------------|
| VM nimi | `azure vm list` |
| Ressursirühm nimi | `azure vm list` |
| Asukoht | `azure vm list` |
| Salvestusruumikonto nimi | `azure storage account list -g <resourceGroup>` |
| Container nimi | `azure storage container list -a <sourcestorageaccountname>` |
| Andmeallika VM VHD faili nimi | `azure storage blob list --container <containerName>` |



- Peate teha mõned oma uue VM valikuid:   <br> -Container nimi   <br> -VM nimi   <br> -VM suurus   <br> -vNet nimi   <br> -Alamvõrgu nimi   <br> -IP nimi   <br> -NIC nimi
    

## <a name="login-and-set-your-subscription"></a>Logi sisse ja määrake oma tellimuse

1. Logi CLI.
        
        azure login

2. Veenduge, et olete ressursihaldur režiimis.
    
        azure config mode arm

3. Määrake õige tellimus. Saate kasutada "Azure'i konto loend" kõigi tellimuste kuvamiseks.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>VM peatamine 

Lõpetamine ja deallocate allika VM. Saate kasutada "Azure'i vm loend" saada kõigi oma tellimust ja nende ressursside VM rühma nime.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopeerige soovitud VHD


Saate kopeerida soovitud VHD allika salvestusruumi sihtkoha abil soovitud `azure storage blob copy start`. Selles näites me kopeerida soovitud VHD salvestusruumi sama konto, kuid erinevad ümbrises.

Tippige soovitud VHD kopeerimiseks teise container sama salvestusruumi kontole:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Teie uus VM virtuaalse võrgu häälestamine

Häälestada virtuaalse võrgu- ja NIC oma uue VM. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Looge uus VM 

Nüüd saate luua VM [Ressursi halduri malli abil](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) oma üleslaaditud virtuaalse ketta või CLI kaudu, määrates URI oma kopeeritud kettale, tippides:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas Azure'i CLI virtual seadme haldamiseks kasutada, leiate [Azure'i CLI käsud Azure'i ressursihaldur](azure-cli-arm-commands.md).
