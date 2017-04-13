<properties
   pageTitle="Rakendamise hübriid võrgu arhitektuur koos Azure ExpressRoute | Viide arhitektuur | Microsoft Azure'i"
   description="Kuidas rakendada turvaline-saidilt võrgu arhitektuur, mis hõlmavad on Azure virtuaalse võrgu ja kohapealse võrgu, mis ühendatud Azure'i ExpressRoute abil."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Hübriidjuurutuse võrgu arhitektuur koos Azure'i ExpressRoute rakendamine

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse head tavad ühendus luua mõne kohapealse võrgu virtuaalse võrgu Azure ExpressRoute abil. ExpressRoute ühendused on tehtud, kasutades privaatne sihtotstarbeline ühendus mõne muu ühenduvuse pakkuja kaudu. Privaatne ühenduse ulatub kohapealse võrgu Azure juurdepääsu oma IaaS infrastruktuuri Azure, avaliku lõpp-punktid PaaS teenuste ja Office365 SaaS teenuseid kasutada. Selle dokumendi keskendub võrguga ühe Azure virtuaalse (VNet) abil, mida nimetatakse privaatne silmitsemine ExpressRoute abil.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See näidis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Hübriid rakendused, kust töökoormused on jaotatud kohapealse võrgu- ja Azure vahel.

- Helistamine suuremahuliste, olulise töökoormus, mis nõuavad skaleeritavus.

- Suuremahuliste varundus ja taaste seadmete jaoks tuleb salvestada mujal läbiviidavat andmeid.

- Käsitsemise Big Data töökoormus.

- Kasutades Azure avariitaastet – saidile.

> [AZURE.NOTE] [Tehniline ülevaade ExpressRoute] [ expressroute-technical-overview] annab sissejuhatuse ExpressRoute.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm olulisemad toimingud see arhitektuur olulised komponendid.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on lehel "Hübriid võrgu - ER".

![[0]][0]

- **Azure'i virtuaalne võrkude (VNets).** Iga VNet asub ühe Azure piirkonnas ja majutada taotlus mitmetasandiline. Rakenduse astme saate segmenditud alamvõrku kasutamine iga VNet ja/või võrgu turberühmad (NSGs). 

- **Azure'i teenuste.** Need on Azure teenused, mida saab kasutada hübriid rakenduses. Need teenused on saadaval ka avaliku Interneti kaudu, kuid juurdepääs neile on ExpressRoute ringi kaudu pakub madal latentsus ja nõuaksid vähem lisatööd jõudluse, kuna liikluse ei lähe Interneti kaudu. Ühenduste kasutamine **avaliku silmitsemine**, mis kas kuuluvad teie organisatsiooni või esitatud pakkuja Ühenduvus läbi. 

- **Office 365 teenustega.** Need on avalikult kättesaadavaks Office 365 rakenduste ja teenuste Microsofti. Ühendused tehakse kasutades **Microsoft silmitsemine**, mis kas kuuluvad teie organisatsiooni või esitatud pakkuja Ühenduvus.

>[AZURE.NOTE] Saate ka luua ühenduse otse Microsoft CRM Online'i kaudu mõne Microsoft silmitsemine.

- **Kohapealse ettevõtte võrgus.** See on võrgu arvutites ja seadmetes, kuvatakse ettevõttes töötavate kohaliku piirkonna privaatvõrgu kaudu ühendatud.

- **Kohaliku serva ruuterid.** Need on ruuterid, mis kohapealse võrgu ühenduse pakkuja hallatavad ringi. Sõltuvalt sellest, kuidas teie ühendus on ette valmistatud, tuleb sisestada ruuterid kasutavad avaliku IP-aadressid. 

- **Microsoft edge ruuterid.** Need on aktiivne aktiivne tugevalt saadaval konfiguratsiooni marsruuterist. Need ruuterid lubada Ühenduvus pakkuja ühenduse oma elektriskeemide otse oma andmekeskusesse. Sõltuvalt sellest, kuidas teie ühendus on ette valmistatud, tuleb sisestada ruuterid kasutavad avaliku IP-aadressid.

- **ExpressRoute ringi.** See on 2 kiht või layer 3 ringi esitatud ühenduvuse pakkuja selle ühenduste asutusesisese võrgu Azure äärmiste kaudu. Ringi kasutab riistvara infrastruktuuri ühenduvuse pakkuja hallatavad.

- **Ühenduvus pakkujad.** Need on ettevõtted, mis pakuvad ühenduse, kasutades kas layer 2 või layer 3 Ühenduvus oma andmekeskuse ja on Azure andmekeskuse.

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme andnud mõni Azure ressursihaldur Mall viide arhitektuur soovituste järgneva installimiseks. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="connectivity-providers"></a>Ühenduvus pakkujad

Valige sobiv ExpressRoute ühenduvuse pakkuja asukoha. Teie asukohas saadaval connectivity pakkujate loendit hankimiseks järgmised Azure PowerShelli käsu kasutamine

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute ühenduvuse pakkujate ühendust oma andmekeskuse Microsoft järgmisel viisil:

- **Pilveteenuse Exchange'i koostööd asub.** Kui asute koostööd asutuses koos pilvepõhise Exchange'i, saate tellida virtuaalse rist-ühenduste Microsofti pilveteenuse kaasautorluse kohta pakkuja Ethernet Exchange'i kaudu. Kaasautorluse kohta pakkujad võivad pakkuda kiht 2 rist-ühendused või hallatavate Layer 3 rist-ühendused oma taristu saanud poole ja Microsofti pilveteenuse vahel.

- **Kakspunkt Ethernet ühendused.** Saate luua ühenduse oma kohapealse andmekeskuste/büroode Microsofti pilveteenuse kakspunkt Ethernet linkide kaudu. Kakspunkt Ethernet pakkujate pakkuda kiht 2 ühendused või hallatavate Layer 3 ühendused saidile ja Microsofti pilveteenuse vahel.

- **Kõik mis (IPVPN) võrkude.** Microsofti pilveteenuse saate oma WAN integreerida. IPVPN (tavaliselt MPLS VPN) pakuvad kõik mis Ühenduvus teie harukontorite ja andmekeskuste. Microsofti pilveteenuse võivad olla omavahel seotud oma WAN teha see välja nii nagu mis tahes muud kontoris. WAN tavaliselt pakuvad hallatavate Layer 3 Ühenduvus.

Ühenduvus pakkujate kohta leiate lisateavet teemast [ExpressRoute ahelatega ja marsruutimise domeenide][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute ringi

Veenduge, et teie asutus on vastanud [ExpressRoute eeltingimuste] [ expressroute-prereqs] ühenduse Azure'i.

Kui te pole seda juba teinud, lisage alamvõrgu, mis on nimega `GatewaySubnet` abil oma Azure'i VNet ja ExpressRoute virtuaalse võrgu lüüsi Azure'i VPN-lüüsi teenuse abil luua. Selle protsessi kohta leiate lisateavet teemast [ExpressRoute töövoogude ringi ettevalmistamise ja ringi riikide][ExpressRoute-provisioning].

Luua ka ExpressRoute ringi järgmiselt:

1. Käivitage PowerShelli järgmine käsk:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Saatke soovitud `ServiceKey` jaoks uue ringi pakkujaga.

3. Oodake, kuni pakkuja ringi ette valmistada. Järgmine PowerShelli käsu abil saate kontrollida ettevalmistamise olek on topoloogia.

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

Funktsiooni `Provisioning state` väljal soovitud `Service Provider` osas on väljund muutub `NotProvisioned` abil `Provisioned` kui ringi on valmis.

>[AZURE.NOTE]Kui kasutate layer 3 ühendus, pakkuja peaks konfigureerimine ja haldamine marsruutimine esitate pakkuja rakendamiseks vastavat marsruudib vajaliku teabe.

Kui kasutate layer 2 ühendus, tehke järgmist:

1. Kaks reserveerida/30 alamvõrku koostatud kehtiv avaliku IP-aadresside iga silmitsemine, mida soovite rakendada. Need/30 alamvõrku kasutatakse ruuterid kasutatakse ringi ette IP-aadressid. Kui te rakendavad era, avalik ja Microsoft silmitsemine, peate 6 /30 alamvõrku kehtiv avaliku IP-aadressid.     

2. Konfigureerige marsruutimine ExpressRoute ringi. Peate käivitama iga all käsk silmitsemine, mida soovite konfigureerida (era, avalik ja Microsoft).

    >[AZURE.NOTE]Vt [loomine ja nende muutmine marsruutimine on ExpressRoute ringi] [ configure-expressroute-routing] üksikasju. Marsruutimise liikluse silmitsemine võrgu lisamiseks järgmist PowerShelli käskude kasutamine

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Teine pool kehtiv avaliku IP-aadresside NAT kasutamise jaoks avaliku ja Microsoft silmitsemine reserveerida. Soovitatav on mõne muu kausta iga silmitsemine. Määrake rakenduskausta ühenduvuse pakkujale, kui nii, et need saate konfigureerida BGP reklaamimine neist vahemikest.

[Link oma isiklike VNet(s) pilves ExpressRoute ringi][link-vnet-to-expressroute]. Kasutage järgmist PowerShelli käske.

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Võtke arvesse järgmist.

- ExpressRoute kasutab vahetamiseks marsruudi teavet oma võrgu ja Azure äärise lüüsi protokolli (BGP).

- Saate ühendada mitu VNets asuvad eri piirkondadest, et sama ExpressRoute ringi seni, kuni kõik VNets ja ExpressRoute ringi on geopoliitiline piirkonna.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

ExpressRoute elektriskeemide pakuvad kõrge läbilaskevõime tee vahel. Üldiselt suurem läbilaskevõime suurem maksumus. Ehkki mõne abil saate muuta oma läbilaskevõime, veenduge, et valite algse läbilaskevõime, mis ületab teie vajadustele ja pakub ruumi kasvu. Kui teil on vaja läbilaskevõime tulevikus suurendada, on jäänud kaks võimalust.

Suurendamine ribalaiust. Pidage meeles, et kõik pakkujad võimaldavad dünaamiliselt seda teha. Ja tuleb teha nii võimalikult palju. Kuid vajadusel pärast kontrollimist teenusepakkuja juures, käivitage alltoodud käsud.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Saate suurendada ilma katkemise ribalaiust. Kasutuselevõttu ribalaiust tulemuseks häirete ühenduvuse. Teil on ringi kustutada ja see uue konfiguratsiooni uuesti luua.

Kui te kasutate Standard SKU ExpressRoute ja vaja uuendada Premium või muuta, mida olete oma hinnad kavandamine (mahupõhise või määramata), käivitage järgmised käsud. Funktsiooni `Sku.Tier` atribuudi võib olla `Standard` või `Premium`; funktsiooni `Sku.Name` atribuudi võib olla `MeteredData` või `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Veenduge, et selle `Sku.Name` atribuudi vasteid on `Sku.Tier` ja `Sku.Family`. Kui muudate pere ja teise, kuid mitte nime, keelatakse ühendust.

SKU katkestamata versiooniuuendust, kuid te ei saa vahetada piiramatu hinnakirjad kava Mahupõhised. Kui alandamine kasutatava SKU, oma läbilaskevõime tarbimine peab jääma standard SKU vaikimisi piires.

> [AZURE.NOTE] ExpressRoute pakub kahte hinnakirjad lepingute klientidele, mõõtmine või määramata andmete põhjal. Vt [ExpressRoute hinnad] [ expressroute-pricing] üksikasju. Kulude sõltuvalt ringi läbilaskevõime. Läbilaskevõime tõenäoliselt erinevad pakkuja pakkuja. Kasutage funktsiooni `Get-AzureRmExpressRouteServiceProvider` cmdlet andmemahud, et nad pakuvad ka teie regioonis saadaval pakkujate kuvamiseks.

Ühe ExpressRoute ringi toetab peerings ja VNet linkide arv. Teemast [ExpressRoute] [ expressroute-limits] lisateabe saamiseks.

Tasuta, pakub ExpressRoute Premium lisandmoodul.

- Kuni 10 000 marsruudib ringi kohta. Ilma ExpressRoute Premium lisandmoodul, on limiit 4000 marsruudib ringi kohta.

- Globaalse ühenduvuse lubamine ExpressRoute ringi, mis asub igal pool juurdepääs ressurssidele mis tahes piirkond, mitte ainult regioonide sama continent maailmas.

- Suurenemine VNet linkide kohta suuremat piiramiseks olenevalt ringi ribalaiust 10 ringi arv.

ExpressRoute topoloogia on mõeldud luba ajutine võrk puruneb kuni kaks korda läbilaskevõime piiri saate hangitud täiendavad tasuta. See on saavutada liigsete linkide kaudu. Siiski kõik ühenduvuse pakkujate toeta seda funktsiooni; Veenduge, et pakkuja Ühenduvus võimaldab selle funktsiooni enne selle sõltuvalt.

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Saate konfigureerida kõrge-saadavus Azure-ühenduse eri viisidel olenevalt sellest, saate kasutada pakkuja tüüp ja arv ExpressRoute ahelatega ja virtuaalse lüüsi võrguühenduste soovite konfigureerida. Allpool punktide võimalused kättesaadavuse Kokkuvõte

- ExpressRoute ei toeta ruuteri koondamise protokolle, näiteks HSRP ja VRRP rakendada kõrge kättesaadavus. Selle asemel kasutatakse BGP seansside kohta silmitsemine liigsete paari. Võrgu tugevalt saadaval ühenduste hõlbustamiseks Microsoft Azure'i sätted saate kaks liigsete pordid marsruuterist (Microsoft edge osa) konfigureerimisel aktiivne aktiivne.

- Kui kasutate layer 2 ühendus, juurutada liigsete ruuterite kohapealse võrgu on aktiivne aktiivne konfiguratsiooni. Mille ühenduse ühe ruuteri ja teisene ringi teise. See annab teile väga kättesaadav ühenduse kummaski otsas olevate ühendus. See on vajalik, kui teil on vaja ExpressRoute SLA. Leiate [Azure'i ExpressRoute jaoks SLA] [ sla-for-expressroute] üksikasju.

Järgmine diagramm näitab konfiguratsiooni liigsete kohapealse ruuterid on ühendatud esmaseid ja teiseseid topoloogia. Iga ringi tegeleb liikluse avaliku silmitsemine ja privaatne silmitsemine (iga silmitsemine on määratud /30 paari aadress tühikuid, nagu eelmises jaotises kirjeldatud).

![[1]][1]

Kui kasutate layer 3 ühendus, veenduge, et liigsed BGP pakub teile kättesaadavus käsitlemiseks seansid.

Virtuaalne võrkude ühendamiseks mitme ExpressRoute elektriskeemide ja iga elektriskeemide saab esitada teenusepakkujate. Selle strateegia pakub täiendavaid kõrge-saadavus ja katastroofi funktsioonid.

Seadistada VPN saitide jaoks ExpressRoute Tõrkesiirde tee. See kehtib ainult privaatne silmitsemine. Azure'i ja Office 365 teenuste, Interneti-ühendus on ainult Tõrkesiirde tee.

Vaikimisi kasutavad BGP seansid jõudeaja ajalõpu väärtusest 60 sekundi järel. Kui seanss on aegunud 3 korda, protsessi on märgitud saadaval ja ülejäänud ruuteri on kogu liikluse ümber. See 180-teise ajalõpu võib olla liiga pikk, kriitiliste rakenduste. Sellisel juhul saate muuta oma kohapealse marsruuteri BGP ajalõpu sätteid väiksem väärtus.

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Saate kasutada [Azure ühenduvuse tööriistakomplekti (AzureCT)] [ azurect] Ühenduvus teie asutusesisese andmekeskuse ja Azure jälgimiseks.

## <a name="security-considerations"></a>Turvalisuse alused

Saate konfigureerida turbesuvandid Azure-ühenduse erinevatel viisidel olenevalt teie turvakaalutlused ja nõuetele vastavus vajadustele. Allpool punktide summarize oma turbesuvandid.

ExpressRoute toimib layer 3. Ohud rakenduse kiht saab vältida, kasutades võrgu turvalisus seadet, mis piirab liikluse korralikud ressursid. Lisaks ExpressRoute ühendusi, kasutades avaliku silmitsemine saab algatada ainult kohapealse. See takistab petturitest teenuse juurdepääsu ja kompromisse kohapealsed andmed avaliku Interneti kaudu.

Turvalisus maksimeerimiseks lisada võrgu turvalisus seadmete kohapealse võrgu ja pakkuja serva ruuterid vahel. See aitab piirata volitamata liikluse saamine on VNet:

![[2]][2]

Auditi ja nõuetele vastavus eesmärgil, võib osutuda vajalikuks keelata otsest juurdepääsu komponentide käitamine on VNet Interneti kaudu ja rakendada [sunnitud tunneling][forced-tuneling]. Sellisel juhul tuleks Interneti-liikluse ümber suunata kohapealse töötab, kui seda saab auditeerida proxy piires tagasi. Puhverserveri saab konfigureerida blokeerida volitamata liikluse voolab läbi ja filtreerida potentsiaalselt ohtliku sissetulev liiklus.

![[3]][3]

Vaikimisi Azure VMs jätke andmisel login pääseda haldus – RDP ja Remote Powershell Windows VMs ja SSH Linux-põhine vms juurutatud Azure portaali kaudu kasutada lõpp-punktid. Maksimeerimine turvalisus, lubada avaliku IP-aadressi oma vms ja veenduge, et need VMs ei ole avalikult juurdepääsetava NSGs abil. VMs ainult peaks olema saadaval abil sisemise IP-aadress. Need aadressid saate teha kättesaadavaks ExpressRoute võrgu kaudu, lubada kohapealse DevOps töötajate kõik vajalikud konfiguratsiooni või hooldustööd.

Kui välise võrgu peab jätke vms halduse lõpp-punktid, kasutage NSGs ja/või kasutada juhtelemendi loendite piiramiseks on nimekiri IP-aadresside või võrkude nähtavus järgmised pordid.

## <a name="troubleshooting-considerations"></a>Kaalutlused tõrkeotsing

Varem toimiva ExpressRoute ringi nurjumisel nüüd ühenduse puudumisel mis tahes konfiguratsiooni muudatuste kohapealse või teie isiklike VNet peate ühenduvuse pakkuja poole pöördumine ja nendega töötada probleemi lahendamiseks. Saate järgmisi Azure PowerShelli käske piiratud kontrollimine ja aitab määratleda, kus probleemid võivad:

- Veenduge, et ringi on ette valmistatud.

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Selle käsu väljund kuvatakse mitu atribuutide oma ringi, sh `ProvisioningState`, `CircuitProvisioningState`, ja `ServiceProviderProvisioningState` nagu allpool näidatud.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Kui soovitud `ProvisioningState` on seatud `Succeeded` pärast seda, kui proovisite luua uue ringi, ringi eemaldada käsuga allpool ja proovige uuesti luua.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Kui pakkuja oli juba ette valmistatud ringi, ja `ProvisioningState` on seatud `Failed`, või `CircuitProvisioningState` pole `Enabled`, pöörduge abi saamiseks oma pakkuja.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Kui teil on juba konfigureeritud sobiva võrgu seadme olemasoleva kohapealse infrastruktuuri, saate järgmiste juhiste järgi juurutada arhitektuur viide:

Kui teil on juba konfigureeritud VPN seadme olemasoleva kohapealse infrastruktuuri, saate järgmiste juhiste järgi juurutada arhitektuur viide:

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Oodake, kuni link peaks avama Azure'i portaalis, siis tehke järgmist: 
  - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-hybrid-er-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutuse lõpuleviimiseks.

4. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Oodake, kuni link peaks avama Azure'i portaalis, siis tehke järgmist: 
  - Valige jaotises **ressursirühm** **kasutada olemasolevaid** ja sisestage `ra-hybrid-er-rg` tekstiväljale.
  - Valige piirkonnale **asukoht** rippmenüüst.
  - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
  - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
  - Klõpsake nuppu **osta** .

6. Oodake, kuni juurutuse lõpuleviimiseks.

## <a name="next-steps"></a>Järgmised sammud

- Vt [rakendamise tugevalt saadaval hübriid võrgu arhitektuur] [ highly-available-network-architecture] teavet võrgu hü kättesaadavamaks põhjal ExpressRoute võtnud üle VPN-ühendus.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Azure'i ExpressRoute kasutamine hü võrgu arhitektuur"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Liigsete ruuterid funktsiooniga ExpressRoute esmaseid ja teiseseid topoloogia"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Kohapealse võrgu seadmeid lisamine"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Abil sunnitud tunneling auditeeritavad Internet seotud liikluse"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Asukoha ServiceKey ExpressRoute topoloogia"
