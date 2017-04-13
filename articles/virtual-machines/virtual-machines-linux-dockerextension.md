<properties
   pageTitle="Azure keskmise suurusega VM laiend abil | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada keskmise suurusega VM laiend juurutamiseks kiiresti ja turvaliselt keskmise suurusega keskkonna Azure ressursihaldur mallide kasutamine."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Keskmise suurusega keskkonna loomine Azure abil keskmise suurusega VM laiend
Keskmise suurusega on levinud container haldus ja kujutise platvormi, mis võimaldab teil kiiresti töötada ümbriste Linux (ja ka Windowsi). Azure'is, on mitmel viisil juurutamist keskmise suurusega vastavalt oma vajadustele. See artikkel keskendub keskmise suurusega VM laiend ja Azure ressursihaldur mallide abil. 

Lisateavet erinevate juurutamise meetodid, kasutades keskmise suurusega masina ja Azure Container teenuseid, sh kohta leiate järgmistest artiklitest:

- Kiiresti prototüübile rakendus, saate luua ühe keskmise suurusega host [Keskmise suurusega seadme](./virtual-machines-linux-docker-machine.md)abil.
- Suurema ja veel ühed keskkonnas, saate kasutada Azure keskmise suurusega VM laiend, mis toetab ka [Keskmise suurusega koostamine](https://docs.docker.com/compose/overview/) ühtsete container juurutuste loomiseks. See artikkel üksikasjad Azure keskmise suurusega VM laiend abil.
- Koostamiseks tootmise valmis, scalable keskkonnas, mis annavad täiendavat plaanimis- ja tööriistad, saate juurutada [keskmise suurusega sülem kobar Azure'i Container Services](../container-service/container-service-deployment.md).


## <a name="azure-docker-vm-extension-overview"></a>Azure'i keskmise suurusega VM laiend ülevaade
Azure keskmise suurusega VM laiend installida ja konfigureerida keskmise suurusega daemon, keskmise suurusega klient ja keskmise suurusega Koostage oma Linux virtual kohapeal (VM). Azure keskmise suurusega VM laiendamise abil on teil veel juhtelemendi ja funktsioone kui lihtsalt keskmise suurusega seadme abil või keskmise suurusega host ise luua. Järgmisi lisavõimalusi, näiteks [Keskmise suurusega koostamine](https://docs.docker.com/compose/overview/), teha sobib senisest käepärasemad arendaja või tootmise keskkondades Azure keskmise suurusega VM laiendamine.

Azure'i ressursihaldur Mallid määratleda struktuuri, mis teie keskkonnas. Mallid võimaldavad loomine ja konfigureerimine nagu keskmise suurusega host VMs, salvestusruumi, Rollipõhine juurdepääsu kontroll (RBAC) ja diagnostika. Saate kasutada neid malle luua täiendavaid juurutuste sobival viisil. Azure'i ressursihaldur ja mallide kohta leiate lisateavet teemast [ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Malli laiendiga Azure keskmise suurusega VM juurutamine
Kasutame Ubuntu VM, mis kasutab Azure keskmise suurusega VM laiend installida ja konfigureerida keskmise suurusega host loomiseks olemasoleva Kiirjuhend malli. Saate vaadata siin mall: [lihtsa kasutuselevõtu mõne Ubuntu VM ja keskmise suurusega](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Teil on vaja [Uusim Azure CLI](../xplat-cli-install.md) installitud ja sisse loginud kasutades režiimi ressursihaldur järgmiselt:

```
azure config mode arm
```

Malli Azure'i CLI, täpsustades URI malli abil. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUS` asukoht. Kasutage oma ressursi rühma nime ja asukoha määramist järgmiselt:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Vastake viipasid salvestusruumi konto nime, kasutajanime ja parooli ja DNS-i nimi. Väljund sarnaneb järgmises näites:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Annab vastuseks Azure'i CLI küsimuse pärast ainult mõne sekundi, kuid teie keskmise suurusega host on ikkagi luua ja konfigureerida Azure keskmise suurusega VM laiend. Kulub paar minutit juurutamiseks lõpuleviimiseks. Lisateavet keskmise suurusega hosti oleku kasutamise kohta saate vaadata selle `azure vm show` käsk.

Järgmises näites kontrollib nimega VM olekut `myDockerVM` (mallist - vaikenime ei muuda selle nime) ressursirühma nimega `myResourceGroup`. Sisestage nimi eelmises juhises loodud ressursirühma.

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Väljund on `azure vm show` käsk sarnaneb järgmises näites:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Väljund ülaosas kuvatakse soovitud `ProvisioningState` VM. Kui kuvatakse `Succeeded`, juurutamise on lõpule jõudnud, ja saate SSH VM abil.

Väljund, lõpus `FQDN` kuvatakse täielik domeeninimi oma keskmise suurusega hosti nimi. Selle FQDN on kasutate SSH oma keskmise suurusega Host sisse ülejäänud juhised.


## <a name="deploy-your-first-nginx-container"></a>Oma esimese nginx container juurutamine
Kui juurutamise on lõpule jõudnud, SSH oma uue keskmise suurusega Host kohalikust arvutist. Sisestage oma kasutajanimi ja FQDN järgmiselt:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Kui keskmise suurusega hosti sisse logitud, käivitage vaatame mõne nginx container:

```
sudo docker run -d -p 80:80 nginx
```

Väljund sarnaneb järgmises näites nagu nginx pilt alla ja ümbris alustada.

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Värskendustoimingu oleku ümbriste, mis töötab teie keskmise suurusega hosti järgmiselt:

```
sudo docker ps
```

Väljund sarnaneb järgmises näites, näitab, et nginx container on töötab ja TCP-pordid 80 ja 443 ja edastamist:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Oma toimingu ümbris paanide kuvamiseks veebibrauseris avada ja sisestage oma keskmise suurusega hosti FQDN-i nimi.

![Töötava ngnix container](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure'i keskmise suurusega VM laiend malli viide
Olemasoleva Kiirjuhend malli eelmises näites. Ressursihaldur mallide abil saate juurutada Azure keskmise suurusega VM laiend. Seda teha, lisage järgmine ressursihaldur malle, määratledes selle `vmName` oma VM õigesti:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Leiate [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)lugedes ressursihaldur mallide kasutamise kohta lisateavet üksikasjaliku selgituse.


## <a name="next-steps"></a>Järgmised sammud
Soovite [konfigureerida keskmise suurusega daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), mõista [keskmise suurusega turvalisus](https://docs.docker.com/engine/security/security/)või juurutada ümbriste abil [Keskmise suurusega koostamine](https://docs.docker.com/compose/overview/). Azure'i keskmise suurusega VM laiend enda kohta leiate lisateavet teemast [GitHub projekti](https://github.com/Azure/azure-docker-extension/).

Lugege lisateavet keskmise suurusega juurutamise lisasuvandid Azure.

- [Keskmise suurusega seadme abil Azure draiver](./virtual-machines-linux-docker-machine.md)  
- [Keskmise suurusega ja koostamine määratlemine ja Azure virtuaalne arvutis rakendus mitme container kasutamise alustamine](virtual-machines-linux-docker-compose-quickstart.md).
- [Azure'i teenus on kobar juurutamine](../container-service/container-service-deployment.md)
