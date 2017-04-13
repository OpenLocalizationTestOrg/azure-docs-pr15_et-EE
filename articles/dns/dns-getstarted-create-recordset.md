<properties
   pageTitle="Kirjekomplekti ja PowerShelli kaudu DNS-i tsooni kirjeid luua | Microsoft Azure'i"
   description="Kuidas luua Azure'i DNS-i hosti kirjed. Kirje määrab ja kirjete PowerShelli abil"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Looge DNS-i kirje komplekti ja kirjete PowerShelli abil


> [AZURE.SELECTOR]
- [Azure'i portaal](dns-getstarted-create-recordset-portal.md)
- [PowerShelli](dns-getstarted-create-recordset.md)
- [Azure'i CLI](dns-getstarted-create-recordset-cli.md)

Selles artiklis tutvustatakse kirjeid ja kirjete komplekti loomine Windows PowerShelli abil. Pärast oma DNS-i tsooni loomist lisada oma domeeni DNS-i kirjed. Selle tegemiseks peate esmalt mõista DNS-i kirjed ja kirje komplektid.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Veenduge, et teil on PowerShelli uusim versioon

Veenduge, et olete installinud uusima versiooni Azure ressursihaldur PowerShelli cmdlet-käsud. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

## <a name="create-a-record-set-and-record"></a>Kirjekomplekti ja kirje loomine

Selles jaotises kirjeldatakse, kuidas luua kirjekomplekti ja kirje.


### <a name="1-connect-to-your-subscription"></a>1. ühenduse loomiseks oma tellimuse

Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Järgmises näites abil saate ühendada:

    Login-AzureRmAccount

Kontrollige konto tellimused.

    Get-AzureRmSubscription

Määrake tellimus, mida soovite kasutada.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

PowerShelli töötamise kohta leiate lisateavet teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2 kirjekomplekti loomine

Saate luua kirje komplekti, kasutades funktsiooni `New-AzureRmDnsRecordSet` cmdlet-käsk. Kui loote kirjekomplekti, peate määrama kirje nimi, tsooni, aeg live (TTL) ja kirjetüüpi seadmine.

Tipp on tsooni määramine kirje loomiseks (sel juhul "contoso.com"), kasutage kirje nimi "@", sealhulgas jutumärgid. See on levinud DNS-i mess.

Järgmises näites luuakse kirje määramine suhteline nimega "www" DNS-i tsooni "contoso.com". Nõuetele täielikult vastav kirje määramine nimi on "www.contoso.com". Kirje tüüp on "A" ja TTL on 60 sekundi järel. Pärast seda toimingut, siis on tühi "www" kirjekomplekti muutuv *$rs*määratud.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Kui kirje määratud juba olemas

Kui kirje määramine juba olemas, käsk nurjub, kui selle *-kirjutada* kasutatakse lülitit. Funktsiooni *-kirjutada* suvand käivitab kinnitamise viip, mis võib olla rõhutud abil soovitud *-Jõusta* aktiveerimine.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


Selles näites saate määrata tsooni abil tsooni nimi ja ressursside rühma nime. Teise võimalusena saate määrata zone objekti, nagu on tagastatud `Get-AzureRmDnsZone` või `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Tagastab kohaliku objekti, mis tähistab kirjekomplekti, mis on loodud Azure'i DNS-i.

### <a name="3-add-a-record"></a>3-kirje lisamine

Äsja loodud "www" kirjekomplekti kasutamiseks peate kirjeid lisada. Saate lisada IPv4 *A* kirjed "www"-kirjele määramine, kasutades järgmises näites. Selles näites tugineb muutuv *$rs* eelmises etapis määratavad.

Kirjete lisamine kirje määramine, kasutades `Add-AzureRmDnsRecordConfig` on ühenduseta toiming. Ainult kohaliku muutuja *$rs* värskendatakse.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4 muudatuste kinnitamine

Kinnitage kirjekomplekti soovitud muudatused. Kasutage `Set-AzureRmDnsRecordSet` muudatuste üleslaadimiseks määramine Azure DNS-i kirje.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. toomiseks kirje määramine

Saate tuua abil määrata Azure'i DNS-i kirje `Get-AzureRmDnsRecordSet` nagu on näidatud järgmises näites.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Saate kasutada ka tööriista nslookup või muude DNS-i tööriistade päringu uue kirjekomplekti.

Kui te pole veel delegeeritud nimeserverite Azure'i DNS-i domeeni, peate määrata nimi, serveri ja aadress oma tsooni.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Iga tüüpi kirjekomplekti ühe kirje loomine


Järgmised näited kujutavad kirjekomplekti, kindlat tüüpi kirje loomise kohta. Iga kirje kogum sisaldab üheks kirjeks.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Järgmised sammud

[Kuidas hallata DNS-i tsoonid PowerShelli abil](dns-operations-dnszones.md)

[DNS-i kirjeid hallata ning kirje määrab PowerShelli abil](dns-operations-recordsets.md)

[.NET SDK Azure toimingute automatiseerimine](dns-sdk.md)
