|    | **VPN-lüüsi läbilaskevõime (1)** | **VPN-lüüsi max IPsec tunnelid (2)** | **ExpressRoute lüüsi läbilaskevõime** | **VPN-lüüsi ja ExpressRoute kõrvuti**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Tavaline SKU (3)(5)**              |  100 Mbps | 10                         |  500 Mbps                           | Ei   |
| **Standardse SKU (4)(5)**           |  100 Mbps | 10                         | 1000 Mbps                           | Jah  |
| **Suure jõudlusega SKU (4)**   | 200 Mbps  | 30                         | 2000 Mbps                           | Jah  |

- (1) VPN läbilaskevõime on ligikaudne põhjal mõõtmed VNets Azure piirkonna vahel. See ei ole tagatud läbilaskevõime asutusesiseses ühenduste Internetis. On võimalik maksimaalse läbilaskevõime mõõtühikute.
- (2) tunnelite arvu viidata RouteBased VPN. PolicyBased VPN toetab ainult ühe VPN saidilt tunneliga.
- (3) tavaline SKU BGP ei toetata.
- (4) see SKU PolicyBased VPN ei toetata. Need on lihtne SKU ainult toetatud.
- (5) aktiivne aktiivne S2S VPN-lüüsi ühendused selles SKU pole toetatud. Aktiivne-active toetatakse ainult HighPerformance SKU.