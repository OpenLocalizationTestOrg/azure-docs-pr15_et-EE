<properties
    pageTitle="Vastastikuste laadi koormusetasakaalustusteenuse rakenduses Azure'i ressursihaldur abil Azure'i CLI IPv6-ga Interneti loomine | Microsoft Azure'i"
    description="Saate teada, kuidas luua Interneti vastastikuste laadi koormusetasakaalustusteenuse rakenduses Azure'i ressursihaldur abil Azure'i CLI IPv6-ga"
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

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Laadi koormusetasakaalustusteenuse rakenduses Azure'i ressursihaldur abil Azure'i CLI IPv6-ga vastastikuste Interneti loomine

> [AZURE.SELECTOR]
- [PowerShelli](./load-balancer-ipv6-internet-ps.md)
- [Azure'i CLI](./load-balancer-ipv6-internet-cli.md)
- [Mall](./load-balancer-ipv6-internet-template.md)

Mõne Azure laadi koormusetasakaalustusteenuse on kiht-4 (TCP, UDP) laadi koormusetasakaalustusteenuse. Laadi koormusetasakaalustusteenuse pakub kõrge-saadavus jagades liiklust terve teenuse pilveteenuste aknad või virtuaalmasinates laadi koormusetasakaalustusteenuse komplekti vahel. Azure'i laadi koormusetasakaalustusteenuse saate esitada ka nende teenuste mitme pordid, mitu IP-aadressi või mõlemad.

## <a name="example-deployment-scenario"></a>Näide juurutamise stsenaarium

Järgmine diagramm näitab tasakaalustamiseks lahenduse laadi kasutusele selles artiklis kirjeldatud näide malli abil.

![Laadi koormusetasakaalustusteenuse stsenaarium](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Selle stsenaariumi loote Azure järgmistest allikatest:

- kahe virtuaalmasinates (VM)
- virtuaalse võrgu kasutajaliidese jaoks iga VM määratud nii IPv4 ja IPv6 aadressid
- mõne IPv4 ja IPv6 avaliku IP-aadress on Interneti-ühendusega laadi koormusetasakaalustusteenuse
- Kättesaadavus seadmine abil, mis sisaldab kahte VMs
- kaks laadimine tasakaalustavad reeglid avaliku VIP vastendamiseks privaatne lõpp-punktid

## <a name="deploying-the-solution-using-the-azure-cli"></a>Kasutades Azure CLI lahenduse juurutamine

Järgmised toimingud näitab, kuidas luua vastastikuste laadi koormusetasakaalustusteenuse Azure'i ressursihaldur funktsiooniga CLI Interneti. Koos Azure ressursihaldur, iga ressursi on loodud ja konfigureeritud eraldi ja seejärel panete koos ressursi loomiseks.

Laadi koormusetasakaalustusteenuse juurutamiseks loomist ja konfigureerimist järgmiste objektide:

- IP-konfiguratsiooni ees - sisaldab avaliku IP-aadresside sissetulevat võrguliiklust.
- Tagaandmebaas aadress pool - sisaldab virtuaalmasinates saada võrguliiklust laadi koormusetasakaalustusteenuse võrgu liidesed (NICs).
- Reeglite koormusetasandus - sisaldab vastendamise avaliku porti laadi koormusetasakaalustusteenuse pordi tagaandmebaas aadress kogumi reeglid.
- Sissetulevad reeglid NAT - sisaldab avaliku porti koormusetasakaalustusteenuse laadimine ja teatud virtuaalse masina tagaandmebaas aadress rakenduskausta pordi vastendamise reeglid.
- Sondid – sisaldab seisund sondid kasutada saadavuse virtuaalmasinates eksemplaride tagaandmebaas aadress kogumi.

Lisateabe saamiseks lugege teemat [Azure ressursihaldur laadi koormusetasakaalustusteenuse kasutajatugi](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Azure'i ressursihaldur kasutama CLI keskkonna häälestamine

Selle näite puhul me töötab CLI tööriistad PowerShelli käsuviiba aken. Me ei kasuta Azure PowerShelli cmdletid, kuid me kasutame PowerShelli 's skriptimine võimalusi parandada loetavuse ja taaskasutuse.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.

2. Ressursihaldur lülitumine käsu **azure config režiim** .

        azure config mode arm

    Oodatav väljund:

        info:    New mode is arm

3. Azure'i sisse logida ja tellimuste loendit.

        azure login

    Sisestage oma Azure mandaat, kui seda küsitakse.

        azure account list

    Valige tellimus, mida soovite kasutada. Märkige üles tellimuse Id järgmise juhise jaoks.

4. PowerShelli muutujate CLI käsud jaoks häälestada.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Ressursirühma, laadi koormusetasakaalustusteenuse, virtuaalse võrgu ja alamvõrku loomine

1. Ressursi rühma loomine

        azure group create $rgName $location

2. Laadi koormusetasakaalustusteenuse loomine

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Saate luua virtuaalse võrgu (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Looge kaks alamvõrku selle VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Avaliku IP-aadresside ees kausta loomine

1. PowerShelli muutujate häälestamine

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Luua avaliku IP-aadressi ees IP pool.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Laadi koormusetasakaalustusteenuse kasutab selle FQDN avaliku IP domeeni silt. See klassikaline kasutuselevõtu, mis kasutab pilveteenusesse muudatuse nime laadi koormusetasakaalustusteenuse FQDN.
    >Selles näites on FQDN *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Ees- ja luua

Selles näites loob ees IP kogumi saab sissetuleva võrguliikluse koormusetasakaalustusteenuse laadimine ja tagaandmebaas IP pool, kus ees pool saadab koormus tasakaalustatud võrguliiklust.

1. PowerShelli muutujate häälestamine

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Saate luua ees IP pool koormusetasakaalustusteenuse laadimine ja eelmises etapis loodud avaliku IP seostada.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Juures, NAT reeglid ja LB reeglite loomine

Selles näites luuakse järgmised üksused:

- TCP 80 pordiga ühenduvuse kontrollimiseks reegli juures
- reegli NAT tõlkida kõik liiklust port 3389 pordi 3389 RDP<sup>1</sup>
- reegli NAT tõlkida kõik liiklust Port 3391 pordi 3389 RDP<sup>1</sup>
- Laadi koormusetasakaalustusteenuse reegli kõik liiklust port 80 tagaandmebaas kogumi aadressid porti 80 tasakaalustamiseks.

<sup>1</sup> NAT reeglid on seotud kindla virtuaalse masina eksemplari laadi koormusetasakaalustusteenuse taha. Teatud virtuaalse masina ja seostatud reegli NAT pordi saadetakse saabuvate port 3389 võrguliiklust. Määrake reegli NAT protokolli (UDP või TCP). Nii Protokollid ei saa määrata sama port.

1. PowerShelli muutujate häälestamine

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Funktsiooni juures loomine

    Järgmises näites luuakse on TCP juures mis kontrollib Ühenduvus tagaandmebaas TCP pordi 80 iga 15 minutit. See märgi tagaandmebaas ressursi saadaval pärast kaks järjestikust ebaõnnestumist.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. NAT sissetulevad reeglid, mis võimaldavad RDP ühendused tagaandmebaas ressursside loomine

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Laadi koormusetasakaalustusteenuse reeglid, mis saadavad liikluse eri tagaandmebaas pordid sõltuvalt sellest, millist esiosa saanud kutse loomine

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Kontrollige sätteid

        azure network lb show --resource-group $rgName --name $lbName

    Oodatav väljund:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>NICs loomine

Looge NICs ja seostada NAT reeglid, laadi koormusetasakaalustusteenuse reeglid ja sondid.

1. PowerShelli muutujate häälestamine

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. NIC, iga tagaandmebaas jaoks luua ja lisada konfigureerimisel IPv6.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Tagaandmebaas VM ressursside loomine ja iga NIC manustamine

VMs loomiseks peab teil olema salvestusruumi konto. Koormusetasakaalustuseks, peavad olema mõne kättesaadavus määra liikmetele VMs. VMs loomise kohta leiate lisateavet teemast [loomine on Azure VM PowerShelli abil](../virtual-machines/virtual-machines-windows-ps-create.md)

1. PowerShelli muutujate häälestamine

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Selles näites kasutatakse vms avatekstina kasutajanimi ja parool. Sobiv peab olema ettevaatlik kasutades identimisteabe selge. Turvalisem meetod töötlemise PowerShellis mandaat, leiate teemast [Get-mandaati](https://technet.microsoft.com/library/hh849815.aspx) cmdlet-käsk.

2. Luua salvestusruumi konto ja -saadavus määramine

    Olemasoleva salvestusruumi konto võib kasutada, kui loote VMs. Järgmine käsk loob uue konto salvestusruumi.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Järgmisena looge kättesaadavus määramine.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Funktsiooni virtuaalmasinates seotud NICs loomine

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Järgmised sammud

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-cli.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
