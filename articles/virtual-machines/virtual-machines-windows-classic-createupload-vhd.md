<properties
    pageTitle="Luua ja üles laadida pildi VM PowerShelli kaudu | Microsoft Azure'i"
    description="Siit saate teada, luua ja üles laadida generalized Windows Server pilt (VHD) klassikaline juurutamise mudeli ja Azure PowerShelli abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Luua ja üles laadida Windows Server VHD Azure

Selles artiklis kirjeldatakse, kuidas üles laadida generalized VM pilt nimega virtuaalse kõvaketta (VHD) abil saate luua virtuaalmasinates. Ketast ja VHDs Microsoft Azure kohta leiate lisateavet teemast [kohta ketast ja VHDs Virtuaalmasinates jaoks](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Samuti saate [üles laadida](virtual-machines-windows-upload-image.md) virtuaalse masina ressursihaldur mudel. 

## <a name="prerequisites"></a>Eeltingimused

Selles artiklis eeldatakse, et teil on:

- **An Azure tellimuse** – kui teil pole, saate [avada Azure'i konto tasuta](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure'i PowerShelli](../powershell-install-configure.md)** - olete installinud ja konfigureerinud oma tellimuse kasutama Microsoft Azure'i PowerShelli mooduli. 

- **A . VHD faili** – toetatud Windowsi operatsioonisüsteem salvestatud faili .vhd ja manustatud virtuaalse masina. Kontrollige, kui on VHD töötava serveri rollid ei toeta Sysprep. Lisateabe saamiseks vt [Sysprep serverirollide tugi](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] Microsoft Azure'i VHDX vorming ei toeta. VHD vorming Hyper-V halduri või [Teisenda-VHD cmdlet-käsu](http://technet.microsoft.com/library/hh848454.aspx)abil saate teisendada ketas. Lisateavet leiate teemast selle [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Samm 1: Ettevalmistamine on VHD 

Enne selle VHD üleslaadimine Azure'i peab see olema üldiste Sysprep tööriista abil. See valmistab VHD kasutatava pildina. Sysprep kohta leiate üksikasjalikumat teavet leiate teemast [Kuidas kasutada Sysprepi: Sissejuhatus](http://technet.microsoft.com/library/bb457073.aspx). Varundage VM enne Sysprep käivitamist.

Virtuaalne arvutist, mille operatsioonisüsteem on installitud, tehke järgmist:

1. Logige sisse operatsioonisüsteemi.

2. Avage käsuviiba aken administraatorina. **%Windir%\system32\sysprep**kataloog muuta, ja seejärel käivitage `sysprep.exe`.

    ![Avage käsuviiba aken](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Kuvatakse dialoogiboks **Süsteemi ettevalmistamise tööriista** .

    ![Sysprep käivitamine](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  **Süsteemi ettevalmistamise tööriista**, valige **Sisestage süsteemi välja Box Experience (OOBE)** ja veenduge, et märgitud on **Generalize** .

5.  **Sulgemise suvandid**, valige **sulgumist**.

6.  Klõpsake nuppu **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Samm 2: Looge salvestusruumi konto ja ümbris

Peate salvestusruumi konto Azure nii, et teil on koht .vhd faili üles laadida. Selles etapis tuleb näitab, kuidas luua konto või konto toomine soovitud teave. Asendage muutujad &lsaquo; sulgudes &rsaquo; oma andmetega.

1. Sisselogimine

        Add-AzureAccount

1. Määrake Azure tellimuse.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Mäluruumi uue konto loomine. Salvestusruumi konto nimi peavad olema kordumatud, 3-24 märki. Nimi võib olla mis tahes kombinatsioon tähti ja numbreid. Samuti vajate nagu "Ida meie" asukoha määramiseks
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Uue konto salvestusruumi vaikimisi.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Saate luua uue ümbrises.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Samm 3: .vhd faili üles laadida

[Lisa-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) abil üles soovitud VHD.

Kasutasite eelmises etapis Azure PowerShelli aknas tippige järgmine käsk ja asendada muutujad &lsaquo; sulgudes &rsaquo; oma andmetega.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Samm 4: Pildi lisamiseks loendisse kohandatud pilte

[Lisa-AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) cmdlet pildi lisamiseks kasutage oma kohandatud piltide loetelu.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Järgmised sammud

Saate nüüd [luua kohandatud VM](virtual-machines-windows-classic-createportal.md) abil saate üles laadida pildi.

