<properties
    pageTitle="Azure'i Linux Agent GitHub kaudu värskendada | Microsoft Azure'i"
    description="Siit saate teada, kuidas oma Linux VM Azure lateset versiooni Github vä Azure Linux Agent"
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
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Kuidas värskendada Azure'i Linux Agent VM uusim versioon GitHub kaudu

Värskendage oma [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) Linux VM Azure, peab teil olema juba:

1. Töötava Linux VM Azure.
2. Ühenduse selle Linux VM SSH abil.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Kui teile täita selle ülesande desinstallimine Windowsiga arvutist, saate PuTTY SSH abil sisse arvuti Linux. Lisateavet leiate teemast [virtuaalse masina töötab Linux sisse logida](virtual-machines-linux-mac-create-ssh-keys.md).

Azure'i kinnitatud Linux distros on sellele Azure Linux Agent pakkimine oma hoidlate nii kontrollige ja installige uusim versioon selle distributsiooni hoidla esmalt võimalusel.  

Ubuntu, lihtsalt tüüp:

    #sudo apt-get install walinuxagent

Ja CentOS, tippige:

    #sudo yum install waagent


Oracle'i Linuxi, veenduge, et selle `Addons` hoidla on lubatud. Faili redigeerimiseks valige `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) või `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), ja muuta rida `enabled=0` et `enabled=1` **[ol6_addons]** või **[ol7_addons]** selle faili.

Azure'i Linux Agent uusima versiooni installimiseks tippige:


    #sudo yum install WALinuxAgent

Kui te ei leia lisandmooduli hoidla saate lihtsalt lisada need read .repo faili lõpus vastavalt oma Oracle'i Linux väljaanne:

Oracle'i Linux 6 virtuaalmasinates: jaoks

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Oracle'i Linux 7 virtuaalmasinates: jaoks

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Tippige:

    #sudo yum update WALinuxAgent

Tavaliselt on see kõik peate, kuid kui mingil põhjusel peate selle installida https://github.com otse, siis tehke järgmist.


## <a name="install-wget"></a>Installige wget

Logige sisse oma VM SSH abil.

Installige wget (on mõned distros, mida ei installi vaikimisi, nt Redhat, CentOS ja Oracle'i Linuxi versioonide 6.4 ja 6.5), tippides `#sudo yum install wget` käsureal.


## <a name="download-the-latest-version"></a>Laadige alla uusim versioon

Avage [Azure'i Linux Agent GitHub versiooni](https://github.com/Azure/WALinuxAgent/releases) veebilehele, ja teada uusima versiooni number. (Oma praeguse versiooni leidmist, tippides `#waagent --version`.)

### <a name="for-version-20x-type"></a>Versiooni 2.0.x, tüüp:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Järgmine rida kasutab versiooni 2.0.14, nagu näiteks:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Versiooni 2.1.x või uuemat versiooni, tippige:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Järgmine rida kasutab versioon 2.1.0, nagu näiteks:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Installige Azure'i Linux Agent

### <a name="for-version-20x-use"></a>Versiooni 2.0.x kasutamine:

 Veenduge waagent käivitatava.

    #chmod +x waagent

 Kopeerige uus käivitatava /usr/sbin /.

  Enamiku Linux, kasutage.

    #sudo cp waagent /usr/sbin

  CoreOS, kasutage:

    #sudo cp waagent /usr/share/oem/bin/

  Kui tegemist on uue installi Azure Linux Agent, käivitage:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Versiooni 2.1.x kasutamine:

Võib-olla peate installima paketi `setuptools` esimesena - leiate [siit](https://pypi.python.org/pypi/setuptools). Käivitage:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Taaskäivitage waagent teenus

Enamik Linuxi Distros:

    #sudo service waagent restart

Ubuntu, kasutage.

    #sudo service walinuxagent restart

CoreOS, kasutage:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Kinnitage Azure Linux Agent versioon

    #waagent -version

CoreOS, ei pruugi eeltoodud käsu.

Näete, et uus versioon on värskendatud Azure Linux Agent versioon.

Azure Linux Agent kohta lisateabe saamiseks lugege teemat [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).
