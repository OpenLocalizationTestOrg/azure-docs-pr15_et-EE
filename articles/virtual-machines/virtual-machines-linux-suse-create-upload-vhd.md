<properties
    pageTitle="Luua ja üles laadida SUSE Linux VHD Azure"
    description="Siit saate teada, luua ja üles laadida on Azure virtuaalse kõvaketta (VHD) mis sisaldab operatsioonisüsteemi SUSE Linux."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Azure'i SLES või openSUSE virtuaalse masina ettevalmistamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Eeltingimused ##

Selles artiklis eeldatakse, et teil on juba installitud SUSE või openSUSE Linux operatsioonisüsteemi virtuaalse kõvakettale. Mitme tööriistad olemas .vhd failidest, näiteks virtualization lahenduse nagu Hyper-V loomiseks. Lisateabe saamiseks vaadake [Hyper-V roll ja konfigureerimine virtuaalse masina installimine](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE installimärkmed

- Lugege ka [Üldine Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) rohkem näpunäiteid Azure'i Linux ettevalmistamine.

- VHDX vorming ei toeta Azure, ainult **kindla VHD**.  VHD vorming Hyper-V halduri või Teisenda-vhd cmdlet-käsu abil saate teisendada ketas.

- Linuxi süsteemi installimisel on soovitatav kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. [LVM](virtual-machines-linux-configure-lvm.md) või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.

- Konfigureerige Vaheta sektsiooni OS kettal. Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks.  Lisateavet selle kohta leiate alltoodud juhiseid.

- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.


## <a name="use-suse-studio"></a>Kasutage SUSE Studio
[SUSE Studio](http://www.susestudio.com) saate hõlpsalt luua ja hallata oma SLES ja openSUSE pilte Azure ja Hyper-V. See on soovitatav lähenemine kohandamise oma SLES ja openSUSE pildid.

Alternatiivina koostamise oma VHD SUSE avaldab ka BYOS (tuua oma oma tellimust) piltide SLES [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007)juures.


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 ettevalmistamine ##

1. Valige keskmisel paanil Hyper-V halduri virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina akna avamiseks.

3. Lubage see värskendusi alla laadida ja installida pakettide arvutisüsteemi SUSE Linux Enterprise registreerida.

4. Värskendage uusima plaastrid süsteem.

        # sudo zypper update

5. Installige Azure'i Linux Agent SLES hoidla.

        # sudo zypper install WALinuxAgent

6. Kui waagent on seatud "on" chkconfig sisse möllida, ja kui pole, lubage selle automaatselt käivitada ka:
               
        # sudo chkconfig waagent on

7. Kontrollige, kui waagent teenus töötab, ja kui pole, käivitage see. 

        # sudo service waagent start
                
8. Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Teha seda Ava "/ boot/grub/menu.lst" tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    See tagab kõigi konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid.

9. Veenduge, et /boot/grub/menu.lst ja /etc/fstab mõlemad viidata ketast selle UUID (järgi uuid) asemel ketta ID (järgi-id). 

    Ketas UUID hankimine
    
        # ls /dev/disk/by-uuid/

    Kui /dev/disk/by-id / on kasutanud, värskendada nii /boot/grub/menu.lst ja/jne/fstab proper abil uuid väärtusega

    Enne muutmine
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Pärast muutmine
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Udev reeglid vältida staatilise reeglid Ethernet liidese/liideste loomist muuta. Reegleid võib põhjustada probleeme, kui Microsoft Azure'i või Hyper-v virtuaalse masina kloonimine

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. See on soovitatav faili redigeerimiseks "/ jne/sysconfig võrgu dhcp" ja muuta selle `DHCLIENT_SET_HOSTNAME` parameeter järgmist:

        DHCLIENT_SET_HOSTNAME="no"

12. "/ Jne/sudoers", kommentaaride välja või Eemalda järgmised read, kui need on olemas.

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel.  See on tavaliselt vaikimisi.

14. Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linuxi Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.


----------

## <a name="prepare-opensuse-131"></a>OpenSUSE 13.1 + ettevalmistamine ##

1. Valige keskmisel paanil Hyper-V halduri virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina akna avamiseks.

3. Kest, käivitage käsk "`zypper lr`". Kui käsk tagastab väärtuse väljundi järgmise sisuga, siis on hoidlate on õigesti konfigureeritud--pole ei ole vaja (Pange tähele, et numbrid võivad olla erinevad):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Kui käsk tagastab "Hoidlate määratletud..." siis nende repos lisamiseks kasutada järgmised käsud:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Seejärel saate kontrollida selle hoidlate on lisatud käsu "`zypper lr`' uuesti. Juhuks, kui üks oluline värskendus hoidlate on lubatud, lubage see järgmine käsk:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Värskenda tuum uusimat saadaolevat:

        # sudo zypper up kernel-default

    Või värskendamiseks uusimate plaastrid süsteem.

        # sudo zypper update

5.  Installige Azure'i Linux Agent.

        # sudo zypper install WALinuxAgent

6.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks avage "/ boot/grub/menu.lst" tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    See tagab kõigi konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. Lisaks, eemaldage järgmiste parameetrite tuum buutimine rea kui need on olemas:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  See on soovitatav faili redigeerimiseks "/ jne/sysconfig võrgu dhcp" ja muuta selle `DHCLIENT_SET_HOSTNAME` parameeter järgmist:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Oluline:** "/ Jne/sudoers", kommentaaride välja või Eemalda järgmised read, kui need on olemas.

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel.  See on tavaliselt vaikimisi.

10. Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linuxi Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Veenduge, et Azure Linux Agent töötab käivitamisel:

        # sudo systemctl enable waagent.service

13. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.

## <a name="next-steps"></a>Järgmised sammud
Nüüd olete valmis SUSE Linux virtuaalse kõvaketta abil saate luua uue virtuaalmasinates Azure. Kui see on esimene kord, et üleslaaditava faili .vhd Azure, lugege juhiseid 2 – 3 [loomine](virtual-machines-linux-classic-create-upload-vhd.md)ja üleslaadimise virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteemi.
