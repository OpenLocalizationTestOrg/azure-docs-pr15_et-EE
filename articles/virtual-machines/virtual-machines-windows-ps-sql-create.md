<properties
    pageTitle="Luua SQL serveri virtuaalse masina Azure PowerShelli (ressursihaldur) | Microsoft Azure'i"
    description="Pakub juhiseid ja PowerShelli skriptide loomise on Azure VM SQL serveri virtuaalse masina Galerii piltidega."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>SQL serveri virtuaalse masina Azure PowerShelli (ressursihaldur) kaudu ettevalmistamine

> [AZURE.SELECTOR]
- [Portaal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShelli](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Ülevaade

Selle õpetuse näitab, kuidas luua ühe Azure virtuaalse masina näidise **Azure'i ressursihaldur** juurutamise Azure PowerShelli cmdlet-käskude abil. Selles õpetuses loome ühe virtuaalse masina ühe kettale pilt galeriis SQL-i abil. Loome uue pakkujate salvestusruumi, võrgu ja Arvuta ressursse, mis kasutavad virtuaalse masina. Kui teil on olemasoleva pakkujad võivad need ressursid, saate need pakkujad.

Kui teil on vaja selle teema tavaversiooni, lugege teemat [sätte SQL serveri virtuaalse masina abil Azure PowerShelli klassikaline](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse peate:

- Azure'i konto ja tellimuse enne alustamist. Kui teil pole ühte [tasuta prooviversiooni](https://azure.microsoft.com/pricing/free-trial/)kasutajaks.
- [Azure PowerShelli)](../powershell-install-configure.md), miinimumversioon 1.4.0 või uuemat versiooni (selle õpetuse kirjutada, kasutades versiooni 1.5.0).
    - Tippige oma versiooni toomiseks **Get-moodul Azure-ListAvailable**.

## <a name="configure-your-subscription"></a>Tellimuse konfigureerimine

Avage Windows PowerShelli ja luua Accessi Azure'i kontosse, käitades järgmine cmdlet-käsk. Ilmub märgiga kuval sisestage oma kasutajanimi ja parool. Kasutage sama meiliaadressi ja parooliga, mida kasutada Azure portaali sisse logida.

    Add-AzureRmAccount

Pärast edukalt sisselogimisel kuvatakse osa teabest kuval, mis sisaldab SubscriptionId, kellega te sisse loginud. See on see tellimus, mille selles õpetuses mõeldud ressursid luuakse juhul, kui muudate mõnda muud tellimust. Kui teil on mitu SubscriptionIds, käivitage järgmine cmdlet tagastamiseks kõigi oma SubscriptionIds loendit:

    Get-AzureRmSubscription

Mõne muu SubscriptionID muutmiseks käivitage järgmine cmdlet koos teie soovitud SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Pildi muutujate määratlemine

Kasutatavuse ja mõistmise lõplikus käsikirjaga selles õpetuses lihtsustamiseks Alustame määratledes muutujate arv. Parameetrite väärtused muuta, kui sobib, kuid Hoiduge nimetamine nime pikkus ja erimärkide esitatud väärtuste muutmisel seotud piirangud.

### <a name="location-and-resource-group"></a>Asukoht ja ressursirühm
Kahe muutujate abil saate määratleda andmeala ja ressursirühm, millesse muud ressursid virtual masina loote.

Vastavalt soovile muuta ja sooritage järgmised cmdlet-käsud selle lähtestada.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Salvestusruumi atribuudid

Järgmiste muutujate abil saate määratleda salvestusruumi konto ja kasutada virtuaalse masina tüüpi.

Vastavalt soovile muuta ja seejärel käivitada järgmine cmdlet-käsk selle lähtestada. Pange tähele, et selles näites me kasutame [Premium mälu](../storage/storage-premium-storage.md), mis on soovitatav tootmise töökoormus. Juhised ja muud soovitused kohta täpsema teabe saamiseks vt [jõudluse head tavad SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Atribuudid

Järgmiste muutujate abil saate määratleda võrgu liidese, TCP/IP eraldatud meetod, virtuaalse võrgu nimi, virtuaalse alamvõrgu nimi, virtuaalse võrgu vahemik, IP-aadressid, vahemik, IP-aadresside alamvõrgu ja üldkasutatava nimesildi kasutavad võrgu virtual kohapeal.

Vastavalt soovile muuta ja seejärel käivitada järgmine cmdlet-käsk selle lähtestada.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Virtuaalse masina atribuudid

Järgmiste muutujate abil saate määratleda virtuaalse masina nimi, arvuti nimi, virtuaalse masina suurus ja operatsioonisüsteemi ketta virtuaalse masina nimi.

Vastavalt soovile muuta ja seejärel käivitada järgmine cmdlet-käsk selle lähtestada.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Pildi atribuudid

Järgmiste muutujate abil saate määratleda pildi virtuaalse masina jaoks kasutada. Selles näites kasutatakse SQL Server 2016 Enterprise pilt.

Vastavalt soovile muuta ja seejärel käivitada järgmine cmdlet-käsk selle lähtestada.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Võtke arvesse, et saate SQL serveri pilt pakkumiste käsk Get-AzureRmVMImageOffer täieliku loendi.

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Ja te näete saadaval käsk Get-AzureRmVMImageSku pakkumine SKU-d. Järgmine käsk kuvatakse kõigis SKU-des saadaval **SQL2014SP1-WS2012R2** pakkuda.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Ressursi rühma loomine

Ressursihaldur juurutamise mudeliga on esimene objekt, mille loote ressursirühma. Cmdlet-käsu [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) kasutame on Azure ressursirühm ja ressursside loomine ressursi rühma nime ja asukoha määratletud muutujate, mida te varem lähtestada.

Käivitada järgmine cmdlet-käsk luua oma uue ressursirühma.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

Virtuaalse masina nõuab salvestusruumi ressursid operatsioonisüsteemi ketta ja SQL serveri andmeid ja Logi failid. Lihtsa, saame luua ühe ketta mõlemad. Saate lisada täiendavad ketast hiljem, kasutades cmdlet-käsu [Lisa-Azure'i ketas](https://msdn.microsoft.com/library/azure/dn495252.aspx) , et paigutada SQL serveri andmeid ja logifailide spetsiaalne kettale. Kasutame cmdlet-käsu [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) oma uue ressursirühma ja salvestusruumikonto nimi, salvestusruumi Sku nimi ja asukoht määratletud muutujate, mida te varem lähtestatud kindlad konto loomiseks.

Käivitada järgmine cmdlet-käsk salvestusruumi uue konto loomiseks.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Võrgu ressursse loomine

Virtuaalse masina on vaja võrgu ressursse võrguühendus.

- Iga virtuaalse masina nõuab virtuaalse võrgus.
- Virtuaalse võrgu peab olema vähemalt üks alamvõrgu määratletud.
- Võrgu liidese tuleb määratleda, kas avalik või privaatne IP-aadress.

### <a name="create-a-virtual-network-subnet-configuration"></a>Looge virtuaalse võrgu alamvõrgu konfiguratsioon

Alustame loomisega alamvõrgu konfiguratsioon meie virtuaalse võrgu jaoks. Meie õpetuse loome Vaikimisi alamvõrgu, cmdlet-käsu [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) abil. Loome meie virtuaalne alamvõrgu võrgukonfiguratsioon eesliitega alamvõrgu nime ja aadressi määratletud muutujate, mida te varem lähtestada.

>[AZURE.NOTE] Täiendavate atribuutide virtuaalse võrgu alamvõrgu konfiguratsioonist cmdlet-käsu abil saate määratleda, kuid see on selles õpetuses väljapoole.

Luua oma virtuaalse alamvõrgu konfiguratsioon järgmine cmdlet-käsk käivitada.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Virtuaalse võrgu loomine

Järgmiseks loome meie virtuaalse võrgu cmdlet-käsu [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) abil. Loome uue ressursirühma meie virtuaalse võrgu, nime, asukoha ja aadressi eesliite määratletud kasutatud muutujad, mida te varem lähtestada, ja kasutades alamvõrgu konfiguratsioonist, mis eelmises etapis määratletud.

Käivitada järgmine cmdlet-käsk virtuaalse võrgu loomiseks.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Avaliku IP-aadressi loomine

Nüüd kus oleme meie virtuaalse võrgu määratletud, tuleb konfigureerida Ühenduvus virtuaalse masina IP-aadress. Selles õpetuses loome avaliku IP-aadressi kasutades dünaamilise IP-aadresside toetama Interneti-ühendus. Kasutame cmdlet-käsu [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) loodud prevously ressursirühma ja nime, asukoht, eraldatud meetod ja DNS-i domeeni nimesildi abil muutujad, mida saate lähtestada varem määratletud avaliku IP-aadressi loomiseks.

>[AZURE.NOTE] Täiendavate atribuutide avaliku IP-aadress see cmdlet-käsu abil saate määratleda, kuid see on algse õppeteema väljapoole. Staatilise aadressiga võiks luua ka privaatne aadress või aadress, kuid see on ka selles õpetuses väljapoole.

Käivitada järgmine cmdlet-käsk luua avaliku IP-aadressi.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Võrgu liidese loomine

Meil on nüüd valmis looma meie virtuaalse masina kasutavate võrgu liidese. Kasutame cmdlet-käsu [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) meie võrgu liidese loomist ressursirühma loodud varasemates ja nimi, asukoht, alamvõrgu ja eelnevalt määratletud avaliku IP-aadress.

Käivitada järgmine cmdlet-käsk luua oma võrgu kasutajaliides.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>VM objekti konfigureerimine

Nüüd kus oleme määratletud salvestusruumi ja ressursid, oleme valmis määratleda virtuaalse masina Arvuta ressursid. Meie juhend me määrata virtuaalse masina suurus ja erinevate operatsioonisüsteemi atribuudid, määrake määratlemine bloobimälu võrgu kasutajaliides, mis varem loonud, ja seejärel määrake operatsioonisüsteemi ketas.

### <a name="create-the-vm-object"></a>VM objekti loomine

Alustame määrates virtuaalse masina suurus. Selles õpetuses mõeldud meil on täpsustades on DS13. Kasutame cmdlet-käsu [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) loomiseks konfigureeritav virtuaalse masina nimi ja suurus määratletud muutujate, mida te varem lähtestada.

Käivitada järgmine cmdlet-käsk virtuaalse masina objekti loomiseks.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Hoidke all nimi ja parool kohaliku administraatori identimisteave mandaadi objekti loomine

Enne, kui me saate määrata virtuaalse masina operatsioonisüsteemi atribuutide, läheb vaja identimisteabe turvalist stringina kohaliku administraatorikonto esitama. Selleks kasutame cmdleti [Get-mandaati](https://technet.microsoft.com/library/hh849815.aspx) .

Käivitada järgmine cmdlet-käsk ja Windows PowerShelli mandaati taotluse aknas, sisestage nimi ja parool, mida kasutada kohaliku administraatorikonto Windows virtual kohapeal.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Virtuaalne arvuti opsüsteem atribuutide seadmine

Nüüd on valmis virtual arvuti opsüsteem atribuutide seadmine. Kasutame cmdlet [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) seadmine Windowsi operatsioonisüsteemi tüüp, nõua [virtuaalse masina agent](virtual-machines-windows-classic-agents-and-extensions.md) olema installitud, saate määrata, et cmdlet võimaldab automaatne värskendamine ja virtuaalse masina nimi, arvuti nimi ja mandaati kasutades muutujate, mida te varem lähtestada.

Käivitada järgmine cmdlet-virtual arvuti opsüsteem atribuutide seadmine.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Virtuaalse masina võrgu liidese lisamine

Seejärel lisame võrgu liidese loodud varem virtuaalse masina. Kasutame [Lisa-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) cmdlet-käsk võrgu liidese varem määratletud võrgu kasutajaliidese muutuja abil lisada.

Käivitada järgmine cmdlet-käsk võrgu liidese virtual arvuti määramiseks.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Kasutatav virtuaalse masina ketta bloobimälu talletuskoha seadmine

Järgmiseks me seada bloobimälu talletuskoha kettale kasutada virtuaalse masina varem määratletud muutujaid.

Käivitada järgmine cmdlet-käsk bloobimälu salvestusruumi asukoha määramiseks.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Operatsioonisüsteemi virtuaalse masina ketta atribuutide seadmine

Järgmiseks me seada operatsioonisüsteem virtuaalse masina ketta atribuudid. Kasutame cmdlet [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) määrata, virtuaalse masina operatsioonisüsteemi pärit pildi määramiseks vahemällu lugeda ainult (kuna samale kettale installitakse SQL serveri) ja määrata virtuaalse masina nimi ja operatsioonisüsteemi ketas määratletud me varem määratletud muutujate abil.

Käivitada järgmine cmdlet-käsk operatsioonisüsteemi jaoks virtual arvuti kettale atribuutide määramiseks.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Saate määrata pildi platvormi virtuaalse masina jaoks

Meie viimases etapis konfigureerimine on meie virtuaalse masina platvormi pildi määrata. Meie juhend kasutame uusim SQL Server 2016 CTP pilt. Cmdlet [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) kasutame varem määratletud muutujad määratletud selle pildi kasutamiseks.

Käivitada järgmine cmdlet-käsk platvormi pildi virtual arvuti määramiseks.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>SQL-i VM loomine

Nüüd, kui olete konfigureerimise juhised, olete valmis looma virtuaalse masina. Kasutame cmdlet-käsu [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) virtuaalse masina muutujaid, mis meil on määratletud loomiseks.

Järgmine cmdlet-käsk loomiseks arvuti virtuaalne käivitada.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Luuakse virtuaalse masina. Pange tähele, et kindlad konto loodud buutimine diagnostika, kuna virtual arvuti kõvaketta jaoks määratud salvestusruumi konto on premium salvestusruumi konto.

Nüüd saate vaadata selle arvuti Azure'i portaalis näha [avaliku IP-aadressi ja selle täielik domeeninimi](virtual-machines-windows-portal-sql-server-provision.md#Connect).  

## <a name="example-script"></a>Skripti näide

Järgmine skript sisaldab täieliku PowerShelli skripti selles õpetuses. Eeldatakse, et teil on juba häälestamise Azure tellimuse **Lisa-AzureRmAccount** ja **Valige-AzureRmSubscription** käske kasutada.


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Järgmised sammud
Pärast virtuaalse masina on loodud, olete valmis ühenduse virtuaalse masina RDP ja häälestamise Ühenduvus. Lisateavet leiate teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure (ressursihaldur)](virtual-machines-windows-sql-connect.md).
