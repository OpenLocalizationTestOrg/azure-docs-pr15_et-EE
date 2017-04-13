<properties 
    pageTitle="Kuidas installida Apache Qpid prootonpumba-C Linux VM | Microsoft Azure'i"
    description="Kuidas luua CentOS Linux VM abil Azure'i Virtuaalmasinates ja kuidas luua ja installida Apache Qpid prootonpumba-C teek."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Installige Azure'i Linux VM Apache Qpid prootonpumba-C

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Selles jaotises kirjeldatakse, kuidas luua CentOS Linux VM abil Azure'i Virtuaalmasinates ja kuidas alla laadida, luua ja installida Apache Qpid prootonpumba-C teegis koos Python ja PHP keele seosed. Pärast neid juhiseid, saab selle juhendi Python ja PHP valimite käivitamiseks.

Kõigepealt sooritatakse [Azure klassikaline portaalis][]. Järgmisel kuvatõmmisel on kuvatud, kuidas CentOS VM nimega "scott centos" on loodud.

![Klõpsake Azure'i Linux VM prootonpumba][0]

Pärast ettevalmistamise, kuvatakse järgmised portaalis

![Klõpsake Azure'i Linux VM prootonpumba][1]

Logige arvutisse, peate teadma pordi lõpp-punkti jaoks SSH. Saate hankida selle väärtuse [Azure klassikaline portaalis][] valige äsja loodud VM ja klõpsake vahekaardil **lõpp-punktid** . Järgmisel kuvatõmmisel näitab avaliku SSH pordi selle arvuti 57146.

![Klõpsake Azure'i Linux VM prootonpumba][2]

Nüüd saate luua ühenduse arvuti SSH abil. Selles näites kasutatakse kitt tööriist, nagu järgmine pilt:

![Klõpsake Azure'i Linux VM prootonpumba][3]

Selles näites kasutatakse Python ja PHP rakenduste prootonpumba kliendi teegid Apache. Need teegid on [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)allalaadimiseks saadaval. Jaotuse pakkimine Readme-faili selgitatakse installima sõltuvused ja koostada prootonpumba etapid. Siit leiate ülevaate juhiseid:

1.  Yum config faili redigeerida (/ etc/yum.conf) ja kommentaari, välja arvatud värskenduste tuum päised (\# välistamine = tuum\*). See on vaja installida gcc koostaja.

2.  Eelnevalt nõutud pakettide installimiseks tehke järgmist.

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Prootonpumba teegi allalaadimiseks tehke järgmist.

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Jaotuse arhiivi prootonpumba koodi ekstraktimiseks tehke järgmist.

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Koostamine ja installige koodi, täitke järgmised juhised, mis on võetud Readme-faili.

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Pärast nende toimingute prooton on arvutisse installitud ja kasutamiseks valmis.

## <a name="next-steps"></a>Järgmised sammud

Kas olete valmis saada lisateavet? Külastage järgmist linki:

- [Teenuse siini AMQP ülevaade][]

[Teenuse siini AMQP ülevaade]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure'i klassikaline portaal]: http://manage.windowsazure.com


