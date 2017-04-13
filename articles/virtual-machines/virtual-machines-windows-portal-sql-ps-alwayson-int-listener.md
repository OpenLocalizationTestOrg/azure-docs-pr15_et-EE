<properties
   pageTitle="Alati kättesaadavus rühma kuulajatele – Microsoft Azure konfigureerimine"
   description="Saate konfigureerida kättesaadavus rühma kuulajatele Azure'i ressursihaldur mudel, on sisemine laadi koormusetasakaalustusteenuse abil üks või mitu IP-aadressid."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Ühe või mitme alati klõpsake kättesaadavus rühma kuulajatele - ressursihaldur konfigureerimine 

Selles teemas kirjeldatakse, kuidas teha.

- Saate luua ka sisemise koormuse koormusetasakaalustusteenuse SQL serveri kättesaadavus rühmade PowerShelli cmdlet-käskude abil.

- Laadi koormusetasakaalustusteenuse toetamiseks rohkem kui üks SQL serveri kättesaadavus rühma täiendavad IP-aadressid lisada. 

SQL Server on kättesaadavus rühma kuulajale on virtuaalse võrgu nimi, mis klientrakendustega juurdepääsuks andmebaasi põhi- või koopia. Klõpsake Azure'i virtuaalmasinates, kehtib laadi koormusetasakaalustusteenuse kuulajale IP-aadress. Laadi koormusetasakaalustusteenuse marsruudib liikluse SQL Server, mis on juures pordi kuulamise eksemplari. Enamikul juhtudel kättesaadavus rühma kasutab mõnda sisemise koormuse koormusetasakaalustusteenuse. Azure'i sisemine koormusetasakaalustus majutada ühe või mitu IP-aadressid. Iga IP-aadressi kasutab teatud juures porti. Selle dokumendi näitab, kuidas PowerShelli abil saate luua uue laadi koormusetasakaalustusteenuse või IP-aadresside lisamiseks mõne olemasoleva laadi koormusetasakaalustusteenuse SQL serveri kättesaadavus rühmade jaoks. 

Võimalus määrata mitu IP-aadressid on sisemine laadi koormusetasakaalustusteenuse on uus Azure ja kättesaadav ainult ressursihaldur mudel. Selle toimingu tegemiseks peate olema juurutatud Azure'i virtuaalmasinates ressursihaldur mudel SQL serveri kättesaadavus rühma. Nii SQL serveri virtuaalmasinates peab kuuluma samu kättesaadavus. [Microsoft malli](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) abil saate luua automaatselt kättesaadavus rühma Azure'i ressursihaldur. See mall loob automaatselt kättesaadavus rühma, sh sisemise koormuse koormusetasakaalustusteenuse teie eest. Soovi korral saate [ka kättesaadavusrühma käsitsi konfigureerimine](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

See teema nõuab, et kättesaadavus rühm on juba konfigureeritud.  

Seotud teemad on järgmised:

- [Azure'i VM (GUI) Kättesaadavusrühmad konfigureerimine](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Azure'i ressursihaldur ja PowerShelli abil VNet-VNet ühenduse konfigureerimine](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Windowsi tulemüüri konfigureerimine

Windowsi tulemüüri SQL serveri juurdepääsu lubamiseks konfigureerimine. Peate tulemüüri lubama ühendusi TCP pordid kasutamine SQL serveri eksemplar, kui ka kuulajale juures kasutatud pordi konfigureerimiseks. Üksikasjalikud juhised teemast [konfigureerimine Windowsi tulemüüri andmebaasi mootor juurdepääsu](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Pordi SQL serveri ja pordi juures sissetuleva reegli loomine.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Näide skripti: Luua ka sisemise koormuse koormusetasakaalustusteenuse PowerShelli abil

> [AZURE.NOTE] Kui olete loonud kättesaadavus rühma [Microsoft malli](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) laadi peate seda toimingut. 

Järgmist PowerShelli skripti loob ka sisemise koormuse koormusetasakaalustusteenuse, konfigureerib tasakaalustamiseks reeglid laadi ja laadi koormusetasakaalustusteenuse seab IP-aadress. Skripti käivitamiseks avage Windows PowerShell ISE ja skripti kleepimine skripti paanil. Kasutage `Login-AzureRMAccount` login PowerShelli. Kui teil on mitu Azure tellimust, kasutage `Select-AzureRmSubscription ` seadmiseks tellimus. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Näide skripti: IP-aadressi lisamiseks mõne olemasoleva laadi koormusetasakaalustusteenuse PowerShelli abil

Kui soovite kasutada rohkem kui ühte kättesaadavus rühma, täiendav IP-aadressi lisamiseks mõne olemasoleva laadi koormusetasakaalustusteenuse PowerShelli kasutamine. Iga IP-aadressi jaoks on vaja oma laadi reegel, juures port ja eesmise port.

Pordi ees on port rakenduste abil ühenduse loomine SQL serveri eksemplar. IP-aadresside kättesaadavus eri rühmade abil saate sama esiosa port.

>[AZURE.NOTE] SQL serveri kättesaadavus rühmade jaoks nõuab iga IP-aadressi pordi teatud juures. Näiteks kui ühe IP-aadressi laadi koormusetasakaalustusteenuse kasutab juures pordi 59999, ei ole muud IP-aadresside selle laadi koormusetasakaalustusteenuse saate kasutada juures pordi 59999. 

- Laadi koormusetasakaalustusteenuse teavet piirangud leiate jaotises [Networking piirid - Azure'i ressursihaldur](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits) **Privaatne ette lõpetamine IP laadi koormusetasakaalustusteenuse kohta** .

- Kättesaadavus kohta teavet teemast piirangud [piirangud (kättesaadavus rühmad)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Järgmise skripti lisatakse mõne olemasoleva laadi koormusetasakaalustusteenuse uue IP-aadress. Värskendage keskkonna muutujaid. Funktsiooni ILB kasutab kuulajale pordi koormust tasakaalustavad esiosa pordi. Selle pordi võib olla SQL serveri kuulamise port. See on vaikimisi on SQL serveri pordi 1433. Laadi tasakaalustamiseks reegel kättesaadavus rühma nõuab ujuv IP (saatja otsest server) nii, et tagasi end pordi on sama, mis esiosa pordi.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Laadi koormusetasakaalustusteenuse IP-aadressi kasutada kobar konfigureerimine 

Järgmiseks on kuulajale konfigureerimine klaster ja tuua kuulajale võrgus. Selleks tehke järgmist. 

1. Tõrkesiirdeklastri kuulajale kättesaadavus rühma loomine  

2. Tuua kuulajale võrgus

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. tõrkesiirdeklastri kuulajale kättesaadavus rühma loomine

Selles etapis tuleb lisada kliendi juurdepääsu punkti tõrkesiirdeklastri Tõrkesiirde halduriga ja konfigureerige kobar ressursi kuulata juures pordi PowerShelli abil. 

1. Kasutada RDP ühenduse loomiseks Azure virtuaalse masina, mis majutab esmane koopia. 

2. Avatud tõrkesiirdeklastri halduri.

3. Valige **võrgu** sõlm ja kobar võrgu nimi. Kasutage seda nime soovitud `$ClusterNetworkName` muutuv PowerShelli skripti.

4. Laiendage kobar nimi ja klõpsake **rollid**.

5. **Rollide** paanil Paremklõpsake kättesaadavus rühma nimi ja valige **Ressursi lisamine** > **Kliendi pääsupunkti**.

6. Väljal **nimi** selle uue kuulajale nime loomine ja seejärel kaks korda nuppu **edasi** ja seejärel klõpsake nuppu **valmis**. Too kuulajale või online ressursi sel hetkel.
 
    Uue kuulajale nimi on kasutavate rakenduste kättesaadavus rühma SQL Serveri andmebaasiga ühenduse võrgu nimi.

7. Klõpsake vahekaarti **ressursid** ja seejärel laiendage äsja loodud kliendi pääsupunkti. Paremklõpsake IP ressursi ja klõpsake käsku Atribuudid. Pange tähele nime IP-aadress. Seda nime kasutate selle `$IPResourceName` muutuv PowerShelli skripti.

8. **IP** -aadress suvandit **Staatiline IP-aadressid** ja seadke staatiline IP-aadress sama aadress, kui seate Azure'i portaalis laadi koormusetasakaalustusteenuse IP-aadress. 

9. Keela NetBIOS selle aadressi ja klõpsake nuppu **OK**. Korrake seda toimingut iga IP ressurss, kui teie lahendus ulatub üle mitme Azure'i VNets. 

10. Muuta SQL serveri kättesaadavus rühma ressurssi sõltuvad IP-aadress. Ressursi halduri paremklõps, see on **ressursid** vahekaardil jaotises **Muud ressursid**. 

11. Kobar sõlme, mis praegu majutab esmane koopia, avage mõni administraatoriõigustes PowerShell ISE ja kleepige uue skripti järgmised käsud. Klõpsake vahekaardil **sõltuvused** kuulajale nime.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Värskendage muutujate ja käivitage PowerShelli skripti IP-aadress ja pordi konfigureerimiseks uus kuulajale.

 >[AZURE.NOTE] Kui teie SQL-i serverid on eraldi regioonid, peate käivitama PowerShelli skripti kaks korda. Esimest korda kasutada kobar võrgu nimi, kobar IP ressursinimi ja esimese ressursirühm alla laadida koormusetasakaalustusteenuse IP-aadress. Teist korda kasutada kobar võrgu nimi, kobar IP ressursinimi ja alla laadida teise ressursirühm koormusetasakaalustusteenuse IP-aadress.

Klaster on nüüd kättesaadavus rühma kuulajale ressursi.

## <a name="2-bring-the-listener-online"></a>2 tuua kuulajale võrgus

Kättesaadavus rühma kuulajale ressursi konfigureeritud, saate tuua kuulajale võrgus nii, et luua rakendusi andmebaasidele, mis on kuulajale jaotises kättesaadavus.

1. Liikuge tagasi abil tõrkesiirdeklastri halduri. Laiendage **rollid** ja seejärel tõsta kättesaadavus rühma. Vahekaardil **ressursid** paremklõpsake kuulajale nimi ja klõpsake käsku **Atribuudid**.

1. Klõpsake vahekaarti **sõltuvused** . Kui loendis on loetletud mitu ressursid, veenduge, et IP-aadressid on või pole ja sõltuvused. Klõpsake nuppu **OK**.

1. Paremklõpsake kuulajale nimi ja klõpsake nuppu **Too veebis**.

1. Kui kuulajale on veebis, vahekaardil **ressursid** , paremklõpsake kättesaadavus rühma ja klõpsake käsku **Atribuudid**.

1. Looge sõltuvus kuulajale nimi ressursi (mitte IP address ressursid nimi). Klõpsake nuppu **OK**.

1. Käivitage SQL Server Management Studio ja ühendage peamine koopia.

1. Liikuge **AlwaysOn kõrge-saadavus** | **kättesaadavus rühmad** | **kättesaadavus rühma kuulajatele**. 

1. Nüüd näete kuulajale nimi, mille olete loonud tõrkesiirdeklastri halduris. Paremklõpsake kuulajale nimi ja klõpsake käsku **Atribuudid**.

1. Klõpsake väljal **Port** määrata pordinumber kättesaadavus rühma kuulajale, kasutades funktsiooni $EndpointPort varem kasutatud (1433 oli vaikimisi), seejärel klõpsake nuppu **OK**.

Nüüd on teil kättesaadavus rühma SQL Server Azure'i virtuaalmasinates ressursihaldur režiimis. 

## <a name="test-the-connection-to-the-listener"></a>Kuulajale ühenduse testimiseks

Ühenduse testimiseks tehke järgmist.

1. RDP SQL Server, mis on virtuaalse samasse võrku, kuid ei ole oma koopia. See võib olla muude SQL serveri klaster.

2. Ühenduse testimiseks kasutada **sqlcmd** kasuliku. Näiteks järgmine skript loob **sqlcmd** ühenduse kaudu Windowsi autentimist kuulajale peamine koopia:

    ```
    sqlmd -S <listenerName> -E
    ```

    Kui kuulajale kasutab vaikesättest port port (1433), määrake port ühendusstring. Näiteks ühendab on kuulajale Port 1435 sqlcmd järgmine käsk: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Ühenduse sqlcmd automaatselt loob ühenduse SQL serveri eksemplar sellest, millist majutab esmane koopia. 

>[AZURE.NOTE] Veenduge, et teie määratud pordi on nii SQL-i serverite tulemüüris avatud. Nii vaja sissetulevast TCP pordi, mida kasutate. Lisateabe saamiseks vaadake [lisamine või redigeerimine tulemüüri reegel](http://technet.microsoft.com/library/cc753558.aspx) . 

## <a name="guidelines-and-limitations"></a>Juhised ja piirangud

Võtke arvesse järgmisi juhiseid kättesaadavus rühma kuulajale Azure sisemine kasutamise kohta laadimine koormusetasakaalustusteenuse.

- Laadi koos sisemine koormusetasakaalustusteenuse pääsete kuulajale kaudu sama virtuaalse võrgustikus.

## <a name="for-more-information"></a>Lisateabe saamiseks

Lisateabe saamiseks vaadake teemat [konfigureerimine alati sisse-saadavus rühmitamine Azure VM käsitsi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShelli cmdlet-käsud

Järgmine PowerShelli cmdlet-käskude abil saate luua ka sisemise koormuse koormusetasakaalustusteenuse Azure'i virtuaalmasinates jaoks.

- `New-AzureRmLoadBalancer`Laadi koormusetasakaalustusteenuse loob. Lisateabe saamiseks vaadake [New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`loob ees laadi koormusetasakaalustusteenuse konfigureerimine IP. Lisateabe saamiseks vaadake [New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`loob jaoks laadi koormusetasakaalustusteenuse konfiguratsiooni reegel. Lisateabe saamiseks vaadake [New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`loob kirjutamata aadressi rakenduskausta konfiguratsiooni laadi koormusetasakaalustusteenuse. Lisateabe saamiseks vaadake [New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`loob jaoks laadi koormusetasakaalustusteenuse konfiguratsiooni juures. Lisateabe saamiseks vaadake [New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Kui teil on vaja laadi koormusetasakaalustusteenuse eemaldamine on Azure ressursirühm, kasutage `Remove-AzureRmLoadBalancer`. Lisateavet leiate teemast [Eemalda-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
