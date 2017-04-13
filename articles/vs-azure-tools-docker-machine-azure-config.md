<properties
   pageTitle="Keskmise suurusega hosts loomine Azure keskmise suurusega masina | Microsoft Azure'i"
   description="Kirjeldatakse keskmise suurusega masina loomiseks keskmise suurusega hosts Azure kasutamine."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Keskmise suurusega Hosts Azure ja keskmise suurusega-seadme loomine

[Keskmise suurusega](https://www.docker.com/) ümbriste töötab nõuab host töötab keskmise suurusega daemon VM.
Selles teemas kirjeldatakse, kuidas [keskmise suurusega-masina](https://docs.docker.com/machine/) käsu abil saate luua uue Linuxi VMs, keskmise suurusega deemon, töötavad Azure konfigureeritud. 

**Märkus:** 
- *Selles artiklis sõltub keskmise suurusega-arvuti versioon 0.7.0 või suurem*
- *Windowsi ümbriste toetatakse keskmise suurusega-seadme kaudu tulevikus*

## <a name="create-vms-with-docker-machine"></a>Keskmise suurusega masina VMs loomine

Luua keskmise suurusega host VMs Azure koos selle `docker-machine create` käsk abil soovitud `azure` draiver. 

Azure'i draiver peate oma tellimuse ID-ga. Saate tuua tellimuse Azure'i [Azure CLI](xplat-cli-install.md) või [Azure portaali](https://portal.azure.com) . 

**Azure'i portaalis**
- Valige vasakpoolsel navigeerimisribal lehelt tellimused ja kopeerige tellimuse id.

**Azure'i CLI abil**
- Tippige ```azure account list``` ja kopeerige Tellimuse ID-d.

Tippige `docker-machine create --driver azure` suvandid ja vaikeväärtused kuvamiseks.
Näete [keskmise suurusega Azure'i draiveri dokumentatsiooni](https://docs.docker.com/machine/drivers/azure/) kohta lisateabe saamiseks. 

Järgmises näites tugineb vaikeväärtused, kuid soovi korral seda avada pordi 80 VM Interneti-ühenduse. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Keskmise suurusega host ja keskmise suurusega-seadme valimine
Kui olete oma pakkuja keskmise suurusega-seadme kirjet, saate määrata vaikimisi host keskmise suurusega käsud.
##<a name="using-powershell"></a>PowerShelli kasutamine

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Bash abil

```bash
eval $(docker-machine env MyDockerHost)
```

Nüüd saate keskmise suurusega käsud vastu määratud host

```
docker ps
docker info
```

## <a name="run-a-container"></a>Ümbris käivitamine

Juures, mis on konfigureeritud, nüüd saate lihtsa veebiserverisse testimiseks oma hosti on õigesti konfigureeritud.
Siin me standard nginx pildi kasutamine, saate määrata, et see peaks kuulata porti 80 ja, et kui hosti VM taaskäivitamist ümbris on samuti taaskäivitage (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Väljund peaks välja nägema umbes järgmine:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Ümbris testimine

Töötava ümbriste abil uurida `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Ja kontrollige töötava ümbris, tippige `docker-machine ip <VM name>` IP-aadressi sisestamine brauseris leidmiseks:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Töötava ngnix container](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Kokkuvõte
Keskmise suurusega-seadme abil saate hõlpsalt keskmise suurusega hosts Azure jaoks oma üksikuid keskmise suurusega host valideerimised ette.
Tootmise hosting ümbriste, leiate [Azure'i teenus](http://aka.ms/AzureContainerService)

Visual Studio .NET Core rakenduste arendamise, lugege teemat [Keskmise suurusega Tools for Visual Studio](http://aka.ms/DockerToolsForVS)
