<properties
   pageTitle="Luua Linux VM Azure CLI abil | Microsoft Azure'i"
   description="Luua Linux VM Azure CLI abil."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Luua Linux VM Azure CLI abil

Selles artiklis kirjeldatakse kiiresti juurutamise Linux virtuaalse masina (VM) Azure, kasutades funktsiooni `azure vm quick-create` käsk Azure käsurea liides (CLI). Funktsiooni `quick-create` käsk kasutab VM lihtne, turvaline taristu prototüübile kasutada või testida mõiste kiiresti sees. See artikkel nõuab tehke järgmist.

- Azure'i konto ([saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure'i CLI](../xplat-cli-install.md) sisse logitud `azure login`.

- Azure'i CLI _peab olema_ Azure'i ressursihaldur režiimi `azure config mode arm`.

[Azure'i portaali](virtual-machines-linux-quick-create-portal.md)abil saate juurutada ka kiiresti Linux VM.

## <a name="quick-commands"></a>Kiirülevaate käsud

Järgmises näites kujutatakse CoreOS VM juurutada ja kinnitage oma Secure Shell (SSH) key (teie argumendid võivad olla erinevad):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse

Järgmine juhend on UbuntuLTS VM, mis on juurutatud, samm-sammult koos selgitustega iga toimingut teha.

## <a name="vm-quick-create-aliases"></a>VM kiire loomine pseudonüümid

Kiiresti valida iseloomustab jaotuse on kasutada Azure CLI pseudonüümid, vastendatud kõige levinum OS jaotuse. Järgmises tabelis on loetletud aliases (alates Azure'i CLI 0,10 versioon). Kõik juurutuste, mis kasutavad `quick-create` vaikimisi vms, mis on tagatud välkdraiv (SSD) salvestusruumi, mis pakub kiiremini ettevalmistamise ja suure jõudlusega juurdepääsu kettale. (Need pseudonüümid moodustavad saadaval jaotuse Azure väike osa. Otsige veel pilte Azure'i turuplatsi [otsimine PowerShellis pilt](virtual-machines-linux-cli-ps-findimage.md), [veebirakenduse](https://azure.microsoft.com/marketplace/virtual-machines/)või [kohandatud pildi üleslaadimine](virtual-machines-linux-create-upload-generic.md).)

| Alias (pseudonüüm)     | Publisheri | Pakkumine        | SKU-GA         | Versioon |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | Viimane  |
| CoreOS    | CoreOS    | CoreOS       | Stabiilne      | Viimane  |
| Debian    | credativ  | Debian       | 8           | Viimane  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | uusima  |
| RHEL      | Punane rolli    | RHEL         | 7.2         | Viimane  |
| UbuntuLTS | Kanoonilise | Ubuntu Server | 14.04.4-LTS | Viimane  |

Järgmine sections kasutamine on `UbuntuLTS` **ImageURN** suvandi pseudonüümi (`-Q`) serveriks Ubuntu 14.04.4 LTS juurutamiseks.

Eelmise `quick-create` näide ainult nupuga soovitud `-M` lipu tuvastamiseks SSH avalik võti üles laadida ajal keelamine SSH paroole, et küsitakse järgmisi argumente:

- ressursirühma nimi (mis tahes string on tavaliselt hea esimese Azure ressursirühma)
- VM nimi
- asukoht (`westus` või `westeurope` on hea vaikesätted)
- Linux (Azure'i teada, millised OS, mida soovite lasta)
- kasutajanimi

Järgmises näites saate määrata kõik väärtused, et veelgi küsimata on nõutav. Kui teil on mõni `~/.ssh/id_rsa.pub` ssh-rsa avalik võti vormingus, see toimib, nagu on:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Väljund peaks välja nägema järgmine väljund plokk.

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Uue VM sisse logida

Logige sisse oma VM, kasutades avaliku IP-aadress, mis on loetletud väljund. Samuti saate täielik domeeninimi (FQDN), mis on loetletud:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Sisselogimise protsess peaks välja nägema umbes järgmine väljund blokeerimine:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Järgmised sammud

Funktsiooni `azure vm quick-create` käsk on võimalus kiiresti juurutada VM nii, et saate bash shell sisse logida ja saada töötamine. Siiski abil `vm quick-create` teile olulisel kontrolli samuti ei luba teil luua keerukamaid keskkond.  Linux VM, mis on kohandatud teie infrastruktuuri juurutada, tehke ühte järgmistest artiklitest:

- [Azure'i ressursihaldur malli kasutamiseks teatud juurutamise loomiseks](virtual-machines-linux-cli-deploy-templates.md)
- [Luua kohandatud keskkonna Linux VM otse Azure'i CLI käskude kasutamine](virtual-machines-linux-create-cli-complete.md)
- [Luua mõne SSH turvatud Linux VM Azure mallide kasutamine](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Võite ka [kasutada funktsiooni `docker-machine` Azure juht erinevaid käske kiiresti luua Linux VM keskmise suurusega hostiks](virtual-machines-linux-docker-machine.md).
