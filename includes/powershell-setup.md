<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Kuidas häälestada PowerShelli

Enne Azure PowerShelli kasutamiseks tehke järgmist.

### <a name="verify-powershell-versions"></a>Veenduge, et PowerShelli versioonid

Windows PowerShelli kasutamiseks peab olema Windows PowerShelli, versiooni 3.0 või 4.0. Windows PowerShelli versiooni leidmiseks tippige see käsk Windows PowerShelli käsuviibas.

    $PSVersionTable

Peaksite nägema umbes selline.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Veenduge, et **PSVersion** väärtus 3.0 või 4.0. Ühilduvad versiooni installimiseks lugege teemat [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) või [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Teil peab olema ka Azure PowerShelli versiooni 0.8.0 või uuem versioon. Saate vaadata installitud see käsk Azure PowerShelli käsuviibas Azure PowerShelli versioon.

    Get-Module azure | format-table version

Peaksite nägema umbes selline.

    Version
    -------
    0.8.16.1

Juhiseid ja lingi uusim versioon, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Saate häälestada Azure'i konto ja tellimuse

Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

Avage see käsk leiate Azure'i PowerShelli käsuviiba ning Azure'i sisse logida.

    Add-AzureAccount

Kui teil on mitu Azure tellimust, saate selle käsuga Azure tellimuste nimekirja.

    Get-AzureSubscription

Saate teavet järgmist tüüpi:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Saate praeguse Azure tellimuse käivitades Azure PowerShelli käsuviibas järgmised käsud. Asendage kõik hinnapakkumised, sh selle < ja > märkide õige nimega.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Azure'i tellimused ja kontode kohta leiate lisateavet teemast [kohta: tellimuse ühenduse](powershell-install-configure.md#Connect).
