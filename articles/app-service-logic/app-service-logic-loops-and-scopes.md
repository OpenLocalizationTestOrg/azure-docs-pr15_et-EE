<properties
   pageTitle="Loogika rakenduste silmuseid, otsinguulatuste ja Debatching | Microsoft Azure'i"
   description="Loogika rakenduse tsükkel, ulatuse ja debatching mõisted"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Loogika rakenduste silmuseid, otsinguulatuste ja Debatching
  
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2016-04-01-preview skeemi ja uuemad versioonid.  Kontseptsioonid on sarnased vanemate skeemid, kuid otsinguulatuste on ainult selle skeemi saadaval ja uuemad versioonid.
  
## <a name="foreach-loop-and-arrays"></a>ForEach tsükkel ja massiive
  
Loogika rakenduste võimaldab pidev andmete kogumi arvestuses ja iga üksuse toimingu sooritamine.  See on võimalik kaudu soovitud `foreach` toiming.  Kujundaja, saate määrata lisamiseks on iga tsükkel jaoks.  Pärast valiku soovite üle kordamiseks massiiv, võite alustada toiminguid lisama.  Praegu teil on piiratud ainult ühe toimingu kohta foreach tsükkel, kuid see piirang tühistatakse järgmise nädala jooksul.  Kui sees kursis saate hakata määramiseks, mis peaks toimuma massiivi iga väärtuse.

Kui kood-view abil saate määrata mõne jaoks iga tsükkel, nagu allpool.  See on näide on iga tsükkel, mis saadab e-posti iga meiliaadressi, mis sisaldab "microsoft.com" jaoks:

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` toimingut saate itereerima üle massiivi kuni 5000 read.  Iga iteratsiooni käivitamiseks on vajalik lisada sõnumite järjekorda andmevoo juhtimine vajadusel samal ajal.
  
## <a name="until-loop"></a>Tsükkel kuni
  
  Saate teha toiming või toimingute sarja kuni tingimus on täidetud.  Kõige levinum stsenaarium helistab lõpp, kuni kuvatakse otsite vastus.  Designer, saate määrata lisamiseks on kuni tsükkel.  Pärast lisamist toimingud kursis sees, saate määrata välju tingimus, samuti kursis piirangud.  Tsükkel tsüklit vahel on 15 minutit viivitust.
  
  Kui kood-view abil saate määrata ka kuni tsükkel nagu allpool.  See on kujutatud helistamiseks HTTP lõpp kuni vastuse keha väärtus on "Lõppenud".  Millal see täielik kummagi 
  
  * HTTP-vastus on "Lõppenud" olek
  * See on 1 tund proovinud
  * See on looped 100 korda
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn ja Debatching

Mõnikord käivitamiseks saada massiivi üksused, mille soovite debatch ja kohta üksuse töövoo käivitamine.  Seda saab teha kaudu soovitud `spliton` käsk.  Kui teie päästik ärplema määrab last, mis on massiiv, on `spliton` lisatakse ja hakake kestab iga üksuse kohta.  SplitOn saab lisada ainult käivitamiseks.  See saate käsitsi konfigureeritud või alistada määratlus kood-vaadata.  Praegu saab debatch SplitOn massiivi kuni 5000 üksust.  Te ei saa mõnda `spliton` ja rakendada ka syncronous vastuse muster.  Mis tahes töövoo nimega, mis on mõne `response` toimingu Lisaks `spliton` kestab asyncronously ja saata kohe `202 Accepted` vastus.  

SplitOn saab määrata koodi vaates, nagu järgmises näites.  See saab massiivi üksuste ja debatches iga rea kohta.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Otsinguulatuste

On võimalik, mille ulatus koos kasutamise toimingute sarja rühmitamiseks.  See on eriti kasulik rakendamiseks erandi töötlemine.  Kujundajas saate lisada uue ulatuse ja alustada mis tahes toimingute sees lisamine.  Saate määratleda otsinguulatuste kood-vaadata näiteks järgmist:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
