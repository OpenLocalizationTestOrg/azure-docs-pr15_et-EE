<properties
    pageTitle="Levinud võrgu PowerShelli käske vms | Microsoft Azure'i"
    description="Levinud PowerShelli käske, et saaksite alustamine virtuaalse võrgu ja selle seotud ressursid vms loomine."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Levinud võrgu Azure PowerShelli käskude vms

Kui soovite luua virtuaalse masina, peate luua [virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) või teadma olemasolevat virtuaalse võrku VM lisamist. Tavaliselt VM loomisel peate selles artiklis kirjeldatud ressursside loomine silmas pidada.

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.

## <a name="create-network-resources"></a>Võrgu ressursse loomine

Ülesanne | Käsk 
-------------- | -------------------------
Alamvõrgu konfiguratsioone loomine | $subnet1 = [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) – nimi "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$subnet2 = New-AzureRmVirtualNetworkSubnetConfig – nimi "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Tüüpilised võrgus võib olla mõne [Interneti vastastikuste laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-internet-overview.md) on alamvõrgu ja eraldi alamvõrgu jaoks on [sisemine laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-internal-overview.md). |
Virtuaalse võrgu loomine | $vnet = [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -resource_group_name"nimi"virtual_network_name"- ResourceGroupName"-asukoht "location_name" - AddressPrefix XX. X.X.X/XX-alamvõrgu $subnet1, $subnet2
Kordumatu domeeninime testimine | [Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "domeeni_nimi"-asukoht "location_name"<BR><BR>Saate määrata [avaliku IP ressursi](../virtual-network/virtual-network-ip-addresses-overview-arm.md), mis loob Azure'i hallatav DNS-serverite jaoks domainname.location.cloudapp.azure.com avaliku IP-aadressi vastenduse DNS-i domeeni nimi. Nimi võib sisaldada ainult tähti, numbreid ja sidekriipse. Esimese ja viimase märgi peab olema tähe või numbri ja domeeninimi peab olema kordumatu oma Azure asukohas. Kui **True,** tagastatakse, on teie pakutud nime globaalselt kordumatu.
Avaliku IP-aadressi loomine | $pip = [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) – nimi "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel "domeeni_nimi"-asukoht "location_name" - AllocationMethod dünaamiline<BR><BR>Avaliku IP-aadressi kasutab varem kontrollitud ja laadi koormusetasakaalustusteenuse konfiguratsiooni frontend kasutab domeeni nime.
Frontend IP konfiguratsiooni loomine | $frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) – nimi "frontend_ip_name" - PublicIpAddress $pip<BR><BR>Frontend konfiguratsioon sisaldab varem loodud sissetuleva võrguliikluse jaoks avaliku IP-aadress.
Kirjutamata aadress kausta loomine | $beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) – nimi "backend_pool_name"<BR><BR>Pakub sisemise aadressid kirjutamata, laadi koormusetasakaalustusteenuse, mis on kättesaadav võrgu liidese kaudu.
Mõne juures loomine | $healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) – nimi "probe_name" - RequestPath 'HealthProbe.aspx'-protokolli http-Port 80 – IntervalInSeconds 15 - ProbeCount 2<BR><BR>Sisaldab seisundi sondid kasutada saadavuse virtuaalmasinates eksemplaride kirjutamata aadress kogumi.
Koormus tasakaalustamiseks reegli loomine | $lbRule = [Uus AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -nime HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-Probe $healthProbe-protokolli Tcp - FrontendPort 80 – BackendPort 80<BR><BR>Sisaldab reeglid, mis määramine avaliku porti laadi koormusetasakaalustusteenuse kirjutamata aadress kogumi porti.
Sissetuleva NAT reegli loomine | $inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) – nimi "rule_name" - FrontendIpConfiguration $frontendIP-protokolli TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Sisaldab avaliku porti koormusetasakaalustusteenuse laadimine ja teatud virtuaalse masina kirjutamata aadress rakenduskausta pordi vastendamise reeglid.
Laadi koormusetasakaalustusteenuse loomine | $loadBalancer = [New-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName "resource_group_name"-nime "load_balancer_name"-asukoht "location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-Probe $healthProbe
Võrgu liidese loomine | $nic1 = [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName "resource_group_name"-nime "network_interface_name"-asukoht "location_name" - PrivateIpAddress XX. X.X.X-alamvõrgu subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Looge võrgu liidese, kasutades avaliku IP-aadress ja virtuaalse alamvõrku, varem loodud.
    
## <a name="get-information-about-network-resources"></a>Võrgu ressursside kohta teabe saamine

Ülesanne | Käsk 
-------------- | -------------------------
Virtuaalne võrkude loend | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Loetleb kõik virtuaalse võrkude ressursirühma.
Virtuaalse võrgu kohta teabe saamine | Get-AzureRmVirtualNetwork-resource_group_name"nimi"virtual_network_name"- ResourceGroupName"
Loendi alamvõrku virtuaalse võrgus | Get-AzureRmVirtualNetwork-nime "virtual_network_name" - ResourceGroupName "resource_group_name" & #124; Valige alamvõrku
Mõne alamvõrgu kohta teabe saamine | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) – nimi "subnet_name" - VirtualNetwork $vnet<BR><BR>Saab teavet alamvõrgu määratud virtuaalse võrgu. $vnet väärtus tähistab tagastatud Get-AzureRmVirtualNetwork objekt.
IP-aadresside loend | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Loetleb ressursirühma avaliku IP-aadressid.
Loendi koormus soolise | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Loetleb koormus soolise ressursirühma.
Loendi võrgu liidesed | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Loetleb kõik võrgu liidesed ressursirühma.
Võrgu liidese kohta teabe saamine | Get-AzureRmNetworkInterface-resource_group_name"nimi"network_interface_name"- ResourceGroupName"<BR><BR>Saab teavet teatud võrgu liidese.
Võrgu liidese IP-konfiguratsiooni hankimine | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) – nimi "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Saab teavet määratud võrgu liidese IP-konfiguratsiooni. $nic väärtus tähistab tagastatud Get-AzureRmNetworkInterface objekt.

## <a name="manage-network-resources"></a>Võrgu ressursse haldamine

Ülesanne | Käsk 
-------------- | -------------------------
Mõne alamvõrgu virtuaalse võrku lisamine | [Lisage AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX – nimi "subnet_name" - VirtualNetwork $vnet<BR><BR>Lisab on alamvõrgu olemasolevat virtuaalse võrku. $vnet väärtus tähistab tagastatud Get-AzureRmVirtualNetwork objekt.
Virtuaalse võrgu kustutamine | [Eemalda – AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -resource_group_name"nimi"virtual_network_name"- ResourceGroupName"<BR><BR>Eemaldab määratud virtuaalse võrgu ressursirühma.
Võrgu liidese kustutamine | [Eemalda – AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -resource_group_name"nimi"network_interface_name"- ResourceGroupName"<BR><BR>Eemaldab määratud võrgu liidese ressursirühma.
Laadi koormusetasakaalustusteenuse kustutamine | [Eemalda – AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -resource_group_name"nimi"load_balancer_name"- ResourceGroupName"<BR><BR>Eemaldab määratud laadi koormusetasakaalustusteenuse ressursirühma.
Avaliku IP-aadressi kustutamine | [Eemalda – AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-resource_group_name"nimi"ip_address_name"- ResourceGroupName"<BR><BR>Eemaldab määratud avaliku IP-aadressi ressursirühma.

## <a name="next-steps"></a>Järgmised sammud

- Kasutage võrgu liidese, et äsja loodud järgmistel juhtudel saate [luua VM](virtual-machines-windows-ps-create.md).
- Teavet, kuidas saate [luua mitme võrgu liidesed koos VM](../virtual-network/virtual-networks-multiple-nics.md).
