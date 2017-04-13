<properties
   pageTitle="Laadi koormusetasakaalustusteenuse hinnale toimingute ja sündmusi jälgida | Microsoft Azure'i"
   description="Saate teada, kuidas lubada teatiste sündmused ja probe seisundiandmete logisse kandmise jaoks Azure'i laadi koormusetasakaalustusteenuse olek"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Log analüüsiteabe Azure laadi koormusetasakaalustusteenuse (eelvaade)

Azure'i erinevat tüüpi logid saate hallata ja koormus soolise tõrkeotsing. Mõned need logid pääseb portaali kaudu. Kõik logid saate ekstraktimist Azure'i bloobimälu ja vaadata erineva tööriista, nt Excelist ja PowerBI. Saate lisateavet eri tüüpi logid loendist.

- **Audit logid:** [Azure'i auditilogid](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (varem funktsionaalseid logid) abil saate vaadata oma Azure'i subscription(s) ja nende oleku tegemisest kõik toimingud. Auditilogide on vaikimisi sisse lülitatud ja Azure portaalis saab vaadata.
- **Teavita sündmuselogide:** Selle logi abil saate vaadata, millised teatised laadi koormusetasakaalustusteenuse on Astendatud. Laadi koormusetasakaalustusteenuse olek kogutakse iga viie minuti järel. See logi kirjutatakse ainult kui tõstetakse laadi koormusetasakaalustusteenuse teatiste sündmus.
- **Seisundi juures logid:** Selle logi abil saate otsida juures seisundi kontrolli olekut, mitu eksemplari on võrgus koormusetasakaalustusteenuse laadi tagaandmebaas ja saades võrguliiklust laadi koormusetasakaalustusteenuse virtuaalmasinates protsent. See logi on kirjutatud juures olek sündmuse muutmine.

>[AZURE.IMPORTANT] Logige analytics praegu töötab ainult Internet vastastikuste koormus soolise. Logide saadaval ainult juurutatud ressursihaldur juurutamise mudeli ressursid. Te ei saa kasutada logid klassikaline juurutamise mudeli ressursid. Juurutamise mudelite kohta leiate lisateavet teemast [mõistmine ressursihaldur juurutus- ja klassikaline juurutamise](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Logimise lubamine

Auditilogi automaatselt iga ressursihaldur ressursi jaoks lubatud. Peate lubama sündmus ja seisundi juures logida kaudu nende logide andmeid. Järgmiste juhiste abil saate logimise.

Logige sisse [Azure portaali](http://portal.azure.com). Kui teil pole veel laadi koormusetasakaalustusteenuse, [luua laadi koormusetasakaalustusteenuse](load-balancer-get-started-internet-arm-ps.md) enne jätkamist.

1. Portaalis, klõpsake nuppu **Sirvi**.
2. Valige **koormus soolise**.

    ![portaali - laadi-koormusetasakaalustusteenuse](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Valige mõne olemasoleva laadi koormusetasakaalustusteenuse >> **Kõik sätted**.
4. Laadi koormusetasakaalustusteenuse nime all dialoogiboksi paremas servas liikuge **jälgimis**, klõpsake nuppu **diagnostika**.

    ![portaali - koormuse koormusetasakaalustusteenuse-sätted](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. Valige paanil **diagnostika** jaotises **olek** **sees**.
6. Klõpsake nuppu **konto salvestusruumi**.
7. **LOGIDE**, klõpsake jaotises Valige salvestusruumi konto või looge uus. Liuguri abil saate määratleda, mitu päeva sündmuse dat säilitatakse sündmuste logid. 8. Klõpsake nuppu **Salvesta**.

    ![Portaali - diagnostika logid](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] auditilogid ei nõua eraldi salvestusruumi konto. Kasuta salvestusruumi sündmus ja seisundi juures logimine tekivad tasusid.

## <a name="audit-log"></a>Auditilogi

Auditilogi luuakse vaikimisi. Logid säilitatakse Azure sündmuselogide poes 90 päeva. Lisateavet nende logid [sündmuste vaatamine ja audit logid](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) artiklist.

## <a name="alert-event-log"></a>Teatiste sündmuste logi

See logi luuakse ainult siis, kui olete selle lubanud, klõpsake soovitud laadi koormusetasakaalustusteenuse alus kohta. Sündmuste on sisse logitud JSON-vormingus ja talletatud määratud kui märkisite logimine salvestusruumi konto. Järgmises näites sündmuse.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

JSON väljund näitab *eventname* atribuut, mis kirjeldab laadi koormusetasakaalustusteenuse, mis on loodud teatise põhjus. Sel juhul loodud teatise oli TCP pordi õiguste lõppemise põhjustatud allika IP NAT piirangud (SNAT).

## <a name="health-probe-log"></a>Seisund juures log

See logi luuakse ainult siis, kui olete selle lubanud, klõpsake soovitud laadi koormusetasakaalustusteenuse alus nagu ülaltoodud kohta. Kui märkisite logimine määratud salvestusruumi konto andmed on salvestatud. Ümbris nimega "ülevaateid – logid-loadbalancerprobehealthstatus" on loodud ja logitakse järgmised andmed:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

JSON väljund kuvatakse välja atribuudid põhiteave juures seisund olek. Atribuudi *dipDownCount* kuvatakse tagaandmebaas, mis on saanud võrguliikluse tõttu nurjunud juures vastuste eksemplaride arv.

## <a name="view-and-analyze-the-audit-log"></a>Vaatamine ja analüüsimine auditilogi

Saate vaadata ja analüüsida logiandmete, kasutades ühte järgmistest:

- **Azure Tööriistad:** Auditilogide Azure PowerShelli, Azure'i käsurea liides (CLI), Azure'i REST API-ga või Azure eelvaade portaali kaudu otsida teavet. Üksikasjalikud juhised igal meetodil on üksikasjalikult kirjeldatud [auditilogi toimingute abil ressursihaldur](../../articles/resource-group-audit.md) artikkel.
- **Power BI:** Kui teil juba on [Power BI](https://powerbi.microsoft.com/pricing) konto, võite tasuta. [Azure'i auditilogide sisu pack Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs)abil saate oma andmeid eelnevalt konfigureeritud armatuurlauad analüüsida või saate kohandada vastavalt oma vajadustele vaated.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Vaatamine ja analüüsimine seisundi juures ja sündmuste logi

Peate talletusruumi kontoga ühenduse loomiseks ja tuua JSON Logi kirjed sündmus ja seisundi juures logid. Kui JSON-failide allalaadimiseks saate teisendada need CSV- ja Excel, PowerBI või muude andmete visualiseerimine tööriista vaates.

>[AZURE.TIP] Kui olete tuttav rakendusega Visual Studio ja põhimõtted konstandid ja C# muutujate väärtusi muuta, saate kasutada [log muundur tööriistad](https://github.com/Azure-Samples/networking-dotnet-log-converter) saadaval Github.

## <a name="additional-resources"></a>Lisaressursid

- [Visualiseeri Azure'i auditilogid Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) ajaveebi postitamine.
- [Vaade ja analüüsida Azure'i auditilogid Power BI ja rohkem](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) ajaveebipostituse.

## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse sondid mõistmine](load-balancer-custom-probe-overview.md)
