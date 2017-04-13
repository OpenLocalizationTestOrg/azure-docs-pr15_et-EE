<properties 
    pageTitle="Konfigureerige LVM töötab Linux | Microsoft Azure'i" 
    description="Saate teada, kuidas konfigureerida LVM Azure Linux." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Linux VM Azure LVM konfigureerimine

Selle dokumendi arutada, kuidas konfigureerida Azure virtuaalne arvuti loogilise helitugevuse Manager (LVM). Ajal, siis on võimalik konfigureerimine LVM manustatud virtuaalse masina mis tahes kettal, vaikimisi enamik pilve pilte ei ole LVM kettal OS konfigureeritud. See on dubleeritud helitugevuse rühmade tõrgete vältimiseks, kui ketas OS kunagi seotud teise VM jaotuse ja sama tüüpi, st taastamine stsenaariumi. Seetõttu on soovitatav ainult LVM kasutamine andmete kettale.


## <a name="linear-vs-striped-logical-volumes"></a>Lineaarne vs musta Triibutatud loogiline mahud

LVM saab ühendada ühe mälu mahu arvu füüsiliste. Vaikimisi tavaliselt loob LVM lineaarse loogiline mahud, mis tähendab, et füüsilise säilitamise ühendatakse koos. Sel juhul lugemis-ja kirjutamisõigusega toimingud tavaliselt ainult saadetakse ühe kettale. Aga me saame luua musta Triibutatud loogiline mahud, kus loeb ja kirjutab levitatakse mitme ketast sisalduvate helitugevuse rühma (st RAID0 sarnane). Jõudluse tagamiseks on tõenäoline, mida soovite oma loogiline mahud triip nii, et loeb ja kirjutab kasutada kõik lisatud andmed ketast.

Selle dokumendi kirjeldab, kuidas ühendada mitu andmete ketast ühte rühma ja seejärel looge musta Triibutatud loogilise helitugevust. Alltoodud juhiseid on mõnevõrra üldiste enamikus töötamiseks. Enamikul juhtudel Utiliidid ja töövoogude haldamise LVM Azure ei ole põhimõtteliselt erinevas muude keskkonnas. Nagu tavaliselt, ka oma Linux tarnija saamiseks lugege dokumentatsiooni ja oma kindla jaotuse LVM kasutamise head tavad.


## <a name="attaching-data-disks"></a>Seotud andmete ketast
Üks tavaliselt soovite alustada tühja andmete kahe või enama ketast LVM kasutamisel. Vastavalt oma vajadustele IO, saate manustada ketast, mis on talletatud meie standardne salvestusruumi kuni 500 IO/PS kettale või meie Premium salvestusruumi kuni 5000 IO/PS ketas. See artikkel ei lähe üksikasjalikult kohta, kuidas ette valmistada ja Linux virtuaalse masina andmete ketast manustamiseks. Lugege selle Microsoft Azure'i artiklis [manustada kettal](virtual-machines-linux-add-disk.md) üksikasjalikud juhised kohta, kuidas manustada mõnda tühja andmed kettale Linux virtuaalse masina Azure.

## <a name="install-the-lvm-utilities"></a>Installige LVM Utiliidid

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS ja Oracle'i Linux**

        # sudo yum install lvm2

- **SLES 12 ja openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Klõpsake SLES11 peab ka redigeerida /etc/sysconfig/lvm ja seadke `LVM_ACTIVATED_ON_DISCOVERED` "lubada":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>LVM konfigureerimine
Sellest juhendist loodame, et teil on kinnitatud kolme andmete ketast, mida me viidata nimega `/dev/sdc`, `/dev/sdd` ja `/dev/sde`. Pange tähele, et need võivad olla alati sama tee nimed oma VM. Saate käivitada '`sudo fdisk -l`' või sarnane käsk oma saadaval ketast.

1. Ettevalmistused füüsilise mahud:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Helitugevuse rühma loomine. Selles näites me kutsume soovitud maht jaotises "andmed-vg01":

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Saate luua loogika draivid. Oleme all käsk loob ühe loogilise draivi nimega "andmed-lv01" üle kogu rühma, kuid Pange tähele, et see on võimalik luua mitu loogilise draivi maht jaotises.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Loogika helitugevuse vormindamine

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] SLES11 kasutamisel "-t ext3" asemel ext4. SLES11 toetab ainult kirjutuskaitstud juurdepääsu ext4 failisüsteemi.


## <a name="add-the-new-file-system-to-etcfstab"></a>Uue faili süsteemi lisamine /etc/fstab

**Ettevaatlik:** Valesti /etc/fstab faili redigeerimise võib põhjustada unbootable süsteemi. Kui te pole kindel, vaadake selle jaotuse dokumentatsiooni Lisateavet õigesti selle faili redigeerimine. Samuti on soovitatav enne redigeerimist on loodud /etc/fstab failist varukoopia.

1. Loo uus failisüsteemi jaoks soovitud ühenduspunkti näiteks:

        # sudo mkdir /data


2. Leidke loogiline helitugevuse tee

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Avage tekstiredaktoris /etc/fstab ja lisada kirjet uue failisüsteemi, näiteks:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Seejärel salvestage ja sulgege /etc/fstab.


4. Testida, et/jne/fstab kirje on õige:

        # sudo mount -a

    Kui see käsk on tulemuseks tõrketeate kontrollige süntaksi/jne/fstab faili.

    Järgmine käivitada soovitud `mount` tagamiseks on ühendatud failisüsteemi käsk:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Valikuline) Failsafe buutimine parameetrite /etc/fstab

    Sisaldab palju jaotuse, kas selle `nobootwait` või `nofail` mount/jne/fstab faili lisatud parameetrid. Järgmiste parameetrite lubamine tõrkeid, kui tulevad kindla failisüsteemi ja luba Linuxi jätkata ka siis, kui seda ei saa õigesti ühendada RAID failisüsteemi käivitamiseks. Vaadake oma jaotuse dokumentatsiooni järgmiste parameetrite kohta lisateabe saamiseks.

    Näide (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
