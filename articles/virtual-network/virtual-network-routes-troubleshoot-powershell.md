<properties 
   pageTitle="Marsruudib - PowerShelli tõrkeotsing | Microsoft Azure'i"
   description="Saate teada, kuidas tõrkeotsing marsruudib Azure'i ressursihaldur juurutamise mudeli Azure PowerShelli kaudu."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
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
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-azure-powershell"></a>Marsruudib Azure PowerShelli kaudu tõrkeotsing

> [AZURE.SELECTOR]
- [Azure'i portaal](virtual-network-routes-troubleshoot-portal.md)
- [PowerShelli](virtual-network-routes-troubleshoot-powershell.md)

Kui teil on probleeme võrgu ühenduvusprobleemide abil või kaudu oma Azure-virtuaalse masina (VM), marsruudib mõjutavad oma VM liikluse puhul. Selles artiklis antakse ülevaade diagnostika võimaluste lennuliinide täpsemaks tõrkeotsinguks.

Marsruutimiseks tabelid on seostatud alamvõrku ja on kõigi võrgu liideste (NIC) selle alamvõrku tõhusad. Järgmist tüüpi marsruudib saab rakendada iga võrgu liidese:

- **Süsteemi marsruudib:** Vaikimisi on iga alamvõrgu Azure virtuaalse võrgu (VNet) loodud süsteemi marsruutimiseks tabelid, mis võimaldavad kohaliku VNet liikluse ja kohapealse liikluse kaudu VPN lüüsid Interneti-liikluse. Süsteemi marsruudib on olemas ka piilus VNets.
- **BGP marsruudib:** Olevatesse võrgu liideste kaudu ExpressRoute või -saidilt VPN-ühendused. Lisateave BGP marsruutimine [BGP koos VPN lüüside](../vpn-gateway/vpn-gateway-bgp-overview.md) ja [ExpressRoute ülevaade](../expressroute/expressroute-introduction.md) artiklite lugemine.
- **Kasutaja määratletud marsruudib (UDR):** Kui kasutate võrgu virtuaalse seadmete või on sunnitud tunneling liikluse kohapealse võrgu kaudu-saidilt VPN, võib-olla peate kasutaja määratletud marsruudib (UDRs) alamvõrgu marsruutimiseks tabeliga seotud. Kui olete tuttav UDRs, lugege artiklit [marsruudib kasutaja määratletud](virtual-networks-udr-overview.md#user-defined-routes) .

Võrgu liidese saab rakendada eri marsruudib, võib olla keeruline kindlaks määrata, millised liitväärtuse marsruudib pole tõhusad. VM võrguühendus tõrkeotsinguks saate vaadata kõiki efektiivse marsruudib võrgu kasutajaliidese Azure ressursihaldur juurutamise mudeli.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Efektiivse marsruudib abil VM liiklust tõrkeotsing

Selles artiklis kasutab näidisena järgmistel illustreerimiseks efektiivse marsruudib võrgu kasutajaliidese tõrkeotsing:

Funktsiooni VNet ühendatud VM (*VM1*) (*VNet1*, eesliide: 10.9.0.0/16) ühenduse loomiseks klõpsake äsja piilus VNet (*VNet3*, eesliite 10.10.0.0/16) on VM(VM3) nurjub. On BGP ega UDRs marsruudib VM1-NIC1 võrgu liidese ühendatud VM rakendatud, rakendatakse ainult süsteemi marsruudib.

Selles artiklis selgitatakse, kuidas määrata tegeliku marsruudib võimalus kasutamine Azure ressursihalduse juurutamise mudeli ühenduse tõrke, põhjuse.
Ajal näide kasutab ainult süsteemi marsruudib, saab kasutada samu juhiseid iga marsruutimiseks tüübi üle sissetulevate ja väljaminevate ühenduse tõrkeid määratlemiseks.

>[AZURE.NOTE] Kui teie VM on rohkem kui üks NIC, millele on manustatud, märkige ruut efektiivse marsruudib iga NICs diagnoosimine ja sealt VM võrgu ühenduvusprobleemide.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Vaate efektiivse marsruudib virtuaalse masina jaoks

Aggregate marsruudib, mis on rakendatud VM vaatamiseks tehke järgmist:

### <a name="view-effective-routes-for-a-network-interface"></a>Vaate efektiivse marsruudib võrgu kasutajaliidese

Aggregate marsruudib, mis on rakendatud võrgu liidese vaatamiseks tehke järgmist:

1.  Azure'i sisse logida ja Azure PowerShelli seansi käivitamine Kui olete tuttav Azure PowerShelli, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) artiklist.

2.  Järgmine käsk tagastab kõik marsruudib võrgu liidese, nimega *VM1-NIC1* ressursirühm *RG1*rakendatud.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Kui te ei tea võrgu liidese nime, tippige järgmine käsk tuua kõigi võrgu liideste ressursi group.* nimed

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    Järgmine väljund näeb välja väljundi iga protsessi rakendatud NIC on ühendatud alamvõrgu jaoks:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Pange tähele järgmist väljund.
    - **Nimi**: tõhus protsessi nimi võib olla tühi, kui järel konkreetselt nimetatud marsruudib kasutaja määratletud. 
    - **Olek**: näitab efektiivse marsruutimiseks olekut. Võimalikud väärtused on "Aktiivne" või "Kehtetu"
    - **AddressPrefixes**: saate määrata tegeliku marsruutimiseks aadress eesliide CIDR märke. 
    - **nextHopType**: näitab funktsiooni järgmise sõnumihüppe kohta, pärast antud protsessi. Võimalikud väärtused on *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*või *Null*. Väärtus on *Null* **nextHopType** sisse on UDR võib viidata mõne sobimatu marsruutimiseks. Näiteks kui **nextHopType** on *VirtualAppliance* ja võrgu virtuaalse seadme VM pole ette valmistatud töötab olekus. Kui **nextHopType** on *VPNGateway* ja on ette valmistatud/töötab antud VNet pole lüüsi, protsessi muutuvad kehtetuks.
    - **NextHopIpAddress**: saate määrata selle järgmise sõnumihüppe kohta, pärast efektiivse marsruudi IP-aadress.
    
    Järgmine käsk tagastab selle marsruudib lihtsam tabeli kuvamiseks:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    Järgmised väljund on osa väljund saanud eelnevalt kirjeldatud stsenaariumi:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. On pole loetletud on *WestUS-VNet3* VNet (eesliide 10.10.0.0/16)* * kaudu *WestUS-VNet1* (eesliide 10.9.0.0/16) väljund eelmises juhises protsessi. Nagu on näidatud järgmisel pildil, *WestUS-VNet3* VNet VNet silmitsemine seos on *Ühendus katkestatud* olekus.
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Kahesuunalise link soovitud silmitsemine on katkenud, mis selgitab, miks VM1 ühendust ei VM3 *WestUS-VNet3* VNet sisse. Installiprogramm kahesuunalise VNet silmitsemine link *WestUS-VNet1* ja *WestUS-VNet3* VNets. Pärast VNet silmitsemine link on loodud õigesti tagastatud väljund järgmiselt:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Kui olete kindlaks teinud probleeme, saate lisada, eemaldada, muuta marsruudib ja marsruutimine tabelid. Tippige järgmine käsk selleks kasutatavate käskude loendi kuvamiseks:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Kaalutlused

Tagastatava marsruudib loendit vaadates tuleks silmas pidada paari asja.

- Marsruutimise kohta pikima eesliite Match (LPM) vahel UDRs, põhineb BGP ja süsteem. Kui seal on rohkem kui üks tee sama LPM vaste, siis marsruudi valitakse selle päritolu järgmises järjestuses:
    - Kasutaja määratletud marsruutimiseks
    - BGP marsruutimiseks
    - Süsteemi (vaikeväärtus) marsruutimiseks

    Efektiivse marsruudib, näete ainult efektiivse marsruudib, mis on kõik availble marsruudib põhjal LPM vasted. Näitab, kuidas funktsiooni marsruudib on tegelikult hinnatud antud NIC, see on palju hõlpsam teatud marsruudib, mis võivad mõjutada ühenduvust teie VM tõrkeotsing.

- Kui teil on UDRs ja saadavad liikluse on võrgu virtuaalse seadme (Dessandi) koos *VirtualAppliance* nimega **nextHopType**, tagada Dessandi, vastuvõtmine liiklus on lubatud IP ümbersuunamine või paketid kõrvaldatakse. 
- Kui sunniviisilise tunneling on lubatud, suunatakse kohapealsesse kõigi väljaminevate Interneti-liikluse. RDP/SSH Interneti kaudu oma VM ei pruugi see säte, olenevalt sellest, kuidas käsitleb asutusesisese selle liiklust. 
  Sunnitud tunneling saab lubada:
    - Kui seadmine kasutaja määratletud marsruutimiseks (UDR) koos nextHopType nimega VPN-lüüsi-saidilt VPN-i abil
    - Kui vaikimisi marsruudi on piisavalt pika aja BGP
- VNet silmitsemine liikluse õigesti töötada, peab süsteemi marsruutimiseks koos **nextHopType** *VNetPeering* piilus VNet eesliite vahemiku jaoks olemas. Kui pole sellist lennuliini ja VNet silmitsemine link on korras.
    - Oodake paar minutit ja proovige uuesti, kui see on vastloodud silmitsemine link. Aeg-ajalt võtab rohkem kajastuma marsruudib võrgu liidesed on alamvõrgu abil.
    - Võrgu turberühma (NSG) reeglid võib mõjutavad liikluse puhul. Lisateavet leiate artiklist [Võrgu turberühmad tõrkeotsing](virtual-network-nsg-troubleshoot-powershell.md) .
