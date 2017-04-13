<properties
    pageTitle="MySQL-i Linux VM häälestamine | Microsoft Azure'i "
    description="Siit saate teada, kuidas installida MySQL-i virnas Azure'i Linux virtuaalse masina (Ubuntu või RedHat pere OS)"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Kuidas installida Azure MySQL-i


Sellest artiklist saate teada, kuidas installida ja konfigureerida MySQL-i Azure virtuaalne arvutisse, mis töötab Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>Arvuti virtuaalne MySQL-i installimine

> [AZURE.NOTE] Juba peab teil olema Microsoft Azure virtuaalse masina töötab Linux selles õpetuses lõpuleviimiseks. Leiate [Azure'i Linux VM õpetuse](virtual-machines-linux-quick-create-cli.md) loomine ja Linux VM abil häälestada `mysqlnode` nimeks VM ja `azureuser` kasutajana enne jätkamist.

Sel juhul kasutada 3306 pordi MySQL-i pordi.  

Ühenduse Linux loodud kaudu putty VM. Kui kasutate Azure Linux VM esimest korda, vaadake, kuidas kasutada kitt ühenduse Linux VM [siin](virtual-machines-linux-mac-create-ssh-keys.md).

Kasutame hoidla paketi installimiseks MySQL5.6 näitena selle artikli. Tegelikult MySQL5.6 on veel täiustamise tulemusi kui MySQL5.5.  Lisateabe saamiseks [siin](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Kuidas installida MySQL5.6 Ubuntu
Me kasutame Linux VM Ubuntu Azure siin.

- Samm 1: Installi MySQL-i serveri 5.6 aktiveerimine `root` kasutaja:

            #[azureuser@mysqlnode:~]sudo su -

    Mysql-server 5.6 installimiseks tehke järgmist.

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Installimise ajal kuvatakse dialoogiboks akna poping kuni Küsi alloleval MySQL-i parool seada, ja te vajate parooli määramiseks siin.

    ![pilt](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Sisestage parool uuesti.

    ![pilt](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Samm 2: Login MySQL-i Server

    Kui MySQL-i serveri installimine on lõpule jõudnud, käivitatakse MySQL-i teenuse automaatselt. MySQL-i serverisse sisselogimist `root` kasutaja.
    Kasutage funktsiooni all käsk Logi sisse ja sisestatud parool.

             #[root@mysqlnode ~]# mysql -uroot -p

- Samm 3: Töötava MySQL-i teenuse haldamine

    (a) MySQL-i teenuse olekut hankimine

             #[root@mysqlnode ~]# service mysql status

    (b) MySQL-i teenuse käivitamine

             #[root@mysqlnode ~]# service mysql start

    (c) MySQL-i teenuse peatamine

             #[root@mysqlnode ~]# service mysql stop

    d MySQL-i teenuse taaskäivitamine

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Kuidas installida MySQL-i punane rolli OS pere nagu CentOS Oracle'i Linux
Me kasutame Linux VM CentOS või Oracle Linux siin.

- Samm 1: Lisa MySQL-i Yum hoidla vahetamise abil `root` kasutaja:

            #[azureuser@mysqlnode:~]sudo su -

    Laadige alla ja installige MySQL-i väljaanne pakett.

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Samm 2: Redigeerige faili allalaadimiseks MySQL5.6 paketi hoidla MySQL-i lubamiseks all.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Värskendage iga väärtuse selle faili allpool.

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Samm 3: Install MySQL MySQL-i hoidla installida MySQL-i kaudu:

           #[root@mysqlnode ~]#yum install mysql-community-server

    MySQL-i pööret MINUTIS paketi ja kõigi sellega seotud elemendid on installitud.

- Samm 4: Töötava MySQL-i teenuse haldamine

    (a) MySQL-i serveri teenuse oleku kontrollimine

           #[root@mysqlnode ~]#service mysqld status

    (b) kontrollige, kas vaikimisi portide, MySQL-i server töötaks.

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) MySQL-i server alustamiseks tehke järgmist.

           #[root@mysqlnode ~]#service mysqld start

    d MySQL-i server lõpetamiseks tehke järgmist.

           #[root@mysqlnode ~]#service mysqld stop

    (e) seada käivituma MySQL-i süsteemis buutimine üles:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Kuidas installida MySQL-i SUSE Linux
Me kasutame Linux VM koos OpenSUSE siin.

- Samm 1: Alla laadida ja installida MySQL-i Server

    Aktiveerige `root` kasutaja kaudu all käsk:  

           #sudo su -

    Laadige alla ja installige MySQL-i pakett.

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Samm 2: Töötava MySQL-i teenuse haldamine

    (a) MySQL-i server oleku kontrollimine

           #[root@mysqlnode ~]# rcmysql status

    (b) sisse kas vaikeport MySQL-i server:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) MySQL-i server alustamiseks tehke järgmist.

           #[root@mysqlnode ~]# rcmysql start

    d MySQL-i server lõpetamiseks tehke järgmist.

           #[root@mysqlnode ~]# rcmysql stop

    (e) seada käivituma MySQL-i süsteemis buutimine üles:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Järgmise juhise juurde
Kasutus- ja MySQL-i [siin](https://www.mysql.com/)teavet leida.
