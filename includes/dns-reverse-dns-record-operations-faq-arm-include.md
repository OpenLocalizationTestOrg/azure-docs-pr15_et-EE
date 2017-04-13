<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>KKK – vastupidise DNS-i Azure määratud IP-aadressi

### <a name="how-much-do-reverse-dns-records-cost"></a>Palju tagurpidi DNS-i kirjete maksumus?
Need on tasuta!  On täiendavaid tasuta vastupidise DNS-i kirjeid või päringud.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Kas vastupidise DNS-kirjeid oma Azure'i määratud avaliku IP-aadressi lahendamiseks Interneti kaudu?
Jah. Kui määrate atribuudi vastupidise DNS-i avaliku IP-aadress, haldab Azure'i kõigi esinduste DNS-i ja DNS-i tsoonid, et tagada kõigile Interneti kasutajatele lahendab vastupidise DNS-i kirje.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Vaikimisi vastupidise DNS-i kirje luuakse minu avaliku IP-aadresside?
Ei. Vastupidise DNS-i on funktsioon spetsiaalseid sisse. Kui te ei soovi nende konfigureerimine luuakse vaikimisi tagant DNS-i kirjeid.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Mis on täielikult täielik domeeninimi (FQDN) vorming?
FQDN-i on määratud edasi järjestuses ja lõpetada dot (nt "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Mis juhtub, kui ma määratud vastupidise DNS-i jaoks kontroll ei?
Kui kontrollimisel vastupidise DNS-i valideerimine nurjub, teenuse haldus toiming nurjub. Palun parandamine vastupidise DNS-i väärtus vastavalt vajadusele uuesti.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Saate hallata vastupidise DNS-i veebisaiti Azure?
Vastupidise DNS-i Azure veebilehed ei toetata. Azure'i Virtuaalmasinates toetatakse vastupidise DNS-i.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Saate konfigureerida mitme vastupidise DNS-i kirjete minu avaliku IP-aadress?
Ei. Azure'i toetab ühe vastupidise DNS-i kirje iga avaliku IP-aadressi. Iga avaliku IP-aadressi aga võib olla oma vastupidise DNS-i kirje.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Saate konfigureerida vastupidise DNS-i kirjete IPv6 avaliku IP-aadressi?
Ei.  Sel ajal, vastupidise DNS-i kirjed on toetatud IPv4 avaliku IP-aadresside ainult.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Saate konfigureerida vastupidise DNS-i kirje minu avaliku IP-aadress on määratud DomainNameLabel ilma?
Ei. Koosolekuteabe puhul kasutada avaliku IP-aadresside vastupidise DNS-i kirjeid, määrake atribuudi DomainNameLabel.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Saate saata e-kirju väliste domeenide minu Azure'i arvutada teenused?
Ei. [Azure'i Arvutage teenuste ei toeta meilide saatmine välise domeenid](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
