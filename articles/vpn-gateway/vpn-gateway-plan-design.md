<properties 
   pageTitle="VPN-lüüsi plaanimine ja kujundus | Microsoft Azure'i"
   description="Lisateavet VPN-lüüsi plaanimine ja asukohaga, hübriid ja VNet-VNet ühendused kujundus"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Kavandamise ja kujunduse VPN-lüüsi jaoks

Plaanimine ja kujundamine oma asutusesiseses ja VNet-VNet konfiguratsioone võib olla lihtne või keerukas võrgustik vajadust. Selles artiklis tutvustatakse plaanimine ja kujundus tavaline peaksite arvesse võtma.

## <a name="planning"></a>Plaanimine


### <a name="compare"></a>Asutusesiseses ühenduvuse suvandid

Kui soovite oma kohapealse saitide turvaliselt ühendada virtuaalse võrgu, on teil kolm võimalust seda teha: saidi saidi, punkt-saidi ja ExpressRoute. Võrrelge erinevate asutusesiseses ühendused, mis on saadaval. Valitud suvand võivad sõltuda erinevatest asjaoludest, näiteks:


- Millist liiki läbilaskevõime teie lahendus nõutav?
- Kas soovite üle avaliku Interneti kaudu turvaline VPN või privaatne kaudu suhtlemine?
- Kas teil on avalik kasutamiseks saadaval IP-aadress?
- Kas plaanite kasutada VPN seadme? Sel juhul on ühilduvad?
- Te loote vaid mõne arvutis või te soovite pidev ühendus oma saidile?
- Mis tüüpi VPN-lüüsi on vaja lahenduse, mida soovite luua?
- Millised lüüsi SKU tuleks kasutada?


Järgmine tabel aitab teil otsustada Ühenduvus on parim valik oma lahenduse.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Lüüsi nõuded VPN-tüüp ja SKU-ga

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Lüüsi SKU-de jaoks kohta leiate lisateavet teemast [VPN-lüüsi sätted](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Aggregate läbilaskevõime SKU ja VPN tüübi järgi

Järgmises tabelis on lüüsi tüübid ja hinnanguline liitväärtuse läbilaskevõime. Hinnanguline liitväärtuse läbilaskevõime võib määravad tegur kujunduse jaoks.
Hinnad erinevad lüüsi SKU-de jaoks. Lisateavet hindade kohta leiate teemast [VPN lüüsi hinnad](https://azure.microsoft.com/pricing/details/vpn-gateway/). Selles tabelis kehtib ressursihaldur ja klassikaline juurutamise mudelid.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Tippige toetatud konfiguratsioone Tootekood ja VPN

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Töövoo

Järgmises loendis kirjeldatakse levinud töövoo pilvepõhist ühenduvust:

1.  Kujundamine ja oma ühenduvuse topoloogia kavandamine ja loendi aadress tühikute kõikides võrkudes, millega soovite ühenduse.
2.  Luua Azure virtuaalse. 
3.  Saate luua VPN-lüüsi virtuaalse võrgu jaoks.
4.  Luua ja konfigureerida kohapealse võrgu või mõne muu virtuaalse võrguga ühendusi (vajadusel).
5.  Loomine ja konfigureerimine punkti saidi ühenduse oma Azure'i VPN-lüüsi (vajadusel).
 

## <a name="design"></a>Kujundus

### <a name="topologies"></a>Ühenduse topoloogiatest

Alustuseks vaadates [VPN lüüsi](vpn-gateway-about-vpngateways.md) artikli diagrammid. Artikkel sisaldab lihtsa skeemid, iga topoloogia (ressursihaldur või klassikaline) juurutamise näidised ja millised juurutamise tööriistad abil saate juurutada konfiguratsioonist.   

### <a name="designbasics"></a>Kujunduse alused

Järgmistes lõikudes käsitletakse lüüsi VPN-i põhitõed. Pidage silmas ka [võrgu teenuste piirangud](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Alamvõrku kohta

Ühenduste loomisel peate arvesse võtma teie alamvõrku vahemikke. Ei saa kattuvate alamvõrgu aadresside vahemikud. Kuvatakse kattuvad alamvõrgu on, kui üks virtuaalse võrgu või kohapealse asukoht sisaldab sama aadress ruumi, mis sisaldab teise asukohta. See tähendab, et peate oma võrgu insenerid teie kohaliku kohapealse võrgu jaoks kasutamiseks Azure'i IP vahemiku leida ruumi/alamvõrku adressaatide valimiseks. Peate aadressiruumi, mis ei kasutata kohaliku kohapealse võrgus. 

Alamvõrku kattumise vältimiseks on oluline, kui töötate VNet-VNet ühendused. Kui teie alamvõrku kattuvad ja IP-aadress on olemas saatva ja sihtkoha VNets, VNet-VNet ühendusi. Azure'i ei saa andmeid muude VNet marsruutimine, kuna sihtaadressi on saatmise VNet osa. 

VPN lüüside jaoks on vaja teatud alamvõrku, nimetatakse lüüsi alamvõrgu. Kõik lüüsi alamvõrku peab nimega GatewaySubnet töötaks õigesti. Kindlasti mitte teie alamvõrku lüüsi nimi mõni muu nimi ja ei juurutada lüüsi alamvõrgu VMs või midagi muud. Vaadata [lüüsi alamvõrku](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Kohalikus võrgus lüüside kohta

Lüüsi kohtvõrgu viitab tavaliselt oma kohapealse asukoht. Klassikaline juurutamise mudeli, kohtvõrgu lüüsi on edaspidi kohaliku võrgu saidi. Kui konfigureerite kohtvõrgu lüüsi, saate sellele nime panna, määrake kohapealse VPN seade avaliku IP-aadress ja aadress eesliidete tähised, mis on kohapealse asukoha määramiseks. Azure'i suunatud võrguliikluse eesliiteid sihtkoha aadress, konsulteerib konfiguratsioonis määratud kohtvõrgu lüüsi ja marsruudib paketid vastavalt sellele. Saate muuta nende aadress eesliiteid vastavalt vajadusele. Lisateavet leiate teemast [kohtvõrgu lüüsid](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="gwtype"></a>Lüüsi tüübid

Äärmiselt oluline on teie topoloogia jaoks õige lüüsi tüübi valimine. Kui valite vale tüüpi, lüüsi ei tööta õigesti. Lüüsi tüüpi saate määrata, kuidas ise lüüsi loob ja on nõutav konfiguratsioon sätte ressursihaldur juurutamise mudeli.

Lüüsi tüübid on:

- VPN
- ExpressRoute

#### <a name="connectiontype"></a>Ühenduse tüübid

Iga konfiguratsiooni nõuab teatud ühenduse tüüp. Ühenduse tüübid on:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>VPN tüübid

Iga konfiguratsiooni nõuab teatud tüüpi VPN. Kui olete ühendatud kahe konfiguratsioone, nt saidilt ühenduse ja sama VNet, punkt-ja saidi ühenduse loomiseks peate kasutama VPN-tüüp, mis vastab nii ühenduse nõuetele.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

Järgmistes tabelites on tippige VPN nagu see iga ühenduse konfiguratsiooni kaardid. Veenduge, et teie Gateway VPN-tüüp vastab konfiguratsioon, mida soovite luua. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>VPN seadmed saitide ühenduste jaoks

Konfigureerimine saidilt ühendus, olenemata juurutamise mudeli, peate järgmised üksused:

- VPN seade, mis ei ühildu Azure'i VPN lüüsid
- Avaliku IPv4 IP-aadress, mis pole taga NAT

Peate seadme VPN konfigureerimise teenuse või on keegi, et saate konfigureerida seadme jaoks. VPN seadmete kohta leiate lisateavet teemast [kohta VPN seadmed](vpn-gateway-about-vpn-devices.md). VPN seadmete artikkel sisaldab teavet valideeritud seadmed, seadmetes, mis on kinnitatud nõuded ja lingid seadme konfiguratsiooni dokumendid, kui see on saadaval.

### <a name="forcedtunnel"></a>Kaaluge jõustatud tunneliga marsruutimine

Enamik konfiguratsioone, saate konfigureerida jõustatud tunneling. Jõustatud tunneling võimaldab teil redirect või "Jõusta" kõik Interneti seotud liikluse tagasi oma kohapealse asukohta VPN saidilt tunneliga ülevaatuse ja auditeerimine kaudu. See on kriitiline turvalisus nõue enamik ettevõtte IT poliitika. 

Jõustatud tunneling ilma Interneti seotud liiklus teie VMs Azure on alati läbida: Azure'i võrgu taristule otse välja Interneti-ühendus, ilma suvandi abil saate kontrollida või kontrollida liiklust. Volitamata Interneti-ühendus võib põhjustada potentsiaalselt teabe või muud tüüpi turvalisus seotud rikkumiste eest.

**Sunnitud tunnelite skeem**

![Sunnitud Tunneling ühendus] (./media/vpn-gateway-plan-design/forced-tunnel.png "sunnitud tunneling")

Jõustatud tunnelite ühenduse saab konfigureerida nii juurutamise mudelite ja erinevate tööriistade abil. Vt lisateavet järgmises tabelis. Uuendame see tabel, kui selle konfiguratsiooni saadaval uus artikleid, uusi juurutamise mudeleid ja täiendavad tööriistad. Kui artikkel on saadaval, me lingi otse tabelist.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet, mis aitavad teil oma kujundus [VPN lüüsi KKK](vpn-gateway-vpn-faq.md) ja [VPN lüüsi](vpn-gateway-about-vpngateways.md) artikleid.

Teatud lüüsi sätete kohta leiate lisateavet teemast [VPN lüüsi sätete kohta](vpn-gateway-about-vpn-gateway-settings.md).




