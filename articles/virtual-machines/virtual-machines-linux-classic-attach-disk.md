<properties
    pageTitle="Manustamine kettal Linux VM | Microsoft Azure'i"
    description="Saate teada, kuidas manustada andmed kettale Azure virtuaalse masina, mis töötab Linux ja lähtestage see, et see on kasutamiseks valmis."
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
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Kuidas lisada andmed kettale Linux virtuaalse masina

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lugege teemat Kuidas [lisada andmed kettal ressursihaldur juurutamise mudeli abil](virtual-machines-linux-add-disk.md).

Saate lisada tühja ketast nii ketast, mis sisaldavad andmeid oma Azure vms. Mõlemat tüüpi ketast on .vhd faile, mis asu Azure storage konto. Koos Linux kohapeal, mis tahes ketta lisamine pärast manustate ketas peate käivitada ja seda vormindada nii, et see on kasutamiseks valmis. See artikkel üksikasjad, lisades juba sisaldavad andmeid, et teie VMs ning seejärel lähtestamine ja vormindamine uue ketta ketast nii tühja ketast.

> [AZURE.NOTE]See on parim kasutada üks või mitu erinevat ketast virtuaalse masina andmete talletamiseks. Azure'i virtuaalarvuti loomisel on on operatsioonisüsteem kettale ja ajutise kettaruumi. **Ärge kasutage ajutist ketas püsivate andmete talletamiseks.** Nagu nimi viitab, pakub ainult ajutine salvestusruum. Kuna see ei asu Azure storage pakub pole liigsed või varundamine.
> Ajutiste ketas on tavaliselt haldab Azure Linux Agent ja automaatselt paigaldatud **/mnt/resource** (või **/mnt/mnt** Ubuntu pildid). Teisalt, andmed kettale võib nimi Linux salvestab midagi `/dev/sdc`, ja soovite partition, vormindamine ja mount ressurss. Leiate [Azure'i Linux Agent kasutusjuhendi] [ Agent] üksikasju.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Uute andmete ketta Linux lähtestamine

1. SSH oma VM. Lisateabe saamiseks vaadake, [Kuidas töötab Linuxi sisselogimiseks][Logon].

2. Järgmiseks tuleb leida seadme tunnuse andmed kettale lähtestada. On kaks võimalust seda teha.

    a) Grep SCSI seadmeid logid, nagu näiteks järgmine käsk:

            $sudo grep SCSI /var/log/messages

    Tehtud Ubuntu jaotuse, peate kasutama `sudo grep SCSI /var/log/syslog` Kuna logida `/var/log/messages` võivad olla keelatud vaikimisi.

    Saate otsida identifikaator viimase andmeid ketas, mis lisati sõnumid, mis on kuvatud.

    ![Ketas sõnumite toomine](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    VÕI

    (b) kasutamine on `lsscsi` käsk välja selgitada seadme ID-d. `lsscsi`Kas saab installida `yum install lsscsi` (punane rolli põhineb jaotuse) või `apt-get install lsscsi` (Debian vastavalt üldiste müügijaotustega tutvumine). Saate otsida ketas otsite _lun_ või **loogilise üksuse number**. Näiteks _lun_ ketast, saate manustatud hõlpsasti näha `azure vm disk list <virtual-machine>` nimega:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Võrdle neid andmeid väljund `lsscsi` jaoks sama Kuulake virtuaalse masina:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Korteeži igas reas viimane number on _lun_. Vt `man lsscsi` lisateabe saamiseks.

3. Vastava viiba kuvamisel tippige järgmine käsk seadme loomiseks:

        $sudo fdisk /dev/sdc


4. Vastava viiba kuvamisel tippige **n** , luuakse uus sektsioon.


    ![Seadme loomine](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Vastava viiba kuvamisel tippige **p** esmane sektsioon sektsiooni teha. Tippige **1** teha esimese sektsiooni ja sisestage kinnitamiseks silinder vaikeväärtuse tüüp. Mõnes süsteemis, saate selle kuvada esimese ja viimase sektoritele asemel silinder vaikeväärtused. Kui soovite, et aktsepteerida nende vaikesätted.


    ![Sektsiooni loomine](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Tippige **p** ketas, mis on on liigendatud üksikasjade kuvamiseks.


    ![Loendi ketas teave](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Tippige **w** kirjutada ketta sätteid.


    ![Kirjutada ketta muudatused](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Nüüd saate luua uue sektsiooni failisüsteemis. Lisa sektsiooni number seadme ID (järgmises näites `/dev/sdc1`). Järgmises näites luuakse mõne ext4 partition /dev/sdc1 kohta:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Failisüsteemi loomine](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] SuSE Linux Enterprise 11 süsteemide ainult toetus lugemisõiguse ext4. Need süsteemid on soovitatav vormindamiseks uue failisüsteemi ext3 ext4 asemel.


9. Tehke kataloog ühendada uue faili süsteemi järgmiselt:

        # sudo mkdir /datadrive


10. Samuti saate ühendate ketas, järgmiselt:

        # sudo mount /dev/sdc1 /datadrive

    Andmed kettale on nüüd **/datadrive**kasutamiseks valmis.
    
    ![Kausta luua ja mount ketas](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Lisage uus draiv /etc/fstab.

    Ketas on remounted automaatselt pärast uuesti tagamiseks peab lisatakse/jne/fstab faili. Lisaks on soovitatav, et UUID (üldiselt ainuidentifikaator) kasutatakse /etc/fstab viidata ketas, mitte ainult seadme nime (nt /dev/sdc1). Abil soovitud UUID aitab vältida valede ketas paigaldatud teatud asukohta, kui OS tuvastab ketta tõrke buutimine ja kõik ülejäänud andmed ketast, siis määratud nende seadme ID-d. Uue draivi UUID otsimiseks saate kasutada **blkid** kasuliku:

        # sudo -i blkid

    Väljund näeb välja järgmine:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Valesti **/etc/fstab** faili redigeerimise võib põhjustada unbootable süsteemi. Kui te pole kindel, vaadake selle jaotuse dokumentatsiooni kohta teavet õigesti selle faili redigeerimiseks. Samuti on soovitatav enne redigeerimist on loodud /etc/fstab failist varukoopia.

    Järgmiseks avage tekstiredaktoris **/etc/fstab** fail:

        # sudo vi /etc/fstab

    Selles näites me kasutame UUID väärtus uute eelmisi juhiseid ja mountpoint **/datadrive**loodud **/dev/sdc1** seadme. Lisage järgmine rida **/etc/fstab** faili lõppu.

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Või operatsioonisüsteemides põhjal SuSE Linux võib-olla peate kasutama veidi muus vormingus:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] Funktsiooni `nofail` suvand tagab, et VM käivitatakse juhul, kui failisüsteemi on vigane või ketas pole käivitamisel. Ilma seda võimalust, võivad ilmneda käitumine, nagu on kirjeldatud [Ei saa SSH Linux VM FSTAB tõrgete tõttu](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Nüüd saate testida failisüsteemi õigesti ühendatud unmounting ja seejärel remounting failisüsteemi, st Käesolevas näites ühenduspunkti `/datadrive` varasemas etapis loodud:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Kui soovitud `mount` käsk annab tulemiks vea, kontrollige/jne/fstab faili õige süntaksi. Kui loodud täiendavate andmete ketast või sektsiooni, / jne/fstab eraldi kui ka sisestamine.

    Tehke selle käsu abil kirjutatav ketas:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Seejärel eemaldamise andmed kettale ilma redigeerimise fstab võib põhjustada VM nurjumise käivitamiseks. Kui see on levinud, enamikus pakkuda, kas selle `nofail` ja/või `nobootwait` fstab Valikud, mis süsteemi käivitada ka siis, kui ketas nurjub ühendada veebisaidil käivitada aega. Järgmiste parameetrite kohta lisateabe saamiseks lugege oma jaotuse dokumentatsiooni.

### <a name="trimunmap-support-for-linux-in-azure"></a>Funktsiooni TRIM ja UNMAP Linux Azure tugi
Mõned Linux tuumad toeta TRIM ja UNMAP toimingute hülgamiseks kasutamata plokid kettal. Need toimingud on kasulik peamiselt standard salvestusruumi Azure'i kustutatud lehtede ei kehti enam ja saab hüljata. Lehtede hüljates saate salvestada maksumus, kui loote suuri faile ja seejärel kustutage need.

On kaks võimalust lubada TRIM toetamiseks oma Linux VM. Nagu tavaliselt, küsige oma jaotuse soovitatav:

- Kasutage funktsiooni `discard` mount suvand `/etc/fstab`, näiteks:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Teise võimalusena võite käivitada soovitud `fstrim` käsk käsitsi käsurea kaudu või lisada selle oma crontab käivitumise:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Tõrkeotsing
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Järgmised sammud
Lugege järgmisi artikleid oma Linux VM kasutamise kohta:

- [Kuidas töötab Linux sisse logida][Logon]

- [Kuidas eemaldada arvutist Linux virtual kettal](virtual-machines-linux-classic-detach-disk.md)

- [Azure'i CLI mudeliga klassikaline juurutamise abil](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
