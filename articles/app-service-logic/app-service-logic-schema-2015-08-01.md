<properties 
    pageTitle="Uue skeemi versioon 2015-08-01-eelvaade" 
    description="Siit saate teada, kuidas kirjutada JSON definitsiooni loogika rakenduste uusima versiooni" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Uue skeemi versioon 2015-08-01-eelvaade

Uue skeemi ja API loogika rakenduste versioon on mitmeid parandusi, mille usaldusväärsuse parandamiseks ja hõlpsamaks kasutamise loogika rakenduste. On 4 Peamised erinevused.

1. Uue **APIConnection** toimingu tüüp on uuendatud **APIApp** toimingu tüüp.
2. **Korrake** ümbernimetatud **Foreach**.
3. **HTTP kuulajale** API rakendus pole enam vaja.
4. Uue skeemi helistamiseks lapse töövoogude kasutab.

## <a name="1-moving-to-api-connections"></a>1. teisaldada API ühendused

Suurim muudatus on see, et teil pole enam vaja juurutamine API rakendusi kasutada API Azure tellimuse sisse. On 2 võimalust, saate kasutada API-d.
* Hallatav API
* Oma kohandatud Web API

Kõik need käsitletakse veidi erineda, kuna nende haldamiseks ja majutuse mudelite erinevad. Üks eeliseid näites on te olete enam piiratud ressursse, mis on juurutatud ressursirühma. 

### <a name="managed-apis"></a>Hallatav API

Mitmesuguseid API, mida haldab Microsoft teie nimel, näiteks Office 365, Salesforce, Twitteri, FTP jne... Mõned nende hallatav API saab kasutada-on, nt Bing tõlkida, aga vajavad konfigureerimine. Selle konfiguratsiooni nimetatakse *ühendus*.

Kui kasutate teenusekomplekti Office 365, peate luua ühendus, mis sisaldab teie Office 365 sisselogimine luba. Selle märgiks salvestatakse turvaliselt ja värskendada, et teie loogika rakenduse saate alati helistada Office 365 API. Teise võimalusena, kui soovite oma FTP- või SQL serveriga ühendust luua, peate ühenduse loomiseks, mis on ühendusstring. 

Sees määratluse nimetatakse neid toiminguid `APIConnection`. Siin on näide ühendus, mis nõuab Office 365 e-posti saatmine:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Sisendeid osa, mis on kordumatu API ühendused on selle `host` objekti. See koosneb kahest osast: `api` ja `connection`.

Funktsiooni `api` on URL, kus mis hallatav API majutab runtime. Saate vaadata kõiki saadaval hallatava API-de jaoks helistades `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Kui kasutate API, võib või võib olla mis tahes **ühenduse parameetrid** . Kui see ei on nõutav puudub **ühendus** . Kui seda ei, siis on ühenduse loomiseks. Kui loote selle ühenduse, kui see on teie valitud nime ja seejärel viidatava, mis on `connection` objekti sees on `host` objekti. Ühenduse loomiseks ressursirühma helistada.

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Keha järgmist:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Juurutamine hallatav API-de Azure'i ressursi halduri mallis

Saate luua rakenduse täisversioon ARM mallis, kui see ei nõuaks Interaktiivne sisselogimine. Kui see nõuab sisselogimist, saate häälestada kõik ARM malliga, kuid on veel külastage ühendused lubada portaali. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Saate vaadata selle näite, et ühendused on lihtsalt tavaline ressursse, mis talletatakse teie ressursirühma. Need viitavad teie tellimus teile saadaval managedAPIs.

### <a name="your-custom-web-apis"></a>Kohandatud Web API-d

Kui kasutate oma API (täpsemalt, mitte Microsoft haldusega need), siis kasutage **HTTP** valmistoimingunuppude helistamine. Selleks, et optimaalne kogemus, peaks teie API jätke ärplema endpoint. See võimaldab loogika rakenduse designer renderdamiseks sisendi ja väljundi oma API. Ilma on ärplema kujundaja saab ainult sisendina ja väljundeid läbipaistmatu JSON objektina.

Siin on näide, kus on kuvatud uue `metadata.apiDefinitionUrl` atribuut:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Kui majutate oma Veebiteenuste **Rakenduse** teenuse, siis kuvatakse see automaatselt kuvada loendis Saadaolevad kujundajas toimingud. Kui ei, tuleb teil otse URL-i kleepida. Lõpp-punkti ärplema peab autentimata selleks, et kasutada sees loogika rakenduste designer (kuigi võib secure API abil soovitud ärplema toetatud meetodite olenemata).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>2015-08-01-eelvaatega juba käimasolevas API rakenduste kasutamine

Kui olete varem juurutanud API rakenduse, saate selle toimingu **HTTP** kaudu helistada.

Näiteks kui kasutate Dropboxi failide loend, teil võib olla umbes selline rakenduses definitsiooni **2014-12-01-eelvaade** skeemi versioon:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Saate luua sama HTTP toimingut nagu allpool (loogika rakenduse määratluse jääb muutmata parameetrite lõik):

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Jalutuskäigul atribuutidest ükshaaval:

| Toimingu atribuut |  Kirjeldus |
| --------------- | -----------  |
| `type` | `Http`Selle asemel`APIapp` |
| `metadata.apiDefinitionUrl` | Kui soovite seda toimingut kasutada Designeris loogika rakendused, mida soovite kaasata metaandmete lõpp-punkti. See on koostatud kaudu.`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | See on koostatud kaudu.`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Alati`POST` |
| `inputs.body` | Identne api rakenduse parameetrid | 
| `inputs.authentication` | Identne api rakenduse autentimine |

Seda moodust peaks töötama kõik API rakenduse toimingud. Kuid pidage meeles need eelmise API rakendused enam ei toetata, mis peaks teisaldamine üks kahest muude suvandist kohal (hallatav API või majutada oma kohandatud veebi-API).

## <a name="2-repeat-renamed-to-foreach"></a>2 uueks nimeks Foreach kordamine

Me ei saanud klientide tagasiside palju skeemi eelmise versiooni selle **korrake** oli segane ja ei õigesti jäädvustamiseks, et see on tegelikult on iga tsükkel jaoks. Selle tulemusena me on uueks nimeks see **Foreach**. Näiteks:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Soovite kirjutada nüüd nimega:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Varem funktsiooni `@repeatItem()` kasutati viitamiseks on näidata üle praeguse üksuse. See on lihtsustatud lihtsalt `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Funktsiooni Foreach väljundid viitamine
Veelgi lihtsustada mähitud väljundid **Foreach** toimingud ei **repeatItems**nimega objekti. See tähendab, samal ajal kui ülaltoodud korda väljundid:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Nüüd on see:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Kui viitate need väljundid, saada toimingu keha oleksite teha:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Nüüd saate teha selle asemel.

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Nende muudatuste funktsioonide `@repeatItem()`, `@repeatBody()` ja `@repeatOutputs()` eemaldatakse.

## <a name="3-native-http-listener"></a>3. kohalikke HTTP kuulajale 
HTTP kuulajale võimaluste on valmis, nüüd nii, et teil pole enam vaja HTTP kuulajale API rakenduse juurutamine. Lugege, [Kuidas teha oma loogika rakenduse lõpp-punkti sissenõutav siin kõiki üksikasju](app-service-logic-http-endpoint.md). 

Nende muudatuste funktsiooni `@accessKeys()` eemaldatakse ja on asendatud selle `@listCallbackURL()` funktsioon eesmärgil saada lõpp-punkti (vajadusel). Lisaks kohe peate määratlema vähemalt üks päästik loogika rakenduse kohe. Kui soovite `/run` töövoo, peate olema üks on `manual`, `apiConnectionWebhook` või `httpWebhook` päästikute. 

## <a name="4-calling-child-workflows"></a>4. helistaja lapse töövood

Varem, helistades lapse töövoogude nõutav, saab selle töövoo, saada juurdepääsu luba ja seejärel kleepida mis loogika rakendus, mida soovite kõne lapse määratlus. Uue skeemi versiooniga automaatselt genereerida loogika rakenduste engine on SAS käitusajal lapse töövoo, mis tähendab, et teil pole kleepida mis tahes saladusi määratluse.  Siin on näide:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Teine täiustamise on me annab lapse töövood täielik juurdepääs sissetulevate taotlusele. See tähendab, et saate edastada parameetreid jaotises *päringute* ja *päised* objekt ja, et saate määratleda täielikult kogu sisu.

Lõpuks on nõutav lapse töövoo muudatustest. Seetõttu võib lihtsalt vasakusse lapse töövoo otse; Nüüd, peate määratlemine päästik endpoint töövoo ema helistada. Üldiselt see tähendab, et saate lisate käivitamiseks on tippige **käsitsi** ja seejärel mis kasutamine vanem määratlus. Pange tähele, et selle `host` spetsiaalselt on atribuut on `triggerName`, kuna tuleb alati määrata, millised päästik, mida on kasutada.

## <a name="other-changes"></a>Muud muudatused

### <a name="new-queries-property"></a>Uue päringu atribuudi
Kõigi toimingu toetavad nüüd uue sisendit, nimetatakse **Päringud**. See võib olla liigendatud objekti asemel peaksite otsustama stringi käsitsi.

### <a name="parse-function-renamed"></a>Parse() funktsioon ümber
Kui meil kiiresti lisada veel sisutüüpe, on `parse()` funktsioon on ümber, et `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Tulekul: Enterprise integreerimine API-d
Aeg, ei ole veel õnnestunud versioonides Enterprise integreerimine API saadaval (nt AS2). Need tulevad varsti kaetud [juhised](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). Samal ajal, saate oma olemasoleva juurutatud BizTalki API kaudu HTTP toimingut, kui hõlma tekstivormingus "kasutab juba käimasolevas API rakenduste."
