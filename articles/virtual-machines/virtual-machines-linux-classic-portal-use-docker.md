<properties
    pageTitle="Keskmise suurusega VM laiend kasutamise Linux | Microsoft Azure'i"
    description="Kirjeldatakse keskmise suurusega ja Azure'i Virtuaalmasinates laiendid ja kuidas luua Azure'i Virtuaalmasinates, mis on keskmise suurusega hosts Azure'i CLI klassikaline juurutamise mudeli kasutamine."
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
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Keskmise suurusega VM laiend Azure klassikaline portaali kasutamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Keskmise suurusega](https://www.docker.com/) on üks kõige populaarsemate virtualization lähenemisel, mis kasutab [Linux ümbriste](http://en.wikipedia.org/wiki/LXC) asemel virtuaalmasinates viis andmete eraldamiseks ja arvuti ühiskasutusega ressursid. Keskmise suurusega VM laiend, haldab [Azure Linux Agent] abil saate luua keskmise suurusega VM, mis hostib suvalist arvu ümbriste Azure rakendusi.

> [AZURE.NOTE] Selles teemas kirjeldatakse, kuidas luua keskmise suurusega VM Azure klassikaline portaalist. Keskmise suurusega VM käsurida loomise kohta vt teemast [keskmise suurusega VM laiend: Azure'i käsurea liides (Azure'i CLI) kasutamise kohta]. Ümbriste ja eelised üksikasjalik arutelu, leiate artiklist [Keskmise suurusega kõrge taseme tahvel](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Luua uue VM Pilt galeriist
Kõigepealt jaoks on vaja mõnda Azure VM Linux pildilt, mis toetab keskmise suurusega VM laiend, kasutades Ubuntu 14.04 LTS pilt Pildigalerii pildi näide server ja Ubuntu 14.04 töölaua klient. Portaalis valige **+ Uus** alumises vasakus nurgas luua uue eksemplari VM ja valige pildi Ubuntu 14.04 LTS saadaolevate valikute või täielik Pildigalerii pilt, nagu allpool näidatud.

> [AZURE.NOTE] Ainult Ubuntu 14.04 LTS piltide uuem kui juuli 2014 toetus keskmise suurusega VM laiend.

![Looge uus Ubuntu pilt](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Keskmise suurusega sertide loomine

Pärast VM on loodud, siis veenduge, et keskmise suurusega on installitud klientarvutis. (Lisateabe saamiseks vt [keskmise suurusega installimisjuhiseid](https://docs.docker.com/installation/#installation).)

Keskmise suurusega suhtlemiseks vastavalt [Töötab keskmise suurusega ja HTTPS-i] sert ja võti failide loomine ja paigutage need on **`~/.docker`** kataloog klientarvutis.

> [AZURE.NOTE] Keskmise suurusega VM laiend portaalis praegu nõuab kasutajaandmeid, mis on kodeeritud base64.

Käsurida, kasutage **`base64`** või mõnda muud lemmik kodeering tööriista base64-kodeeringuga teemade loomiseks. Sellega on lihtsate sert ja võti faile, mis võib välja umbes järgmine:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Keskmise suurusega VM laiend lisamine
Keskmise suurusega VM laiend lisamiseks leidke VM eksemplari, mis on loodud ja liikuge kerides allapoole jaotiseni **laiendid** ja klõpsake seda, et tuua esile VM laiendid, nagu allpool näidatud.
> [AZURE.NOTE] Seda funktsiooni toetatakse ainult eelvaade portaalis: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Lisage laiend
Klõpsake nuppu **+ Lisa** võimalike VM laiendid, saate lisada selle VM kuvamiseks.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Valige keskmise suurusega VM laiend
Valige keskmise suurusega VM laiend, mis avab keskmise suurusega kirjelduse ja olulised lingid, ja klõpsake nuppu **Loo** allosas alustamiseks installimise juhiseid.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Sert ja võti failide lisamiseks tehke järgmist.

Vormi väljad, sisestage base64-kodeeringuga versioonide oma CA sert, oma serveri serti ja tootenumbri Server, nagu on näidatud järgmisel joonisel.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Pange tähele, et (nagu eelmisel pilti) soovitud 2376 täidetakse vaikimisi. Mis tahes lõpp-punkti siia saate sisestada, kuid järgmise juhise juurde saab avada vastavat lõpp-punkti. Kui muudate vaikimisi, veenduge, et avada järgmise juhise kattuvad lõpp-punkti.

## <a name="add-the-docker-communication-endpoint"></a>Keskmise suurusega side lõpp-punkti lisamine
Olete loonud ressursirühma kuvamisel valige seostatud teie VM võrgu turberühma ja käsku **Turvalisus sissetulevad reeglid** reegleid kuvamiseks, nagu on näidatud siin.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Klõpsake nuppu **+ Lisa** lisada teise reegli ja vaikimisi juhul sisestage (näites **keskmise suurusega**) lõpp-punkti nimi ja 2376 'sihtkoha Port vahemikus. Protocol (protokoll) väärtus, millel on kujutatud **TCP**ja klõpsake nuppu **OK** , et luua reegli.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Testige oma kliendi keskmise suurusega ja Azure keskmise suurusega Host
Leidke ja kopeerige nimi oma VM domeeni ja teie klientarvuti, tippige käsurida `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (kus *dockerextension* asendatakse alamdomeen oma VM).

Tulem kuvatakse umbes järgmine:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Kui olete eespool kirjeldatud toimingud, on teil on nüüd täielikult toimiv keskmise suurusega host töötab Azure VM, mis eemalt ühendada teiste klientide konfigureeritud.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Olete valmis [Keskmise suurusega kasutusjuhendi] avada ja kasutada oma keskmise suurusega VM. Kui soovite automatiseerida loomise keskmise suurusega hosts Azure VMs kaudu käsurea liides, vaadake, [Kuidas kasutada keskmise suurusega VM laiend: Azure'i käsurea liides (Azure'i CLI)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Kuidas kasutada keskmise suurusega VM laiend: Azure'i käsurea liides (Azure'i CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure'i Linux Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Keskmise suurusega töötab https-iga]: http://docs.docker.com/articles/https/
[Keskmise suurusega kasutusjuhend]: https://docs.docker.com/userguide/
