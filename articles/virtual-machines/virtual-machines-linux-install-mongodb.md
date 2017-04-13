<properties
   pageTitle="Installige MongoDB Linux VM | Microsoft Azure'i"
   description="Siit saate teada, kuidas installida ja konfigureerida MongoDB Linux virtuaalse masina Azure'i ressursihaldur juurutamise mudeli kasutamine."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Installige ja konfigureerige MongoDB Linux VM Azure
[MongoDB](http://www.mongodb.org) on populaarsed avatud lähtekoodi, suure jõudlusega NoSQL andmebaas. Selles artiklis näidatakse, kuidas installida ja konfigureerida MongoDB Linux VM Azure ressursihaldur juurutamise mudeli abil. Näited kuvatakse selle üksikasjad, kuidas soovite:

- [Käsitsi installimine ja konfigureerimine lihtsa MongoDB eksemplari](#manually-install-and-configure-mongodb-on-a-vm)
- [Saate luua lihtsa MongoDB eksemplari ressursihaldur malli abil](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Looge keerukas MongoDB, sharded kobar koopiaga määrab ressursihaldur malli abil](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis on vaja:

- Azure'i konto ([saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/)).
- [Azure'i CLI](../xplat-cli-install.md) sisse logitud`azure login`
- funktsiooni Azure'i CLI *peab olema* Azure'i ressursihaldur režiimi kasutamine`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Käsitsi installimine ja konfigureerimine MongoDB VM
MongoDB [installimise juhiste andmine](https://docs.mongodb.com/manual/administration/install-on-linux/) Linux distros, sh punane rolli / CentOS, SUSE, Ubuntu ja Debian. Järgmises näites luuakse mõne `CoreOS` VM abil SSH võti, mis säilitatakse juures `.ssh/azure_id_rsa.pub`. Vastata salvestusruumikonto nimi, DNS-i nimi ja administraatori identimisteave kuvatavaid juhiseid.

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Logige sisse VM abil kuvatakse eelnev VM loomise etappi lõpus avaliku IP-aadress:

```bash
ssh ops@40.78.23.145
```

Installi allikad MongoDB lisamiseks loomine on `yum` hoidla faili järgmiselt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

MongoDB repo faili redigeerimiseks avada. Lisage järgmised read.

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Installige MongoDB abil `yum` järgmiselt:

```bash
sudo yum install -y mongodb-org
```

Vaikimisi on SELinux jõustatud CentOS pilte, mis takistab teil juurdepääsemiseks MongoDB. Poliitika Haldusriistad installida ja konfigureerida SELinux lubamiseks MongoDB sõitmiseks vaikimisi TCP port 27017 järgmiselt. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

MongoDB teenuse käivitamine järgmiselt:

```bash
sudo service mongod start
```

Ühenduse loomine kohaliku abil MongoDB installimise kinnitamiseks `mongo` kliendi:

```bash
mongo
```

Nüüd testimiseks MongoDB eksemplari, lisades sinna mõned andmed ning seejärel otsimise tehke järgmist.

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Soovi korral konfigureerida MongoDB taaskäivitada ajal automaatselt käivituma.

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Saate luua lihtsa MongoDB eksemplari CentOS malli abil
Ühe CentOS VM kaudu Github järgmised Azure Kiirjuhend malli abil saate luua lihtsa MongoDB eksemplari. See mall kasutab kohandatud skript laiend Linuxi lisamiseks on `yum` hoidla äsja loodud CentOS VM ja seejärel installige MongoDB.

- [Tavaline MongoDB eksemplari CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUS` piirkond. Sisestage oma väärtused järgmiselt:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure'i CLI annab teile viip mõne sekundi juurutamise loomise, kuid installimine ja konfigureerimine võtab paar minutit. Värskendustoimingu oleku juurutus, nii et `azure group deployment show myResourceGroup`, vastavalt oma ressursirühm nime sisestamine. Oodake, kuni soovitud `ProvisioningState` näitab "Õnnestus" enne SSH VM abil.

Kui juurutamise on lõpule jõudnud, SSH VM. Saada IP-aadress VM abil soovitud `azure vm show` käsk, nagu järgmises näites:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Väljund, lõpus olevat `Public IP address` ei kuvata. SSH oma VM oma VM IP-aadressiga:

```bash
ssh ops@138.91.149.74
```

Ühenduse loomine kohaliku abil MongoDB installimise kinnitamiseks `mongo` kliendi järgmiselt:

```bash
mongo
```

Nüüd testida eksemplari, lisades sinna mõned andmed ning otsimise järgmiselt:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Looge keerukas MongoDB Sharded kobar CentOS malli abil
Keerukate MongoDB sharded kobar järgmised Github: Azure'i Kiirjuhend malli abil saate luua. Selle malli järgib [MongoDB sharded kobar heade tavade](https://docs.mongodb.com/manual/core/sharded-cluster-components/) koondamise ja kõrge kättesaadavus. Mall loob kaks shards, kolm sõlmed igas kogumis koopia. Samuti on loodud ühe config Serveri koopias kolm sõlmed seadistatud, pluss kaks `mongos` ruuteri serverid esitada kogu shards järjepidevuse rakenduste kaudu.

- [MongoDB Sharding kobar CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Juurutamine MongoDB keerukate sharded kobar nõuab rohkem kui 20 tuuma, mis on tavaliselt vaikimisi core arvu piirkond tellimuse kohta. Avage Azure'i tugiteenuse taotluse, et suurendada oma core arvu.

Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUS` piirkond. Sisestage oma väärtused järgmiselt:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure'i CLI annab teile viip mõne sekundi juurutamise loomise, kuid installimine ja konfigureerimine saavad võtta üle tunni lõpuleviimiseks. Värskendustoimingu oleku juurutus, nii et `azure group deployment show myResourceGroup`, muuta vastavalt vajadusele oma ressursi rühma nime. Oodake, kuni soovitud `ProvisioningState` kuvatakse enne ühenduse VMs "Õnnestus".


## <a name="next-steps"></a>Järgmised sammud
Nendes näidetes loote MongoDB eksemplari kohalikult VM. Kui soovite teise VM või võrgust MongoDB näiteks ühendada, veenduge, et vastav [võrgu turberühma reeglid luuakse](virtual-machines-linux-nsg-quickstart.md).

Loomise mallide kasutamise kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).

Azure'i ressursihaldur mallide abil kohandatud skript laiendamine alla laadida ja käivitada skriptide oma VMs. Lisateabe saamiseks leiate [Azure'i kohandatud skript laiend Linux Virtuaalmasinates abil](virtual-machines-linux-extensions-customscript.md).