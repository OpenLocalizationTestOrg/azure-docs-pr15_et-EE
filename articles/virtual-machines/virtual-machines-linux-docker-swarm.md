<properties
   pageTitle="Keskmise suurusega ja Azure sülem kasutamise alustamine"
   description="Kirjeldatakse, kuidas luua rühma vms VM laiendiga keskmise suurusega ja sülem abil saate luua keskmise suurusega kobar."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Kuidas kasutada keskmise suurusega sülem

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Selles teemas kirjeldatakse väga lihtne viis kasutada [keskmise suurusega](https://www.docker.com/) [sülem](https://github.com/docker/swarm) luua sülem haldusega kobar Azure. See loob neli virtuaalmasinates Azure, üks tegutseda sülem ülemus ja kolme kobar keskmise suurusega hosts osana. Kui olete lõpetanud, saate vaadata klaster ning seejärel hakata keskmise suurusega kasutamine sülem. Lisaks Azure CLI kõned selles teemas kasutada teenuse juhtimist (asm). 

> [AZURE.NOTE] Selles teemas kasutatakse keskmise suurusega sülem ja selle Azure'i CLI *ilma* **keskmise suurusega-seadme** kasutamine selleks, et kuvada, kuidas erinevate tööriistad koos töötada, kuid jäävad sõltumatu. **keskmise suurusega-arvuti** on **--sülem** parameetrid, mis võimaldavad lisada otse sõlmed sülem **keskmise suurusega-seadme** abil. Näiteks vt [keskmise suurusega-seadme](https://github.com/docker/machine) dokumentatsiooni. Kui te vastamata **keskmise suurusega-seadme** töötab Azure VMs suhtes, lugege teemat [Azure keskmise suurusega-seadme kasutamine](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Looge keskmise suurusega hosts Azure'i Virtuaalmasinates

Selles teemas loob neli VMs, kuid saate kasutada mis tahes arv, mida soovite. Kõne järgmised * &lt;parooli&gt; * asendatud valitud parool.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Kui olete lõpetanud, peaks oskama **Azure'i vm loendi** abil saate vaadata oma Azure VMs:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Installimise sülem sülem juhtslaidi VM

Selles teemas kasutab funktsiooni [container mudeli installi soovitud keskmise suurusega sülem dokumentatsiooni](https://github.com/docker/swarm#1---docker-image) --, kuid võite ka SSH **sülem-juhtslaidi**. Selles mudelis **sülem** on alla keskmise suurusega container, kus töötab sülem nimega. Allpool me läbi selle juhise *kaugühenduse teel, kasutades keskmise suurusega sülearvuti kaudu* ühenduse **sülem-juhtslaidi** VM ja sellel kobar id loomine käsu **sülem loomine**. Kobar-ID-d on, kuidas **sülem** leiab sülem rühma liikmeid. (Saate klooni hoidla ja koostada seda ise, mis annab teile Täielik kasutusõigus ja lubada silumine.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Et viimane rida on kobar-id; Kopeerige see kuhugi, kuna te kasutate selle uuesti, kui liitute sõlm VMs sülem juhtslaidi loomine "sülem". Selles näites on kobar id **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Lihtsalt, et oleks selge, kasutame meie kohaliku keskmise suurusega installi **sülem-juhtslaidi** VM Azure ja juhendamine **sülem-juhtslaidi** allalaadimine, installimine ja käivitamine **loomine** käsk, mis annab meie kobar-id, mida me kasutame discovery eesmärgil hiljem ühenduse.
<!-- -->
> Kontrollimaks, käivitage `docker -H tcp://` * &lt;hostname&gt; * ` images` loetleda container protsessid arvutis **sülem-juhtslaidi** ja teise sõlme võrdlus (Kuna me parandusfunktsiooni eelmise sülem käsk koos parameetriga **– rm** ümbris on eemaldatud, kui see on lõpule jõudnud, kasutab **keskmise suurusega ps - lisamine** ei tagasta midagi).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Kui olete tuttav **keskmise suurusega**, teate, et teisi sõlmi on kirjeid, sest pole pilte on alla laadida ja käivitada veel.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Liituda sõlm VMs meie keskmise suurusega kobar

Loetle iga sõlme abil Azure'i CLI lõpp-punkti teave. Allpool seda **sülem-sõlm-1** keskmise suurusega hosti jaoks soovitud sõlm keskmise suurusega pordi saamiseks.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Kasutades **keskmise suurusega** ja `-H` suvand, et teie sõlm VM, siis osutage keskmise suurusega kliendi liituda sõlme loomisel, läbides kobar-id ja selle sõlm keskmise suurusega pordi sülem (viimase **saatja-aadressi**abil):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Mis näeb välja hea. Kinnitamaks, et **sülem** töötab **sülem-sõlm-1** me tüüp:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Korrake teiste sõlme klaster. Meie puhul seda **sülem-sõlm-2** ja **3/sülem-sõlm**.

## <a name="begin-managing-the-swarm-cluster"></a>Alustage sülem kobar haldamine

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

ja seejärel saate loetleda välja oma sõlmed klaster sisse.

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Avage oma sülem asju käitamist. Teemast otsimiseks inspiratsiooni [https://github.com/docker/swarm/](https://github.com/docker/swarm/)või võib-olla [video](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
