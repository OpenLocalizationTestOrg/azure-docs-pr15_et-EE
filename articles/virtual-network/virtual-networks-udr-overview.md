<properties 
   pageTitle="Mis on kasutaja määratletud marsruudib ja IP edastamine?"
   description="Saate teada, kuidas kasutada kasutaja määratletud marsruudib (UDR) ja IP edastamine edasi liikluse võrku Azure virtuaalne seadmed."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Mis on kasutaja määratletud marsruudib ja IP edastamine?
Virtuaalne võrku (VNet) Azure'i virtuaalmasinates (VM) lisamisel te teate, et VMs on võimalik omavahel suhelda üle võrgu automaatselt. Pole vaja määrata lüüsi, ehkki VMs on teise alamvõrku. Sama kehtib VMs teatis avaliku Interneti ja isegi oma kohapealse võrgu kui hübriid ühendus Azure'i oma andmekeskuse on Esita.

See teatis vool on võimalik, kuna Azure kasutab süsteemi marsruudib sarja, et määrata, kuidas IP liikluse liigub sujuvalt. Süsteemi marsruudib kontrollida, side järgmistel juhtudel:

- Kaudu sama alamvõrgu sees.
- Kaudu mõne teise on VNet alamvõrgu.
- Kaudu VMs Interneti-ühendus.
- Kaudu on VNet, et teise VNet VPN lüüsi kaudu.
- Kaudu on VNet oma kohapealse võrgu kaudu VPN-lüüsi.

Joonisel on lihtne häälestus on VNet, kaks alamvõrku ja mõned VMs koos süsteemi marsruudib, mis võimaldavad IP-liikluse voog.

![Süsteemi marsruudib Azure](./media/virtual-networks-udr-overview/Figure1.png)

Kuigi süsteemi marsruudib kasutamine hõlbustab liikluse automaatselt juurutamiseks, on juhtumeid, kus soovite reguleerida paketid marsruutimine virtuaalse seadme kaudu. Nii loomisega kasutaja määratletud marsruudib, mida saate selle järgmise sõnumihüppe kohta, pärast pakettide minevaid teatud alamvõrgu avamiseks oma virtuaalse seadme hoopis ja lubada IP ümbersuunamine töötab virtuaalse seadme VM.

Joonis kujutab näidet kasutaja määratletud marsruudib ja IP ümbersuunamine jõustamine paketid saadetakse ühe alamvõrgu teisest läbida virtuaalse seadme kolmanda alamvõrgu.

![Süsteemi marsruudib Azure](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Kasutaja määratletud marsruudib rakendatakse ainult liiklust jättes mõne alamvõrgu. marsruudib, et määrata, kuidas liiklust tuleb on alamvõrgu Internetist, näiteks ei saa luua. Lisaks edastate liikluse seadme ei tohi olla sama alamvõrgu, kus liiklus pärineb. Alati luua eraldi alamvõrgu oma seadmete jaoks. 

## <a name="route-resource"></a>Marsruutimiseks ressurss
Paketid marsruuditakse marsruutimiseks tabelil põhineva määratletud iga sõlme füüsilise võrgu TCP/IP võrgus. Tabeli marsruutimiseks on eraldi lennuliinide otsustada, kuhu suunata paketid põhjal sihtkoha IP-aadressi kasutatakse kogum. Protsess koosneb järgmistest:

|Atribuut|Kirjeldus|Piiranguid|Kaalutlused|
|---|---|---|---|
| Aadress eesliide | Sihtkoha CIDR, millele kehtib protsessi, nt 10.1.0.0/16.|Peab olema lubatud CIDR vahemiku tähistav aadressid avaliku Interneti-, Azure virtuaalse võrgu-või asutusesisese andmekeskuse.|Veenduge, et **aadress eesliide** ei sisalda aadressi **järgmise sõnumihüppe kohta, pärast aadressi**, muidu lähtekoha kavatse funktsiooni järgmise sõnumihüppe kohta, pärast ilma kunagi sihtkohta jõudmine esitatavaks sisestab oma paketid. |
| Järgmise sõnumihüppe kohta, pärast tüüp | Azure'i sõnumihüppe kohta, pärast tüüpi paketi tuleks saata. | Peab olema üks järgmistest väärtustest: <br/> **Virtuaalse võrgu**. Tähistab virtuaalse kohtvõrgu. Näiteks, kui teil on kaks alamvõrku, 10.1.0.0/16 ja 10.2.0.0/16 virtuaalse samasse võrku, lennuliini iga alamvõrgu marsruudi tabelis on *Virtuaalse*võrgu järgmise sõnumihüppe kohta, pärast väärtus. <br/> **Virtuaalse võrgu lüüsi**. Azure'i S2S VPN-lüüsi tähistab. <br/> **Internet**. Tähistab vaikimisi Interneti-lüüsi esitatud Azure infrastruktuuri. <br/> **Virtuaalse seadme**. Tähistab virtuaalse seadme, võrgu Azure virtuaalse lisatud. <br/> **Mitte keegi**. Tähistab must Ava. Üldse ei edastata paketid edastatud must Ava.| Kaaluge **puudub** tüüp peatamiseks paketid voolamist antud sihtkohta. | 
| Järgmise sõnumihüppe kohta, pärast aadress | Järgmise sõnumihüppe kohta, pärast aadress sisaldab paketid edasisaadetava IP-aadress. Järgmise sõnumihüppe kohta, pärast väärtused on lubatud ainult marsruudib, kus järgmise sõnumihüppe kohta, pärast tüüp on *Virtuaalse seadme*.| IP-aadress, mis on kättesaadav, kui kasutaja määratletud marsruutimiseks rakendatakse virtuaalse võrgustikus peab olema. | Kui IP-aadressi tähistab VM, veenduge, et lubate [IP edastamine](#IP-forwarding) Azure VM. |

Azure'i PowerShelli mõnda väärtust "NextHopType" on erinevad nimed:
- Virtuaalne võrk on VnetLocal
- Virtuaalse võrgu lüüs on VirtualNetworkGateway
- Virtuaalse seadme on VirtualAppliance
- Internet on Internet
- Ükski pole

### <a name="system-routes"></a>Süsteemi marsruudib
Iga loodud virtuaalse võrgu alamvõrgu on automaatselt seotud marsruutimiseks tabeli, mis sisaldab järgmist süsteemi marsruutimiseks reeglite:

- **Kohaliku Vnet reeglit**: see reegel luuakse automaatselt iga alamvõrgu virtuaalse võrku. Saate määrata, et selle VNet VM vahel on otsene ja on vahe järgmise sõnumihüppe pole kohta, pärast.
- **Reegli asutusesisese**: selle reegli asutusesisese aadress vahemiku sinna kõik liikluse kehtib ja kasutab VPN-lüüsi järgmise sõnumihüppe kohta, pärast sihtkoht.
- **Interneti-reegel**: see reegel pidemete kogu liikluse sinna avaliku Interneti (aadressi prefiks 0.0.0.0/0) ja kasutab taristu Interneti-lüüsi on järgmine sõnumihüppe kohta, pärast kõigi liiklus sinna Interneti-ühendus.

### <a name="user-defined-routes"></a>Kasutaja määratletud marsruudib
Enamik keskkonna jaoks peate vaid süsteemi marsruudib, mis on juba määratletud Azure. Siiski võib-olla peate marsruutimiseks tabeli loomine ja lisada ühe või mitu teatud juhtudel, näiteks:

- Jõusta tunneling Interneti kohapealse võrgu kaudu.
- Virtuaalne kasutamise Azure keskkonnas.

Ülaltoodud stsenaariume, on teil marsruutimiseks tabeli loomine ja kasutaja määratletud marsruudib lisada. Saate määrata mitu marsruutimiseks tabelid ja sama marsruutimiseks tabeli võib olla seotud üks või mitu alamvõrku. Ja iga alamvõrgu saab ainult ühe marsruutimiseks tabeli seostada. Kõik VMs ja pilveteenustega alamvõrgu Kasuta marsruutimiseks tabeli seotud selle alamvõrgu.

Alamvõrku toetuvad süsteemi marsruudib kuni marsruutimiseks tabel on seotud alamvõrgu. Kui ühendus on olemas, marsruutimine tehakse klõpsake pikima eesliite Match (LPM) nii kasutaja määratletud marsruudib ja protsesside süsteemi vahel. Kui on rohkem kui üks viis, kuidas sama LPM vaste siis marsruudi valitakse selle päritolu järgmises järjestuses:

1. Kasutaja määratletud marsruutimiseks
1. BGP marsruutimiseks (kui kasutatakse ExpressRoute)
1. Süsteemi marsruutimiseks

Saate teada, kuidas luua kasutaja määratletud marsruudib, vaadake, [Kuidas luua ja luba IP edastamine Azure](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Kasutaja määratletud marsruudib rakendatakse ainult Azure VMs ja cloud services. Näiteks, kui soovite lisada tulemüüri virtuaalse seadme oma kohapealse võrgu ja Azure vahel, on teil luua kasutaja määratletud marsruudi Azure marsruutimiseks tabelite jaoks, mis edastab kõigi liiklus läheb kohapealse aadressiruumi virtuaalse seadmele. Kasutaja määratletud marsruutimiseks (UDR) saate lisada ka GatewaySubnet ümber suunama kogu liikluse kohapealse Azure virtuaalse seadme kaudu. See on viimase täienduse.

### <a name="bgp-routes"></a>BGP marsruudib
Kui teil on ExpressRoute seost kohapealse võrgu ja Azure, saate lubada BGP levitada marsruudib kohapealse võrgu kaudu Azure. BGP lennuliinide kasutatakse samamoodi nagu süsteemi marsruudib ja kasutaja määratletud marsruudi iga Azure'i alamvõrgu. Lisateabe saamiseks vaadake teemat [Sissejuhatus ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Saate konfigureerida Azure keskkonna Jõusta tunneling kohapealse võrgu kaudu, luua kasutaja määratletud marsruutimiseks alamvõrgu 0.0.0.0/0, mis kasutab funktsiooni järgmise sõnumihüppe kohta, pärast VPN-lüüsi jaoks kasutada. Kuid see toimib ainult, kui kasutate VPN-lüüsi, mitte ExpressRoute. ExpressRoute, jõustatud tunneling on konfigureeritud BGP kaudu.

## <a name="ip-forwarding"></a>IP ümbersuunamine
Nagu on kirjeldatud ülal, üks luua kasutaja määratletud marsruutimiseks põhjuseks on virtuaalse seadme-liikluse suunata. Virtuaalse seadme on midagi enamat kui VM, mis töötab rakendus, mida kasutatakse käsitlema võrguliiklust mingil viisil, nt tulemüüri või NAT seade.

Selle virtuaalse seadme VM peavad saama Sissetuleva liikluse, mis on adresseeritud ise. VM saada adresseeritud muude sihtkohtade liikluse lubamiseks peate lubama IP edastamine VM. See on mõni Azure säte, mitte säte Külastajate operatsioonisüsteemi.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [luua marsruudib ressursihaldur juurutamise mudeli](virtual-network-create-udr-arm-template.md) ja alamvõrku seostada. 
- Siit saate teada, [marsruudib klassikaline juurutamise mudeli](virtual-network-create-udr-classic-ps.md) loomiseks ja alamvõrku seostada.
