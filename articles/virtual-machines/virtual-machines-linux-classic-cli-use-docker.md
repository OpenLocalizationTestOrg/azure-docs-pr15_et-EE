<properties
    pageTitle="Keskmise suurusega VM laiend Linuxi Azure abil"
    description="Keskmise suurusega ja Azure'i Virtuaalmasinates laiendid kirjeldatakse ja näidatakse, kuidas luua programmiliselt Virtuaalmasinates Azure'i, mis on keskmise suurusega hostide kaudu Azure'i CLI käsurea kaudu."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Keskmise suurusega VM laiend, käsurea Azure abil kasutajaliides (Azure'i CLI)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



Selles teemas kirjeldatakse, kuidas luua teenuse haldus (asm) režiimi Azure'i CLI mis tahes platvormi kaudu laiendiga keskmise suurusega VM VM. [Keskmise suurusega](https://www.docker.com/) on üks kõige populaarsemate virtualization lähenemisel, mis kasutab [Linux ümbriste](http://en.wikipedia.org/wiki/LXC) asemel virtuaalmasinates viis andmete eraldamiseks ja arvuti ühiskasutusega ressursid. Keskmise suurusega VM laiend ja [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) abil saate luua keskmise suurusega VM, mis hostib suvalist arvu ümbriste Azure rakendusi. Ümbriste ja eelised üksikasjalik arutelu, leiate artiklist [Keskmise suurusega kõrge taseme tahvel](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Kuidas kasutada Azure keskmise suurusega VM laiend
Keskmise suurusega VM laiend Azure'i kasutamiseks tuleb teil installida [Azure käsurea kasutajaliidese](https://github.com/Azure/azure-sdk-tools-xplat) (Azure'i CLI) suurem kui 0.8.6 versiooni (nagu kirjutamise praegune versioon on 0.10.0). Saate installida Mac, Linux ja Windows Azure'i CLI.


Keskmise suurusega kasutamine Azure täieliku protsess on lihtne:

+ Installige Azure'i CLI ja sõltuvustega, millest soovite juhtelemendi Azure'i arvuti (Windows, seda saab Linux jaotuse, kui virtuaalse masina)
+ Azure'i CLI keskmise suurusega käskude abil saate luua VM keskmise suurusega host Azure
+ Kohaliku keskmise suurusega käskude abil saate hallata oma keskmise suurusega ümbriste sisse oma keskmise suurusega VM Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Installimine on Azure käsurea liides (Azure'i CLI)

Installida ja konfigureerida Azure CLI, vaadake, [Kuidas installida Azure käsurea liides](../xplat-cli-install.md). Installimise kinnitamiseks tippige `azure` käsuviibale ja näete Azure'i CLI ASCII art lühike hetke pärast, mis on loetletud põhilised käsud saadaval. Kui installimine töötanud õigesti, peaks olema võimalus Sisestage `azure help vm` ja kas üks loetletud käsud on "keskmise suurusega".

> [AZURE.NOTE] Keskmise suurusega on tööriistad Windowsi [Keskmise suurusega masina](https://docs.docker.com/installation/windows/), mida saate kasutada ka keskmise suurusega klient, mille abil saate töötada Azure VMs nagu keskmise suurusega hosts loomise automatiseerida.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Azure'i CLI kontoga ühenduse loomiseks Azure
Azure'i CLI kasutamiseks peab Azure'i konto identimisteave seostada Azure'i CLI platvormile. [Ühendamiseks Azure tellimuse](../xplat-cli-connect.md) jaotis selgitab, kuidas alla laadida ja oma **.publishsettings** faili importimine või ettevõtte id oma Azure'i CLI seostada.

> [AZURE.NOTE] Kui kasutate ühte või teiste meetodite autentimine, et olla kindel, et lugeda ülaltoodud mõista erinevaid funktsioone dokumendi on mõned erinevused käitumist.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Keskmise suurusega installida ja kasutada Azure keskmise suurusega VM laiend
Järgige [installimisjuhiseid keskmise suurusega](https://docs.docker.com/installation/#installation) keskmise suurusega kohalikult oma arvutisse installida.

Keskmise suurusega ja on Azure virtuaalse masina kasutamiseks Linux pilti kasutada VM peab olema installitud [Azure Linux VM Agent](virtual-machines-linux-agent-user-guide.md) . Praegu on ainult kahte tüüpi pilte, mis pakuvad järgmine:

+ Ubuntu Azure'i Pildigalerii pilt või

+ Azure'i Linux VM agendi loodud kohandatud Linux pildi installinud ja konfigureerinud. Lisateavet selle kohta, kuidas luua kohandatud Linux VM Azure VM agendi teemast [Azure Linux VM Agent](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Azure'i pildi galerii abil

Bash või Terminal seansi, kasutage järgmist Azure'i CLI käsku leidmiseks kasutada, tippides galeriis VM Ubuntu pilt

`azure vm image list | grep Ubuntu-14_04`

ja valige üks pilt nimed, nagu `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, ja järgmise käsu abil saate luua uue VM abil selle pildi.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

kui:

+ * &lt;vm cloudservice nimi&gt; * on nimi, mis muutub keskmise suurusega container hosti arvuti Azure VM

+  * &lt;kasutajanimi&gt; * on vaikimisi root kasutaja VM kasutajanimi

+ * &lt;parooli&gt; * täidab keerukuse Azure *kasutajanime* konto parool on

> [AZURE.NOTE] Praegu parooli peab olema vähemalt 8 märgid, sisaldavad ühe väiketähti ja suurtähed ühe märgi, arv ja erimärgi näiteks üks järgmistest märkidest: `!@#$%^&+=`. Ei, ei ole perioodi lõpus eelmise lause erimärgi.

Kui käsk õnnestus, peaksite nägema midagi, olenevalt täpne argumendid ja suvandid, mida kasutasite järgmist:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Luua virtuaalse masina võib kuluda mõni minut, kuid pärast seda on ette valmistatud (riik väärtus on `ReadyRole`) käivitab keskmise suurusega daemon (keskmise suurusega teenus) ja keskmise suurusega container host saate luua ühenduse.

Keskmise suurusega VM testimiseks loomist Azure type

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

Kui * &lt;vm-nime--kasutasite&gt; * on kasutatud ümbersuunamine virtuaalse masina nimi `azure vm docker create`. Peaksite nägema midagi sarnast järgmist teavet, mis näitab, et teie keskmise suurusega Host VM üles ja Azure töötab ja ootab teie käske. 

Nüüd võite proovida ühenduse loomiseks kasutada oma keskmise suurusega kliendi teavet (mõned keskmise suurusega kliendi seadistuse, näiteks Mac-arvutisse, peate kasutama `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Ainult, et olla kindel, et see on kõik töötamise, saate uurida VM keskmise suurusega pikendamise eest:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Keskmise suurusega Host VM autentimine

Lisaks keskmise suurusega VM, luua selle `azure vm docker create` käsk loob automaatselt ka keskmise suurusega klientarvuti Azure container Host HTTPS-i abil ühenduse lubamiseks vajalikud serdid ja serdid on talletatud klient ja host arvutites vastavalt vajadusele. Edasiste katsete, olemasoleva serdid on uuesti kasutada ja ühiskasutusse uue host.

Vaikimisi paigutatakse serdid `~/.docker`, ja keskmise suurusega konfigureeritakse pordi **2376**käivitada. Kui soovite kasutada eri pordi või kataloogist, siis võite kasutada ühte järgmistest `azure vm docker create` Käsurea võtmed konfigureerida oma keskmise suurusega container majutada VM kasutada erinevat porti või muu serdid ühenduse kliendid:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Keskmise suurusega daemon host on konfigureeritud Kuulake ja autentida kliendi ühendused määratud Port abil loodud serdid on `azure vm docker create` käsk. Kliendi seadme peab olema juurde pääseda keskmise suurusega host need serdid.

> [AZURE.NOTE] Võrku ühendatud host töötab ilma need serdid võidakse kellelegi, et saate ühenduse loomiseks arvuti. Enne muuta vaikimisi konfiguratsiooni, veenduge, et aru, riskid arvutite ja rakenduste.

## <a name="next-steps"></a>Järgmised sammud

* Olete valmis [Keskmise suurusega kasutusjuhendi] avada ja kasutada oma keskmise suurusega VM. Luua uue portaalis keskmise suurusega lubatud VM, vaadake, [Kuidas kasutada keskmise suurusega VM laiend portaalis].

* Azure keskmise suurusega VM laiend toetab samuti keskmise suurusega koostamine, mis kasutab kasutage arendaja modelleerida rakenduse mis tahes keskkonna ja luua ühtsete juurutamise deklaratiivseid YAML fail. Vt [Keskmise suurusega ja koostamine määratlemine ja Azure virtuaalne arvutis rakendus mitme container kasutamise alustamine].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Kuidas kasutada portaali keskmise suurusega VM laiend]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Keskmise suurusega kasutusjuhend]: https://docs.docker.com/userguide/
 
[Keskmise suurusega ja koostamine määratlemine ja käivitada mitme container Azure virtuaalne arvutis kasutamise alustamine]:virtual-machines-linux-docker-compose-quickstart.md