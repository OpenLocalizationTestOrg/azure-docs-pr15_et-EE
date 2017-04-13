<properties
pageTitle="Oracle'i Linux virtuaalse masina ettevalmistamine Azure'i | Microsoft Azure'i"
description="Samm-sammult konfiguratsiooni Oracle'i virtuaalse masina töötab Linux Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure'i ka Oracle'i Linux virtuaalse masina ettevalmistamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Azure'i ka Oracle'i Linux 6.4 + virtuaalse masina ettevalmistamine](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Azure'i ka Oracle'i Linux 7.0 + virtuaalse masina ettevalmistamine](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et on juba installitud Oracle'i Linux operatsioonisüsteemi virtuaalse kõvakettale. Mitme tööriistad olemas .vhd failidest, näiteks virtualization lahenduse nagu Hyper-V loomiseks. Juhised leiate teemast [installida Hyper-V ja luua virtuaalse masina](http://technet.microsoft.com/library/hh846766.aspx).

**Oracle'i Linux installimärkmed**

- Oracle'i punane rolli ühilduvad tuum ja selle UEK3 (purunematu ettevõtte tuum) on mõlemad toeta Hyper-V ja Azure. Parimate tulemuste saamiseks kindlasti värskendamiseks uusimate tuum oma Oracle'i Linux VHD koostamise ajal.

- Oracle'i UEK2 ei toeta Hyper-V ja Azure nagu ei sisalda nõutud draiverid.

- Azure'i ei toeta VHDX uude vormingusse. Hyper-V halduri või Teisenda-vhd cmdlet-käsu abil saate teisendada VHD vormingus ketas.

- Linuxi süsteemi installimisel soovitame kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. LVM või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.

- NUMA suuremad VM suurused Linuxi tuum versioonid all 2.6.37 vea tõttu ei toetata. See probleem mõjutab peamiselt jaotuse, mis kasutavad eelnev punane rolli 2.6.32 tuum. Käsitsi installimine Azure'i Linux agent (waagent) keelatakse automaatselt NUMA Linuxi tuum GRUB konfiguratsioon. Lisateavet selle kohta leiate alltoodud juhiseid.

- Konfigureerige Vaheta sektsiooni OS kettal. Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks. Lisateavet selle kohta leiate alltoodud juhiseid.

- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.

- Veenduge, et selle `Addons` hoidla on lubatud. Faili redigeerimiseks valige `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) või `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), ja muuta rida `enabled=0` et `enabled=1` **[ol6_addons]** või **[ol7_addons]** selle faili.


## <a name="oracle-linux-64"></a>Oracle'i Linux 6.4 +
Teatud konfigureerimistoimingute operatsioonisüsteemi virtual masina Azure'i käivitamiseks peate täitma.

1. Valige keskmisel paanil Hyper-V halduri virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina akna avamiseks.

3. Desinstallige NetworkManager, käivitage järgmine käsk:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Kui pakett on juba installitud, see käsk ei õnnestu kuvatakse tõrketeade. See peaks.

4. Looge fail nimega **** võrgu/etc/sysconfig/kausta, mis sisaldab järgmine tekst:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Looge fail nimega klõpsake soovitud /etc/sysconfig/network-scripts **ifcfg-eth0** / kausta, mis sisaldab järgmine tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Liikumine (või eemaldada) udev reeglid vältida Ethernet kasutajaliidese staatilise reeglid. Reegleid põhjustada probleeme, kui te kasutate kloonimine virtuaalse masina Azure'i või Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # chkconfig network on

8.  Installige python-pyasn1, käivitage järgmine käsk:

        # sudo yum install python-pyasn1

9.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks avage "/ boot/grub/menu.lst" tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See Blokeeri NUMA Oracle'i punane rolli ühilduvad tuum vea tõttu.

    Lisaks ülaltoodud, soovitame teil *eemaldada* järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

10.  Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel. See on tavaliselt vaikimisi.

11.  Installige Azure'i Linuxi Agent, käivitage järgmine käsk:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum installida WALinuxAgent

    Pange tähele, et WALinuxAgent paketi installimist eemaldatakse selle NetworkManager NetworkManager-gnome pakettide kui need pole juba eemaldati, nagu on kirjeldatud juhises 2.

12.  Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM. Pange tähele, et kohaliku ressursi ketas on *ajutine* kettal tühjendatakse, kui VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Märkus: määrake see iganes teil vaja seda.

13.  Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-jõustada - deprovision
        # <a name="export-histsize0"></a>ekspordi HISTSIZE = 0
        # <a name="logout"></a>Logi välja

14.  Klõpsake **toimingu -\> sulgeda** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.

## <a name="oracle-linux-70"></a>Oracle'i Linux 7.0 +
**Oracle'i Linux 7 muudatused**

Azure'i ka Oracle'i Linux 7 virtuaalse masina ettevalmistamine on väga sarnane Oracle'i Linux 6 protsess. Siiski on mitu olulist erinevused märkimist.

-   Punane rolli ühilduvad tuum nii Oracle'i UEK3 on toetatud Azure. Soovitame UEK3 tuum.

-   NetworkManager paketi enam vastuolus agent Azure Linux. Vaikimisi installitakse selle paketi ja soovitame, et see on eemaldatud.

-   Nüüd kasutatakse GRUB2 vaikimisi bootloader, et protseduuri tuum parameetrite redigeerimiseks on muutunud (vt allpool).

-   XFS on nüüd failisüsteemi. Ext4 failisüsteemis saate soovi korral siiski kasutada.

**Konfigureerimistoimingute**

1.  Valige Hyper-V halduri, virtuaalse masina.

2.  Klõpsake nuppu **Loo ühendus** virtuaalse masina konsooli akna avamiseks.

3.  Looge fail nimega **** võrgu/etc/sysconfig/kausta, mis sisaldab järgmine tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Looge fail nimega klõpsake soovitud /etc/sysconfig/network-scripts **ifcfg-eth0** / kausta, mis sisaldab järgmine tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Liikumine (või eemaldada) udev reeglid vältida Ethernet kasutajaliidese staatilise reeglid. Reegleid põhjustada probleeme, kui te kasutate Microsoft Azure'i või Hyper-V virtuaalse masina kloonimine.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

7.  Installige python-pyasn1 pakett, käivitage järgmine käsk:

        # sudo yum install python-pyasn1

8.  Praeguse yum metaandmete tühjendamiseks ja installige värskendused järgmine käsk:

        # sudo yum clean all
        # sudo yum -y update

9.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selleks, avatud "/ grub/jne/vaikimisi" tekstiredaktoris ja Redigeeri GRUB\_KÄSUREA\_LINUX parameeter, näiteks:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. Lisaks ülaltoodud, soovitame teil *eemaldada* järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

10.  Kui olete redigeerimise "/ grub/jne/vaikimisi", taastada grub konfiguratsioon järgmine käsk:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2-mkconfig - o /boot/grub2/grub.cfg

11.  Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel. See on tavaliselt vaikimisi.

12.  Installige Azure'i Linuxi Agent, käivitage järgmine käsk:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum installida WALinuxAgent

13.  Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM. Pange tähele, et kohaliku ressursi ketas on *ajutine* kettal tühjendatakse, kui VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Märkus: määrake see iganes teil vaja seda.

14.  Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-jõustada - deprovision
        # <a name="export-histsize0"></a>ekspordi HISTSIZE = 0
        # <a name="logout"></a>Logi välja

15.  Klõpsake **toimingu -\> sulgeda** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.
