<properties 
    pageTitle="Jäljendatud hübriid pilve testimiskeskkonnas | Microsoft Azure'i" 
    description="Jäljendatud hübriidkeskkonna cloud seda pro või arengu testimine, kasutades kahe Azure virtuaalse võrgu ja VNet-VNet ühenduse loomiseks." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Jäljendatud hübriid cloud keskkonnas testimiseks häälestamine

Selles artiklis juhendab teid jäljendatud hübriidkeskkonna pilve loomine Microsoft Azure kahe Azure virtuaalse võrgu kaudu. Siin on tulemuseks konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

See jäljendab hübriid pilve tootmiskeskkonna ja koosneb järgmistest elementidest:

- Lihtsustatud ja jäljendatud kohapealse võrgu majutatud Azure virtuaalse võrku (TestLab virtuaalse võrgu).
- Jäljendatud asutusesiseses virtuaalse võrgu majutatud Azure (TestVNET).
- Kahe virtuaalse võrgu vahelise ühenduse VNet-VNet.
- Teisese domeenikontrolleri TestVNET virtuaalse võrgu.

See pakub eraldi ja ühise alates käsk, mille saate:

- Arendamise ja testida rakenduste jäljendatud hübriidkeskkonnas pilve.
- Saate luua testi konfiguratsioone arvutid, mõned TestLab virtuaalse võrgustikus ja mõned TestVNET virtuaalse võrgus simuleerida hübriid pilvepõhist IT töökoormus.

On neli põhilist etapist hübriid pilve testimiskeskkonnas häälestamiseks.

1.  Konfigureerige TestLab virtuaalse võrgu.
2.  Selle virtuaalse võrgu loomine.
3.  Looge VNet-VNet VPN-ühendus.
4.  Konfigureerige DC2. 

Selle konfiguratsiooni nõuab Azure tellimuse. Kui olete tellimuse MSDN-i või Visual Studio, vaadake [kuu Azure'i krediiti Visual Studio tellijad](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Virtuaalmasinates ja Azure virtuaalse võrgu lüüsid, kehtib töötamise ja jooksvate rahaliste eest. See kulu arve vastu teie MSDN-i või tasutud tellimuse. Mõne Azure'i VPN-lüüsi rakendatakse kahe Azure'i virtuaalmasinates kogumina. Kulude minimeerimiseks loomine test keskkond ja tehke oma vajadusele katsetada ja tutvustada nii kiiresti kui võimalik.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Etapp 1: Konfigureerimine TestLab virtuaalse võrgu

Kasutage juhiseid teemas [Base konfiguratsiooni testimiseks keskkonnas](https://technet.microsoft.com/library/mt771177.aspx) konfigureerida DC1, APP1 ja CLIENT1 arvutite Azure virtuaalse võrgu nimega TestLab. 

Järgmiseks käivitamine on Azure PowerShelli käsuviibas.

> [AZURE.NOTE] Järgmine käsk määrab Azure PowerShelli kasutamine 1,0 ja uuemad versioonid.

Logige sisse oma konto.

    Login-AzureRMAccount

Oma tellimuse nime abil järgmine käsk saada.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Määrake Azure tellimuse. Kasutage sama tellimus, mida kasutasite koostamiseks põhiline konfiguratsioonist etapp 1. Asendage kõik hinnapakkumised, sh selle < ja > märkide õige nimega.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Järgmiseks lisada lüüsi alamvõrgu TestLab virtuaalse võrgu base konfiguratsiooni, mida kasutatakse Azure lüüsi majutada.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Järgmiseks taotleda avaliku IP-aadressi määramiseks lüüsi TestLab virtuaalse võrgu jaoks.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Järgmisena Looge oma lüüsi.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Pidage meeles, mida uue lüüsid saavad võtta 20 minutit või rohkem, et luua.

Azure'i portaalis teie kohaliku arvuti ühenduse DC1 CORP\User1 identimisteabega. Arvutite ja kasutajad kasutavad oma kohaliku domeenikontrolleri autentimiseks CORP domeeni konfigureerimiseks käivitage järgmised käsud administraatori tasemel Windows PowerShelli käsuviiba DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

See on teie praeguse konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Etapp 2: Looge TestVNET virtuaalse võrgu

Esmalt looge TestVNET virtuaalse võrgu ja kaitsta seda võrgu turberühma.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Järgmiseks taotleda TestVNET virtuaalse võrgu lüüsi eraldada ja luua oma lüüsi avaliku IP-aadress.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

See on teie praeguse konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Etapp 3: VNet-VNet ühenduse loomine

Kõigepealt juhuslike, krüptimise osas tugev, 32-kohaline eelnevalt jagatud võti hankida võrgu või turberühma administraatorilt. Vaheldumisi kasutada teabe [Loo juhuslik string on IPsec autentimisel eelnevalt jaotatud võtme](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) saada eelnevalt jagatud võti.

Järgmiseks VNet-VNet VPN-ühendus, mis võib võtta aega loomiseks kasutada järgmisi käske.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Mõne minuti pärast peaksid ühendust luua.

See on teie praeguse konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Etapp 4: Konfigureerimine DC2

Esmalt looge virtuaalse masina DC2 jaoks. Käivitage järgmised käsud Azure PowerShelli käsuviibas kohalikus arvutis.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Järgmiseks ühenduse uue DC2 virtuaalse masina Azure portaalist.

Järgmiseks konfigureerida Windowsi tulemüüri reegli ühenduvuse kontrollimise liikluse lubamiseks. Administraatori tasemel Windows PowerShelli Käsuviip DC2 kohta, käivitage järgmised käsud.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Ping-käsk peaks tulemuseks nelja eduka vastused IP-aadress 10.0.0.4. See on test liikluse üle VNet-VNet ühendus.

Järgmisena lisage eest andmed kettale DC2 uus draiv ketas tähega f nimega.

1.  Serveri halduri vasakpaanil nuppu **fail ja salvestusruumi teenused**ja klõpsake **ketast**.
2.  Paanil sisu **ketast** jaotises nuppu **kettalt 2** (koos **Partition** määramine **tundmatu**).
3.  Klõpsake nuppu **Tööülesanded**ja seejärel klõpsake nuppu **Uus draiv**.
4.  Enne Alustage uut helitugevuse viisardi lehel, klõpsake nuppu **edasi**.
5.  Valige lehe server ja kettale, klõpsake **kettale 2**ja seejärel klõpsake nuppu **edasi**. Kui kuvatakse vastav viip, klõpsake nuppu **OK**.
6.  Määrake helitugevuse lehe suuruse, klõpsake nuppu **edasi**.
7.  Ketas tähe või kausta lehe määramine, klõpsake nuppu **edasi**.
8.  Lehel Valige faili sätted, klõpsake nuppu **edasi**.
9.  Klõpsake lehel Confirm Valikud nuppu **Loo**.
10. Kui olete lõpetanud, klõpsake nuppu **Sule**.

Järgmiseks konfigureerida DC2 corp.contoso.com domeeni domeenikontrolleri koopia. Need käsud käivitada Windows PowerShelli käsuviiba DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Pange tähele, et teil palutakse nii CORP\User1 parool ja parooli Directory Services taastada režiim (DSRM) ja taaskäivitage DC2.

Nüüd, kui TestVNET virtuaalse võrgu on oma DNS-i server (DC2), tuleb konfigureerida TestVNET virtuaalse võrgu kasutada selle DNS-i serveri.

1.  Azure portaali vasakul paanil klõpsake virtuaalse võrgu ikooni ja klõpsake **TestVNET**.
2.  Klõpsake vahekaardil **sätted** nuppu **DNS-serverid**.
3.  **Esmane DNS-i server**tippige **192.168.0.4** 10.0.0.4 asendada.
4.  Klõpsake nuppu **Salvesta**.

See on teie praeguse konfiguratsiooni. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Teie jäljendatud hübriidkeskkonna pilveteenuses on nüüd testida.

## <a name="next-step"></a>Järgmise juhise juurde

- Saate häälestada [ärirakendusele rida veebipõhine,](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) selle keskkonnas.
