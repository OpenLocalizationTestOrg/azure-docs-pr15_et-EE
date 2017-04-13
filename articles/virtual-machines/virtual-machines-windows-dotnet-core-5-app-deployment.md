<properties
   pageTitle="Toimingute automatiseerimine rakenduse juurutamine virtuaalse masina laiendiga | Microsoft Azure'i"
   description="Azure virtuaalse masina DotNet Core õpetus"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Rakenduse juurutamine Azure'i ressursihaldur mallide kasutamine

Kui kõik Azure infrastruktuuri nõuetele tuvastatud ja tõlkida juurutamise malli tegeliku rakenduse juurutamine peaks käsitlema. Rakenduse juurutamine siin viitab installimist tegeliku rakenduse kahendfaile peale Azure ressursse. Muusika poe proovi, .net põhi- ja IIS-i peavad olema installitud ja konfigureeritud iga virtuaalse masina. Muusika poe kahendfaile tuleb peale virtual arvutisse olema installitud ja muusika poe andmebaas eelnevalt loodud.

Selle dokumendi üksikasjad, kuidas saate virtuaalse masina laiendid automatiseerida rakenduse juurutamine ja Azure'i virtuaalmasinates konfigureerimine. Kõik sõltuvused ja kordumatute konfiguratsioone on esile tõstetud. Parima kasutuskogemuse saamiseks eelnevalt juurutada eksemplari lahendus Azure tellimuse ja töö koos Azure ressursihaldur mall. Täielik malli leiate siit – [Muusika poe juurutamise Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Konfigureerimise skripti

Virtuaalse masina laiendid on eriotstarbeline programmid käivitada vastu virtuaalmasinates esitada konfiguratsiooni automatiseerimine. Laiendid on saadaval, nt viirusetõrje, logimine konfigureerimine ja keskmise suurusega konfiguratsiooni mitmel teatud otstarbel. Kohandatud skript laiend saab käivitada igal skripti virtuaalse masina suhtes. Muusika poe valimi, kus on kohandatud skript laiend konfigureerimine Windows virtuaalmasinates ja installida muusika poe rakendus.

Enne üksikasjalikult, kuidas Azure'i ressursihaldur mallis deklareeritud virtuaalse masina laiendid, uurida skripti, mida käitatakse. See skript konfigureerib Windowsi virtuaalse masina majutada muusika poe rakendus. Kui käivitate, skripti installitakse kõik vajalik tarkvara, installige poe rakendus muusika andmeallika juhtelemendi ja andmebaasi ettevalmistamine. 

> See näide on tegevused.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>VM skripti laiend

VM laiendid saab käivitada vastu virtuaalse masina Koosta ajal kaasamise laiend ressursi Azure'i ressursihaldur malli abil. Pikendamise saab lisada viisardiga Visual Studio lisada ressursi või lisades kehtiv JSON mall. Skripti laiend ressursi on pesastatud virtuaalse masina ressurss; See on näha Järgnevas näites.

Seda linki JSON valimi ressursihaldur Mall – [VM skripti laiend](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339)kuvamiseks. 

Teade kuvatakse allpool JSON, et script on talletatud GitHub. Azure'i bloobimälu võib ka salvestatakse see skript. Azure'i ressursihaldur Mallid luba ka skripti täitmise stringi ehitatakse nii, et malli parameetrite väärtused saate kasutada parameetrite skripti täitmist. Sel juhul andmed juurutamisel Mallid ja need väärtused saab kasutada skripti täitmisel.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Kohandatud skript laiend kasutamise kohta leiate lisateavet teemast [kohandatud skript laiendid ressursihaldur mallide abil](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Järgmise juhise juurde

<hr>

[Lisateavet Azure ressursihaldur Mallid uurimine](https://github.com/Azure/azure-quickstart-templates)
