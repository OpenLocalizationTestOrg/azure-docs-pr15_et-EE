<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>KKK – vastupidise DNS-i Azure määratud IP-aadressi

### <a name="how-much-do-reverse-dns-records-cost"></a>Palju tagurpidi DNS-i kirjete maksumus?
Need on tasuta!  On täiendavaid tasuta vastupidise DNS-i kirjeid või päringud.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Vastupidise DNS-i kirjeid lahendab Interneti kaudu?
Jah. Kui vastupidise DNS-i atribuutide seadmine oma pilveteenuses Azure'i haldab kõigi esinduste DNS-i ja DNS-i tsoonid, et tagada kõigile Interneti kasutajatele lahendab vastupidise DNS-i kirje.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Vaikimisi vastupidise DNS-i kirje luuakse minu pilveteenustega?
Ei. Vastupidise DNS-i on funktsioon spetsiaalseid sisse. Kui te ei soovi nende konfigureerimine luuakse vaikimisi vastupidise DNS-i kirjeid.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Mis on täielikult täielik domeeninimi (FQDN) vorming?
FQDN-i on määratud edasi järjestuses ja lõpetada dot (nt "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Mis juhtub, kui ma määratud vastupidise DNS-i jaoks kontroll nurjuda?
Kui kontrollimisel vastupidise DNS-i valideerimine nurjub, teenuse haldus toiming nurjub. Palun parandamine vastupidise DNS-i väärtus vastavalt vajadusele uuesti.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Saate hallata vastupidise DNS-i Azure veebisaiti?
Vastupidise DNS-i Azure veebilehed ei toetata. Azure'i PaaS rollid ja IaaS virtuaalmasinates vastupidise DNS-i on toetatud.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Mitme vastupidise DNS-i kirjeid saate konfigureerimine minu pilveteenuses?
Ei. Azure'i toetab ühe vastupidise DNS-i kirje iga Azure'i pilveteenuses. Iga Azure'i pilveteenuses aga võib olla oma vastupidise DNS-i kirje.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Saate saata e-kirju väliste domeenide minu Azure'i Arvutage teenused?
Ei. [Azure'i arvutada teenuste ei toeta meilide saatmine välise domeenid](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
