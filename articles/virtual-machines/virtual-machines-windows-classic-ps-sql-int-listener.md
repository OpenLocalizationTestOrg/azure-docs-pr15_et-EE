<properties
    pageTitle="Konfigureerimine on ILB kuulajale jaoks alati kättesaadavus rühmad | Microsoft Azure'i"
    description="Selles õpetuses kasutab ressursse, mis on loodud mudeliga klassikaline juurutamise ja loob on alati klõpsake kättesaadavus rühma kuulajale Azure on sisemine laadi koormusetasakaalustusteenuse (ILB) abil."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Azure'i mõne ILB kuulajale alati klõpsake kättesaadavus rühmade konfigureerimine

> [AZURE.SELECTOR]
- [Sisemise kuulajale](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Välise kuulajale](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Ülevaade

Selles teemas näidatakse, kuidas konfigureerida on kuulajale alati klõpsake kättesaadavus rühma jaoks on **Sisemine laadi koormusetasakaalustusteenuse (ILB)**abil.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressursihaldur mudelis konfigureerimine on ILB kuulajale alati kättesaadavus rühma jaoks, vaadake [konfigureerimine on sisemine laadi koormusetasakaalustusteenuse Azure alati kättesaadavus rühma jaoks](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Kättesaadavus rühma võivad sisaldada koopiad, mis on kohapealse ainult Azure ainult või ühendada nii kohapealse ja Azure hübriidjuurutuse jaoks. Azure'i koopiad võivad asuda samas piirkonnas või mitme piirkondade mitme virtuaalse võrgu (VNets) abil. Allpool juhistes eeldatakse, et teil on juba [konfigureeritud kättesaadavus rühma](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , kuid on konfigureeritud on kuulajale.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Sisemise kuulajatele seadmine
Võtke arvesse järgmisi juhiseid kättesaadavus rühma kuulajale Azure ILB kasutamise kohta.

- Kättesaadavus rühma kuulajale on toetatud Windows Server 2008 R2, Windows Server 2012 ja Windows Server 2012 R2.

- Ainult üks sise-saadavus rühma kuulajale toetatakse kohta pilveteenuses, kuna selle ILB kuulajale on konfigureeritud ja on ainult üks ILB pilveteenuses kohta. Siiski, on võimalik luua mitme välise kuulajatele. Lisateavet leiate teemast [konfigureerimine mõne välise kuulajale alati klõpsake kättesaadavus rühmade Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Luua ka sisemise kuulajale sama pilveteenusesse, kus teil on mõne välise kuulajale abil soovitud pilveteenuses avaliku VIP ei toetata.

## <a name="determine-the-accessibility-of-the-listener"></a>Valitud hõlbustusfunktsioonide kuulajale määratlemine

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

See artikkel keskendub loomise kuulajale, mis kasutab mõnda **Sisemise laadi koormusetasakaalustusteenuse (ILB)**. Kui teil on vaja mõnda avalik/väline kuulajale, näete versiooni käesoleva artikli, mis on [kuulajale välise](virtual-machines-windows-classic-ps-sql-ext-listener.md) häälestamise juhised

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Luua otse serveriga saatja koormusetasakaalustusega VM lõpp-punktid

ILB, peate esmalt looma sisemise koormuse koormusetasakaalustusteenuse. Seda skripti allpool.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. **ILB**, tuleks määrata staatiline IP-aadress. Esmalt uurida VNet konfiguratsioonile, käivitage järgmine käsk:

        (Get-AzureVNetConfig).XMLConfiguration

1. Pange tähele alamvõrku, mis sisaldab selle koopiad majutavad VMs **alamvõrgu** nimi. Seda kasutatakse **$SubnetName** parameetri skripti.

1. Seejärel Lisa **VirtualNetworkSite** nime ja käivitamine **AddressPrefix** alamvõrku, mis sisaldab selle koopiad majutavad VMs. Otsige mõlemad väärtused **Test-AzureStaticVNetIP** käsu läbimise ja uurida **AvailableAddresses**saadaval IP-aadress. Näiteks kui soovitud VNet nimega *MyVNet* ja oli alamvõrgu aadress vahemiku *172.16.0.128*käivitamise, Saadaval aadressid loetletakse järgmine käsk:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Valige üks saadaval aadressid ja kasutada seda parameetrit **$ILBStaticIP** järgmist skripti.

3. Kopeerige PowerShelli skripti all tekstiredaktoris ja seadke muutujaga vastavalt teie keskkonnas (Pange tähele, et vaikesätted on toodud mõned parameetrid). Pange tähele, et olemasoleva juurutuste, mis kasutavad osaleja rühmad ei saa lisada ILB. ILB nõuete kohta leiate lisateavet teemast [Sisemise laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-internal-overview.md). Lisaks, kui kättesaadavus rühma ulatub Azure regioonid, peate käivitama skripti üks kord iga andmekeskuses pilveteenuses ja sõlmed, mis ei asu selle andmekeskuse.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Kui olete seadistanud muutujaid, kopeerige skripti tekstiredaktoris üheks seansi Azure PowerShelli seda käivitamiseks. Kui endiselt kuvatakse viip >>, tippige uuesti veendumaks, et skript käivitub ENTER. Märkus

>[AZURE.NOTE] Azure'i klassikaline portaali ei toeta sisemise laadi koormusetasakaalustusteenuse sel ajal, nii, et te ei näe soovitud ILB või Azure klassikaline portaali lõpp-punktid. Siiski **Get-AzureEndpoint** tagastab sisemise IP-aadress, kui see töötab laadi koormusetasakaalustusteenuse. Vastasel korral tagastab null.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Veenduge, et KB2854082 on installitud vajaduse korral

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Avage tulemüüri portide kättesaadavus rühma sõlmed

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kättesaadavus rühma kuulajale loomine

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. ILB, peate kasutama IP-aadressi, kuvatakse sisemise laadi koormusetasakaalustusteenuse (ILB) varem loodud. Kasutage järgmist skripti hankimiseks PowerShelli see IP-aadress.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Üks VMs, kopeerige PowerShelli skripti opsüsteem tekstiredaktoris ja seadke muutujate eespool märgitud väärtused.

    Windows Server 2012 või uuem versioon, kasutage järgmist skripti:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Windows Server 2008 R2 kasutage järgmist skripti:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Üks kord teil on seatud muutujaid, avage administraatoriõigustes Windows PowerShelli aken, siis kopeerige script editor teksti ja kleepida selle käivitamiseks seansi Azure PowerShelli. Kui endiselt kuvatakse viip >>, tippige uuesti veendumaks, et skript käivitub ENTER.

2. Korrake seda toimingut iga VM. See skript konfigureerib IP-aadressi ressursi pilveteenusesse IP-aadress ja määrab muude parameetrite nagu pordi juures. Kui IP-aadressi ressursi ühendamise, saate seda siis vastata küsitlused juures Port loodud selles õpetuses koormusetasakaalustusega lõpp.

## <a name="bring-the-listener-online"></a>Tuua kuulajale võrgus

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Järeltegevuse üksused

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testige kättesaadavus rühma kuulajale (jooksul sama VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
