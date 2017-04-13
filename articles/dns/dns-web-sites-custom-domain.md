<properties
   pageTitle="Luua kohandatud DNS-i kirjete web appi | Microsoft Azure'i  "
   description="Kuidas luua kohandatud domeeni DNS-i kirjeid web appi Azure'i DNS-i abil."
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

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Luua kohandatud domeeni DNS-i kirjete web app

Azure'i DNS-i abil kohandatud domeeni majutada oma web apps. Näiteks loote Azure web app ja soovite kasutajatele juurde ühte kasutades contoso.com või www.contoso.com FQDN.

Selle tegemiseks peate looma kahe kirje:

- Osutage kursoriga contoso.com kirje root "A"
- "CNAME" kirje www nimi, mis viitab A-kirje

Pidage meeles, et web appi A-kirje loomisel Azure A-kirje peab olema käsitsi värskendada kui aluseks oleva IP-aadress web appi muudatuste.

## <a name="before-you-begin"></a>Enne alustamist

Enne alustamist peate esmalt Azure'i DNS-i DNS-i tsooni loomine ja delegaadi sisse oma registraatori Azure'i DNS-i tsooni.

1. DNS-i tsooni loomiseks järgige loomine [DNS-i tsooni](dns-getstarted-create-dnszone.md).
2. DNS-i Azure DNS delegeerida juhised [DNS-i domeeni delegeerimine](dns-domain-delegation.md).

Pärast luua ja delegeerida Azure'i DNS-i, saate luua seejärel kirjed oma kohandatud domeeni.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. oma kohandatud domeeni A-kirje loomine

Vastendage nimi selle IP-aadressi kasutatakse A-kirje. Järgmises näites oleme määramine @ nimega IPv4-aadress A-kirje:

### <a name="step-1"></a>Samm 1

A-kirje loomine ja muutuja $rs määramine

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Samm 2

Varem loodud kirjekomplekti IPv4 väärtuse lisamiseks "@" määratud $rs muutuja abil. Määratud väärtus IPv4 saab oma veebirakenduse IP-aadress.

IP-aadressi otsimine web appi, järgige [kohandatud domeeninime teenuses Azure rakenduse konfigureerimine](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Samm 3

Kinnitage kirjekomplekti soovitud muudatused. Kasutage `Set-AzureRMDnsRecordSet` muudatuste üleslaadimiseks seatud Azure'i DNS-i kirje:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2 oma kohandatud domeeni CNAME-kirje loomine

Kui teie domeeni haldab juba Azure'i DNS-i (vt [DNS-i domeeni delegeerimine](dns-domain-delegation.md), saate kasutada järgmisi näide contoso.azurewebsites.net CNAME-kirje loomiseks.

### <a name="step-1"></a>Samm 1

Avage PowerShelli ja luua uusi CNAME record ja määrata muutuja $rs. Selles näites on määramine kirjetüübi loomiseks CNAME "aega, et live" 600 sekundit DNS-i tsooni nimega "contoso.com".

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Samm 2

Pärast CNAME-kirje määramine on loonud, peate looma alias (pseudonüüm) väärtus, mis web appi punkt.

Eelnevalt määratud muutuv "$rs" abil saate PowerShelli käsu alla web appi contoso.azurewebsites.net jaoks pseudonüümi loomine.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Samm 3

Muutuste, kasutades funktsiooni `Set-AzureRMDnsRecordSet` cmdlet-käsk:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Saate kinnitada kirje on loodud õigesti päringute "www.contoso.com" abil nslookup, nagu allpool näidatud:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Kirje "awverify" veebirakenduste loomine


Kui otsustate kasutada A-kirje oma veebirakenduse, peate läbima ostuõiguse kinnitamise protsessi tagamiseks kohandatud Domeen kuulub teile. Kinnitamine tehakse seda nimega "awverify" teisiti CNAME-kirje loomise abil. See jaotis kehtib ainult kirjed.


### <a name="step-1"></a>Samm 1

Looge kirje "awverify". Alltoodud näites loome "aweverify" kirje jaoks kohandatud domeeni omandiõiguse kinnitamiseks contoso.com.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Samm 2

Kui kirjekomplekti "awverify" on loodud, määrata seadmine alias CNAME-kirje. Järgmises näites, anname määratud pseudonüümi awverify.contoso.azurewebsites.net CNAME-kirje.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Samm 3

Muutuste, kasutades funktsiooni `Set-AzureRMDnsRecordSet cmdlet`, nagu on näidatud allpool käsk.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Järgmised sammud

Järgige [konfigureerida kohandatud domeeninime rakenduse teenuse jaoks](../app-service-web/web-sites-custom-domain-name.md) konfigureerida kohandatud domeeni kasutada oma veebirakenduse.








