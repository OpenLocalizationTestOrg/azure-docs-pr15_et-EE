<properties
   pageTitle="Azure'i CLI klaster ACS mastaapimiseks | Microsoft Azure'i"
   description="Kuidas mastaapimiseks klaster Azure'i teenus Azure'i CLI abil."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Teenuse Azure Container skaala

Kui muudate, arv sõlmed oma Azure Container teenus (ACS) on Azure CLI tööriista abil. Azure'i CLI mastaapimiseks kasutamisel tagastab tööriista uus konfiguratsioonifail tähistav rakendatud ümbris muutmine.

## <a name="about-the-command"></a>Käsu kohta

Azure'i CLI peab olema interaktiivseks Azure'i ümbriste Azure'i ressursihaldur režiim. Mida saab vahetada ressursihaldur režiimi helistades `azure config mode arm`. Funktsiooni `acs` käsk on lapse-käsu nimega `scale` mis teeb skaala toimingute container teenuse. Saate kasutada käsku skaala käivitades erinevate parameetrite kohta abi `azure acs scale --help`, mille väljundid midagi sarnast:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Kasutage käsku mastaapimiseks

Container teenuse mastaapimiseks, peate esmalt leida **ressursirühm** ning **Azure Container teenus (ACS) nimi**ning määrata ka agentide uue arvu. Suurema või väiksema arvu abil saate mastaapimiseks alla või üles vastavalt.

Kui soovite teada, mis praeguse arvu agentide container teenuse enne, kui muudate. Kasutage funktsiooni `azure acs show <resource group> <ACS name>` käsk ACS config tagastamiseks. Pange tähele <mark>arvu</mark> tulemi.

#### <a name="see-current-count"></a>Vt praegune arv

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Uue arvestatakse skaala

Ilmselt juba on iseenesestmõistetav, saate muudate teenuse container helistades `azure acs scale` ja **ressursirühm**, **ACS nimi**ning **agent loendus**. Kui muudate teenuse container, tagastab Azure'i CLI JSON string, mis tähistab uue konfiguratsiooni container teenuse, sh uus agent arvu.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Järgmised sammud

- [Klaster juurutamine](container-service-deployment.md)