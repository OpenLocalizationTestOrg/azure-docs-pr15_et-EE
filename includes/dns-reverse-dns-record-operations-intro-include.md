## <a name="what-is-reverse-dns"></a>Mis on vastupidise DNS-i?

Tavaline DNS-i kirjete võimaldavad vastenduse DNS-i nimi (nt "www.contoso.com") IP-aadress (nt 64.4.6.100).  Vastupidise DNS-i võimaldab tõlge IP-aadressi (64.4.6.100) naasmiseks nimi ('www.contoso.com).

Vastupidise DNS-i kirjete kasutatakse erinevates olukordades. Näiteks vastupidise DNS-i kirjed on levinud vastu e-posti rämpsposti saatja meilisõnumi kontrollides.  Meili vastuvõtmise server tuua vastupidise DNS-i kirje saatmise serveri IP-aadressi, ja veenduge, et kui majutavate on õigus saada e-posti pärit domeeni. (Pange tähele, kuid see [ei toeta Azure arvutada teenuste välise domeenid meilide saatmine](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Kuidas vastupidise DNS-i töötab

Vastupidise DNS-i kirjed on hostitud teisiti DNS-i tsoonid, nimetatakse "ARPA" tsoonid.  Tsoonid vormi eraldi hierarhia DNS-i majutusteenuse domeenid, nt "contoso.com" tavaline hierarhia paralleelselt.

Näiteks rakendatakse DNS-i kirje "www.contoso.com" zone contoso.com DNS-i "A" kirje nimi www kasutamine.  A-kirje viitab vastava IP-aadress, praegusel juhul 64.4.6.100.  Tagant otsing rakendatakse eraldi, kasutades kirje "PTR" nimega "100" tsoonis "6.4.64.in-addr.arpa" (Pange tähele ARPA piirkondades vastupidises IP-aadressid.)  See PTR-kirje, kui see on õigesti konfigureeritud osutab www.contoso.com nimi.

Kui ettevõtte määratakse IP aadressiploki, nad saavad ka juhtida vastava ARPA tsooni. Vastav IP address plokid kasutatavaid Azure'i ARPA tsoonid on majutatud ja haldab Microsoft. Teie ISP võib majutada oma IP-aadresside ARPA tsooni teile või võimaldab teil ARPA tsooni DNS-i teenus teie valitud, nt Azure DNS-i hosti.

>[AZURE.NOTE] Edasi DNS otsingud ja vastupidise DNS otsingud rakendatakse eraldi, paralleelselt DNS-i hierarhiate. Pööratud otsinguga "www.contoso.com" **pole** majutatud tsooni "contoso.com", pigem see on majutatud vastavate IP Aadressiploki ARPA tsooni.

Vastupidise DNS-i kohta leiate lisateavet teemast [Vastupidise DNS-i otsing](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure'i vastupidise DNS-i tugi

Azure'i toetab kahte eraldi stsenaariumid, mis on seotud DNS-i tühistada.

1. Majutada oma IP-aadresside blokeerimine vastav ARPA tsooni.
2. Võimaldab teil konfigureerida vastupidise DNS-i kirje Azure'i teenust määratud IP-aadressi.

Endise, Azure DNS-i toetamiseks saab majutada oma ARPA tsoonid ja hallata iga vastupidise DNS-i lookup PTR kirjed.  Protsessi loomise ARPA tsooni, delegeerimine häälestamise ja konfigureerimise PTR kirjete on sama mis tavaline DNS-tsooni.  Ainult erinevused on delegeerimine peab olema konfigureeritud kaudu oma ISP, mitte oma DNS-i domeeniregistraatori, ja tuleks kasutada ainult PTR kirje tüüp.

Viimase toetamiseks Azure'i abil saate konfigureerida tagant otsingu jaoks teenust eraldatud IP-aadressid.  Selle tagant otsingu on konfigureeritud, Azure'i vastava ARPA tsooni PTR-kirje.  Nende ARPA tsoonide vastav IP vahemikke Azure, mida majutatakse Microsoft. **Ülejäänud selles artiklis kirjeldatakse üksikasjalikult seda stsenaariumi.**
