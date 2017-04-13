<properties
    pageTitle="Azure'i virnas Linux külalistele | Microsoft Azure'i"
    description="Siit saate teada, kuidas luua Linux-põhine virtuaalmasinates Azure'i virnas."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Linux virtuaalmasinates kohta Azure virnas juurutamine

Juurutamist Linux virtuaalmasinates klõpsake Azure virnas POC üheks Azure'i virnas turuplatsi Linuxi-põhiste piltide lisamise teel. Mitme Linuxi müüjad on esitatud pilte, mis on Azure virnas POC saab lisada või luua oma.

## <a name="download-an-image"></a>Pildi allalaadimiseks

 1. Laadige alla või pildi Azure virnas ühilduv ekstraktida järgmisi linke, ettevalmistamine oma:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Eraldamiseks pilti VHD vajadusel ja [kuvatakse Marketplace'ist pildi lisada](azure-stack-add-vm-image.md). Veenduge, et selle `OSType` parameeter on seatud `Linux`.
 
 3. Pärast pildi lisamist kuvatakse Marketplace'ist, turuplatsi üksus on loodud ja Linux virtuaalse masina juurutamist.
  
## <a name="prepare-your-own-image"></a>Oma pildi ettevalmistamine

1. Ettevalmistused oma Linux pilt, kasutades ühte järgmist:
 - [CentOS vastavalt üldiste müügijaotustega tutvumine](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle'i Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Punane rolli Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES ja openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Laadige alla ja installige [Azure'i Linux Agent](https://github.com/Azure/WALinuxAgent/)

    Azure'i Linux Agent versioon 2.1.3 või suurem on vaja oma Linux VM Azure virnas ette valmistada. Jaotuse, ülalpool juba sisaldavad selle versiooni agent või suurem paketina nende hoidlate (tavaliselt `WALinuxAgent` või `walinuxagent`). Juhul, kui Azure agent paketi versioon on väiksem kui 2.1.3 (st 2.0.18 või madalam), ja seejärel installige agent käsitsi. Installitud versiooni saate kindlaks määrata paketi nimi või käivitades `/usr/sbin/waagent -version` VM.

    Järgige alltoodud juhiseid, et installida Azure Linux agent käsitsi-

 - Esmalt Laadige alla uusim Azure Linux agent kaudu [Github](https://github.com/Azure/WALinuxAgent/releases), näiteks:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Azure'i agendi paki:

            # tar -vzxf v2.2.0.tar.gz

 - Installige python-setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Installige Azure'i agent.

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Süsteemide Python 2.x- ja Python 3.x installitud-kõrvuti võimalik, et peate käivitage järgmine käsk:

        # sudo python3 setup.py install --register-service

    Lisateavet leiate teemast Azure Linux Agent [seletusfail](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Lisa pilt kuvatakse Marketplace'ist](azure-stack-add-vm-image.md). Veenduge, et selle `OSType` parameeter on seatud `Linux`.

4. Pärast pildi lisamist kuvatakse Marketplace'ist, turuplatsi üksus on loodud ja Linux virtuaalse masina juurutamist.

## <a name="next-steps"></a>Järgmised sammud

[Korduma kippuvad küsimused Azure'i virnas](azure-stack-faq.md)