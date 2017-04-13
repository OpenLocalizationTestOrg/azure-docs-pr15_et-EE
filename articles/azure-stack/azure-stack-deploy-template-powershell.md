<properties
    pageTitle="Mallide PowerShelliga Azure'i virnas juurutamine | Microsoft Azure'i"
    description="Saate teada, kuidas juurutamine virtuaalse masina ressursihaldur Mall ja PowerShelli abil."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Mallide PowerShelli kaudu Azure'i virnas juurutamine

PowerShelli abil saate juurutada Azure virnas POC Azure'i ressursihaldur mallid.  Ressursihaldur Mallid juurutada ja kõik ressursside rakenduse ühe ja koordineeritud toiming.

## <a name="run-azurerm-powershell-cmdlets"></a>Käivitage AzureRM PowerShelli cmdlet-käsud

Selles näites saate käivitada juurutada virtuaalse masina Azure'i virnas POC ressursihaldur malli abil.  Olete [installinud ja konfigureerinud PowerShelli](azure-stack-connect-powershell.md) veenduge enne jätkamist  

VHD, kasutatakse selle malli näide on turuplatsi vaikepildi (WindowsServer-2012 – R2-andmekeskuse).

1.  Minge <http://aka.ms/AzureStackGitHub>, otsige **101-lihtne windows vm** malli ja salvestage see järgmises asukohas: c:\\mallide\\azuredeploy-101-lihtne – windows – vm.json.

2.  PowerShelli, käivitage järgmine juurutamise skript. Asendage *kasutajanimi* ja *parool* oma kasutajanime ja parooliga. Edaspidised kasutusvõimaluste, inkrementida juurutamise ülekirjutamise vältimiseks *$myNum* parameetri väärtus.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Avage Azure'i virnas portaal, klõpsake nuppu **Sirvi**, valige **virtuaalmasinates**ning otsida virtual seadme (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Video näide: virtuaalse masina hübriidjuurutuse

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Järgmised sammud

[Visual Studio malle juurutamine](azure-stack-deploy-template-visual-studio.md)
