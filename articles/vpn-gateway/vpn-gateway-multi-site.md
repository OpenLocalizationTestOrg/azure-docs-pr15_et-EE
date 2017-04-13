<properties 
   pageTitle="Virtuaalse ühendada mitu saitide VPN-lüüsi ja PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis juhendab teid mitme kohalik kohapealse saidi VPN-lüüsi kasutamise klassikaline juurutamise mudeli virtuaalse võrguga ühenduse loomine."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>On VNet koos olemasoleva ühenduse lüüs VPN-saidilt ühenduse lisamine

> [AZURE.SELECTOR]
- [Ressursihaldur - portaal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassikaline - PowerShelli](vpn-gateway-multi-site.md)

Selles artiklis tutvustatakse lisada saidi saidi (S2S) ühendused VPN-lüüsi, mis sisaldab mõne olemasoleva ühenduse PowerShelli kaudu. Seda tüüpi ühendus on sageli edaspidi "mitme sait" konfigureerimine. 

See artikkel kehtib virtuaalne võrkude loodud klassikaline juurutamise mudeli (tuntud ka kui teenuste haldamine). ExpressRoute /-saidilt kooseksisteerimisele ühenduse konfiguratsioone ei rakendata järgmisi juhiseid. Kohta leiate teavet [ExpressRoute/S2S kooseksisteerimisele ühendused](../expressroute/expressroute-howto-coexist-classic.md) kooseksisteerimisele ühendused.

### <a name="deployment-models-and-methods"></a>Juurutamise mudelid ja meetodid

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Uuendame selle tabeli nimega uue artikleid ja täiendavad Tabeliriistad selle konfiguratsiooni puhul. Kui artikkel on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Ühenduse loomise kohta

Saate luua ühenduse mitmesse kohapealse ühe virtuaalse võrgu. See on eriti atraktiivseks hoone hübriid pilve lahendusi. Mitme saidi Azure virtuaalse võrgu lüüsi ühenduse loomise on väga sarnane muude saitide ühenduste loomine. Tegelikult saate kasutada olemasolevaid Azure'i VPN-lüüsi, kui lüüs on dünaamiline (marsruutimiseks-põhine).

Kui teil juba on staatiline lüüsi virtuaalse võrku ühendatud, saate muuta lüüsi tüüp dünaamiliseks ilma, et majutada mitme saidi virtuaalse võrgu taastada. Enne marsruutimise tüübi muutmine veenduge, et teie asutusesisese VPN-lüüsi toetab marsruutimiseks vastavalt VPN-i konfiguratsioone. 

![mitme saidi skeem] (./media/vpn-gateway-multi-site/multisite.png "mitme saidi")

## <a name="points-to-consider"></a>Punktide silmas pidada?

**Te ei saa Azure klassikaline portaali abil saate muuta selle virtuaalse võrku.** Selles versioonis, peate muudatuste tegemiseks võrgu konfiguratsioonifail asemel Azure'i klassikaline portaalis. Kui muudate Azure'i klassikaline portaalis kuvatakse kirjutate üle mitme saidi viide sätete virtuaalse võrku. 

Mida peaks mugav päris võrgu konfigureerimise faili abil mitme saidi toimingu lõpetamist aeg. Kui teil on mitu inimesed töötavad teie võrgu konfigureerimise, peate veenduge, et igaüks teab seda piirangut. See ei tähenda, et ei saa kasutada Azure klassikaline portaali üldse. Saate kõik muu peale selle kindla virtuaalse võrguga konfiguratsioonimuudatuste tegemist.

## <a name="before-you-begin"></a>Enne alustamist

Enne alustamist konfiguratsiooni, veenduge, et teil on järgmine:

- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).

- Ühilduvad VPN riistvara iga asutusesisese asukoht. Märkige ruut [Kohta VPN seadmete virtuaalse võrguühendus](vpn-gateway-about-vpn-devices.md) kontrollimaks, kas seade, mida soovite kasutada on midagi teadaolevalt ühilduvad.

- Väliselt suunatud avaliku IPv4 IP-aadressi iga VPN-seade. IP-aadressi ei leita taga mõnda NAT. See on nõue.

- Peate Azure PowerShelli cmdletid uusima versiooni installimiseks. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

- Keegi, kes on vilunud veebisaidil riistvara VPN-i konfigureerimine. Te ei saa automaatselt loodud VPN skriptide Azure klassikaline portaali abil saate konfigureerida VPN-seadmetes. See tähendab, et peate keerukas mõista, kuidas konfigureerida seadme VPN või kellegagi, kes ei tööta.

- IP-aadressid mida soovite kasutada virtuaalse võrgu (kui te ei ole juba loonud). 

- IP-aadressid iga kohtvõrgu saite, et teil tuleb ühenduse. Peate veenduge, et iga kohalikus võrgus saidid, mida soovite ühendada ei kattuks vahemikud IP-aadress. Muul juhul kõnest üleslaaditava konfiguratsiooni Azure klassikaline portaali või REST API-ga. 

    Näiteks, kui teil on kaks kohtvõrgu saite, et mõlemad sisaldavad IP-aadress vahemikus 10.2.3.0/24 ja teil on pakett Sihtaadress 10.2.3.3Laevade, Azure ei tea, millist saiti soovite saata pakett, kuna aadresside vahemikud on kattuvad. Marsruutimise probleemide vältimiseks Azure'i ei luba teil laadida konfiguratsioonifail, mis sisaldab kattuvaid vahemikke.



## <a name="1-create-a-site-to-site-vpn"></a>1. VPN saitide loomine

Kui teil on juba dünaamilise marsruutimise lüüsi, VPN saidilt tore! Pidage eksportida [virtuaalse võrgu sätete konfigureerimine](#export). Kui ei, siis tehke järgmist:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Kui teil on juba-saidilt virtuaalse võrgus, kuid see on staatiline (poliitika-põhine) marsruutimise lüüsi:

1. Dünaamiline marsruutimine muuta oma lüüsi tüüp. Mitme saidi VPN nõuab dünaamiline (tuntud ka kui marsruutimiseks-põhine) marsruutimise lüüsi. Teie lüüsi tüüp muutmiseks peate esmalt olemasoleva lüüsi kustutamine ja seejärel looge uus. Juhiste saamiseks vaadake, [Kuidas muuta oma lüüsi VPN marsruutimise tüüpi](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Teie uus lüüsi konfigureerimine ja looge oma VPN tunneliga. Juhised leiate teemast [konfigureerimine VPN-lüüsi Azure'i klassikaline portaalis](vpn-gateway-configure-vpn-gateway-mp.md). Dünaamiline marsruutimine esmalt muuta oma lüüsi tüüp. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Kui teil pole-saidilt virtuaalse võrgu:

1. Neid juhiseid kasutades virtuaalse võrgu saitide loomine: [virtuaalse võrgu-saidilt VPN-ühenduse Azure klassikaline portaali loomine](vpn-gateway-site-to-site-create.md).  

2. Neid juhiseid dünaamilise marsruutimise lüüsi konfigureerimine: [VPN-lüüsi konfigureerimine](vpn-gateway-configure-vpn-gateway-mp.md). Valige oma lüüsi tüüp **dünaamiline marsruutimine** kindlasti.

## <a name="export"></a>2. võrgu konfigureerimise faili eksportimine 

Oma võrgu konfigureerimise faili eksportida. Saate eksportida faili kasutatakse uue mitme saidi sätete konfigureerimiseks. Kui vajate juhiseid, kuidas eksportida faili, vt artiklist: [Kuidas luua mõne VNet võrgu konfigureerimise faili Azure portaali kaudu](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. võrgu konfigureerimise faili avamine

Avage allalaaditud võrgu konfigureerimise faili viimases etapis. Mis tahes XML-i redaktoris, mida soovite kasutada. Faili peaks välja nägema umbes järgmine:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. lisamine mitme saidi viited

Kui lisate või eemaldate saidi teavet, tuleb teil ConnectionsToLocalNetwork/LocalNetworkSiteRef konfiguratsiooni muudatusi teha. Uue kohaliku saidi viide päästikute Azure'i luua uue tunneliga lisamine. Järgmises näites on võrgu konfigureerimise ühenduse ühe – saidile. Kui olete muudatuste tegemise lõpetanud, salvestage fail.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. võrgu konfigureerimise faili importimine

Võrgu konfigureerimise faili importimine. Muudatused koos selle faili importimisel lisatakse uus tunnelid. Tunnelid kasutatakse dünaamilise lüüsi varem loodud. Kui teil on vaja juhised, kuidas faili importida, vt artiklist: [Kuidas luua mõne VNet võrgu konfigureerimise faili Azure portaali kaudu](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. laadida võtmed

Kui olete lisanud oma uue tunnelid, kasutage PowerShelli cmdletti `Get-AzureVNetGatewayKey` saada iga tunneliga IPsec/IKE eelnevalt jagatud võtmed.

Näiteks:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Soovi korral saate kasutada ka *Saada virtuaalse võrgu jagatud Lüüsivõtit* REST API saada eelnevalt jagatud võtmed.

## <a name="7-verify-your-connections"></a>7. Kontrollige ühendusi

Mitme saidi tunneliga olekut saate kontrollida. Pärast allalaadimist iga tunneliga klahvid, peaksite kinnitamiseks ühendused. Kasutage `Get-AzureVnetConnection` saada loendi virtuaalse võrgu tunnelid, nagu on näidatud järgmises näites. VNet1 on selle VNet nimi.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Järgmised sammud

VPN lüüside kohta leiate lisateavet teemast [VPN lüüsid](../vpn-gateway/vpn-gateway-about-vpngateways.md).

