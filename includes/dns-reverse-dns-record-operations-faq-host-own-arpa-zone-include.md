<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>KKK – teie ARPA tsooni Azure'i DNS-i majutusteenuse

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Saan majutada ARPA tsoonid oma ISP määratud IP plokid Azure'i DNS-i jaoks?
Jah. ARPA tsoonid jaoks oma IP-vahemikke Azure'i DNS-i majutusteenuse täielikult toetatud.

Lihtsalt [luua Azure'i DNS-i tsooni](dns-getstarted-create-dnszone.md), siis oma ISP volitatud [tsooni](dns-domain-delegation.md)töötamine.  Seejärel saate hallata iga tagant lookup PTR kirjed sarnaselt muude kirjetüüpide.

Samuti saate [olemasoleva tagant otsing tsooni, mis Azure'i CLI abil importida](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Kui palju maksab majutusteenuse minu ARPA tsooni maksumus?
Hosting ARPA tsooni oma ISP määratud IP Azure'i DNS-i plokk lisandu [standard Azure'i DNS-i](https://azure.microsoft.com/pricing/details/dns/)määrade.

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Saan majutada ARPA tsoonid nii IPv4 ja IPv6 aadressid Azure'i DNS-i jaoks?
Jah.