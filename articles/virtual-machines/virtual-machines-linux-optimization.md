<properties
    pageTitle="Optimeerida oma Linux VM Azure | Microsoft Azure'i"
    description="Siit saate teada, veendumaks, et olete häälestanud oma Linux VM optimaalse jõudluse tagamiseks Azure näpunäiteid optimeerimine"
    keywords="Linux virtuaalse masina, virtuaalse masina linux, ubuntu virtuaalse masina" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Oma Linux VM Azure optimeerimine

Luua Linux virtuaalse masina (VM) on lihtne teha käsurea kaudu või portaalist. Selles õpetuses näitab teile, kuidas tagada, et olete häälestanud optimeerida oma Microsoft Azure'i platvormil jõudlust. Selles teemas kasutab mõnda Ubuntu Server VM, kuid saate luua ka Linuxi virtuaalse masina [oma piltidega malle](virtual-machines-linux-create-upload-generic.md).  

## <a name="prerequisites"></a>Eeltingimused

See teema eeldab, et teil juba on töö Azure tellimuse ([tasuta prooviversiooni kasutajaks registreerumist](https://azure.microsoft.com/pricing/free-trial/)) [installitud Azure'i CLI](../xplat-cli-install.md) ja on juba ettevalmistatud VM Azure tellimuse. Enne teete midagi koos Azure - peate tellimuse autentimiseks. Selleks Azure'i CLI, tippige lihtsalt `azure login` interaktiivse alustamiseks. 

## <a name="azure-os-disk"></a>Azure'i OS kettale

Kui loote Linux Vm Azure'is, on kaks plaati seotud. /dev/SDA on teie OS kõvakettale, SDB oma ajutist ketas.  Kasutage peamine OS ketas (/ arendaja/sda) midagi peale operatsioonisüsteem on optimeeritud kiiresti VM buutimine aja ja esitada oma töökoormus head jõudlust. Soovite manustada üks või enam ketast oma VM saada püsiv ja mäluruumi teie andmete jaoks optimeeritud. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Lisades ketast maht ja jõudlus 

Põhineb VM suurus, saate lisada kuni 16 täiendavad ketast on A seeria, D – rea 32 ketast ja G-rea 64 ketast masina - iga kuni 1 TB suurus. Saate lisada täiendavat ketast vastavalt vajadusele oma ruumi ja IOps nõuete kohta. Iga ketta on 500 IOps jõudluse eesmärgiks kindlad ja kuni 5000 IOps kohta kettale Premium mälu.  Premium salvestusruumi ketast kohta lisateabe saamiseks vaadake [Premium mälu: Azure'i vms suure jõudlusega salvestusruumi](../storage/storage-premium-storage.md)

Saavutada kõrgeim IOps Premium salvestusruumi kettale, kuhu oma vahemälu sätted on seatud "Kirjutuskaitstud" või "Pole", kui keelate "takistused" ajal paigutusega faili süsteem Linux. Takistused pole vaja, kuna selle kirjutab Premium salvestusruumi tagatud ketast on püsival vahemälu nende sätete jaoks.

- **ReiserFS**kasutamisel keelamine takistused mount suvandiga "takistuseks = pole" (lubamine takistused, kasutage "takistuseks tühjendamise =")
- Kui kasutate **ext3/ext4**, keelamine takistused mount suvandiga "takistuseks = 0" (lubamine takistused, kasutage "takistuseks = 1")
- Kui kasutate **XFS**, keelamine takistused mount suvandiga "nobarrier" (takistused, mis võimaldab, kasutage suvandit "takistuseks")

## <a name="storage-account-considerations"></a>Salvestusruumi konto peaksite arvesse võtma

Kui loote oma Linux VM Azure, veenduge, manustate ketast salvestusruumi kontodelt elavate samas piirkonnas oma VM tagada vahetus ja võrgu latentsus.  Iga kindlad konto on kuni 20 k IOps ja 500 TB suurus maht.  See töötab välja ligikaudu 40 tugevalt kasutatud ketast, sh nii OS kettale ja mis tahes andmete ketast, saate luua. Salvestusruumi Premium konto korral ei ole lubatud IOps piiratud, kuid on 32 TB suurusepiirangu. 

Kui kõrge IOps töökoormus ja valinud kindlad oma ketast, peate tükeldamiseks on ketast üle mitme salvestusruumi kontod veendumaks, et klõpsamist on 20 000 IOps limiit kindlad kontod. Teie VM võib sisaldada kombinatsiooni ketast kaudu üle erinevate salvestusruumi kontode ja salvestusruumi kontotüüpide optimaalse konfiguratsioonist saavutamiseks. 

## <a name="your-vm-temporary-drive"></a>Oma ajutist VM ketas

Vaikimisi VM, loomisel Azure'i annab teile mõne OS ketas (/ arendaja/sda) ja ajutine kettal (/ arendaja/sdb).  Kõik täiendavad ketast, saate lisada ilmub /dev/sdc, /dev/sdd, /dev/sde jne. Kõik andmed ajutiste kõvakettal (/ arendaja/sdb) pole püsiv ja saate kaotsi minna, kui teatud sündmuste VM suurust, positsioonide, või hooldustööd jõustab uuesti oma VM.  Suurus ja tippige oma ajutist kettale on seotud teie valitud juurutamise ajal VM suuruse. Mis tahes suurusega premium VMs (DS, G ja DS_V2 sarja) puhul ajutine ketas toetavad kohaliku SSD täiendavad jõudluse kuni 48 k IOps. 

## <a name="linux-swap-file"></a>Faili Linux vahetamine

Kui teie Azure VM on Ubuntu või CoreOS pilt, saate CustomData pilveteenuses – käivitamise cloud-config saatmiseks kasutada. Kui saate [üles laadida pildi kohandatud Linux](virtual-machines-linux-upload-vhd.md) pilveteenuses – käivitamise kasutav, konfigureerida Vaheta sektsioonid pilveteenuses – käivitamise abil.

Ubuntu Cloud pilte, peate kasutama pilveteenuses – käivitamise Vaheta partition konfigureerimiseks. Järgmisel leheküljel näidata Ubuntu viki lähemalt: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Pilveteenuse – käivitamise toetuseta pilte, on VM piltide juurutatud Azure'i turuplatsilt VM Linux Agent integreeritud OS. See agent lubab suhelda mitmesuguste Azure VM. Eeldades, et olete juurutanud standard pildi Azure'i turuplatsilt, peate tehke Linux Vaheta faili sätete konfigureerimiseks järgmist:

Otsimine ja muutmine kaks kirjet failis **/etc/waagent.conf** . Nad kontrolli sihtotstarbeline Vaheta faili olemasolu ja Vaheta faili maht. Soovite muuta parameetrid on `ResourceDisk.EnableSwap=N` ja`ResourceDisk.SwapSizeMB=0` 

Peate muuta järgmist:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size MB teie vajadustele} 

Kui olete teinud muutmine, peate selle waagent taaskäivitage või oma Linux VM taguse.  Teate, et muudatused on rakendatud ja Vaheta faili on loodud, kui kasutate funktsiooni `free` käsk vaba ruumi vaatamiseks. Järgmises näites on 512MB Vaheta faili waagent.conf faili muutmise tulemusena loodud.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/O ajastamise algoritmi Premium Storage

Koos on 2.6.18 Linux tuum ja ajastamise algoritmi I/O muudeti tähtaja CFQ (täielikult ja tehnikamessi andmebaasitõrge algoritmi) vaikimisi. Muutmälu I/O mustrid, on väikese vahe jõudluse erinevused CFQ ja tähtaja vahel.  SSD-põhiste ketast, kui ketta I/O mustri on valdavalt järjestikku, tagasi minna NOOP või tähtaeg algoritmi võite saavutada I/O töökindluse.

### <a name="view-the-current-io-scheduler"></a>Praeguse I/O ajasti kuvamine

Kasutage järgmine käsk:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Kuvatakse väljund, mis näitab praegust ajasti pärast.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Muutke praegust seadet (/ arendaja/sda) sisend algoritmi plaanimine

Saate kasutada järgmisi käske:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Säte see /dev/sda eraldi pole kasu. See peab olema määratud kõigi andmete ketta kus järjestikku I/O domineerib I/O mustri.  

Peaksite nägema järgmine väljund, mis näitab, et grub.cfg on edukalt taastatud ja vaikimisi ajasti on uuendatud NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Redhat jaotuse pere, peate ainult järgmine käsk:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Kasutades tarkvara RAID saavutada kõrgem / Ops

Kui teie töökoormus IOps rohkem kui ühe ketta, peate kasutama mitut ketast, tarkvara RAID konfiguratsiooni. Kuna Azure'i sooritab juba ketta paindlikkust kihis kohaliku struktuuri, saavutate kõrgeima jõudluse RAID 0 striping konfiguratsioonist.  Peate ettevalmistamine ja luua uus ketast Azure keskkonnas ja lisada need oma Linux VM enne eraldamine, vormingu ja paigaldus draivid.  Rohkem üksikasju, tarkvara RAID häälestamise kohta oma Linux VM Azure konfigureerimise kohta leiate dokumendis **[Konfigureerida tarkvara RAID Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Järgmised sammud

Pidage meeles, et nagu kõik optimeerimine arutelu, mille peate tegema kontrollib enne ja pärast iga muudatuse muudatus on mõju.  Optimeerimine on samm-sammult protsessi, mis on erinevates arvutites teie keskkonnas piirkonna kohta teistsugused tulemused.  Mis toimib üks konfiguratsioon ei pruugi teistele.

Mõned kasulikud lingid lisaressurssidele: 

- [Premium mälu: Suure jõudlusega Azure virtuaalse masina töökoormus salvestusruum](../storage/storage-premium-storage.md)
- [Azure'i Linux Agent kasutusjuhend](virtual-machines-linux-agent-user-guide.md)
- [Optimeerimine Azure Linux VMs MySQL-i jõudlus](virtual-machines-linux-classic-optimize-mysql.md)
- [Tarkvara RAID Linux konfigureerimine](virtual-machines-linux-configure-raid.md)
