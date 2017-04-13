<properties
    pageTitle="Pilveteenuse – käivitamise abil saate kohandada Linux VM loomise ajal | Microsoft Azure'i"
    description="Pilveteenuse – käivitamise abil saate kohandada Linux VM loomise ajal."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Pilveteenuse – käivitamise abil saate kohandada Linux VM loomise ajal

Selles artiklis kirjeldatakse kuidas teha pilveteenuses – käivitamise skripti määramiseks hostname, värskendus installitud paketid ja hallata kasutajakontosid.  Pilveteenuse – käivitamise skriptide nimetatakse: Azure'i CLI VM loomise ajal.  See artikkel nõuab tehke järgmist.

- Azure'i konto ([saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure'i CLI](../xplat-cli-install.md) sisse logitud `azure login`.

- Azure'i CLI _peab olema_ Azure'i ressursihaldur režiimi `azure config mode arm`.

## <a name="quick-commands"></a>Kiirülevaate käsud

Luua pilve-init.txt skripti, mis määrab hostname, värskendab kõik paketid ja lisab sudo kasutaja Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Looge ressursirühma käivitada VMs sisse.

```bash
azure group create myResourceGroup westus
```

Looge pilveteenuses – käivitamise abil saate konfigureerida selle käigus buutimine Linux VM.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse

### <a name="introduction"></a>Sissejuhatus

Uue Linux VM käivitamisel saite standard Linux VM midagi kohandatud või valmis teie vajadustele. [Pilveteenuse – käivitamise](https://cloudinit.readthedocs.org) on standardne viis sisestab selle Linux VM on skripti või konfiguratsiooni sätted, nagu see käivitamist sisse esimest korda.

Azure, on kolm võimalust peale Linux VM muudatusi teha, kui see on juurutatud või käivitatud.

- Annavad skriptide abil pilveteenuses – käivitamise.
- Annavad skriptide abil Azure [VMAccess laiendamine](virtual-machines-linux-using-vmaccess-extension.md).
- Azure'i Mall, kasutades pilveteenuses – käivitamise.
- Azure'i Mall, kasutades [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Sisestab skriptide igal ajal pärast buutimine:

- SSH käsud otse käivitamiseks
- Annavad skriptide abil Azure [VMAccess laiend](virtual-machines-linux-using-vmaccess-extension.md)kindlasti või mõni Azure Mall
- Konfiguratsiooni Haldusriistad nagu Ansible, sool, Chef ja nuku.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Pilveteenuse – käivitamise-saadavus Azure'i VM kiire loomine pilt pseudonüümid:

| Alias (pseudonüüm)     | Publisheri | Pakkumine        | SKU-GA         | Versioon | pilveteenuse – käivitamise |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | CentOS       | 7.2         | Viimane  | Ei         |
| CoreOS    | CoreOS    | CoreOS       | Stabiilne      | Viimane  | Jah        |
| Debian    | credativ  | Debian       | 8           | Viimane  | Ei         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | Viimane  | Ei         |
| RHEL      | RedHat    | RHEL         | 7.2         | Viimane  | Ei         |
| UbuntuLTS | Kanoonilise | UbuntuServer | 14.04.4-LTS | Viimane  | Jah        |

Microsoft teeb koostööd partneritega saada cloud käivitamise sisalduv ja pilte, et nad pakuvad Azure tööd.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Azure'i CLI VM loomine pilveteenuses – käivitamise skripti lisamine

Loob VM Azure pilveteenuse – käivitamise skripti käivitamiseks määrata pilveteenuses – käivitamise faili, kasutades Azure CLI `--custom-data` aktiveerimine.

Looge ressursirühma käivitada VMs sisse.

```bash
azure group create myResourceGroup westus
```

Looge pilveteenuses – käivitamise abil saate konfigureerida selle käigus buutimine Linux VM.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Pilveteenuse – käivitamise skripti seadmiseks Linux VM hostname loomine

Üks lihtsaim ja kõige olulisem sätete mis tahes Linux VM oleks hostname. Me saab määrata see pilveteenuses – käivitamise selle skripti abil.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Näide: pilveteenuste – käivitamise skripti nimega `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

VM algse käivitamisel see pilveteenuses – käivitamise skript määrab hostname `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Logi sisse ja veenduge, et hostname uue VM.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Cloud-käivitamise skripti värskendada Linux loomine

Turvalisus, soovite oma Ubuntu VM esimese buutimine värskendada.  Pilveteenuse – käivitamise saaksime teha, et jälgi skripti, olenevalt kasutate Linux jaotuse abil.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Näide pilveteenuses – käivitamise skripti `cloud_config_apt_upgrade.txt` Debian pere jaoks

```bash
#cloud-config
apt_upgrade: true
```

Pärast Linux on käivitatud, värskendatakse installitud paketid kaudu `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Logi sisse ja veenduge, et kõik pakette uuendatakse.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Pilveteenuses – käivitamise skript kasutaja lisamiseks Linux loomine

Mis tahes uue Linux VM esimese tööülesannete on enda jaoks kasutaja lisamiseks või Vältige `root`. SSH võtmed on parim turvalisus ja kasutatavuse ja need lisatakse selle `~/.ssh/authorized_keys` faili selle skripti pilveteenuses – käivitamise.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Näide pilveteenuses – käivitamise skripti `cloud_config_add_users.txt` Debian pere jaoks

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Pärast Linux on käivitatud, on loetletud kasutajate loodud ja sudo rühma lisada.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Logi sisse ja veenduge, et äsja loodud kasutaja.

```bash
ssh myVM
cat /etc/group
```

Väljund

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Järgmised sammud

Pilveteenuse – käivitamise muutub üks standardne viis, kuidas muuta oma Linux VM buutimine. Azure'i on ka VM laiendid, mis võimaldavad teil muuta oma LinuxVM buutimine või kui see töötab. Näiteks saate lähtestada SSH või kasutaja teabe VM töötamise Azure'i VMAccessExtension. Pilveteenuse käivitamise, siis oleks vaja uuesti parooli lähtestamiseks.

[Virtuaalse masina laiendid ja funktsioonide kohta](virtual-machines-linux-extensions-features.md)

[Kasutajate haldamine, SSH, ja märkige või parandada ketast Azure Linux VMs VMAccess laiend kasutamise kohta](virtual-machines-linux-using-vmaccess-extension.md)
