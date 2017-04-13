<properties
   pageTitle="Sisemise koormuse koormusetasakaalustusteenuse, mis PowerShelli kaudu sisse ressursihaldur loomine | Microsoft Azure'i"
   description="Saate teada, kuidas luua sisemise koormuse koormusetasakaalustusteenuse, mis rakenduses ressursihaldur PowerShelli kaudu"
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

# <a name="create-an-internal-load-balancer-using-powershell"></a>Saate luua ka sisemise koormuse koormusetasakaalustusteenuse PowerShelli kaudu

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Järgmised juhised selgitavad loomiseks on sisemine laadi koormusetasakaalustusteenuse Azure'i ressursihaldur PowerShelli abil. Koos Azure ressursihaldur, luua sisemise koormuse koormusetasakaalustusteenuse üksused eraldi konfigureeritud ja seejärel kombineerida, et luua laadi koormusetasakaalustusteenuse.

Peate loomine ja konfigureerimine järgmiste objektide laadi koormusetasakaalustusteenuse juurutamiseks:

- Lõpeta IP konfiguratsiooni ees - konfigureerib sissetulevat võrguliiklust privaatne IP-aadress
- Taustväärtus aadress pool - tuleb konfigureerida võrgu liidesed, mis saadetakse koormus tasakaalustatud liiklus varsti esiosa IP kaustast
- Laadi tasakaalustamiseks reeglid – lähte-kui ka kohaliku port laadi koormusetasakaalustusteenuse konfigureerimine.
- Sondid – konfigureerib seisund oleku juures virtuaalse masina eksemplarid.
- Sissetulev NAT reeglid – konfigureerib otse juurdepääsemiseks ühte eksemplari virtuaalse masina pordi reeglid.

Saate avada laadimine kohta lisateavet koormusetasakaalustusteenuse komponendid koos Azure ressursihaldur veebisaidil [Azure'i ressursihaldur kasutajatugi laadi koormusetasakaalustusteenuse](load-balancer-arm.md).

Järgmised juhised selgitavad konfigureerida laadi koormusetasakaalustusteenuse kaks virtuaalmasinates vahel.


## <a name="setup-powershell-to-use-resource-manager"></a>PowerShelli kasutamine ressursihaldur häälestamine

Veenduge, et Azure'i mooduli tootmise uusim versioon on PowerShelli jaoks, ja PowerShelli häälestus on õigesti Azure tellimuse juurdepääsu.

### <a name="step-1"></a>Samm 1

        Login-AzureRmAccount

### <a name="step-2"></a>Samm 2

Kontrollige konto tellimused

        Get-AzureRmSubscription

Teil palutakse autentimise ja mandaadi.<BR>

### <a name="step-3"></a>Samm 3

Valida Azure tellimuste kasutada. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Laadi koormusetasakaalustusteenuse ressursi rühma loomine

### <a name="step-4"></a>Samm 4

Saate luua uue ressursirühma (Jäta see juhis kui olemasolevasse rühma ressursi abil)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Seda kasutatakse vaikeasukohaks selle ressursi jaotises ressursid. Veenduge, et kõik käsud Laadi koormusetasakaalustusteenuse loomiseks kasutatakse sama ressursirühma.

Ülaltoodud näites lõime ressursirühma nimega "NRP RG" ja asukoht "Lääne meie".

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Luua virtuaalse võrgu ja esiosa IP pool privaatne IP-aadress


### <a name="step-1"></a>Samm 1

Loob alamvõrku, virtuaalse võrgu ja määrab muutuja $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Virtuaalse võrgu loomiseks tehke järgmist.

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Loob virtuaalse võrgu ja lisab lb alamvõrgu olema alamvõrgu virtuaalse võrgu NRPVNet ja määrab muutuja $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Ette end IP pool ja kirjutamata aadress kausta loomine

Häälestamise esiosa IP pool sissetulevad laadi koormusetasakaalustusteenuse võrgu liikluse ja kirjutamata aadress pool vastuvõtmiseks laadi tasakaalustatud liikluse.

### <a name="step-1"></a>Samm 1

Looge esiosa IP pool privaatne IP-aadress 10.0.2.5 kasutamise selle alamvõrgu 10.0.2.0/24 mis sissetuleva võrgu liikluse lõpp-punkti.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Samm 2

Tagasi end aadress pool kasutatakse Sissetuleva liikluse saadud esiosa IP pool häälestamiseks tehke järgmist.

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>LB reeglid, NAT reeglite loomiseks juures ja koormusetasakaalustusteenuse laadimine

Pärast esiosa IP pool ja kirjutamata aadress rakenduskausta loomist peate luua reeglid, mis kuuluvad laadi koormusetasakaalustusteenuse ressurss:

### <a name="step-1"></a>Samm 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Ülaltoodud näites on loomine järgmised üksused:

- NAT reegel, mis lähevad kõik pordi 3441 liiklust port 3389.
- teise NAT reegli, mis lähevad kõik pordi 3442 liiklust port 3389.
- Laadi koormusetasakaalustusteenuse reegel, mis laadib saldo kõik sissetulevad liikluse avaliku port 80 kohaliku pordi 80 tagasi end aadress kogumi.
- juures reegli, mis kontrollib seisund olek tee "HealthProbe.aspx"



### <a name="step-2"></a>Samm 2

Laadi koormusetasakaalustusteenuse, lisades koos kõigi objektide (NAT reeglid, laadi koormusetasakaalustusteenuse reeglid, juures konfiguratsioone) loomiseks tehke järgmist.

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Võrgu liidesed loomine

Pärast sisemise koormuse koormusetasakaalustusteenuse loomist tuleb määratleda, milline võrgu liidesed saavad sissetulevad koormus tasakaalustatud võrguliiklust NAT reeglid ja juures. Võrgu liidese sel juhul on konfigureeritud eraldi ja saab määrata virtuaalse masina uuem versioon.


### <a name="step-1"></a>Samm 1


Saada ressursi virtuaalse võrgu ja alamvõrgu võrgu liidesed loomiseks:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Selles etapis tuleb loob võrgu liidese, mis kuuluvad laadi koormusetasakaalustusteenuse tagasi end rakenduskausta ja seostada esimese reegli NAT RDP selle võrgu kasutajaliidese:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Samm 2

Looge teine võrgu liidese nimega LB Nic2 olla:

Selles etapis tuleb loob teise võrgu liidese, sama laadi koormusetasakaalustusteenuse tagasi end rakenduskausta määramine ja teise reegli NAT seostamise loodud RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Seetõttu kuvatakse järgmiselt:

    $backendnic1

Oodatav väljund:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Samm 3

Kasutage käsku Lisa-AzureRmVMNetworkInterface virtuaalse masina NIC määramiseks.

Leiate üksikasjalikud juhised virtuaalse masina loomine ja määramine NIC, mis on jälgitavate dokumentide: [Loo on Azure VM PowerShelli kaudu](../virtual-machines/virtual-machines-windows-ps-create.md).

Kui teil on juba loodud virtuaalse masina, saate lisada võrgu liidese tehke järgmist:

#### <a name="step-1"></a>Samm 1

Laadi koormusetasakaalustusteenuse ressursside laadimine muutuja (kui see pole seda veel teinud). Kasutatav muutuja nimetatakse $lb ja kasutada sama nimega laadi koormusetasakaalustusteenuse ressursist loodud kohal.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Samm 2

Laadi kirjutamata konfiguratsioon, muutujana.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Samm 3

Juba loodud võrgu liidese laadimine muutujana. kasutatav muutuja nimi on $nic. Kasutatav võrgu kasutajaliidese nimi on sama Ülaltoodud näites.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Samm 4

Võrgu liides taustväärtus konfiguratsiooni muutmine.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Juhis 5

Võrgu kasutajaliidese objekti salvestada.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Pärast võrgu liidese lisatakse laadi koormusetasakaalustusteenuse kirjutamata pool, see algab vastuvõtmine põhinevad reeglid see laadi koormusetasakaalustusteenuse ressurss tasakaalustamiseks laadi võrguliiklust.

## <a name="update-an-existing-load-balancer"></a>Mõne olemasoleva laadi koormusetasakaalustusteenuse värskendamine


### <a name="step-1"></a>Samm 1

Laadi koormusetasakaalustusteenuse kaudu Ülaltoodud näites kasutamisel muutuv $slb abil Get-AzureRmLoadBalancer laadi koormusetasakaalustusteenuse objekti määramine

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Samm 2

Järgmises näites lisate uue reegli NAT sissetulev port 81 eesmise lõpuks ja port 8181 kasutamise tagasi end pool, kui soovite mõne olemasoleva laadi koormusetasakaalustusteenuse

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Samm 3

Salvestage uus konfiguratsioon Set-AzureLoadBalancer abil

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Laadi koormusetasakaalustusteenuse eemaldamine

Kasutage käsku Eemalda-AzureRmLoadBalancer kustutamiseks on varem loodud laadi koormusetasakaalustusteenuse nimega "NRP-LB" ressursi rühma nimega "NRP RG"

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Saate kasutada valikuline aktiveerimine - Jõusta vältida viiba kustutamiseks.



## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)