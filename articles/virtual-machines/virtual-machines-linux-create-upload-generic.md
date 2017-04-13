<properties
    pageTitle="Luua ja üles laadida Linux VHD Azure"
    description="Siit saate teada, luua ja üles laadida ka Azure virtuaalse kõvaketta (VHD) sisaldava Linux operatsioonisüsteemi."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Teave-kinnitatud üldiste müügijaotustega tutvumine #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Tähtis**: The Azure'i platvormi SLA kehtib virtuaalmasinates Linuxi opsüsteemi ainult siis, kui üks [kinnitatud jaotuse](virtual-machines-linux-endorsed-distros.md) kasutatakse. Kõik Linuxi, mis on toodud Azure Pildigalerii on kinnitatud jaotuse nõutav konfiguratsioon.

- [Azure - Linux kinnitatud üldiste müügijaotustega tutvumine](virtual-machines-linux-endorsed-distros.md)
- [Microsoft Azure'i Linux pildid tugi](https://support.microsoft.com/kb/2941892)

Kõik jaotuse töötavate Azure'i tuleb vastavad arvu eeltingimused on võimalus platvormi õigesti käivitamiseks.  See artikkel ei ole täielik nagu iga jaotuse erineb; ja see on täiesti võimalik, et ka siis, kui te ei vasta kõigile kriteeriumidele, mis on allpool te vajate ikkagi saate märkimisväärselt oma Linuxi tagamaks, et see töötab õigesti platvormi.

On põhjus, miks soovitame alustada ühte meie [Linuxi Azure'i kinnitatud jaotuse](virtual-machines-linux-endorsed-distros.md) kui võimalik. Järgmised artiklid juhendab teid kuidas valmistada erinevate kinnitatud Linuxi, mis on toetatud Azure:

- **[CentOS vastavalt üldiste müügijaotustega tutvumine](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle'i Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Punane rolli Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES ja openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Ülejäänud see artikkel keskendub töötab teie Linux jaotuse Azure'i üldised juhised.


## <a name="general-linux-installation-notes"></a>Üldine Linux Installimärkmed ##

- VHDX vorming ei toeta Azure, ainult **kindla VHD**.  VHD vorming Hyper-V halduri või Teisenda-vhd cmdlet-käsu abil saate teisendada ketas. Kui kasutate VirtualBox tähendab see, valides **kindlaksmääratud suurusega** asemel dünaamiliselt eraldatud ketas loomisel vaikimisi.

- Linuxi süsteemi installimisel on *soovitatav* kasutada standard sektsioonid, mitte LVM (tihti paljude installid vaikesäte). See aitab vältida LVM nimi on vastuolus kloonitud VMs, eriti siis, kui mõni OS ketast on vaja kunagi tuleb lisada teise identse VM tõrkeotsing. [LVM](virtual-machines-linux-configure-lvm.md) või [RAID](virtual-machines-linux-configure-raid.md) võib kasutada andmete kettale.

- Tuum tugi UDF failisüsteemi puhul on nõutav. Azure esimesel käivitamisel on ettevalmistamise konfiguratsiooni edasi Linuxi Külastajate lisatud UDF-vormingus meedia kaudu. Azure'i Linux agent tuleb ühendada oma konfiguratsiooni lugemine ja ettevalmistamise VM UDF-failisüsteemis.

- Linuxi tuum versioonid all 2.6.37 ei toeta NUMA Hyper-v suuremat VM suurusega. Selle probleemi peamiselt mõju eelnev kasutamine vanem jaotuse punane rolli 2.6.32 tuum, ja oli määratud RHEL 6.6 (tuum-2.6.32-504). Opsüsteemi kohandatud tuumad, mis on vanemad kui 2.6.37 või RHEL vastavalt tuumad vanema, kui 2.6.32-504 määrama buutimine parameetri `numa=off` kohta rakenduses grub.conf käsurea tuum. Lisateabe saamiseks vt punane rolli [KB 436883](https://access.redhat.com/solutions/436883).

- Konfigureerige Vaheta sektsiooni OS kettal. Linux agent saab konfigureerida kettal ajutine ressursi Vaheta faili loomiseks.  Lisateavet selle kohta leiate alltoodud juhiseid.

- Kõik selle VHDs peab olema suurused, mis on kordne 1 MB.


### <a name="installing-linux-without-hyper-v"></a>Installimise Linux ilma Hyper-V ###

Mõnel juhul võib hõlmata Linux paigaldajad draiverid Hyper-v algse mäluketas (initrd või initramfs) juhul, kui tuvastab, et see töötab ka Hyper-V keskkonnas.  Erinevate virtualization süsteemi (st Virtualbox, KVM jne) kasutamisel ette valmistada Linux pilt peate initrd tagamaks, et taastada vähemalt `hv_vmbus` ja `hv_storvsc` tuum moodulid on saadaval algse mäluketas.  See on teadaolev probleem vähemalt operatsioonisüsteemides varustava punane rolli jaotuse põhjal.

Süsteemi initrd või initramfs pildi taastamiseks võivad erineda sõltuvalt jaotuse. Vaadake oma jaotuse dokumentatsiooni või kasutajatugi proper toimingut.  Siin on üks näide selle kohta, kuidas taastada initrd abil soovitud `mkinitrd` kasuliku:

Esmalt varundamiseks initrd olemasoleva pildi tehke järgmist.

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Järgmiseks taastada initrd abil soovitud `hv_vmbus` ja `hv_storvsc` tuum moodulid:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>VHDs suuruse muutmine ###

Azure'i VHD pildid peab olema virtuaalse suurus koondatud 1MB.  Tavaliselt VHDs Hyper-V abil loodud juba tuleb joondada õigesti.  Kui selle VHD on joondatud õigesti siis võidakse kuvada tõrketeade, mis sarnaneb järgmise kui proovite luua *pilt* oma VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Parandamiseks saate pildi suurust Hyper-V halduri konsooli või [Suurust-VHD](http://technet.microsoft.com/library/hh848535.aspx) PowerShelli cmdleti VM.  Kui te ei kasuta Windows keskkonnas siis on soovitatav qemu-img abil saate teisendada (vajadusel) ja selle VHD.

> [AZURE.NOTE] Teadaolev viga qemu-img versioonides on > = 2.2.1, mille tulemuseks on valesti vormindatud VHD. Probleem on lahendatud QEMU 2.6. Soovitatav on kasutada qemu-img 2.2.0 või alumises või värskendada, et 2.6 või uuem versioon. Viide: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Suuruse muutmise VHD, otse tööriistadega nagu `qemu-img` või `vbox-manage` võib kaasa tuua mõne unbootable VHD.  Seega on soovitatav esimese Teisenda VHD töötlemata plaat.  Kui VM pilt on juba loodud nimega töötlemata plaat (vaikimisi mõned Hypervisors, nt KVM) võib vahele seda toimingut:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Arvutab nõutud suuruse plaat tagamaks, et virtuaalse suurus on koondatud 1MB.  Järgmised bash shell script aitab see.  Skript kasutab "`qemu-img info`" plaat virtuaalse suuruse ja seejärel arvutab järgmise 1 MB suurus:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Suuruse muutmine töötlemata ketast $rounded_size nagu ülaltoodud skripti:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Teisendage töötlemata ketas naasmiseks fikseeritud suurus VHD.

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Linuxi tuum nõuded ##

Linux Integration Services (Poolelioleva) draiverid Hyper-V ja Azure on otse panuse varustava Linuxi tuum. Mitme jaotuse, mis sisaldavad tehtud Linuxi tuum versiooni (st 3.x) on need saadaval draiverid juba või muul viisil anda need draiverid backported versioonid nende tuumad.  Need draiverid pidevalt uuendatakse rakenduses varustava tuum uusi parandusi ja funktsioone, seega võimaluse korral on soovitatav käivitamiseks on [kinnitatud jaotuse](virtual-machines-linux-endorsed-distros.md) mis sisaldab need parandused ja värskendused.

Kui teil on punane rolli Enterprise Linuxi versioonid **6.0-6.3**variandi, siis kas peate installima Poolelioleva uusimad draiverid Hyper-V. Draiverid leiate [selles asukohas](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL **6.4 +** (ja derivaadid) on juba Poolelioleva draiverid kaasas tuum ja nii pole täiendavad install pakette on vaja need süsteemid Azure käivitamiseks.

Kohandatud tuum on nõutav, on soovitatav kasutada tuum uuemat versiooni (st **3.8 +**). Nende jaotuse või müüjad, kes säilitada oma tuum, mõned vaeva puhul nõutav regulaarselt backport Poolelioleva draiverid varustava tuum oma kohandatud-suhte kaudu.  Isegi juhul, kui teil on juba tuum suhteliselt uuem versioon, on väga soovitatav silma peal hoida mis tahes parandused varustava Poolelioleva draiverid ja backport neid vastavalt vajadusele. Poolelioleva draiver lähtefailide asukoha on saadaval Linuxi tuum allikas puu [SÄILITAJAD](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) fail:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Väga minimaalne järgmised plaastrid puudumisel on teada, et põhjustada probleeme Azure ja nii need sisalduva tuum. Selles loendis ei ole üldine või valmis jaoks kõik üldiste müügijaotustega tutvumine:

- [ata_piix: edasi lükata ketast Hyper-V draiverid vaikimisi](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: konto-teel pakettide tee lähtestamine](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: vältida WRITE_SAME kasutamist](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: keelata kirjutada sama RAID ja virtuaalne host võrguadapteri draivereid](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: NULL kursori dereference parandamine](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: ring puhvri tõrkeid võib kaasa tuua I/O külmutamine](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: kaitsta __scsi_remove_device kahekordsete täitmine](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Azure'i Linux Agent ##

[Azure'i Linux Agent](virtual-machines-linux-agent-user-guide.md) (waagent) on vajalik õigesti ettevalmistamise Azure virtuaalse masina Linux. Saate hankida uusim versioon, faili probleemid või pull taotlusi [Linux Agent GitHub repo](https://github.com/Azure/WALinuxAgent)juures.

- Linux agent välja Apache 2.0 litsentsid. Paljude jaotuse juba ette pööret MINUTIS või deb pakettide agent ja seega mõnel juhul see saab installitud ja värskendada vähese vaevaga.

- Azure'i Linux Agent nõuab Python v2.6 +.

- Agent on vaja ka python-pyasn1 moodulit. Enamikus pakkuda seda eraldi paketina, mille saate installida.

- Mõnel juhul ei pruugi Azure Linux Agent NetworkManager kooskõlas. Paljud pööret MINUTIS/Deb paketid, mis on esitatud jaotuse konfigureerida NetworkManager konflikt waagent pakkida ja seega on desinstallimine NetworkManager Linux agent paketi installimisel.


## <a name="general-linux-system-requirements"></a>Üldine Linux süsteeminõuded ##

- Muutke tuum buutimine rea GRUB või GRUB2 sisaldavad järgmisi. See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    See tagab ka kõik konsooli sõnumid saadetakse esimese järjestikune port, mis võib aidata Azure'i tugi silumine probleemid.

    Lisaks on soovitatav *eemaldada* järgmiste parameetrite korral:

        rhgb quiet crashkernel=auto

    Graafiline ja vaiksesse buutimine pole kasu cloud keskkonnas, kus soovime kõik logid järjestikune pordi saatmiseks.

    Funktsiooni `crashkernel` suvand võib olla konfigureeritud soovi vasakule, kuid teate, et see parameeter vähendab saadaval mälu VM 128MB või rohkem, mis võib olla väiksemate VM suurused probleemne.

- Azure'i Linux Agent installimine

    Azure'i Linux Agent on nõutav Azure Linux pildi ettevalmistamine.  Mitme jaotuse pakuvad agent dokumendina pööret MINUTIS või Deb (paketi tavaliselt nimetatakse "WALinuxAgent" või "walinuxagent").  Agent saab installida ka käsitsi [Linux Agent juhendi](virtual-machines-linux-agent-user-guide.md)juhiseid järgides.

- Veenduge, et SSH server on installitud ja konfigureeritud käivituma käivitamisel.  See on tavaliselt vaikimisi.

- Luua Vaheta ruumi kettal OS

    Azure'i Linux Agent automaatselt saate konfigureerida kasutada kohaliku ressursi ketas, mis on lisatud pärast ettevalmistamise Azure VM Vaheta ruumi. Pange tähele, et kohaliku ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud. Pärast installimist Azure Linuxi Agent (vt eelmises etapis), muuta õigesti /etc/waagent.conf järgmised parameetrid:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Käivitage viimase sammuna deprovision virtuaalse masina järgmised käsud:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Klõpsake Virtualbox võidakse kuvada järgmine tõrketeade jooksutamise ' waagent-jõustada - deprovision': `[Errno 5] Input/output error`. See tõrketeade kuvatakse äärmiselt oluline on ja seda võib eirata.

- Seejärel peate virtual arvuti sulgeda ja Azure on VHD üleslaadimiseks.
