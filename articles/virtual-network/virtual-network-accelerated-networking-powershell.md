<properties 
   pageTitle="Kiirendada networking virtuaalse masina - PowerShelli jaoks | Microsoft Azure'i"
   description="Saate teada, kuidas konfigureerida kiirendada Networking Azure virtuaalse masina, mis PowerShelli kaudu."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Kiirendatud võrgunduse virtuaalse masina jaoks

> [AZURE.SELECTOR]
- [Azure'i portaal](virtual-network-accelerated-networking-portal.md)
- [PowerShelli](virtual-network-accelerated-networking-powershell.md)

Kiirendatud võrgunduse võimaldab ühe juurkausta i/o Virtualization (SR-IOV) virtuaalse masina (VM), parandades oma võrgu jõudlust. See suure jõudlusega tee, mille liiklus möödub host Andmeteed, vähendada latentsus, närvitsema ja Protsessori kasutuse jaoks kõige rangete võrgu töökoormus toetatud VM tüüpide kaudu. Selles artiklis selgitatakse, kuidas konfigureerida kiirendada Networking Azure'i ressursihaldur juurutamise mudeli Azure PowerShelli kasutamine. Samuti saate luua VM kiirendada võrgutoe Azure'i portaalis. Saate teada, kuidas, klõpsake välja Azure portaali selle artikli alguses.

Järgmisel pildil on kujutatud kahe virtuaalmasinates (VM) ja ilma kiirendada Networking suhtlemine:

![Võrdlus](./media/virtual-network-accelerated-networking-powershell/image1.png)

Ilma kiirendada võrgu kõigi võrgu liikluse ja sealt VM tuleb läbida selle hosti ja virtual switch. Virtuaalne lüliti pakub kõik poliitika jõustamine, nt võrgu turberühmad, pääsuloendid, eraldamise ja muude teenuste võrgu virtualiseeritud võrguliiklust. Lisateabe saamiseks lugege artiklit [Hyper-V võrgu Virtualization ja virtuaalse vahetamine](https://technet.microsoft.com/library/jj945275.aspx) .

Kiirendada võrgutoe võrguliiklust saabub võrgu kaart (NIC) ja seejärel edastatakse VM. Kõigi võrgu poliitika, mis virtual switch kehtib ilma kiirendada Networking on üle viidud ja riistvara rakendatud. Riistvara poliitika rakendamist võimaldab NIC VM, säilitades kõik selle hosti rakendatakse see poliitika selle hosti ja virtuaalse parameetrit mööda otse edasi võrguliikluse abil.

Kiirendada Networking eelised rakenduvad ainult VM, mis see on sisse lülitatud. Parimate tulemuste saamiseks sobib vähemalt kaks VMs ühendatud sama VNet selle funktsiooni.  Üle VNets või ühendusjooni kohapealse esitamisel on see funktsioon on minimaalne mõju üldise latentsuse.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Eelised

- **Lower latentsus / kõrgema paketid kohta teine (pps):** Funktsiooni Andmeteed virtual switch eemaldamine eemaldab aega kulub paketid host poliitika töötlemiseks ja paketid, mida saab töödelda sees VM arv suureneb.
- **Vähendatud jitter:** Virtuaalne lüliti töötlemine sõltub poliitika, mis tuleb haldurilt summa ja CPU, mis teeb töötlemise töökoormus. Selle hajuvuse mahalaadimine poliitika jõustamine riistvara abil pakkuda paketid otse VM, eemaldamise hosti VM suhtlus ja tarkvara katkestuste ja kontekstis parameetrid eemaldatakse.
- **Vähenemine Protsessori kasutuse:** Mööda virtuaalse sisse selle hosti viib vähem CPU kasutuse töötlemiseks võrguliiklust.

## <a name="limitations"></a>Piirangud

Selle funktsiooni kasutamisel olemas järgmised piirangud:
 
- **Network kasutajaliidese loomine:** Kiirendatud võrgunduse saab lubada ainult uue võrgu liidese.  See ei saa lubada olemasoleva võrgu liidesel.
- **VM loomine:** Võrgu liidese kiirendatud võrguühendusega saate manustada ainult VM VM loomisel. Võrgu liidese ei saa lisada mõne olemasoleva VM.
- **Regioonid:** Ainult Lääne keskse meile ja Lääne Euroopa Azure piirkondades saadaval. Piirkondade määramine laiendab tulevikus.
- **Toetatud operatsioonisüsteem:** Microsoft Windows Server 2012 R2 ja Windows Server 2016 tehnilise eelvaate 5. Linux ja Windows Server 2012 tugi lisatakse peagi.
- **VM suurus:** Standard_D15_v2 ja Standard_DS15_v2 on ainus toetatud VM eksemplari suurused. Lisateavet leiate artiklist [Windows VM suurused](../virtual-machines/virtual-machines-windows-sizes.md) . Toetatud VM eksemplari suuruses määramine laiendab tulevikus.

Need piirangud muudatustest avaldatakse [värskenduste Azure virtuaalse Networking](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lehe kaudu.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Windowsiga VM kiirendatud võrguühendusega loomine

1. Avage käsuviip PowerShelli ja täitke ülejäänud juhised selle jaotise PowerShelli ühe seansi jooksul. Kui te pole veel installinud ja konfigureerinud PowerShelli, täitke [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) artiklis toodud juhiseid.
2. Eelvaate registreeruda meilisõnumit saata [Kiirendada Networking tellimused](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) oma tellimuse ID-ga ja kasutamine. Täitke ülejäänud juhised kuni, kui olete saanud e-posti sõnumiga, kas te olete aktsepteerinud eelvaate sisse.
3. Registri võimalus tellimus sisestades järgmised käsud:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Asendage *westcentralus* nimi teise asukohta, mis toetab seda võimalust, mis on loetletud käesoleva artikli jaotises [piirangud](#limitations) (soovi korral). Sisestage asukoha muutuja seadmiseks järgmine käsk:

        $locName = "westcentralus"

5. Asendage *RG1* nimi ressursirühm, mis sisaldavad uue võrgu liidese ja sisestage selle loomiseks järgmised käsud:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Muuta *VM1-NIC1* , mida soovite võrgu liidese nime ja seejärel sisestage järgmine käsk:

        $NICName = "VM1-NIC1"

7. Võrgu liidese peab olema ühendatud võrgustikus mõne olemasoleva Azure virtuaalse (VNet) on alamvõrgu samasse asukohta ja [tellimuse](../azure-glossary-cloud-terminology.md#subscription) võrgu liides. Lisateave VNets, [virtuaalse võrgu ülevaate](virtual-networks-overview.md) artikli lugemist, kui olete tuttav. Mõne VNet loomiseks täitke [loomine on VNet](virtual-networks-create-vnet-arm-ps.md) artiklis toodud juhiseid. Järgmised $Variables "väärtused" VNet ja soovite luua ühenduse võrgu liidest alamvõrgu nime muuta.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Kui te ei tea mõne olemasoleva VNet sammus 3 valitud asukoha nimi, sisestage järgmine käsk:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Kui tagastatud loend on tühi, peate looma VNet soovitud asukohta. Mõne VNet loomiseks täitke [loomine virtuaalse võrgu](virtual-networks-create-vnet-arm-ps.md) artiklis toodud juhiseid.

    Alamvõrku sees on VNet nime, tippige järgmised käsud ja Asendage *Subnet1* kohal on alamvõrgu nimi:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Sisestage tuua VNet ja alamvõrgu ja nende määramiseks muutujate järgmised käsud.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Leidke mõne olemasoleva avaliku IP aadressi ressurss, mis võib olla seotud võrgu liidese nii, et saate luua ühenduse selle Interneti kaudu. Kui te ei soovi VM võrgu liidesega Interneti kaudu juurde, saate selle sammu vahele jätta. Ilma avaliku IP-aadressi, tuleb ühendada VM ühendatud sama VNet teise VM. 

    Saate muuta *PIP1* mõne olemasoleva avaliku IP aadressi ressursi asukoha loote võrgu liides ja mis pole praegu seostatud teise võrgu liidese olemasoleva nime. Vajadusel muutke $rgName nime ressursirühma avaliku IP-aadressi ressursi on olemas ja sisestage järgmine käsk:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Kui te ei tea olemasoleva avaliku IP address ressursi nimi, sisestage järgmised käsud:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Kui **IPConfiguration** veerg sisaldab tagastatud väljund pole väärtust, ei seostata olemasoleva võrgu liidest avaliku IP-aadressi ressursi ja seda saab kasutada. Kui loend on tühi või saadaval avaliku IP address ressursse pole, saate luua ühe käsu New-AzureRmPublicIPAddress.

    >[AZURE.NOTE] Avaliku IP-aadressid on nominaalse tasu. Lisateave hinnad lugedes lehe [hinnad IP-aadress](https://azure.microsoft.com/pricing/details/ip-addresses) .
10. Kui te pole kasutajaliideses avaliku IP-aadressi ressursi lisamine, eemaldamine *-PublicIPAddress $PIP1* järgneva käsu lõpus. Loo võrgu liidese kiirendatud võrgunduse sisestage järgmine käsk:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Määrake võrgu liidese VM VM loomisel juhiseid 3 – 6 [loomine VM](../virtual-machines/virtual-machines-windows-ps-create.md) artikli juhiseid järgides. Juhises 6-2, Asendage *Standard_A1* ühte käesoleva artikli jaotises [piirangud](#limitations) VM suurust.

    >[AZURE.NOTE] Kui olete muutnud $locName, $rgName või $nic muutujate selle artikli *nime* , juhist 6 VM artiklis loomine nurjub. Saate siiski muuta muutujate *väärtused* .

12. Kui VM on loodud, [kiirendada Networking draiver](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84)alla laadida, ühenduse VM ja käivitage draiveri Installeri VM sees.

13. Paremklõpsake Windowsi nuppu ja klõpsake nuppu **Seadmehaldur**. Veenduge, et **Mellanox ConnectX-3 virtuaalse funktsioon Ethernet adapterit** kuvatakse jaotises **võrku** suvand, kui laiendatud, nagu on näidatud järgmisel pildil.

    ![Seadmehaldur](./media/virtual-network-accelerated-networking-powershell/image2.png)