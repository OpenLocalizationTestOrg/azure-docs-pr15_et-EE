<properties
    pageTitle="Saada e-posti ja webhook teatiste autoscale toimingute abil. | Microsoft Azure'i"
    description="Vaadake, kuidas kõne web URL-id või saatmine meiliteatised Azure'i kuvari autoscale toimingute abil. "
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
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Saada e-posti ja webhook teatiste Azure'i kuvari autoscale toimingute abil

Selles artiklis kirjeldatakse, kuidas häälestamist päästikute, et saate helistada teatud web URL-id või saata e-kirju autoscale toimingud Azure põhjal.  

## <a name="webhooks"></a>Webhooks
Webhooks võimaldavad teil on Azure teatiste marsruutimiseks muudest süsteemidest pärast töötlemiseks või kohandatud teatiste saamiseks. Näiteks marsruutimine teatise sissetulevate web taotlus saata SMS, Logi vigu, teavitage services meeskond vestluse kasutamine või sõnumside käsitlemiseks jne. Webhook URI peab olema lubatud HTTP- või HTTPS-i lõpp.

## <a name="email"></a>E-posti
E-posti saab saata mis tahes e-posti aadress. Administraatorid ja kaasadministraatorite tellimuse, kus reegel töötab ka teavitama.


## <a name="cloud-services-and-web-apps"></a>Pilveteenustega ja Web Apps
Te saate osalemine Azure portaalist pilveteenustega ja serveri parkides (Web Apps).

- Valige meetermõõdustik **skaala järgi** .

![skaala järgi](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtuaalse masina skaala komplektid
Uuem Virtuaalmasinates loodud koos ressursihaldur (virtuaalse masina skaala komplekti), saate konfigureerida see REST API-ga, ressursihaldur Mallid, PowerShelli ja CLI abil. Portaali kasutajaliides pole veel saadaval.
Kui REST API-ga või ressursihaldur malli abil kaasata teatised elemendi järgmistest suvanditest.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Väli                              |Kohustuslikud? |Kirjeldus|
|---                                |---        |---|
|toiming                          |Jah        |väärtus peab olema "Skaala"|
|sendToSubscriptionAdministrator    |Jah        |väärtus peab olema "true" või "false"|
|sendToSubscriptionCoAdministrators |Jah        |väärtus peab olema "true" või "false"|
|customEmails                       |Jah        |väärtus võib olla tühi [] või stringi massiivi e-kirju.|
|webhooks                           |Jah        |väärtus võib olla tühi või kehtiv Uri|
|serviceUri                         |Jah        |lubatud https Uri|
|atribuudid                         |Jah        |väärtus peab olema tühi {} või võib sisaldada võti ja väärtuse paarideks|


## <a name="authentication-in-webhooks"></a>Webhooks autentimine
On kaks autentimise URI vorme.

1. Luba-base autentimine, kui salvestate webhook URI päringu parameetrina Turbeloa ID-ga. Näiteks https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Elementaarne autentimine, kus saate kasutada kasutaja ID ja parooliga. Näitekshttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Autoscale teatis webhook last skeem
Kui luuakse autoscale teate, kaasatakse järgmised metaandmete webhook last:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Väli  |Kohustuslikud?|    Kirjeldus|
|---|---|---|
|olek |Jah    |Oleku, mis näitab, et toimingu autoscale on loodud|
|toiming| Jah |Eksemplaride suurendada, see tuleb "skaala" ja vähenemine eksemplari, see olema "Skaala In"|
|kontekst|   Jah |Autoscale toimingu kontekstile|
|ajatempli| Jah |Ajatempliga autoscale toimingu käivitamisel|
|ID |Jah|   Ressursihaldur ID autoscale säte|
|Nimi   |Jah|   Nime autoscale säte|
|üksikasjad|   Jah |Toiming, mille autoscale teenus võttis ja eksemplari arv muutuse selgitus|
|subscriptionId|    Jah |Tellimuse ID on on mastaabitud target ressursi|
|resourceGroupName| Jah|    Mis on on mastaabitud target ressursi ressursirühm nimi|
|resourceName   |Jah|   Mis on on mastaabitud target ressursi nimi|
|resourceType   |Jah|   Kolm toetatud väärtused: "microsoft.classiccompute/domainnames/slots/roles" – pilveteenuses rollid, "microsoft.compute/virtualmachinescalesets" - virtuaalse masina skaala komplektid ja "Microsoft.Web/serverfarms" - Web App|
|ResourceIdkasutamisel |Jah|Sihtrakenduse ressurss, mis on on mastaabitud ressursihaldur ID-d|
|portalLink |Jah    |Sihtrakenduse ressursi kokkuvõtteleht Azure portaali link|
|oldCapacity|   Jah |Praegune (vana) eksemplari arv Autoscale toimumise skaala toiming|
|newCapacity|   Jah |Uue eksemplari arv, mis Autoscale mastaabitud ressurssi|
|Atribuudid|    Ei| Valikuline. < Võtme väärtus > Sea paari (nt sõnastik < stringi, stringi >). Välja atribuutide pole kohustuslik. Saate sisestada kohandatud kasutajaliidese või loogika vastavalt rakenduse töövoo, võtmed ja väärtused, mis saab edasi olevat last. Alternatiivne võimalus edastada kohandatud atribuutide webhook helistamise on kasutada webhook URI ise (nagu päringu parameetrid)|
