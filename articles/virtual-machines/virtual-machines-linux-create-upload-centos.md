<properties
    pageTitle="Luua ja üles laadida CentOS vastavalt Linux VHD Azure"
    description="Siit saate teada, luua ja üles laadida on Azure virtuaalse kõvaketta (VHD) mis sisaldab operatsioonisüsteemi Linux CentOS-põhine."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Azure'i CentOS vastavalt virtuaalse masina ettevalmistamine

- [Azure'i CentOS 6.x virtuaalse masina ettevalmistamine](#centos-6x)
- [Azure'i CentOS 7.0 + virtuaalse masina ettevalmistamine](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Eeltingimused ##

Selles artiklis eeldatakse, et teil on juba installitud on CentOS (või sarnane tuletatud) Linux operatsioonisüsteemi virtuaalse kõvakettale. Mitme tööriistad olemas .vhd failidest, näiteks virtualization lahenduse nagu Hyper-V loomiseks. Lisateabe saamiseks vaadake [Hyper-V roll ja konfigureerimine virtuaalse masina installimine](http://technet.microsoft.com/library/hh846766.aspx).


**CentOS installimärkmed**

- Lugege ka [Üldine Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) rohkem näpunäiteid Azure'i Linux ettevalmistamine.

- VHDX vorming ei toeta Azure, ainult **kindla VHD**.  VHD vorming Hyper-V halduri või Teisenda-vhd cmdlet-käsu abil saate teisendada ketas.

- Linuxi süsteemi installimisel on soovitatav kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing.  [LVM](virtual-machines-linux-configure-lvm.md) või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.

- NUMA suuremad VM suurused Linuxi tuum versioonid all 2.6.37 vea tõttu ei toetata. See probleem mõjutab peamiselt jaotuse abil eelnev punane rolli 2.6.32 tuum. Käsitsi installimine Azure'i Linux agent (waagent) keelatakse automaatselt NUMA Linuxi tuum GRUB konfiguratsioon. Lisateavet selle kohta leiate alltoodud juhiseid.

- Konfigureerige Vaheta sektsiooni OS kettal. Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks.  Lisateavet selle kohta leiate alltoodud juhiseid.

- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Valige Hyper-V halduri, virtuaalse masina.

2. Klõpsake nuppu **Loo ühendus** virtuaalse masina konsooli akna avamiseks.

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

        # sudo chkconfig network on


8. **CentOS 6.3 ainult**: draiverite jaoks Linux Integration Services (Poolelioleva).

    **Tähtis: Samm on ainult CentOS 6.3 jaoks sobiv ja varasemad versioonid.**  CentOS 6.4 + Linux Integration Services on *veel saadaval standard tuum*.

    - Installimise juhiste järgi [Poolelioleva allalaadimislehele](https://www.microsoft.com/en-us/download/details.aspx?id=46842) ja installige soovitud pööret MINUTIS peale teie pilt.  


9. Installige python-pyasn1 pakett, käivitage järgmine käsk:

        # sudo yum install python-pyasn1

10. Kui soovite kasutada jooksul Azure andmekeskuste majutatud OpenLogic peeglid, siis asendada järgmiste hoidlate /etc/yum.repos.d/CentOS-Base.repo faili.  Selle funktsiooni **[openlogic]** hoidlas, mis sisaldab pakette Azure'i Linux agent ka:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Märkus:** Sellest juhendist ülejäänud endale kasutate vähemalt [openlogic] repo, mida kasutatakse installida Azure Linuxi agent allpool.


11. Lisage järgmine rida /etc/yum.conf.

        http_caching=packages

    Ja **CentOS 6.3 ainult** lisage järgmine rida.

        exclude=kernel*

12. Keela yum mooduli "fastestmirror", redigeerides faili "/ etc/yum/pluginconf.d/fastestmirror.conf", ja alla `[main]`, tippige järgmine:

        set enabled=0

13. Praeguse yum metaandmete tühjendage järgmine käsk:

        # yum clean all

14. **Klõpsake CentOS 6.3 ainult**, värskendada tuum abil järgmine käsk:

        # sudo yum --disableexcludes=all install kernel

15. Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks avage "/ boot/grub/menu.lst" tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See Blokeeri NUMA CentOS 6 tuum versioon vea tõttu.

    Lisaks on soovitatav *eemaldada* järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter vähendab saadaval mälu VM 128MB või rohkem, mis võib olla väiksemate VM suurused probleemne.


16. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel.  See on tavaliselt vaikimisi.

17. Installige Azure'i Linuxi Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent

    Pange tähele, et WALinuxAgent paketi installimist eemaldatakse selle NetworkManager NetworkManager-gnome pakettide kui need pole juba eemaldati, nagu on kirjeldatud juhises 2.

18. Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Muudatuste CentOS 7 (ja sarnase derivaadid)**

Azure'i CentOS 7 virtuaalse masina ettevalmistamine on väga sarnane CentOS 6, aga on mitmeid olulisi erinevusi märkimist:

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

6.  Udev reeglid vältida staatilise reeglid Ethernet liidese/liideste loomist muuta. Reegleid võib põhjustada probleeme, kui Microsoft Azure'i või Hyper-v virtuaalse masina kloonimine

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

7. Installige python-pyasn1 pakett, käivitage järgmine käsk:

        # sudo yum install python-pyasn1

8. Kui soovite kasutada jooksul Azure andmekeskuste majutatud OpenLogic peeglid, siis asendada järgmiste hoidlate /etc/yum.repos.d/CentOS-Base.repo faili.  Selle funktsiooni **[openlogic]** hoidlas, mis sisaldab pakette Azure'i Linux agent ka:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Märkus:** Sellest juhendist ülejäänud endale kasutate vähemalt [openlogic] repo, mida kasutatakse installida Azure Linuxi agent allpool.

9.  Praeguse yum metaandmete tühjendamiseks ja installige värskendused järgmine käsk:

        # sudo yum clean all
        # sudo yum -y update

10. Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selleks, avatud "/ grub/jne/vaikimisi" tekstiredaktoris ja Redigeeri selle `GRUB_CMDLINE_LINUX` parameeter, näiteks:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See ka lülitab CentOS 7 nimereeglid NICs. Lisaks on soovitatav *eemaldada* järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter vähendab saadaval mälu VM 128MB või rohkem, mis võib olla väiksemate VM suurused probleemne.

11. Kui olete redigeerimise "/ grub/jne/vaikimisi" kohta, käivitage järgmine käsk taastada grub konfiguratsioon:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Veenduge, et SSH server on installinud ja konfigureerinud käivitamisel alustada.  See on tavaliselt vaikimisi.

13. **Ainult juhul, kui koostamise VMWare, VirtualBox või KVM pilt:** Saate lisada initramfs Hyper-V moodulid:

    Redigeeri `/etc/dracut.conf`, lisage sisu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Taastada selle initramfs:

        # dracut –f -v

14. Installige Azure'i Linuxi Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Luua Vaheta ruumi kettal OS.

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linuxi Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klõpsake- **toimingu -> Sule alla** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.

## <a name="next-steps"></a>Järgmised sammud
Nüüd olete valmis CentOS Linux virtuaalse kõvaketta abil saate luua uue virtuaalmasinates Azure. Kui see on esimene kord, et üleslaaditava faili .vhd Azure, lugege juhiseid 2 – 3 [loomine](virtual-machines-linux-classic-create-upload-vhd.md)ja üleslaadimise virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteemi.
