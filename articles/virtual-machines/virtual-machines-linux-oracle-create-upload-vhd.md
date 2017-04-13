<properties
    pageTitle="Luua ja üles laadida ka Oracle'i Linux VHD | Microsoft Azure'i"
    description="Siit saate teada, luua ja üles laadida ka Azure virtuaalse kõvaketta (VHD) sisaldava Oracle'i Linux operatsioonisüsteemi."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure'i ka Oracle'i Linux virtuaalse masina ettevalmistamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Eeltingimused ##

Selles artiklis eeldatakse, et on juba installitud Oracle'i Linux operatsioonisüsteemi virtuaalse kõvakettale. Mitme tööriistad olemas .vhd failidest, näiteks virtualization lahenduse nagu Hyper-V loomiseks. Lisateabe saamiseks vaadake [Hyper-V roll ja konfigureerimine virtuaalse masina installimine](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Oracle'i Linux installimärkmed

- Lugege ka [Üldine Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) rohkem näpunäiteid Azure'i Linux ettevalmistamine.

- Oracle'i punane rolli ühilduvad tuum ja nende UEK3 (purunematu ettevõtte tuum) on mõlemad toeta Hyper-V ja Azure. Parimate tulemuste saamiseks kindlasti värskendamiseks uusimate tuum oma Oracle'i Linux VHD koostamise ajal.

- Oracle'i UEK2 ei toeta Hyper-V ja Azure nagu ei sisalda nõutud draiverid.

- VHDX vorming ei toeta Azure, ainult **kindla VHD**.  VHD vorming Hyper-V halduri või Teisenda-vhd cmdlet-käsu abil saate teisendada ketas.

- Linuxi süsteemi installimisel on soovitatav kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. [LVM](virtual-machines-linux-configure-lvm.md) või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.

- NUMA suuremad VM suurused Linuxi tuum versioonid all 2.6.37 vea tõttu ei toetata. See probleem mõjutab peamiselt jaotuse abil eelnev punane rolli 2.6.32 tuum. Käsitsi installimine Azure'i Linux agent (waagent) keelatakse automaatselt NUMA Linuxi tuum GRUB konfiguratsioon. Lisateavet selle kohta leiate alltoodud juhiseid.

- Konfigureerige Vaheta sektsiooni OS kettal. Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks.  Lisateavet selle kohta leiate alltoodud juhiseid.

- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.

- Veenduge, et selle `Addons` hoidla on lubatud. Faili redigeerimiseks `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) või `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), ja muuta rida `enabled=0` et `enabled=1` **[ol6_addons]** või **[ol7_addons]** selle faili.


## <a name="oracle-linux-64"></a>Oracle'i Linux 6.4 + ##

Teatud konfigureerimistoimingute operatsioonisüsteemi virtual masina Azure'i käivitamiseks peate täitma.

1. Valige keskmisel paanil Hyper-V halduri virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina akna avamiseks.

3. Desinstallige NetworkManager, käivitage järgmine käsk:

        # sudo rpm -e --nodeps NetworkManager

    **Märkus:** Kui pakett on juba installitud, see käsk ei õnnestu kuvatakse tõrketeade. See peaks.

4.  Looge fail nimega **võrk** on `/etc/sysconfig/` kausta, mis sisaldab järgmine tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Looge fail nimega **ifcfg-eth0** klõpsake soovitud `/etc/sysconfig/network-scripts/` kausta, mis sisaldab järgmine tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Udev reeglid vältida staatilise reeglid Ethernet liidese/liideste loomist muuta. Reegleid võib põhjustada probleeme, kui Microsoft Azure'i või Hyper-v virtuaalse masina kloonimine

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # chkconfig network on

8. Installige python-pyasn1, käivitage järgmine käsk:

        # sudo yum install python-pyasn1

9.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Teha seda Ava "/ boot/grub/menu.lst" tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See Blokeeri NUMA Oracle'i punane rolli ühilduvad tuum vea tõttu.

    Lisaks on soovitatav *eemaldada* järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter vähendab saadaval mälu VM 128MB või rohkem, mis võib olla väiksemate VM suurused probleemne.


10. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel.  See on tavaliselt vaikimisi.

11. Installige Azure'i Linux Agent, käivitage järgmine käsk. Uusim versioon on 2.0.15.

        # sudo yum install WALinuxAgent

    Pange tähele, et WALinuxAgent paketi installimist eemaldatakse selle NetworkManager NetworkManager-gnome pakettide kui need pole juba eemaldati, nagu on kirjeldatud juhises 2.

12. Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linuxi Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.


----------


## <a name="oracle-linux-70"></a>Oracle'i Linux 7.0 + ##

**Oracle'i Linux 7 muudatused**

Azure'i ka Oracle'i Linux 7 virtuaalse masina ettevalmistamine on väga sarnane Oracle'i Linux 6, aga on mitmeid olulisi erinevusi märkimist:

 - Punane rolli ühilduvad tuum nii Oracle'i UEK3 on toetatud Azure.  Soovitatav on UEK3 tuum.
 - NetworkManager paketi enam vastuolus agent Azure Linux. Vaikimisi installitakse selle paketi ja soovitame, et see on eemaldatud.
 - Nüüd kasutatakse GRUB2 vaikimisi bootloader, et protseduuri tuum parameetrite redigeerimiseks on muutunud (vt allpool).
 - XFS on nüüd failisüsteemi. Ext4 failisüsteemis saate soovi korral siiski kasutada.


**Konfigureerimistoimingute**

1. Valige Hyper-V halduri, virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina konsooli akna avamiseks.

3.  Looge fail nimega **võrk** on `/etc/sysconfig/` kausta, mis sisaldab järgmine tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Looge fail nimega **ifcfg-eth0** klõpsake soovitud `/etc/sysconfig/network-scripts/` kausta, mis sisaldab järgmine tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Udev reeglid vältida staatilise reeglid Ethernet liidese/liideste loomist muuta. Reegleid võib põhjustada probleeme, kui Microsoft Azure'i või Hyper-v virtuaalse masina kloonimine

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

7. Installige python-pyasn1 pakett, käivitage järgmine käsk:

        # sudo yum install python-pyasn1

8.  Praeguse yum metaandmete tühjendamiseks ja installige värskendused järgmine käsk:

        # sudo yum clean all
        # sudo yum -y update

9.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selleks avatud "/ grub/jne/vaikimisi" tekstiredaktoris ja Redigeeri selle `GRUB_CMDLINE_LINUX` parameeter, näiteks:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See ka lülitab OEL 7 nimereeglid NICs. Lisaks on soovitatav *eemaldada* järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter vähendab saadaval mälu VM 128MB või rohkem, mis võib olla väiksemate VM suurused probleemne.


10. Kui olete redigeerimise "/ grub/jne/vaikimisi" kohta, käivitage järgmine käsk taastada grub konfiguratsioon:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel.  See on tavaliselt vaikimisi.

12. Installige Azure'i Linuxi Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.


## <a name="next-steps"></a>Järgmised sammud
Nüüd olete valmis oma Oracle'i Linux .vhd abil saate luua uue virtuaalmasinates Azure. Kui see on esimene kord, et üleslaaditava faili .vhd Azure, lugege juhiseid 2 – 3 [loomine](virtual-machines-linux-classic-create-upload-vhd.md)ja üleslaadimise virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteemi.
