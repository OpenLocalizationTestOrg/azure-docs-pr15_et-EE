<properties
    pageTitle="Luua ja üles laadida pildi kohandatud Linux | Microsoft Azure'i"
    description="Luua ja üles laadida virtuaalse kõvaketta (VHD) Azure ressursihaldur juurutamise näidise kohandatud Linux pildiga."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Üles laadida ja luua kohandatud plaat Linux VM

Selles artiklis kirjeldatakse, kuidas üles laadida ressursihaldur juurutamise näidise Azure virtuaalse kõvaketta (VHD) ja luua kohandatud pilt Linux VMs. Selle funktsiooni abil saate installida ja konfigureerida Linuxi distributsiooni teie ja seejärel selle VHD abil kiiresti luua Azure'i virtuaalmasinates (VMs).

## <a name="quick-commands"></a>Kiirülevaate käsud
Kui teil on vaja kiiresti täita tööülesanne, jaotis järgmisega alus vajalikke käske üles laadida VM Azure. Üksikasjalikumat teavet ja iga toimingu kontekst leiate [siit alates](#requirements)dokumendi ülejäänud.

Veenduge, et olete sisse logitud ja ressursihaldur režiimi kasutamine [Azure CLI](../xplat-cli-install.md) .

```bash
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisalduv `myResourceGroup`, `mystorageaccount`, ja `myimages`.

Esmalt looge ressursirühma. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUs` asukoht:

```bash
azure group create myResourceGroup --location "WestUS"
```

Hoidke oma virtuaalse ketast salvestusruumi konto loomine. Järgmises näites luuakse salvestusruumi konto nimega `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Konto salvestusruumi kiirklahvide loendi. Kirjutage `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Looge ümbris kontol salvestusruumi hankisite salvestusruumi võtme abil. Järgmises näites luuakse ümbrist, mis on nimega `myimages` abil salvestusruumi võtmeväärtuse kaudu `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Lõpetuseks, laadige oma VHD s.o ümbris, mille olete loonud. Määrake jaotises teie VHD kohaliku tee `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Nüüd saate luua VM oma üleslaaditud virtuaalse ketta [ressursihaldur malli abil](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Võite kasutada ka CLI, määrates URI oma kettale (`--image-urn`). Järgmises näites luuakse nimega VM `myVM` abil virtuaalse ketta varem üles laadida:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Sihtkoha salvestusruumi konto peab olema sama, kus saate üles laadida virtuaalse kettale. Samuti peate määrama või vastuse küsib, kõik nõutud täiendavad parameetrid on `azure vm create` käsk, näiteks virtuaalse võrgu, avaliku IP-aadress, kasutajanimi ja SSH võtmed. Lugege lisateavet [Saadaolevate CLI ressursihaldur parameetrite](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines)kohta.

## <a name="requirements"></a>Nõuded
Täitke järgmised juhised, peate.

- **Linux operatsioonisüsteemi installitud .vhd faili** - installimine on [Azure kinnitatud Linux jaotuse](virtual-machines-linux-endorsed-distros.md) (või [- kinnitatud jaotuse teabe](virtual-machines-linux-create-upload-generic.md)kuvamiseks) virtuaalse ketta VHD vormingus. Mitme tööriistad olemas VM ja VHD loomiseks:
    - Installima ja konfigureerima [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) või [KVM](http://www.linux-kvm.org/page/RunningKVM), jälgides, et kasutada VHD oma pildi vorming. Vajadusel saate [muuta pildi](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) abil `qemu-img convert`.
    - Saate kasutada ka Hyper-V [opsüsteemis Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) või [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Azure'i ei toeta VHDX uude vormingusse. Kui loote VM, saate määrata VHD vorming. Vajadusel saate teisendada VHDX ketast VHD abil [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) või [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShelli cmdlet-käsk. Lisaks Azure ei toeta üleslaadimise dünaamiline VHDs, seega peate enne üleslaadimist staatilise VHDs sellise ketast teisendamine. Näiteks [Azure'i VHD Utiliidid jaoks, avage](https://github.com/Microsoft/azure-vhd-utils-for-go) abil saate teisendada dünaamiliseks ketast Azure'i üleslaadimine käigus.

- Loodud kohandatud pildile VMs peab asuma samas salvestusruumi konto kui pilt ise
    - Salvestusruumi konto ja container kohandatud pilt ja loodud VMs loomine
    - Kui olete loonud kõik teie VMs, saate kustutada turvaline pilt

Veenduge, et olete sisse logitud ja ressursihaldur režiimi kasutamine [Azure CLI](../xplat-cli-install.md) .

```bash
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisalduv `myResourceGroup`, `mystorageaccount`, ja `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Üles laadida pildi ettevalmistamine

Azure'i toetab erinevate Linuxi (vt [Jaotuse kinnitatud](virtual-machines-linux-endorsed-distros.md)). Järgmised artiklid juhatavad teid kuidas valmistada erinevate Linuxi, mis on toetatud Azure:

- **[CentOS vastavalt üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle'i Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Punane rolli Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES ja openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Muud --kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-generic.md)**

Vaata ka **[Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** Linux pildid ettevalmistamine Azure'i üldisema näpunäiteid.

> [AZURE.NOTE] [Azure'i platvormi SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) kehtib VMs Linux töötab ainult siis, kui üks kinnitatud jaotuse kasutatakse määratletud jaotises "Toetatud versioonid" [Linuxi Azure-Endorsed jaotuse](virtual-machines-linux-endorsed-distros.md)üksikasjadega konfigureerimine.


## <a name="create-a-resource-group"></a>Ressursi rühma loomine
Ressursi rühmade loogiliselt koondada kõik Azure ressursse, mis toetavad oma virtuaalmasinates, nt virtuaalse võrgunduse ja salvestusruumi. Lugege lisateavet [Azure ressursi rühmade siin](../azure-resource-manager/resource-group-overview.md). Enne oma kohandatud plaat üles laadida ja luua VMs, peate esmalt ressursi rühma loomine. 

Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUS` asukoht:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine
Lehe plekid salvestusruumi kontol salvestatakse VMs. Lugege lisateavet [Azure'i bloobimälu siin](../storage/storage-introduction.md#blob-storage). Saate luua kohandatud plaat ja VMs salvestusruumi konto. Mis tahes VMs, mida loote oma kohandatud plaat peavad olema sama salvestusruumi kontole selle pildina.

Järgmises näites luuakse salvestusruumi konto nimega `mystorageaccount` varem loodud ressursi jaotises:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Loendi salvestusruumi konto võtmed
Azure'i loob kaks 512-bitine kiirklahvide iga salvestusruumi konto. Nende kiirklahvide kasutatakse autentimine salvestusruumi konto, näiteks kirjutamine toiminguid. Lisateavet [mäluruumi siin juurdepääsu haldamise](../storage/storage-create-storage-account.md#manage-your-storage-account)kohta. Kiirklahvide abil saate kuvada soovitud `azure storage account keys list` käsk.

Kiirklahvide jaoks loodud salvestusruumi konto vaatamiseks tehke järgmist.

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Väljund on sarnane:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Kirjutage `key1` nagu saate suhelda salvestusruumi konto järgmiste juhiste juurde.

## <a name="create-a-storage-container"></a>Salvestusruumi container loomine
Teie loodud erinevate kataloogide loogiliselt korraldamiseks kohaliku failisüsteemi samal viisil loote ümbriste salvestusruumi kontol korraldamiseks oma virtuaalse ja pilte. Salvestusruumi konto võib olla mis tahes arv ümbriste. 

Järgmises näites luuakse ümbrist, mis on nimega `myimages`, täpsustades kiirklahv saadud eelmises etapis (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>VHD üleslaadimine
Nüüd saate tegelikult laadida oma kohandatud plaat. Nagu kõik virtuaalse ketast, mis kasutavad VMs, saate üles laadida ja hoida oma kohandatud plaat lehe bloobimälu.

Määrake oma kiirklahv ümbris loodud eelmises etapis ja seejärel kohandatud plaat tee kohalikus arvutis:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Luua kohandatud pildilt VM
Kui loote oma kohandatud plaat VMs, määrata URI on plaat. Veenduge, et teie kohandatud plaat talletuskoht sihtkoha salvestusruumi konto vasteid. Saate luua oma VM Azure CLI või ressursihaldur JSON malli abil.


### <a name="create-a-vm-using-the-azure-cli"></a>Luua VM Azure CLI abil
Teie määratud soovitud `--image-urn` parameetrite abil soovitud `azure vm create` käsku valige oma kohandatud ketta pilti. Tagada, et `--storage-account-name` vastab salvestusruumi konto, kuhu on talletatud teie kohandatud plaat. Teil on kasutada samal pakendi kohandatud plaat salvestada oma VMs. Veenduge, et luua mis tahes täiendavaid ümbriste enne oma kohandatud ketta Piltide üleslaadimise samamoodi nagu varem kirjeldatud juhiseid.

Järgmises näites luuakse nimega VM `myVM` kaudu oma kohandatud plaat:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Peate siiski määrama või vastuse küsib, kõik nõutud täiendavad parameetrid on `azure vm create` käsk, näiteks virtuaalse võrgu, avaliku IP-aadress, kasutajanimi ja SSH võtmed. Lugege lisateavet [Saadaolevate CLI ressursihaldur parameetrid](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Luua VM JSON malli abil
Azure'i ressursihaldur Mallid on JavaScript Object märke (JSON) faile, mis määratlevad soovite koostada keskkonna. Mallid on jaotatud eri ressursi pakkujad nagu Arvuta või võrku. Saate kasutada olemasolevaid malle või kirjutada oma. Lugege lisateavet [ressursihaldur ja mallide kasutamise](../azure-resource-manager/resource-group-overview.md)kohta.

Sees on `Microsoft.Compute/virtualMachines` pakkuja malli, kui teil on mõne `storageProfile` sõlm, mis sisaldab teie VM konfiguratsiooni üksikasjad. On kaks peamist parameetrid redigeerimiseks on `image` ja `vhd` käsk oma kohandatud plaat ja teie uus VM virtuaalse ketta URI-d. Järgmisel joonisel on kujutatud JSON kohandatud plaat kasutamise näide:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Saate [luua kohandatud pildilt VM seda olemasoleva malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) kasutada või [Azure ressursihaldur mallide loomise](../resource-group-authoring-templates.md)kohta vt. 

Kui olete konfigureerinud malli, saate luua oma VMs abil soovitud `azure group deployment create` käsk. URI JSON malli abil saate määrata selle `--template-uri` parameeter:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Kui teil on arvutisse salvestatud kohalikult JSON faili, saate selle `--template-file` parameetri hoopis:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Järgmised sammud
Kui olete valmis ja oma kohandatud virtuaalse ketta üles laaditud, saate lugeda Lisateavet [ressursihaldur ja mallide kasutamise](../azure-resource-manager/resource-group-overview.md)kohta. Samuti võite lisada [andmed kettale](virtual-machines-linux-add-disk.md) oma uue vms. Kui teil on oma VMs, mille juurde pääsemiseks vajate töötavad rakendused, kindlasti [avatud pordid ja lõpp-punktid](virtual-machines-linux-nsg-quickstart.md).