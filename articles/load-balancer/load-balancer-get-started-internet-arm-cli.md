<properties
   pageTitle="Luua Interneti vastastikuste laadi koormusetasakaalustusteenuse rakenduses ressursihaldur Azure'i CLI abil | Microsoft Azure'i"
   description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse rakenduses ressursihaldur Azure'i CLI abil"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Sisemise koormuse koormusetasakaalustusteenuse, mis abil Azure'i CLI loomine

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse klassikaline juurutamise abil](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Kasutades Azure CLI lahenduse juurutamine

Järgmised toimingud näitab, kuidas luua vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur funktsiooniga CLI Interneti. Azure'i ressursihaldur iga ressursi on loodud ja konfigureeritud, siis pane koos ressursi loomiseks.

Peate looma ja järgmiste objektide juurutamiseks laadi koormusetasakaalustusteenuse konfigureerimine:

- IP-konfiguratsiooni ees - sisaldab avaliku IP-aadresside sissetulevat võrguliiklust.
- Tagaandmebaas aadress pool - sisaldab virtuaalmasinates saada võrguliiklust laadi koormusetasakaalustusteenuse võrgu liidesed (NICs).
- Reeglite koormusetasandus - sisaldab vastendamise avaliku porti laadi koormusetasakaalustusteenuse pordi tagaandmebaas aadress kogumi reeglid.
- Sissetulevad reeglid NAT - sisaldab avaliku porti koormusetasakaalustusteenuse laadimine ja teatud virtuaalse masina tagaandmebaas aadress rakenduskausta pordi vastendamise reeglid.
- Sondid – sisaldab seisund sondid kasutada saadavuse virtuaalmasinates eksemplaride tagaandmebaas aadress kogumi.

Lisateavet leiate [Azure'i ressursihaldur laadi koormusetasakaalustusteenuse kasutajatugi](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Saate seadistada CLI ressursihaldur

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.

2. Käsu **azure config režiimi** ressursihaldur lülitumine, nagu allpool näidatud.

        azure config mode arm

    Oodatav väljund:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Virtuaalse võrgu ja ees IP pool avaliku IP-aadressi loomine

1. Luua nimega *NRPVnet* Ida-USA asukoha abil nimega *NRPRG*ressursirühma virtuaalse võrgu (VNet).

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Looge alamvõrku, mis on nimega *NRPVnetSubnet* koos CIDR plokk 10.0.0.0/24 *NRPVnet*sisse.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Luua nimega *NRPPublicIP* ees IP kõrval, mille DNS-i nimi *loadbalancernrp.eastus.cloudapp.azure.com*kasutada avaliku IP-aadress. Käsu alla kasutab staatilise eraldatud tüüp ja jõudeaja ajalõpu 4 minutit.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Laadi koormusetasakaalustusteenuse peaks kasutama oma FQDN avaliku IP domeeni silt. See klassikaline kasutuselevõtu, mis kasutab pilveteenuses muudatuse teenuse laadi koormusetasakaalustusteenuse täielikult kvalifitseeritud domeeni nimi (FQDN).
    >Selles näites on FQDN *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse loomine

Järgmine käsk loob laadi koormusetasakaalustusteenuse, nimega *NRPlb* *NRPRG* ressursirühm *Ida-USA* Azure asukoht.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Ees IP pool ja kirjutamata aadress kausta loomine

Selles näites näitab, kuidas luua ees IP kogumi saab sissetuleva võrguliikluse koormusetasakaalustusteenuse laadimine ja kirjutamata IP pool, kus ees pool saadab koormus tasakaalustatud võrguliiklust.

1. Saate luua ees IP pool koormusetasakaalustusteenuse laadimine ja eelmises etapis loodud avaliku IP seostada.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Saate häälestada tagaandmebaas aadress pool, kasutatakse Sissetuleva liikluse saadud ees IP pool.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>LB reeglid, NAT reeglid ja juures loomine

Selles näites luuakse järgmist.

- reegli NAT tõlkida kõik liiklust port 21 port 22<sup>1</sup>
- NAT reegli kõik liiklust Port 23 pordi 22 tõlkimine
- Laadi koormusetasakaalustusteenuse reegli kõik liiklust port 80 tagaandmebaas kogumi aadressid porti 80 tasakaalustamiseks.
- reegli juures seisund oleku lehele nimega *HealthProbe.aspx*.

<sup>1</sup> NAT reeglid on seotud kindla virtuaalse masina eksemplari laadi koormusetasakaalustusteenuse taha. Teatud virtuaalse masina port 22 seostatud selle reegli NAT saadetakse saabuvate port 21 võrguliiklust. Määrake reegli NAT protokolli (UDP või TCP). Nii Protokollid ei saa määrata sama port.

1. NAT reeglite loomine.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Laadi koormusetasakaalustusteenuse reegli loomine.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Looge seisundi juures.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Kontrollige sätteid.

        azure network lb show nrprg nrplb

    Oodatav väljund:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>NICs loomine

Peate NICs loomiseks (või muuta olemasolevaid), kuid seostada NAT reeglid, laadi koormusetasakaalustusteenuse reeglid ja sondid.

1. Luua nimega Võrguadapter *lb nic1 olema*, ja *rdp1* NAT reegli ja *NRPbackendpool* tagaandmebaas aadress rakenduskausta seostada.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Oodatav väljund:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Luua nimega Võrguadapter *lb nic2 olema*, ja *rdp2* NAT reegli ja *NRPbackendpool* tagaandmebaas aadress rakenduskausta seostada.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Luua nimega *web1*virtuaalse masina (VM) ja seostada nimega NIC *lb nic1 olla*. Enne käsu allpool loodi salvestusruumi konto, mida nimetatakse *web1nrp* .

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Laadi koormusetasakaalustusteenuse VMs peavad olema sama kättesaadavus määramine. Kasutage `azure availset create` saadavus loomiseks.

    Väljund peaks olema umbes järgmine:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] **See on konfigureeritud publicIP ilma Võrguadapter** teavitamise sõnumi peaks alates NIC laadi koormusetasakaalustusteenuse, mis Internetis laadi koormusetasakaalustusteenuse avaliku IP-aadressi ühenduse luua.

    Kuna selle *lb nic1 olema* NIC on seotud *rdp1* NAT reegel, saate luua ühenduse *web1* abil RDP laadi koormusetasakaalustusteenuse 3441 porti kaudu.

4. Luua nimega *web2*virtuaalse masina (VM) ja seostada nimega NIC *lb nic2 olla*. Enne käsu allpool loodi salvestusruumi konto, mida nimetatakse *web1nrp* .

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Mõne olemasoleva laadi koormusetasakaalustusteenuse värskendamine

Saate lisada reeglid viitamine olemasoleva laadimine koormusetasakaalustusteenuse. Järgmises näites lisatakse uus laadi koormusetasakaalustusteenuse reegel olemasoleva laadi koormusetasakaalustusteenuse **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse kustutamine

Laadi koormusetasakaalustusteenuse eemaldada järgmise käsu abil:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-cli.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
