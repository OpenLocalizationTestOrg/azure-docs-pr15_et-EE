<properties
    pageTitle="SQL Server Azure'i Premium mälu abil | Microsoft Azure'i"
    description="Selles artiklis kasutab ressursse, mis on loodud mudeliga klassikaline juurutamise ja antakse juhiseid kasutades Azure Premium Storage SQL Server Azure'i Virtuaalmasinates töötab."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>SQL Server Virtuaalmasinates Azure Premium mälu kasutamine


## <a name="overview"></a>Ülevaade

[Azure Premium mälu](../storage/storage-premium-storage.md) on järgmine genereerimine salvestusruumi, mis pakub madal latentsus ja kõrge läbilaskevõime IO. See toimib kõige paremini võtme IO intensiivse töökoormus, nt SQL Server IaaS [Virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Selles artiklis antakse kavandamise ja juhised migreerimine töötab SQL serveri kasutada Premium mälu. See hõlmab Azure'i infrastruktuuri (rakendus, salvestusruumi) ja Külastajate Windows VM juhiseid. [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) näites täielikku täielik teisaldamise suuremat VMs ära täiustatud kohaliku SSD salvestusruumi PowerShelliga postid lõpuni.

See on oluline mõista kasutades Azure Premium Storage SQL Server IAAS VMs protsessi lõpuni. See hõlmab:

- Kood Premium salvestusruumi kasutamise eeltingimused.
- Teenuse juurutamisel SQL Server IaaS Premium salvestusruumi uue juurutuste näited.
- Näiteid migreerimine olemasoleva juurutuste eraldiseisev serverid nii juurutuste SQL alati kohta kättesaadavus rühmade kasutamine.
- Võimalikud migreerimise võimalused.
- Täielik end-to-end näide Azure, Windowsi ja SQL serveri juhised migreerimine olemasoleva juurutuse alati nähtaval.

SQL Server Azure'i Virtuaalmasinates tausta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** Daniel Sol **tehnilise läbivaatajate:** Luis Carlos Vargas heeringas Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Eeltingimused Premium Storage

On mitu Premium salvestusruumi kasutamise eeltingimused.

### <a name="machine-size"></a>Seadme suurus

Premium salvestusruumi kasutamise peate kasutama DS sarja Virtuaalmasinates (VM). Kui te pole oma pilveteenuses enne kasutanud DS sarja masinad, peab kustutada olemasolevate VM, manustatud ketast hoida ja seejärel looge uus pilveteenuses enne taasloomine VM DS * rolli suurusega. Virtuaalse masina suuruse kohta leiate lisateavet teemast [virtuaalse masina ja pilvepõhise teenuse suurused Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Pilveteenused

Premium mälu abil saate kasutada ainult DS * VMs, kui nad on loodud uue pilveteenuses. Kui kasutate SQL serveri alati Azure, alati On kuulajale viitab Azure'i sisemise või välise laadi koormusetasakaalustusteenuse IP-aadress, mis on seotud mõnda pilveteenusesse. See artikkel keskendub kuidas saate migreerida säilitades kättesaadavus selle stsenaariumi.

> [AZURE.NOTE] Sarja DS * peab olema uute pilveteenusesse juurutatud esimese VM.

### <a name="regional-vnets"></a>Piirkondliku VNETS

DS * vms tuleb konfigureerida virtuaalse võrgu (VNET) majutada oma VMs olema piirkondliku. See "laiendab" on VNET võimaldab suuremat VMs olema ette valmistatud teiste rühmades ja nende vahel. Järgmine pilt kuvatakse asukoht piirkondliku VNETs, esimene tulemus näitab "kitsas" VNET.

![RegionalVNET][1]

Microsofti tugiteenuste Piletite piirkondliku VNET siirdamiseks võite tõsta, Microsoft muudatuste tegemine ja seejärel piirkondliku VNETs, et migreerimise lõpule viimiseks atribuudi AffinityGroup võrgu konfiguratsiooni muuta. Esmalt eksportida võrgukonfiguratsioon PowerShellis ja Asendage **VirtualNetworkSite** elementi **AffinityGroup** atribuudi atribuut **asukoht** . Määrake `Location = XXXX` kus `XXXX` on Azure piirkonna. Importige uus konfiguratsioon.

Näiteks arvestades pärast VNET konfiguratsioon:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

See teisaldamiseks piirkondliku VNET Lääne Euroopa muuta konfiguratsiooni järgmine:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Salvestusruumi kontod

Peate uue salvestusruumi konto on konfigureeritud Premium Storage loomine. Aga DS * sarja VM kasutamisel saate manustada VHD's Premium ja kindlad kontodelt, Pange tähele, et Premium salvestusruumi kasutuse on seatud salvestusruumi konto, mitte üksikute VHDs. See võib käsitleda kui te ei soovi OS VHD Premium salvestusruumi kontosse sisselogimise viia.

**Uus-AzureStorageAccountPowerShell** järgmine käsk "Premium_LRS" **Tüüp** loob Premium salvestusruumi konto.

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>VHDs vahemälusätted

Peamine erinevus ketast, mis on osa Premium salvestusruumi konto loomise on ketta vahemälu sätted. SQL serveri andmetega helitugevuse ketast on soovitatav kasutada "**Lugemine vahemällu**". Tehingu Logi maht, ketta vahemälu sätted peaks olema seatud '**pole**'. See erineb soovitused kindlad kontod.

Kui soovitud VHDs on lisatud, vahemälu säte ei saa muuta. Peaksite eemaldamist on VHD koos mõne sätte värskendatud vahemälu ja.

### <a name="windows-storage-spaces"></a>Windowsi salvestusruum

Saate [Windowsi salvestusruum](https://technet.microsoft.com/library/hh831739.aspx) , kui tegite eelmise Standard salvestusruumi, nii saate migreerida VM, mis on juba kasutades salvestusruum. Näide [liites](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (samm 9 ja edasisaatmine) näitab PowerShelli koodi eraldamiseks ja importida koos mitme manustatud VHDs VM.

Salvestusruumi kaustu kasutati täiustamiseks läbilaskevõime ja latentsusaeg vähendamiseks Standard Azure'i salvestusruumi kontoga. Võite leida väärtuse testimine salvestusruumi seal on Premium salvestusruumi uue juurutuste, kuid need lisada täiendavad keerukuse salvestusruumi häälestamine.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Kuidas leida mis Azure virtuaalse ketast kaardi salvestusruumi kaustu

Erinevate vahemälu säte soovitused manustatud VHDs on te otsustate selle VHDs kopeerimiseks Premium salvestusruumi konto. Kui te neid uuesti uue DS sarja VM manustama, peate vahemälu sätteid muuta. Oleks lihtsam rakendada Premium talletamist vahemälusätete soovitatav, kui teil on eraldi VHDs SQL-i andmefailide ja logifailide (mitte ühe VHD, mis sisaldab nii).

> [AZURE.NOTE] Kui teil on SQL serveri andmeid ja logi faile sama helitugevuse, sõltub vahemällu suvandist IO Accessi mustrite oma andmebaasi töökoormus. Ainult testimine saate näidata, mis vahemällu on parim selle stsenaariumi.

Kui kasutate Windows salvestusruum, mis on vaja vaadata mitme VHDs koosnevad oma algse skriptid tuvastamine, millega seotud VHDs on mis teatud rakenduskausta nii saate seejärel määrata objektivahemälu sätete vastavalt iga ketta.

Kui teil pole saadaval, näete, kus VHDs kaart talletusmahtu algse skripti, saate vaba/salvestusruumi rakenduskausta vastenduse määramiseks toimige järgmiselt.

Iga ketta, siis tehke järgmist:

1. Manustatud VM käsk **Get-AzureVM** ketast loendi kuvada.

    Get-AzureVM - teenuse nimi <servicename> -nime <vmname> | Get-AzureDataDisk

1. Pange tähele, et Diskname ja LUN.

    ![DisknameAndLUN][2]

1. Kaugtöölaud sisse VM. Minge **Arvutis halduse** | **Seadmehaldur** | **kettadraivide**. Vaadake atribuudid 'Microsoft Virtual ketast' iga

    ![VirtualDiskProperties][3]

1. LUN arv siin on viide LUN arvu teie määratud selle VHD manustamisel VM.
1. "Microsoft Virtual ketas" valige vahekaarti **üksikasjad** , siis **atribuudi** loendis liikuge **Juht võti**. **Väärtus**, Märkus funktsiooni **Offset**, mis on 0002 järgmine pilt. Funktsiooni 0002 tähistab soovitud talletusmahtu viitav PhysicalDisk2.

    ![VirtualDiskPropertyDetails][4]

2. Iga talletusmahtu jaoks dump välja seotud ketast:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Nüüd saate selle teabe seostamiseks lisatud VHDs füüsilise ketast salvestusruumi kaustadesse.

Kui teil on vastendatud VHDs füüsilise ketast salvestusruumi kaustadesse saate seejärel lahti ja kopeerige need üle Premium salvestusruumi konto, siis lisab selle sättega õige vahemälu. Lugege [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)näide 8 – 12 juhiseid. Need juhised näitab, kuidas ekstraktida VM manustatud VHD ketta konfiguratsiooni CSV-faili, kuvatakse VHDs kopeerida, muuta ketta konfiguratsiooni vahemälusätete ja lõpuks ümberkorraldamine VM DS sarjana VM kõik manustatud ketast.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM salvestusruumi läbilaskevõime ja VHD salvestusruumi läbilaskevõime

Salvestusruumi jõudluse summa sõltub DS * VM suurus ja VHD suurused. VMs on erinevate mahupiirangud VHDs, mida saab manustada arvu ja maksimaalse läbilaskevõime, neid toetab (MB/s). Teatud läbilaskevõime arvud, vt [virtuaalse masina ja pilvepõhise teenuse suurused Azure](virtual-machines-linux-sizes.md).

Suurema IOPS on saavutada suurem ketta suurus. Peaksite seda, kui asute kaaluma oma migreerimise tee. Lisateabe saamiseks [vt tabel IOPS ja ketta tüüpi](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Lõpetuseks, võtke arvesse, et VMs on erinevad maksimaalne ketta andmemahud need kõik ketast, millele on manustatud toetab. Suure koormuse võib küllastusgaaside maksimaalne ketta läbilaskevõime sel VM rolli suurus. Näiteks on Standard_DS14 toetab kuni 512 MB/s; Seetõttu võib kolme P30 ketast küllastusgaaside ketta läbilaskevõime VM. Kuid selles näites läbilaskevõime limiit ületamise sõltuvalt lugemine ja kirjutamine iOS-i.

## <a name="new-deployments"></a>Uue juurutuste

Järgmistes demonstreerivad seda, kuidas saate juurutada SQL serveri VMs Premium mälu. Nagu enne mainitud, mitte tingimata peate paigutada OS ketas peale Premium mälu. Võite seda teha, kui kavatsete kõik intensiivse IO töökoormus paigutamiseks OS VHD.

Esimene näide näitab, kasutades olemasoleva Azure'i galerii pildid. Teine näide näitab, kuidas kasutada kohandatud VM pilt, mida teil on kindlad konto.

> [AZURE.NOTE] Need näited eeldavad, et olete juba loonud piirkondliku VNET.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Looge uus VM Premium salvestusruumi Galerii

Järgmises näites kujutatakse paigutada OS VHD peale premium salvestusruumi ja manustage Premium salvestusruumi VHDs. Siiski saate ka OS ketas paigutamine normaliseeritud salvestusruumi konto ja seejärel manustage VHDs paiknevaid Premium salvestusruumi konto. Mõlemal juhul on näidanud.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Samm 1: Premium salvestusruumi konto loomine


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Samm 2: Looge uus pilveteenuses

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Samm 3: Reserveerida pilvepõhise teenuse VIP (valikuline)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Samm 4: Looge VM Container
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Juhis 5: Pannes OS VHD Standard või Premium salvestusruum
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Samm 6: Looge VM
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Luua uue VM kasutada kohandatud pildiga Premium salvestusruum

See stsenaarium näitab, kus teil on olemasoleva kohandatud pilte, mis asuvad kindlad konto. Nagu mainitud, kui soovite paigutada OS VHD Premium Storage peate kopeerige pilt, mis on olemas kindlad konto ja need üle Premium salvestusruumi enne seda saab kasutada. Kui teil on pildi eeldusel, saate seda meetodit kasutada ka mis kopeerida otse Premium salvestusruumi konto.

#### <a name="step-1-create-storage-account"></a>Samm 1: Salvestusruumi konto loomine
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Samm 2 loomine pilveteenuses
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Samm 3: Olemasoleva pildi kasutamine
Saate olemasoleva pildi. Või saate [võtta mõne olemasoleva seadme pilt](virtual-machines-windows-classic-capture-image.md). Pange tähele, saate pildi ei pea olema DS* masina kohapeal. Kui olete pildi, järgmist näitab, kuidas kopeerimiseks Premium salvestusruumi konto abil soovitud * *Algus-AzureStorageBlobCopy** PowerShell abil.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Samm 4: Kopeeri bloobimälu salvestusruumi kontode vahel
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Juhis 5: Kontrollige regulaarselt Kopeeri olek.
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Samm 6: Lisada pildi ketas Azure kettale hoidla tellimus
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Võib juhtuda, et isegi juhul, kui olek aruanded nimega edu, võib saate endiselt ketta üürilepingu tõrke. Sel juhul oodake umbes 10 minutit.

#### <a name="step-7--build-the-vm"></a>Juhis 7: Koostada VM
Siin on loomine VM kaudu oma pilt ja lisades kaks Premium salvestusruumi VHDs:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Olemasoleva juurutuste, mis ei kasuta alati klõpsake kättesaadavus rühmad

> [AZURE.NOTE] Olemasoleva juurutuste korral esmalt jaotisest [eeltingimused](#prerequisites-for-premium-storage) selle teema.

On erinevad kaalutlused SQL serveri juurutuse, mis ei kasuta alati klõpsake kättesaadavus rühmad ja need, mida teha. Kui te ei kasuta alati ja teil mõne olemasoleva autonoomse SQL serveri, saate versiooniks Premium salvestusruumi uue pilvepõhise teenuse ja salvestusruumi konto abil. Võtke arvesse järgmisi valikuid.

- **Loo uus SQL serveri VM**. Saate luua uue SQL serveri VM, mis kasutab salvestusruumi Premium konto, nagu uue juurutuste dokumenteerida. Seejärel varundus ja taaste SQL Server configuration ja kasutajale andmebaasid. Rakenduse tuleb viitamiseks uus SQL Server, kui pääseb väliselt värskendada. Peate kõigi 'välja db' objektide kopeerimine kui tegite kõrvuti (SxS) SQL serveri migreerimise. See hõlmab objektide, näiteks sisselogimise serdid ja lingitud serverites.
- **Migreerimine mõne olemasoleva SQL serveri VM**. Selleks on vaja võttes SQL serveri VM ühenduseta ja seejärel teisaldamist uue pilveteenusesse, mis sisaldab kõiki selle lisatud VHDs kopeerimine Premium salvestusruumi konto. Kui VM võrguühenduse, kuvatakse rakenduse viide serveri hosti nimi nagu enne. Pange tähele, et olemasolev ketta mõjutab parameetritele. Näiteks 400 GB vaba saab ümardatakse on 20. Kui teate, et te ei pea selle jõudluse, siis võib uuesti luua nimega DS sarja VM VM ja manustamine Premium salvestusruumi VHDs vajate maht ja jõudlus kirjelduse. Seejärel võib eemaldada ja SQL-i DB failid manustama.

> [AZURE.NOTE] Kui kopeerimise VHD ketast, arvestage suurusega suurusest tähendab millist Premium salvestusruumi ketta nad kuuluvad, seda määrab ketta jõudluse määratlus. Azure'i saavad round ülespoole lähima ketas suuruse muutmiseks nii, et kui teil on 400 GB vaba, ümardatakse see on 20 abil. Olenevalt teie olemasoleva IO nõuetele OS VHD, võib pole vaja migreerida Premium salvestusruumi konto.

Kui SQL serveris on väliselt, muutub pilvepõhise teenuse VIP. Samuti peate värskenduse lõpp-punktid, ACL-ID ja DNS-i sätted.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Olemasoleva juurutuste kasutavate alati klõpsake kättesaadavus rühmad

> [AZURE.NOTE] Olemasoleva juurutuste korral esmalt jaotisest [eeltingimused](#prerequisites-for-premium-storage) selle teema.

Algselt selle jaotise käsitleme kuidas alati suhtleb Azure'i Networking. Meil on seejärel murda migratsioon sisse, et kahte järgmist stsenaariumi: migratsioon, kus saate lubada mõned tööseisakute ja migratsioon, kus teil tuleb saavutada minimaalne ajakulu.

Asutusesisese SQL serveri alati klõpsake kättesaadavus rühmad on kuulajale kohapealse registreerib virtuaalse DNS-i nimi koos IP-aadress, mis on jagatud ühe või mitme SQL serveri kasutamine Kui klientrakendustega marsruuditakse nende kuulajale IP esmane SQL serveri kaudu. See on server, mis kuulub alati On IP ressursi sel ajal.

![DeploymentsUseAlways kohta][6]

Microsoft Azure teil ainult üks IP-aadress määratud NIC, VM, seega võtmiseks asutusesiseselt, kui sama kiht saavutamiseks Azure kasutab IP-aadress on määratud sisemine/väline koormus soolise (ILB/ELB). IP-ressursi, mis on jagatud serverid on seatud sama IP kui ILB/ELB. See on avaldatud DNS-i ja kliendi liikluse läbib ILB/ELB esmane SQL serveri koopia. ILB/ELB teab, milline SQL serveri on esmane, kuna see kasutab sondid sond alati On IP ressursi. Eelmises näites, see sondid iga sõlme, mis on viidatud ELB/ILB lõpp, kumb vastab on esmane SQL serveri.

> [AZURE.NOTE] ILB ja ELB mõlemad määratakse kindla Azure pilveteenusesse, seetõttu mõnda pilveteenusesse migreerimiseks Azure tõenäoliselt tähendab, et laadi koormusetasakaalustusteenuse IP muutub.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migreerimine alati juurutuste puhul, mis võimaldab mõned tööseisakute

On kaks strateegiad alati juurutuste, mis võimaldavad mõned seisakuaja migreerida.

1. **Veel teisene koopiad lisamiseks mõne olemasoleva alati klõpsake kobar**
1. **Uus alati klõpsake klaster migreerimine**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. lisada rohkem teisene koopiad mõne olemasoleva alati klõpsake kobar

Üks strateegia on veel sekundaaride alati sisse kättesaadavus rühma lisada. Peate need uue pilveteenuses lisada ja värskendada kuulajale uue laadi koormusetasakaalustusteenuse IP.

##### <a name="points-of-downtime"></a>Tööseisakute punktide:

- Kobar valideerimine.
- Uue sekundaaride alati failovers testimine.

Kui kasutate Windows salvestusruumi kaustu sees VM suurema IO läbilaskevõimega, siis need on võrguühenduseta ajal kobar kinnitamine. Tulemustega on nõutav, kui lisate sõlmed klaster. Käivitage test kuluv aeg võib olla muutuv, nii, et see peaks testida tüüpilised testimiskeskkonnas saada, kui kaua see võtab ligikaudne kellaaeg.

Peaks ettevalmistamise aeg, kus saate teha käsitsi Tõrkesiirde ja testimine kaose äsja lisatud sõlmed tagamiseks alati On kõrge kättesaadavus funktsioonide ootuspäraselt.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Lõpetage kõik eksemplarid, SQL Server, kus kasutatakse salvestusruumi kaustu enne valideerimine käivitatakse.
##### <a name="high-level-steps"></a>Üksikasjalik järgmiselt.

1. Uue pilveteenuses manustatud Premium Storage luua kaks uut SQL-i serverid.
1. Kopeerige üle täielik varukoopiate ja **NORECOVERY**taastada.
1. Kopeerige üle 'välja kasutaja DB' sõltuvad objektid, nt sisselogimise jne.
1. Loo uus uue sisemise laadi koormusetasakaalustusteenuse (ILB) või kasutage mõnda välise laadi koormusetasakaalustusteenuse (ELB) ja seejärel häälestamine laadi tasakaalustatud lõpp-punktid nii uusi sõlmi.
> [AZURE.NOTE] Märkige ruut kõik sõlmed on õige lõpp-punkti konfiguratsiooni enne jätkamist

1. Juurepääsu kasutaja/rakendus SQL Server (kui kasutades salvestusruumi kaustu).
1. Lõpetage SQL Server Engine Services kõik sõlmed (kui kasutades salvestusruumi kaustu).
1. Saate lisada uusi sõlmi klaster ja käivitada kinnitamine.
1. Kui valideerimine on edukalt, käivitage kõik SQL Serveri teenuseid.
1. Tehingu logid varundamine ja taastamine kasutaja andmebaasid.
1. Lisada uusi sõlmi alati klõpsake rühma kättesaadavus ja asetage dispersioonanalüüs **sünkroonne**sisse.
1. Lisage mitme saidi näide [liites](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)põhjal IP-aadressi ressursi, selle uue pilvepõhise teenuse ILB/ELB jaoks alati PowerShelli kaudu. Määrake Windowsi rühmitamise, vana uue sõlmi **IP-aadressi** ressursi **võimalike omanikud** . [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)jaotisest "Lisamise IP Address ressurss sama alamvõrgu".
1. Ühe uue sõlmed Tõrkesiirde.
1. Tehke uue sõlmed automaatne Tõrkesiirde partnerid ja testi failovers.
1. Algne sõlmed kättesaadavus rühmast eemaldada.

##### <a name="advantages"></a>Eelised

- Uue SQL-i serverid võib olla (SQL Server ja rakenduse) katsetada, enne, kui need lisatakse alati.
- Saate VM suuruse muutmine ja kohandada talletamist oma täpse nõuetele. Siiski oleks kasulik hoida kõik SQL-i teed.
- Saate määrata käivitati DB varukoopiate üleviimine teisene koopiad. See erineb abil Azure **Algus-AzureStorageBlobCopy** abil kopeerida VHDs, kuna see on asünkroonne eksemplar.

##### <a name="disadvantages"></a>Puudused
- Kui kasutate Windows salvestusruumi kaustu, on täielik kobar valideerimisel uue täiendavad sõlmed kobar tööseisakute.
- Sõltuvalt SQL serveri versioon ja olemasoleva arvu teisene koopiad, ei pruugi olla lisada rohkem teisene koopiad olemasoleva sekundaaride eemaldamata.
- SQL-i andmete edastamine aega häälestamisel on sekundaaride võiks.
- On lisatasu migreerimisel ning teil töötab samal ajal uued masinad.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. migreerimiseks uue alati kohta arvutikobaras

Teise strateegia on luua täiesti uus alati klõpsake klaster uus sõlmed pilveteenuses uus ja seejärel suunake kliendid seda kasutada.

##### <a name="points-of-downtime"></a>Tööseisakute punktide

Kui teisaldate uue alati kuulajale rakenduste ja kasutajad on tööseisakute. Funktsiooni tööseisakute sõltub:

- Lõplik tehingu varundamine taastamiseks andmebaaside uue serverites kuluv aeg.
- Värskendamiseks kasutama uut alati kuulajale klientrakendustes kulunud aeg.

##### <a name="advantages"></a>Eelised

- Tootmisprotsessi keskkonnas, SQL Server, saate testida ja OS koostada muudatused.
- Teil on võimalik kohandamiseks talletamist ja VM mahtu vähendada. See võib põhjustada kulude vähendamiseks.
- Selle toimingu käigus saate värskendada oma SQL serveri ehitada või versioon. Täiendamist operatsioonisüsteem.
- Eelmise alati klõpsake klaster võib toimida ühtlase tagasipööramine sihtkoht.

##### <a name="disadvantages"></a>Puudused

- Peate kuulajale DNS-i nime muuta, kui soovite, et nii alati kogumite töötab samal ajal. Sellega lisatakse Administreerimine kohal migreerimisel vastavalt kliendi rakenduse stringide sisaldama kuulajale uus nimi.
- Peate rakendama sünkroonimise süsteemi vahel kahes keskkonnas hoida neid lõplik sünkroonimise nõuetele migreerimise enne minimeerimiseks võimalikult lähedalt.
- Migreerimisel on lisatud kulu sel ajal, kui uude keskkonda, kus töötab.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migreerimine alati klõpsake juurutuste jaoks minimaalne ajakulu

Minimaalne seisakuaja on kaks migreerimine alati juurutuste strateegiad.

1. **Kasutada mõne olemasoleva teisese: ühe – saidile**
1. **Olemasoleva teisene Replica(s) kasutada: mitme saidi**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Kasutage mõne olemasoleva teisese: ühe – saidile

Minimaalne ajakulu ühe strateegia on võtta mõne olemasoleva pilveteenuse teisene ja eemaldamine praeguse pilveteenuses. Seejärel kopeerige soovitud VHDs Premium salvestusruumi uue konto ja luua uue pilveteenuses VM. Seejärel värskendage kuulajale rühmitamise ja Tõrkesiirde.

##### <a name="points-of-downtime"></a>Tööseisakute punktide

- On tööseisakute koormusetasakaalustusega lõpp-punkti lõplik sõlm värskendamisel.
- Oma kliendi sisselülitamise võib sõltuvalt teie kliendi/DNS-i konfiguratsiooni edasi lükata.
- Kui otsustate võtta alati kobar klõpsake jaotises ühenduseta vahetada välja IP-aadressid on täiendavad tööseisakute. Saate vältida selle abil võimalik omanikele ja OR sõltuvus lisatud IP-aadressi ressursi. [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)jaotisest "Lisamise IP Address ressurss sama alamvõrgu".

> [AZURE.NOTE] Kui soovite lisatud sõlm partnerina alati klõpsake Tõrkesiirde osa saada, peate lisama lõpp Azure'i viitega laadi tasakaalustatud seadmine. Toiming käsk **Lisa-AzureEndpoint** käivitamisel ei saa praeguse ühendused jäävad avatud, kuid uute ühenduste kuulajale seniks laadi koormusetasakaalustusteenuse on värskendatud. Testimine see oli näha viimase 90-120seconds, et see tuleb kontrollida.

##### <a name="advantages"></a>Eelised

- Ilma täiendava migreerimisel kulud.
- Üks-ühele migreerimist.
- Vähendatud keerukuse.
- Võimaldab suurema IOPS Premium salvestusruumi SKU-de jaoks. Kui soovitud ketast lahti VM ja kopeeritud uue pilveteenusesse, on 3. osapoole tööriista saab kasutada suurendamine VHD, mis pakub kõrgema läbilase. Suureneva VHD suurused, leiate selle [foorumi arutelu](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Puudused

- Migreerimisel on ajutine kaotus HA ja DR.
- See on 1:1 migreerimise, on teil VM miinimummaht, mis toetab oma arvu VHDs, nii, et te ei pruugi teie VMs koheselt kasutada.
- Selle stsenaariumi kasutaks Azure **Algus-AzureStorageBlobCopy** abil, mis on asünkroonne. Ei ole SLA eksemplari lõpetamise. Aeg eksemplarid muutub, kuigi see sõltub sõltub ka andmete edastamiseks hulga järjekorras ootamine. Kopeeri aeg suurendab kui ülekandmist saab teise Azure andmekeskuse teises regioonis Premium mälu toetab. Kui teil on ainult 2 sõlmed, kaaluge võimalike vähendamiseks juhuks, kui Kopeeri kestab kauem kui testimine. See võib sisaldada järgmisi ideid.
    - Lisamine ajutine 3 SQL serveri sõlm HA enne migreerimise abil kokkulepitud tööseisakute.
    - Käivitage migreerimise väljaspool Azure ajastatud hooldustööd.
    - Veenduge, et teie kobar kvoorumi on õigesti konfigureeritud.  

##### <a name="high-level-steps"></a>Üksikasjalik järgmiselt.

Selle dokumendi näidata täieliku lõpuni näites, aga [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) sisaldab üksikasju, mida saate kasutada seda teha.

![MinimalDowntime][8]

- Koguge ketta konfiguratsiooni ja eemaldada sõlme (kustutada manustatud VHDs).
- Looge konto Premium salvestusruumi ja kopeerige kindlad konto VHDs
- Looge uus pilveteenuses ja Juurutage uuesti SQL2 VM, et pilveteenuses. Saate luua VM abil kopeeritud algse OS VHD ja lisades kopeeritud VHDs.
- Konfigureerimine ILB / ELB ja lisage lõpp-punktid.
- Kas kuulajale värskendamiseks tehke järgmist.
    - Alati klõpsake jaotises ühenduseta tegemine ja värskendamist alati klõpsake kuulajale uue ILB / ELB IP-aadress.
    - Või IP lisamine rakendusse Windows rühmitamise ressursi, uue pilvepõhise teenuse ILB/ELB PowerShelli kaudu. Seejärel määratud IP-aadressi ressursi võimalike omanikud migreeritud sõlme SQL2, ning seada OR sõltuvus võrgu nimi. [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)jaotisest "Lisamise IP Address ressurss sama alamvõrgu".
- Märkige ruut konfiguratsiooni DNS-i Levitamistoimingute klientidele.
- Migreerimine SQL1 VM ja läbida juhiseid 2 – 4.
- Kui juhiseid 5ii, lisage SQL1 võimalik omanikuna lisatud IP-aadressi ressursi jaoks
- Testige failovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2 kasutada olemasoleva teisene replica(s): mitme saidi

Kui teil on rohkem kui üks Azure andmekeskuse (näiteks Põhiliselt) sõlmed või kui teil on hübriidkeskkonna loomiseks, siis saate alati konfiguratsiooni selles keskkonnas tööseisakute minimeerimiseks.

Lähenemine on alati sünkroonimise muuta sünkroonne kohapealse või sekundaarne Azure'i AV, ja seejärel Tõrkesiirde üle SQL serveri. Seejärel kopeerige soovitud VHDs Premium salvestusruumi konto ja Juurutage uuesti masina uue kasutusele cloud. Kuulajale värskendada, ja seejärel uuesti nurjuda.

##### <a name="points-of-downtime"></a>Tööseisakute punktide

Funktsiooni tööseisakute koosneb aeg Tõrkesiirde alternatiivne AV ja tagasi. See sõltub teie kliendi/DNS-i konfigureerimine ja oma kliendi sisselülitamise seniks.
Kaaluge võimalust hübriidkonfiguratsiooni alati järgmises näites:

![MultiSite1][9]

##### <a name="advantages"></a>Eelised

- Saate kasutada olemasolevaid taristu.
- Teil on võimalus versiooniuuenduse-eelne Azure storage DR Azure'i näiteks Põhiliselt klõpsake esmalt.
- DR Azure'i AV salvestusruumi saate uuesti konfigureeritud.
- Migreerimisel, välja arvatud testi failovers on vähemalt kaks failovers.
- Teil pole vaja teisaldamine SQL serveri andmeid varundamise ja taastamise.

##### <a name="disadvantages"></a>Puudused

- Olenevalt kliendi juurdepääsu SQL Server, võib olla suurem latentsus kui SQL Server töötab ka alternatiivne näiteks Põhiliselt rakendusse.
- Kopeeri aega VHDs Premium mäluruumi võib olla pikk. See võib mõjutada teie otsustamisel hoida sõlme kättesaadavus rühma. Kaaluge selle log intensiivse töö töötab laadimise ajal migreerimise korral nõutav, kuna esmane sõlm on hoida oma tehingulogi unreplicated tehingud. Seetõttu see võib oluliselt kasvada.
- Selle stsenaariumi kasutaks Azure **Algus-AzureStorageBlobCopy** abil, mis on asünkroonne. Pärast lõpetamist on puudub SLA. Aeg eksemplarid muutub, kuigi see sõltub ootamine järjekorda, sõltub ka andmete edastamiseks hulk. Seega peate vaid ühe sõlme 2 data Centeri kaudu, võtke vähendamiseks juhiseid juhuks, kui Kopeeri kestab kauem kui testimine. See võib sisaldada järgmisi ideid.
    - Ajutiste 2. SQL-i sõlme lisamine enne koos kokkulepitud tööseisakute migreerimise jaoks HA.
    - Käivitage migreerimise väljaspool Azure ajastatud hooldustööd.
    - Veenduge, et teie kobar kvoorumi on õigesti konfigureeritud.

See stsenaarium eeldab, et teil on dokumenteerida oma installi ja teada, kuidas talletamist on vastendatud optimaalse kettapuhastusriista vahemälu sätete muudatuste tegemiseks.

##### <a name="high-level-steps"></a>Üksikasjalik järgmiselt.
![Multisite2][10]

- Veenduge, et asutusesisese / vahelduvatele AV Azure SQL serveri esmane ja muutke see muude automaatne Tõrkesiirde partneri (AFP).
- SQL2 ketta konfiguratsiooni teabe kogumine ja eemaldada sõlme (kustutada manustatud VHDs).
- Salvestusruumi Premium konto loomine ja kopeerida VHDs kindlad konto.
- Luua uue pilveteenuses ja luua oma lisatasud salvestusruumi ketast, millele on manustatud SQL2 VM.
- Konfigureerimine ILB / ELB ja lisage lõpp-punktid.
- Uue ILB alati klõpsake kuulajale värskendada / ELB IP-aadress ja testige Tõrkesiirde.
- DNS-i konfiguratsiooni kontroll
- Muuta AFP SQL2, migreerimine SQL1 ja läbida juhiseid 2 – 5.
- Testige failovers.
- Aktiveerige AFP SQL1 ja SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Lisa: Migreerimine on alati klõpsake kobar mitmes kohas Premium salvestusruum

Ülejäänud selles teemas antakse üksikasjalik näide mitme saidi alati kobar teisendamine Premium mälu. See ka teisendab kuulajale kasutada ka välise koormuse koormusetasakaalustusteenuse (ELB) on sisemine laadi koormusetasakaalustusteenuse (ILB).

### <a name="environment"></a>Keskkonnas

- Windows 2k 12 / SQL 2k 12
- SP 1 DB failid
- 2 x salvestusruumi kaustu sõlm kohta.

![Appendix1][11]

### <a name="vm"></a>VM:

Selles näites me liikumine on ELB ILB näitamiseks. ELB oli enne ILB, saadaval nii, et see näitab, kuidas see üleviimise ajal minna.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Eelnevalt järgmiselt: Tellimuse ühenduse

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Samm 1: Loo uus salvestusruumi konto ja teenus Cloud
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Samm 2: Suurendada lubatud tõrkeid ressursid<Optional>
Teatud ressursse, mis kuuluvad alati klõpsake kättesaadavus rühma on piirangud mitu tõrkeid, mis võivad esineda punkt, kus kobar teenuse proovib taaskäivitage ressursirühma. Soovitatav on teil suurendada teil on kõndimise seda toimingut, kuni alates kui te pole käsitsi Tõrkesiirde ja päästik failovers, saate selle piirmäära lähedale masinad sulgemise ajal.

Oleks mõistlik topeltklõpsake tõrge toetus, tõrkesiirdeklastri halduris, selleks valige ressursirühma alati atribuudid:

![Appendix3][13]

Saate muuta 6 maksimaalne ebaõnnestumist.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Samm 3: Lisaks IP-aadressi ressursi kobar rühma<Optional>

Kui teil on ainult üks IP-aadress kobar rühma ja see joondatakse cloud alamvõrku, hoiduge, kui te võtate ühenduseta kõik kobar sõlmed võrgus siis kobar IP ressursi pilves ja kobar võrgu nimi ei saa veebis saadaval. See korral see aitab vältida värskenduste muude kobar ressurssidele.

#### <a name="step-4-dns-configuration"></a>Samm 4: DNS-i konfigureerimine

Rakendada sujuva ülemineku sõltub sellest, kuidas DNS-i on kasutatud ja värskendada.
Kui alati on installitud, loob see Windowsi kobar ressursirühma, kui avate tõrkesiirdeklastri halduri, kuvatakse et vähemalt see on kolm ressursse, kaks, mis viitab dokument on:

- Virtuaalne võrgu nimi (VNN) – see on DNS-i nimi kliendi ühenduse kui soovivad SQL serveriga alati kaudu.
- IP-aadressi ressursi – see on selle VNN seotud IP-aadressi, saate määrata rohkem kui üks ja mitmes kohas konfigureerimine on teil IP-aadress saidi/alamvõrgu kohta.

Kui ühenduse SQL Server, SQL serveri klient draiver on tuua kuulajale seotud DNS-kirjed ja proovite luua ühendust iga alati kohta seotud IP-aadress, all käsitleme mõned tegurid, mis võivad mõjutada see.

Samaaegseid DNS-i kirjed, mis on seotud kuulajale nime arv sõltub mitte ainult seotud, IP-aadresside arvu, kuid selle ' RegisterAllIpProviders'setting sisse tõrkesiirdeklastrite alati sees VNN ressursi.

Kui juurutamist alati Azure on teistmoodi loomiseks kuulajale ja IP-aadressid, tuleb käsitsi konfigureerimine 'RegisterAllIpProviders' 1, see on erinevate on sees eeldusel alati juurutamise, kui see on juba seatud 1.

Kui 'RegisterAllIpProviders' on 0, siis kuvatakse ainult üks kuulajale seotud DNS-i DNS-i kirje.

![Appendix4][14]

Kui 'RegisterAllIpProviders' on 1.

![Appendix5][15]

Alljärgnev kood dump välja VNN sätted ja määraks teie eest, võtke arvesse muudatuse jõustumiseks peate selle VNN võrguühenduseta ja lülitage see tagasi võrgurežiimi, see võtaks kuulajale ühenduseta põhjustada klientrakenduse ühenduvuse segadusi vältida.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Hiljem migreerimise etapis on vaja värskendada kuulajale alati värskendatud IP-aadress, mis viitab laadi koormusetasakaalustusteenuse, see hõlmab ka IP-aadressi ressursi eemaldamine ja lisamine. Pärast IP värskendamist, peate uue IP-aadress on värskendatud DNS-i tsooni ja, et kliendid on oma DNS-i kohaliku vahemälu värskendamine.

Kui kliendid elavad erinevate võrgu lõigu ja viitavad erinevad DNS-i server, peate kaaluma, mis juhtub kohta DNS-i tsooni edastamine migreerimisel nagu rakendus uuesti aeg piiranud vähemalt Zone kõne ajal uued IP-aadressid kuulajale. Kui teil on aega piirang siin jaotises, peaksite arutada ja testida sunnib astmeline tsooni edastamine koos teie Windowsi meeskondadel ja ka sellele DNS-i hosti kirje abil on väiksem aja abil Live (TTL), et kliendid värskendage. Lisateavet leiate teemadest [Suureneva tsooni üle](https://technet.microsoft.com/library/cc958973.aspx) ja [Algus-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Vaikimisi DNS-i kirje, mis on seotud kuulajale alati sisse Azure TTL on 1200 sekundit. Soovi korral võite seda vähendada, kui olete jaotises piirang tagamaks, et kliendid teie migreerimisel värskendada oma DNS-i värskendatud IP-aadressi kuulajale aeg. Saate vaadata ja välja soovitud VNN konfiguratsiooni suhtes konfiguratsiooni muutmiseks.

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Pange tähele, väiksem on 'HostRecordTTL', suurema hulga DNS-i liikluse võivad ilmneda.

##### <a name="client-application-settings"></a>Kliendi rakenduse sätted

Kui SQL-i klientrakenduse toetab .net 4.5 SQLClient, siis saate kasutada ' MULTISUBNETFAILOVER = TRUE "märksõna, see on soovitatav rakendatud, kuna see võimaldab kiirem ühenduse SQL alati kohta kättesaadavus rühma Tõrkesiirde ajal. See loetleb kõik alati kuulajale paralleelselt seotud IP-aadresside kaudu ja tõhusamaks TCP ühendus uuesti kiiruse sooritab Tõrkesiirde ajal.

Ülaltoodud sätete kohta leiate lisateavet teemast [MultiSubnetFailover märksõna ja seotud funktsioonid](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Vt ka [SqlClient kõrge kättesaadavus avariitaastet tugi](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Juhis 5: Kobar kvoorumi sätted

Kui te ei kavatse võtmine vähemalt üks SQL serveri alla korraga, muutke sätte kobar kvoorumi kui 2 sõlmed faili ühiskasutusse andmine tunnistaja (FSW) abil, määrake kvoorumi sõlm enamik lubada ja kasutada dünaamilise hääletamine ja see on ühe positsiooni jääda sõlm lubamiseks.


    Set-ClusterQuorum -NodeMajority  

Haldamine ja kobar kvoorumi konfigureerimise kohta lisateabe saamiseks lugege [konfigureerimine ja haldamine Windows Server 2012 tõrkesiirdeklastrite kvoorumi](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Juhist 6: Olemasoleva lõpp-punktid ja ACL ekstraktimiseks
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Salvestage see tekstifail.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Juhis 7: Tõrkesiirde partnerid ja Dispersioonanalüüs režiimide muutmine

Kui teil on rohkem kui 2 SQL-i serverid, peaksite muuta mõne muu teisese teise AV või ettevõtete Tõrkesiirde "Sünkroonne" ja veenduge, et see on automaatne Tõrkesiirde partneri (AFP), see on nii, et haldate HA samal ajal, kui muudate. Saate seda kuni TSQL, kuigi SSMS muutmine:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Samm 8: Pilveteenus teisene VM eemaldamine

Kavatsete peaks olema migreerida pilve teisene sõlm esmalt kui see on praegu esmane, annate käsitsi Tõrkesiirde.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Samm 9: Ketta vahemällu CSV-faili sätete muutmine ja salvestamine

Andmete maht olema need määratud kirjutuskaitstud.

TLOG maht seadma need pole.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Samm 10: Kopeeri VHDS
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Kopeeri VHDs Premium salvestusruumi konto oleku kontrollimine

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Oodake, kuni kõik need registreeritakse edu.

Lisateavet üksikud plekid:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Samm 11: Register OS kettale

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Samm 12: Teisene importimine uue pilveteenuses

Alljärgnev kood kasutab ka lisatud võimalus siin saate seade importimise ja kasutada retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Samm 13: Luua ILB uue Cloud Svc, lisada laadi tasakaalustatud lõpp-punktid ja ACL-ID
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Samm 14: Update alati
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nüüd eemaldada vana pilveteenuses IP-aadress.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>15 juhis: DNS-i värskendamine sisse

Nüüd peaks kontrollida DNS-serverid võrguga SQL serveri klient ja veenduge, et rühmitamise on lisatud täiendav hosti kirje lisatud IP-aadressi. Kui need DNS-serverid on värskendatud, kaaluge sunnib DNS-i tsooni edastamine ja veenduge, et klientidele seal alamvõrgu suudavad lahendamiseks nii alati sisse IP-aadressid, see on nii, et teil pole vaja ootama automaatse DNS-i kopeerimine.

#### <a name="step-16-reconfigure-always-on"></a>Samm 16: Taaskonfigureerimine alati kohta

Selles etapis oodata teisese sõlme täielikult sünkroonige uuesti koos kohapealne sõlm ja aktiveerige sünkroonse dispersioonanalüüs sõlm ja oleks AFP migreerimise.  

#### <a name="step-17-migrate-second-node"></a>Samm 17: Migreerimine teise sõlme
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Samm 18: Ketta vahemällu CSV-faili sätete muutmine ja salvestamine

Andmete maht olema need määratud kirjutuskaitstud.

TLOG maht seadma need pole.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Samm 19: Looge uus iseseisev konto teisene sõlm
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Samm 20: Kopeeri VHDS
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Saate otsida kõiki VHDs VHD Kopeeri olek: ForEach ($disk sisse $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. DiskLabel $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Oodake, kuni kõik need registreeritakse edu.

Lisateavet üksikute plekid: #Check induvidual bloobimälu olek Get-AzureStorageBlobCopyState-Bloobivahemälu "danRegSvcAms-dansqlams1 – 2014-07-03.vhd"-Container $containerName-kontekstis $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Samm 21: Register OS kettale
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Samm 22: Lisada laadi tasakaalustatud lõpp-punktid ja ACL-ID
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Samm 23: Test Tõrkesiirde

Nüüd peaksite sünkroonimine sõlm kohapealne alati, pange see sünkroonse dispersioonanalüüs režiimi ja oodake, kuni see on sünkroonitud migreeritud sõlme. Siis kohapeal rikketeenus esimese sõlme migreerida, mis on AFP. Kui see on töötanud, muuta viimase migreeritud sõlm AFP.

Peaksite testida failovers vahel kõik sõlmed ja käivitada, kuigi kaose kontrollib failovers töö tagamiseks oodatud ja õigeaegne mõisas.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Samm 24: Sellele tagasi kobar kvoorumi sätted / DNS-i TTL / Tõrkesiirde Pntrs / sätete sünkroonimine
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Sama alamvõrgu IP-aadressi ressursi lisamine

Kui on ainult 2 SQL-i serverite ja soovite migreerida need uue pilveteenusesse, kuid soovite säilitada neid sama alamvõrgu, saate vältida kuulajale ühenduseta viimine kustutage algne alati sisse IP-aadress ja lisage uus IP-aadress. Kui migreerite VMs teise alamvõrku teil pole vaja toiming on täiendavad kobar võrk, mis viitab selle alamvõrgu.

Kui teil on migreeritud teisese üles ja uue IP-aadressi ressursi jaoks Tõrkesiirde olemasoleva primaarne enne uue pilveteenuses lisanud, võtke kobar Tõrkesiirde halduri järgmiselt:

IP-aadressi lisada, vt [Lisa](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)samm 14.

1. Järgmises näites, 'dansqlams4' "Olemasoleva esmane SQL serveriga", võimalike omaniku muutmine praeguse IP-aadressi ressursi:

    ![Appendix13][23]

1. Uue ressursi IP-aadress, võimalik omanik 'Migrated teisene SQL serveri", järgmises näites, 'dansqlams5' muutmiseks tehke järgmist.

    ![Appendix14][24]

1. Kui see on loodud, saate Tõrkesiirde ja kui viimane sõlm migreeritakse võimalike omanikud saab redigeerida nii sõlme lisatakse võimalik omanik.

    ![Appendix15][25]

## <a name="additional-resources"></a>Lisaressursid
- [Azure Premium mälu](../storage/storage-premium-storage.md)
- [Virtuaalmasinates](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
