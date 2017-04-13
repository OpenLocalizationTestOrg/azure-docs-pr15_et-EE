<properties
    pageTitle="Ülevaade: Azure'i Diagnostikalogid | Microsoft Azure'i"
    description="Siit saate teada, mis on Azure Diagnostikalogid ja kuidas saate neid kasutada, on Azure ressurss toimuvate sündmuste mõista."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Azure'i Diagnostikalogid ülevaade
**Azure'i Diagnostikalogid** on logid kiiratava ressursi selle ressursi toimimise kohta rikkalike, sagedased andmetega varustavate. Need logid sisu muutub ressursside tüübi järgi (nt Windows system sündmuselogide on üks kategooria diagnostika log VMs ja bloobimälu ja tabeli jaoks ja järjekorda logid kategooriate Diagnostikalogid salvestusruumi kontode jaoks) ja selle [tegevuse Log (varem auditilogi või funktsionaalseid Log)](monitoring-overview-activity-logs.md), mis annab ülevaate toimingud, mis olid läbi ressursside tellimuse erineda. Kõik ressursid toetavad uut tüüpi siin kirjeldatud Diagnostikalogid. Allpool toetatud teenuste loend näitab, milliste ressursside toetavad uue Diagnostikalogid.

![Diagnostikalogide loogiline paigutamine](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Mida saab teha Diagnostikalogid
Järgnevalt mõned asjad, mida saate teha Diagnostikalogid.

- Salvestage need **Salvestusruumi konto** Auditeerimispoliitika või käsitsi kontrollimiseks. Säilituspoliitika aeg (päevades) **Diagnostika sätete**abil saate määrata.
- Kolmanda osapoole teenusele või kohandatud analytics lahenduse, nt PowerBI manustamisest [voona neid **Sündmuse jaoturi** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) .
- Need [OMS Log](../log-analytics/log-analytics-azure-storage-json.md) Analytics analüüs

## <a name="diagnostic-settings"></a>Diagnostikasätete
Diagnostikalogide-Arvuta ressursid on konfigureeritud diagnostika sätete abil. Ressursi juhtelemendi **Diagnostikasätete** :

- Kui Diagnostikalogid saadetakse (salvestusruumi konto, sündmuse jaoturi või OMS Log Analytics).
- Log kategooriate saadetakse.
- Kui kaua tuleks igas kategoorias log säilitada salvestusruumi konto – säilitamine null päeva tähendab, et logifailid terve igavik. Muul juhul seda väärtust saate vahemikus 1 kuni 2147483647. Kui Säilituspoliitikad on loonud, kuid logide salvestamise salvestusruumi konto on keelatud (nt kui sündmus jaoturi või OMS suvandid on valitud ainult), säilituspoliitikate ei mõjuta.

Neid sätteid konfigureeritakse hõlpsalt diagnostika tera ressursi Azure portaali kaudu või kaudu [Azure'i kuvari REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)kaudu Azure PowerShelli ja CLI käsud.

> [AZURE.WARNING] Diagnostikalogid ja mõõdikute Arvuta ressursid (nt VMs või teenuse struktuuri) kasutada [eraldi süsteemi konfiguratsioon valiku väljundeid](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Kuidas lubada saidikogumi Diagnostikalogid.
Diagnostikalogide kogum saab lubada osana luua ressursi või ressurss on loodud ressursi blade portaali kaudu. Mis tahes hetkel Azure PowerShelli või CLI käskude abil või Azure kuvari REST API abil saate lubada Diagnostikalogid.

> [AZURE.TIP] Need juhised võib pöörduda otse iga ressursi. Vaadake skeemi linke mõista teisiti samme, mis võivad teatud ressursi selle lehe allservas.

[Selles artiklis näidatakse, kuidas saate kasutada malli ressursi lubamiseks Diagnostikasätete ressursi loomisel](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Luba Diagnostikalogid portaalis
Saate lubada Diagnostikalogid Arvuta ressursi tüüpi loomisel, võimaldades Windowsi või Linuxi Azure'i diagnostika laiend Azure'i portaalis.

1.  Avage **Uus** ja valige olete huvitatud ressursi.
2.  Pärast lihtsa sätete konfigureerimine ja valides **sätted** labale jaotises **jälgimine**, suurus, valige **lubatud** ja salvestusruumi konto, kuhu soovite salvestada selle Diagnostikalogid. Kui saadate diagnostika salvestusruumi konto teilt tasu tavaline andmete määr salvestusruumi ja tehingud.

    ![Luba Diagnostikalogid ressursi loomise ajal](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Klõpsake nuppu **OK** ja looge ressurss.

Arvuta ressursid, saate lubada Diagnostikalogid Azure'i portaalis pärast ressursi loomist, tehes järgmist:

1.  Minge tera ressursi ja avage **diagnostika** tera.
2.  **Klõpsake** ja valige salvestusruumi konto ja/või sündmuse jaoturi.

    ![Luba Diagnostikalogid pärast ressursside loomine](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  Valige jaotises **logid**, **Logi kategooriad** soovite koguda või taustal alla laadida.
4.  Klõpsake nuppu **Salvesta**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Luba Diagnostikalogid PowerShelli kaudu
Diagnostikalogide Azure PowerShelli cmdlet-käskude kaudu lubamiseks kasutada järgmisi käske.

Luba säilitamine Diagnostikalogid salvestusruumi konto, kasutage järgmist käsku:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

Salvestusruumi konto ID on salvestusruumi konto, kuhu soovite logid saata ressursi ID-d.

Diagnostikalogide sündmuse jaoturiga voogesituse lubamiseks kasutage järgmist käsku:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Teenuse siini reegel ID on string, mis on selles vormingus: `{service bus resource ID}/authorizationrules/{key name}`.

Diagnostikalogid Log Analytics tööruumi saatmise lubamiseks kasutage järgmist käsku:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] Parameetri WorkspaceId pole saadaval versioonis oktoober. Muutub kättesaadavaks November väljaanne.

Saate hankida oma Log Analytics tööruumi ID Azure'i portaalis.

Saate ühendada need parameetrid lubada mitme Väljastussuvandid.

### <a name="enable-diagnostic-logs-via-cli"></a>Luba Diagnostikalogid CLI kaudu
Diagnostikalogide Azure CLI kaudu lubamiseks saate kasutada järgmisi käske:

Luba säilitamine Diagnostikalogid salvestusruumi konto, kasutage järgmist käsku:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

Salvestusruumi konto ID on salvestusruumi konto, kuhu soovite logid saata ressursi ID-d.

Diagnostikalogide sündmuse jaoturiga voogesituse lubamiseks kasutage järgmist käsku:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Teenuse siini reegel ID on string, mis on selles vormingus: `{service bus resource ID}/authorizationrules/{key name}`.

Diagnostikalogid Log Analytics tööruumi saatmise lubamiseks kasutage järgmist käsku:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] Parameetri workspaceId pole saadaval versioonis oktoober. Muutub kättesaadavaks November väljaanne.

Saate hankida oma Log Analytics tööruumi ID Azure'i portaalis.

Saate ühendada need parameetrid lubada mitme Väljastussuvandid.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Luba Diagnostikalogid REST API kaudu
Azure'i kuvari REST API abil diagnostika sätete muutmiseks vt [selles dokumendis](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Portaali diagnostika sätete haldamine

Tagamaks, et kõik teie ressursid on õigesti häälestatud diagnostikasätete, saate liikuda **jälgimis** tera portaali ja avage **Diagnostikalogid** tera.

![Diagnostika logid blade portaalis](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Võib-olla peate klõpsama "Rohkem teenuseid" jälgimis tera leidmiseks.

See tera, saate vaadata ja filtreerida kõigi ressursse, mis toetavad kuvamiseks, kui neil diagnostika lubatud ja millised salvestusruumi konto, sündmuse jaoturi ja/või Log Analytics tööruum on need logid minevaid Diagnostikalogid.

![Diagnostika tulemused logid blade portaalis](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Ressursi klõpsamisel kuvatakse kõik logid, mis on salvestatud salvestusruumi konto ja teile võimalust välja lülitada või diagnostika sätete muutmine. Klõpsake ikooni alla laadida alla laadida logid teatud aja jooksul.

![Diagnostika logid tera üks ressurss](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Diagnostikalogide selles vaates kuvatakse ainult ning allalaadimiseks saadaval olla, kui olete konfigureerinud diagnostika sätete salvestamiseks salvestusruumi konto.

Klõpsake linki **Diagnostika** sätete avab tera diagnostika sätted, kus saate lubamine, keelamine või teie valitud ressursi diagnostika sätete muutmine.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Toetatud teenuste ja Diagnostikalogid skeem
Diagnostikalogide skeemi erineb olenevalt kategooriast, ressursside ja logige. Allpool on toetatud teenuste ja nende skeemi.

| Teenus                       | Skeemi & dokumendid                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Tarkvara koormust koormusetasakaalustusteenuse     |    [Log analüüsiteabe Azure laadi koormusetasakaalustusteenuse (eelvaade)](../load-balancer/load-balancer-monitor-log.md)             |
|    Võrgu turberühmad    |    [Log analytics võrgu turberühmad (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Rakenduse lüüsid       |    [Diagnostika logimine rakenduse lüüsi jaoks](../application-gateway/application-gateway-diagnostics.md)     |
|    Võtme Vault.                  |    [Azure'i võtme Vault logimine](../key-vault/key-vault-logging.md)                                                 |
|    Azure'i otsing               |    [Lubamine ja otsing liikluse Analytics kasutamine](../search/search-traffic-analytics.md)                         |
|    Andmesalve Lake            |    [Juurdepääs Azure'i andmed Lake poe diagnostikalogid](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Andmeanalüüsi Lake        |    [Juurdepääs diagnostikalogid Azure Lake andmeanalüüsi jaoks](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Loogika rakendused                 |    Skeemi pole saadaval.                                                                                         |
|    Azure'i paketi                |    [Azure'i paketi diagnostikalogimine](../batch/batch-diagnostics.md)                                              |
|    Azure'i automatiseerimine           |    [Log Kasutusanalüüsi Azure automatiseerimine](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Sündmuse jaoturi                  |    Skeemi pole saadaval.                                                                                         |
|    Teenuse siini                |    Skeemi pole saadaval.                                                                                         |
|    Voo Analytics           |    Skeemi pole saadaval.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Toetatud log kategooriad ressursi liigi kohta

|Ressursi tüüp|Kategooria|Kategooria kuvatav nimi|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Töö logid|
|Microsoft.Automation/automationAccounts|JobStreams|Töö voogu|
|Microsoft.Batch/batchAccounts|ServiceLog|Teenuste logid|
|Microsoft.DataLakeAnalytics/accounts|Auditilogi|Auditilogide|
|Microsoft.DataLakeAnalytics/accounts|Taotlused|Logide taotlemine|
|Microsoft.DataLakeStore/accounts|Auditilogi|Auditilogi|
|Microsoft.DataLakeStore/accounts|Taotlused|Logide taotlemine|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arhiivi logid|
|Microsoft.EventHub/namespaces|OperationalLogs|Funktsionaalseid logid|
|Microsoft.KeyVault/vaults|AuditEvent|Auditilogide|
|Microsoft.Logic/workflows|WorkflowRuntime|Käitusaja diagnostika töövoosündmused|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Võrgu turberühma sündmuse|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Counter võrgu turberühma reegel|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Võrgu turberühma reegli kulgemist sündmus|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Laadi koormusetasakaalustusteenuse teatiste sündmused|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Laadi koormusetasakaalustusteenuse juures seisund olek|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Rakenduse lüüsi Accessi Logi|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Rakenduse lüüsi jõudluse Logi|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Rakenduse lüüsi tulemüüri Logi|
|Microsoft.Search/searchServices|OperationLogs|Logi|
|Microsoft.ServerManagement/nodes|RequestLogs|Logide taotlemine|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Funktsionaalseid logid|
|Microsoft.StreamAnalytics/streamingjobs|Täitmise|Täitmise|
|Microsoft.StreamAnalytics/streamingjobs|Loome|Loome|

## <a name="next-steps"></a>Järgmised sammud
- [Kui soovite **sündmuse jaoturi** voo Diagnostikalogid](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Azure'i kuvari REST API abil diagnostika sätete muutmine](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Logide OMS Log Analytics analüüs](../log-analytics/log-analytics-azure-storage-json.md)
