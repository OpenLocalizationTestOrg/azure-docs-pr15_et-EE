<properties
    pageTitle="Luua ja üles laadida Linux VHD | Microsoft Azure'i"
    description="Luua ja üles laadida ka Azure virtuaalse kõvaketta (VHD) klassikaline juurutamise mudeliga Linux operatsioonisüsteemi."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Luua ja üles laadida virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteem

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Samuti saate [üles laadida kohandatud plaat Azure'i ressursihaldur abil](virtual-machines-linux-upload-vhd.md).

Selles artiklis kirjeldatakse, kuidas luua ja üles laadida virtuaalse kõvaketta (VHD) abil saate selle pildina oma Azure'i virtuaalmasinates loomiseks. Saate teada, kuidas valmistada operatsioonisüsteemi abil saate luua mitme virtuaalmasinates selle pildi põhjal. 

>  [AZURE.NOTE] Kui teil on mõne hetke aega, aidake meil täiustada Azure Linux VM dokumentatsiooni võttes selle [kiire küsitluse](https://aka.ms/linuxdocsurvey) oma kogemusi. Iga vastus aitab aitavad teil oma töö valmis.

## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on järgmised üksused:

- **Installitud .vhd faili Linux operatsioonisüsteemi** - teie installitud mõni [Azure kinnitatud Linux jaotuse](virtual-machines-linux-endorsed-distros.md) (või [- kinnitatud jaotuse teabe](virtual-machines-linux-create-upload-generic.md)kuvamiseks) virtuaalse ketta VHD vormingus. Mitme tööriistad olemas VM ja VHD loomiseks:
    - Installima ja konfigureerima [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) või [KVM](http://www.linux-kvm.org/page/RunningKVM), jälgides, et kasutada VHD oma pildi vorming. Vajadusel saate [muuta pildi](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) abil `qemu-img convert`.
    - Saate kasutada ka Hyper-V [opsüsteemis Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) või [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Azure'i ei toeta VHDX uude vormingusse. Kui loote VM, saate määrata VHD vorming. Vajadusel saate teisendada VHDX ketast VHD abil [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) või [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShelli cmdlet-käsk. Lisaks Azure ei toeta üleslaadimise dünaamiline VHDs, seega peate enne üleslaadimist staatilise VHDs sellise ketast teisendamine. Näiteks [Azure'i VHD Utiliidid jaoks, avage](https://github.com/Microsoft/azure-vhd-utils-for-go) abil saate teisendada dünaamiliseks ketast Azure'i üleslaadimine käigus.

- **Azure'i käsurea liides** - installi uusima [Azure'i käsurea liides](../virtual-machines-command-line-tools.md) üleslaadimine on VHD.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Samm 1: Valmistada pilt üles laadida

Azure'i toetab erinevate Linuxi (vt [Jaotuse kinnitatud](virtual-machines-linux-endorsed-distros.md)). Järgmised artiklid juhendab kuidas valmistada erinevate Linuxi, mis on toetatud Azure. Pärast järgmised juhendid juhiste tule siia tagasi, kui olete VHD faili, mis on valmis Azure'i üles laadida:

- **[CentOS vastavalt üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle'i Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Punane rolli Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES ja openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Muud --kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Azure'i platvormi SLA kehtib virtuaalmasinates Linuxi opsüsteemi ainult siis, kui üks kinnitatud jaotuse kasutatakse määratletud jaotises "Toetatud versioonid" [Linuxi Azure-Endorsed jaotuse](virtual-machines-linux-endorsed-distros.md)üksikasjadega konfigureerimine. Kõik Linuxi Azure pilt galeriis on kinnitatud jaotuse nõutav konfiguratsioon.

Vaata ka **[Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** Linux pildid ettevalmistamine Azure'i üldisema näpunäiteid.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Samm 2: Ettevalmistamine Azure'i ühendus

Veenduge, et kasutate Azure CLI klassikaline juurutamise mudeli (`azure config mode asm`), siis logige sisse oma konto:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Samm 3: Azure'i pildi üleslaadimine

Teil on vaja salvestusruumi konto VHD faili üleslaadimiseks. Kas saate valida olemasoleva salvestusruumi konto või [looge uus](../storage/storage-create-storage-account.md).

Azure'i CLI abil üles laadida pildi abil järgmine käsk:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Eelmises näites:

- **BlobStorageURL** on salvestusruumi konto, mida plaanite kasutada URL
- **YourImagesFolder** on pakendi sees bloobimälu, kuhu soovite salvestada oma pilte
- **VHDName** on silt, mis kuvatakse portaalis tuvastamiseks kettaruumi.
- **PathToVHDFile** on täielik tee ja nimi .vhd faili oma arvutisse.

Järgmisel joonisel on kujutatud täielik näide:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Samm 4: Luua VM pilt
Saate luua VM kohta, kasutades `azure vm create` samamoodi nagu tavalise VM. Määrake andsite eelmises etapis oma pildi nimi. Järgmises näites kasutame eelmises etapis antud **UbuntuLTS** pildi nimi:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Luua oma VMs, sisestage oma kasutajanimi + parool, asukoht, DNS-i nimi ja pildi nimi.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Azure CLI juhend Azure klassikaline juurutamise mudeli](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
