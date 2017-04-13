<properties
   pageTitle="Luua ka sisemise koormuse koormusetasakaalustusteenuse abil Azure'i CLI rakenduses ressursihaldur | Microsoft Azure'i"
   description="Saate teada, kuidas luua ka sisemise koormuse koormusetasakaalustusteenuse abil Azure'i CLI ressursihaldur rakenduses"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Azure'i CLI abil luua ka sisemise koormuse koormusetasakaalustusteenuse

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Azure'i CLI abil lahenduse juurutamine

Järgmiste juhiste kuvamine kasutades Azure ressursihaldur CLI on Interneti-ühendusega laadi koormusetasakaalustusteenuse loomise kohta. Koos Azure ressursihaldur, iga ressursi on loodud ja konfigureeritud eraldi ja seejärel panete koos ressursi loomiseks.

Peate loomine ja konfigureerimine järgmiste objektide laadi koormusetasakaalustusteenuse juurutamiseks:

- **Eesprotsessor IP-konfiguratsiooni**: sisaldab sissetuleva võrguliikluse jaoks avaliku IP-aadressid
- **Tagaandmebaas aadress pool**: sisaldab võrgu liidesed (NICs) virtuaalmasinates saada võrguliiklust laadi koormusetasakaalustusteenuse lubamine
- **Koormust tasakaalustavad reegleid**: sisaldab reeglid, mis port tagaandmebaas aadress kogumi avaliku porti laadi koormusetasakaalustusteenuse vastendamine
- **NAT sissetulevad reeglid**: sisaldab reeglid, mis teatud virtuaalse masina tagaandmebaas aadress rakenduskausta pordi avaliku porti laadi koormusetasakaalustusteenuse vastendamine
- **Sondid**: sisaldab seisund sondid virtuaalmasinates eksemplaride tagaandmebaas aadress kogumi saadavuse kontrollimiseks kasutatud

Lisateabe saamiseks lugege teemat [Azure ressursihaldur laadi koormusetasakaalustusteenuse kasutajatugi](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Saate seadistada CLI ressursihaldur

1. Kui te pole kunagi varem Azure'i CLI, lugege teemat [installida ja konfigureerida Azure CLI](../../articles/xplat-cli-install.md). Järgige selle kohani, kus saate valida oma Azure'i konto ja tellimus.

2. Käivitage käsk **azure config režiimi** lülitumine ressursihaldur, järgmiselt:

        azure config mode arm

    Oodatav väljund:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Saate luua ka sisemise koormuse koormusetasakaalustusteenuse samm-sammult

1. Azure'i sisse logida.

        azure login

    Kui kuvatakse vastav viip, sisestage mandaat Azure.

2. Saate muuta menüü Tööriistad käsku Azure'i ressursihaldur režiim.

        azure config mode arm

## <a name="create-a-resource-group"></a>Ressursi rühma loomine

Kõik ressursid Azure'i ressursihaldur on seotud ressursirühma. Kui te pole seda veel teinud, luua ressursirühma.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Luua ka sisemise koormuse koormusetasakaalustusteenuse määramine

1. Saate luua ka sisemise koormuse koormusetasakaalustusteenuse

    Nimega nrprg ressursirühma luuakse järgmistel juhtudel Ida-USA piirkond.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Kõik ressursid sisemise laadi soolise, nt virtuaalne võrkude ja virtuaalse alamvõrku, peab olema sama ressursirühm ja piirkonna.

2. Looge sisemise koormuse koormusetasakaalustusteenuse ees IP-aadress.

    IP-aadress, mida kasutate peab olema virtuaalse võrgu alamvõrgu vahemikus.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Looge tagaandmebaas aadress pool.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Pärast ülesande määratlemist ees IP-aadress ja tagaandmebaas aadress pool, saate luua laadi koormusetasakaalustusteenuse reeglid, NAT sissetulevad reeglid, ja kohandatud seisund sondid.

4. Sisemise koormuse koormusetasakaalustusteenuse laadi koormusetasakaalustusteenuse reegli loomine

    Kui järgite eelmisi juhiseid, loob käsu laadi-koormusetasakaalustusteenuse reegli port 1433 ees kogumi kuulata ja saadab koormusetasakaalustusega võrguliiklust tagaandmebaas aadress pool, ka kasutamise port 1433.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Sissetuleva NAT reeglite loomine.

    Laadi koormusetasakaalustusteenuse, mis teatud virtuaalse masina eksemplari lõpp-punktid loomiseks kasutatakse NAT sissetulevad reeglid. Loonud kaks NAT kaugtöölaua eelmisi juhiseid.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Laadi koormusetasakaalustusteenuse seisund sondid luua.

    Seisund juures kontrollib virtuaalse masina läbivalt kogu veendumaks, et nad saavad saata võrguliiklust. Nurjunud juures kontrollid virtuaalse masina eksemplariga eemaldatakse laadi koormusetasakaalustusteenuse seni, kuni see läheb võrguühenduse taastamisel ja juures sisse määratleb terve.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Microsoft Azure'i platvormi kasutab mitmesuguseid haldus stsenaariumid staatilise, avalikult marsruuditavaid IPv4 aadress. IP-aadress on 168.63.129.16. IP-aadress peaks ei blokeeritaks kõik tulemüürid, kuna see võib põhjustada käitu.
    >Suhtes Azure sisemine laadi tasakaalustamiseks, IP-aadressi kasutatakse sondid alla laadimine koormusetasakaalustusteenuse jälgides määratlemiseks virtuaalmasinates koormusetasakaalustusega komplekti jaoks seisundioleku. Kui network turberühma kasutatakse liikluse piiramiseks Azure'i virtuaalmasinates ettevõttesiseselt koormusetasakaalustusega ematermin või rakendatakse virtuaalse alamvõrku, tagama võrgu turvalisus reegli lisamise kaudu 168.63.129.16 liikluse lubamiseks.

## <a name="create-nics"></a>NICs loomine

Peate NICs loomiseks (või muuta olemasolevaid), kuid seostada NAT reeglid, laadi koormusetasakaalustusteenuse reeglid ja sondid.

1. Luua mõne NIC nimega *lb nic1 olla*, ja seejärel siduda selle reegli *rdp1* NAT ja *beilb* tagaandmebaas aadress pool.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Luua mõne NIC nimega *lb nic2 olla*, ja seejärel siduda selle reegli *rdp2* NAT ja *beilb* tagaandmebaas aadress pool.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Luua nimega *DEG1*virtuaalse masina ja seejärel seostada nimega NIC *lb nic1 olla*. Salvestusruumi konto, mida nimetatakse *web1nrp* luuakse enne käivitatakse järgmine käsk:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Laadi koormusetasakaalustusteenuse VMs peavad olema sama kättesaadavus määramine. Kasutage `azure availset create` saadavus loomiseks.

4. Luua nimega *DB2*virtuaalse masina (VM) ja seejärel seostada nimega NIC *lb nic2 olla*. Salvestusruumi konto, mida nimetatakse *web1nrp* loodi enne järgmise käsu.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse kustutamine

Laadi koormusetasakaalustusteenuse eemaldamiseks kasutage järgmine käsk:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi abil allika IP osaleja konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
