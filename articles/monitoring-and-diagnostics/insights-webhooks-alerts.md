<properties
    pageTitle="Konfigureerimine webhooks Azure argumendil teatised | Microsoft Azure'i"
    description="Azure'i teatiste muudest süsteemidest Azure'i marsruudi."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Azure'i argumendil teatisele on webhook konfigureerimine

Webhooks abil saate marsruutida Azure teatis teatis muudest süsteemidest post töötlemiseks või kohandatud toimingute jaoks. Saate mõne webhook teatisele marsruutimiseks teenused, mida saata SMS, logige vigu, teavitage meeskond teenuste jututoa/sõnumside kaudu või mis tahes arv muid toiminguid teha. Selles artiklis kirjeldatakse, kuidas määrata mõne webhook Azure argumendil teatisele ja last jaoks soovitud webhook HTTP POSTITADA välja näeb. Lisateavet häälestamise ja -skeemifailid on Azure tegevuse Log Teavita (teatis sündmused), [selle asemel sellelt lehelt leiate](./insights-auditlog-to-webhook-email.md).

Azure'i teatiste HTTP POST JSON teavita sisu vormindamiseks skeemi määratletud all, on webhook URI esitate teatise loomisel. Selle URI peab olema lubatud HTTP- või HTTPS-i lõpp. Azure'i postituste ühe kande taotluse teatise aktiveerimisel.

## <a name="configuring-webhooks-via-the-portal"></a>Portaali kaudu webhooks konfigureerimine

Saate lisada või värskendada webhook URI [portaali](https://portal.azure.com/)loomine või uuendada teatiste kuval.

![Reegli lisamine](./media/insights-webhooks-alerts/Alertwebhook.png)

Samuti saate konfigureerida teatise postitamine on webhook URI [Azure PowerShelli cmdlet-käsud](./insights-powershell-samples.md#create-alert-rules), [Mitu platvormi CLI](./insights-cli-samples.md#work-with-alerts)või [Azure kuvari REST API -ga](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Funktsiooni webhook autentimine

Funktsiooni webhook saate autentida nende meetodite abil:

1. **Luba põhinev autoriseerimine** - webhook URI on salvestatud Turbeloa ID-ga, nt. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Tavaline luba** – webhook URI on salvestatud kasutajanime ja parooli, nt. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Last skeem

POSTITUSE toiming sisaldab järgmist JSON last ja skeemi meetermõõdustik vastavalt kõik teatised.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Väli | Kohustusliku | Fikseeritud väärtuste määramine | Märkmete |
| :-------------| :-------------   | :-------------   | :-------------   |
|olek|Y|"Aktiveeritud", "Lahendada"|Olek oleks seadistatud vastavalt tingimusele eemaldada teatise.|
|kontekst| Y | | Teatiste kontekstis.|
|ajatempli| Y | | Kus teatis käivitati aeg.|
|ID | Y | | Iga reegli on kordumatu ID-ga.|
|Nimi               |Y                  |                   | Teatiste nimi.|
|kirjeldus        |Y                  |                           |Teatise kirjeldus.|
|conditionType      |Y                  |"Argumendil", "Sündmus"          |Toetatud on kahte tüüpi teatised. Üks põhineb argumendil tingimus ja teise sündmuse tegevuste Logi sisse. Selle väärtuse abil saate kontrollida, kui teatise põhjal väärtuseks meetermõõdustik või sündmuse.|
|tingimus          |Y                  |                           | Kindlad väljad, et otsida aluseks olevat conditionType.|
|metricName         |Argumendil teatiste jaoks  |                           |Meetermõõdustik, mis määratleb, mis jälgib reegli nimi.|
|metricUnit         |Argumendil teatiste jaoks  |"Baiti", "BytesPerSecond", "Arv", "CountPerSecond", "Protsenti", "Sekundit"|     Üksuse mõõdiku lubatud. [Lubatud väärtused on loetletud siin](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |Argumendil teatiste jaoks  |                           |Teatise põhjustanud mõõdiku tegelik väärtus.|
|lävi          |Argumendil teatiste jaoks  |                           |Kus on aktiveeritud teatise lävi väärtus.|
|windowSize         |Argumendil teatiste jaoks  |                           |Aja jooksul kasutavad aluseks olevat teatiste jälgimiseks. Peab olema 5 minutit kuni 1 päeva. ISO 8601 kestus vormingus.|
|timeAggregation    |Argumendil teatiste jaoks  |"Keskmine", "Viimane", "Kuni", "Miinimum", "Pole", "kogumüük" | Kuidas peaks kogutud andmete kombineerida aja jooksul. Vaikeväärtus on keskmine. [Lubatud väärtused on loetletud siin](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|tehtemärk           |Argumendil teatiste jaoks  |                           |Tehtemärk kasutada võrdlemiseks piirmäärast praeguse argumendil andmed.|
|subscriptionId     |Y                  |                           |Azure'i Tellimuse ID-ga.|
|resourceGroupName  |Y                  |                           |Kannatavas ressursi ressursirühma nimi.|
|resourceName       |Y                  |                           |Ressursinimi kannatavas ressursi.|
|resourceType       |Y                  |                           |Ressursi tüüp kannatavas ressursi.|
|ResourceIdkasutamisel         |Y                  |                           |Ressursi ID kannatavas ressursi.|
|resourceRegion     |Y                  |                           |Piirkond või asukoht kannatavas ressursi.|
|portalLink         |Y                  |                           |Otselink portaali ressursi kokkuvõtteleht.|
|atribuudid         |N                  |Valikuline.                   |Kogum `<Key, Value>` paari (st `Dictionary<String, String>`), mis sisaldavad sündmuse üksikasjad. Välja atribuutide pole kohustuslik. Kohandatud UI või loogika rakenduse-põhiste töövoo, sisestage kasutajate klahv/väärtused, mis saab edasi kaudu soovitud last. Alternatiivsete viisil edastada kohandatud atribuudid on webhook on kaudu (nagu päringu parameetrite) ise webhook uri|


>[AZURE.NOTE] Välja atribuudid saate määrata ainult [Azure kuvari REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)abil.

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet Azure teatised ja webhooks video [Integreerida Azure'i teatiste PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Azure'i automaatika skriptide (tegevusraamatud) käivitada Azure teatised](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Azure'i teatise saata SMS kaudu Twilio loogika rakenduse kasutamine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Loogika rakenduse abil Azure teatise vaikne sõnumi saatmine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Sõnumi saatmine on Azure järjekorda Azure teatise loogika rakenduse abil](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
