<properties
   pageTitle="Hallata oma virtuaalmasinates Azure PowerShelli abil | Microsoft Azure'i"
   description="Siit saate teada, ülesannete automatiseerimiseks programmis oma virtuaalmasinates haldamiseks kasutatavad käsud."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Azure'i PowerShelli abil oma virtuaalmasinates haldamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Iga päev, et hallata oma VMs teha paljusid toiminguid automatiseerida Azure PowerShelli cmdlet-käskude abil. See artikkel annab teile näide käsud lihtsam tööülesanded ja linke artiklitele, mis käskude keerukamate tööülesannete kuvamine.

>[AZURE.NOTE] Kui te pole veel installinud ja konfigureerinud Azure PowerShelli veel, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md)artikli juhiste saamiseks.

## <a name="how-to-use-the-example-commands"></a>Näide käskude kasutamine
Peate osa tekstist käsud asendada teksti, mis sobib teie keskkonnas. Funktsiooni < ja > sümbolid näitavad tekst, mida soovite asendada. Kui asendate tekst, sümbolid eemaldada, aga hinnapakkumise märgid alles jätta.

## <a name="get-a-vm"></a>Saada VM
See on lihtne ülesanne harva. Selle abil saate VM kohta teabe saamine, tehke toiminguid VM või saada väljund talletamiseks muutujana.

Käivitage VM kohta teabe saamiseks selle käsu, asendades kõik jutumärgid, sh selle < ja > märgid:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Salvestada väljund $vm muutuja, käivitage.

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Windowsi-põhiste VM sisse logida

Käivitage järgmised käsud:

>[AZURE.NOTE] **Get-AzureVM** käsu Kuva saad virtuaalse masina ja pilvepõhise teenuse nimi.
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>VM peatamine

Käivitage see käsk:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Selle parameetri abil saate hoida virtuaalse IP (VIP) pilveteenusesse, juhuks, kui see on viimase VM, et pilveteenuses. <br><br> Kui kasutate StayProvisioned parameeter, saate endiselt esitatakse arve VM.

## <a name="start-a-vm"></a>Käivitage VM

Käivitage see käsk:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Andmete kettal manustamine
Selle toimingu jaoks on vaja mõned juhised. Esmalt kasutada funktsiooni *** lisa-AzureDataDisk *** cmdlet-käsu ketas $vm objekti lisamine. Seejärel kasutage **Update-AzureVM** cmdlet-käsk VM konfiguratsiooni värskendamiseks.

Samuti peate otsustada, kas soovite lisada uue ketta või üks, mis sisaldab andmeid. Uue ketta, käsk .vhd faili loob ja lisab selle.

Saate lisada uue ketta, käivitage see käsk:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Manusta olemasolevat andmete ketast, käivitage see käsk:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Olemasoleva .vhd faili sisse bloobimälu manustamine andmete ketast, käivitage see käsk:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Windowsi-põhiste VM loomine

Luua uue Windowsi-põhiste virtuaalse masina Azure, kasutamine [Azure PowerShelli kasutamine loomiseks ja Windowsi-põhiste virtuaalmasinates preconfigure](virtual-machines-windows-classic-create-powershell.md)juhiseid. See teema juhiseid teid läbi mõne Azure PowerShelli käsu määrata loomine loob Windowsi-põhiste VM, mida saate eelkonfigureeritud:

- Active Directory domeeni liikmeskonnaga.
- Täiendavad ketast.
- Olemasoleva koormusetasakaalustusega liikme määrata.
- Staatiline IP-aadress.
