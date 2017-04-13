<properties
   pageTitle="Azure'i DNS-i alustamine | Microsoft Azure'i"
   description="Saate teada, kuidas luua DNS-i tsoonid Azure'i DNS-i. See on samm-sammult saada oma esimese DNS-i tsooni loodud alustada oma DNS-i domeeni PowerShelli kaudu."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-a-dns-zone-using-powershell"></a>Looge DNS-i tsooni PowerShelli abil

> [AZURE.SELECTOR]
- [Azure'i portaal](dns-getstarted-create-dnszone-portal.md)
- [PowerShelli](dns-getstarted-create-dnszone.md)
- [Azure'i CLI](dns-getstarted-create-dnszone-cli.md)

See artikkel annab teile juhised DNS-i tsooni loomiseks PowerShelli abil. Saate luua ka DNS-i tsooni CLI või Azure portaali kaudu.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Etags ja siltide kohta

### <a name="etags"></a>Etags

Oletame, et kaks inimest või kaks protsessi proovige muuta DNS-i kirje samal ajal. Millist võitis? Ja ei tea võitja, et need on lihtsalt üle kellegi teise poolt loodud muudatused?

Azure'i DNS-i kasutatakse Etags reageerimine samaaegseid muudatuste sama ressurss turvaline. Iga DNS-i ressursikeskuse (zone või kirjekomplekti) on on seostatud Etag. Iga kord, kui ressursi tuuakse, tuuakse ka selle Etag. Ressursi värskendamisel on teil võimalus edasi tagasi soovitud Etag nii, et Azure'i DNS-i saate kontrollida, kas selle Etag serveri vasteid. Kuna iga värskendamiseks ressursi, on tulemuseks Etag, on uuesti, on Etag lahknevuse näitab samaaegseid muutunud. Etags kasutatakse ka kui loote uue ressursi tagamaks, et ressurss pole veel olemas.

Vaikimisi kasutab Azure DNS-i PowerShelli Etags blokeerida tsoonid samaaegseid muudatused ja salvestada komplektid. Valikuline *-kirjutada* vahetamise saab kasutada mis tahes Etag kontrollid, sel juhul toimunud samaaegseid muutused kirjutatakse.

Azure'i DNS-i REST API tasemel Etags on määratud, kasutades HTTP päised.  Järgmises tabelis on esitatud nende käitumine:

|Päis|Käitumine|
|------|--------|
|Ükski|Pane alati õnnestus (Etag kontroll)|
|IF-match<etag>|Pane ainult õnnestub, kui on olemas ressurss ja Etag vastab|
|IF-match *     | Pane ainult õnnestub, kui ressurss|
|Kui ükski-match * |  Pane ainult õnnestus, kui ressurss pole olemas|

### <a name="tags"></a>Sildid

Siltide erinevad Etags. Sildid on loendi nimi ja väärtuse paarideks ja kasutatakse Azure ressursihaldur selles arvelduse ja rühmitamistasemel sildi ressurssidele. Siltide kohta leiate lisateavet teemast [kasutamine siltide korraldamiseks oma Azure ressursse](../resource-group-using-tags.md).

Azure'i DNS-i PowerShelli toetab sildid nii alad ja suvandite abil määratud kirje komplekti `-Tag` parameeter.


## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et on enne konfiguratsioonile järgmist.

- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).

- Peate Azure ressursihaldur PowerShelli cmdletid (1.0 või uuem versioon) uusima versiooni installimiseks. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

## <a name="step-1---sign-in"></a>Samm 1 - sisselogimine

Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Lisateavet leiate teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md).

Järgmises näites abil saate ühendada:

    Login-AzureRmAccount

Kontrollige konto tellimused.

    Get-AzureRmSubscription

Määrake tellimus, mida soovite kasutada.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Samm 2 - ressursside rühma loomine

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Seda kasutatakse vaikeasukohaks selle ressursi jaotises ressursid. Siiski, kuna kõik DNS-i ressursid on globaalne, mitte piirkondliku, ressursside rühma asukoha valik ei mõjuta Azure'i DNS-i.

Kui kasutate ressursside olemasolevasse rühma, võite selle sammu vahele jätta.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Samm 3 - Register

Azure'i DNS-i teenuse haldab Microsoft.Network ressursi pakkuja. Azure'i tellimuse peab olema registreeritud selle ressursi pakkuja kasutama, enne kui saate kasutada Azure DNS-i. See on ühekordne toiming iga tellimuse jaoks.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Juhis 4 – DNS-i tsooni loomine

DNS-i tsooni on loodud, kasutades funktsiooni `New-AzureRmDnsZone` cmdlet-käsk. Näiteid all loomise DNS-i tsooni või ilma sildid. Siltide kohta leiate lisateavet jaotisest [Sildid](#tags) selle artikli.

>[AZURE.NOTE] Azure'i DNS-i, tuleks täpsustada zone nimed on lõpetamise **"."**. Näiteks kui "**contoso.com**" asemel "**contoso.com.**".

### <a name="to-create-a-dns-zone"></a>DNS-i tsooni loomine

Järgmises näites luuakse DNS-i tsooni, nimega *contoso.com* nimetatakse *MyResourceGroup*ressursirühma. Käesolevas näites abil saate luua DNS-i tsooni, asendades selle väärtused ise.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>DNS-i tsooni siltide loomiseks

Järgmises näites on kujutatud kahe siltidega DNS-i tsooni loomiseks *projekti = demo* ja *keskkonna = test*. Käesolevas näites abil saate luua DNS-i tsooni, asendades selle väärtused ise.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Kirjete vaatamine

DNS-i tsooni loomine loob ka järgmised DNS-i kirjed:

- *Alustamine asutuse* (SOA) kirje. See on iga DNS-i tsooni juurtasemel Esita.

- Autoriteetsete nimeserveri (NS) kirjeid. Need näitavad, millised nimeserverite hosting tsooni. Azure'i DNS-i kasutab kogumi nimeserverite ja nii erinevad nimeserverite ülesandeks võidakse määrata Azure'i DNS-i tsooni. Vaadake lisateavet [volitatud esindaja Azure'i DNS-i domeeni](dns-domain-delegation.md) .

Nende kirjete kuvamiseks kasutage `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Kirje määrab juursait (või *tipp*) DNS-i tsooni kasutamiseks **@** kirje määratud nimi.


## <a name="test"></a>Test

DNS-i tööriistade näiteks nslookup, dig või [lahenda DnsName PowerShelli cmdlet-käsu](https://technet.microsoft.com/library/jj590781.aspx)abil saate oma DNS-i tsooni testida.

Kui te pole veel oma domeeni kasutama uut tsooni Azure'i DNS-i, peate DNS-i päringu otse mõnele oma tsooni nimeservereid suunaks. Nimeserverite oma tsooni NS-kirjed on esitatud loetletud `Get-AzureRmDnsRecordSet` kohal. Kindlasti funktsiooni substitute õigete väärtuste oma tsooni sisse käsu alla.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Järgmised sammud

Pärast DNS-i tsooni loomist Looge [kirje komplekti ja kirjete](dns-getstarted-create-recordset.md) alustamiseks lahendavad nimesid oma Interneti-domeeni.

