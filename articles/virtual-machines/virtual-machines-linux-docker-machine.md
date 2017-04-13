<properties
    pageTitle="Keskmise suurusega hosts loomine Azure keskmise suurusega masina | Microsoft Azure'i"
    description="Kirjeldatakse keskmise suurusega masina loomiseks keskmise suurusega hosts Azure kasutamine."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Keskmise suurusega seadme abil Azure draiver

[Keskmise suurusega](https://www.docker.com/) on üks kõige populaarsemate virtualization lähenemisel, mis kasutab Linux ümbriste asemel virtuaalmasinates viis rakenduse andmete eraldamiseks ja arvuti ühiskasutusega ressursid. Selles teemas kirjeldatakse, millal ja kuidas kasutada [Keskmise suurusega masina](https://docs.docker.com/machine/) (selle `docker-machine` käsk) luua uue Linuxi VMs Azure keskmise suurusega hostiks oma ümbriste Linuxi jaoks lubatud.


## <a name="create-vms-with-docker-machine"></a>Keskmise suurusega masina VMs loomine

Luua keskmise suurusega host VMs Azure koos selle `docker-machine create` käsk abil soovitud `azure` draiver suvand draiveri argumendi (`-d`) ja muid argumente. 

Järgmises näites tugineb vaikeväärtused, kuid see avada Interneti test nginx container, teeb pordi 80 VM `ops` sisselogimise kasutaja SSH ja kõnede uue VM `machine`. 

Tippige `docker-machine create --driver azure` suvandid ja vaikeväärtused; Samuti võite lugeda [keskmise suurusega Azure'i dokumentatsioonist](https://docs.docker.com/machine/drivers/azure/). (Pange tähele, et kui teil on lubatud kahekordne autentimine, küsitakse autentida teise teguri).

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Väljund peaks välja nägema umbes selline, sõltuvalt sellest, kas teil on teie konto on konfigureeritud kahekordne autentimine.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Oma keskmise suurusega shell konfigureerimine

Nüüd tippige `docker-machine env <VM name>` näha, mida peate tegema selleks, et konfigureerida shell. 

```bash
docker-machine env machine
```

Mis prinditakse keskkonna teave, mis näeb välja umbes selline. Pange tähele, et IP-aadress on määratud, mis on teil vaja testimiseks VM.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Kas käivitada käsk soovitatud konfiguratsiooni või saate määrata keskkonna muutujate ise. 

## <a name="run-a-container"></a>Ümbris käivitamine

Nüüd saate käivitada lihtsa veebiserverisse testimiseks kas kõik töötab õigesti. Siin me standard nginx pildi kasutamine, saate määrata, kas see peaks kuulata porti 80 ja, et kui VM taaskäivitamist ümbris peaks taaskäivitage ka (`--restart=always`). 

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

Ja kontrollige töötava ümbris, tippige `docker-machine ip <VM name>` leida IP-aadress (kui olete unustanud kaudu soovitud `env` käsk):

![Töötava ngnix container](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Järgmised sammud

Kui olete huvitatud, võite proovida välja Azure [Keskmise suurusega VM laiend](virtual-machines-linux-dockerextension.md) Azure'i CLI või Azure ressursi halduri mallide abil sama toimingu tegemiseks. 

Veel näiteid keskmise suurusega töötamine, vt [töötamine keskmise suurusega](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 ühendamine [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Lisateavet quickstarts HealthClinic.biz demo, leiate [Azure'i Arendaja tööriistad Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

