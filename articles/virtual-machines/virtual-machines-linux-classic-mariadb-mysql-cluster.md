<properties
    pageTitle="Azure'i töötavate MariaDB (MySQL-i) kobar"
    description="Luua mõne MariaDB + Galera MySQL-i klaster Azure'i Virtuaalmasinates"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>MariaDB (MySQL-i) kobar - Azure õpetus

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB Enterprise kobar on nüüd saadaval Azure'i turuplatsilt.  Uue pakkumise kuvatakse automaatselt juurutada MariaDB Galera kobar on ARM. Kasutage uue pakkumise https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ kaudu 

Loome mitme juhtslaidi [Galera](http://galeracluster.com/products/) kobar, [MariaDBs](https://mariadb.org/en/about/), töökindlate, skaleeritav ja usaldusväärne drop-in asendust MySQL-i tugevalt saadaval keskkonnas Azure'i Virtuaalmasinates töötada.

## <a name="architecture-overview"></a>Arhitektuuri ülevaade

See teema teeb järgmist:

1. 3-sõlm kobar loomine
2. Andmete ketast eraldamiseks OS ketas
3. Andmete ketast RAID 0/triibuline säte suurendamiseks IOPS loomine
4. Azure'i laadimine koormusetasakaalustusteenuse 3 sõlmed koormuse tasakaalustamiseks kasutamine
5. Korduvate töö minimeerimiseks VM pilt, mis sisaldab MariaDB + Galera loomine ja selle abil saate luua muu kobar VMs.

![Arhitektuur](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  Selles teemas kasutab [Azure CLI](../xplat-cli-install.md) tööriistu, veenduge, et alla laadida ja ühendada need Azure tellimuse juhiste järgi. Kui teil on vaja Azure CLI käske viite, vaadake seda linki [Azure'i CLI käsk viide](../virtual-machines-command-line-tools.md). Saate ka luua [mõne SSH võtit] vaja ja märkige üles **.pem faili asukoht**.


## <a name="creating-the-template"></a>Malli loomine

### <a name="infrastructure"></a>Taristu

1. Hoida ressursside koos osaleja rühma loomine

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Virtuaalse võrgu loomine

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Looge konto salvestusruumi majutada kõik meie ketast. Teate, et te ei peaks pannes rohkem kui 40 tugevalt kasutada ketast sama konto salvestusruumi pihta 20 000 IOPS konto talletuslimiidi vältimiseks. Sel juhul me ei kaugele see number, nii et soovime säilitada kõik sama konto jaoks lihtne

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. CentOS 7 Virtual arvuti pildi nime otsimine

        azure vm image list | findstr CentOS
See väljund umbes `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Kasutage järgmist toimingut nimi.

4. Asendades **/path/to/key.pem** talletuskohaks loodud .pem SSH võtme tee VM malli loomine

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. 4 x 500GB andmete ketast manustamiseks VM RAID konfiguratsiooni kasutamiseks

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH malli VM, mida loodud **mariadbhatemplate.cloudapp.net:22** ja ühenduse loomiseks kasutada oma privaatvõti.

### <a name="software"></a>Tarkvara

1. Saada root

        sudo su

2. Installige RAID tugi.

     - Installige mdadm

                yum install mdadm

     - Luua RAID0/triip konfiguratsiooni EXT4 failisüsteemis

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Ühendamine punkti kausta loomine

                mkdir /mnt/data

     - Äsja loodud RAID seadme UUID tuua

                blkid | grep /dev/md0

     - /Etc/fstab redigeerimine

                vi /etc/fstab

     - Lisada sinna luba automaatne mouting taaskäivitamisel, asendades selle UUID väärtus, mis on saadud enne käsu **blkid** seade

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Uue sektsiooni Mount

                mount /mnt/data

3. Installige MariaDB.

     - MariaDB.repo faili loomiseks tehke järgmist.

                vi /etc/yum.repos.d/MariaDB.repo

     - Seda funktsiooni all sisu

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Olemasoleva postfix ja mariadb-kena tegevust jälle konfliktide vältimiseks eemaldamine

            yum remove postfix mariadb-libs-*

     - Installige MariaDB Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Liikumine MySQL-i andmete kataloogi RAID Blokeeri seade

     - Kopeerige praeguse MySQL-i kataloogi uude asukohta ja vana kataloogi eemaldamine

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Klõpsake uue directory vastavalt õiguste määramine

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Luua nimeviidana osutab uude asukohta RAID sektsioonis vana kataloog

            ln -s /mnt/data/mysql /var/lib/mysql

5. Kuna [SELinux takistab kobar toimingud](http://galeracluster.com/documentation-webpages/configuration.html#selinux)on vaja keelamine käimasolev seanss (kuni ühilduva kuvatakse). Redigeeri `/etc/selinux/config` keelamiseks edaspidised taaskäivitamist:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Kinnitage MySQL-i käivitatakse

    - MySQL-i käivitamine

            service mysql start

    - Turvaline MySQL-i installimise, root parooli määramiseks, eemaldada anonüümsed kasutajad, keelamise remote juurkausta sisselogimine ja testi andmebaasi eemaldamine

            mysql_secure_installation

    - Kasutaja kobar toimingute ja soovi korral saate oma rakenduste andmebaasi loomine

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - MySQL-i peatamine

            service mysql stop

7. Konfiguratsiooni kohatäite loomine

    - MySQL-i konfigureerimine loomiseks kohatäite kobar sätete redigeerimine Asendada selle **`<Vairables>`** või kommenteerige nüüd välja. Mis juhtub pärast loome VM selle malli põhjal.

            vi /etc/my.cnf.d/server.cnf

    - **[Galera]** jaotis redigeerimine ja tühjendage see

    - **[Mariadb]** jaotise redigeerimine

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Avatud nõutav pordid tulemüüri (kasutades FirewallD CentOS 7)

    - MySQL-i:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Laadi tulemüüri:`firewall-cmd --reload`

9.  Optimeerida süsteemi jõudluse. Vaadake käesoleva artikli [jõudluse häälestamine strateegia](virtual-machines-linux-classic-optimize-mysql.md) kohta rohkem üksikasju

    - MySQL-i Otsingukonfiguratsiooni faili uuesti redigeerimine

            vi /etc/my.cnf.d/server.cnf

    - **[Mariadb]** jaotise redigeerimine ja lisab selle all

    > [AZURE.NOTE] Soovitatav on **innodb\_puhvri\_pool_size** on 70% oma VM mälu. See on seatud 2.45 GB siin keskmise Azure VM 3,5 GB RAM.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. MySQL-i peatada, keelata MySQL-i teenuse käivitus, et vältida segi klaster uus sõlm lisamisel, kus töötab ja deprovision masina.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Jäädvustada VM portaali kaudu. (Praegu [probleemi #1268 CLI Azure'i] tööriistad kirjeldatakse asjaolu, et piltide poolt Azure'i CLI tööriistad jäädvustada manustatud andmete ketast.)

    - Sulgumist masina portaali kaudu
    - Klõpsake jäädvustada ja määrata pildi nime **mariadb-galera -** pildina ja kirjeldama ja vaadake, "On run waagent".
    ![Jäädvustada virtuaalse masina](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![jäädvustada virtuaalse masina](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Klaster loomine

Saate luua malli, saate just loodud ja seejärel käivitamiseks ja konfigureerimiseks klaster välja 3 VMs.

1. Luua esimese CentOS 7 VM **mariadb-galera-pilt** pilt, mis on loodud, pakkudes virtuaalse võrgu nimi **mariadbvnet** ja alamvõrgu **mariadb**masina suurus **Keskmine**, läbides pilveteenusesse nimi olla **mariadbha** (või nimi, mida soovite mariadbha.cloudapp.net kaudu) nimi see säte masina **mariadb1** ja **azureuser**olema kasutajanimi ja SSH juurdepääsu lubamine ja läbides SSH certificate .pem faili ja asendamisel **/path/to/key.pem** teega kus te Salvestatud loodud .pem SSH võti.

    > [AZURE.NOTE] Allpool käsud on jagada mitmel real selgust üle, kuid tuleb sisestada iga ühe reana.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. _Ühenduse_ loomiseks 2 rohkem Virtuaalmasinates neile praegu loodud **mariadbha** pilveteenuses, pole konfliktis teiste sama pilveteenuses VMs kordumatu pordiga **VM nime** , samuti **SSH pordi** muutmine.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
ja MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Peate iga 3 VMs sisemise IP-aadressi saamiseks järgmise juhise juurde:

    ![IP-aadressi otsimine](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH 3 VMs sisse ja ja iga konfiguratsiooni faili redigeerimine

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** ja **`wsrep_cluster_address`** , eemaldades selle **#** alguses ja valideerimine need on tõepoolest otsitavat.
    Lisaks asendamine **`<ServerIP>`** sisse **`wsrep_node_address`** ja **`<NodeName>`** sisse **`wsrep_node_name`** soovitud VM IP-aadress. vastavalt nime ja kommenteerige välja ka need read.

5. MariaDB1 klaster käivitamine ja laske käivitamisel

        sudo service mysql bootstrap
        chkconfig mysql on

6. Käivitage MySQL-i MariaDB2 ja MariaDB3 ja lasta käivitamisel

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Laadi tasakaalustamiseks klaster
Rühmitatud VMs loomisel saate lisada need on ligipääsetavusele määratud nimega **clusteravset** tagada need muu viga ja domeenid ja et Azure'i kunagi ei hooldustööd kõik seadmed korraga värskendada. Selle konfiguratsiooni vastab ei toeta, selle Azure teenindustaseme leping (SLA).

Nüüd saate Azure'i laadi koormusetasakaalustusteenuse saldo taotlusi meie 3 sõlme vahel.

Käivitage selle all käskude abil Azure'i CLI teie arvutis.
Käsu parameetrid struktuur on:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Lõpetuseks, kuna CLI määrab laadi-koormusetasakaalustusteenuse juures intervall 15 minutit (mis võib olla veidi liiga pikk), muuta selle portaali jaotises **lõpp-punktid** kõigis VMs

![Lõpp-punkti redigeerimine](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

seejärel klõpsake Reconfigure The Load-Balanced seadmine ja valige edasi

![Reconfigure laadimine tasakaalustatud määramine](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

seejärel sekundi Probe intervalli muutmine ja salvestamine

![Muuda juures intervall](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Klaster kontrollimine

Suur töö on valmis. Klaster peaks olema nüüd kättesaadav `mariadbha.cloudapp.net:3306` mis tabas koormusetasakaalustusteenuse laadimine ja marsruutimine taotlusi vahel 3 VMs tõrgeteta ja tõhusalt.

Kasutada oma lemmik MySQL-i kliendi ühendada või lihtsalt ühenduse ühest VMs kinnitamaks, et see ei tööta.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Klõpsake uue andmebaasi loomiseks ja asustada mõned andmed

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Järgmises tabelis on tulemuseks

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Selles artiklis on loodud 3 sõlm MariaDB + Galera tugevalt saadaval kobar klõpsake Azure'i Virtuaalmasinates töötab CentOS 7. VMs leiate Azure'i laadimine koormusetasakaalustusteenuse vahekonto laadi.

Võite heita pilk [teine võimalus kobar MySQL-i Linux](virtual-machines-linux-classic-mysql-cluster.md) ja kuidas [optimeerida ja Azure Linux VMs MySQL-i jõudlust testida](virtual-machines-linux-classic-optimize-mysql.md).

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Looge SSH alus autentimine]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[probleem #1268 Azure'i CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
