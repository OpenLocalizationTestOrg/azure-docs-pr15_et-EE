<properties
    pageTitle="Optimeeri MySQL-i jõudlus Linux VMs | Microsoft Azure'i"
    description="Saate teada, kuidas optimeerida MySQL-i töötavate Azure'i virtuaalarvuti (VM) töötab Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optimeerimine Azure Linux VMs MySQL-i jõudlus

On palju tegureid, mis mõjutavad MySQL-i jõudlus Azure, mõlemad valiku ja tarkvara virtuaalse riistvara konfiguratsiooni. See artikkel keskendub optimeerimise jõudluse salvestusruumi, süsteemi ja andmebaasi konfiguratsioone kaudu.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Kasutades RAID Azure virtuaalne arvutis
Salvestusruumi on kõige olulisem tegur, mis mõjutab andmebaasi jõudluse cloud keskkonnas.  Võrreldes ühe ketta, RAID saate sisestada kiirem juurdepääs kokkulangevus kaudu.  Vaadake [Standard RAID tasemeid](http://en.wikipedia.org/wiki/Standard_RAID_levels) rohkem üksikasju.   

Ketta I/O läbilaskevõime ja I/O vastuse kellaaja Azure võib oluliselt parandada RAID. Meie laborianalüüside kuvada ketta I/O läbilaskevõime saate kahekordseks ja I/O aega saab vähendada pool keskmiselt kui RAID ketast arv on kahekordseks (2 kuni 4, 4-8 jms). Üksikasju vt [lisa A](#AppendixA) .  

Lisaks ketta I/O, MySQL-i jõudlus parandab RAID tihendada.  Üksikasju vt [B lisa](#AppendixB) .  

Võiksite kaaluda kogumi suurus. Üldiselt kui teil on suuremaks kogumi, saate alumise kohal, eriti suurte kirjutab jaoks. Kui kogumi maht on liiga suur, selle võib lisada täiendavad pea kohal ja te ei saa ära RAID. Praeguse vaikesuurus on 512KB, mis on tõendada on kõige üldine tootmise keskkondades optimaalne. Üksikasjad leiate [Lisa C](#AppendixC) .   

Pange tähele, et mitu kettale saate lisada erinevaid virtuaalse masina tüüpi on piirangud. Need piirangud on üksikasjalikult [virtuaalse masina](http://msdn.microsoft.com/library/azure/dn197896.aspx)ja pilvepõhise teenuse suurusega Azure. Kuigi võite määrata, et häälestada RAID vähem ketast, peate 4 manustatud andmete ketast RAID näide selles artiklis jälgimiseks.  

Selles artiklis eeldatakse, et olete juba loonud Linuxi virtuaalse masina ja MySQL-i installinud ja konfigureerinud. Alustamise kohta lisateabe saamiseks vaadake Kuidas installida Azure MySQL-i.  

###<a name="setting-up-raid-on-azure"></a>Kuidas häälestada RAID Azure
Järgmised toimingud näitab, kuidas luua RAID Azure Azure klassikaline portaalis. Te saate häälestada ka RAID Windows PowerShelli skriptide abil.
Selles näites me konfigureerimine RAID 0 4 ketast.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Samm 1: Virtual arvuti kettal andmete lisamine  

Klassikaline portaali Azure'i Virtuaalmasinates lehel nuppu virtuaalse masina, millele soovite lisada andmed kettale. Selles näites on virtuaalse masina mysqlnode1.  

![][1]

Valige lehel virtuaalse masina **armatuurlaud**.  

![][2]


Klõpsake tööülesande **manustamine**.

![][3]

Ja seejärel nuppu **Manusta tühjale kettapuhastusriista**.  

![][4]

Andmete ketast, jaoks seadma **pole** **Host vahemälu eelistused** .  

See lisab ühe tühjale kettapuhastusriista virtual arvuti. Korrake seda toimingut kolm korda, et teil on 4 andmete ketast RAID.  

Saate vaadata tuum teadete Logi vaadates lisatud draivid virtual kohapeal. Kuvamiseks Ubuntu, kasutage järgmist käsku:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Samm 2: Looge RAID täiendavad ketast
Sellest artiklist leiate üksikasjalikud RAID häälestamise juhised tehke järgmist.  

[Tarkvara RAID Linux konfigureerimine](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Kui kasutate XFS failisüsteemi, järgige alltoodud juhiseid, kui olete loonud RAID.

XFS Debian, Ubuntu või Linux Mint installimiseks kasutage järgmine käsk:  

    apt-get -y install xfsprogs  

XFS kohta Fedora, CentOS või RHEL installimiseks kasutage järgmine käsk:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Samm 3: Uus salvestusruumi tee häälestamine
Kasutage järgmine käsk:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Samm 4: Algsed andmed kopeerimine uue salvestusruumi tee
Kasutage järgmine käsk:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Juhis 5: Õiguste muutmiseks nii, et pääsete juurde MySQL-i (lugemine ja kirjutamine) andmed kettale
Kasutage järgmine käsk:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Ketta I/O ajastamise algoritmile reguleerimine
Linux rakendab nelja tüüpi sisend plaanimine algoritmide kohta:  

-   NOOP algoritmile (nr toimingu)
-   Tähtaeg algoritmile (tähtaeg)
-   Täielikult ja tehnikamessi andmebaasitõrge algoritmile (CFQ)
-   Eelarve perioodi algoritmile (Anticipatory)  

Saate valida erinevaid I/O skeduloijat erinevate stsenaariumide optimeerida jõudlust. Täielikult muutmälu keskkonnas, ei ole suur erinevus jõudluse algoritmide CFQ ja tähtaja vahel. Soovitatav on üldiselt määratud MySQL-i andmebaasi keskkonna stabiilsuse tähtaeg. Kui seal on palju järjestikku sisend, võib CFQ vähendada I/O jõudluse parandamine.   

SSD ja muid seadmeid, kasutades NOOP või tähtaeg jõudlus parem kui vaikimisi ajasti.   

2,5 tuum on vaikimisi I/O ajastamise algoritmi tähtaeg. Alates tuum 2.6.18, CFQ sai vaikimisi I/O ajastamise algoritmi.  Saate määrata selle sätte tuum käivitamisel või süsteemi töötamisel dünaamiliselt seda sätet muuta.  

Järgmises näites näitab, kuidas kontrollida ja määrata vaikimisi ajasti NOOP algoritmile.  

Pere Debian jaotuse:

###<a name="step-1view-the-current-io-scheduler"></a>Samm 1. vaadata praeguse I/O ajasti
Kasutage järgmine käsk:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Kuvatakse väljund, mis näitab praegust ajasti pärast.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Samm 2. Muutke praegust seadet (/ arendaja/sda) sisend algoritmi plaanimine
Saate kasutada järgmisi käske:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Säte see /dev/sda eraldi pole kasu. See peab olema määratud kõigi andmete ketta andmebaasi asukoht.  

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

##<a name="configure-system-file-operations-settings"></a>Süsteemi faili toimingute sätete konfigureerimine
Ühe parim on keelata atime logimisfunktsioon failisüsteemis. Atime on faili Accessi viimast. Iga kord, kui faili on kättesaadav, failisüsteemis kirjete ajatemplit Logi sisse. Siiski seda teavet kasutatakse harva. Saate selle keelata, kui te ei vaja, mis vähendab ketta Accessi aega.  

Atime logimise keelamiseks peate faili süsteemi konfiguratsioon faili /etc/ fstab muuta ja lisada **noatime** suvand.  

Näiteks redigeerida vim /etc/fstab faili lisamine noatime, nagu allpool näidatud.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Seejärel paigalda failisüsteemi järgmine käsk:  

    mount -o remount /RAID0

Testige muudetud tulemi. Pange tähele, et kui test faili muutmiseks Accessi aeg ei värskendata.  

Näide: enne     

![][5]

Näide: pärast

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Süsteemi pidemete jaoks kõrge kokkulangevus suurima arvu suurendamine
MySQL-i on kõrge kokkulangevus andmebaas. Samaaegseid pidemete vaikearvu on 1024 Linuxi, mis alati ei piisa. **Suurendamiseks maksimaalne samaaegseid pidemeid süsteemi toetamiseks kõrge kokkulangevus MySQL toimige järgmiselt**.

###<a name="step-1-modify-the-limitsconf-file"></a>Samm 1: Limits.conf faili muutmiseks.
Lisage järgmised neli rida /etc/security/limits.conf faili suurim lubatud samaaegseid pidemeid suurendamiseks. Pange tähele, et 65536 on maksimaalne arv, mis võib toetada.   

    * pehmete nofile 65536
    * suur nofile 65536
    * pehmete nproc 65536
    * suur nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Samm 2: Värskendada süsteemi uue piirangud
Käivitage järgmine käsk:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Samm 3: Veenduge, et piiranguid värskendatakse käivitamisel
Panna käivitus järgmised käsud /etc/rc.local faili, nii et see jõustub buutimine igal ajal.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>MySQL-i andmebaasi optimeerimine
Saate konfigureerida MySQL-i Azure nimega kohapealse arvutis on sama jõudluse häälestamine strateegiat.  

Peamised I/O optimeerimise sätted on:   

-   Suurendage vahemälu.
-   Vähendage I/O vastuse aeg.  

Optimeerida MySQL-i serveri sätted, saate värskendada my.cnf fail, mis on vaikimisi konfiguratsioonifail nii serveris ja kliendi arvuti.  

Järgmist konfiguratsiooni on peamised MySQL-i jõudlust mõjutavad tegurid.  

-   **innodb_buffer_pool_size**: puhvri pargi sisaldab puhverdatud andmeid ja register. Tavaliselt on see määratud 70% füüsilise mälu.
-   **innodb_log_file_size**: see on tee uuesti Logi maht. Saate tagada, et kirjutada toiminguid on kiire, usaldusväärseid ja saab pärast krahhi uuestitegemine logid. See on seatud 512MB, mis annab teile palju ruumi logimine kirjutada toiminguid.
-   **max_connections**: mõnikord rakendused sulgeda ühendused õigesti. Suurem väärtus annab server rohkem aega prügikastis tegevusetult ühendused. Maksimaalne ühendused on 10000, kuid soovitatav kuni 5000.
-   **Innodb_file_per_table**: see säte lubamine või keelamine InnoDB võimalus salvestada eraldi faili tabelid. Selle suvandi sisselülitamine tagab mitu täpsemalt administreerimiskeskuse toimingute tõhus rakendatud. Jõudluse seisukohast, saate tabeli ruumi edastamise kiirendamiseks ja optimeerida praht haldus. Seega on soovitatav seda sätet sees.</br>
    MySQL-i 5.6, kaudu on vaikimisi sees. Seetõttu enam pole vaja midagi. Versioonidega, mis on vanemad kui 5.6, vaikesätteid on väljas. See ON on nõutav. Ja rakendada see enne, kui andmed on laaditud, kuna see mõjutab ainult äsja loodud tabelid.
-   **innodb_flush_log_at_trx_commit**: vaikeväärtus on 1, mille ulatus väärtuseks 0 ~ 2. Vaikeväärtus on autonoomse MySQL-i DB sobivaim võimalus. 2 säte võimaldab enamiku andmeterviklus ja sobib juhtslaidi MySQL-i klaster. 0 see säte võimaldab andmekao, mis võivad mõjutada töökindluse mõnedel juhtudel parema jõudluse tagamiseks ja sobib ori MySQL-i kobar.
-   **Innodb_log_buffer_size**: Logi puhvri võimaldab tehinguid käivitamiseks ilma tühjendage Alustuseks Logi kettale enne tehingute Kinnita. Kui suur kahendarvu objekti või teksti välja, vahemälu on tarbitud väga kiiresti ja sagedased ketta I/O käivitatakse. On parem suurendada puhvri kui Innodb_log_waits olekus muutuja pole 0.
-   **query_cache_size**: on parim valik on algusest välja lülitada. Seadke query_cache_size 0 (see on nüüd MySQL-i 5.6 vaikesäte) ja päringute kiirendamiseks kasutada muid viise.  

Pärast optimeerimine võrdlemiseks vt [Lisa D](#AppendixD) .


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>MySQL-i aeglane päring Logi analüüsimiseks jõudluse kitsaskoht sisselülitamine
MySQL-i aeglane päring Logi aitavad teil tuvastada aeglane päringute MySQL-i jaoks. Pärast lubamine MySQL-i aeglane päring Logi, saate kasutada MySQL-i tööriistade nagu **mysqldumpslow** jõudluse kitsaskoht tuvastamiseks.  

Pange tähele, et see on vaikimisi pole lubatud. Aeglane päring Logi sisselülitamine võib kasutada mõne CPU ressursse. Seetõttu on soovitatav, et lubate seda ajutiselt kitsaskohtade jõudluse tõrkeotsingu.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Samm 1: My.cnf faili muuta, lisades järgmised read lõppu   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Samm 2: Taaskäivitage MySQL-i server
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Samm 3: Kontrollida, kas võtab selle sätte mõju käsk "Kuva"

![][7]   

![][8]

Selle näite puhul saate vaadata funktsiooni aeglane päring on sisse lülitanud. Seejärel saate tööriista **mysqldumpslow** kindlaks, et täitmise kitsaskohtadest ja optimeerida jõudlust, näiteks saate lisada registrid.





##<a name="appendix"></a>Lisa

Järgnevad näidisandmed jõudluse test, toodetud suunatud lab keskkonnas, need, esitada Üldine taust jõudluse andmete trend erinevate jõudluse häälestamine lähenemisel, kuid jaotises muu keskkonnas või toote versioonid võivad tulemused erineda.

<a name="AppendixA"></a>Lisa A:  
**Jõudluse (IOPS) erinevate RAID tasemed**


![][9]

**Testi käsud:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE'I. Märkus: Katse töökoormus kasutab 64 teemad, jõua RAID ülempiiri.

<a name="AppendixB"></a>Lisa B:  
**MySQL-i jõudlus (läbilaskevõime) võrdlus erinevate RAID tasemed**   
(XFS failisüsteemis)


![][10]  
![][11]

**Testi käsud:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL-i jõudlus (OLTP) võrdlus erinevate RAID tasemed**  
![][12]

**Testi käsud:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Lisa C:   
**Ketas (IOPS) võrdlus erinevate kogumi suurused**  
(XFS failisüsteemis)


![][13]

**Testi käsud:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Pange tähele, et selle testimiseks kasutatavate failimaht on 30GB ja 1GB, koos RAID 0(4 disks) XFS fie süsteem.


<a name="AppendixD"></a>Lisa D:  
**MySQL-i jõudlus (läbilaskevõime) võrdlus enne ja pärast optimeerimine**  
(XFS failisüsteemis)


![][14]

**Testi käsud:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Otsingukonfiguratsiooni säte vaike-ja optmization on järgmine:**

|Parameetrid |Vaikimisi    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Ükski   |7G
|**innodb_log_file_size**   |5M |512 MB
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Rohkem üksikasjalikku optimeerimine konfiguratsiooni parameetrid, vaadake ametliku MySQL-i juhiseid.  

[http://dev.MySQL.com/doc/refman/5.6/EN/innodb-Configuration.html](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/EN/innodb-parameters.html#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Testimiskeskkonnas**  

|Riistvara   |Üksikasjad
|-----------|-------
|CPU    |AMD Opteron(tm) protsessor 4171 HE / 4 südamikud
|Mälu |14G
|ketas   |10G/kettalt
|OS |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
