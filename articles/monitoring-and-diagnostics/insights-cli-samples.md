<properties
    pageTitle="Azure'i kuvari CLI Lühijuhend näidised. | Microsoft Azure'i"
    description="Proovi CLI käsud Azure'i kuvari funktsioonide jaoks. Azure'i kuvar on Microsoft Azure'i teenus, mis võimaldab teil saada teatiste, helistage web URL-ide konfigureeritud telemeetria andmed ja autoScale pilveteenustega, Virtuaalmasinates ja veebirakenduste väärtuste põhjal."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure'i kuvari platvormidel CLI Kiire alustamine näidised

Selles artiklis kirjeldatakse proovi käsurea liides (CLI) käsud aitavad Azure kuvari funktsioone. Azure'i jälgimine võimaldab teil AutoScale pilveteenustega, Virtuaalmasinates ja veebirakenduste ja teatiste saatmine või kõne web URL-ide väärtuste konfigureeritud telemeetria andmete põhjal.

>[AZURE.NOTE] Azure'i kuvar on kuni 25 Sept 2016 mis kutsuti "Azure ülevaateid" uus nimi. Siiski soovitud nimeruumid ja seega allpool käsud sisaldavad siiski "ülevaateid".


## <a name="prerequisites"></a>Eeltingimused

Kui te pole seda juba installitud Azure'i CLI, leiate [Azure'i CLI installida](../xplat-cli-install.md). Kui te pole varem selliseid Azure'i CLI, saate lugeda Lisateavet sellest [kasutamine Azure CLI Mac, Linux, ja Windows Azure'i ressursihaldur](../xplat-cli-azure-resource-manager.md).


Klõpsake Windowsi installida npm [Node.js veebisaidi](https://nodejs.org/). Pärast installi lõpuleviimist Käivita administraatorina õigustega CMD.exe abil, käivitada kausta, kuhu on installitud npm järgmist:

```console
npm install azure-cli --global
```

Järgmiseks avage mis tahes kausta/asukohta soovite ja tippige käsurea:

```console
azure help
```

## <a name="log-in-to-azure"></a>Azure'i sisse logida

Esimese sammuna tuleb Azure'i kontosse sisse logida.

```console
azure login
```

Pärast selle käsu teil sisse logida kaudu kuvatavaid juhiseid. Pärast seda, kuvatakse teie konto, TenantId ja vaikimisi Tellimuse ID-ga. Kõik käsud töötavad vaikimisi tellimuse kontekstis.

Praeguse tellimuse üksikasjade loend käsu.

```console
azure account show
```

Töötamine kontekstis muuta mõnda muud tellimust, käsu.

```console
azure account set "subscription ID or subscription name"
```

Azure'i ressursihaldur ja Azure kuvari käskude kasutamiseks peate olema Azure'i ressursihaldur režiimis.

```console
azure config mode arm
```

Kõigi toetatud Azure'i kuvari käskude loendi kuvamiseks tehke järgmist.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Vaate auditilogid tellimusele

Auditilogide vaatamiseks tehke järgmist.

```console
azure insights logs list [options]
```

Proovige kõigi saadaolevate suvandite kuvamiseks järgmist.

```console
azure insights logs list -help
```

Siit leiate loendi logid on resourceGroup, näiteks

```console
azure insights logs list --resourceGroup "myrg1"
```

Loendi logid helistaja näide

```console
azure insights logs list --caller "myname@company.com"
```

Näide loendisse logivad helistaja ressursi tüübi, sees algus ja lõpp kuupäev

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Teatiste töötamine
Saate teavet jaotises teatiste töötamiseks.

### <a name="get-alert-rules-in-a-resource-group"></a>Saada teatis reeglid ressursirühma

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Argumendil teatiste reegli loomine

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Log teatiste reegli loomine

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Lisandmooduli webtest reegli loomine

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Reegli kustutamine

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Profiilid
Teabe kasutamine selles jaotises Profiilid töötamiseks.

### <a name="get-a-log-profile"></a>Logi profiili hankimine

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Lisage logi profiil ilma säilitus.

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Logi profiili eemaldamine

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Logi koos säilituspoliitika profiili lisamine

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Säilitus-ja EventHub Logi profiili lisamine

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostika
Selles jaotises teabe abil diagnostikasätete töötamine.

### <a name="get-a-diagnostic-setting"></a>Saada diagnostika seade

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Diagnostika seade keelamine

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Luba diagnostika seade ilma säilitus.

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Autoscale
Selles jaotises teabe abil töötamine autoscale sätted. Peate muutma need näited.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Saada ressursirühma autoscale sätted

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Saada autoscale sätted ressursirühma nime järgi

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Auotoscale sätete määramine

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
