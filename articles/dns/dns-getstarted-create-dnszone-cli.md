<properties
   pageTitle="Looge DNS-i tsooni kaudu CLI | Microsoft Azure'i"
   description="Saate teada, kuidas luua Azure'i DNS-i DNS-i tsoonid samm-sammult oma DNS-i domeeni CLI kasutamise alustamiseks"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Azure'i DNS-i tsooni, mis abil CLI loomine


> [AZURE.SELECTOR]
- [Azure'i portaal](dns-getstarted-create-dnszone-portal.md)
- [PowerShelli](dns-getstarted-create-dnszone.md)
- [Azure'i CLI](dns-getstarted-create-dnszone-cli.md)


Selles artiklis juhendab teid juhised DNS-i tsooni loomiseks CLI abil. Saate luua ka DNS-i tsooni PowerShelli või Azure portaali.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Enne alustamist

Need juhised kasutada Microsoft Azure'i CLI. Veenduge, et uusim Azure CLI värskendamine (0.9.8 või uuem versioon) Azure'i DNS-i käske kasutada. Tippige `azure -v` kontrollida, kas teie arvutisse praegu installitud Azure'i CLI versiooni.

## <a name="step-1---set-up-azure-cli"></a>Samm 1 - Azure'i CLI häälestamine

### <a name="1-install-azure-cli"></a>1. installige Azure'i CLI

Saate installida Windows Azure'i CLI, Linux või Mac-arvutisse Järgmised toimingud tuleb lõpule viia, enne kui saate hallata Azure DNS-i abil Azure'i CLI. Lisateave on saadaval veebisaidil [installida Azure CLI](../xplat-cli-install.md). DNS-i käsud nõuavad Azure'i CLI versioon 0.9.8 või uuem versioon.

Võrgu kõik pakkuja käsud CLI võib leida, kasutades järgmist käsku:

    azure network

### <a name="2-switch-cli-mode"></a>2 switch CLI režiim

Azure'i DNS-i kasutatakse Azure ressursihaldur. Veenduge, et vahetate CLI režiimi ARM käske kasutada.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. oma Azure'i kontosse sisselogimine

Teil palutakse autentimiseks mandaat. Pidage meeles, et saate kasutada ainult ORGID kontod.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Valige see tellimus

Valida Azure tellimuste kasutada.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. ressursside rühma loomine

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Seda kasutatakse vaikeasukohaks selle ressursi jaotises ressursid. Siiski, kuna kõik DNS-i ressursid on globaalne, mitte piirkondliku, ressursside rühma asukoha valik ei mõjuta Azure'i DNS-i.

Kui kasutate ressursside olemasolevasse rühma, võite selle sammu vahele jätta.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. register

Azure'i DNS-i teenuse haldab Microsoft.Network ressursi pakkuja. Azure'i tellimuse peab olema registreeritud selle ressursi pakkuja kasutama, enne kui saate kasutada Azure DNS-i. See on ühekordne toiming iga tellimuse jaoks.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Samm 2 – DNS-i tsooni loomine

DNS-i tsooni on loodud, kasutades funktsiooni `azure network dns zone create` käsk. Soovi korral saate luua DNS-i tsooni koos sildid. Sildid on loendi nimi ja väärtuse paarideks ja kasutatakse Azure ressursihaldur selles arvelduse ja rühmitamistasemel sildi ressurssidele. Siltide kohta leiate lisateavet teemast [kasutamine siltide korraldamiseks oma Azure ressursse](../resource-group-using-tags.md).

Azure'i DNS-i, tuleks täpsustada zone nimed on lõpetamise **"."**. Näiteks kui "**contoso.com**" asemel "**contoso.com.**".


### <a name="to-create-a-dns-zone"></a>DNS-i tsooni loomine

Järgmises näites luuakse DNS-i tsooni, nimega *contoso.com* nimetatakse *MyResourceGroup*ressursirühma.

Käesolevas näites abil saate luua oma DNS-i tsooni, asendades selle väärtused ise.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>DNS-i tsooni ja siltide loomiseks.

Azure'i DNS-i CLI toetab sildid DNS-tsooni määratud, kasutades valikuline *-silt* parameeter. Järgmises näites kujutatakse Loo DNS-i tsooni kaks sildid, projekti = demo ja keskkonna = test.

Järgmises näites abil saate luua DNS-i tsooni ja sildid, asendades selle väärtused ise.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Kirjete vaatamine

DNS-i tsooni loomine loob ka järgmised DNS-i kirjed:

- "Käivitamine, asutus" (SOA) kirje. See on iga DNS-i tsooni juurtasemel Esita.

- Autoriteetsete nimeserveri (NS) kirjeid. Need näitavad, millised nimeserverite hosting tsooni. Azure'i DNS-i kasutab kogumi nimeserverite ja nii erinevad nimeserverite saab määrata erinevaid alasid Azure'i DNS-i. Lisateavet leiate [Azure'i DNS-i domeeni volitatud esindaja](dns-domain-delegation.md) .

Nende kirjete kuvamiseks kasutage `azure network dns-record-set show`.<BR>
*Kasutamine: network DNS-i kirje seadistatud Kuva < ressursirühm >< DNS-tsooni nimi > <name><type>*


Järgmises näites, kui käivitate selle käsu abil ressursi rühma *myresourcegroup*, kirje nime seadmiseks *"@"* (jaoks juurkausta kirje), ja tippige *SOA*, see toovad järgmine väljund:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Vaatamiseks NS-kirjed loodud tsooni, kasutage järgmist käsku:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Kirje määrab juursait (või *tipp*) DNS-i tsooni kasutamiseks **@** kirje määratud nimi.

## <a name="test"></a>Test

Nslookup DIG, nt DNS-i tööriistade abil saate testida oma DNS-i tsooni või `Resolve-DnsName` PowerShelli cmdlet-käsk.

Kui te pole veel oma domeeni kasutama uut tsooni Azure'i DNS-i, peate DNS-i päringu otse mõnele oma tsooni nimeservereid suunamiseks. Nimeserverite oma tsooni on esitatud NS-kirjed loetletud "azure võrgu DNS-i kirje seadistatud show" kohal. Kindlasti funktsiooni substitute õigete väärtuste oma tsooni käsu alla.

Järgmises näites kasutatakse DIG päringu oma domeeni contoso.com DNS-tsooni määratud nimeserverite abil. Päringus on serveri nimi, mille kasutasime osutamiseks * @ * ja tsooni nimi DIG abil.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Järgmised sammud

Pärast DNS-i tsooni loomist Looge [kirje komplekti ja kirjete](dns-getstarted-create-recordset-cli.md) alustamiseks lahendavad nimesid oma Interneti-domeeni.
