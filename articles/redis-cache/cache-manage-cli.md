<properties 
    pageTitle="Kuidas luua ja hallata Azure Redis vahemälu Azure'i käsurea liides (Azure'i CLI) abil | Microsoft Azure'i" 
    description="Siit saate teada, kuidas installida Azure CLI mis tahes platvormi, kuidas seda kasutada oma Azure kontoga ühenduse loomiseks ja loomise ja haldamise Azure'i CLI Redis vahemälu." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Kuidas luua ja hallata Azure Redis vahemälu Azure'i käsurea liides (Azure'i CLI) abil

> [AZURE.SELECTOR]
- [PowerShelli](cache-howto-manage-redis-cache-powershell.md)
- [Azure'i CLI](cache-manage-cli.md)

Azure'i CLI on suurepärane viis oma Azure taristu haldamise mis tahes platvormi. Selles artiklis kirjeldatakse, kuidas luua ja hallata Azure Redis vahemälu eksemplaride Azure'i CLI abil.

## <a name="prerequisites"></a>Eeltingimused

Saate luua ja hallata Azure Redis vahemälu eksemplarid Azure'i CLI abil, peate tehke järgmist.

-   Azure'i konto peab teil olema. Kui teil pole ühte, saate luua ainult mõne hetke on [tasuta konto](https://azure.microsoft.com/pricing/free-trial/) .
-   [Installige Azure'i CLI](../xplat-cli-install.md).
-   Installi Azure'i CLI ühendust Azure isikliku kontoga, või töö või kooli Azure'i konto ja sisse logida Azure'i CLI abil soovitud `azure login` käsk. Mõista nende erinevusi ja valida, vt [ühenduse Azure tellimuse: Azure'i käsurea liides (Azure'i CLI)](../xplat-cli-connect.md).
-   Enne kasutusel järgmised käsud, lülitage Azure'i CLI ressursihaldur režiimi käivitades funktsiooni `azure config mode arm` käsk. Lisateavet leiate teemast [Azure ressursihaldur režiim](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis vahemälu atribuudid

Järgmised atribuudid kasutatakse loomine ja värskendamine Redis vahemälu eksemplaris.

| Atribuut            | Üleminek                                  | Kirjeldus                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi                | -n,--nimi                              | Redis vahemälu nimi.                                                                                                                                                                                                                             |
| ressursirühm      | -g,---ressursirühm                    | Ressursirühma nimi.                                                                                                                                                                                                                          |
| asukoht            | -l,--asukoht                          | Asukoht vahemälu luua.                                                                                                                                                                                                                            |
| suurus                | -y,--suurus                              | Redis vahemälu maht. Kehtivad väärtused: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU-ga                 | -x,--sku                               | Redis SKU-ga. Peaks olema üks: [tavaline, Standard, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e,--enable-mitte-ssl-port               | EnableNonSslPort atribuut Redis vahemälu. Selle lipu lisada, kui soovite lubada mitte Port SSL-i vahemälu                                                                                                                                    |
| Redis konfigureerimine | -c,--redis-konfigureerimine               | Redis konfigureerimine. Sisestage vormindatud JSON string konfiguratsiooni võtmed ja väärtused siin. Vorming: "{" ":""," ":" "}"                                                                                                                                     |
| Redis konfigureerimine | -f,--redis konfiguratsioonifail          | Redis konfigureerimine. Sisestage faili, mis sisaldab konfiguratsiooni võtmed ja väärtused siin tee. Faili sisenemise vorming: {"": "","": ""}                                                                                                                |
| Kildu loendamine         | -r,--Kildu-count                       | Arv Shards Premium kobar vahemälu rühmitamise abil luua.                                                                                                                                                                               |
| Virtuaalse võrgu     | -v,--virtuaalse võrk                   | Kui hosting vahemälu on VNET, määrab täpse ARM ressursi ID virtuaalse võrgu juurutamiseks redis vahemälu. Vormingu näide: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| võtme tüüp            | -t,--klahv-tüüpi                          | Klahv pikendamiseks tüüp. Kehtivad väärtused: [esmane, teisene]                                                                                                                                                                                             |
| StaticIP            | -p,--staatiline ip < staatiline ip >             | Hosting vahemälu on VNET, saate määrata kordumatu IP-aadress, alamvõrgu vahemälu. Kui ei esitata, üks valitakse teie eest alamvõrgu.                                                                                                |
| Alamvõrgu              | t,--alamvõrgu<subnet>                    | Kui hosting vahemälu on VNET, saate määrata alamvõrku, kus juurutada vahemälu nimi.                                                                                                                                                    |
| VirtualNetwork      | -v,--virtuaalse võrgu < virtuaalse-võrgu > | Kui hosting vahemälu on VNET, määrab täpse ARM ressursi ID virtuaalse võrgu juurutamiseks redis vahemälu. Vormingu näide: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Tellimuse        | -s,--tellimus                      | Tellimuse ID.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Lugege teemat vahemälu Redis kõik käsud

Kõik Redis vahemälu käsud ja nende parameetrid kuvamiseks kasutage funktsiooni `azure rediscache -h` käsk.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Redis vahemälu luua

Redis vahemälu loomiseks kasutage järgmine käsk:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Lisateavet selle käsu käivitamine on `azure rediscache create -h` käsk.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Olemasoleva Redis vahemälu kustutamine

Redis vahemälu kustutamiseks kasutage järgmine käsk:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Lisateavet selle käsu käivitamine on `azure rediscache delete -h` käsk.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Loetle kõik Redis vahemälu teie tellimus või ressursirühm

Kõik teie tellimus või ressursirühm vahemälu Redis loendis käsuga:

    azure rediscache list [options]

Lisateavet selle käsu käivitamine on `azure rediscache list -h` käsk.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Olemasoleva Redis vahemälu atribuutide kuvamine

Olemasoleva Redis vahemälu atribuutide kuvamiseks kasutage järgmist käsku:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Lisateavet selle käsu käivitamine on `azure rediscache show -h` käsk.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Olemasoleva Redis vahemälu sätete muutmine

Olemasoleva Redis vahemälu sätete muutmiseks kasutage järgmist käsku:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Lisateavet selle käsu käivitamine on `azure rediscache set -h` käsk.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Olemasoleva Redis vahemälu autentimise võti uuendamine

Olemasoleva Redis vahemälu uuendamine autentimise võti, kasutage järgmine käsk:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Määrake `Primary` või `Secondary` jaoks `key-type`.

Lisateavet selle käsu käivitamine on `azure rediscache renew-key -h` käsk.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Olemasoleva Redis vahemälu loendi põhi- ja teisese võtmed

Loendi põhi- ja teisese võtmed Redis olemasoleva vahemälu, kasutage järgmine käsk:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Lisateavet selle käsu käivitamine on `azure rediscache list-keys -h` käsk.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
