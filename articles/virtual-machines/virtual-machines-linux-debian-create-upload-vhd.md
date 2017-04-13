<properties
    pageTitle="Ettevalmistused Debian Linux VHD | Microsoft Azure'i"
    description="Saate teada, kuidas luua Debian 7 ja 8 VHD failide juurutamiseks Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Azure'i Debian VHD ettevalmistamine

## <a name="prerequisites"></a>Eeltingimused
Selles jaotises eeldab, et olete juba installinud Debian Linux operatsioonisüsteem on virtuaalse kõvaketta [Debian veebisaidi](https://www.debian.org/distrib/) alla laadida faili topeltklõpsamisel. Mitme tööriistad olemas luua .vhd faile; Hyper-V on ainult üks näide. Juhised Hyper-V kasutamise kohta leiate artiklist [Hyper-V roll ja konfigureerimine virtuaalse masina installimine](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Installimärkmed

- Lugege ka [Üldine Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) rohkem näpunäiteid Azure'i Linux ettevalmistamine.
- Azure'i ei toeta VHDX uude vormingusse. VHD vorming Hyper-V halduri või **Teisenda-vhd** cmdlet-käsu abil saate teisendada ketas.
- Linuxi süsteemi installimisel on soovitatav kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. [LVM](virtual-machines-linux-configure-lvm.md) või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.
- Konfigureerige Vaheta sektsiooni OS kettal. Azure'i Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks. Lisateavet selle kohta leiate alltoodud juhiseid.
- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Debian VHDs loomiseks kasutada Azure haldamine

Saadaval on ka tööriistu loomiseks Debian VHDs Azure, nagu on [azure-haldamine](https://github.com/credativ/azure-manage) skriptide [credativ](http://www.credativ.com/). See on soovitatav lähenemine võrreldes pildi loomine algusest peale. Näiteks luua Debian 8 VHD käivitage järgmised käsud allalaadimiseks Azure'i juhtida (ja sõltuvused) ja käivitage azure_build_image skript:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Käsitsi Debian VHD ettevalmistamine

1. Valige Hyper-V halduri, virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina konsooli akna avamiseks.

3. Kommentaari välja **deb cdrom** sisse rida `/etc/apt/source.list` Kui häälestate VM vastu ISO-faili.

4. Redigeeri selle `/etc/default/grub` fail ja muuta järgmiselt lisada täiendavad tuum parameetrite Azure **GRUB_CMDLINE_LINUX** parameeter.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Taastada grub ja käivitada.

        # sudo update-grub

6. Lisage Debian's Azure hoidlate /etc/apt/sources.list Debian 7 või 8.

    **Debian 7.x "vilistav"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Installige Azure'i Linux Agent.

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7, on vaja käivitada 3.16-põhine tuum vilistav backports hoidla. Esmalt looge fail nimega /etc/apt/preferences.d/linux.pref sisuga järgmist:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Seejärel Käivita "sudo apt-get install Linuxi-pilt-amd64" uus tuum installimiseks.

8. Deprovision virtuaalse masina ja selle ettevalmistamine Azure ettevalmistamise ja käivitage:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klõpsake **toimingu** -> Sule alla Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.


## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis oma Debian virtuaalse kõvaketta abil saate luua uue virtuaalmasinates Azure. Kui see on esimene kord, et üleslaaditava faili .vhd Azure, lugege juhiseid 2 – 3 [loomine](virtual-machines-linux-classic-create-upload-vhd.md)ja üleslaadimise virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteemi.
