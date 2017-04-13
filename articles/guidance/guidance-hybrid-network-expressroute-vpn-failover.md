<properties
   pageTitle="Väga kättesaadav hübriid võrgu arhitektuur rakendamise | Microsoft Azure'i"
   description="Kuidas rakendada turvaline-saidilt võrgu ülesehituse, mis hõlmavad on Azure virtuaalse võrgu ja kohapealse võrgu, mis ühendatud VPN lüüsi Tõrkesiirde ExpressRoute abil."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Rakendamise tugevalt saadaval hübriid võrgu arhitektuur

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad on kohapealse võrgu ühenduse virtuaalse võrgu Azure-saidilt virtuaalse privaatvõrgu (VPN) ühenduse Tõrkesiirde ExpressRoute, kasutades. Liiklus liigub sujuvalt kohapealse võrgu ja muu Azure virtuaalse kaudu (VNet) ExpressRoute-ühenduse kaudu.  Rakenduses ExpressRoute ringi katkemise korral suunatakse liikluse läbi mõne IPSec VPN tunneliga.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See näidis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Hübriid rakendused, kust töökoormused on jaotatud kohapealse võrgu- ja Azure vahel.

- Helistamine suuremahuliste, olulise töökoormus, mis nõuavad skaleeritavus.

- Suuremahuliste varundus ja taaste seadmete jaoks tuleb salvestada mujal läbiviidavat andmeid.

- Käsitsemise Big Data töökoormus.

- Kasutades Azure avariitaastet – saidile.

Pange tähele, et kui ExpressRoute ringi pole saadaval, kuvatakse VPN marsruutimiseks ainult hakkama privaatne silmitsemine ühendused. Silmitsemine ja Microsoft silmitsemine ühendused läheb Interneti kaudu.

## <a name="architecture-diagram"></a>Arhitektuur skeem

>[AZURE.NOTE] [Azure'i VPN-lüüsi teenuse] [ azure-vpn-gateway] rakendab kahte tüüpi virtuaalse võrgu lüüside; VPN virtuaalse võrgu lüüside ja ExpressRoute virtuaalse võrgu lüüsid. Selles dokumendis mõiste *VPN-lüüsi* viitab Azure'i teenuse fraasidest *VPN virtuaalse võrgu lüüsi* ja *ExpressRoute virtuaalse võrgu lüüsi* kasutatakse vastavalt VPN ja ExpressRoute rakendusi, lüüsi viidata.

Järgmine diagramm olulisemad toimingud see arhitektuur olulised komponendid.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "hübriid võrgu - ER-VPN" leht.

![[0]][0]

- **Azure'i virtuaalne võrkude (VNets).** Iga VNet asub ühe Azure piirkonnas ja majutada taotlus mitmetasandiline. Rakenduse astme saate segmenditud alamvõrku kasutamine iga VNet ja/või võrgu turberühmad (NSGs).

- **Kohapealse ettevõtte võrgus.** See on võrgu arvutites ja seadmetes, kuvatakse ettevõttes töötavate kohaliku piirkonna privaatvõrgu kaudu ühendatud.

- **VPN-seade.** See on seadme või teenus, mis sisaldab välise ühenduvuse kohapealse võrku. VPN-seade võib olla riistvara või see võib olla näiteks marsruutimine ja Remote Access Service (RRAS) Windows Server 2012 tarkvara lahendus.

> [AZURE.NOTE] Loendi toetatud VPN-ja teavet valitud VPN seadmete ühenduse Azure'i VPN-lüüsi konfigureerimise kohta leiate teemast juhised soovitud seade [ei toeta Azure VPN seadmete loendi][vpn-appliance].

- **VPN virtuaalse võrgu lüüsi.** VPN virtuaalse võrgu lüüsi võimaldab VNet ühenduse loomiseks kohapealse võrgu VPN seadme. VPN virtuaalse võrgu lüüsi on konfigureeritud aktsepteerige kohapealse võrgu ainult VPN seadme kaudu. Lisateavet leiate teemast [ühenduse loomine mõne kohapealse võrgu loomine Microsoft Azure virtuaalse][connect-to-an-Azure-vnet].

- **ExpressRoute virtuaalse võrgu lüüsi.** ExpressRoute virtuaalse võrgu lüüsi võimaldab VNet ühenduse loomiseks kohapealse võrgu Ühenduvus kasutatakse ExpressRoute ringi.

- **Lüüsi alamvõrgu.** Virtuaalse võrgu lüüside toimuvad sama alamvõrgu.

- **VPN-ühendus.** Ühendus on atribuudid, mis määravad ühenduse tüüp (IPSec) ja kohapealse VPN seadme ühiskasutusse klahvi liikluse krüptimiseks.

- **ExpressRoute ringi.** See on 2 kiht või layer 3 ringi esitatud ühenduvuse pakkuja selle ühenduste asutusesisese võrgu Azure äärmiste kaudu. Ringi kasutab riistvara infrastruktuuri ühenduvuse pakkuja hallatavad.

- **N-astme pilve rakendus.** See on majutatud Azure rakendus. See võib sisaldada mitmetasandiline, mitu alamvõrku Azure koormus soolise kaudu ühendatud abil. Iga alamvõrgu liiklus võib-olla maksma reeglid, mis on määratletud [Azure'i võrgu turberühmad]abil[azure-network-security-group](NSGs). Lisateabe saamiseks vaadake teemat [Alustamine Microsoft Azure'i turvalisusega][getting-started-with-azure-security].

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme pakkunud viide arhitektuur soovituste järgneva installimiseks on Azure ressursihaldur malli. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="vnet-and-gatewaysubnet"></a>VNet ja GatewaySubnet

Sama VNet ExpressRoute virtuaalse võrgu lüüsi ja VPN virtuaalse võrgu lüüsi loomine See tähendab, et nad jagavad sama nimega **GatewaySubnet** alamvõrgu

Kui soovitud VNet juba sisaldab mõnda alamvõrgu nimega **GatewaySubnet**, tagada, et see on mõne /27 või suurem aadressiruumi. Kui olemasoleva alamvõrgu on liiga väike, eemaldage see järgmiselt ja looge uus, nagu on näidatud järgmise täpi.

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Kui soovitud VNet ei sisalda soovitud nimega **GatewaySubnet**alamvõrku, siis looge uus järgmiselt:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN-i ja ExpressRoute lüüsid

Veenduge, et teie asutus vastab [ExpressRoute eeltingimuste] [ expressroute-prereq] ühenduse Azure'i.

Kui teil on juba teie Azure'i VNet VPN virtuaalse võrgu lüüsi, selle eemaldada, nagu allpool näidatud.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Järgige rakendamisel [hübriid võrgu ülesehituse koos Azure'i ExpressRoute] [ implementing-expressroute] oma ExpressRoute ühendust luua.

Järgige juhiseid teemas [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN] [ implementing-vpn] oma VPN virtuaalne lüüsi võrguühenduse.

Pärast seda, kui olete loonud virtuaalse võrguühenduste lüüsi, testige keskkonna järgmiselt:

1. Veenduge, et saate luua ühenduse kohapealse võrgu kaudu oma Azure'i VNet.

2. Pöörduge teenusepakkuja ExpressRoute ühenduvuse kontrollimise lõpetada.

3. Veenduge, et te saate luua ühenduse kohapealse võrgu kaudu oma Azure'i VNet abil virtuaalse võrgu lüüsi VPN-ühendus.

4. Võtke ühendust oma pakkujat reestabilish ExpressRoute Ühenduvus.

## <a name="considerations"></a>Kaalutlused

ExpressRoute peaksite arvesse võtma, vaadake [hübriidjuurutuse võrgu arhitektuur koos Azure'i ExpressRoute rakendamise] [ guidance-expressroute] juhised.

Saidi saidi VPN peaksite arvesse võtma, leiate teemast [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN] [ guidance-vpn] juhised.

Üldine Azure turvakaalutlused, leiate [Microsofti pilveteenustega ja võrgu turvalisuse][best-practices-security].

## <a name="solution-deployment"></a>Lahenduse juurutamine

Kui teil on juba konfigureeritud sobiva võrgu seadme olemasoleva kohapealse infrastruktuuri, saate järgmiste juhiste järgi juurutada arhitektuur viide:

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Oodake, kuni link peaks avama Azure'i portaalis, siis tehke järgmist: 
  - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-hybrid-vpn-er-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutuse lõpuleviimiseks.

4. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Oodake, kuni lingi avamine Azure'i portaalis, seejärel sisestage siis tehke järgmist: 
  - Valige jaotises **ressursirühm** **kasutada olemasolevaid** ja sisestage `ra-hybrid-vpn-er-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Väga kättesaadav hübriid võrgu arhitektuur abil ExpressRoute ja VPN lüüsi arhitektuur"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
