<properties 
   pageTitle="Marsruudib - portaali tõrkeotsing | Microsoft Azure'i"
   description="Saate teada, kuidas tõrkeotsing marsruudib Azure'i ressursihaldur juurutamise mudeli Azure'i portaalis."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Azure'i portaalis marsruudib tõrkeotsing

> [AZURE.SELECTOR]
- [Azure'i portaal](virtual-network-routes-troubleshoot-portal.md)
- [PowerShelli](virtual-network-routes-troubleshoot-powershell.md)

Kui teil on probleeme võrgu ühenduvusprobleemide abil või kaudu oma Azure-virtuaalse masina (VM), marsruudib mõjutavad oma VM liikluse puhul. Selles artiklis antakse ülevaade diagnostika võimaluste lennuliinide täpsemaks tõrkeotsinguks.

Marsruutimiseks tabelid on seostatud alamvõrku ja on kõigi võrgu liideste (NIC) selle alamvõrku tõhusad. Järgmist tüüpi marsruudib saab rakendada iga võrgu liidese:

- **Süsteemi marsruudib:** Vaikimisi on iga alamvõrgu Azure virtuaalse võrgu (VNet) loodud süsteemi marsruutimiseks tabelid, mis võimaldavad kohaliku VNet liikluse ja kohapealse liikluse kaudu VPN lüüsid Interneti-liikluse. Süsteemi marsruudib on olemas ka piilus VNets.
- **BGP marsruudib:** Olevatesse võrgu liideste kaudu ExpressRoute või -saidilt VPN-ühendused. Lisateave BGP marsruutimine [BGP koos VPN lüüside](../vpn-gateway/vpn-gateway-bgp-overview.md) ja [ExpressRoute ülevaade](../expressroute/expressroute-introduction.md) artiklite lugemine.
- **Kasutaja määratletud marsruudib (UDR):** Kui kasutate võrgu virtuaalse seadmete või on sunnitud tunneling liiklust kohapealse võrgu kaudu-saidilt VPN, võib-olla peate kasutaja määratletud marsruudib (UDRs) alamvõrgu marsruutimiseks tabeliga seotud. Kui olete tuttav UDRs, lugege artiklit [marsruudib kasutaja määratletud](virtual-networks-udr-overview.md#user-defined-routes) .

Võrgu liidese saab rakendada eri marsruudib, võib olla keeruline kindlaks määrata, millised liitväärtuse marsruudib pole tõhusad. VM võrguühendus tõrkeotsinguks saate vaadata kõiki efektiivse marsruudib võrgu kasutajaliidese Azure ressursihaldur juurutamise mudeli.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Efektiivse marsruudib abil VM liiklust tõrkeotsing

Selles artiklis kasutab näidisena järgmistel illustreerimiseks efektiivse marsruudib võrgu kasutajaliidese tõrkeotsing:

Funktsiooni VNet ühendatud VM (*VM1*) (*VNet1*, eesliide: 10.9.0.0/16) ühenduse loomiseks klõpsake äsja piilus VNet (*VNet3*, eesliite 10.10.0.0/16) on VM(VM3) nurjub. On BGP ega UDRs marsruudib VM1-NIC1 võrgu liidese ühendatud VM rakendatud, rakendatakse ainult süsteemi marsruudib.

Selles artiklis selgitatakse, kuidas määrata tegeliku marsruudib võimalus kasutamine Azure ressursihalduse juurutamise mudeli ühenduse tõrke, põhjuse.
Ajal näide kasutab ainult süsteemi marsruudib, saab kasutada samu juhiseid iga marsruutimiseks tüübi üle sissetulevate ja väljaminevate ühenduse tõrkeid määratlemiseks.

>[AZURE.NOTE] Kui teie VM on rohkem kui üks NIC, millele on manustatud, märkige ruut efektiivse marsruudib iga NICs diagnoosimine ja sealt VM võrgu ühenduvusprobleemide.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Vaate efektiivse marsruudib virtuaalse masina jaoks

Aggregate marsruudib, mis on rakendatud VM vaatamiseks tehke järgmist:

1. Veebisaidil https://portal.azure.com Azure portaali sisse logida.
2. Klõpsake nuppu **rohkem teenuseid**, siis klõpsake kuvatavas loendis **virtuaalmasinates** .
3. Valige loendist, mis kuvatakse tõrkeotsingu VM ja VM abaluuga suvandid kuvatakse.
4. Klõpsake **diagnoosimine & lahendamine** ja seejärel valige üldine probleem. Selle näite puhul **ei saa ühendust oma Windows VM** on märgitud. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Juhised kuvatakse jaotises probleem, nagu on näidatud järgmisel pildil: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klõpsake *efektiivse marsruudib* loendis soovituslikku toimingut.

6. **Efektiivse marsruudib** tera kuvatakse, nagu on näidatud järgmisel pildil:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Kui teie VM on ainult üks NIC, on see vaikimisi valitud. Kui teil on rohkem kui üks NIC, valige NIC, mille jaoks soovite vaadata efektiivse marsruudib.

    >[AZURE.NOTE] Kui seostatud NIC VM pole töötab, efektiivse marsruudib ei kuvata. Ainult esimesed 200 efektiivse marsruudib kuvatakse portaalis. Täieliku loendi leiate nuppu **Laadi alla**. Saate filtreerida täpsemaks tulemuste allalaaditud csv-failist.

    Pange tähele järgmist väljund.
    - **Allikas**: näitab marsruutimiseks tüüpi. Süsteemi marsruudib näidatakse, kui *kasutaja* ja lüüsi marsruudib kuvatakse *vaikimisi*, UDRs (staatiline või BGP) on kujutatud *VPNGateway*.
    - **Olek**: näitab efektiivse marsruutimiseks olekut. Võimalikud väärtused on *aktiivne* või *ei sobi*.
    - **AddressPrefixes**: saate määrata tegeliku marsruutimiseks aadress eesliide CIDR märke. 
    - **nextHopType**: näitab funktsiooni järgmise sõnumihüppe kohta, pärast antud protsessi. Võimalikud väärtused on *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*või *Null*. Väärtus on *Null* **nextHopType** sisse on UDR võib viidata mõne sobimatu marsruutimiseks. Näiteks kui **nextHopType** on *VirtualAppliance* ja võrgu virtuaalse seadme VM pole ette valmistatud töötab olekus. Kui **nextHopType** on *VPNGateway* ja on ette valmistatud/töötab antud VNet pole lüüsi, protsessi muutuvad kehtetuks.
    
7. On pole loetletud *WestUS-VNET3* VNet (eesliide 10.10.0.0/16): selle *WestUS-VNet1* (eesliide 10.9.0.0/16) pildil eelmises etapis protsessi. Järgmisel pildil silmitsemine link on *Ühendus katkestatud* olekus:
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Kahesuunalise link soovitud silmitsemine on katkenud, mis selgitab, miks VM1 ühendust ei VM3 *WestUS-VNet3* VNet sisse.

8. Järgmisel pildil on kujutatud lennuliini pärast kahesuunalise silmitsemine link:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Tõrkeotsingu stsenaariumid sunnitud tunneling ja marsruutimiseks hindamise lugege käesoleva artikli [kaalutlused](virtual-network-routes-troubleshoot-portal.md#Considerations) jaotis.

### <a name="view-effective-routes-for-a-network-interface"></a>Vaate efektiivse marsruudib võrgu kasutajaliidese

Kui võrguliiklust mõjutab konkreetse võrgu kasutajaliidese (NIC), saate vaadata efektiivse marsruudib täieliku loendi lisamine NIC kohta otse. Aggregate marsruudib, mis on rakendatud Võrguadapter vaatamiseks tehke järgmist:

1. Veebisaidil https://portal.azure.com Azure portaali sisse logida.
2. **Rohkem teenuseid**ja seejärel käsku **võrgu liidesed**
3. Otsige loendist soovitud NIC nimi või valige see loendist, mis kuvatakse. Selles näites on valitud **VM1-NIC1** .
4. Valige **efektiivse marsruudib** **võrgu liidese** tera, nagu on näidatud järgmisel pildil:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    **Ulatus** on vaikimisi valitud võrgu liidese.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Vaate efektiivse marsruudib marsruutimiseks tabeli jaoks

Kasutaja määratletud marsruudib (UDRs) marsruutimiseks tabeli muutmisel soovite läbi mõju, mis marsruudib, lisamisest kindla VM. Tabeli marsruutimiseks võib olla mis tahes arv alamvõrku seotud. Nüüd saab kuvada kõik efektiivse marsruudib jaoks NICs, rakendatava antud marsruutimiseks tabel, ilma vajaduseta aktiveerimine keelest antud marsruutimiseks tabeli kontekstis.

Selle näite puhul on UDR (*UDRoute*) on määratud tabelis marsruutimiseks (*UDRouteTable*). Selle protsessi saadab kõik Interneti-liikluse *Subnet1* *WestUS-VNet1* VNet, klõpsake soovitud võrgu virtuaalse seadme (Dessandi) *Subnet2* sama VNet, kuni. Järgmisel pildil on kujutatud protsessi:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Aggregate marsruudib marsruutimiseks tabeli kuvamiseks tehke järgmist:

1. Veebisaidil https://portal.azure.com Azure portaali sisse logida.
2. **Rohkem teenuseid**ja seejärel käsku **marsruutimine tabelid**
3. Otsige marsruutimiseks tabeli soovite näha liitväärtuse lennuliinide ja valige see loendist. Selles näites on valitud **UDRouteTable** . Kuvatakse valitud marsruutimiseks tabeli tera, nagu on näidatud järgmisel pildil:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Valige **Efektiivse marsruudib** **marsruutimiseks tabeli** tera. **Ulatus** on määratud marsruutimiseks tabeliga, mille valisite.
5. Mitu alamvõrku marsruutimiseks tabeli saab rakendada. Valige **alamvõrgu** soovite läbi vaadata loendist. Selles näites on valitud **Subnet1** .
6. Valige soovitud **võrgu liidese**. Kõigi ühendatud valitud alamvõrgu NICs on loetletud. Selles näites on valitud **VM1-NIC1** .

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Kui NIC ei ole käivitatud VM seostatud, kuvatakse pole tõhusad marsruudib.

## <a name="considerations"></a>Kaalutlused

Tagastatava marsruudib loendit vaadates tuleks silmas pidada paari asja.

- Marsruutimise kohta pikima eesliite Match (LPM) vahel UDRs, põhineb BGP ja süsteem. Kui seal on rohkem kui üks tee sama LPM vaste, siis marsruudi valitakse selle päritolu järgmises järjestuses:
    - Kasutaja määratletud marsruutimiseks
    - BGP marsruutimiseks
    - Süsteemi (vaikeväärtus) marsruutimiseks

    Efektiivse marsruudib, näete ainult efektiivse marsruudib, mis on kõik availble marsruudib põhjal LPM vasted. Näitab, kuidas funktsiooni marsruudib on tegelikult hinnatud antud NIC, see on palju hõlpsam teatud marsruudib, mis võivad mõjutada ühenduvust teie VM tõrkeotsing.

- Kui teil on UDRs ja saadavad liikluse on võrgu virtuaalse seadme (Dessandi) koos *VirtualAppliance* nimega **nextHopType**, tagada Dessandi, vastuvõtmine liiklus on lubatud IP ümbersuunamine või paketid kõrvaldatakse. 
- Kui sunniviisilise tunneling on lubatud, suunatakse kohapealsesse kõigi väljaminevate Interneti-liikluse. See säte, olenevalt sellest, kuidas käsitleb asutusesisese see liiklus ei pruugi RDP/SSH Interneti kaudu oma VM. 
  Sunnitud tunneling saab lubada:
    - Kui seadmine kasutaja määratletud marsruutimiseks (UDR) koos nextHopType nimega VPN-lüüsi-saidilt VPN-i abil
    - Kui vaikimisi marsruudi on piisavalt pika aja BGP
- VNet silmitsemine liikluse õigesti töötada, peab süsteemi marsruutimiseks koos **nextHopType** *VNetPeering* piilus VNet eesliite vahemiku jaoks olemas. Kui sellist lennuliini pole olemas ja kõik on korras VNet silmitsemine link:
    - Oodake paar minutit ja proovige uuesti, kui see on vastloodud silmitsemine link. Aeg-ajalt võtab rohkem kajastuma marsruudib võrgu liidesed on alamvõrgu abil.
    - Võrgu turberühma (NSG) reeglid võib mõjutavad liikluse puhul. Lisateavet leiate artiklist [Võrgu turberühmad tõrkeotsing](virtual-network-nsg-troubleshoot-portal.md) .
