<properties
   pageTitle="Jälgida toimingute ja sündmusi hinnale NSGs | Microsoft Azure'i"
   description="Saate teada, kuidas hinnale, sündmused ja NSGs funktsionaalseid logimise lubamine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Log analytics võrgu turberühmad (NSGs)

Saate kasutada erinevat tüüpi logid Azure haldamiseks ja NSGs tõrkeotsing. Mõned neid logisid saab kasutada portaali kaudu ja kõik logid saate ekstraktimist Azure'i bloobimälu ja vaadata erineva tööriista, nt [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Exceli ja PowerBI. Saate lisateavet eri tüüpi logid loendist.

- **Audit logid:** [Azure'i auditilogid](../monitoring-and-diagnostics/insights-debugging-with-events.md) (varem funktsionaalseid logid) abil saate vaadata oma Azure'i subscription(s) ja nende oleku tegemisest kõik toimingud. Auditilogide on vaikimisi sisse lülitatud ja Azure eelvaade portaalis saab vaadata.
- **Sündmuselogide:** Selle logi abil saate vaadata, millised NSG reeglite rakendamise VMs ja MAC-aadressi eksemplari rollid. Reegleid olek kogutakse iga 60 sekundi järel.
- **Counter logid:** Selle logi abil saate vaadata, mitu korda iga NSG reegel rakendatud keelamiseks või liikluse lubamiseks.

>[AZURE.WARNING] Logide saadaval ainult juurutatud ressursihaldur juurutamise mudeli ressursid. Te ei saa kasutada logid klassikaline juurutamise mudeli ressursid. Viide paremini mõista kahe mudeli, [mõistmine ressursihaldur juurutus- ja klassikaline juurutamise](../resource-manager-deployment-model.md) artikkel.

##<a name="enable-logging"></a>Logimise lubamine
Auditilogi automaatselt lubatud veebisaidil alati iga ressursihaldur ressurss. Peate lubama sündmus ja counter logida kaudu nende logide andmeid. Logimise lubamiseks tehke järgmist.

1.  Logige sisse [Azure portaali](https://portal.azure.com). Kui teil pole veel olemasoleva võrgu turvalisus rühma, [luua mõne NSG](virtual-networks-create-nsg-arm-ps.md) enne jätkamist.

2.  Eelvaate portaalis, klõpsake nuppu **Sirvi** >> **võrgu turberühmad**.

    ![Eelvaate portaali - võrgu turberühmad](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Valige olemasoleva võrgu turberühma.

    ![Eelvaate portaali - jaotises võrku turbesätted](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. Klõpsake nuppu **diagnostika**labale **sätted** ja **seejärel paanil **diagnostika** **olek**, kõrval klõpsake**
5. Tera **sätted** , klõpsake **Salvestusruumi konto**ja kas valige salvestusruumi konto või looge uus.  

>[AZURE.INFORMATION] auditilogid ei nõua eraldi salvestusruumi konto. Salvestusruumi kasutuse sündmus ja reegli logimine, kehtib tasusid.

6. **Salvestusruumi konto**all ripploendis valige kas soovite logige sündmusi, hinnale või mõlemad ja klõpsake siis nuppu **Salvesta**.

    ![Eelvaate portaali - diagnostika logid](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Auditilogi
Azure'i luuakse vaikimisi selle log (varasema nimega "funktsionaalseid Logi").  Logid säilitatakse Azure sündmuselogide poes 90 päeva. Lisateavet nende logid [sündmuste vaatamine ja auditilogi logid](../monitoring-and-diagnostics/insights-debugging-with-events.md) artiklist.

## <a name="counter-log"></a>Counter log
See logi luuakse ainult siis, kui olete lubanud kohta NSG alusel nagu eespool kirjeldatud. Kui märkisite logimine määratud salvestusruumi konto andmed on salvestatud. Nagu allpool näha, logitakse iga reegel rakendatud ressursid JSON-vormingus.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Sündmuste logi
See logi luuakse ainult siis, kui olete lubanud kohta NSG alusel nagu eespool kirjeldatud. Kui märkisite logimine määratud salvestusruumi konto andmed on salvestatud. Logitakse järgmised andmed:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Vaatamine ja analüüsimine auditilogi
Saate vaadata ja analüüsida logiandmete, kasutades ühte järgmistest:

- **Azure Tööriistad:** Auditilogide Azure PowerShelli, Azure'i käsurea liides (CLI), Azure'i REST API-ga või Azure eelvaade portaali kaudu otsida teavet.  Üksikasjalikud juhised igal meetodil on üksikasjalikult kirjeldatud [auditilogi toimingute abil ressursihaldur](../resource-group-audit.md) artikkel.
- **Power BI:** Kui te pole veel [Power BI](https://powerbi.microsoft.com/pricing) konto, võite tasuta. [Azure'i auditilogide sisu pack Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) abil saate oma andmeid analüüsida eelnevalt konfigureeritud armatuurlauad, mida saate kasutada-on või kohandamine.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Vaatamine ja analüüsimine counter ja sündmuste logi

Azure'i [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) saab koguda näidiku ja sündmuste logi kontolt bloobimälu salvestusruumi ja sisaldab visualiseeringuid ja võimsad otsinguvõimalused oma logid analüüsimiseks.

Saate salvestusruumi kontoga ühenduse loomiseks ja tuua JSON Logi kirjed sündmus ja counter logid. Kui JSON-failide allalaadimiseks saate teisendada need CSV- ja Excel, PowerBI või muude andmete visualiseerimine tööriista vaates.

>[AZURE.TIP] Kui olete tuttav rakendusega Visual Studio ja põhimõtted konstandid ja C# muutujate väärtusi muuta, saate kasutada [log muundur tööriistad](https://github.com/Azure-Samples/networking-dotnet-log-converter) saadaval Github.

## <a name="next-steps"></a>Järgmised sammud

- Counter ja sündmuselogide [Log](../log-analytics/log-analytics-azure-networking-analytics.md) Analytics visualiseerimine
- [Visualiseeri Azure'i auditilogid Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) ajaveebi postitamine.
- [Vaade ja analüüsida Azure'i auditilogid Power BI ja rohkem](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) ajaveebipostituse.
