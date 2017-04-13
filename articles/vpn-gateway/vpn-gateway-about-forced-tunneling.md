<properties 
   pageTitle="Jõustatud tunneling saidilt ühenduste näidise klassikaline juurutuse konfigureerimine | Microsoft Azure'i"
   description="Kuidas redirect või "jõustada kõik Interneti seotud liikluse oma kohapealse võrra tagasi."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Jõustatud tunneling näidise klassikaline juurutuse konfigureerimine

> [AZURE.SELECTOR]
- [PowerShelli – klassikaline](vpn-gateway-about-forced-tunneling.md)
- [PowerShelli - ressursihaldur](vpn-gateway-forced-tunneling-rm.md)

Jõustatud tunneling võimaldab teil redirect või "Jõusta" kõik Interneti seotud liikluse tagasi oma kohapealse asukohta VPN saidilt tunneliga ülevaatuse ja auditeerimine kaudu. See on kriitiliste turvalisus nõue enamik ettevõtte IT poliitika. 

Jõustatud tunneling ilma Interneti seotud liiklus teie VMs Azure on alati läbida: Azure'i võrgu taristule otse välja Interneti-ühendus, ilma suvandi abil saate kontrollida või kontrollida liiklus. Volitamata Interneti-ühendus võib põhjustada potentsiaalselt teabe või muud tüüpi turvalisus seotud rikkumiste eest.

Selles artiklis juhendab teid konfigureerimise sunnitud tunneling virtuaalse võrkude klassikaline juurutamise mudeli abil loodud. 

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Juurutamise mudelite ja jõustatud tunneling tööriistad**

Jõustatud tunnelite ühenduse saab konfigureerida nii klassikaline juurutamise mudeli ja ressursihaldur juurutamise mudel. Vt lisateavet järgmises tabelis. Uuendame see tabel, kui selle konfiguratsiooni saadaval uus artikleid, uusi juurutamise mudeleid ja täiendavad tööriistad. Kui artikkel on saadaval, me lingi otse tabelist.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Nõuded ja kaalutlused

Jõustatud tunneling Azure on konfigureeritud virtuaalse võrgu kasutaja määratletud viisil (UDR) kaudu. Kohapealse saidi liikluse ümber väljendatud hinnaks dollarites vaikimisi marsruudi Azure'i VPN lüüsiga. Järgmises jaotises on loetletud praeguse piiramise marsruutimist tabeli ja marsruudib Azure virtuaalse võrgu jaoks:


-  Iga virtuaalse alamvõrku on valmis, süsteemi marsruutimise tabeli. Süsteemi marsruutimise tabelis on marsruudib järgmised kolm rühmad.

    - **Kohaliku VNet marsruudib:** Otse sihtkohta VMs virtuaalse samasse võrku
    
    - **Ruumide lennuliinide:** Azure'i VPN Gateway
    
    - **Vaikimisi marsruudi:** Otse Internetiga. Kõrvaldatakse paketid sinna privaatne IP-aadressid pole hõlmatud eelmise kahel viisil.


-  Kasutaja määratletud marsruudib versiooni, saate lisada vaikimisi marsruudi marsruutimise tabeli loomine ja seejärel seostada nii teie VNet subnet(s) lubamiseks klõpsake nende alamvõrku jõustatud tunneling.

- Peate määrama "vaikimisi saidi" asutusesiseses kohaliku saite virtuaalse võrku ühendatud.

- Jõustatud tunneling peavad olema seostatud VNet, mis sisaldab dünaamilise marsruutimise VPN lüüsi (mitte staatilise lüüs).
 
- ExpressRoute sunnitud tunneling kaudu see süsteem pole konfigureeritud, kuid selle asemel on lubanud reklaami vaikimisi marsruudi ExpressRoute BGP silmitsemine seansid kaudu. Lugege lisateavet [ExpressRoute dokumentatsiooni](https://azure.microsoft.com/documentation/services/expressroute/) .



## <a name="configuration-overview"></a>Konfiguratsiooni ülevaade

Järgmises näites on Frontend alamvõrgu ei joondata tunneled. Töökoormus Frontend alamvõrku, saate jätkata aktsepteerida ja vastata kliendi Interneti kaudu otse. Keskel tasandi ja Taustväärtus alamvõrku sunnitakse tunneled. Väljaminevad ühendused Interneti kaudu nende kahe alamvõrku on sunnitud või ümber tagasi anda kohapealse saidi ühe S2S VPN tunnelite kaudu.

See võimaldab teil piirata ja kontrollida Interneti-ühendus oma virtuaalmasinates või pilveteenused Azure, jätkates lubamiseks oma mitmekihilise teenuse arhitektuur nõutav. Samuti saate rakendada jõustatud tunneling kogu virtuaalse võrkudega kui virtuaalse võrguga puudub Interneti-ühendusega töökoormus.


![Sunnitud Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et enne konfigureerimine on järgmist.

- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).

- Virtuaalne võrgus konfigureeritud. 

- Azure'i PowerShelli cmdlet-käskude uusim versioon. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.


## <a name="configure-forced-tunneling"></a>Jõustatud tunneling konfigureerimine

Järgmiste toimingute abil saate määrata jõustatud tunneling virtuaalse võrgu jaoks. Konfiguratsioonifail VNet võrgu konfigureerimise juhised vastavad.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

Selles näites on virtuaalse võrgu "MultiTier VNet" kolme alamvõrku: *Frontend*, *Midtier*ja *Taustväärtus* alamvõrku, neli rist tööruumide ühendused: *DefaultSiteHQ*ja kolme *harude*. 

Juhised seadmine *DefaultSiteHQ* jaoks vaikimisi saidi ühendus sunnitud tunneling ja konfigureerida selle Midtier ja Taustväärtus alamvõrku kasutada sunnitud tunneling.


1. Marsruutimise tabeli loomine. Järgmine cmdlet-käsk abil saate luua tabeli marsruutimiseks.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Saate lisada nii vaikimisi marsruudi. 

    Järgmises näites liidetakse vaikimisi marsruudi sammus 1 loodud marsruutimise tabelisse. Pange tähele, et ainult marsruutimiseks toetatud on "0.0.0.0/0" "VPNGateway" NextHop eesliite sihtkoht.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Marsruutimise tabelis, et selle alamvõrku seostada. 

    Pärast tabeli marsruutimine on loodud ja marsruudi lisanud, kasutada järgmises näites lisamiseks või marsruutimiseks tabeli VNet alamvõrgu seostada. Käesolevas näites lisab Midtier ja kirjutamata alamvõrku VNet MultiTier-VNet marsruutimiseks tabeli "MyRouteTable".

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Määrake vaikimisi saidi jaoks sunnitud tunneling. 

    Eelmises etapis cmdlet näidisskriptid loodud nii ja seotud marsruutimiseks tabeli kaks VNet alamvõrku. Ülejäänud samm on vaikesaidi või tunneliga kohaliku saidi mitme saidi ühendused virtuaalse võrgu vahel valimine.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Täiendavad PowerShelli cmdlet-käsud

### <a name="to-delete-a-route-table"></a>Marsruutimiseks tabeli kustutamiseks

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Loendisse tabeli marsruutimiseks

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Marsruudi kustutamiseks tabelist marsruutimiseks

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Mõne alamvõrgu marsruudi eemaldamine

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Kui soovite marsruutimiseks tabeli, mis on seostatud mõne alamvõrgu loend
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Lüüsi VNet VPN vaikimisi saidi eemaldamine

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






