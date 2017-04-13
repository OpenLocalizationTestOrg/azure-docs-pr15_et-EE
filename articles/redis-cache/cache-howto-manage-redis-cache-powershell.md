<properties
    pageTitle="Azure'i Redis vahemälu Azure PowerShelli abil hallata | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i Redis vahemälu Azure PowerShelli kaudu haldustoimingute sooritamiseks."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Azure'i Redis vahemälu Azure PowerShelliga haldamine

> [AZURE.SELECTOR]
- [PowerShelli](cache-howto-manage-redis-cache-powershell.md)
- [Azure'i CLI](cache-manage-cli.md)

Selles teemas näete, kuidas teha levinud toiminguid nagu luua, värskendada, ja mastaapimiseks oma Azure'i Redis vahemälu eksemplaris, kuidas taastada Pääsuklahvide ja kuidas saate vaadata teavet oma vahemälu. Azure'i Redis vahemälu PowerShelli cmdlet-käskude täieliku loendi leiate [Azure'i Redis vahemälu cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline mudel](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Eeltingimused

Kui teil on juba installitud Azure PowerShelli, peab teil olema Azure PowerShelli versioon 1.0.0 või uuem versioon. Saate vaadata installitud see käsk Azure PowerShelli käsuviibas Azure PowerShelli versioon.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Esmalt peate sisse logima Azure see käsk.

    Login-AzureRmAccount

Määrake sisselogimise dialoogiboksi Microsoft Azure'i meiliaadress Azure'i konto ja parooli.

Edasi, kui teil on mitu Azure tellimust, peate Azure tellimuse seadmine. Praeguse tellimuste loendi kuvamiseks käivitage see käsk.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Määramaks, et see tellimus, käivitage järgmine käsk. Järgmises näites on tellimuse nime `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Enne kui saate kasutada Windows PowerShelli Azure'i ressursihaldur, vajate järgmist:

- Windows PowerShelli, versiooni 3.0 või 4.0. Tippige Windows PowerShelli versiooni otsimiseks:`$PSVersionTable` ja veenduge, et väärtus `PSVersion` on 3.0 või 4.0. Ühilduvad versiooni installimiseks lugege teemat [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) või [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Mis tahes cmdlet-käsk selle õpetuse näha üksikasjalikku abi saamiseks kasutage cmdlet abi saamiseks.

    Get-Help <cmdlet-name> -Detailed

Näiteks spikri funktsiooni `New-AzureRmRedisCache` cmdlet-käsu tüüp:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Kuidas luua ühendus Azure'i Government Cloud või Azure Hiina pilveteenuses

Vaikimisi on Azure keskkond on `AzureCloud`, mis tähistab globaalne Azure pilveteenuse eksemplari. Ühenduse loomiseks muu eksemplariga, kasutage funktsiooni `Add-AzureRmAccount` käsu selle `-Environment` või -`EnvironmentName` käsurea vahetamise soovitud keskkonda või keskkonna nimi.

Siit leiate loendi saadaval keskkonnas, käivitage selle `Get-AzureRmEnvironment` cmdlet-käsk.

### <a name="to-connect-to-the-azure-government-cloud"></a>Azure'i Government Cloud ühendamiseks

Azure'i Government Cloud ühendamiseks kasutage ühte järgmistest käskudest.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

või

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Vahemälu luua Azure Governmenti pilves, kasutage ühte järgmistest kohtadest.

-   Autoriõiguspõhimõtted Virginia
-   Autoriõiguspõhimõtted Iowa

Azure'i Government Cloud kohta leiate lisateavet teemast [Microsoft Azure'i riigi](https://azure.microsoft.com/features/gov/) - ja [Microsoft Azure'i Government arendaja juhend](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Azure'i Hiina pilveteenuses ühendamiseks

Ühendada pilveteenustega Azure'i Hiina, kasutage ühte järgmistest käskudest.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

või

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Vahemälu luua Azure'i Hiina pilves, kasutage ühte järgmistest kohtadest.

-   Hiina Ida.
-   Hiina Põhja

Azure'i Hiina pilveteenuses kohta leiate lisateavet teemast [AzureChinaCloud Azure, mida käitab 21Vianet Hiinas](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Azure'i Redis vahemälu PowerShelli kasutatakse atribuudid

Järgmine tabel sisaldab atribuudid ja kirjelduste levinumaid parameetrite kui loomise ja haldamise Azure'i Redis vahemälu eksemplaride Azure PowerShelli kaudu.

| Parameetri          | Kirjeldus                                                                                                                                                                                                        | Vaikimisi  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Nimi               | Vahemälu nimi                                                                                                                                                                                                  |          |
| Asukoht           | Vahemälu asukoht                                                                                                                                                                                              |          |
| ResourceGroupName  | Ressursi rühma nimi, kus soovite luua vahemälu                                                                                                                                                                   |          |
| Suurus               | Suurus, vahemälu. Sobivad väärtused on: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Shards loomiseks premium vahemälu loomisel abil rühmitamise lubatud arv. Sobivad väärtused on: 1, 2, 3, 4, 5, 6, 7, 8, 9 ja 10                                                                                                      |          |
| SKU-GA                | Saate määrata SKU vahemälu. Sobivad väärtused on: lihtne, Standard, lisatasu                                                                                                                                         | Standard |
| RedisConfiguration | Saate määrata Redis Otsingukonfiguratsiooni sätetest. Iga sätte kohta täpsema teabe saamiseks vt [RedisConfiguration atribuudid](#redisconfiguration-properties) järgmises tabelis. |          |
| EnableNonSslPort   | Näitab, kas pordi SSL on lubatud.                                                                                                                                                                     | FALSE    |
| MaxMemoryPolicy    | See parameeter on taunitud – kasutage selle asemel RedisConfiguration.                                                                                                                                              |          |
| StaticIP           | Hosting vahemälu on VNET, saate määrata kordumatu IP-aadress, alamvõrgu vahemälu. Kui ei esitata, üks valitakse teie eest alamvõrgu.                                                                                                                     |          |
| Alamvõrgu             | Kui hosting vahemälu on VNET, saate määrata alamvõrku, kus juurutada vahemälu nimi.                                                                                                                  |          |
| VirtualNetwork     | Kui hosting vahemälu on VNET, saate määrata VNET, kus juurutada vahemälu ressursi ID-d.                                                                                                             |          |
| KeyType            | Saate määrata, millist Accessi klahvi, et taastada kiirklahvide uuendamisel. Sobivad väärtused on: esmane, teisene |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration atribuudid

| Atribuut                      | Kirjeldus                                                                                                          | Astme hinnad       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| varundus RDB lubatud            | Kas [andmete püsimine Redis](cache-how-to-premium-persistence.md) on lubatud                                     | Ainult Premium        |
| RDB salvestusruumi-ühendusstring | Ühendusstringi salvestusruumi kontole [Redis andmete püsimine](cache-how-to-premium-persistence.md)       | Ainult Premium        |
| RDB-varundus-sagedus          | Varukoopia sagedus [Redis andmete püsimine](cache-how-to-premium-persistence.md)                               | Ainult Premium        |
| maxmemory reserveeritud            | Konfigureerib [reserveeritud mälu](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) -vahemälu protsesside jaoks | Standard- ja Premium |
| maxmemory-poliitika              | Konfigureerib [väljatõstmine poliitika](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) vahemälu           | Kõik hinnakirjad astme   |
| Teavitage keyspace sündmused        | Konfigureerib [keyspace teatised](cache-configure.md#keyspace-notifications-advanced-settings)                     | Standard- ja Premium |
| räsi max-ziplist kirjed      | Konfigureerib small liitväärtuse andmetüübid [mälu optimeerimine](http://redis.io/topics/memory-optimization)          | Standard- ja Premium |
| räsi-max ziplist väärtus        | Konfigureerib small liitväärtuse andmetüübid [mälu optimeerimine](http://redis.io/topics/memory-optimization)          | Standard- ja Premium |
| Set-max intset-kirjed        | Konfigureerib small liitväärtuse andmetüübid [mälu optimeerimine](http://redis.io/topics/memory-optimization)          | Standard- ja Premium |
| zset max-ziplist kirjed      | Konfigureerib small liitväärtuse andmetüübid [mälu optimeerimine](http://redis.io/topics/memory-optimization)          | Standard- ja Premium |
| zset-max ziplist väärtus        | Konfigureerib small liitväärtuse andmetüübid [mälu optimeerimine](http://redis.io/topics/memory-optimization)          | Standard- ja Premium |
| andmebaasid                     | Konfigureerib andmebaaside arvu. Selle atribuudi saab konfigureerida ainult veebisaidil vahemälu loomine.                          | Standard- ja Premium |

## <a name="to-create-a-redis-cache"></a>Redis vahemälu loomine

Azure'i Redis vahemälu uued eksemplarid luuakse [Uus-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet-käsu abil.

>[AZURE.IMPORTANT] Esimest korda Redis vahemälu luua tellimus, mis on Azure portaalis portaali registreerib funktsiooni `Microsoft.Cache` nimeruum selle tellimuse jaoks. Kui proovite luua esimese Redis vahemälu tellimuse PowerShelli kaudu, peate registreeruma selle nimeruumi järgmise käsuga; näiteks muidu cmdlettide `New-AzureRmRedisCache` ja `Get-AzureRmRedisCache` nurjuda.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `New-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Vaikimisi parameetritega vahemälu luua, käivitage järgmine käsk.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, ja `Location` on vaja parameetreid, kuid ülejäänud on valikulised ja on vaikeväärtused. Eelmise käsu loob Standard SKU Azure'i Redis vahemälu eksemplari määratud nime, asukoha ja ressursirühma, mis on 1 GB suuruseid-SSL Port keelatud.

Premium vahemälu luua, määrake suurus P1 (6 GB – 60 GB), P2 (13 GB – 130 GB) P3 (26 GB – 260 GB) või P4 (53 GB – 530 GB). Rühmitamise lubamiseks määrake Kildu count abil soovitud `ShardCount` parameeter. Järgmine näide loob 3 shards P1 premium vahemälu. P1 premium vahemälu on 6 GB suuruseid ja kuna me määratud kolme shards suuruse on 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Väärtuste määramiseks soovitud `RedisConfiguration` parameeter, ümbritsege väärtused sees `{}` nimega /-väärtuse paarideks, näiteks `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Järgmises näites luuakse standard 1 GB vahemälu koos `allkeys-random` maxmemory poliitika ja keyspace teatised konfigureeritud `KEA`. Lisateavet leiate teemast [Keyspace teatised (Täpsemad sätted)](cache-configure.md#keyspace-notifications-advanced-settings) ja [Maxmemory-poliitika ja maxmemory reserveeritud](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Andmebaaside, vahemälu loomise ajal säte konfigureerimine

Funktsiooni `databases` säte saab konfigureerida ainult vahemälu loomise ajal. Järgmises näites luuakse lisatasu P3 (26 GB) vahemälu cmdlet-käsu [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) abil 48 andmebaasidega.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Lisateavet selle `databases` atribuudi, leiate [Azure'i Redis vahemälu vaikimisi serveri konfiguratsiooni](cache-configure.md#default-redis-server-configuration). Cmdlet-käsu [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) abil vahemälu loomise kohta leiate lisateavet jaotisest eelmise [Redis vahemälu luua](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Redis vahemälu värskendamine

Azure'i Redis vahemälu eksemplarid värskendatakse cmdlet-käsu [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) abil.

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Set-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Funktsiooni `Set-AzureRmRedisCache` cmdlet-käsk saab kasutada näiteks atribuutide värskendamine `Size`, `Sku`, `EnableNonSslPort`, ja `RedisConfiguration` väärtused. 

Järgmine käsk värskendab maxmemory-poliitika nimega myCache Redis vahemälu.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Mastaapimiseks Redis vahemälu

`Set-AzureRmRedisCache`skaala on Azure Redis vahemälu saab kasutada näiteks järgmistel juhtudel kuvatakse `Size`, `Sku`, või `ShardCount` atribuute on muudetud. 

>[AZURE.NOTE]PowerShelli kaudu vahemälu mastaapimist on sama piirides ja juhised nagu mastaapimist vahemälu Azure portaalist. Mida saab skaala eri hinnakirjad taseme järgmiste piirangutega.
>
>-  Te ei saa skaala kaudu hinnakirjad jõudmine madalama hinnakirjad tasandi.
>    -    Te ei saa skaala **Premium** vahemälu allapoole on **Standardne** või **lihtsa** vahemälu.
>    -    Te ei saa skaala **Standard** vahemälust **lihtsa** vahemälu allapoole.
>-  Saab **Standard** vahemälu **lihtsa** vahemälust skaala, kuid ei saa muuta suuruse samal ajal. Kui teil on vaja erineva suuruse, mida saab teha edaspidised skaleerimise toiming soovitud suuruseni.
>-  Ei saa otse, et **Premium** vahemälu **lihtsa** vahemälust skaala. Muudate **tavaline** **Standard** ühe skaleerimise toiminguga, ja seejärel **Standard** **Premium** skaleerimise järgmise toiminguga.
>-  Ei saa muudate suuremaks alla, et selle **C0 (250 MB)** suurus.
>
>Lisateavet leiate teemast [skaala Azure'i Redis vahemälu](cache-how-to-scale.md).

Järgmises näites kujutatakse mastaapimiseks nimega vahemälu `myCache` 2,5 GB vahemälu. Pange tähele, et see käsk töötab nii põhiline või Standard vahemälu.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Kui see käsk on välja antud, tagastatakse vahemälu olekut (sarnaselt helistamine `Get-AzureRmRedisCache`). Pange tähele, et selle `ProvisioningState` on `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Kui skaleerimise toiming on lõpule jõudnud, kuvatakse `ProvisioningState` muutub `Succeeded`. Kui teil on vaja teha edaspidised skaleerimise toimingu muutmine standardseks alates lihtsamatest ja seejärel muutmise suurus, näiteks peate ootama, kuni eelmine toiming on lõpule jõudnud või saate tõrketeate, mis sarnaneb järgmisega.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Redis vahemälu kohta teabe saamiseks

Teavet vahemälu [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet-käsu abil saate tuua.

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Get-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Kõik vahemälu kohta käiva teabe tagastamiseks praeguse tellimuse, käivitage `Get-AzureRmRedisCache` ilma parameetrid.

    Get-AzureRmRedisCache

Tagasi kõik vahemälu teavet mõne kindla ressursirühma, käivitage `Get-AzureRmRedisCache` abil soovitud `ResourceGroupName` parameeter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Teatud vahemälu teabe tagastamiseks käivitada `Get-AzureRmRedisCache` abil soovitud `Name` parameeter, mis sisaldab nime, vahemälu, ja `ResourceGroupName` parameetri ressursirühm, mis sisaldab selle vahemälu.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Kiirklahvide jaoks Redis vahemälu toomiseks

Kiirklahvide jaoks vahemälu toomiseks saate kasutada cmdlet-käsu [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Get-AzureRmRedisCacheKey`, käivitage järgmine käsk.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Vahemälu klahvid toomiseks kõne on `Get-AzureRmRedisCacheKey` cmdlet ja edastama nimi vahemälu ressursirühm, mis sisaldab vahemälu nimi.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Kui soovite taastada kiirklahvide jaoks vahemälu Redis

Kiirklahvide jaoks vahemälu taastada, saate kasutada cmdlet-käsu [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `New-AzureRmRedisCacheKey`, käivitage järgmine käsk.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Vahemälu võti põhi- või taastada, kõne on `New-AzureRmRedisCacheKey` cmdlet ja edastama nimi, ressursirühm, ning määrata, kas `Primary` või `Secondary` jaoks soovitud `KeyType` parameeter. Järgmises näites on sekundaarne Accessi võti vahemälu uuesti.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Redis vahemälu kustutamine

Redis vahemälu kustutamiseks kasutada cmdlet-käsu [Eemalda-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Remove-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Järgmises näites vahemälu nimega `myCache` on eemaldatud.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Importimiseks Redis vahemälu

Andmete importimine Azure'i Redis vahemälu eksemplar kasutab funktsiooni `Import-AzureRmRedisCache` cmdlet-käsk.

>[AZURE.IMPORTANT] Lisaks on saadaval [premium taseme](cache-premium-tier-intro.md) vahemälu ainult impordi/ekspordi. Import/eksport kohta leiate lisateavet teemast [Azure Redis vahemälu andmete importimine ja eksportimine](cache-how-to-import-export-data.md).

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Import-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Järgmine käsk impordi andmed bloobimälu, määratud SAS uri Azure'i Redis vahemälu sisse.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Redis vahemälu ekspordiks

Andmete eksportimine Azure'i Redis vahemälu eksemplar kasutab funktsiooni `Export-AzureRmRedisCache` cmdlet-käsk.

>[AZURE.IMPORTANT] Lisaks on saadaval [premium taseme](cache-premium-tier-intro.md) vahemälu ainult impordi/ekspordi. Import/eksport kohta leiate lisateavet teemast [Azure Redis vahemälu andmete importimine ja eksportimine](cache-how-to-import-export-data.md).

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Export-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Järgmine käsk ekspordib andmed üheks määratud SAS uri ümbris eksemplari Azure'i Redis vahemälu.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Taaskäivitamiseks Redis vahemälu

Azure'i Redis vahemälu eksemplar kasutab taaskäivitamist soovitud `Reset-AzureRmRedisCache` cmdlet-käsk.

>[AZURE.IMPORTANT] Taaskäivitage arvuti kättesaadav ainult [premium taseme](cache-premium-tier-intro.md) vahemälu. Taaskäivitus vahemälu kohta lisateabe saamiseks lugege teemat [vahemälu haldus - arvuti taaskäivitada](cache-administration.md#reboot).

Saadaolevate parameetrite ja nende kirjeldused leiate loendi kuvamiseks `Reset-AzureRmRedisCache`, käivitage järgmine käsk.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Järgmine käsk taaskäivitamisega nii sõlmed määratud vahemälu.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Järgmised sammud

Lisateavet Windows PowerShelli kaudu Azure, leiate järgmistest teemadest:

- [Azure'i Redis vahemälu cmdlet dokumentatsiooni MSDN-is](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure'i ressursihaldur cmdlet-käsud](http://go.microsoft.com/fwlink/?LinkID=394765): saate teada, kuidas kasutada cmdlet-käskude AzureResourceManager moodul.
- [Ressursi kasutamine rühmade haldamiseks oma Azure ressursse](../resource-group-template-deploy-portal.md): saate teada, kuidas luua ja hallata ressursside rühmade Azure'i portaalis.
- [Azure'i ajaveeb](http://blogs.msdn.com/windowsazure): Azure uusi funktsioone tutvustava Õppevideo.
- [Windows PowerShelli ajaveeb](http://blogs.msdn.com/powershell): Windows PowerShelli uusi funktsioone tutvustava Õppevideo.
- ["Kuule, skriptimise mees!" Ajaveebi](http://blogs.technet.com/b/heyscriptingguy/): toomine Windows PowerShelli ühenduse reaalseid nõuandeid ja näpunäiteid.
