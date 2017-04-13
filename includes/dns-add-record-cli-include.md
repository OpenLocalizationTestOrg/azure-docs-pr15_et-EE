#### <a name="create-an-a-record-set-with-single-record"></a>Koos ühe kirje A-kirje loomine

Kirjekomplekti loomiseks kasutage `azure network dns record-set create`. Ressursirühm, tsooni nimi, määrake kirje määrata suhteline nimi, kirjetüüpi ja kellaaeg live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

A loomise järel kirje määramine, lisada IPv4 aadress seadistatud kirje `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Koos ühe kirje AAAA kirje loomine

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Koos ühe kirje CNAME-kirje loomine

CNAME-kirje lubada ainult ühe ühe stringiväärtus.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>MX-kirje koos ühe kirje loomine

Selles näites me kasutame kirje hulga nimi "@" tipus zone MX-kirje loomiseks (sel juhul "contoso.com"). See on tavaline MX-kirjed.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Luua ka NS kirjekomplekti ühe kirje

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Koos ühe kirje PTR-kirje loomine  
Sel juhul "minu-arpa-zone.com' tähistab tähistav IP-vahemiku ARPA tsooni.  Iga selle tsooni määramine PTR-kirje vastab IP-aadress see IP vahemikus.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Koos ühe kirje SRV-kirje loomine

Kui SRV-kirje loomisel tsooni juurkaustas, saate määrata "_service" ja "_protocol" kirje nimi. Ei ole vaja kaasata "@" kirje nimi.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>TXT-kirje koos ühe kirje loomine

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
