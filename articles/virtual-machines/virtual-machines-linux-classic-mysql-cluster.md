<properties
    pageTitle="MySQL-i clusterize koos koormusetasakaalustusega komplekti | Microsoft Azure'i"
    description="Häälestamise koormusetasakaalustusega, kõrge kättesaadavus, Linux MySQL-i kobar loodud Azure mudeliga klassikaline juurutamine"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Koormusetasakaalustusega komplekti clusterize Linux MySQL-i abil

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Selle artikli eesmärk on uurida ja kirjeldavad saadaval Microsoft Azure'i, uurimine MySQL-i Server kõrge-saadavus kruntvärvina tugevalt saadaval Linuxi-põhiste teenuste juurutamine erinevus. Seda moodust illustreeriv video on saadaval [kanali 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Me liigendada ühiskasutuses midagi kaks sõlme ühe-juhtslaidi MySQL-i kõrge-saadavus lahenduse DRBD, Corosync ja südamestimulaator. Ainult üks sõlm töötab MySQL-i korraga. Lugemine ja kirjutamine DRBD ressurss on piiratud ainult üks sõlm korraga.

Ei ole vaja VIP lahenduse nagu LVS, kuna me kasutame Microsoft Azure'i laadi tasakaalustatud komplekti osutada nii funktsiooni round-jaan lõpp-punkti avastamine, eemaldamine ja graatsiline taastada, VIP. VIP on määratud Microsoft Azure'i kui luua pilveteenusesse globaalselt marsruuditavaid IPv4 aadress.

Muude võimalike arhitektuurides MySQL-i, sh kobar NBD, Percona ja Galera samuti mitu vahevara lahendusi, sh vähemalt üks jaoks on saadaval nimega VM [VM Depot](http://vmdepot.msopentech.com)kohta. Kui järgmisi lahendusi ainusaate vs multisaate või leviedastuse kohta saate kopeerida ja ei looda ühine või mitme võrgu liidesed, stsenaariume peaks olema lihtne Microsoft Azure juurutamine.

Muidugi nende klastrite arhitektuurides saate laiendada muude toodete samal viisil nagu PostgreSQL-i ja OpenLDAP. Näiteks selle protseduuri tasakaalustamiseks koos ühiskasutusega midagi laadi on edukalt testitud mitme juhtslaidi OpenLDAP ja võite vaadata seda meie kanali 9 ajaveebi.

## <a name="getting-ready"></a>Kuidas valmis

Peate Microsoft Azure'i konto abil saate luua vähemalt kaks (2) VMs kehtiv tellimus (XS on selles näites kasutatakse), võrgus ja on alamvõrku, ka osaleja rühma ja saadavus määrata, võimaluse luua uue VHDs sama piirkonna pilveteenusesse ja Linux VMs manustamiseks.

### <a name="tested-environment"></a>Testitud keskkonnas

- Ubuntu 13.10
  - DRBD
  - MySQL-i Server
  - Corosync ja südamestimulaator

### <a name="affinity-group"></a>Osaleja rühma

Mõne osaleja rühma lahendus on loodud logige Azure klassikaline portaali kerimine allapoole sätted ja osaleja uue rühma loomine. Selle osaleja rühma määratakse hiljem loodud eraldatud.

### <a name="networks"></a>Võrgu

On loodud uue võrgu ja on alamvõrgu on loodud võrgu sees. Valisime 10.10.10.0/24 võrgu ainult üks /24 alamvõrgu sees.

### <a name="virtual-machines"></a>Virtuaalmasinates

Esimene Ubuntu 13.10 VM Endorsed Ubuntu galerii abil loodud ja nimetatakse `hadb01`. Protsessi, mida nimetatakse hadb on loodud uue pilveteenuses. Me kutsume sellisel viisil illustreerimiseks jagatud, koormusetasakaalustusega laadi teenus on kui lisame veel ressursse. Loomine `hadb01` on sündmustevaene ja täidetakse portaalis. Lõpp-punkt SSH luuakse automaatselt ja meie loodud võrgu oleks märgitud. Samuti valida luua uue kättesaadavus, VMs seadmine.

Kui esimene VM on loodud (tehniliselt, kui luuakse pilveteenusesse) me jätkata luua teise VM `hadb02`. Teine VM ka kasutame Ubuntu 13.10 VM portaalis galeriist, kuid me otsustate kasutada olemasoleva pilveteenuses `hadb.cloudapp.net`, selle asemel, et luua uue eksemplari. Meil peaks olema automaatselt valitud võrgu ja -saadavus määramine. Luuakse SSH lõpp liiga.

Pärast seda, kui mõlemad VMs on loodud, võtame märkuse SSH pordi `hadb01` (TCP 22) ja `hadb02` (automaatselt määratud Azure)

### <a name="attached-storage"></a>Manustatud salvestusruum

Me lisada uue ketta nii VMs ja looge uus 5 GB ketast protsessi. Funktsiooni ketast valmimiseni VHD ümbrises meie peamine operatsioonisüsteem ketast jaoks. Kui ketast on loodud ja lisatud ei ole vaja meil taaskäivitage Linux nagu tuum kuvatakse uuele seadmele (tavaliselt `/dev/sdc`, saate kontrollida `dmesg` väljund)

Iga VM me jätkata luua uus sektsioon kasutades `cfdisk` (esmane, Linux sektsioon) ja kirjutage sektsiooni uus tabel. **Looge selle sektsiooni failisüsteemi** .

## <a name="setting-up-the-cluster"></a>Kuidas häälestada klaster

Mõlemad Ubuntu VMs peame APT Corosync, südamestimulaator ja DRBD installimiseks kasutada. Kasutades `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Installige MySQL-i sel ajal** . Debiani ja Ubuntu installi skriptide lähtestada MySQL-i andmete kataloogi sisse `/var/lib/mysql`, kuid kuna kataloogi on asendada DRBD failisüsteemi, läheb vaja selleks hiljem.

Selles etapis meil peaks ka kontrollida (abil `/sbin/ifconfig`) nii VMs kasutate 10.10.10.0/24 alamvõrgu aadressid ja, et nad ping omavahel nime järgi. Soovi korral saate kasutada ka `ssh-keygen` ja `ssh-copy-id` veendumaks, et nii VMs saate suhelda SSH nõudmata parool.

### <a name="setting-up-drbd"></a>Kuidas häälestada DRBD

Loome DRBD ressurss, mis kasutab aluseks olevate `/dev/sdc1` partition andes on `/dev/drbd1` ressursi võimeline olema vormindatud ext3 ja kasutada nii põhi- ja sõlmed. Selle tegemiseks Avage `/etc/drbd.d/r0.res` ja kopeerige järgmine ressursi määratlus. Mõlemad VM tehke järgmist.

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Pärast seda, käivitada ressursi abil `drbdadm` nii vms:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

Ja lõpuks klõpsake esmast (`hadb01`) jõustada DRBD ressursi omandiõiguse (primaarne):

    sudo drbdadm primary --force r0

Kui/proc/drbd sisu (`sudo cat /proc/drbd`) nii VMs, peaksite nägema `Primary/Secondary` kohta `hadb01` ja `Secondary/Primary` kohta `hadb02`, kooskõlas sel hetkel lahendus. 5 GB vaba sünkroonitakse klientidele tasuta 10.10.10.0/24 võrgu kaudu.

Kui ketta sünkroonitakse saate luua failisüsteemi `hadb01`. Katsetamiseks kasutasime ext2, kuid ext3 failisüsteemi loomiseks järgmisi juhiseid.

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>Paigaldus DRBD ressurss

Klõpsake `hadb01` nüüd olete valmis DRBD ressursid ühendada. Debiani ja derivaatide kasutamine `/var/lib/mysql` MySQL-i andmete kausta. Kuna me ei ole installinud MySQL-i, saame luua kausta ja mount DRBD ressursi. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>MySQL-i seadistamine

Nüüd olete valmis MySQL-i installimine `hadb01`:

    sudo apt-get install mysql-server

Jaoks `hadb02`, on teil kaks võimalust. Saate installida mysql-server kohe, mis loob /var/lib/mysql ja täida see uue andmekataloogi, ja siis edasi Eemalda sisu. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Teine võimalus on Tõrkesiirde `hadb02` ja seejärel installige mysql-server (installimise skriptide teate olemasoleva installi ja ei puudutage seda)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Kui te ei plaani Tõrkesiirde DRBD kohe, on lihtsam, kuigi väidetavalt vähem elegantne esimene variant. Kui olete selle häälestanud, saate alustada tööd MySQL-andmebaasiga. Klõpsake `hadb02` (või sellest, millist üks serverid on aktiivne, vastavalt DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Hoiatus**: Viimane väide tegelikult keelab selle tabeli root kasutaja autentimise. See tuleks asendada oma tootmise äriklassi anda laused ja on kaasatud ainult toodud.

Samuti peate lubama networking MySQL kui soovite teha päringuid: väljaspool VMs, mis on selle juhendi eesmärk. Avage mõlemad VMs `/etc/mysql/my.cnf` ja liikuge sirvides `bind-address`, muutes selle 127.0.0.1 0.0.0.0. Pärast faili salvestamist välja lisamine `sudo service mysql restart` oma praeguse primaarne kohta.

### <a name="creating-the-mysql-load-balanced-set"></a>Laadi MySQL-i loomise tasakaalustatud määramine

Me portaali naasmiseks ja liikuge sirvides soovitud `hadb01` VM, siis lõpp-punktid. Uue lõpp-punkti loomiseks, valige MySQL-i (TCP 3306) rippmenüü ja märkige ruut *Loo uus koormusetasakaalustusega määramine* . Kutsume meie koormus tasakaalustatud lõpp-punkti `lb-mysql`. Me ei jäta enamik suvandid eraldi, välja arvatud aeg, mida me ei vähenda-5 (sekundites, miinimum)

Lõpp-punkti loomise järel me minna `hadb02`, lõpp-punktid, ja luua uue lõpp-punkti, kuid me valida `lb-mysql`, seejärel valige rippmenüüst MySQL-i. Samuti saate selles etapis Azure'i CLI.

Praegu meil kõike, mida läheb vaja käsitsi tööks klaster.

### <a name="testing-the-load-balanced-set"></a>Laadi testimine tasakaalustatud määramine

Katset võib teha ka muust arvutist, sel juhul mis tahes MySQL-i klient, samuti rakenduste (nt phpMyAdmin Azure'i veebisait töötab) abil kasutasime MySQL-i käsurea tööriista teise Linux kasti.

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Käsitsi suuda üle

Saate simuleerida failovers nüüd sulgemise MySQL-i, üleminek DRBD tema esmase ja MySQL-i uuesti käivitada.

Klõpsake hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Seejärel klõpsake hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Kui te Tõrkesiirde käsitsi korrake oma serveri päring ja see peaks töötada täiesti.

## <a name="setting-up-corosync"></a>Kuidas häälestada Corosync

Corosync on aluseks kobar infrastruktuuri vaja soovitus töötada. Südamelöögi ajal v1 ja v2 kasutajate (ja teiste meetodite nagu Ultramonkey) Corosync on jagatud CRM-i funktsioone, seni, südamestimulaator Hearbeat veel sarnaseid funktsioone.

Azure'i jaoks Corosync peamine piirang on Corosync eelistab multisaate leviedastuse ainusaate suhtlus üle, et Microsoft Azure'i võrgunduse toetab ainult ainusaate.

Õnneks Corosync on töötamise ainusaate režiimi ja ainult real piirang on see, et kuna kõik sõlmed on suhtlemine omavahel *automagically*, peate määratlema sõlmed failides konfiguratsiooni, sh IP-aadressid. Me kasutame Corosync näide failide jaoks ainusaate ja lihtsalt muutke siduda aadress, sõlm loendid ja logimine directory (Ubuntu kasutab `/var/log/corosync` ajal näiteks failide kasutamine `/var/log/cluster`) ja kvoorumi tööriistad.

**Märkus on `transport: udpu` järgnevalt ja käsitsi määratletud IP-aadresside sõlmed**.

Klõpsake `/etc/corosync/corosync.conf` nii sõlmed:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Me kopeerige see konfiguratsioonifail nii VMs ja käivitage Corosync nii sõlmed:

    sudo service start corosync

Varsti pärast teenuse käivitamine tuleks klaster praeguse ring ja on otsustusvõimeline. Me vaadates logid märkige ruut selle funktsiooni või:

    sudo corosync-quorumtool –l

Tehke peaks väljund sarnaneb järgmisel pildil.

![corosync-quorumtool - l väljundi näidis](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Kuidas häälestada südamestimulaator

Südamestimulaatori kasutab klaster ressursid jälgida, kui primaries minna määratlemine ja nende ressursside aktiveerige sekundaaride. Ressursid saate määratleda kogumi saadaval skriptide või LSB (käivitamise nagu) skriptide hulgas muud Valikud.

Soovime soovitus "oma" DRBD ressurss, on mountpoint ja MySQL-i teenus. Kui südamestimulaator saab sisse ja välja lülitada DRBD, kinnitage / umount selle ja käivitamise ja lõpetamise MySQL-i õiges järjestuses, kui midagi halba juhtub esmane, meie häälestamine on lõpule viidud.

Südamestimulaatori esmakordsel installimisel konfiguratsioonist peaks olema lihtne, nt:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Märkige see töötab `sudo crm configure show`. Nüüd looge faili (öelge `/tmp/cluster.conf`) koos järgmistest allikatest:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

Ja nüüd laadimine konfiguratsiooni (ainult peate selleks ühe sõlme).

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Lisaks veenduge, et südamestimulaator algab nii sõlmed käivitamisel:

    sudo update-rc.d pacemaker defaults

Mõne sekundi, ja kasutades `sudo crm_mon –L`, veenduge, et üks teie sõlmed on muutunud juhtslaidi klaster ning töötab kõik ressursid. Saate kontrollida, et ressursside töötavad mount ja ps.

Järgmisel pildil on näha `crm_mon` koos ühe sõlme peatatud (välju abil CTRL-c.)

![crm_mon sõlm on peatatud](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

Ja sellel kuvatõmmisel on kuvatud nii sõlmed ühe juhtslaidi ja üks ori.

![crm_mon funktsionaalseid juhtslaidi/slave](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testimine

Olete valmis, kuvatakse automaatselt Tõrkesiirde modelleerimiseks. On kaks võimalust, et sellega: pehmed ja suur. Pehmete viis on kasutusel on kobar sulgumist funktsioon: ``crm_standby -U `uname -n` -v on``. Kasutades seda juhtslaidil ori võtab üle. Pidage meeles, et see tagasi määratud välja lülitatud (crm_mon teile ühe sõlme on puhkerežiimis teisiti)

Kõva viis on sulgemise esmane VM (hadb01) portaali kaudu või muutmise Töötase VM (s.o peatada, sulgumist) ja seejärel meil aidata Corosync ja südamestimulaator,-signaal juhtslaidi 's läheb alla. Võite seda kontrollida (kasulik hooldustööd windows), kuid me jõustada ka seda stsenaariumi VM lihtsalt külmutamise teel.

## <a name="stonith"></a>STONITH

See peaks saama probleemi VM sulgumist Azure'i CLI asemel STONITH skripti, mis määrab füüsilise seadme kaudu. Saate kasutada `/usr/lib/stonith/plugins/external/ssh` base ja luba STONITH soovitud klaster konfiguratsiooni. Azure'i CLI globaalselt peaks olema installitud ja avalda sätted/profiili tuleks laadida soovitud klaster kasutaja jaoks.

Proovi kood saadaval [github](https://github.com/bureado/aztonith)ressursi. Peate muutma selle kobar konfiguratsiooni, lisades järgmist `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Märkus:** skripti ei teha üles/alla kontrollid. Algne SSH ressurss oli 15 ping kontrolli, kuid taastamise jaoks on Azure VM aja võib olla mitu muutujana.

## <a name="limitations"></a>Piirangud

Kehtivad järgmised piirangud.

- Linbit DRBD ressursi skripti, mis haldab DRBD ressurssi südamestimulaator kasutab `drbdadm down` kui ka siis, kui lihtsalt läheb sõlme valige sõlm, sulgemise. See ei ole optimaalne, kuna ori on ei olla sünkroonimine DRBD ressursi ajal juhteksemplari saab kirjutab. Kui juhteksemplari ei lahkelt, ori mõne vanema failisüsteemi riigi üle võtta. On kaks võimalikku võimalust lahendada see.
  - Jõustamine on `drbdadm up r0` kõik kobar sõlmed kaudu kohaliku (mitte clusterized) valvekoer, või
  - Redigeerimise linbit DRBD skript, et `down` on kutsutud, sisse `/usr/lib/ocf/resource.d/linbit/drbd`.
- Laadi koormusetasakaalustusteenuse peab olema vähemalt 5 minutit vastata, et rakenduste tuleks arvestada kobar ja olla rohkem, ajalõpp; muude arhitektuurides aitab, näiteks Rakendusesisese järjekorrad, päringu middlewares jne.
- MySQL-i häälestamine on vaja tagada kirjutamist tehakse sane tempos ja vahemälu on tühjendatud kettale nii tihti kui võimalik mälu kaotus minimeerimiseks
- Kirjutage jõudlust on sõltuv VM omavahel virtuaalse aktiveerimine, kui see on kasutatud, DRBD ise seade
