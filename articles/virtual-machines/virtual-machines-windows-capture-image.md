<properties
    pageTitle="Jäädvustada VM pilt generalized Azure'i VM | Microsoft Azure'i"
    description="Saate teada, kuidas jäädvustada VM pilt generalized Azure'i VM loodud ressursihaldur juurutamise mudeli"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Kuidas jäädvustada VM pilt generalized Azure'i VM


Selles artiklis kirjeldatakse, kuidas Azure'i PowerShelli abil saate luua generalized Azure VM pilt. Seejärel saate luua teise VM pilt. Pilt sisaldab OS kettale ja andmete ketast, mis on lisatud virtuaalse masina. Pilt ei sisalda virtuaalse võrgu ressursse, et soovite häälestada nende ressursse, kui loote uue VM. 


## <a name="prerequisites"></a>Eeltingimused

- Peate olema juba [üldiste VM](virtual-machines-windows-generalize-vhd.md). Üldistamist VM eemaldab kogu teie isikliku konto teave, muu hulgas ja valmistab kohapeal kasutatava pildina.

- Peab teil olema Azure PowerShelli versiooni 1.0.x või uuemat versiooni installitud. Kui te pole seda juba installitud PowerShelli, lugege [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise juhised.


## <a name="log-in-to-azure-powershell"></a>Azure'i PowerShelli sisse logida

1. Avage Azure'i PowerShelli ja Azure'i kontosse sisse logida.

    ```powershell
    Login-AzureRmAccount
    ```

    Saate sisestada mandaat Azure'i konto avatakse hüpikaknas.

2. Tellimuse ID hankimine saadaolevate tellimuste.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Määrake õige tellimuse abil tellimuse ID-ga.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Deallocate VM ja riigi üldiste seadmine       

1. Deallocate VM ressursse.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Azure'i portaalis VM *olek* muutub **peatamiseni** **peatamiseni (deallocated)**.

2. **Generalized**virtuaalse masina oleku määramine 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Oleku VM. VM **OSState/üldiste** jaotises peaks olema **DisplayStatus** **VM üldiste**määramine.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Pildi loomine 

1. Virtuaalne arvuti pildi kopeerimine sihtkoha salvestusruumi ümbris selle käsu. Pilt on loodud virtuaalse masina algse nimega sama salvestusruumi konto. Funktsiooni `-Path` parameetri salvestab JSON malli kohalik koopia. Funktsiooni `-DestinationContainerName` parameeter on ümbris, mida soovite oma pilte hoidke nimi. Kui ümbris pole olemas, on see teie jaoks loodud.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Saate oma pildi URL-i kaudu JSON failimalli. Minge **ressursse** > **storageProfile** > **tavalise nimetusega osDisk** > **pilt** > **uri** jaotisest täielik tee oma pilt. Pildi URL näeb välja umbes: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Samuti saate kontrollida URI portaalis. Pilt kopeeritakse ümbris nimega **süsteemi** teie salvestusruumi konto. 


## <a name="next-steps"></a>Järgmised sammud

- Nüüd saate [luua VM pilt](virtual-machines-windows-create-vm-generalized.md).

