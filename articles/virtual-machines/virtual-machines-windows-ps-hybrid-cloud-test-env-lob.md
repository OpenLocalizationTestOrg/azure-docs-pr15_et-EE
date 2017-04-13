<properties 
    pageTitle="LOB rakenduse testimiskeskkonnas | Microsoft Azure'i" 
    description="Saate teada, kuidas seda pro pilve hübriidkeskkonna või arengu testimine veebipõhise rida ärirakendusele loomine." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Hübriid pilveteenuses testimiseks veebipõhine LOB rakenduse häälestamine

Selles teemas juhendab teid loomise jäljendatud hübriid cloud keskkonnas testimiseks veebipõhine rida majutatud Microsoft Azure ärirakendusele (LOB). Siin on tulemuseks konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Selle konfiguratsiooni koosneb järgmistest elementidest:

- Jäljendatud kohapealse võrgu majutatud Azure (TestLab VNet).
- Asutusesiseses virtuaalse võrgu majutatud Azure (TestVNET).
- VNet-VNet VPN-ühendus.
- Veebipõhise LOB server, SQL serveri ja teisese domeenikontrolleri TestVNET virtuaalse võrgu.

Selle konfiguratsiooni pakub põhjal ja mille abil saab levinud alguspunkti.

- Arendamise ja testida LOB rakenduste majutatud 2014 SQL serveri andmebaasi taustväärtus Azure Internet Information Services (IIS).
- Testida jäljendatud hübriidi pilvepõhist see töökoormus.

On kolme isikuandmete hübriid pilve testimiskeskkonnas häälestamiseks.

1.  Saate häälestada jäljendatud hübriidkeskkonna pilve.
2.  Saate konfigureerida SQL serveri arvuti (SQL1).
3.  Konfigureerige LOB serveri (LOB1).

See töökoormus nõuab Azure tellimuse. Kui olete tellimuse MSDN-i või Visual Studio, vaadake [kuu Azure'i krediiti Visual Studio tellijad](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Näiteks valmistamisel LOB rakenduse majutatud Azure, leiate teemast **ärirakenduste** arhitektuur näidis [Microsofti tarkvara arhitektuur diagrammide](http://msdn.microsoft.com/dn630664)ja jooniste.

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Etapp 1: Jäljendatud hübriid pilve keskkonna häälestamine

Saate luua [modelleerida hübriid pilve testimiskeskkonnas](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Kuna see testimiskeskkonnas ei nõua APP1 serveri Corpnet alamvõrgu olemasolu, saate sulgeda nüüd.

See on teie praeguse konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Etapp 2: Konfigureerimine SQL serveri arvuti (SQL1)

Azure'i portaalis, käivitage DC2 arvuti vajaduse korral.

Järgmiseks luua virtuaalse masina SQL1 neid käske Azure PowerShelli käsuviibas kohalikus arvutis. Muutuv väärtuste sisestamiseks enne töötab need käsud, ja eemaldage selle < ja > märgid.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Kasutada Azure portaali ühenduse SQL1 SQL1 kohaliku administraatori kontoga.

Järgmiseks konfigureerida Windowsi tulemüüri reeglid ühenduvuse testimine ja SQL serveri liikluse lubamiseks. Administraatori tasemel Windows PowerShelli Käsuviip SQL1 kohta, käivitage järgmised käsud.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Ping-käsk peaks tulemuseks nelja eduka vastused IP-aadress 192.168.0.4.

Järgmisena lisage eest andmed kettale SQL1 uus draiv ketas tähega f nimega.

1.  Serveri halduri vasakpaanil nuppu **fail ja salvestusruumi teenused**ja klõpsake **ketast**.
2.  Paanil sisu **ketast** jaotises nuppu **kettalt 2** (koos **Partition** määramine **tundmatu**).
3.  Klõpsake nuppu **Tööülesanded**ja seejärel klõpsake nuppu **Uus draiv**.
4.  Enne Alustage uut helitugevuse viisardi lehel, klõpsake nuppu **edasi**.
5.  Valige lehe server ja kettale, klõpsake **kettale 2**ja seejärel klõpsake nuppu **edasi**. Kui kuvatakse vastav viip, klõpsake nuppu **OK**.
6.  Määrake helitugevus lehe suuruse, klõpsake nuppu **edasi**.
7.  Ketas tähe või kausta lehe määramine, klõpsake nuppu **edasi**.
8.  Lehel Valige faili sätted, klõpsake nuppu **edasi**.
9.  Klõpsake lehel Confirm Valikud nuppu **Loo**.
10. Kui olete lõpetanud, klõpsake nuppu **Sule**.

Käivitage SQL1 Windows PowerShelli käsuviibas järgmised käsud:

    md f:\Data
    md f:\Log
    md f:\Backup

Järgmiseks liituda SQL1 CORP Windows Server Active Directory domeeni neid SQL1 klõpsake viiba Windows PowerShelli käske.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Kasutage CORP\User1 konto küsimise esitama domeeni konto identimisteave käsk **Lisa-arvuti** jaoks.

Pärast taaskäivitada, Azure portaali ühenduse loomiseks kasutada SQL1 *, SQL1 kohaliku administraatorikontoga*.

Järgmiseks konfigureerida SQL Server 2014 kasutada uute andmebaaside ja kasutajale konto õiguste f ketas.

1.  Avakuvale ja tippige **SQL Server Management**ja seejärel käsku **SQL Server 2014 Management Studio**.
2.  **Loo ühendus serveriga**, klõpsake käsku **Ühenda**.
3.  Objekti Exploreri puu paanil **SQL1**paremklõpsake ja seejärel klõpsake käsku **Atribuudid**.
4.  Klõpsake aknas **Atribuudid** **Andmebaasi sätted**.
5.  Leidke **andmebaasi vaikeasukohta** ja määrata need väärtused. 
    - **Andmete**korral tippige tee **f:\Data**.
    - **Log**, tippige tee **f:\Log**.
    - **Varundus**, tippige tee **f:\Backup**.
    - Märkus: Ainult uute andmebaaside kasutamine järgmistesse asukohtadesse.
6.  Klõpsake nuppu **OK** , sulgege aken.
7.  Avage paanil **Objekti Exploreri** puu **Turvalisus**.
8.  Paremklõpsake **sisselogimise** ja seejärel klõpsake nuppu **Uus Logi sisse**.
9.  Tippige väljale **kasutajanimi** **CORP\User1**.
10. Lehel **Serverirollide** **süsteemiadministraator**nuppu ja seejärel klõpsake nuppu **OK**.
11. Sulgege Microsoft SQL Server Management Studio.

See on teie praeguse konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Etapp 3: Konfigureerimine LOB serveri (LOB1)

Esmalt looge virtuaalse masina LOB1 neid käske Azure PowerShelli käsuviibas kohalikus arvutis.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Järgmiseks LOB1 ühenduse loomine kohaliku administraatorikonto, LOB1 identimisteabega Azure portaali abil.

Järgmiseks konfigureerida Windowsi tulemüüri reegli ühenduvuse kontrollimise liikluse lubamiseks. Administraatori tasemel Windows PowerShelli Käsuviip LOB1 sisse, käivitage järgmised käsud.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Ping-käsk peaks tulemuseks nelja eduka vastused IP-aadress 192.168.0.4.

Järgmiseks liita LOB1 CORP Active Directory domeeni ja need käsud Windows PowerShelli kaudu.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Kasutage CORP\User1 konto küsimise esitama domeeni konto identimisteave käsk **Lisa-arvuti** jaoks.

Pärast taaskäivitada, Azure portaali ühenduse loomiseks kasutada LOB1 CORP\User1 konto ja parooliga.

Järgmiseks LOB1 IIS-i konfigureerimine ja testimine CLIENT1 juurdepääs.

1.  Kaudu serveri haldur käsku **Lisa rollid ja funktsioonid**.
2.  **Enne alustamist** lehel klõpsake nuppu **edasi**.
3.  Klõpsake lehel **Valige installi tüübi** , klõpsake nuppu **edasi**.
4.  Lehel **valige sihtkoht server** , klõpsake nuppu **edasi**.
5.  Klõpsake lehel **serverirollide** **rollid**loendis **Web Server (IIS)** .
6.  Vastava viiba kuvamisel klõpsake nuppu **Lisa funktsioone**ja seejärel klõpsake nuppu **edasi**.
7.  **Valige** lehel klõpsake nuppu **edasi**.
8.  Klõpsake lehel **Web Server (IIS)** klõpsake nuppu **edasi**.
9.  Lehel **Valige rolli teenused** valige või tühjendage märkeruudud teenused, mida vajate LOB rakenduse testimiseks ja seejärel klõpsake nuppu **edasi**.
10. **Valikute kinnitamine installimise** lehel klõpsake nuppu **Installi**.
11. Oodake, kuni komponentide installimine on lõpule viidud ja seejärel klõpsake nuppu **Sule**.
12. Azure'i portaalis CORP\User1 konto mandaati CLIENT1 arvutiga ja seejärel käivitage Internet Explorer.
13. Tippige **http://lob1/** aadressiribale ja vajutage sisestusklahvi ENTER. Peaksite nägema veebilehe vaikimisi IIS-i 8.

See on teie praeguse konfiguratsiooni.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
See keskkond on nüüd valmis LOB1 veebipõhise rakenduse juurutamine ja testimine Corpnet alamvõrgu CLIENT1 funktsioone.

## <a name="next-step"></a>Järgmise juhise juurde

- Saate lisada uue virtuaalse masina [Azure portaali](virtual-machines-windows-hero-tutorial.md).
