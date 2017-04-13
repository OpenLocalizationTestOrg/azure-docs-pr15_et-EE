<properties
    pageTitle="Üles laadida Windows VHD ressursihaldur jaoks | Microsoft Azure'i"
    description="Siit saate teada, üleslaadimiseks Windows virtuaalse masina VHD asutusesisesest Azure, ressursihaldur juurutamise mudeli abil. Saate üles laadida: Kas on generalized on VHD või eriotstarbeline VM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Windows Azure on kohapealse VM VHD üleslaadimine 


Selles artiklis kirjeldatakse, kuidas luua ja üles laadida Windows virtuaalse kõvaketta (VHD) on Azure Vm loomisel kasutada. Saate üles laadida on VHD generalized VM või eriotstarbeline VM. 

Ketast ja Azure VHDs kohta leiate lisateavet teemast [ketast ja VHDs jaoks virtuaalmasinates kohta](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>VM ettevalmistamine 

Saate üles laadida nii generalized ja eriotstarbelisi VHDs Azure. Kindlat tüüpi nõuab, et valmistada VM enne alustamist.

- **Üldiste VHD** - generalized VHD olnud kogu teie isikliku konto teabe Sysprepi abil eemaldada. Kui kavatsete kasutada selle VHD pildina luua uue VMs kaudu, peaksite:
    - [Windowsi VHD üleslaadimiseks Azure'i ettevalmistamine](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Generalize virtuaalse masina Sysprepi abil](virtual-machines-windows-generalize-vhd.md). 

- **Eriotstarbelisi VHD** - eriotstarbelisi VHD säilitab Kasutajakontod, rakenduste ja muude olekus andmete oma algse VM. Kui kavatsete kasutada VHD nimega-on luua uus VM, tagada järgmiste juhiste täitmist. 
    - [Ettevalmistamine Windows VHD Azure'i üleslaadimine](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Ärge** üldistada VM Sysprepi abil.
    - Eemaldage kõik Külastajate virtualiseerimise tööriistad ja agentide, mis on installitud VM (st VMware tööriistad).
    - Veenduge, et tõmmata oma IP-aadress ja DNS-i sätted DHCP on konfigureeritud VM. See tagab, et server saab IP-aadressi sees on VNet käivitamisel. 

## <a name="log-in-to-azure"></a>Azure'i sisse logida

Kui teil pole veel PowerShelli versiooni 1,4 või installitud, lugege, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md)kohal.

1. Avage Azure'i PowerShelli ja Azure'i kontosse sisse logida. Saate sisestada mandaat Azure'i konto avatakse hüpikaknas.

    ```powershell
    Login-AzureRmAccount
    ```


2. Tellimuse ID hankimine saadaolevate tellimuste.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Määrake õige tellimuse abil tellimuse ID-ga. Asendage `<subscriptionID>` õige Tellimuse ID-ga.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Salvestusruumi konto

Peate salvestusruumi konto Azure üleslaaditud VM salvestada. Saate kasutada olemasolevat salvestusruumi kontot või looge uus. 

Tippige saadaolevat kontode kuvamiseks:

```powershell
Get-AzureRmStorageAccount
```

Kui soovite kasutada olemasoleva salvestusruumi konto, jätkake [VM pilt üles](#upload-the-vm-vhd-to-your-storage-account) jaotis.

Kui teil on vaja luua salvestusruumi konto, tehke järgmist.

1. Peate ressursirühm, kus luuakse salvestusruumi konto nimi. Soovite teada saada kõik teie tellimus on ressurss rühmad, tippige:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Nimega **myResourceGroup** piirkonnas **Lääne USA** ressursirühma loomiseks tippige:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Salvestusruumi konto nimega **mystorageaccount** selle ressursi jaotises [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) cmdleti abil loomiseks tehke järgmist.

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    -SkuName sobivad väärtused on:

    - **Standard_LRS** - kohalik liigsete salvestusruumi. 
    - **Standard_ZRS** - Zone liigsete salvestusruumi.
    - **Standard_GRS** - Geo liigsete salvestusruumi. 
    - **Standard_RAGRS** - lugemisõigus geo liigsete salvestusruumi. 
    - **Premium_LRS** - Premium kohalikult liigsete salvestusruumi. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Mäluruumi teie konto on VHD üleslaadimine

Pildi üleslaadimine on teie konto salvestusruumi container [Lisa-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet-käsu abil. Selles näites, laaditakse fail **myVHD.vhd** : `"C:\Users\Public\Documents\Virtual hard disks\"` on salvestusruumi konto nimega **mystorageaccount** **myResourceGroup** ressursi jaotises. Fail on paigutatud ümbris nimega **mycontainer** ja faili nimi on **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Kui õnnestub, kuvatakse vastust, mis näeb välja umbes järgmine:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Sõltuvalt teie võrguühendus ja oma VHD faili maht, see käsk võib kuluda aega


## <a name="next-steps"></a>Järgmised sammud

- [Luua VM Azure generalized VHD](virtual-machines-windows-create-vm-generalized.md)
- [Loo VM Azure eriotstarbelisi VHD](virtual-machines-windows-create-vm-specialized.md) manustamine on OS ketas, kui loote uue VM.


