### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>BGP toetavad kõik Azure'i VPN lüüsi SKU-de jaoks?

Ei, BGP on toetatud Azure **Standard** - ja **HighPerformance** VPN lüüsid. **Tavaline** SKU ei toetata.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Saate kasutada BGP Azure Policy-Based VPN lüüside?

Ei, BGP toetatakse ainult marsruutimiseks vastavalt VPN lüüsid.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Privaatne ASNs (autonoomse süsteemi arvud) saab kasutada?

Jah, saate oma ASNs avalik või privaatne ASNs kohapealse võrgu ja Azure'i.

#### <a name="are-there-asns-reserved-by-azure"></a>Kas ASNs reserveeritud Azure?

Jah, järgmine ASNs on reserveeritud Azure'i siseste ja väliste peerings jaoks:

- Avaliku ASNs: 8075, 8076, 12076
- Privaatne ASNs: 65515 65517 65518, 65519, 65520

Ei saa määrata need ASNs Azure VPN lüüside ühendamisel oma sees ettevõttes VPN seadmete jaoks.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Saate kasutada sama ASN kohapealse VPN võrgu-ja Azure VNets?

Ei, peate määrama erinevate ASNs oma kohapealse võrgu ja oma Azure'i VNets kui ühendate need koos BGP. Azure'i VPN lüüsid on vaikimisi ASN, 65515 määratud, kas pole teie asutusesiseses ühenduvuse jaoks on lubatud BGP. Saate alistada see vaikimisi määramine erinevate ASN VPN-lüüsi loomisel või selle ASN pärast lüüsi loomist muuta. Peate määrama oma kohapealse ASNs vastavate Azure'i kohaliku võrgu lüüsid.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Millist aadressi eesliidete tähised on Azure VPN lüüside reklaamida mulle?

Azure'i VPN-lüüsi reklaamida oma kohapealse BGP seadmetes järgmisel viisil:

- Teie VNet aadress eesliiteid
- Azure'i VPN-lüüsi ühendatud aadress eesliiteid iga kohaliku võrgu lüüside jaoks
- Marsruudib õppinud Azure'i VPN-lüüsi, **välja arvatud vaikimisi marsruudi või mis tahes VNet eesliitega kattuv marsruudib**ühendatud teiste BGP silmitsemine seansid.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Saab reklaamida Azure'i VPN lüüside vaikimisi marsruudi (0.0.0.0/0)?

Pole sel ajal.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Saab reklaamida täpse eesliiteid nimega Minu virtuaalse võrgu eesliiteid?

Ei, reklaami eesliiteid sama nimega virtuaalse võrgu mõni aadress eesliidete tähised on blokeeritud või Azure'i platvormi järgi filtreeritud. Siiski saate reklaamida eesliide, mis on teil sees virtuaalse võrgu versioonist. Näiteks virtuaalse võrgu võib kasutada aadress ruumi 10.10.0.0/16 ja teil võib reklaamida 10.0.0.0/8.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Saate kasutada BGP minu VNet-VNet ühendused?

Jah, saate kasutada nii asutusesiseses ühendused ja VNet-VNet ühendused BGP.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Ma segada BGP-BGP ühenduste jaoks minu Azure'i VPN lüüside?

Jah, võite segada sama Azure'i VPN lüüsi BGP nii BGP ühendused.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Kas Azure'i VPN lüüsi tugi BGP teel marsruutimist?

Jah, BGP teel marsruutimine on toetatud, välja arvatud, et Azure'i VPN lüüsid saavad **ei** reklaamida vaikimisi marsruudib, et muud BGP kolleegidega. Teel marsruutimine üle mitme Azure'i VPN lüüsi lubamiseks peate lubama BGP kõik vahe VNet-VNet ühendused.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kas mul on mitu tunnelid Azure'i VPN-lüüsi ja kohapealse võrgu vahel?

Jah, saate luua rohkem kui üks S2S VPN tunnelite on Azure VPN-lüüsi ja kohapealse võrgu vahel. Pange tähele, et kõik need tunnelid loendatakse tunnelites oma Azure'i VPN lüüside koguarvu suhtes. Näiteks, kui teil on kaks liigsete tunnelit Azure'i VPN lüüsi ja ühe kohapealse võrgu vahel, nad tarbib 2 tunnelid välja kokku piirmäära Azure'i VPN lüüsi (10 Standard) ja 30 HighPerformance.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kas mul on mitu tunnelid vahel kaks Azure'i VNets koos BGP?

Ei, liigsed tunnelid virtuaalne võrkude paari vahele ei toetata.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Kas saan kasutada BGP S2S VPN-i konfigureerimisel ExpressRoute/S2S VPN koostöö olemas?

Pole sel ajal.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Millist aadressi Azure'i VPN-lüüsi BGP omavahelistes IP kasutada?

Azure'i VPN-lüüsi eraldab GatewaySubnet vahemiku virtuaalse võrgu jaoks määratletud IP-aadressi. Vaikimisi on vahemiku teine viimase aadressi. Näiteks kui oma GatewaySubnet on 10.12.255.0/27, ulatuvad 10.12.255.0 10.12.255.31, siis Azure VPN lüüsi BGP omavahelistes IP-aadress on 10.12.255.30. Kui olete loendi Azure'i VPN lüüsi teavet, saate selle teabe leiate.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Mis on VPN seadmes BGP omavahelistes IP-aadresside nõuded?

Teie kohapealse BGP vastastikune aadress **Ei tohi** olla sama seadme VPN avaliku IP-aadress. Kasutage BGP omavahelistes IP seadmes VPN eri IP-aadress. See võib olla seadme loopback liides aadressi. Määrake selle aadressi vastavate kohaliku võrgu lüüsi tähistav asukoht.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Mida tuleks määrata nimega Minu aadress eesliiteid kohaliku võrgu lüüsi BGP kasutamisel?

Azure'i kohaliku võrgu lüüsi määrab esmase meiliaadressi eesliiteid kohapealse võrgu jaoks. BGP, mille peate eraldada hosti eesliite (/ 32 eesliide), nagu aadressiruumi selle kohapealse võrgu jaoks BGP omavahelistes IP-aadressi. Kui teie BGP omavahelistes IP on 10.52.255.254, tuleks määrate "10.52.255.254/32" localNetworkAddressSpace kohaliku võrgu lüüsi tähistav kohapealse võrku. See on tagada, et Azure'i VPN-lüüsi luuakse BGP seansi tunneliga S2S VPN-i kaudu.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Mida tuleks lisada kohapealse VPN seadmes BGP silmitsemine seansi jaoks?

Lisage osutab IPsec S2S VPN tunneliga VPN seadmes hosti marsruudi Azure'i BGP omavahelistes IP-aadressi. Näiteks kui Azure VPN omavahelistes IP on "10.12.255.30", tuleks lisada hosti marsruudi jaoks "10.12.255.30" nexthop liides kattuvad IPsec tunneliga liides VPN-seadmes.
