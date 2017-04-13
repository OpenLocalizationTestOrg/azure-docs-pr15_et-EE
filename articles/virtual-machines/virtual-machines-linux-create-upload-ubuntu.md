<properties
    pageTitle="Luua ja üles laadida ka Ubuntu Linux VHD Azure"
    description="Siit saate teada, luua ja üles laadida ka Azure virtuaalse kõvaketta (VHD) sisaldava Ubuntu Linux operatsioonisüsteemi."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure'i mõne Ubuntu virtuaalse masina ettevalmistamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Ametlik Ubuntu cloud pildid
Nüüd avaldab Ubuntu ametlik Azure'i VHDs jaoks [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/)alla laadida. Kui teil on vaja eriotstarbelisi Ubuntu pilt koostada Azure pigem kui käsitsi toiminguga allpool on soovitatav käivitamiseks nende teadaolevad VHDs tööd ja kohandada vastavalt vajadusele. Pildi uusimate alati leiate järgmisest kohast:

 - Ubuntu 12.04/täpne: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Eeltingimused

Selles artiklis eeldatakse, et on juba installitud Ubuntu Linux operatsioonisüsteemi virtuaalse kõvakettale. Mitme tööriistad olemas .vhd failidest, näiteks virtualization lahenduse nagu Hyper-V loomiseks. Lisateabe saamiseks vaadake [Hyper-V roll ja konfigureerimine virtuaalse masina installimine](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu installimärkmed**

- Lugege ka [Üldine Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) rohkem näpunäiteid Azure'i Linux ettevalmistamine.
- VHDX vorming ei toeta Azure, ainult **kindla VHD**.  VHD vorming Hyper-V halduri või Teisenda-vhd cmdlet-käsu abil saate teisendada ketas.
- Linuxi süsteemi installimisel on soovitatav kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. [LVM](virtual-machines-linux-configure-lvm.md) või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.
- Konfigureerige Vaheta sektsiooni OS kettal. Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks.  Lisateavet selle kohta leiate alltoodud juhiseid.
- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.


## <a name="manual-steps"></a>Juhised

> [AZURE.NOTE] Enne luua oma kohandatud Ubuntu pilt Azure, kaaluge selle asemel kasutada [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) kujutised.


1. Valige keskmisel paanil Hyper-V halduri virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina akna avamiseks.

3.  Asendage praegune hoidlate kasutada Ubuntu on Azure repos pilt. Juhised veidi erineda sõltuvalt Ubuntu versiooni.

    Enne redigeerimist /etc/apt/sources.list, on soovitatav teha varukoopia:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Ubuntu Azure'i pildid on nüüd järgmine tuum *riistvara võimaldamine* (HWE). Operatsioonisüsteemi värskendamine uusima tuum käivitades järgmised käsud:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Saate muuta Grub lisada täiendavad tuum parameetrite Azure tuum buutimine rida. Selleks avatud "/ grub/jne/vaikimisi" tekstiredaktoris, otsige nimega muutuja `GRUB_CMDLINE_LINUX_DEFAULT` (või lisage see vajaduse korral) ja redigeerige seda sisaldavad järgmisi:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Salvestage ja sulgege fail ja seejärel käivitage "`sudo update-grub`". See tagab kõigi konsooli sõnumid saadetakse esimese järjestikune port, abistavaid Azure tehnilise toe poole ja silumine probleemid.

6.  Veenduge, et SSH server on installinud ja konfigureerinud käivitamisel alustada.  See on tavaliselt vaikimisi.

7.  Installige Azure'i Linux Agent.

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Pange tähele, et installimise soovitud `walinuxagent` eemaldab paketi soovitud `NetworkManager` ja `NetworkManager-gnome` pakette, kui need on installitud.

8.  Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.


## <a name="next-steps"></a>Järgmised sammud
Nüüd olete valmis Ubuntu Linux virtuaalse kõvaketta abil saate luua uue virtuaalmasinates Azure. Kui see on esimene kord, et üleslaaditava faili .vhd Azure, lugege juhiseid 2 – 3 [loomine](virtual-machines-linux-classic-create-upload-vhd.md)ja üleslaadimise virtuaalse kõvaketta, mis sisaldab Linuxi operatsioonisüsteemi.

## <a name="references"></a>Viited ##

Ubuntu riistvara võimaldamine (HWE) tuum:

- [http://Blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.html](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://Blog.utlemming.org/2015/02/1204-Azure-Cloud-images-Now-using-hwe.html](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
