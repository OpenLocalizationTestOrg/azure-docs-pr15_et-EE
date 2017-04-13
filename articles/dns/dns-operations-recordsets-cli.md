<properties
   pageTitle="DNS-i kirje komplekti ja kasutades Azure CLI Azure'i DNS-i kirjete haldamine | Microsoft Azure'i"
   description="Haldamise DNS-i kirje määrab ja salvestab Azure'i DNS-i kui oma domeeni Azure'i DNS-i. Kõik CLI käsud toimingute kirje komplekti ja kirjeid."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>DNS-i kirjeid hallata ja kirje määrab CLI abil


> [AZURE.SELECTOR]
- [Azure'i portaal](dns-operations-recordsets-portal.md)
- [Azure'i CLI](dns-operations-recordsets-cli.md)
- [PowerShelli](dns-operations-recordsets.md)


Selles artiklis kirjeldatakse, kuidas kirje komplekti ja oma DNS-i tsooni kirjeid hallata platvormidel Azure käsurea liides (CLI) abil.

See on oluline erinevus DNS-i kirje komplekti ja üksikute DNS-i kirjed. Kirjekomplekti on tsoonis kirjed, mis on sama nime ja on sama tüüpi kogum. Lisateabe saamiseks vt [mõistmine kirje komplekti ja kirjeid](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Mitu platvormi Azure'i CLI konfigureerimine

Azure'i DNS-i on ainult ressursihaldur Azure'i teenus. See ei ole Azure'i teenuse haldamine API. Veenduge, et Azure'i CLI on konfigureeritud kasutama ressursihaldur režiimi, kasutades funktsiooni `azure config mode arm` käsk.

Kui näete **tõrge: "DNS-i" pole käsu käivitava azure**, on tõenäoline, kuna kasutate Azure CLI Azure'i Teenusehaldus režiimis, mitte ressursihaldur režiim.

## <a name="create-a-new-record-set-and-record"></a>Uue kirjekomplekti ja kirje loomine

Azure'i portaalis määrata uue kirje loomiseks vaadake teemat [kirjekomplekti ja kirjete loomine](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Saate tuua kirjekomplekti

Mõne olemasoleva kirjekomplekti toomiseks kasutada `azure network dns record-set show`. Ressursirühm, tsooni nimi, määrake kirje määrata suhteline nime ja tüüpi kirje. Kasutage näide all, asendades väärtused ise.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Loendi kirje komplektid

Saate loetleda kõiki kirjeid DNS-i tsooni, kasutades funktsiooni `azure network dns record-set list` käsk. Peate määrama ressursi rühma nime ja ajavööndi nimi.

### <a name="to-list-all-record-sets"></a>Loendi kirje kõigis kogumites

Selles näites tagastab kirje komplekti, olenemata nime või kirje tüüp:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Loendi kirje failikogumitele antud tüüp

Selles näites tagastatakse kõik kirje komplekti, mis vastavad antud kirje tüüp (kirjetes, praegusel juhul "A"):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Kirjekomplekti kirje lisamine

Saate lisada kirjeid kirje komplekti abil soovitud `azure network dns record-set add-record`käsk. Kirjete lisamine kirjekomplekti Parameetrid erinevad sõltuvalt kirje, mis seatakse tüüp. Näiteks kui kasutate Kirjekomplekti tüüp "A", saate ainult määrata kirjete parameetriga `-a <IPv4 address>`.

Kirjekomplekti loomiseks kasutage funktsiooni `azure network dns record-set create`käsk. Ressursirühm, tsooni nimi, määrake kirje määrata suhteline nimi, kirjetüüpi ja kellaaeg live (TTL). Kui soovitud `--ttl` parameeter pole määratud, on neli vaikimisi väärtuse (sekundites).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Pärast "A" kirjekomplekti loomist lisage IPv4 aadress, kasutades funktsiooni `azure network dns record-set add-record`käsk.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Järgmised näited kujutavad kirjekomplekti, kindlat tüüpi kirje loomise kohta. Iga kirje kogum sisaldab üheks kirjeks.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Kirje kirjekomplekti värskendamine

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Määrata mõne muu IP-aadress (1.2.3.4) lisamiseks olemasoleva kirje "A" ("www").

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Kirjekomplekti olevat väärtust eemaldamine
Kasutage `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Kirje eemaldamine kirjekomplekti

Kirjete eemaldamine abil kirje `azure network dns record-set delete-record`. Kirje, mis on eemaldatud peab olema täpne vaste koos olemasoleva kirje kõik parameetrid.

Viimase kirje eemaldamine kirjekomplekti ei Kustuta kirje määramine. Lisateavet leiate jaotisest käesoleva artikli nimega [kirjekomplekti kustutamine](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Kirjekomplekti AAAA kirje eemaldamine

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Kirjekomplekti CNAME-kirje eemaldamine

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Kirjekomplekti MX-kirje eemaldamine

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Kirjekomplekti NS-kirje eemaldamine

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Kirjekomplekti PTR-kirje eemaldamine
Sel juhul "minu-arpa-zone.com' tähistab tähistav IP-vahemiku ARPA tsooni.  Iga selle tsooni määramine PTR-kirje vastab IP-aadress see IP vahemikus.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Kirjekomplekti SRV-kirje eemaldamine

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT-kirje eemaldamine kirjekomplekti

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Kirje kustutamine

Kirje komplekti saab kustutada, kasutades funktsiooni `Remove-AzureRmDnsRecordSet` cmdlet-käsk. Te ei saa kustutada SOA ja NS-kirje, mis määrab tsooni tipus (nime = "@") loodud automaatselt tsooni loomisel. Need kustutatakse automaatselt, kui tsooni kustutatakse.

Järgmises näites, eemaldatakse "A" määramine "testi a" kirje "contoso.com" DNS-i tsooni:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Valikuline *– q* parameetrit saab kasutada mis tahes Kinnitusviiba kuvamisel.


## <a name="next-steps"></a>Järgmised sammud

Azure'i DNS-i kohta leiate lisateavet teemast [Azure DNS-i ülevaade](dns-overview.md). Automatiseerimise DNS-i kohta leiate teavet teemast [DNS-i loomise alad ja kirje määrab .NET SDK abil](dns-sdk.md).

Kui soovite töötada vastupidise DNS-i kirjeid, vaadake, [Kuidas hallata vastupidise DNS-i kirjete abil Azure'i CLI teenused](dns-reverse-dns-record-operations-cli.md).
