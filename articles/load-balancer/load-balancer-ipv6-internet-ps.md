<properties
    pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse IPv6 jaoks ressursihaldur PowerShelli kaudu Interneti loomine | Microsoft Azure'i"
    description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse IPv6 jaoks ressursihaldur PowerShelli abil"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6 azure laadimine koormusetasakaalustusteenuse, kahekordne virnas, avaliku ip, kohalikke ipv6, mobile, asjade"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Vastastikuste laadi koormusetasakaalustusteenuse IPv6 jaoks ressursihaldur PowerShelli kaudu Interneti loomise alustamiseks

> [AZURE.SELECTOR]
- [PowerShelli](./load-balancer-ipv6-internet-ps.md)
- [Azure'i CLI](./load-balancer-ipv6-internet-cli.md)
- [Mall](./load-balancer-ipv6-internet-template.md)

Mõne Azure laadi koormusetasakaalustusteenuse on kiht-4 (TCP, UDP) laadi koormusetasakaalustusteenuse. Laadi koormusetasakaalustusteenuse pakub kõrge-saadavus jagades liiklust terve teenuse pilveteenuste aknad või virtuaalmasinates laadi koormusetasakaalustusteenuse komplekti vahel. Azure'i laadi koormusetasakaalustusteenuse saate esitada ka nende teenuste mitme pordid, mitu IP-aadressi või mõlemad.

## <a name="example-deployment-scenario"></a>Näide juurutamise stsenaarium

Järgmine diagramm näitab tasakaalustamiseks lahendus selle artikli sinna laadi.

![Laadi koormusetasakaalustusteenuse stsenaarium](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Selle stsenaariumi loote Azure järgmistest allikatest:

- mõne IPv4 ja IPv6 avaliku IP-aadress on Interneti-ühendusega laadi koormusetasakaalustusteenuse
- kaks laadimine tasakaalustavad reeglid avaliku VIP vastendamiseks privaatne lõpp-punktid
- Kättesaadavus seadmine abil, mis sisaldab kahte VMs
- kahe virtuaalmasinates (VM)
- virtuaalse võrgu kasutajaliidese jaoks iga VM määratud nii IPv4 ja IPv6 aadressid

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Azure'i PowerShelli kaudu lahenduse juurutamine

Järgmised toimingud näitab, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur PowerShelli abil. Koos Azure ressursihaldur, iga ressursi on loodud ja konfigureeritud, siis pane koos ressursi loomiseks.

Laadi koormusetasakaalustusteenuse juurutamiseks loomist ja konfigureerimist järgmiste objektide:

- IP-konfiguratsiooni ees - sisaldab avaliku IP-aadresside sissetulevat võrguliiklust.
- Tagaandmebaas aadress pool - sisaldab virtuaalmasinates saada võrguliiklust laadi koormusetasakaalustusteenuse võrgu liidesed (NICs).
- Reeglite koormusetasandus - sisaldab vastendamise avaliku porti laadi koormusetasakaalustusteenuse pordi tagaandmebaas aadress kogumi reeglid.
- Sissetulevad reeglid NAT - sisaldab avaliku porti koormusetasakaalustusteenuse laadimine ja teatud virtuaalse masina tagaandmebaas aadress rakenduskausta pordi vastendamise reeglid.
- Sondid – sisaldab seisund sondid kasutada saadavuse virtuaalmasinates eksemplaride tagaandmebaas aadress kogumi.

Lisateabe saamiseks lugege teemat [Azure ressursihaldur laadi koormusetasakaalustusteenuse kasutajatugi](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShelli kasutamine ressursihaldur häälestamine

Veenduge, et teil on Azure ressursihaldur mooduli tootmise uusim versioon PowerShelli jaoks.

1. Azure sisselogimine

        Login-AzureRmAccount

    Sisestage oma mandaat, kui seda küsitakse.

2. Kontrollige konto tellimused

        Get-AzureRmSubscription

3. Valida Azure tellimuste kasutada.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Ressursirühm (Jäta see juhis kui abil olemasolevasse rühma ressursi) loomine

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Virtuaalse võrgu ja ees IP pool avaliku IP-aadressi loomine

1. Saate luua virtuaalse võrk on alamvõrgu.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Azure'i avaliku IP-aadress (PIP) ressursid ees IP address kausta loomine

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Laadi koormusetasakaalustusteenuse kasutab domeeni silt avaliku IP nagu eesliide selle FQDN. Selles näites on FQDN-i on *lbnrpipv4.westus.cloudapp.azure.com* ja *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Eesprotsessor IP konfiguratsioone ja tagaandmebaas aadress kausta loomine

1. Looge ees aadressi konfiguratsiooni, mis kasutab loodud avaliku IP-aadressid.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Luua tagaandmebaas aadress.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB reeglid, NAT reegleid, on juures ja laadi koormusetasakaalustusteenuse loomine

Selles näites luuakse järgmised üksused:

- NAT reegli kõik pordi 443 pordi 4443 liiklust tõlkimise kohta
- Laadi koormusetasakaalustusteenuse reegli kõik liiklust port 80 tagaandmebaas kogumi aadressid porti 80 tasakaalustamiseks.
- Laadi koormusetasakaalustusteenuse reegli VMs porti 3389 RDP ühenduse lubamine.
- reegli juures seisund oleku lehele nimega *HealthProbe.aspx* või teenuse Port 8080
- Laadi koormusetasakaalustusteenuse, mis kasutab kõik need objektid

1. NAT reeglite loomine.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Looge seisundi juures. On kaks võimalust konfigureerimine on juures.

    HTTP juures

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    või TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    Selle näite puhul me TCP sondid kasutada.

3. Laadi koormusetasakaalustusteenuse reegli loomine.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Laadi koormusetasakaalustusteenuse, mis on varem loodud objektide abil saate luua.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Tagaandmebaas vms NICs loomine

1. Saada virtuaalse võrgu ja virtuaalse alamvõrku, kus NICs vaja luua.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Luua IP konfiguratsioone ja NICs VMs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Kuidas luua virtuaalmasinates ja määrata vastloodud NICs

VM loomise kohta leiate lisateavet teemast [loomine ja preconfigure Windowsi virtuaalse masina ressursihaldur ja Azure PowerShelli](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Kättesaadavus ja salvestusruumi konto loomine

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Looge iga VM ja määrata eelmise loodud NICs

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
