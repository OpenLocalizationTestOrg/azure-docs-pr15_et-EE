<properties
    pageTitle="Ühenduse loomine rakenduse virtuaalse võrgu PowerShelli abil"
    description="Juhised ühenduse loomiseks ja nendega töötada virtuaalne võrkude PowerShelli abil"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Ühenduse loomine rakenduse virtuaalse võrgu PowerShelli abil #

## <a name="overview"></a>Ülevaade ##

Azure'i rakenduse teenus, saate luua ühenduse rakenduse (web, mobiil või API) Azure virtuaalse võrgu (VNet) teie tellimus. See funktsioon on nn VNet integreerimine. Funktsiooni VNet integreerimine ei tohiks segi rakenduse teenuse keskkonna funktsiooni, mille abil saate käivitada Azure'i rakendust Service virtuaalse võrgus.

Funktsiooni VNet integreerimine on kasutajaliidese (UI) abil saate integreerida virtuaalne võrkude klassikaline juurutamise mudeli või Azure ressursihaldur juurutamise mudeli abil juurutatud uus portaalis. Kui soovite lisateavet funktsiooni, lugege teemat [integreerida rakenduse Azure virtuaalse võrguga](web-sites-integrate-with-vnet.md).

See artikkel kehtib mitte sellest, kuidas kasutada UI vaid pigem PowerShelli abil integreerimise lubamise kohta. Kuna iga juurutamise mudeli käsud on erinevad, see artikkel sisaldab iga juurutamise mudeli jaotise.  

Enne jätkamist selle artikliga, veenduge, et teil on:

- Viimane Azure PowerShelli SDK installitud. Saate selle installida Web platvormi Installer.
- Rakenduse Azure'i rakendust Service Standard või Premium SKU-ga.

## <a name="classic-virtual-networks"></a>Klassikaline virtuaalse võrgu ##

Selles jaotises selgitatakse kolme ülesannete jaoks virtuaalse võrkudes, milles klassikaline juurutamise mudeli kasutamine.

1. Rakenduse ühenduse olemasoleva virtuaalse võrk, mis on lüüsi ja on konfigureeritud lubama punkti saidi Ühenduvus.
1. Oma rakenduse virtuaalse võrgu integreerimine teabe värskendamine.
1. Rakenduse katkestamine virtuaalse võrgu.

### <a name="connect-an-app-to-a-classic-vnet"></a>Klassikaline VNet rakenduse ühenduse loomine ###

Rakenduse ühenduse virtuaalse võrgu tehke kolme järgmist.

1. Veebirakenduse deklareerida, et see ühinevad konkreetse virtuaalse võrgu. Rakendus loob virtuaalse võrgu punkti saidi ühenduvuse pööratakse sert.
1. Web appi serdi üleslaadimine virtuaalse võrgu ja seejärel laadida punkti saidi VPN paketi URI.
1. Värskendage web appi virtuaalse võrguühenduse punkti saidi paketi URI.

Esimene ja kolmas toimingud on täielikult Skriptattavat, kuid teise etapi nõuab ühekordse, käsitsi toimingu kaudu portaali või juurdepääs **sellele** või **paik** toimingute teostamiseks virtuaalse võrgu Azure'i ressursihaldur lõpp-punkti. Azure tugiteenuste on see lubatud. Enne alustamist veenduge, et teil on klassikaline virtuaalse võrku, mis on juba lubatud punkti saidi ühenduvust ja juurutatud lüüsi. Lüüsi loomine ja lubamine punkti saidi Ühenduvus, peate kasutama portaali, nagu on kirjeldatud aadressil [VPN-lüüsi loomine][createvpngateway].

Klassikaline virtuaalse võrgu peab olema sama nimega oma rakenduse teenuse leping, rakenduse malliga, millele teil on integreerimise tellimus.

##### <a name="set-up-azure-powershell-sdk"></a>Azure'i PowerShelli SDK häälestamine #####

Avage PowerShelli aken ja häälestada oma Azure'i konto ja tellimuse abil.

    Login-AzureRmAccount

See käsk avab viip, et saada Azure mandaat. Kui olete sisse loginud, kasutage ühte järgmistest käskudest valige tellimus, mida soovite kasutada. Veenduge, et kasutate tellimuse, mis on teie virtuaalse võrgu ja rakenduse teenusleping.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

või

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Selles artiklis kasutatud muutujad #####

Käskude lihtsustamiseks seatakse **$Configuration** PowerShelli muutuja teatud konfiguratsioon.

Määrata muutuja järgmiselt PowerShellis järgmiste parameetritega.

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Rakenduse asukoht peaks olema asukoht ilma tühikuteta. Näiteks Lääne USA on westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Järgmise üksuse on, kus serdi tuleb kirjutada. Peaks olema kirjutatav tee kohalikus arvutis. Veenduge, et kaasata CER lõpus.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Kui soovite näha, mida saate määrata, tippige **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Selles jaotises ülejäänud eeldab, et teil on loodud ainult kirjeldatud muutuja.

##### <a name="declare-the-virtual-network-to-the-app"></a>Deklareerida virtuaalse võrgu rakendus #####

Järgmine käsk abil saate kindlaks teha rakenduse, et selle abil kindla virtuaalse võrku. See võib põhjustada rakenduse loomiseks vajalikud serdid:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Kui see käsk õnnestub, **$vnet** peaks olema **Atribuudid** muutuja ei. **Atribuutide** muutuja peaks sisaldama nii serdi sõrmejälje ja serdi andmeid.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Virtuaalse võrgu web appi serdi üleslaadimine #####

Käsitsi, on vaja iga tellimuse ja virtuaalse võrgu kombinatsiooni ühekordse etappi. See tähendab, kui ühendate rakenduste tellimus A virtuaalse võrgu A-ni, pead seda toimingut teha ainult siis, kui sõltumata sellest, mitu rakendusi saate konfigureerida. Kui soovite lisada uue rakenduse teise virtuaalse võrku, peate selle uuesti tegemiseks. Selle põhjuseks on, et kogumi serdid on loodud Azure'i rakendust Service tasemel tellimus ja määramine luuakse üks kord iga virtuaalse võrgu, mis loob ühenduse rakendused.

Serdid on juba määratud Kui järgisite järgmist või kui te integreeritud virtuaalse samasse võrku portaalis.

Kõigepealt tuleb luua CER-fail. Teise etapi on virtuaalse võrgu CER-fail üles laadida. Luua API kõne varasemas etapis CER-fail, käivitage järgmised käsud.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Serti ei leitud asukohas selle **$Configuration.GeneratedCertificatePath** saate määrata.

Laadige üles sert käsitsi, kasutage [Azure portaali] [ azureportal] ja **Liikuge virtuaalse võrgu (klassikaline)** > **VPN-ühendused** > **SharePointi saidil** > **haldamine serdid**. Laadige siit oma sert.

##### <a name="get-the-point-to-site-package"></a>Punkti saidi paketi hankimine #####

Järgmiseks web Appile virtuaalse võrguühenduse loomisel on saada punkti saidi paketi ja anda oma veebirakenduse.

Salvestage fail nimega GetNetworkPackageUri.json kusagil arvutis, näiteks C:\Azure\Templates\GetNetworkPackageUri.json järgmist malli.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Sisendparameetrite seadmiseks tehke järgmist.

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Kõne skripti:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Muutuja **$output. Outputs.packageUri** sisaldab nüüd paketi URI antakse oma veebirakenduse.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Rakenduse punkti saidi paketi üleslaadimine #####

Viimane toiming on see pakett rakendus pakkuda. Lihtsalt käivitage järgmine käsk:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Kui sõnum palub teil kinnitada, et teil on mõne olemasoleva ressursi ülekirjutamise, veenduge, et selle lubama.

Kui see käsk on loodud, teie rakendus peaks nüüd ühendatud virtuaalse võrku. Kontrollimaks, et edu, minge oma rakenduse konsooli, ja tippige järgmine käsk:

    SET WEBSITE_

Kui mõni muutuja nimega WEBSITE_VNETNAME, mis sisaldab väärtust, mis vastab target virtuaalse võrgu nimi, õnnestunud kõikide jaoks.

### <a name="update-classic-vnet-integration-information"></a>Klassikaline VNet integreerimine teabe värskendamine ###

Värskendada või resync oma teabe, lihtsalt korrake toiminguid, mida olete täitnud integreerimine kõigepealt loomisel. Need juhised on:

1. Määratleda oma konfiguratsiooniteavet.
1. Rakenduse virtuaalse võrgu deklareerida.
1. Punkti saidi paketi hankimine
1. Laadige oma rakenduse punkti saidi paketi.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Klassikaline VNet rakenduse katkestamine ###

Rakenduse katkestamiseks peate konfiguratsiooni teavet, mis on seatud, virtuaalse võrgu integreerimise ajal. Selle teabe abil ei seejärel üks käsk virtuaalse võrgu rakenduse katkestamine.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Ressursihaldur virtuaalne võrkude ##

Ressursihaldur virtuaalne võrkude on Azure ressursihaldur API-d, mis lihtsustavad mõned protsessid klassikaline virtuaalse võrgu võrreldes. Meil on skripti, mis aitavad teil teha järgmist:

- Ressursihaldur virtuaalse võrgu loomine ja rakenduse integreerida.
- Lüüsi loomine, punkti saidi ühenduvuse konfigureerimine olemasoleva ressursihaldur virtuaalse võrku ja seejärel oma rakenduse integreerida see.
- Rakenduse integreerida olemasoleva ressursihaldur virtuaalse võrku, mis on lüüsi ja punkti saidi ühenduvuse lubatud.
- Rakenduse katkestamine virtuaalse võrgu.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Ressursihaldur VNet rakenduse teenuste integreerimine skripti ###

Kopeerige järgmine skript ja salvestage see fail. Kui te ei soovi kasutada skripti, võite näha, kuidas häälestada asju ressursihaldur virtuaalse võrguga õppida.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Salvesta koopia skript. Selles artiklis seda nimetatakse V2VnetAllinOne.ps1, kuid saate kasutada mõne muu nimi. Ei ole argumente seda skripti. Te lihtsalt käivitage see. Kõigepealt tuleb teha script on paluks teil sisse logida. Kui olete sisse loginud, skript saab teie konto üksikasjad ja tagastab loendi tellimustest. Arvestamata taotluse mandaadi, algse skripti täitmise näeb välja selline:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Demo tellimuse (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Lilla Demo tellimuse (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Valige soovitud suvand: 3

    Konto: ccompy@microsoft.com keskkonnas: AzureCloud tellimus: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d rentniku: 722278f-fef1-499f-91ab-2323d011db47

    Sisestage oma rakenduse ressursirühma: hcdemo-rg sisestage oma rakenduse nimi: v2vnetpowershell mida te soovite teha?

    1) UUE virtuaalse võrgu lisamine rakendusse
    2) OLEMASOLEVAT virtuaalse võrku lisamine rakendusse
    3) Rakenduse virtuaalse võrgu eemaldamine

Ülejäänud selles jaotises selgitatakse iga need kolm võimalust.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Ressursihaldur VNet loomine ja selle integreerimine ###

Saate luua uue virtuaalse võrk, mis kasutab ressursihaldur juurutamise mudeli ja integreerida rakenduse, valige **1) lisamine rakendusse uue virtuaalse võrgu**. See küsib teilt virtuaalse võrgu nimi. Minu puhul, nagu näete järgmisi sätteid saab kasutada nime v2pshell.

Skripti annab loodavale virtuaalse võrgu üksikasjad. Kui ma ei soovi, saate muuta mis tahes väärtused. Selles näites täitmisel loodud virtuaalse võrgu, mis sisaldab järgmisi sätteid:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Kui soovite muuta, väärtused, tippige **Y** ja tehke soovitud muudatused. Kui olete rahul, virtuaalse võrgu sätteid, tippige **N** või lihtsalt sisestusklahvi küsimise sätete muutmise kohta. Sealt edasi on lõpetatud, skripti ütleb teile, mida see osa ' i 's tehes seni, kuni see algab virtuaalse võrgu lüüsi loomine. See toiming saate kuluda kuni tund. Selles etapis ei ole Edenemisnäidik, kuid skripti anname teile teada, millal on loodud lüüsi.

Skripti lõpulejõudmisel kirjas **lõpetatud**. Selles etapis, on teil ressursihaldur virtuaalse võrk, mis sisaldab nime ja teie valitud sätted. Uue virtuaalse võrku ühendatakse ka oma rakendusega.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Olemasoleva ressursihaldur VNet rakenduse integreerimine ###

Kui olete integreerimise olemasoleva virtuaalse võrk, kui annate ressursihaldur virtuaalse võrk, mis ei sisalda lüüsi või punkti saidi Ühenduvus, skript loob mis. Kui soovitud VNET on juba need asjad, mis on loodud, läheb skript otse integreerimise rakendus. Selle protsessi käivitamiseks valige lihtsalt **2) lisamine rakendusse on olemasolev virtuaalse võrgu**.

See suvand toimib ainult siis, kui teil on olemasoleva ressursihaldur virtuaalse võrk, mis on sama tellimuse oma rakendusena. Kui olete valinud suvandi, esitatakse ressursihaldur virtuaalse võrguga loendiga.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Valige soovitud suvand: 5

Valige lihtsalt virtuaalse võrgu, mida soovite integreerida. Kui teil on juba lüüsi, mis sisaldab punkti saidi ühenduvuse lubatud, lihtsalt integreerida skripti virtuaalse võrguga rakenduse. Kui teil pole lüüsi, peate määrata lüüsi alamvõrgu. Lüüsi teie alamvõrku peab olema oma virtuaalse võrgu aadressiruumi ja ei saa see olla mis tahes muud alamvõrgu. Kui teil on virtuaalse võrgu ilma lüüsi ja käivitage see toiming, mida näevad välja järgmised:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

Selles näites loodud virtuaalse võrgu lüüsi, mis sisaldab järgmisi sätteid:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Kui soovite neid sätteid muuta, saate seda teha. Muul juhul vajutage sisestusklahvi Enter ja skript loob teie lüüsi ja rakenduse manustamiseks virtuaalse võrgu. Lüüsi loomise ajal on veel üks tund, kuigi, veenduge, et te seda silmas pidama. Kui kõik on valmis, ütlevad skripti **lõpetatud**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Ressursihaldur VNet rakenduse katkestamine ###

Rakenduse ühenduse katkestamine virtuaalse võrgu lüüsi ette võtta või keelata punkti saidi Ühenduvus. Siiski, et kasutate selle midagi muud. See ka ühendage see teistest rakendustest, ei ole esitatud. Selle toimingu tegemiseks valige **3) virtuaalse võrgu eemaldamine rakendusest**. Kui te seda teete, kuvatakse umbes selline:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Kuigi skripti ütleb kustutamine ei kustuta virtuaalse võrgu. See on lihtsalt eemaldamine integreerimine. Pärast võite kinnitada, et see on, mida soovite teha, käsk töödeldakse kiiresti ja ütleb teile **väärtuse True** , kui see on valmis.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
