<properties
    pageTitle="Luua ja üles laadida on punane rolli Enterprise Linux VHD Azure kasutamine"
    description="Siit saate teada, luua ja üles laadida on Azure virtuaalse kõvaketta (VHD) sisaldava operatsioonisüsteem on punane rolli Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Azure'i on punane rolli-põhise arvutiga ettevalmistamine

Sellest artiklist saate teada, kuidas Red rolli Enterprise Linux (RHEL) virtuaalse masina valmistuda Azure kasutamine. RHEL versiooni, mis asuvad selles artiklis on 6,7, 7.1 ja 7.2. Hypervisors koostamiseks, mis asuvad selles artiklis on Hyper-V, tuum põhinev virtuaalse masina (KVM) ja VMware. Vastavusnõuded osalemine punane rolli pilvepõhise juurdepääsu programmi kohta leiate lisateavet teemast [punane rolli pilvepõhise juurdepääsu veebisaidi](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) ja [Azure RHEL töötab](https://access.redhat.com/articles/1989673).

[Ettevalmistused RHEL 6,7 virtuaalse masina Hyper-V halduri kaudu](#rhel67hyperv)

[Ettevalmistused RHEL 7.1/7.2 virtuaalse masina Hyper-V halduri kaudu](#rhel7xhyperv)

[Ettevalmistused RHEL 6,7 virtuaalse masina KVM kaudu](#rhel67kvm)

[Ettevalmistused RHEL 7.1/7.2 virtuaalse masina KVM kaudu](#rhel7xkvm)

[VMware RHEL 6,7 virtuaalse masina ettevalmistamine](#rhel67vmware)

[VMware RHEL 7.1/7.2 virtuaalse masina ettevalmistamine](#rhel7xvmware)

[Ettevalmistused RHEL 7.1/7.2 virtuaalse masina kickstart failist](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Ettevalmistused on punane rolli-põhise arvutiga Hyper-V halduri kaudu
### <a name="prerequisites"></a>Eeltingimused
Selles jaotises eeldab, et on juba installitud RHEL pilt (ISO-failist punane rolli veebisaidilt saadava) virtuaalse kõvakettale (VHD). Operatsioonisüsteemi pildi installimiseks Hyper-V halduri kasutamise kohta leiate lisateavet teemast [Hyper-V roll ja konfigureerimine virtuaalse masina installimine](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL installimärkmed**

- Lugege ka [Üldist Linux Installimärkmed](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) rohkem näpunäiteid Azure'i Linux ettevalmistamine.

- Azure'i ei toeta VHDX uude vormingusse. Hyper-V halduri või **Teisenda-vhd** PowerShelli cmdlet-käsu abil saate teisendada VHD vormingus ketas.

- VHDs peavad olema loodud nagu "fikseeritud"--dünaamiline VHDs ei toetata.

- Linuxi süsteemi installimisel soovitame kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. LVM või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale, kui eelistatud.

- Konfigureerige Vaheta sektsiooni OS kettal. Saate konfigureerida Linux agent kettal ajutine ressursi Vaheta faili loomiseks. Lisateavet selle kohta on saadaval alltoodud juhiseid.

- Kõik selle VHDs peavad olema suurused, mis on kordne 1 MB.

- **Qemu-img** kasutamisel kettale piltide VHD vormingusse teisendada, Pange tähele, et teadaolev viga qemu-img versioonides 2.2.1 või uuem versioon. Selle vea, on tulemuseks on valesti vormindatud VHD. Probleemi eesmärk on eelseisvad versiooni qemu-img kindlaks määrata. Nüüd, soovitame kasutada qemu-img versioon 2.2.0 või PowerPointi varasemas versioonis.

### <a id="rhel67hyperv"> </a>Hyper-V Manager RHEL 6,7 virtuaalse masina ettevalmistamine###


1.  Valige Hyper-V halduri, virtuaalse masina.

2.  Klõpsake nuppu **Loo ühendus** virtuaalse masina konsooli akna avamiseks.

3.  Desinstallige NetworkManager, käivitage järgmine käsk:

        # sudo rpm -e --nodeps NetworkManager

    Pange tähele, et kui pakett on juba installitud, see käsk nurjub tõrketeate. See peaks.

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

6.  Liikumine (või eemaldada) udev reeglid vältida Ethernet kasutajaliidese staatilise reeglid. Reegleid põhjustada probleeme, kui te klooni virtuaalse masina Microsoft Azure'i või Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

8.  Registreerida tellimuse punane rolli lubamiseks pakettide RHEL hoidla installi, käivitage järgmine käsk:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  WALinuxAgent paketi `WALinuxAgent-<version>` on lükatud punane rolli lisad hoidla. Luba lisad hoidla, käivitage järgmine käsk:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks Avage `/boot/grub/menu.lst` tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See Blokeeri NUMA tuum versioon, mida kasutatakse RHEL 6 vea tõttu.

    Lisaks ülaltoodud toimingu, soovitame eemaldada järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Suvandi crashkernel võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

11. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel. See on tavaliselt vaikimisi. Järgmine rida kaasata /etc/ssh/sshd_config muutmiseks tehke järgmist.

        ClientAliveInterval 180

12. Installige Azure'i Linux Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Pange tähele, et WALinuxAgent paketi installimist eemaldatakse selle NetworkManager NetworkManager-gnome pakettide kui need pole juba eemaldati, nagu on kirjeldatud juhises 2.

13. Luua Vaheta ruumi kettal OS.
Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on seotud VM pärast VM on ette valmistatud Azure. Pange tähele, et kohaliku ressursi ketas on ajutine ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Unregister tellimuse (vajadusel), käivitage järgmine käsk:

        # sudo subscription-manager unregister

15. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klõpsake **toimingu > sulgeda** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.
 

### <a id="rhel7xhyperv"> </a>Hyper-V Manager RHEL 7.1/7.2 virtuaalse masina ettevalmistamine###

1.  Valige Hyper-V halduri, virtuaalse masina.

2.  Klõpsake nuppu **Loo ühendus** virtuaalse masina konsooli akna avamiseks.

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

5.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

6.  Registreerida tellimuse punane rolli lubamiseks pakettide RHEL hoidla installi, käivitage järgmine käsk:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks Avage `/etc/default/grub` tekstiredaktoris ja Redigeeri **GRUB_CMDLINE_LINUX** parameeter. Näiteks:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. Lisaks ülaltoodud toimingu, soovitame eemaldada järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.
    Suvandi crashkernel võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

8.  Kui olete lõpetanud redigeerimise `/etc/default/grub`, taastada grub konfiguratsioon järgmine käsk:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel. See on tavaliselt vaikimisi. Muutke `/etc/ssh/sshd_config` lisada järgmine rida:

        ClientAliveInterval 180

10. WALinuxAgent paketi `WALinuxAgent-<version>` on lükatud punane rolli lisad hoidla. Luba lisad hoidla, käivitage järgmine käsk:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installige Azure'i Linux Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Luua Vaheta ruumi kettal OS. Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on seotud VM pärast VM on ette valmistatud Azure. Pange tähele, et kohaliku ressursi ketas on ajutine ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta järgmist parameetrid `/etc/waagent.conf` õigesti:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Kui soovite tellimuse tühistada, käivitage järgmine käsk:

        # sudo subscription-manager unregister

14. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klõpsake **toimingu > sulgeda** Hyper-V halduri. Oma Linux VHD on nüüd valmis Azure'i üles laadida.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Ettevalmistused on punane rolli-põhise arvutiga KVM kaudu

### <a id="rhel67kvm"> </a>Kaudu KVM RHEL 6,7 virtuaalse masina ettevalmistamine###


1.  KVM pilti RHEL 6,7 punane rolli veebisaidilt alla laadida.

2.  Root parooli.

    Luua ka krüptitud parool ja kopeerige käsu väljund:

        # openssl passwd -1 changeme

    Koos guestfish root parooli seadmiseks tehke järgmist.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Muuta root kasutaja teise välja "!" krüptitud parool.

3.  Luua virtuaalse masina KVM qcow2 pildilt, ketta tüübi määramine **qcow2**ja **virtio**virtuaalse võrgu kasutajaliidese seadme mudelist määramine. Seejärel alustage virtuaalse masina ja logige sisse root.

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

6.  Liikumine (või eemaldada) udev reegleid, et vältida Ethernet liidest staatilise reeglite loomist. Reegleid põhjustada probleeme, kui te klooni virtuaalse masina Microsoft Azure'i või Hyper-v

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # chkconfig network on

8.  Registreerida tellimuse punane rolli lubamiseks pakettide RHEL hoidla installi, käivitage järgmine käsk:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks Avage `/boot/grub/menu.lst` tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See Blokeeri NUMA tuum versioon, mida kasutatakse RHEL 6 vea tõttu.

    Lisaks ülaltoodud toimingu, soovitame eemaldada järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.
    Suvandi crashkernel võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter vähendab VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

10. Saate lisada initramfs Hyper-V moodulid:  

    Redigeeri `/etc/dracut.conf` ja sisu lisamine: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Taastada initramfs: # dracut – f - v

11. Pilveteenuse – käivitamise desinstallimiseks tehke järgmist.

        # yum remove cloud-init

12. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel:

        # chkconfig sshd on

    /Etc/ssh/sshd_config lisada järgmised read muutmiseks tehke järgmist.

        PasswordAuthentication yes
        ClientAliveInterval 180

    Taaskäivitage sshd.

        # service sshd restart

13. WALinuxAgent paketi `WALinuxAgent-<version>` on lükatud punane rolli lisad hoidla. Luba lisad hoidla, käivitage järgmine käsk:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Installige Azure'i Linux Agent, käivitage järgmine käsk:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on seotud VM pärast VM on ette valmistatud Azure. Pange tähele, et kohaliku ressursi ketas on ajutine ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta õigesti **/etc/waagent.conf** järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister tellimuse (vajadusel), käivitage järgmine käsk:

        # subscription-manager unregister

17. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Sulgege VM KVM.

19. VHD vormingusse teisendamine qcow2 pilt.
    Esmalt pildi teisendamine vormingusse:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Veenduge, et töötlemata pilt on joondatud 1 MB. Muul juhul ümardamine joondamiseks 1 MB suurus: # MB = $((1024*1024)) # suurus = $(qemu-img teave – f töötlemata--väljund json "rhel 6.7.raw" | \ gawk "vastavad ($0, /"virtuaalse suurus": ([0 – 9] +), /, val) {printimine val[1]}') # rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Teisendage kindlaksmääratud suurusega VHD töötlemata ketas.

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Kaudu KVM RHEL 7.1/7.2 virtuaalse masina ettevalmistamine###


1.  Punane rolli veebisaidilt alla laadida KVM pildi RHEL 7.1 (või 7.2). Kasutame RHEL 7.1 näitega siin.

2.  Root parooli.

    Luua ka krüptitud parool ja kopeerige käsu väljund.

        # openssl passwd -1 changeme

    Koos guestfish parooli juurkausta.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Muuta root kasutaja teise välja "!" krüptitud parool.

3.  Luua virtuaalse masina KVM qcow2 pildilt, ketta tüüp väärtuseks **qcow2**ja **virtio**virtuaalse võrgu kasutajaliidese seadme mudelist määramine. Seejärel alustage virtuaalse masina ja logige sisse root.

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

6.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # chkconfig network on

7.  Registreerida tellimuse punane rolli lubamiseks pakettide RHEL hoidla installi, käivitage järgmine käsk:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks Avage `/etc/default/grub` tekstiredaktoris ja Redigeeri **GRUB_CMDLINE_LINUX** parameeter. Näiteks:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. Lisaks ülaltoodud toimingu, soovitame eemaldada järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.
    Suvandi crashkernel võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

9.  Kui olete lõpetanud redigeerimise `/etc/default/grub`, taastada grub konfiguratsioon järgmine käsk:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Saate lisada initramfs Hyper-V moodulid:

    Redigeeri `/etc/dracut.conf` ja lisage sisu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Taastada initramfs:

        # dracut –f -v

11. Pilveteenuse – käivitamise desinstallimiseks tehke järgmist.

        # yum remove cloud-init

12. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel:

        # systemctl enable sshd

    /Etc/ssh/sshd_config lisada järgmised read muutmiseks tehke järgmist.

        PasswordAuthentication yes
        ClientAliveInterval 180

    Taaskäivitage sshd.

        systemctl restart sshd

13. WALinuxAgent paketi `WALinuxAgent-<version>` on lükatud punane rolli lisad hoidla. Luba lisad hoidla, käivitage järgmine käsk:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installige Azure'i Linux Agent, käivitage järgmine käsk:

        # yum install WALinuxAgent

    Waagent teenuse lubamiseks tehke järgmist.

        # systemctl enable waagent.service

15. Luua Vaheta ruumi kettal OS. Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on seotud VM pärast VM on ette valmistatud Azure. Pange tähele, et kohaliku ressursi ketas on ajutine ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta järgmist parameetrid `/etc/waagent.conf` õigesti:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Unregister tellimuse (vajadusel), käivitage järgmine käsk:

        # subscription-manager unregister

17. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Virtuaalne arvuti rakenduses KVM sulgeda.

19. VHD vormingusse teisendamine qcow2 pilt.

    Esmalt pildi teisendamine vormingusse:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Veenduge, et töötlemata pilt on joondatud 1 MB. Muul juhul ümardamine joondamiseks 1 MB suurus:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Teisendage kindlaksmääratud suurusega VHD töötlemata ketas.

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>VMware on punane rolli-põhise arvutiga ettevalmistamine
### <a name="prerequisites"></a>Eeltingimused
Selles jaotises eeldab, et teil on juba installitud RHEL virtuaalse masina VMware. Operatsioonisüsteemi VMware installimise kohta leiate teemast [VMware Külastajate operatsioonisüsteemi installi juhend](http://partnerweb.vmware.com/GOSIG/home.html).

- Kui installite Linux operatsioonisüsteemi, soovitame kasutada standard sektsioonid, mitte LVM (sageli vaikimisi palju installid). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise VM tõrkeotsing. LVM või RAID saab kasutada andmete kettale, kui eelistatud.

- Konfigureerige Vaheta sektsiooni OS kettal. Saate konfigureerida Linux agent kettal ajutine ressursi Vaheta faili loomiseks. Lisateavet selle kohta leiate alltoodud juhiseid.

- Kui loote kettaruumi, valige **poe virtuaalse ketta ühe failina**.



### <a id="rhel67vmware"> </a>VMware RHEL 6,7 virtuaalse masina ettevalmistamine###

1.  Desinstallige NetworkManager, käivitage järgmine käsk:

         # sudo rpm -e --nodeps NetworkManager

    Pange tähele, et kui pakett on juba installitud, see käsk nurjub tõrketeate. See peaks.

2.  Looge fail nimega **** võrgu/etc/sysconfig/kausta, mis sisaldab järgmine tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Looge fail nimega klõpsake soovitud /etc/sysconfig/network-scripts **ifcfg-eth0** / kausta, mis sisaldab järgmine tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Liikumine (või eemaldada) udev reegleid, et vältida Ethernet liidest staatilise reeglite loomist. Reegleid põhjustada probleeme, kui te klooni virtuaalse masina Microsoft Azure'i või Hyper-v

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

6.  Registreerida tellimuse punane rolli lubamiseks pakettide RHEL hoidla installi, käivitage järgmine käsk:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  WALinuxAgent paketi `WALinuxAgent-<version>` on lükatud punane rolli lisad hoidla. Luba lisad hoidla, käivitage järgmine käsk:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks avage "/ boot/grub/menu.lst" tekstiredaktoris ja veenduge, et vaikimisi tuum sisaldab järgmisi:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. See Blokeeri NUMA tuum versioon, mida kasutatakse RHEL 6 vea tõttu.
    Lisaks ülaltoodud toimingu, soovitame eemaldada järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.
    Suvandi crashkernel võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

9.  Saate lisada initramfs Hyper-V moodulid:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel. See on tavaliselt vaikimisi. Muutke `/etc/ssh/sshd_config` lisada järgmine rida:

        ClientAliveInterval 180

11. Installige Azure'i Linux Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Loote Vaheta ruumi kettal OS:

    Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on seotud VM pärast VM on ette valmistatud Azure. Pange tähele, et kohaliku ressursi ketas on ajutine ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta järgmist parameetrid `/etc/waagent.conf` õigesti:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Unregister tellimuse (vajadusel), käivitage järgmine käsk:

        # sudo subscription-manager unregister

14. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Sulgeda VM ja VMDK faili teisendamine .vhd fail.

    Esmalt pildi teisendamine vormingusse:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Veenduge, et töötlemata pilt on joondatud 1 MB. Muul juhul ümardamine joondamiseks 1 MB suurus:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Teisendage kindlaksmääratud suurusega VHD töötlemata ketas.

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>VMware RHEL 7.1/7.2 virtuaalse masina ettevalmistamine###

1.  Looge fail nimega **** võrgu/etc/sysconfig/kausta, mis sisaldab järgmine tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Looge fail nimega klõpsake soovitud /etc/sysconfig/network-scripts **ifcfg-eth0** / kausta, mis sisaldab järgmine tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Veenduge, et võrguteenuse hakkavad käivitamisel, käivitage järgmine käsk:

        # sudo chkconfig network on

4.  Registreerida tellimuse punane rolli lubamiseks pakettide RHEL hoidla installi, käivitage järgmine käsk:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Muutke tuum buutimine rea lisada täiendavad tuum parameetrite Azure konfiguratsioonis grub. Selle tegemiseks Avage `/etc/default/grub` tekstiredaktoris ja Redigeeri **GRUB_CMDLINE_LINUX** parameeter. Näiteks:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Samuti tagatakse, et kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid. Lisaks ülaltoodud toimingu, soovitame eemaldada järgmiste parameetrite abil:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.
    Suvandi crashkernel võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter on vähendada VM 128 MB või rohkem mälu. See võib olla väiksemate VM suurused probleemne.

6.  Kui olete lõpetanud redigeerimise `/etc/default/grub`, taastada grub konfiguratsioon järgmine käsk:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Saate lisada initramfs Hyper-V moodulid:

    Redigeeri `/etc/dracut.conf`, lisage sisu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Taastada initramfs:

        # dracut –f -v

8.  Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel. See on tavaliselt vaikimisi. Muutke `/etc/ssh/sshd_config` lisada järgmine rida:

        ClientAliveInterval 180

9.  WALinuxAgent paketi `WALinuxAgent-<version>` on lükatud punane rolli lisad hoidla. Luba lisad hoidla, käivitage järgmine käsk:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installige Azure'i Linux Agent, käivitage järgmine käsk:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Luua Vaheta ruumi kettal OS. Azure'i Linux Agent saab automaatselt konfigureerida Vaheta ruumi kohaliku ressursi ketas, mis on seotud VM pärast VM on ette valmistatud Azure. Pange tähele, et kohaliku ressursi ketas on ajutine ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linux Agent (vt eelmises etapis), muuta järgmist parameetrid `/etc/waagent.conf` õigesti:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Kui soovite tellimuse tühistada, käivitage järgmine käsk:

        # sudo subscription-manager unregister

13. Käivitage järgmised käsud deprovision virtuaalse masina ja selle ettevalmistamine ettevalmistamise Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Sulgeda VM ja VMDK faili teisendamine VHD vorming.

    Esmalt pildi teisendamine vormingusse:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Veenduge, et töötlemata pilt on joondatud 1 MB. Muul juhul ümardamine joondamiseks 1 MB suurus:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Teisendage kindlaksmääratud suurusega VHD töötlemata ketas.

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Ettevalmistused on punane rolli-põhise arvutiga ISO automaatselt kickstart-faili abil


### <a id="rhel7xkickstart"> </a>RHEL 7.1/7.2 virtuaalse masina kickstart faili ettevalmistamine###


1.  Kickstart faili loomiseks alltoodud sisu ja salvestage fail. Lisateavet kickstart installimise kohta leiate teemast [Kickstart installi juhend](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Viige kickstart faili kohas, kus on juurdepääsetav süsteemist installi.

3.  Looge uus VM Hyper-V haldur. Klõpsake lehel **ühenduse virtuaalse kõvaketta** valige **Manusta hiljem virtuaalse kõvaketta**ja uue virtuaalse masina viisardi lõpuleviimine.

4.  Avage VM sätted.

    lisamine.  Lisada uue virtuaalse kõvaketta VM. Veenduge, et valida **VHD vorming** ja **Kindlaksmääratud suurusega**.

    b.  Kinnitage installimise ISO DVD-kettale.

    c.  Määrake BIOS kettalt CD-le.

5.  Käivitage VM. Installi juhend kuvamisel vajutage **menüü** suvandid buutimine konfigureerimiseks.

6.  Enter `inst.ks=<the location of the kickstart file>` lõpus buutimine suvandid, seejärel klahvi **Enter**.

7.  Oodata installimise lõpuni. Kui olete valmis, VM olema sulgub automaatselt. Oma Linux VHD on nüüd valmis Azure'i üles laadida.

## <a name="known-issues"></a>Teadaolevad probleemid
On teadaolevad probleemid, kui kasutate RHEL 7.1 Hyper-V ja Azure.

### <a name="disk-io-freeze"></a>Ketta I/O külmutamine

See probleem võib ilmneda juhul sagedased salvestusruumi ketta I/O tegevuste RHEL 7.1 Hyper-V ja Azure ajal.   

Repro määr:

See probleem on katkendlik. Siiski see juhtub veel sageli ajal sagedased Hyper-V ja Azure vaba/v-toimingud.   


[AZURE.NOTE] Punane rolli kõrvaldab juba teadaolev probleem. Seotud parandused installimiseks käivitage järgmine käsk:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V draiver võib ei kaasata algse RAM-i kettal Hyper-V hypervisor kasutamisel

Mõnel juhul võivad Linux paigaldajad Hyper-V draiverid algse RAM ketas (initrd või initramfs) juhul, kui tuvastab, et see töötab Hyper-V keskkonnas.

Erinevate virtualization süsteemi (st Virtualbox ja Xen jne) kasutamisel ette valmistada Linux pilt peate initrd tagamaks, et taastada hv_vmbus ja hv_storvsc tuum moodulid on saadaval algse RAM-i kettal. See on teadaolev probleem vähemalt operatsioonisüsteemides varustava punane rolli jaotuse põhjal.

Selle probleemi lahendamiseks peate lisada initramfs Hyper-V moodulid ja taastada selle:

Redigeeri `/etc/dracut.conf` ja lisage sisu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Taastada initramfs:

        # dracut –f -v

Täpsema teabe saamiseks vaadake teave [initramfs taastamine](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Järgmised sammud
Nüüd olete valmis punane rolli Enterprise Linux virtuaalse kõvaketta abil saate luua uue virtuaalmasinates Azure. Kui see on esimene kord, et üleslaaditava faili .vhd Azure, lugege juhiseid 2 – 3 [loomine](virtual-machines-linux-classic-create-upload-vhd.md)ja üleslaadimise virtuaalse kõvaketta, mis sisaldab Linux operatsioonisüsteemi.

Mis on serditud Red rolli Enterprise Linux käivitamiseks hypervisors kohta leiate lisateavet teemast [punane rolli veebisaidi](https://access.redhat.com/certified-hypervisors).
