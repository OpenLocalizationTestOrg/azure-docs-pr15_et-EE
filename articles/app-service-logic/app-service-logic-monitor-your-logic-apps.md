<properties 
    pageTitle="Teenuses Azure rakendus oma loogika rakenduste jälgimine | Microsoft Azure'i" 
    description="Kuidas vaadata oma rakenduste loogika on teha" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Loogika rakenduste jälgimine

Pärast [loogika rakenduse loomine](app-service-logic-create-a-logic-app.md)näete täieliku ajaloo täitmise Azure'i portaalis.  Te saate häälestada ka teenuseid nagu Azure'i diagnostika- ja Azure teatiste sündmuste reaalajas jälgimiseks ja teatiste sündmuste jaoks meelepärased "kui rohkem kui 5 käivitatakse nurjuda tunnis."

## <a name="monitor-in-the-azure-portal"></a>Azure'i portaalis jälgimine

Ajaloo kuvamiseks valige **Sirvi**ja valige **Loogika rakendused**. Kuvatakse kõik loogika rakendused teie tellimus loendit.  Valige loogika rakendus, mida soovite jälgida.  Kuvatakse kõik toimingud ja selle loogika rakenduse ilmnenud päästikute loendit.

![Ülevaade](./media/app-service-logic-monitor-your-logic-apps/overview.png)

On paar jaotisi, selle tera on abi.

- **Kokkuvõte** on loetletud **kõik töötab** ja **Päästik ajalugu**
    - **Kõik töötab** loendi loogika rakenduse uusim versioon töötab.  Klõpsake nuppu Käivita üksikasjad ükskõik millisele reale või klõpsake paani loetleda veel käivitatakse.
    - **Päästik ajalugu** loetletud päästik tegevuse selle loogika rakenduse.  Päästik tegevuse võiks olla "Vahelejäetud" sisse uutele andmetele (nt kas soovite vaadata, kui uus fail on lisatud FTP), "Õnnestus" tähendab, et andmed tagastati tule loogika rakenduse või "Failed" vastav konfiguratsiooni tõrge.
- **Diagnostika** võimaldab käitusaja üksikasjad ja sündmuste vaatamine ja tellimine [Azure'i teatised](#adding-azure-alerts)

>[AZURE.NOTE] Ülejäänud teenuses loogika rakenduse krüptitud kõik käitusaja üksikasjad ja sündmused. Ainult dekrüptitakse kasutajalt taotluse vaade. Need sündmused juurdepääsu saab kontrollida ka Azure Role-Based juurdepääsu juhtimine (RBAC).

### <a name="view-the-run-details"></a>Käivita üksikasjade kuvamine

Selles loendis, käivitatakse kuvatakse **olek**, **Algusaeg**ja **kestus** konkreetsete Käivita. Valige mis tahes rida töötavad üksikasjade kuvamiseks.

Jälgimise näitab iga toimingu Käivita, sisendeid ja väljundeid ja tõrketeated, mis võib olla occurre.

![Käivita ja toimingud](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Kui vajate täiendavaid üksikasju nagu Käivita **Korrelatsiooni ID** (mis saab kasutada REST API-ga), saate klõpsata nuppu **Käivita üksikasjad** .  See rühm sisaldab kõiki juhiseid, olek ja sisendeid/väljundeid Käivita.

## <a name="azure-diagnostics-and-alerts"></a>Azure'i diagnostika- ja teatised

Lisaks Azure portaali ja REST API ülal esitatud üksikasjad, saate konfigureerida kasutama Azure'i diagnostika veel rikkaliku üksikasju ja silumine rakenduse loogika.

1. Klõpsake jaotises **diagnostika** loogika rakenduse tera
1. Klõpsake nuppu **Diagnostika sätete** konfigureerimine
1. Sündmuse jaoturi või salvestusruumi konto eraldavad andmete konfigureerimine

    ![Azure'i diagnostika sätete](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Azure'i teatiste lisamise

Kui diagnostika on konfigureeritud, saate lisada Azure teatiste tule kui teatud lävede on ületanud.  **Diagnostika** tera, valige **Lisa teatise**ja **teatiste** paani.  See juhendab teid konfigureerimise teatise lävede ja mõõdikute põhjal.

![Azure'i teatiste mõõdikud](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Saate konfigureerida **tingimus**, **lävi**ja **periood** vastavalt soovile.  Lõpuks saate konfigureerida e-posti aadressi saata teate, või mõne webhook konfigureerimine.  Loogika rakenduse [taotluse päästik](../connectors/connectors-native-reqres.md) abil saate käivitada teatisele ka (teha näiteks [vaikne postitada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [saata](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)või [lisada sõnumi järjekorda](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Azure'i diagnostika sätete

Kõik need sündmused sisaldab loogika rakendus ja nagu olek sündmuse üksikasjad.  Siin on näide *ActionCompleted* sündmuse:

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

Kahe atribuudid, mis on eriti kasulik jälgimine ja jälgimiseks on *clientTrackingId* ja *trackedProperties*.  

#### <a name="client-tracking-id"></a>Kliendiid jälgimine

Kliendi jälgimise ID on väärtus, mis kuvatakse oleksid sündmuste üle loogika rakenduse käivitada, sealhulgas pesastatud töövooge, nimetatakse loogika rakenduse osana.  See ID on automaatselt loodud kui mitte esitatud, kuid saate käsitsi määrata kliendi ID jälgimine sooritab käivitamiseks on `x-ms-client-tracking-id` päise ID väärtusega päästik taotluse (taotluse käivitada, HTTP päästik või webhook Käivita).

#### <a name="tracked-properties"></a>Jälitatud atribuudid

Töövoo jälgimiseks sisendina toimingud või väljundi diagnostika andmeid saab lisada jälitatud atribuudid.  See võib olla kasulik, kui soovite jälgida andmeid nagu "Tellimuse ID" sisse oma telemeetria.  Jälitatud atribuudi lisamiseks kaasata selle `trackedProperties` atribuudi toimingu.  Jälitatud atribuudid saab ainult jälitus ühe toimingud sisendina ja väljundeid, kuid te saate kasutada funktsiooni `correlation` atribuudid sündmuste vastavusse viia üle kestab toimingud.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Teie lahenduste laiendamine

Saate kasutada seda telemeetria sündmuse jaoturi või salvestusruumi sisse nagu [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite), [Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/)ja [Power BI](https://powerbi.com) on teie integreerimine töövoogude jälgimine reaalajas muude teenustega.

## <a name="next-steps"></a>Järgmised sammud
- [Levinud näited ja stsenaariumid loogika rakendused](app-service-logic-examples-and-scenarios.md)
- [Loogika rakenduse juurutamine malli loomine](app-service-logic-create-deploy-template.md)
- [Ettevõtte Kliendiintegratsiooni funktsioonide](app-service-logic-enterprise-integration-overview.md)
