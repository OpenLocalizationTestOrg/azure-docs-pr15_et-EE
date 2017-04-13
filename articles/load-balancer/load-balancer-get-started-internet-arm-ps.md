<properties
   pageTitle="Luua mõne Interneti-ühendusega laadi koormusetasakaalustusteenuse rakenduses ressursihaldur PowerShelli abil | Microsoft Azure'i"
   description="Saate teada, kuidas luua mõne Interneti-ühendusega laadi koormusetasakaalustusteenuse rakenduses ressursihaldur PowerShelli abil"
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

# <a name="get-started"></a>On Interneti-ühendusega laadi koormusetasakaalustusteenuse loomine rakenduses ressursihaldur PowerShelli abil

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [teada, kuidas luua ka Interneti-ühendusega laadi koormusetasakaalustusteenuse klassikaline juurutamise mudeli abil](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Lahenduse juurutamine Azure PowerShelli abil

Järgnevalt selgitatakse loomine on Interneti-ühendusega laadi koormusetasakaalustusteenuse Azure'i ressursihaldur PowerShelli abil. Koos Azure ressursihaldur, iga ressursi on loodud ja konfigureeritud eraldi ja seejärel panete koos laadi koormusetasakaalustusteenuse loomiseks.

Peate looma ja järgmiste objektide juurutamiseks laadi koormusetasakaalustusteenuse konfigureerimine:

- Ees IP konfiguratsiooni: sisaldab avaliku IP (PIP) aadressid sissetulevat võrguliiklust.
- Rakenduskausta tagaandmebaas aadress: sisaldab võrgu liidesed (NICs) virtuaalmasinates saada laadi koormusetasakaalustusteenuse võrguliiklust.
- Koormust tasakaalustavad reegleid: sisaldab reeglid, mis avaliku porti laadi koormusetasakaalustusteenuse vastendamine pordi tagaandmebaas aadress kogumi.
- Sissetulev NAT reegleid: sisaldab reeglid, mis teatud virtuaalse masina tagaandmebaas aadress rakenduskausta pordi avaliku porti laadi koormusetasakaalustusteenuse vastendada.
- Sondid: sisaldab seisund sondid kasutada saadavuse virtuaalse masina eksemplaride tagaandmebaas aadress kogumi.

Lisateabe saamiseks lugege teemat [Azure ressursihaldur laadi koormusetasakaalustusteenuse kasutajatugi](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShelli kasutamine ressursihaldur häälestamine

Veenduge, et teil on Azure ressursihaldur mooduli tootmise uusim versioon PowerShelli jaoks.

1. Azure'i sisse logida.

        Login-AzureRmAccount

    Sisestage oma mandaat, kui seda küsitakse.

2. Kontrollige konto tellimused.

        Get-AzureRmSubscription

3. Valida Azure tellimuste kasutada.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Ressursi rühma loomine. (Selle juhise vahele jätta kasutate olemasolevasse rühma ressursi.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Virtuaalse võrgu ja ees IP pool avaliku IP-aadressi loomine

1. Mõne alamvõrgu ja virtuaalse võrgu loomine.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Looge on Azure avaliku IP address ressurss, nimega **PublicIP**, kasutatakse ees IP kõrval, mille DNS-i nimi **loadbalancernrp.westus.cloudapp.azure.com**. Järgmine käsk kasutab staatilise eraldatud tüüp.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Laadi koormusetasakaalustusteenuse kasutab avaliku IP domeeni silt eesliide selle FQDN. See erineb klassikaline juurutamise mudelit, mis kasutab pilveteenusesse laadi koormusetasakaalustusteenuse FQDN.
    >Selles näites on FQDN **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Ees IP pool ja tagaandmebaas aadress kausta loomine

1. Looge ees IP pool nimega **LB-Frontend** , mis kasutab **PublicIp** ressurss.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Looge tagaandmebaas aadress pool, nimega **LB-taustväärtus**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT reeglid, laadi koormusetasakaalustusteenuse reegli, on juures ja laadi koormusetasakaalustusteenuse loomine

Selles näites luuakse järgmised üksused:

- NAT reegli kõik liiklust Port 3441 pordi 3389 tõlkimine
- NAT reegli kõik liiklust Port 3442 pordi 3389 tõlkimine
- Reegli juures seisund oleku lehele nimega **HealthProbe.aspx**
- Laadi koormusetasakaalustusteenuse reegli kõik liiklust port 80 tagaandmebaas kogumi aadressid porti 80 tasakaalustamiseks
- Laadi koormusetasakaalustusteenuse, mis kasutab kõik need objektid

Kasutage neid juhiseid.

1. NAT reeglite loomine.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Looge seisundi juures. On kaks võimalust konfigureerimine on juures.

    HTTP juures

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP juures

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Laadi koormusetasakaalustusteenuse reegli loomine.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Varem loodud objektide abil luua laadi koormusetasakaalustusteenuse.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>NICs loomine

Võrgu liidesed loomiseks (või muuta olemasolevaid) ja seejärel seostada neid NAT reeglid, laadi koormusetasakaalustusteenuse reeglid ja sondid:

1. Saada virtuaalse võrgu ja virtuaalse alamvõrku, kus NICs vaja luua.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Luua nimega Võrguadapter **lb nic1 olla**, ja esimese reegli NAT ja esimese (ja ainult) tagaandmebaas aadress pool seostada.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Luua nimega Võrguadapter **lb nic2 olla**, ja teise reegli NAT ja esimese (ja ainult) tagaandmebaas aadress pool seostada.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Märkige ruut NICs.

        $backendnic1

    Oodatav väljund:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Kasutage funktsiooni `Add-AzureRmVMNetworkInterface` cmdlet NICs määramiseks muu VMs.

## <a name="create-a-virtual-machine"></a>Luua virtuaalse masina

Virtuaalse masina loomine ja määramine Võrguadapter juhised leiate teemast [loomine on Azure VM PowerShelli kaudu](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Laadi koormusetasakaalustusteenuse võrgu liidese lisamine

1. Laadi koormusetasakaalustusteenuse Azure'i tuua.

    Laadi koormusetasakaalustusteenuse ressursside laadimine muutuja (kui see pole seda veel teinud). Muutuja nimetatakse **$lb**. Kasutage sama nimega ressursist laadi koormusetasakaalustusteenuse varem loodud.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Laadi tagaandmebaas konfiguratsioon, muutujana.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Juba loodud võrgu liidese laadimine muutujana. Muutuja nimi on **$nic**. Võrgu kasutajaliidese nimi on sama üks varasem näide.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Võrgu liides tagaandmebaas konfiguratsiooni muutmine.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Võrgu kasutajaliidese objekti salvestada.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Pärast võrgu liidese lisatakse laadi koormusetasakaalustusteenuse tagaandmebaas pool, see algab vastuvõtmine koormust tasakaalustavad see laadi koormusetasakaalustusteenuse ressurss reeglite võrguliiklust.

## <a name="update-an-existing-load-balancer"></a>Mõne olemasoleva laadi koormusetasakaalustusteenuse värskendamine

1. Laadi koormusetasakaalustusteenuse varasemates näiteks abil määrata laadi koormusetasakaalustusteenuse objekti muutuv **$slb** , kasutades `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. Järgmises näites lisada sissetulev NAT reegel--tagaandmebaas pool – kui soovite mõne olemasoleva laadi koormusetasakaalustusteenuse port 81 ees kogumi ja port 8181 abil.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Salvestamine uue konfiguratsiooni, kasutades `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse eemaldamine

Käsuga `Remove-AzureLoadBalancer` kustutamiseks on varem loodud laadi koormusetasakaalustusteenuse nimega **NRP-LB** ressursi rühma nimega **NRP-RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Saate kasutada valikuline parameeter **-Jõusta** vältida viiba kustutamiseks.

## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
