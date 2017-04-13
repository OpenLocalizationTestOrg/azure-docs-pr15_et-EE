<properties
    pageTitle="Võimalust luua Linux VM | Microsoft Azure'i"
    description="Lisateavet erinevate viiside Azure, koos linkidega tööriistad ja õpetused iga meetodi Linux virtuaalse masina loomiseks."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Võimalust luua Linux virtuaalse masina Azure

Teil on paindlikkust Azure'i Linux virtuaalse masina (VM) tööriistade ja töövood on mugav loomiseks. Selles artiklis antakse ülevaade sellest, nende erinevusi ja näited loomise oma Linux VMs.


## <a name="azure-cli"></a>Azure'i CLI 

Azure'i CLI on saadaval eri platvormide kaudu on npm paketi, tingimusel distributsiooni pakettide või keskmise suurusega ümbrises. Lugege lisateavet selle kohta, [Kuidas installida ja konfigureerida Azure CLI](../xplat-cli-install.md). Järgmised õppetükid näiteid Azure'i CLI kasutamise kohta. Lugege lisateavet CLI kiire alustada käsud kuvatakse iga artiklis.

- [Luua Linux VM Azure CLI arendaja ja test](virtual-machines-linux-quick-create-cli.md)
    - Järgmises näites luuakse CoreOS VM abil avalik võti nimega `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Luua turvatud Linux VM on Azure malli abil](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Järgmises näites luuakse VM github salvestatud malli abil:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Täielik Linuxi keskkond abil Azure'i CLI loomine](virtual-machines-linux-create-cli-complete.md)
    - Sisaldab laadi koormusetasakaalustusteenuse ja mitme VMs on kättesaadavus komplekti loomine.

- [Linux VM kettal lisamine](virtual-machines-linux-add-disk.md)
    - Järgmises näites liidetakse 5Gb vaba mõne olemasoleva VM nimega `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure'i portaal

[Azure'i portaalis](https://portal.azure.com) võimaldab VM kiiresti luua, kuna pole teie arvutisse installida. Looge VM Azure portaali abil.

- [Linux VM Azure portaali loomine](virtual-machines-linux-quick-create-portal.md) 
- [Azure'i portaalis kettal manustamine](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Operatsioonisüsteemi ja pilt Valikud
Kui loote VM, valite pildi põhjal operatsioonisüsteem, mida soovite käivitada. Azure'i ja partneritega pakuvad palju pilte, mis sisaldavad rakenduste ja tööriistade eelinstallitud. Või üks oma (vt [järgmist jaotist](#use-your-own-image)) pildid üles laadida.

### <a name="azure-images"></a>Azure'i pildid
Kasutage funktsiooni `azure vm image` CLI käsud, mis on saadaval, publisher, distributsiooni väljaanne ja järgud.

Loendis Saadaolevad tootjad järgmiselt:

```bash
azure vm image list-publishers --location WestUS
```

Loendi saadaval tooted (pakkumised) antud Publisheri järgmiselt:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Loendis Saadaolevad SKU-de jaoks (distributsiooni plaati) antud pakkumise järgmiselt:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Loetle kõik saadaolevad piltide jaoks antud väljaanne järgib:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Rohkem näiteid sirvimine ja saadaval piltide kasutamise kohta leiate [navigeerimine ja valige Azure virtuaalse masina pilte Azure'i CLI](virtual-machines-linux-cli-ps-findimage.md).

Funktsiooni `azure vm quick-create` ja `azure vm create` käsud on aliases abil saate kiiresti juurde pääseda rohkem levinud distros ja nende uusimate. Kasutades pseudonüümid on tihti kiirem kui täpsustades Publisheri, pakkumine, SKU ja versioon iga kord, kui loote VM:

| Alias (pseudonüüm)     | Publisheri | Pakkumine        | SKU-GA         | Versioon |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | Viimane  |
| CoreOS    | CoreOS    | CoreOS       | Stabiilne      | Viimane  |
| Debian    | credativ  | Debian       | 8           | Viimane  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | Viimane  |
| RHEL      | RedHat    | RHEL         | 7.2         | Viimane  |
| SLES      | SLES      | SLES         | 12-SP1      | Viimane  |
| UbuntuLTS | Kanoonilise | UbuntuServer | 14.04.4-LTS | Viimane  |

### <a name="use-your-own-image"></a>Oma pildi kasutamine

Kui vajate eraldi kohandusi, saate pildi põhjal mõne olemasoleva Azure VM *hõivamine* , et VM. Samuti saate üles laadida pildi loodud kohapealse. Toetatud distros ja oma piltide kasutamise kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Azure'i kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-endorsed-distros.md)

- [Teave-kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-generic.md)

- [Kuidas jäädvustada Linux virtuaalse masina ressursihaldur mallina](virtual-machines-linux-capture-image.md).
    - Kiire algus jäädvustada mõne olemasoleva VM näide käsud:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Järgmised sammud

- Looge Linux VM [portaali](virtual-machines-linux-quick-create-portal.md) [CLI](virtual-machines-linux-quick-create-cli.md)või mõni [Azure ressursihaldur malli](virtual-machines-linux-cli-deploy-templates.md)abil.

- Kui olete loonud Linux VM, [andmed kettale lisada](virtual-machines-linux-add-disk.md).

- Kiirtoimingud [lähtestada parooli või SSH võtmed](virtual-machines-linux-using-vmaccess-extension.md) ja kasutajate haldamine
