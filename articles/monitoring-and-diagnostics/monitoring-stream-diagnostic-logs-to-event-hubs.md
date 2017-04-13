<properties
    pageTitle="Voona Azure'i Diagnostikalogid sündmuse jaoturi abil | Microsoft Azure'i"
    description="Saate teada, kuidas voona Azure'i Diagnostikalogid sündmuse jaoturi abil."
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
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Voona Azure'i Diagnostikalogid sündmuse jaoturi abil

**[Azure'i Diagnostikalogid](monitoring-overview-of-diagnostic-logs.md)** saate voona peaaegu reaalajas mõnda rakendust suvandiga sisseehitatud "Ekspordi sündmus jaoturi" portaalis või teenuse siini reegli Id diagnostika seade Azure PowerShelli cmdlet-käskude või Azure CLI kaudu võimaldab.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Mida saate teha ja diagnostika logid sündmuse jaoturi
Siin on paar võimalust teie kasutatavates streaming võimalus Diagnostikalogid.

- **Voo logid 3 tootja logimine ja telemeetria** -aja jooksul sündmuse jaoturi streaming muutub süsteemi toru kolmanda osapoole SIEMs oma Diagnostikalogid ja logige analytics lahendusi.

- **Vaate teenuse seisund streaming "kuum tee" andmed PowerBI** – sündmuse jaoturi kaudu, voo Analytics ja PowerBI, saate hõlpsalt muuta oma diagnostika andmete reaalajas ülevaateid lähedal Azure'i teenuste kohta. [See dokumentatsiooni artikkel annab hea ülevaade sellest, kuidas häälestada mõne sündmuse jaoturi, voo Analytics andmete töötlemiseks ja kasutada PowerBI mõne väljundina](../stream-analytics/stream-analytics-power-bi-dashboard.md). Siin on mõned näpunäited seadistamise Diagnostikalogid:
    - Sündmuse jaoturiga Diagnostikalogid kategooria jaoks luuakse automaatselt, kui ruudu portaalis või lubada PowerShelli, kaudu, nii, et soovite valida sündmuse jaoturiga teenuse siini nimeruumi nimi, mis algab selgitava lausega "ülevaateid-"
    - Siin on näide voo Kasutusanalüüsi päringu abil saate lihtsalt sõeluda kõik Logi andmed tabelisse PowerBI.

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Luua kohandatud telemeetria ja logimine platvormi** – kui teil juba on lähtuvalt ehitatud telemeetria platvormi või on ainult üks koostamise mõelda, väga paindlik avaldada-Telli laadi sündmuse jaoturi võimaldab paindlikult neelata diagnostikalogid. [Vt Dan Rosanova juhendi abil sündmuse jaoturi globaalne skaala telemeetria platvorm siin](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Luba streaming Diagnostikalogid.
Saate lubada voogesituse Diagnostikalogid programmiliselt, on portaali kaudu või kasutades [Azure kuvari REST API -ga](https://msdn.microsoft.com/library/azure/dn931943.aspx). Mõlemal juhul saate valida teenuse siini Namespace ja mõni sündmus jaoturi luuakse iga log kategooria lubate nimeruumi. Diagnostika **Log kategooria** on teatud tüüpi Logi, mis võivad koguda ressursi. Valige Logi kategooriate soovite koguda konkreetse ressursi jaotises diagnostika tera Azure'i portaalis.

![Log kategooriad portaalis](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Lubamine ja streaming diagnostikalogid kaudu Arvuta ressursse (nt VMs või teenuse struktuuri) [nõuab teistsuguseid juhiseid](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>PowerShelli cmdlet-käskude kaudu
Voogesituse [Azure PowerShelli cmdlet-käskude](insights-powershell-samples.md)kaudu lubamiseks saate kasutada funktsiooni `Set-AzureRmDiagnosticSetting` järgmiste parameetrite cmdlet:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Teenuse siini reegel ID on string, mis on selles vormingus: `{service bus resource ID}/authorizationrules/{key name}`, näiteks `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Azure'i CLI kaudu
[Azure'i CLI](insights-cli-samples.md)kaudu streaming lubamiseks saate kasutada funktsiooni `insights diagnostic set` umbes järgmine käsk:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Kasutage sama vormingut teenuse siini reegel ID, nagu on selgitatud PowerShelli cmdlet-käsu jaoks.

###<a name="via-azure-portal"></a>Azure'i portaali kaudu
Azure portaali kaudu streaming lubamiseks liikuge diagnostika sätete ressursi ja valige "Eksport sündmuse jaoturi."

![Sündmuse jaoturi portaalis eksportimine](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Valige konfigureerimiseks, mõne olemasoleva teenuse siini Namespace. Kui sündmus jaoturiga (kui see on teie esimest korda streaming diagnostikalogid) või voona on valitud nimeruumi (kui seal on juba ressursse, mis on voogesituse Logi selle nimeruumi kategooria), ja poliitika määratleb õigused, mis on streaming süsteem. Täna, voogesitus mõni sündmus jaoturi nõuab õiguste haldamine, lugemine ja saatmine. Saate luua või muuta oma teenuse siini Namespace teenuse siini Namespace ühiskasutuses juurdepääsupoliitikaid klassikaline portaalis vahekaardil "Konfigureerimine". Nende diagnostika sätete värskendamiseks olema kliendi ListKey õiguse teenuse siini autoriseerimine reegel.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Kuidas kasutada log andmed sündmuse jaoturi mobiilirakendustes?
Siin sündmuse jaoturiga väljundi näidisandmeid.

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elemendi nime | Kirjeldus                                            |
|--------------|--------------------------------------------------------|
|kirjete       | Kõigi sündmuste logi selle last massiivi.            |
|aeg          | Aeg, mille sündmusele.                      |
|kategooria      | Log kategooria see sündmus.                           |
|ResourceIdkasutamisel    | Ressursi ID loodud sündmuse ressursi. |
|operationName | Toimingu nimi.                                 |
|tase         | Valikuline. Näitab sündmuste logi tase.               |
|atribuudid    | Sündmuse atribuudid.                               |


Saate vaadata loendit kõigi ressursi pakkujad, kes toetavad sündmuse jaoturi [siin](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Järgmised sammud
- [Lisateavet leiate teemast Azure Diagnostikalogid](monitoring-overview-of-diagnostic-logs.md)
- [Sündmuse jaoturi kasutamise alustamine](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
