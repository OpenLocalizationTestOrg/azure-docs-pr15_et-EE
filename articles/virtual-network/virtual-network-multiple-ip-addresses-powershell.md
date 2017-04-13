<properties
   pageTitle="Mitu IP-aadressi virtuaalmasinates - PowerShelli | Microsoft Azure'i"
   description="Saate teada, kuidas määrata mitu IP-aadressi virtuaalse masina Azure PowerShelli kaudu."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Mitu IP-aadressi määramiseks virtuaalmasinates

Azure virtuaalse masina (VM) võib olla ühe või mitme võrgu liidesed (NIC) oleks lisatud. Mis tahes NIC võib olla üks või mitu avalik või privaatne IP-aadresside määratud. Kui olete tuttav Azure IP-aadressid, lugege lisateavet nende [IP-aadresside Azure](virtual-network-ip-addresses-overview-arm.md) artikkel. Selles artiklis selgitatakse, kuidas määrata mitu IP-aadressi VM Azure ressursihaldur juurutamise mudeli Azure PowerShelli kasutamine.

Mitu IP-aadressi määramisel VM võimaldab järgmisi funktsioone.

- Mitme veebisaitide või teenuste eri IP-aadressid ja SSL-sertide ühes serveris majutada.
- Olla võrgu virtuaalse seadme, nt tulemüüri või laadimine koormusetasakaalustusteenuse.
- Võimalus lisada mis tahes privaatne IP-aadresside mis tahes NICs Azure'i laadi koormusetasakaalustusteenuse tagaandmebaas ühendada. Varem võib lisada ainult esmane IP-aadress esmane NIC tagaandmebaas pool.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Eelvaate registreeruda meilisõnumit saata oma tellimuse ID-ga [Mitu IP -d](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) ja kasutamine.

## <a name="scenario"></a>Stsenaarium

Selles artiklis seostate kolme IP konfiguratsioone võrgu liides.
Järgmises näites kuvatakse on loodud ja määratud NIC, mis on kolm privaatne IP-aadressid ja ühe avaliku IP-aadress see määratud.

- IPConfig-1: Dünaamiline privaatne IP-aadress (vaikimisi) ja mõne avaliku IP-aadresside avaliku IP address ressurss nimega PIP1.
- IPConfig-2: Staatilise privaatne IP-aadress ja avaliku IP-aadressi.
- IPConfig-3: Dünaamiline privaatne IP-aadress ja avaliku IP-aadressi.

    ![Teksti alt pilt](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

See stsenaarium eeldab, et teil on nimega *RG1* , kus on soovitud VNet nimetatakse *VNet1* ja alamvõrku, mis on nimega *Subnet1*ressursirühma. Lisaks eeldatakse, et teil on nimega *VM1*VM, sellega seotud võrgu liidese, nimetatakse *VM1-NIC1* ja avaliku IP-aadressi nimetatakse *PIP1*.

[Selles artiklis](./virtual-machines/virtual-machines-windows-ps-create.md ) tutvustatakse, kuidas luua juhuks, kui te pole loonud need enne ülalnimetatud ressursside.

## <a name = "create"></a>Luua VM mitu IP-aadressid

1. Avage käsuviip PowerShelli ja täitke ülejäänud juhised selle jaotise PowerShelli ühe seansi jooksul. Kui te pole veel installinud ja konfigureerinud PowerShelli, täitke [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) artiklis toodud juhiseid.

2. Azure'i [asukoha](https://azure.microsoft.com/regions) nimes [ressursirühm](../azure-resource-manager/resource-group-overview.md#resource-groups), VNet ressursirühma, alamvõrku, et NIC ühendatava ja NIC. nimi on virtuaalse võrgu "väärtused" järgmised $Variables muutmine Täitke juhised lisada mitu IP-aadressi mis tahes NIC manustatud VM, kui vaja.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Kui te ei tea olemasoleva Azure asukoha või ressursirühm nime, tippige järgmine käsk:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>NIC peab olema ühendatud on alamvõrgu võrgustikus mõne olemasoleva Azure virtuaalse (VNet). Kolm osa: NIC, alamvõrgu ja VNet, tuleb kõik olemas sama piirkonna-ja [tellimusega](../azure-glossary-cloud-terminology.md#subscription).  Kui olete tuttav VNets, lugege lisateavet nende või [luua mõne VNet](virtual-networks-create-vnet-arm-ps.md) artiklist saate teada, kuidas luua [virtuaalne võrgu ülevaade](virtual-networks-overview.md) artikkel.

    Kui te ei tea mõne olemasoleva VNet nimi, sisestage järgmine käsk ja Asendage *VNet1* eelmise muutuja nimi on VNet.

        Get-AzureRmVirtualNetwork | Format-Table Name

    Kui tagastatud loend on tühi, peate looma ka VNet. Saate teada, kuidas, lugege artiklit [virtuaalse võrgu loomine](virtual-networks-create-vnet-arm-ps.md) .

    Tippige olemasoleva alamvõrku sees on VNet nime hankimine ja asendamiseks mõne alamvõrgu nime *Subnet1* kohal järgmised käsud:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Sisestage järgmine käsk tuua alamvõrgu ja selle määrata muutujana.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>IP konfiguratsioone, mida soovite määrata NIC. määratlemine Iga konfiguratsiooni võib olla üks staatiline või dünaamiline privaatne IP-aadress ja üks seotud avaliku IP-aadressi ressursi staatiline või dünaamiline aadress.

    Lisage või eemaldage mis tahes arv kuvatakse järgige sõltuvalt sellest, mida soovite siduda NIC ja sätete mitu IP-aadressid, mida soovite konfigureerida.

    **IPConfig-1**

    Muutke *PIP1* mõne olemasoleva avaliku IP aadressi ressurss, mis on olemas loomisel rakenduses NIC asukoha nime ja mis pole praegu seostatud teise NIC. Muuda *RG1* ressursirühma avaliku IP-aadressi ressursi nimi on olemas. Saate muuta nime, mida soovite anda esimese IP-konfiguratsiooni *IPConfig-1* . Sisestage järgmine käsk:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Märkus selle *-esmane* aktiveerimine. Kui määrate mitu IP konfiguratsioone Võrguadapter, üks konfiguratsioon peab olema määratud nimega on *esmane*. Kui te ei tea olemasoleva avaliku IP address ressursi nimi, sisestage järgmine käsk: Get-AzureRMPublicIPAddress | Vormingu tabelinimi, asukoht, sordib, IpConfiguration

    Kui **IPConfiguration** veerg sisaldab tagastatud väljund pole väärtust, ei seostata mõne olemasoleva NIC avaliku IP-aadressi ressursi ja seda saab kasutada. Kui loend on tühi või saadaval avaliku IP address ressursse pole, saate luua ühe käsu **New-AzureRmPublicIPAddress** .

    >[AZURE.NOTE] Avaliku IP-aadressid on nominaalse tasu. IP address hindade kohta lisateabe saamiseks lugege [IP-aadressi hinnad](https://azure.microsoft.com/pricing/details/ip-addresses) lehe.

    **IPConfig-2**

    Saate muuta *IPConfig-2* nime, mida soovite anda teise IP-konfiguratsiooni ja muuta *10.0.0.5* alamvõrku, et NIC määrate kasutamata lubatud IP-aadress. Sisestage järgmine käsk:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Sisestage järgmine käsk, kui te ei tea IP alamvõrgu määratud vahemiku aadress:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    Muuda *IPConfig-3* nime, mida soovite anda kolmanda IP konfiguratsiooni, ning sisestage järgmised käsud:

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Saate määrata kuni 250 privaatne IP-aadress on NIC. Pole avaliku IP-aadressid, mida saab kasutada tellimuse arv on piiratud. Lisateabe saamiseks lugege artiklit [Azure'i piirab](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Looge NIC, kasutades eelmises etapis määratletud IP kuvatakse.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. Manusta NIC VM loomisel [loomine VM](../virtual-machines/virtual-machines-windows-ps-create.md) artiklis toodud juhiseid järgides. Kuigi see artikkel loob VM, kus töötab Windows Server, juhised kehtivad sama Linux VM, valides operatsioonisüsteemi peale. Täitke selle artikli juhiseid 1 – 3. Vahele jätta samme 4 ja 5 ja seejärel valmis juhist 6 VM artiklis loomine.

    >[AZURE.WARNING] Juhist 6 loomine VM artiklis nurjub, kui muudetud nimega $nic midagi muud juhist 6 käesoleva artikli muutuja või pole täitnud käesoleva artikli eelmisi juhiseid.

8. Vaate privaatne IP-aadressid selle Azure'i DHCP määratud NIC ja sisestage järgmine käsk NIC määratud avaliku IP address ressurss:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Käsitsi lisada kõik privaatse IP-aadresside (sh esmast) TCP/IP konfiguratsioon operatsioonisüsteemi. 

**Windows**

1. Käsuviip, tippige *ipconfig/kõik*.  Näete ainult *esmane* privaatne IP-aadress (DHCP) kaudu.
2. Tippige järgmine *ncpa.cpl* käsuviiba aken. See avab uues aknas.
3. Saate avada atribuutide **Kohalik võrguühendus**.
4. Topeltklõpsake Interneti-protokolli versioon 4 (IPv4)
5. Valige **Kasuta järgmist IP-aadress** ja sisestage järgmised väärtused:
    - **IP-aadress**: Sisestage *esmane* privaatne IP-aadress
    - **Alamvõrgu mask**: komplekti, mis põhineb teie alamvõrku. Näiteks kui alamvõrgu on mõne /24 alamvõrgu alamvõrgu mask on 255.255.255.0.
    - **Vaikimisi lüüsi**: alamvõrgu esimese IP-aadress. Kui teie alamvõrku on 10.0.0.0/24, siis gateway IP-aadress on 10.0.0.1.
    - Klõpsake nuppu **Kasuta järgmisi DNS-serveri aadresse** ja sisestage järgmised väärtused:
        - **Eelistatud DNS-server:** Kui kasutate oma DNS-i server, sisestage 168.63.129.16.  Kui teil on, sisestage oma DNS-i serveri IP-aadress.
    - Klõpsake nuppu **Täpsemalt** ja lisage täiendavad IP-aadressid. Lisage iga teisene privaatne IP-aadresside loetletud NIC toiming 8 sama alamvõrgu määratud esmane IP-aadressi.
    - Klõpsake nuppu **OK** , et sulgeda TCP/IP sätted ja seejärel uuesti võrguadapteri sätted sulgemiseks **OK** . See on taastamiseks klõpsake RDP-ühendus.

6. Käsuviip, tippige *ipconfig/kõik*. Kuvatakse kõik lisatud IP-aadressid ja DHCP on välja lülitatud.


**Linux (Ubuntu)**

1. Avage terminal aknas.
2. Veenduge, et olete root kasutaja. Kui te pole, saate seda teha järgmise käsu abil.

            sudo -i
3. Värskendage konfiguratsioonifail võrgu liides (eeldades "eth0").
    - Säilita DHCP olemasoleva kirje. See konfigureerib nagu varem varasemates esmane IP-aadress.
    - Täiendavad staatiline IP-aadressi järgmised käsud konfiguratsiooni lisamiseks tehke järgmist.

                cd /etc/network/interfaces.d/
                ls

        Peaksite nägema .cfg faili.
4. Avage fail: vi *faili nimi*.

    Peaksite nägema faili lõpus järgmised read:

            auto eth0
            iface eth0 inet dhcp
5. Lisage järgmised read pärast read, mis on selles failis olemas.

            iface eth0 inet static
            address <your private IP address here>
6. Salvestage fail abil järgmine käsk:

            :wq
7.  Lähtesta võrgu liidese järgmine käsk:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Käivitage ifdown nii ifup samal real, kui kaugühenduse kaudu.
8. Kontrollige IP-aadress on lisatud võrgu liidese järgmine käsk:

            ip addr list eth0

    Peaksite nägema loendi osana lisatud IP-aadress.

**Linux (Redhat, CentOS ja muud)**

1. Avage terminal aknas.
2. Veenduge, et olete root kasutaja. Kui te pole, saate seda teha järgmise käsu abil.

            sudo -i
3. Sisestage oma parool ja järgige juhiseid, kui seda küsitakse. Kui olete root kasutaja, liikuge skriptide võrgukausta järgmise käsu abil:

            cd /etc/sysconfig/network-scripts
4. Loendi seotud ifcfg faile järgmine käsk:

            ls ifcfg-*

    Peaksite nägema *ifcfg-eth0* ühe failid.
5. Kopeerige fail *ifcfg-eth0* ja pange sellele *ifcfg-eth0:0* järgmine käsk:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Redigeerimine on *ifcfg-eth0:0* faili järgmine käsk:

            vi ifcfg-eth1
7. Seade failis; õige nime muutmine *eth0:0* sel juhul järgmine käsk:

            DEVICE=eth0:0
8. Muuda selle *IPADDR = YourPrivateIPAddress* joone kajastamiseks IP-aadress.
9. Salvestage fail järgmine käsk:

            :wq
10. Taaskäivitage võrguteenuste ja veenduge, et muudatused on eduka käivitades järgmised käsud:

            /etc/init.d/network restart
            Ipconfig

    Peaksite nägema IP-aadress on lisatud, *eth0:0*, tagastatakse loendis.

## <a name="add"></a>Mõne olemasoleva VM IP-aadresside lisamine

Täitke järgmised juhised täiendavad IP-aadresside lisamiseks mõne olemasoleva NIC:

1. Avage käsuviip PowerShelli ja täitke ülejäänud juhised selle jaotise PowerShelli ühe seansi jooksul. Kui te pole veel installinud ja konfigureerinud PowerShelli, täitke [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) artiklis toodud juhiseid.

2. NIC, mille soovite lisada IP-aadressid ressursirühm ja NIC on olemas asukoht ja nimi "väärtused" järgmised $Variables muutmiseks tehke järgmist.

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Kui te ei tea nime, mida soovite muuta, sisestage järgmised käsud NIC, muutke eelmise muutujate väärtusi:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Muutuja loomine ja määrake selle väärtuseks olemasoleva NIC tippige järgmine käsk:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Saate tuua NIC on ühendatud, täites [Samm 3](#subnet) VM loomine mitme jaotises IP addresses käesoleva artikli alamvõrgu ID-d.

5. Saate luua IP konfiguratsioone, mida soovite lisada võrku [juhis 5](#ipconfigs) loomise juhiseid järgides VM mitme jaotises IP addresses käesoleva artikli.

6. Saate muuta *$IPConfigName4* eelmises juhises loodud IP-konfiguratsiooni nimi. Konfiguratsiooni lisamiseks sisestage järgmine käsk:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. NIC IP konfiguratsioon, sisestage järgmine käsk:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Lisage lisasite NIC VM operatsioonisüsteemi loomine VM koos mitme jaotises IP addresses käesoleva artikli juhiste [Samm 9](#os) IP-aadressid.
