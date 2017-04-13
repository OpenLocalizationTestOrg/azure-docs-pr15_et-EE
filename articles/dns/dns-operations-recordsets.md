<properties
   pageTitle="DNS-i kirje komplekti ja kirjete haldamiseks Azure'i portaalis | Microsoft Azure'i"
   description="Haldamise DNS-i kirje määrab ja salvestab Azure'i DNS-i kui oma domeeni Azure'i DNS-i. Kõik PowerShelli käske toiminguid kirje komplekti ja kirjeid."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>DNS-i kirjeid hallata ning kirje määrab PowerShelli abil



> [AZURE.SELECTOR]
- [Azure'i portaal](dns-operations-recordsets-portal.md)
- [Azure'i CLI](dns-operations-recordsets-cli.md)
- [PowerShelli](dns-operations-recordsets.md)



Selles artiklis kirjeldatakse, kuidas kirje komplekti ja oma DNS-i tsooni kirjeid hallata Windows PowerShelli abil.

See on oluline erinevus DNS-i kirje komplekti ja üksikute DNS-i kirjed. Kirje on tsoonis kirjed, mis on sama nime ja on sama tüüpi kogum. Lisateabe saamiseks vt [loomine DNS-i kirje komplekti ja kirjete Azure portaali kaudu](dns-getstarted-create-recordset-portal.md).

Kirje komplekti ja kirjete haldamiseks peate Azure'i ressursihaldur PowerShelli cmdlet-käskude uusim versioon. Lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). PowerShelli töötamise kohta leiate lisateavet teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Uue kirjekomplekti ja kirje loomine

PowerShelli abil määrata kirje loomiseks vaadake teemat [loomine DNS-i kirje komplekti ja kirjete PowerShelli abil](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Kirjekomplekti hankimine

Mõne olemasoleva kirjekomplekti toomiseks kasutada `Get-AzureRmDnsRecordSet`. Määrake kirje määramine suhteline nimi, kirjetüüpi ja tsooni.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Sarnaselt `New-AzureRmDnsRecordSet`, kirje nimi peab olema suhteline nimi, mis tähendab, et see ei tohi sisaldada tsooni nimi.

Saate määrata tsooni abil tsooni nimi ja ressursside rühma nime või kasutades zone objekti:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Tagastab kohaliku objekti, mis tähistab kirjekomplekti, mis on loodud Azure'i DNS-i.

## <a name="list-record-sets"></a>Loendi kirje komplektid

Saate kasutada ka`Get-AzureRmDnsRecordSet` loendi kirje failikogumitele kui argument on *– nimi* ja/või *– RecordType* parameetrid.

### <a name="to-list-all-record-sets"></a>Loendi kirje kõigis kogumites

Selles näites tagastab kirje komplekti, olenemata nime või kirje tüüp:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Loendi kirje failikogumitele antud kirje tüüp

Selles näites tagastatakse kõik kirje komplekti, mis vastavad antud kirjetüüpi. Sel juhul tagastatud kirje määramine, mis on "A" kirjed:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Tsooni saab määrata, kas *– ZoneName* ja *– ResourceGroupName* parameetrid (nagu on näidatud) abil või määrates zone objekti:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Kirjekomplekti kirje lisamine

Saate lisada kirjeid kirje komplekti abil soovitud `Add-AzureRmDnsRecordConfig` cmdlet-käsk. See on ühenduseta toimingu. Ainult kohaliku objekti, mis tähistab kirjekomplekti muudetakse.

Kirjete lisamine kirjekomplekti Parameetrid erinevad olenevalt Kirjekomplekti tüüp. Näiteks kui kasutate Kirjekomplekti tüüp "A", saate ainult määrata kirjete parameetriga *-IPv4Address*.

Täiendavad kirjed lisatakse iga kirje määratud täiendavad kõned `Add-AzureRmDnsRecordConfig`. Saate lisada mis tahes kirjekomplekti kuni 20 kirjed. Siiski record type "CNAME" komplekti võib sisaldada ühe kirje ja kirjekomplekti ei tohi sisaldada kaks identse kirjet. Tühi kirje komplektid (koos null-kirjed) saab luua, kuid ei kuvata serverites Azure'i DNS-i nimi.

Pärast kirje kogum sisaldab kirjete soovitud kogum, peate selle kinnitamiseks, kasutades funktsiooni `Set-AzureRmDnsRecordSet` cmdlet-käsk. Kui kirjekomplekti on kinnitatud, selle asendab olemasoleva kirje määramine Azure DNS-i.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Koos ühe kirje A-kirje loomiseks

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Kirje loomiseks toimingute jada võib olla ka *veetorustiku*, s.t kirje määramine objekti edastatakse toru kasutamise asemel saata see parameetrina. Näiteks:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Täiendavad kirjetüübiga näited

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Olemasoleva kirje komplekti muutmine

Juhised muutmine mõne olemasoleva kirjekomplekti sarnanevad sammud kirjete loomisel. Tegevuste järjestuse on järgmine:

1.  Olemasoleva kirje abil saate tuua `Get-AzureRmDnsRecordSet`.

2.  Kirjete lisamine, eemaldamine kirjed, kirje parameetrite muutmine määratud kirje muuta, või muutmise kirje seadke time to live (TTL). See on ühenduseta toimingu. Ainult kohaliku objekti, mis tähistab kirjekomplekti muudetakse.

3.  Muudatuste kinnitamiseks, kasutades funktsiooni `Set-AzureRmDnsRecordSet` cmdlet-käsk. See asendab olemasoleva kirje määramine Azure DNS-i.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Mõne olemasoleva kirjekomplekti kirje värskendamiseks

Selles näites me muuta IP-aadressi olemasoleva kirje "A":

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Funktsiooni `Set-AzureRmDnsRecordSet` cmdlet-käsk kasutab etag kontrollid, et tagada, et samaaegseid muudatused üle ei kirjutata. Kasutage funktsiooni *-kirjutada* lipp tõkestamine kontrolli. Lisateabe saamiseks lugege [etags ja sildid](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>SOA kirje muutmiseks

Te ei saa lisamine või eemaldamine kirjed automaatselt loodud SOA kirje tsooni tipus määramine (nime = "@"). Siiski saate muuta parameetrid SOA kirjes (välja arvatud "Host") ja kirje Määrake TTL.

Järgmises näites kujutatakse *e-posti* atribuudi SOA kirje:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-kirjed tipus tsooni muutmise

Ei saa lisada, eemaldada või muuta automaatselt loodud NS kirjekomplekti tipus tsooni kirjeid (nime = "@"). Mis on lubatud ainult muudatus on muuta kirjekomplekti TTL.

Järgmises näites kujutatakse TTL atribuudi NS kirjekomplekti:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Kirjete lisamiseks mõne olemasoleva kirjekomplekti

Selles näites lisame kaks täiendavad MX-kirjed olemasoleva kirje määramine:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Mõne olemasoleva kirjekomplekti kirje eemaldamine

Kirjete eemaldamine abil kirje `Remove-AzureRmDnsRecordConfig`. Kirje, mis on eemaldatud peab olema täpne vaste koos olemasoleva kirje kõik parameetrid. Muudatused tuleb toime abil `Set-AzureRmDnsRecordSet`.

Viimase kirje eemaldamine kirjekomplekti ei Kustuta kirje määramine. Lisateavet leiate allpool [kirjekomplekti kustutamine](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Kirje eemaldamiseks kirjekomplekti toimingute jada võib ka veetorustiku, s.t kirje määramine objekti edastatakse toru kasutamise asemel saata see parameetrina. Näiteks:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Kirjekomplekti AAAA kirje eemaldamine

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Kirjekomplekti CNAME-kirje eemaldamine

Kuna CNAME kirjekomplekti võib sisaldada ühe kirje, eemaldades seda kirjet jätab mõnda tühja kirjekomplekti.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Kirjekomplekti MX-kirje eemaldamine

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Kirjekomplekti NS-kirje eemaldamine

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Kirjekomplekti SRV-kirje eemaldamine

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT-kirje eemaldamine kirjekomplekti

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Kirje kustutamine

Kirje komplekti saab kustutada, kasutades funktsiooni `Remove-AzureRmDnsRecordSet` cmdlet-käsk. Te ei saa kustutada SOA ja NS-kirje, mis määrab tsooni tipus (nime = "@") loodud automaatselt tsooni loomisel. Need kustutatakse automaatselt, kui tsooni kustutatakse.

Kasutage ühte järgmised kolm võimalust kirjekomplekti eemaldada.

### <a name="specify-all-the-parameters-by-name"></a>Määrake kõigi parameetrite nime järgi

Valikuline *-Jõusta* parameetrit saab kasutada mis tahes Kinnitusviiba kuvamisel.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Määrake kirjekomplekti nimi ja tüüp ja määrata tsooni objekt

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Määrake määratud objekti kirje

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Kui määrate kirje objekti abil määrata, siis see võimaldab etag kontrolli, et samaaegseid muudatusi ei kustutata. Valikuline *-kirjutada* lipp tõkestab kontrolli. Lisateabe saamiseks vaadake [Etags ja sildid](dns-getstarted-create-dnszone.md#tagetag) .

Kirje määramine objekti saab ka veetorustiku asemel parameetrina:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Järgmised sammud

Azure'i DNS-i kohta leiate lisateavet teemast [Azure DNS-i ülevaade](dns-overview.md). Automatiseerimise DNS-i kohta leiate teavet teemast [DNS-i loomise alad ja kirje määrab .NET SDK abil](dns-sdk.md).

Vastupidise DNS-i kirjete kohta lisateabe saamiseks vaadake, [Kuidas hallata oma teenuste PowerShelli kaudu vastupidise DNS-i kirjeid](dns-reverse-dns-record-operations-ps.md).
