## <a name="about-records"></a>Kirjete kohta

Iga DNS-i kirje on nimi ja tüüp. Kirjed on korraldatud erinevaid neis andmetel. Kõige levinum on "A" kirje, mis kaartide nimi IPv4-aadress. Teine tüüp on "MX" kirje, mis kaardid meiliserveri nimi.

Azure'i DNS-i toetab kõiki levinud DNS-i kirje, sh A, AAAA-, CNAME, MX, NS, PTR, SOA, SRV ja TXT. Pange tähele järgmist.
- SOA kirje komplekti luuakse automaatselt iga tsooni, nad ei saa luua eraldi.
- TXT-kirje tüüp abil luuakse SPF-kirje. Lisateabe saamiseks vaadake [sellelt lehelt](http://tools.ietf.org/html/rfc7208#section-3.1).

Azure'i DNS-i, kirjed on määratud suhteline nimede abil. On "" täielik domeeninimi (FQDN) sisaldab zone nime "suhteline" nimi ei. Näiteks suhteline kirje nimi "www" zone "contoso.com" annab täielikult kvalifitseeritud kirje nimi www.contoso.com.

## <a name="about-record-sets"></a>Kirje komplekti kohta

Mõnikord on vaja rohkem kui üks DNS-i kirje loomiseks antud nimi ja tüüp. Oletame näiteks, et "www.contoso.com" veebisaiti majutatakse kaks erinevat IP-aadressid. Veebisaidi jaoks on vaja kaks erinevat kirjet, üks iga IP-aadress. See on kirjekomplekti näide:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure'i DNS-i haldab DNS-i kirjete abil kirje komplektid. Kirje on DNS-i kirjete tsooni, mis on sama nimi ja on sama tüüpi kogum. Enamik kirje komplektid sisaldavad üheks kirjeks, kuid näited, nagu see, mille kirjekomplekti sisaldab rohkem kui üks kirje, ei ole aeg-ajalt.

SOA ja CNAME-kirje komplektid on erandid. DNS-i standardid ei luba mitme kirje jaoks järgmist tüüpi sama nimega.

Aeg live või TTL, saate määrata, kui kaua iga kirje vahemällu kliendid enne uuesti esitama päringu. Selles näites on TTL, 3 600 sekundit või 1 tund. TTL jaoks kirjekomplekti, mitte iga kirje jaoks on määratud nii, et kõik kirjed, et kirjekomplekti sees kasutatakse sama väärtuse.

#### <a name="wildcard-record-sets"></a>Metamärkide kirje komplektid

Azure'i DNS-i toetab [metamärkide kirjeid](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Need tagastatakse kõik päringu kattuvad nimega (kui on täpsem vaste-metamärkide kirjekomplekti kaudu). Kõigi kirjetüüpide peale NS ja SOA metamärkide kirje komplektid on toetatud.  

Metamärkide kirjekomplekti luua, kasutada kirje nime seadmiseks "\*". Või kasutage nime sildiga "\*", näiteks"\*.foo".

#### <a name="cname-record-sets"></a>CNAME-kirje komplektid

CNAME-kirje komplekti ei saa koos töötada sama nimega hulki kirje. Näiteks ei saa luua CNAME-kirje suhteline nimega "www" seadmine ja A-kirje suhteline nimega "www" samal ajal. Kuna zone tipp (nime = ‘@’) sisaldab alati NS ja SOA salvestada komplekti, mis on loodud tsooni loomisel, ei saa luua tsooni tipus seada CNAME-kirje. Need piirangud tulenevad DNS-i standardid ja ei kasuta Azure'i DNS-i piirangud.
