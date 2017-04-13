<properties 
   pageTitle="Jälgida juurdepääsu ja jõudluse logide ja mõõdikud rakenduse lüüsi | Microsoft Azure'i"
   description="Saate teada, kuidas lubada ja juurdepääsu ja jõudluse logide rakenduse lüüsi haldamine"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Diagnostika logimine ja rakenduse lüüsi mõõdikud

Azure'i pakub võimalust ressursi logimine ja mõõdikute jälgimine

[**Logimise**](#enable-logging-with-powershell) - logimine võimaldab jõudluse, Accessi ja muude logid salvestatud või järelevalve eesmärgil ressurss.

[**Mõõdikute**](#metrics) – rakenduse lüüsi praegu on üks mõõt. Selle mõõdiku meetmed rakenduse lüüsi baiti sekundis läbilaskevõime.

Azure'i erinevat tüüpi logid saate hallata ja lüüside rakenduse tõrkeotsing. Mõned neid logisid saab kasutada portaali kaudu ja kõik logid saate ekstraktimist Azure'i bloobimälu ja vaadata erineva tööriista, nt [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Exceli ja PowerBI. Lisateavet leiate teemast kohta erinevat tüüpi logid järgmisest loendist:

- **Audit logid:** Saate vaadata kõik toimingud, mis on esitatud Azure tellimuse ja nende oleku [Azure'i auditilogid](../monitoring-and-diagnostics/insights-debugging-with-events.md) (varem funktsionaalseid logid). Auditilogide on vaikimisi sisse lülitatud ja Azure eelvaade portaalis saab vaadata.
- **Juurdepääs logid:** Saate selle logi lüüsi Accessi muster vaatamiseks ja analüüsimiseks olulist teavet, sh helistaja IP, taotletud URL, vastuse latentsuse tagasi kood, baiti sisse ja välja. Accessi log kogutakse iga 300 sekundi järel. See logi sisaldab ühe kirje kohta rakenduse lüüsi eksemplari. Atribuudi "instanceId" saab kindlaks teha rakenduse lüüsi eksemplari.
- **Jõudluse logide:** Selle logi abil saate vaadata, kuidas toimivad rakenduse lüüsi eksemplari. See logi sisaldab teavet jõudluse kohta astme alus kokku taotluse served, sh läbilaskevõime baitides, kokku taotlusi serveeritud nurjunud taotluste arv, terve ja vigane tagaandmebaas eksemplari arv. Jõudluse logi kogutakse iga 60 sekundi järel.
- **Tulemüür logid:** Selle logi abil saate vaadata taotlusi, mis on sisse logitud, kas tuvastamise või vältimise režiim rakenduse lüüsi, mis on konfigureeritud tulemüüriga web rakenduse kaudu.

>[AZURE.WARNING] Logide saadaval ainult juurutatud ressursihaldur juurutamise mudeli ressursse. Te ei saa kasutada logid klassikaline juurutamise mudeli ressursid. Viide paremini mõista kaks mudelid, [mõistmine ressursihaldur juurutamise ja klassikaline juurutamise](../resource-manager-deployment-model.md) artikkel.

## <a name="enable-logging-with-powershell"></a>Logimise PowerShelli abil

Auditilogi automaatselt iga ressursihaldur ressursi jaoks lubatud. Tuleb lubada juurdepääsu ja jõudluse logida kaudu nende logide andmeid. Logimise lubamiseks vt järgmist: 

1. Pange tähele salvestusruumi konto ressursi ID-d, Logi andmete talletuskoht. See on vormi: /subscriptions/\<subscriptionId\>/resourceGroups/\<rühma ressursinimi\>/providers/Microsoft.Storage/storageAccounts/\<salvestusruumikonto nimi\>. Saab kasutada mis tahes salvestusruumi konto teie tellimus. Eelvaate portaali abil saate selle teabe leiate.

    ![Portaali - rakenduste lüüsi diagnostika eelvaade](./media/application-gateway-diagnostics/diagnostics1.png)

2. Märkus rakenduse lüüsi ressursi ID mis logimine on tuleb lubada. See on vormi: /subscriptions/\<subscriptionId\>/resourceGroups/\<ressursirühma nimi\>/providers/Microsoft.Network/applicationGateways/\<rakenduse lüüsi nimi\>. Eelvaate portaali abil saate selle teabe leiate.

    ![Portaali - rakenduste lüüsi diagnostika eelvaade](./media/application-gateway-diagnostics/diagnostics2.png)

3. PowerShelli järgmise cmdleti abil diagnostika logimise lubamiseks tehke järgmist.

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] auditilogid ei nõua eraldi salvestusruumi konto. Salvestusruumi kasutuse juurdepääsuks ja jõudluse logimine andmesidekasutusele tasusid.

## <a name="enable-logging-with-azure-portal"></a>Azure'i portaalis logimise lubamine

### <a name="step-1"></a>Samm 1

Liikuge oma ressursi Azure'i portaalis. Klõpsake **diagnostikalogid**. Kui see on esimene kord, kui konfigureerimise diagnostika tera näeb välja nagu järgmisel pildil:

Rakenduse lüüsi, 3 logid on saadaval.

- Accessi Log
- Jõudluse Log
- Tulemüüri Log

Klõpsake nuppu **Lülita sisse diagnostika** andmeid.

![diagnostika säte blade][1]

### <a name="step-2"></a>Samm 2

**Diagnostika sätete** enne, kui selle diagnostikalogid seadistada. Selles näites kasutatakse Log analytics logid talletamiseks. Klõpsake jaotises **Log Analytics** konfigureerida oma tööruumi **konfigureerimine** . Sündmuse jaoturi ja salvestusruumi konto saab kasutada ka diagnostika Logide salvestamine.

![diagnostika blade][2]

### <a name="step-3"></a>Samm 3

Valige mõne olemasoleva tööruumi OMS või looge uus. Selles näites kasutatakse mõne olemasoleva.

![OMS tööruumid][3]

### <a name="step-4"></a>Samm 4

Kui olete lõpetanud, kinnitage sätted ja klõpsake nuppu **Salvesta** sätted salvestada.

![valiku kinnituseks][4]

## <a name="audit-log"></a>Auditilogi

Azure'i luuakse vaikimisi selle log (varasema nimega "funktsionaalseid Logi").  Logid säilitatakse Azure sündmuselogide poes 90 päeva. Lisateavet nende logid [sündmuste vaatamine ja audit logid](../monitoring-and-diagnostics/insights-debugging-with-events.md) artiklist.

## <a name="access-log"></a>Accessi log

See logi luuakse ainult siis, kui olete selle lubanud, klõpsake soovitud rakenduse lüüsi alus eelmist toimingut üksikasjalikult kirjeldatud kohta. Andmed on salvestatud määratud kui märkisite logimine salvestusruumi konto. Iga Accessi rakenduste lüüsi logitakse JSON-vormingus, nagu näha Järgnevas näites:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Jõudluse log

See logi luuakse ainult siis, kui see on lubatud, klõpsake soovitud rakenduse lüüsi alus eelmist toimingut üksikasjalikult kirjeldatud kohta. Kui märkisite logimine määratud salvestusruumi konto andmed on salvestatud. Logitakse järgmised andmed:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Tulemüüri log

See logi luuakse ainult siis, kui see on lubatud, klõpsake soovitud rakenduse lüüsi alus eelmist toimingut üksikasjalikult kirjeldatud kohta. See logi ka jaoks on vaja konfigureerida web rakenduse tulemüüriga rakenduste portaali sisse. Kui märkisite logimine määratud salvestusruumi konto andmed on salvestatud. Logitakse järgmised andmed:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Vaatamine ja analüüsimine auditilogi

Saate vaadata ja analüüsida logiandmete, kasutades ühte järgmistest:

- **Azure Tööriistad:** Auditilogide Azure PowerShelli, Azure'i käsurea liides (CLI), Azure'i REST API-ga või Azure eelvaade portaali kaudu otsida teavet.  Üksikasjalikud juhised igal meetodil on üksikasjalikult kirjeldatud artiklis [auditilogi toimingute abil ressursihaldur](../resource-group-audit.md) .
- **Power BI:** Kui te pole veel [Power BI](https://powerbi.microsoft.com/pricing) konto, võite tasuta. [Azure'i auditilogide sisu pack teenuse Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) abil saate oma andmeid analüüsida eelnevalt konfigureeritud armatuurlauad, mida saate kasutada-on või kohandamine.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Vaatamine ja analüüsimine juurdepääsu, jõudlus ja tulemüüri log

Azure'i [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) saab koguda näidiku ja sündmuste logi kontolt bloobimälu salvestusruumi ja sisaldab visualiseeringuid ja võimsad otsinguvõimalused oma logid analüüsimiseks.

Saate salvestusruumi kontoga ühenduse loomiseks ja tuua JSON log Accessi ja jõudluse logide kirjeid. Kui JSON-failide allalaadimiseks saate teisendada need CSV- ja Excel, PowerBI või muude andmete visualiseerimine tööriista vaates.

>[AZURE.TIP] Kui olete tuttav rakendusega Visual Studio ja põhimõtted konstandid ja C# muutujate väärtusi muuta, saate kasutada [log muundur tööriistad](https://github.com/Azure-Samples/networking-dotnet-log-converter) saadaval Github.

## <a name="metrics"></a>Mõõdikud

Mõõdikute on teatud Azure ressursid, kus saate jõudluse hinnale portaalis. Rakenduse lüüsi, üks mõõt on saadaval käesoleva artikli kirjutamise ajal. Selle mõõdiku on läbilaskevõime ja näha portaalis. Liikuge rakenduste portaali ja klõpsake nuppu **mõõdikute**.  Valige läbilaskevõime **saadaval mõõdikute** jaotist väärtusi. Järgmisel pildil näete näide filtritega, mida saab kasutada andmete kuvamiseks eri aeg vahemikud.

Praeguse toetavad mõõdikute loendi leiate [Azure'i monitoris toetatud mõõdikute](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![argumendil kuvamine][5]

## <a name="alert-rules"></a>Teatiste reeglid

Klõpsake ressursi mõõdikute põhjal, saab alustada teatiste reeglid. See tähendab rakenduse lüüsi, teatise saate kõne on webhook või e-posti administraator, kui rakenduse lüüsi läbilaskevõime on kohale, alla või mõne läve määratud aja jooksul.

Järgmises näites juhendab teid luua reegli, mis saadab e-posti administraator pärast läbilaskevõime lävi on rikutud.

### <a name="step-1"></a>Samm 1

Klõpsake **lisamine argumendil teatise** käivitamiseks. See blade jõuate ka mõõdikute keelest.

![teatiste reeglite blade][6]

### <a name="step-2"></a>Samm 2

Labale **Lisa reegel** täita nimi, tingimus, teavitage jaotised ja klõpsake nuppu **OK** , kui valmis.

**Tingimus** Vaateselektori võimaldab 4 väärtused, **suurem**, **suurem või sellega võrdne**, **väiksem**või **väiksem või võrdne**.

**Perioodi** Vaateselektori, võimaldab valimise 6 tunni jooksul 5 minutit.

**E-posti omanikud, kaasautorid,** ja lugejad e-posti võib olla dünaamiline kasutajad, kellel on juurdepääs selle ressursi põhjal. Muul juhul komaga eraldatud kasutajate loendit saab esitada **täiendavad administraatori email(s)** tekstiväli.

![reegli blade lisamine][7]

Kui piirmäär on rikutud, e-posti saabub sarnaneb järgmisel pildil:

![rikutud lävi e-posti][8]

Teatiste loend kuvatakse pärast argumendil teatise on loodud ja antakse ülevaade sellest, teatiste reegleid.

![reegli kuvamine][9]

Teatiste kohta lisateabe saamiseks külastage [teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Mõistmaks, webhooks ja kuidas saate neid kasutada, ja teatiste kohta lisateavet, külastage [webhook, Azure argumendil teatisele konfigureerimine](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Järgmised sammud

- Counter ning sündmuselogide [Log](../log-analytics/log-analytics-azure-networking-analytics.md) Analytics visualiseerimine 
- [Visualiseeri Azure'i auditilogid koos Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) ajaveebi postitamine.
- [Vaade ja analüüsida Azure'i auditilogid Power BI ja rohkem](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) ajaveebipostituse.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png