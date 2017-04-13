<properties
   pageTitle="DNS-i tsoonid PowerShelli kaudu haldamise | Microsoft Azure'i"
   description="DNS-i tsoonid Azure PowerShelli abil saate hallata. Kuidas värskendada, kustutada ja Azure DNS-i DNS-i tsoonid loomine"
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

# <a name="how-to-manage-dns-zones-using-powershell"></a>Kuidas hallata DNS-i tsoonid PowerShelli abil

> [AZURE.SELECTOR]
- [Azure'i CLI](dns-operations-dnszones-cli.md)
- [PowerShelli](dns-operations-dnszones.md)



Selles artiklis näitab teile, kuidas hallata oma DNS-i tsooni PowerShelli abil. Kasutage neid juhiseid, et peate installige uusim versioon Azure ressursihaldur PowerShelli cmdlet-käskude (1.0 või uuem versioon). Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.


## <a name="create-a-new-dns-zone"></a>Looge uus DNS-i tsooni

DNS-i tsooni loomiseks vaadake teemat [DNS-i tsooni PowerShelli abil loomine](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>DNS-i tsooni hankimine

DNS-i tsooni toomiseks kasutada funktsiooni `Get-AzureRmDnsZone` cmdlet-käsk. See toiming tagastab DNS-i tsooni objekti vastav mõne olemasoleva Azure'i DNS-i tsooni. Objekti sisaldab andmeid tsooni (nt kirje komplektid arv), kuid ei sisalda kirje komplekti, ise.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Loendis DNS-i tsoonid

Jättes selle tsooni nime `Get-AzureRmDnsZone`, mida saate loetleda kõiki alasid ressursirühma. See toiming tagastab massiivi zone objektid.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>DNS-i tsooni värskendamine

DNS-i tsooni ressursi muudatusi saab teha, kasutades `Set-AzureRmDnsZone`. See ei värskendata, mis tahes DNS-i kirje komplekti tsoonis (vt teemat [Kuidas Halda DNS-i kirjed](dns-operations-recordsets.md)). Seda kasutatakse ainult enda zone ressursi atribuutide värskendamine. See on praegu piiratud abil Azure'i ressursihaldur "silte" zone ressursi. Lisateabe saamiseks vaadake [Etags ja sildid](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Kasutage ühte järgmistest Värskenda DNS-i tsooni kaks võimalust:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Määrake tsooni tsooni rühma nimi ja ressursside abil

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Määrake tsooni $zone objekti abil

Määrake $zone objekti abil tsooni `Get-AzureRmDnsZone`. Kui kasutate `Set-AzureRmDnsZone` $zone objekti, kasutatakse Etag kontrollid tagada samaaegseid muudatused üle ei kirjutata. Saate kasutada valikuline *-kirjutada* tõkestamine kontrolli aktiveerimiseks. Lisateabe saamiseks vaadake [Etags ja sildid](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>DNS-i tsooni kustutamine

Eemalda-AzureRmDnsZone cmdlet-käsu abil saate DNS-i tsoonid kustutada.

Enne kustutamist DNS-i tsooni Azure'i DNS-i, peate kustutada kõik kirjed komplekti, välja arvatud NS ja SOA kirjete tsooni juurtasemel automaatselt loodud tsooni loomisel.

Kasutage ühte eemaldamiseks DNS-i tsooni toimetada kahel viisil:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Määrake tsooni tsooni nimi ja ressursside rühma nime abil

See toiming on vabatahtlik *-Jõusta* lüliti, mis takistab kinnitamiseks eemaldada DNS-tsooni.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Määrake tsooni $zone objekti abil

Määrake $zone objekti abil tsooni `Get-AzureRmDnsZone`. See toiming on vabatahtlik *-Jõusta* lüliti, mis takistab kinnitamiseks eemaldada DNS-tsooni. Sarnaselt `Set-AzureRmDnsZone`, $zone objekti abil tsooni määramine lubab Etag kontrolli, et samaaegseid muudatusi ei kustutata. <BR>
Valikuline *-kirjutada* lipp tõkestab kontrolli. Lisateabe saamiseks vaadake [Etags ja sildid](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Samuti võib asemel parameetrina veetorustiku zone objekti:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Järgmised sammud

Pärast DNS-i tsooni loomist Looge [kirje komplekti ja kirjete](dns-getstarted-create-recordset.md) alustamiseks lahendavad nimesid oma Interneti-domeeni.