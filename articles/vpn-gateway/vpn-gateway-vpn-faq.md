<properties 
   pageTitle="Virtuaalse võrgu VPN lüüsi KKK | Microsoft Azure'i"
   description="VPN-lüüsi KKK. Microsoft Azure'i virtuaalse asutusesiseses võrguühenduste, hübriidi konfiguratsiooni ühendused ja VPN lüüside KKK"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>VPN-lüüsi KKK

## <a name="connecting-to-virtual-networks"></a>Virtuaalne võrkude ühendamine

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Saate ühendada virtuaalse võrgu eri Azure piirkondades?
Jah. Tegelikult on piirkond piiranguteta. Ühe virtuaalse võrgu saate luua ühenduse mõne muu virtuaalse võrgu piirkonna või teises regioonis asuva Azure.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Saate ühendada virtuaalse võrkude muu tellimuste?
Jah.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Saate ühendada mitu saitide ühe virtuaalse võrgu kaudu?

Windows PowerShelli ja Azure REST API-de abil saate ühendada mitu saidid. [Mitme saidi- ja VNet-VNet](#multi-site-and-vnet-to-vnet-connectivity) KKK jaotisest.
## <a name="what-are-my-cross-premises-connection-options"></a>Mis on minu asutusesiseses ühendussuvandid?

On toetatud järgmised asutusesiseses ühendused:

- [Saitide](vpn-gateway-site-to-site-create.md) – VPN-ühendus üle IPsec (IKE v1 ja IKE v2). Seda tüüpi ühenduse jaoks on vaja VPN seadme või RRAS.

- [SharePointi saidil](vpn-gateway-point-to-site-create.md) – VPN-ühendus üle SSTP (Secure turvasoklite Tunneling Protocol). See ühendus ei nõua VPN seade.

- [VNet-VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – seda tüüpi ühendus on sama, mis-saitide konfigureerimine. Et VNet VNet on VPN-ühendus üle IPsec (IKE v1 ja IKE v2). See ei nõua VPN seade.

- [Mitme saidi](vpn-gateway-multi-site.md) – see on variatsioon-saidilt konfiguratsiooni, mis võimaldab teil mitme kohapealse saidi virtuaalse võrguga.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute on otsene ühendus Azure kaudu oma WAN, mitte avaliku Interneti. [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md) ja [ExpressRoute KKK](../expressroute/expressroute-faqs.md) lisateabe saamiseks vt.

Ühenduste kohta leiate lisateavet teemast [VPN lüüsi](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Mille poolest erinevad saidilt ühendust ja punkti – saidile?

**Saidilt** ühendused, mis võimaldavad teil ükskõik oma ettevõttes virtuaalse masina või rolli eksemplari virtuaalse võrgu, olenevalt sellest, kuidas te otsustate konfigureerimine marsruutimine asub vahel ühenduse. See on hea valik alati saadaval asutusesiseses ühendust ja sobib hästi hübriidi konfiguratsiooni. Seda tüüpi ühenduse tugineb IPsec VPN seadme (riistvara või pehmete seadme), mis tuleb kasutada võrgu servast. Seda tüüpi ühenduse loomiseks peab teil vaja VPN riistvara ja väliselt suunatud IPv4 aadress.

**SharePointi saidil** ühendused võimaldavad teil ühenduse igal pool ühes arvutis midagi asub virtuaalse võrgu. Windowsi-karbi VPN klient kasutab. Punkti – saidile konfiguratsiooni osana installite sert ja VPN kliendi konfiguratsiooni paketi, mis sisaldab ühenduse, virtuaalse masina või rolli eksemplari virtuaalse võrgustikus arvuti sätteid. See on suur, kui soovite virtuaalse võrguga, kuid pole asub kohapealse. Samuti on hea valik kui teil pole juurdepääsu VPN riistvara või väliselt suunatud IPv4 aadress, mis on nõutav saidilt ühenduse. 

Saate konfigureerida virtuaalse võrgu kasutada-saitide ja punkti saidi üheaegselt, tingimusel, et luua oma saidilt ühenduse kasutamise lüüsi marsruutimiseks vastavalt VPN-tüüp. Marsruutimiseks vastavalt VPN tüüpi nimetatakse dünaamiline lüüside klassikaline juurutamise mudeli.

### <a name="what-is-expressroute"></a>Mis on ExpressRoute?

ExpressRoute võimaldab teil luua privaatne seoseid Microsofti andmekeskuses ja infrastruktuur, mis on oma ettevõttes või kaasautorluse kohta keskkonnas. ExpressRoute, saate luua ühendusi Microsofti pilveteenustega nagu Microsoft Azure'i ja Office 365 ExpressRoute partneri saanud poole või otse ühendust oma olemasoleva WAN võrgust (nt MPLS VPN võrgu teenuse pakkuja).

ExpressRoute ühendused pakuvad parema turvalisuse eesmärgil rohkem töökindlust, suurema läbilaskevõime ja väiksem kui tüüpilised ühendused latentsused Interneti kaudu. Mõnel juhul oma kohapealse võrgu ja Azure vahel andmete edastamine ExpressRoute ühenduste abil saate ka yield kulude eelised. Kui olete juba loonud ühenduse asutusesiseses kohapealse võrgu kaudu Azure, saate migreerida ExpressRoute ühendus säilitades virtuaalse võrgu samaks.

Vt lisateavet [ExpressRoute KKK](../expressroute/expressroute-faqs.md) .

## <a name="site-to-site-connections-and-vpn-devices"></a>Saitide ühendused ja VPN seadmed

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Mida peaks kaaluma, valides VPN seadme?

Meil on kinnitatud standard-saidilt VPN seadmete koostöös kogum seadme hankijatega. Loendi teadaolevad ühilduvate VPN, konfigureerimise juhised või näidiseid ja seadme andmed leiate [siit](vpn-gateway-about-vpn-devices.md). Kõigi seadmete seadme peredele, mis on loetletud teadaolevad ühilduv peaks töötama virtuaalse võrguga. Selleks, et seadme VPN konfigureerida, vaadake seadme konfiguratsiooni näidis või link, mis vastab soovitud seade pere.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Mida teha, kui mul on VPN seadet pole loendis teadaolevad kõnesid?

Kui te ei näe oma seadme loetletud teadaolevad VPN kõnesid ja soovite seda kasutada VPN-ühendus, peate veenduge, et see vastab olevat toetatud IPsec/IKE konfiguratsiooni suvandid ja parameetrite loetletud [siin](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Seadmete miinimumnõuetele tuleks töötada ka VPN lüüsid. Täiendavate tugiteenuste ja konfiguratsiooni juhiste saamiseks pöörduge oma seadme tootjalt.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Miks mu poliitika vastavalt VPN tunneliga läheb jõude liikluse korral?

See on oodatud käitumine marsruutimiseks poliitika-põhise (tuntud ka kui staatiline) VPN lüüsid. Kui liikluse üle soovitud tunneliga on rohkem kui 5 minuti jooksul jõudeolekus, kuvatakse selle tunneliga katki. Käivitamisel liikluse vastupidises kummagi suuna taastub selle tunneliga kohe.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Kasutada tarkvara VPN ühenduse Azure'i?

Toetame Windows Server 2012 marsruutimine ja Remote Access (RRAS) serverites – saidilt asutusesiseses konfigureerimine.

Teiste tarkvara VPN lahenduste peaks töötada meie lüüsi, kui need ei vasta valdkonna standard IPsec rakendusi. Kontaktandmete tarkvara konfiguratsiooni ja tugiteenuste juhised.

## <a name="point-to-site-connections"></a>Punkti saidi ühendused

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Milliseid opsüsteeme saate kasutada punkti – saidile?

Järgmised operatsioonisüsteemid on toetatud:

- Windows 7 (32-bitine ja 64-bitine)

- Windows Server 2008 R2 (ainult 64-bitine)

- Windows 8 (32-bitine ja 64-bitine)

- Windows 8.1 (32-bitine ja 64-bitine)

- Windows Server 2012 (ainult 64-bitine)

- Windows Server 2012 R2 (ainult 64-bitine)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Saate kasutada mis tahes tarkvara VPN klient punkti saidil SSTP toetab?

Ei. Tugi on piiratud ainult eelnimetatud Windowsi operatsioonisüsteemi versiooni.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Kui palju VPN kliendi lõpp-punktid võib olla minu saidi punkti konfiguratsiooni?

Me ei toeta kuni 128 VPN kliente saama võrguga virtuaalse samal ajal.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kasutada oma sisemine PKI root CA punkti saidi Ühenduvus?

Jah. Varem saaks kasutada ainult iseallkirjastatud juursertide. Siiski saate üles laadida 20 juursertide.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Saate ma läbida puhverserverid ja tulemüürid kasutamise võimalus punkti – saidile?

Jah. SSTP (Secure turvasoklite Tunneling Protocol) abil tunneliga tulemüürid kaudu. See tunneliga kuvatakse HTTPs-ühenduse.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Kui taaskäivitamiseks punkti saidi jaoks konfigureeritud klientarvuti VPN-i automaatselt uuesti?

Vaikimisi klientarvuti ei ole taastamiseks VPN-ühendus automaatselt.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Kas punkti – saidile tugiteenuste automaatne-uuesti ja DDNS VPN kliendid?

Automaatne uuesti ja DDNS praegu ei toetata punkti saidi VPN.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kas mul on saitide ja punkti saidi konfiguratsioone eksisteerivad sama virtuaalse võrgu jaoks?

Jah. Mõlemad järgmisi lahendusi töötab, kui teil on oma lüüsi RouteBased VPN tüüp. Klassikaline juurutamise mudeli, peate dünaamiline lüüsi. Me ei tugi punkti saidi staatiline marsruutimine VPN lüüsid või lüüside - VpnType PolicyBased abil.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Saate konfigureerida punkti saidi kliendi korraga mitme virtuaalse võrgu ühenduse?

Jah, on võimalik. Aga virtuaalne võrkude ei tohi olla kattuvad IP eesliiteid ja punkti saidi aadress tühikute peab ei kattuks virtuaalse võrgu vahel.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Kui palju läbilaskevõime oodata,-saidilt või saidi punkti ühenduste kaudu?

See on raske säilitada täpse läbilaskevõime VPN tunnelite. IPsec ja SSTP on raske crypto VPN-i protokollid. Läbilaskevõime on piiratud ka latentsus ja läbilaskevõime oma ettevõttes ja Interneti vahel.

## <a name="gateways"></a>Lüüside

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Mis on poliitika-põhise (staatiline marsruutimine) lüüsi?

Poliitika-põhise lüüside rakendada poliitika vastavalt VPN. Poliitika vastavalt VPN krüptimiseks ja otse paketid kaudu IPsec tunnelid aadress eesliiteid kohapealse võrgu ja Azure VNet kombinatsioonid põhjal. Poliitika (või liikluse valija) on tavaliselt määratletud konfiguratsiooni VPN juurdepääsu loendi.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Mis on marsruutimiseks vastavalt (dünaamiline marsruutimine) lüüsi?

Lüüside marsruutimiseks vastavalt rakendada marsruutimiseks vastavalt VPN. Marsruutimiseks vastavalt VPN kasutage IP edasisaatmine või tabeli marsruutimiseks otse paketid sisse oma vastavate tunneliga liideste "marsruudib". Tunneliga liideste seejärel krüptida või dekrüptida ja sealt tunnelid paketid. Poliitika või liikluse valija jaoks marsruutimiseks vastavalt VPN on konfigureeritud nii kõik mis (või metamärgistust).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Kas ma saan minu VPN gateway IP-aadress enne selle luua?

Ei. Peate looma oma lüüsi esmalt, et leida IP-aadress. IP-aadress muutub kustutamisel ja VPN-lüüsi uuesti luua.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Kuidas mu VPN tunneliga saada autenditud?

Azure'i VPN kasutab PSK (Pre-Shared Key) autentimist. Saame luua eelnevalt jagatud võti (PSK) VPN tunneliga loomisel. Automaatselt loodud PSK saate muuta oma seadmine Pre-Shared Key PowerShelli cmdleti või REST API-ga.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Kasutada seadmine Pre-Shared Key API konfigureerida minu poliitika vastavalt VPN lüüsi (staatiline marsruutimine)?

Jah, saab konfigureerida Azure poliitika-põhise (staatilise) VPN- ja marsruutimiseks vastavalt (dünaamiline) marsruutimise VPN Pre-Shared seadmine API võti ja PowerShelli cmdlet-käsk.

### <a name="can-i-use-other-authentication-options"></a>Kasutada muid autentimise suvandid?

Oleme piirdub eelnevalt jagatud võtmed (PSK) autentimise abil.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Mis on "lüüsi alamvõrgu" ja miks on vaja?

Meil on lüüsi teenust, mida me käivitada asutusesiseses ühenduvuse lubamine. 

Peate lüüsi alamvõrgu jaoks oma VNet konfigureerida VPN-lüüsi loomiseks. Kõik lüüsi alamvõrku peab nimega GatewaySubnet töötaks õigesti. Ärge nimi oma lüüsi alamvõrgu midagi muud. Ja ei juurutada lüüsi alamvõrgu VMs või midagi muud.

Lüüsi alamvõrgu suurus sõltub täielikult konfiguratsioon, mida soovite luua. Kuigi see on võimalik luua lüüsi alamvõrgu /29 jaoks mõne võimalikult väike, me soovitame teil luua lüüsi alamvõrgu /28 või suurem (/ 28, /27, /26 jne.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Juurutada Virtuaalmasinates või rolli aknad minu lüüsi alamvõrgu?

Ei.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Kuidas määrata, milliseid liikluse läbib VPN-lüüsi?

Kui kasutate Azure klassikaline portaali, lisada iga vahemiku, mida soovite lüüsi kaudu saadetud virtuaalse võrgu võrgu lehe jaotises kohaliku võrgu jaoks.

### <a name="can-i-configure-forced-tunneling"></a>Sunnitud Tunneling saate konfigureerida?

Jah. Vaadake [konfigureerimine sunnitud tunneling](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Saate häälestada oma serveri Azure ja seda oma kohapealse võrguga ühenduse loomiseks kasutada?

Jah, saate juurutada oma VPN lüüsid või Azure Azure'i turuplatsi või luua oma VPN ruuterid serverites. Peate konfigureerimine kasutaja määratletud marsruudib virtuaalse võrgu tagada liiklus marsruuditakse õigesti oma kohapealse võrgu ja oma virtuaalse alamvõrku vahel.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Miks avatakse VPN-lüüsi teatud pordid?

Need on nõutav Azure taristu suhtlemiseks. Need on kaitstud (lukus), Azure serdid. Ilma õige serdid, ei saa välistele üksustele, sh kliendid nende lüüside põhjustada mingit mõju nende lõpp-punktid.

VPN-lüüsi on põhjalikult ühe NIC koputades kliendi privaatvõrgu ja üks NIC vastastikuste avaliku võrgu mitme kasutajaid seade. Azure'i infrastruktuuri üksuste ei saa kasutusele võtta kliendi võrgud vastavuse huvides nii, et need on vaja kasutada avaliku lõpp-punktid taristu suhtlemiseks. Avaliku lõpp-punktid otsitakse perioodiliselt, Azure turvalisuse audit.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Lisateavet lüüsi tüübid, nõuded ja jõudlus

Lisateavet leiate teemast [VPN lüüsi sätete](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Mitme saidi ja VNet-VNet Ühenduvus

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Mis tüüpi lüüside toetab mitme saidi ja VNet-VNet ühenduvuse?

Ainult marsruutimiseks vastavalt VPN (dünaamiline marsruutimine).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Mõne VNet saab ühendada RouteBased VPN tüüpi teise VNet PolicyBased VPN tüüpi?

Ei, virtuaalse võrgu kasutama marsruutimiseks vastavalt (dünaamiline marsruutimine) VPN.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Kas VNet-VNet liikluse turvaline?

Jah, see on kaitstud IPsec/IKE krüptimine.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Kas VNet-VNet liikluse liikuda üle Azure nurgakivi?

Jah.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Kui palju kohapealse saidid ja virtuaalse võrgu saate ühe virtuaalse võrgu ühenduse?

Max. 10 kombineeritud põhi- ja dünaamilise standardse marsruutimine lüüside; Suure jõudlusega VPN lüüside 30.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kas ma saan kasutada SharePointi saidil VPN minu virtuaalse võrguga mitme VPN tunnelitega?

Jah, saab kasutada SharePointi saidil (P2S) VPN VPN lüüside mitme kohapealse saidid ja muud virtuaalne võrkude ühendamine.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Minu virtuaalse võrgu ja kohapealse saidi mitme saidi VPN-i abil saate konfigureerida mitme tunnelid?

Ei, liigsed tunnelid on Azure virtuaalse võrgu ja ettevõtte kohapealse saidile vahel pole toetatud.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Saate olemas olla kattuvaid aadress tühikute virtuaalse ühendatud võrke ja kohapealse kohalik saitide hulka?

Ei. Kattuvate aadress tühikute põhjustab võrgu konfigureerimise faili üleslaadimine või "Loomise virtuaalse võrgu" nurjumise.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Ma saan veel rohkem-saidilt VPN kui ribalaiust ühe virtuaalse võrgu jaoks?

Ei, kõik VPN tunnelite, sh punkti saidi VPN, samas lüüsis Azure'i VPN ja läbilaskevõime ühiskasutus.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Kasutada Azure VPN-lüüsi suhtes oma kohapealse saitide vahel või muu virtuaalse võrguga?

**Klassikaline juurutamise mudeli**<br>
Teel liikluse Azure'i VPN-lüüsi kaudu on võimalik klassikaline juurutamise mudeli abil, kuid tugineb staatiliselt määratletud aadress tühikute võrgu konfigureerimise faili. Azure'i virtuaalse võrkude ja VPN lüüside klassikaline juurutamise näidise BGP veel ei toetata. Ilma BGP, käsitsi määratlemise teel aadress tühikute on väga veaväärtuse, kuna võivad tekkida kirjavead ja pole soovitatav.<br>
**Ressursihaldur juurutamise mudeli**<br>
Kui kasutate juurutamise mudeli ressursihaldur, jaotisest [BGP](#bgp) lisateavet.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Kas Azure'i luua sama IPsec/IKE eelnevalt jagatud võti kõik minu VPN ühendused virtuaalse samasse võrku?

Ei, vaikimisi Azure'i loob erinevate VPN ühendused erinevad eelnevalt jagatud võtmed. Siiski saate määrata VPN lüüsi võti REST API või PowerShelli cmdlet-käsk võtmeväärtuse soovite seada. Võti peab olema tähtnumbriline stringi pikkus 1-128 märki.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Kas Azure'i vahel virtuaalne võrkude eest?

Erinevate Azure virtuaalse võrkude liikluse, Azure'i tasud ainult liikluse liiklevad piirkonniti Azure teise. Tasuta määr on loetletud lehel Azure'i [VPN lüüsi hinnad](https://azure.microsoft.com/pricing/details/vpn-gateway/) .


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Saate ühendada virtuaalse võrgu koos IPsec VPN minu ExpressRoute ringi?

Jah, see on toetatud. Lisateabe saamiseks vt [ExpressRoute konfigureerimine ja - saidilt VPN-ühendused, mis kõrvuti](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Asutusesiseses ühenduvuse ja VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Kui minu virtuaalse masina on virtuaalse võrgu ja mul on asutusesiseses ühendus, kuidas peaks ühendada VM?

Teil on mitu võimalust. Kui teil on lubatud RDP ja lõpp on loodud, saate luua ühenduse arvuti virtuaalne VIP abil. Sel juhul saate määrata VIP ja portide, mida soovite ühendada. Peate pordi konfigureerimiseks liiklus virtual arvutis. Tavaliselt oleks Azure'i klassikaline portaalis ja RDP-ühenduse sätete salvestamiseks oma arvutisse. Sätete sisaldavad ühendus vajalik teave.

Kui teil on virtuaalse võrgu asutusesiseses ühenduvuse konfigureeritud, saate luua ühenduse arvuti virtuaalne, kasutades sisemise DIP või privaatne IP-aadress. Oma virtuaalse masina järgi sisemise DIP saate ühendada ka teise virtuaalse masina, mis asub samas virtuaalse võrgus. Te ei saa RDP virtuaalne arvuti, kasutades DIP, kui ühendate asukohast väljaspool virtuaalse võrgu. Näiteks, kui teil on punkti saidi virtuaalse võrgus konfigureeritud ja te ei loote ühenduse oma arvutist, mida ei saa ühendust virtuaalse masina järgi DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Kui minu virtuaalse masina on virtuaalse võrgu asutusesiseses Ühenduvus, läheb kogu liiklus minu VM selle ühenduse kaudu?

Ei. Ainult sihtkoht on liiklust IP, mis sisaldub virtuaalse võrgu kohaliku võrgu IP-aadressid teie määratud avage virtuaalse võrgu lüüsi kaudu. Liikluse on IP paikneva virtuaalse võrgu jääb virtuaalse võrgustikus sihtkohta. Muude liikluse on avalikud võrgud laadi koormusetasakaalustusteenuse kaudu saadetud või Azure VPN-lüüsi kaudu saadetud jõustatud tunneling kasutamise korral. Kui kasutate tõrkeotsingut, on oluline, veenduge, et teil on kõik vahemikud, mis on loetletud teie kohalikus võrgus, mida soovite saata lüüsi kaudu. Veenduge, et kohalikus võrgus aadress vahemikud ei kattuks mis tahes aadresside vahemikud virtuaalse võrgu. Samuti soovite veenduge, et kasutate DNS-i server on lahendada nime õige IP-aadress.


## <a name="virtual-network-faq"></a>Virtuaalse võrgu KKK

Täiendavad virtuaalse võrgu teabe kuvamine [Virtuaalse võrgu KKK](../virtual-network/virtual-networks-faq.md).
 
