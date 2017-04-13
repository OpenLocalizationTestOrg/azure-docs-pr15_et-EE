<properties
    pageTitle="Linux VM on Azure malli abil saate luua | Microsoft Azure'i"
    description="Looge Linux VM Azure on Azure ressursihaldur malli abil."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Luua Linux VM on Azure malli abil

Selles artiklis kirjeldatakse kiiresti juurutamise Linux virtuaalse masina Azure on Azure malli abil.  See artikkel nõuab tehke järgmist.

- Azure'i konto ([saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure'i CLI](../xplat-cli-install.md) sisse logitud `azure login`.

- Azure'i CLI _peab olema_ Azure'i ressursihaldur režiimi `azure config mode arm`.

[Azure'i portaali](virtual-machines-linux-quick-create-portal.md)abil saate juurutada ka kiiresti Linux VM malli.

## <a name="quick-command-summary"></a>Kiirülevaate käsk Kokkuvõte

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse

Mallid võimaldavad luua VMs Azure sätted, mida soovite kohandada ajal käivitada, nt kasutajanimed ja hostinimed sätted. Selle artikli oleme käivitamine mõni Azure Mall, kasutades mõnda Ubuntu VM võrgu turberühma (NSG) koos port 22 avatud SSH jaoks.

Azure'i ressursihaldur Mallid on JSON failid, mida saab kasutada lihtsa ühekordne toimingute abil nagu käivitades mõne Ubuntu VM lõpuleviiduks selle artikli.  Azure'i malle saab kasutada ka ehitada keerukate Azure konfiguratsioone kogu keskkondades nagu testimine, arendaja või tootmise juurutamise virnas.

## <a name="create-the-linux-vm"></a>Linuxi loomine

Koodi järgmises näites kujutatakse helistamiseks `azure group create` ressursirühma luua ja juurutada SSH turvatud Linux VM samal ajal [Azure'i ressursihaldur selle malli](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)abil. Pidage meeles, et teie näites, peate kasutama nimed, mis esinevad vaid teie keskkonnas. Selles näites kasutatakse `myResourceGroup` ressursi rühma nime, ja `myVM` nimeks VM.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Väljund peaks välja nägema järgmine väljund plokk.

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Selle näite juurutatud VM kohta, kasutades funktsiooni `--template-uri` parameeter.  Saate alla laadida või luua malli kohalikult ja edastama malli abil soovitud `--template-file` parameetri argumendina malli faili tee. Azure'i CLI teilt malli nõutud parameetrid.

## <a name="next-steps"></a>Järgmised sammud

Saate teada, millised rakenduse raamistiku juurutada järgmine [kuukalendreid](https://azure.microsoft.com/documentation/templates/) otsida.
