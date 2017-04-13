<properties
    pageTitle="Lisada kettal Linux VM | Microsoft Azure'i"
    description="Saate teada, kuidas lisada oma Linux VM püsivate kettal"
    keywords="Linux virtuaalse masina, ressursi ketta lisamine"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Linux VM kettal lisamine

Selles artiklis kirjeldatakse, kuidas manustada püsivate kettal oma VM nii, et saate säilitada andmete – ka siis, kui teie VM on reprovisioned vahetamisest või suuruse muutmine. Kettal lisamiseks peate [Azure'i CLI](../xplat-cli-install.md) konfigureeritud ressursihaldur režiimis (`azure config mode arm`).  

## <a name="quick-commands"></a>Kiirülevaate käsud

Järgmistes näidetes käsu, asendage väärtusi vahemikus &lt; ja &gt; oma keskkonnast väärtustega.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Manusta kettal

Uue ketta manustamise on kiire. Tippige `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` luua ja lisada uue GB vaba oma VM jaoks. Kui te just teiega tuvastada salvestusruumi konto, mis tahes ketta loomist paigutatakse sama konto salvestusruumi kõvakettal OS asukoht.  See peaks välja nägema umbes järgmine:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Väljund

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Linux VM ühendada uue ketta ühendamine

> [AZURE.NOTE] Selles teemas loob VM abil kasutajanimed ja paroolid. Avalike ja privaatvõ võtme paari abil saate suhelda oma VM, vaadake, [Kuidas kasutada SSH Azure Linux](virtual-machines-linux-mac-create-ssh-keys.md). Saate muuta **SSH** Ühenduvus VMs loodud on `azure vm quick-create` käsu abil soovitud `azure vm reset-access` käsk Lähtesta **SSH** juurdepääs täielikult, lisada või kasutajate eemaldamine või turvaline juurdepääs avaliku võtme faile lisada.

Teil on vaja sisse oma Azure VM sektsiooni SSH, vormindamine ja mount oma uue ketta, saate oma Linux VM. Kui olete tuttav ühendavad **ssh**, käsk vormis `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, ja näeb välja umbes järgmine:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Väljund

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Nüüd, kui olete ühendatud teie VM, olete valmis manustamiseks ketas.  Leidke ketas, kasutades `dmesg | grep SCSI` (meetodi abil saate tuvastada teie uue ketta võivad olla erinevad). Sel juhul näeb umbes kujul:

```bash
dmesg | grep SCSI
```

Väljund

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

selles teemas puhul on `sdc` ketas on üks, mis soovime. Nüüd partition kettale `sudo fdisk /dev/sdc` --eeldades, et teie puhul on ketas `sdc`, ja muutke see peamine kettal partition 1 ja Aktsepteeri muude vaikesätted.

```bash
sudo fdisk /dev/sdc
```

Väljund

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Luua sektsiooni, tippides `p` kuvatakse vastav viip:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Ja failisüsteemi kirjutada sektsiooni **mkfs** käsu, täpsustades oma failisüsteemi tüüp ja seadme nime abil. Selles teemas kirjutit `ext4` ja `/dev/sdc1` ülevalt:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Väljund

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Nüüd saame luua faili süsteemi abil ühendada `mkdir`:

```bash
sudo mkdir /datadrive
```

Ja te directory abil `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Andmed kettale on kasutamiseks valmis `/datadrive`.

```bash
ls
```

Väljund

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Ketas on remounted automaatselt pärast uuesti tagamiseks peab lisatakse/jne/fstab faili. Lisaks on soovitatav, et UUID (üldiselt ainuidentifikaator) kasutatakse/jne/fstab viidata ketas, mitte ainult seadme nimi (näiteks `/dev/sdc1`). Kui OS tuvastab ketta tõrke ajal buutimine, kasutades funktsiooni UUID aitab vältida vale paigaldatud teatud asukohta ketas. Allesjäänud andmete ketast oleks siis määratud nende sama seadme ID-d. Uue draivi UUID otsimiseks saate kasutada **blkid** kasuliku:

```bash
sudo -i blkid
```

Väljund näeb välja järgmine:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Valesti **/etc/fstab** faili redigeerimise võib põhjustada unbootable süsteemi. Kui te pole kindel, vaadake selle jaotuse dokumentatsiooni kohta teavet õigesti selle faili redigeerimiseks. Samuti on soovitatav enne redigeerimist on loodud /etc/fstab failist varukoopia.

Järgmiseks avage tekstiredaktoris **/etc/fstab** fail:

```bash
sudo vi /etc/fstab
```

Selles näites me kasutame UUID väärtus uute eelmisi juhiseid ja mountpoint **/datadrive**loodud **/dev/sdc1** seadme. Lisage järgmine rida **/etc/fstab** faili lõppu.

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Hiljem eemaldamine andmed kettale ilma redigeerimise fstab võib põhjustada VM nurjumise käivitamiseks. Enamikus esitama kas selle `nofail` ja/või `nobootwait` fstab suvandid. Nende suvandite abil käivitada ka siis, kui ketas nurjub ühendada käivitamisel süsteem. Järgmiste parameetrite kohta lisateabe saamiseks lugege oma jaotuse dokumentatsiooni.

>[AZURE.NOTE] Suvandi **nofail** tagab, et VM käivitatakse juhul, kui failisüsteemi on vigane või ketas pole käivitamisel. Ilma see suvand, mis võivad ilmneda käitumine nagu on kirjeldatud [Ei saa SSH Linux VM FSTAB tõrgete tõttu](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>Funktsiooni TRIM ja UNMAP Linux Azure tugi
Mõned Linux tuumad toeta TRIM ja UNMAP toimingute hülgamiseks kasutamata plokid kettal. See on kasulik peamiselt standard salvestusruumi Azure'i kustutatud lehtede ei kehti enam ja saab hüljata. See saate salvestada maksumus, kui loote suuri faile ja seejärel kustutage need.

On kaks võimalust lubada TRIM tugi oma Linux VM. Nagu tavaliselt, küsige oma jaotuse soovitatav:

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

- Pidage meeles, et teie uue ketta ei saa VM, kui ta taaskäivitamisega juhul, kui seda teavet kirjutamise [fstab](http://en.wikipedia.org/wiki/Fstab) faili.
- Tagada teie Linux VM on õigesti konfigureeritud, lugege läbi [optimeerimine Linux arvuti jõudluse](virtual-machines-linux-optimization.md) soovitusi.
- Laiendage teie mälumahuga täiendavad ketast ja täiendavad jõudluse [konfigureerimine RAID](virtual-machines-linux-configure-raid.md) lisamisega.
