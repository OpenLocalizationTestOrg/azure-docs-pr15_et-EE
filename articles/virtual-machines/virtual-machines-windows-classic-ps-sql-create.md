<properties
    pageTitle="Luua SQL serveri virtuaalse masina Azure PowerShelli (klassikaline) | Microsoft Azure'i"
    description="Pakub juhiseid ja PowerShelli skriptide loomise on Azure VM SQL serveri virtuaalse masina Galerii piltidega. Selles teemas kasutab juurutamise klassikaline režiim."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>SQL serveri virtuaalse masina Azure PowerShelli (klassikaline) kaudu ettevalmistamine

## <a name="overview"></a>Ülevaade

Sellest artiklist leiate juhised, kuidas luua PowerShelli cmdlet-käskude abil Azure SQL serveri virtuaalse masina.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Selles teemas ressursihaldur versiooni leiate [sätte SQL serveri virtuaalse masina ressursihaldur Azure PowerShelli abil](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Installima ja konfigureerima PowerShelli:

1. Kui teil pole Azure'i konto, külastage [Azure'i tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

2. [Laadige alla ja installige uusim Azure PowerShelli käske](../powershell-install-configure.md).

3. Käivitage Windows PowerShelli ja ühendage see käsk **Lisa-AzureAccount** Azure tellimuse.

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Teie eesmärk Azure piirkonna määratlemine

SQL serveri virtuaalne arvuti on majutatud pilveteenuses, mis asub teatud Azure piirkond. Järgmised toimingud aidata teil määrata oma piirkond, salvestusruumi ja pilvepõhine teenus, mida kasutatakse õpetuse ülejäänud.

1. Määratlege andmekeskuse, mida soovite kasutada majutada oma SQL serveri VM. PowerShelli järgmised käsud kuvatakse saadaolevate piirkondade üksikasjad, kus Kokkuvõte loendi lõpus.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Kui olete kindlaks teinud, asukoht, määrake nimega **$dcLocation** selle piirkonna muutujana.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Teie tellimus ja salvestusruumi konto määramine

1. Määratlege uue virtuaalse masina kasutate Azure tellimuse.

        (Get-AzureSubscription).SubscriptionName

1. Määrata oma siht Azure tellimuse **$subscr** muutujana. Seejärel seadke see praeguse Azure tellimuse.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Seejärel otsige Olemasoleva salvestusruumi kontod. Järgmise skripti kuvatakse kõik salvestusruumi kontod, mis on olemas teie valitud ala.

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Kui vajate salvestusruumi uuele kontole, kõigepealt looma kõik väiketähtedes salvestusruumi konto nimi käsu New-AzureStorageAccount, nagu järgmises näites: **New-AzureStorageAccount - StorageAccountName "<storage account name>"-asukoht $dcLocation**

1. Saate määrata **$staccount**target salvestusruumi konto nimi. Seejärel kasutage **Set-AzureSubscription** tellimust ja praeguse salvestusruumi konto.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Valige SQL serveri virtuaalse masina pilt

1. Siit saate teada loendis Saadaolevad SQL serveri virtuaalmasinates pilte galeriist. Need kõik pildid on **ImageFamily** atribuut, mis algab selgitava lausega "SQL". Järgmine päring kuvatakse teile, mis on SQL Server eelinstallitud pilt pere saadaval.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Kui olete leidnud virtuaalse masina pilt pere, võib olla mitu avaldatud pilti selle pere. Kasutage järgmist skripti uusim avaldatud virtuaalse masina pildi nimi (nt **SQL Server 2016 RTM Enterprise Windows Server 2012 R2**) teie valitud pildi perekonna leidmiseks:

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Virtuaalse masina loomine

Lõpuks luua virtuaalse masina PowerShelli.

1. Saate luua uue VM majutada mõnda pilveteenusesse. Pange tähele, et ka olemasoleva pilvepõhise teenuse asemel kasutada. Saate luua uue muutuv **$svcname** pilveteenusesse lühike nimi.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Määrake virtuaalse masina nimi ja suurus. Virtuaalse masina suurused kohta leiate lisateavet teemast [Azure virtuaalse masina suurused](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Määrake ja kohaliku administraatorikonto parooli.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Käivitage järgmine skript virtuaalse masina loomiseks.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Täiendavad selgitused ja konfiguratsiooni suvandite, jaotisest **koostada oma käsk Sea** [Loomine ja preconfigure Windowsi-põhiste Virtuaalmasinates Azure PowerShelli kasutamine](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>Näide PowerShelli skripti

Järgmise skripti pakub ja täielik skripti näide, mis loob **SQL Server 2016 RTM Enterprise Windows Server 2012 R2** virtuaalse masina. Kui kasutate seda skripti, peate kohandama algse muutujate põhjal selles teemas eelmisi toiminguid.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Kaugtöölaua kasutajaga

1. Looge selle. Praeguse kasutaja dokumendi kausta käivitada häälestamise lõpuleviimiseks neid virtuaalmasinates RDP-failid:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Dokumentide kataloogi, käivitage RDP-faili. Administraatori kasutajanime ja parooliga varem kasutajaga (nt kui kasutajanimeks VMAdmin, määrake "\VMAdmin", kui kasutaja ja sisestage parool).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>SQL Serveri arvutis Kaugpöördusteenuse konfigureerimise lõpuleviimiseks

Pärast peale arvuti kaugtöölaua logimine konfigureerimine juhiseid [on Azure VM SQL serveri ühenduvust konfigureerimise juhised](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)põhinevad SQL serveri.

## <a name="next-steps"></a>Järgmised sammud

Leiate täiendavad juhised ettevalmistamise virtuaalmasinates PowerShelliga [virtuaalmasinates dokumentatsiooni](virtual-machines-windows-classic-create-powershell.md). Täiendavad skriptide SQL serveri ja Premium salvestusruumi seotud, vt [Kasutamine Premium salvestusruumi Azure'i Virtuaalmasinates SQL Server](virtual-machines-windows-classic-sql-server-premium-storage.md).

Paljudel juhtudel on järgmise juhise juurde oma andmebaasi migreerimine selle uue SQL serveri VM. Andmebaasi migreerimine juhised leiate teemast [on Azure VM SQL serveri andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md).

Kui te pole huvitatud loomine Virtuaalmasinates SQL Azure'i portaali abil, vt [ettevalmistamise Azure SQL serveri virtuaalse masina](virtual-machines-windows-portal-sql-server-provision.md). Pange tähele, et õpetuse, mis juhendab teid portaali loob VMs abil soovitatav ressursihaldur mudeli, mitte selles teemas PowerShelli kasutatud klassikaline mudel.

Lisaks need ressursid, soovitame teil läbi vaadata [Muud teemad, mis töötab SQL Server Azure'i Virtuaalmasinates seotud](virtual-machines-windows-sql-server-iaas-overview.md).
