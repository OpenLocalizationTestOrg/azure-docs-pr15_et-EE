<properties
    pageTitle="Konfigureerimine on webhook Azure tegevuse Log teatised | Microsoft Azure'i"
    description="Vaadake, kuidas tegevuse Log teatiste abil saate helistada webhooks. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Konfigureerimine on webhook on Azure tegevuse Log teatised

Webhooks abil saate marsruutida Azure teatis teatis muudest süsteemidest pärast töötlemiseks või kohandatud toimingute jaoks. Saate mõne webhook teatisele marsruutimiseks teenused, mida saata SMS, logige vigu, teavitage meeskond teenuste jututoa/sõnumside kaudu või mis tahes arv muid toiminguid teha. Selles artiklis kirjeldatakse, kuidas määrata mõne webhook Azure tegevuse Log teatisele ja last jaoks soovitud webhook HTTP POSTITADA välja näeb. Häälestamise ja skeemi Azure argumendil teatise, [selle asemel sellelt lehelt leiate](insights-webhooks-alerts.md)teavet. Tegevuste Logi teatise saate häälestada ka saata meilisõnumeid, kui aktiveeritud.

>[AZURE.NOTE] See funktsioon on praegu eelvaade, nii et oodata muutuv kvaliteeti ning jõudlust.

Saate häälestada teatise tegevuse Log [Azure PowerShelli cmdlet-käsud](insights-powershell-samples.md#create-alert-rules), [Mitu platvormi CLI](insights-cli-samples.md#work-with-alerts)või [Azure kuvari REST API -ga](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Funktsiooni webhook autentimine
NNõutav webhook saate autentida nende meetodite abil:

1. **Luba põhinev autoriseerimine** - webhook URI salvestatakse Turbeloa ID-ga, nt. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Tavaline luba** – webhook URI salvestatakse kasutajanime ja parooli, nt. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Last skeem
POSTITUSE toiming sisaldab järgmist JSON last ja skeemi tegevuste Logi põhinev kõik teatised. Selle skeemi on sarnane poolt meetermõõdustik vastavalt teatised.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Elemendi nime       |Kirjeldus|
|---                |---|
|olek             |Kasutada argumendil teatised. Alati seatud "aktiveeritud" tegevuse Log teatised.|
|kontekst            |Sündmuse kontekstis.|
|resourceProviderName|Ressursi pakkuja kannatavas ressursi.|
|conditionType      |Alati "sündmus".|
|Nimi               |Reegli nimi.|
|ID                 |Ressursi ID teatise.|
|kirjeldus        |Teatise kirjeldus vastavalt teatise loomise ajal.|
|subscriptionId     |Azure'i Tellimuse ID-ga.|
|ajatempli          |Aeg, kus sündmus loodud Azure'i teenus, mis töödeldud kutse.|
|ResourceIdkasutamisel         |Ressursi ID kannatavas ressursi.|
|resourceGroupName  |Kannatavas ressurssi ressursirühma nimi|
|atribuudid         |Kogum `<Key, Value>` paari (st `Dictionary<String, String>`), mis sisaldavad sündmuse üksikasjad.|
|sündmuse              |Element, mis sisaldab metaandmete sündmuse kohta.|
|luba      |Sündmuse RBAC atribuute. Nendeks tavaliselt "toiming", "roll" ja "ulatus."|
|kategooria           |Kategooria soovitud sündmus. Toetatud väärtuste hulka kuuluvad: haldus, Teavita turvalisus, ServiceHealth, soovitused.|
|helistaja             |Kasutaja tehtud toiming, UPN-i taotlust või SPN taotluste kättesaadavuse põhjal meiliaadress. Võib olla teatud süsteemi kõnede null.|
|correlationId      |Tavaliselt GUID string vormingus. Sündmusega correlationId kuuluvad sama suuremat toimingu ja tavaliselt ühiskasutus on correlationId.|
|eventDescription   |Sündmuse kirjeldus staatiliseks tekstiks.|
|eventDataId        |Sündmuse ainuidentifikaator.|
|eventSource        |Azure'i teenus või infrastruktuur, mis on loodud sündmuse nimi.|
|httpRequest        |Tavaliselt hõlmab "clientRequestId", "clientIpAddress" ja "meetod" (nt panna HTTP meetod).|
|tase              |Üks järgmistest väärtustest: "Kriitiline", "Tõrge", "Hoiatus", "Teatised" ja "Verbose."|
|operationId        |Tavaliselt GUID jagada ühe vastab sündmused.|
|operationName      |Toimingu nimi.|
|atribuudid         |Sündmuse atribuudid.|
|olek             |String. Toimingu olekut. Levinud väärtuste hulka kuuluvad: "Alustatud", "Edu", "Õnnestus", "Nurjus", "Aktiivne", "Lahendada".|
|subStatus          |Tavaliselt sisaldab HTTP olekukoodi vastavate ülejäänud kõne. Võivad sisaldada ka muud stringide lisamine substatus kirjeldav. Levinud substatus väärtuste hulka kuuluvad: OK (HTTP olekukoodi: 200), loodud (HTTP olekukoodi: 201), aktsepteeritud (HTTP olekukoodi: 202), pole sisu (HTTP olekukoodi: 204), vigane päring (HTTP olekukoodi: 400), ei leitud (HTTP olekukoodi: 404), konflikti (HTTP olekukoodi: 409), Sisemine serveritõrge (HTTP olekukoodi: 500), teenus pole saadaval (HTTP olekukoodi: 503), lüüsi ajalõpp (HTTP olekukoodi: 504)|

## <a name="next-steps"></a>Järgmised sammud
- [Lisateave tegevuste Logi](monitoring-overview-activity-logs.md)
- [Azure'i automaatika skriptide (tegevusraamatud) käivitada Azure teatised](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Kasutage loogika rakenduse saata SMS Twilio: Azure'i teatise kaudu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Selles näites on argumendil teatised, kuid võib muuta töötamine tegevuse Log teatise.
- [Kasutage loogika rakenduse Azure teatise vaikne sõnumi saatmiseks](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Selles näites on argumendil teatised, kuid võib muuta töötamine tegevuse Log teatise.
- [Kasutage loogika rakenduse sõnum on Azure järjekorda: Azure'i teatise saata](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Selles näites on argumendil teatised, kuid võib muuta töötamine tegevuse Log teatise.
