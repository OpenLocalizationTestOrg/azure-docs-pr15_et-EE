<properties 
    pageTitle="Loogika rakenduse määratlused Autor | Microsoft Azure'i" 
    description="Saate teada, kuidas kirjutada JSON definitsiooni loogika rakendused" 
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
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Autori loogika rakenduse määratlused
Selles teemas näitab, kuidas kasutada [Azure loogika rakenduste](app-service-logic-what-are-logic-apps.md) määratlused, mis on lihtne, deklaratiivseid JSON keel. Kui te pole seda veel teinud, märkige kõigepealt, [Kuidas luua uus loogika rakendus](app-service-logic-create-a-logic-app.md) . Samuti saate lugeda [täielik viide materjali MSDN-i määratlus keele](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Mitu toimingut korrata üle loendi

Saate kasutada [foreach tüüp](app-service-logic-loops-and-scopes.md) korrata üle kuni 10 k massiivi üksuste ja iga toimingu sooritamine.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Kui tõrge käsitlemine samm

Soovite tavaliselt saama kirjutada *parandamise toimingu* – mõned loogika, mis käivitab, kui **ja ainult siis, kui**üks või mitu kõnede nurjus. Selles näites saame andmeid mitmest kohast, kuid kui kõne nurjub, soovin nii, et saate selgitada selle tõrke hiljem kuskil sõnumi POSTITAMINE.  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Saate teha kasutage funktsiooni `runAfter` atribuudi määramiseks soovitud `postToErrorMessageQueue` tuleks käitada ainult `readData` on **nurjunud**.  See võib olla ka loendi võimalikest väärtustest, nii et `runAfter` võib olla `["Succeeded", "Failed"]`.

Lõpetuseks, sest teil on nüüd töödeldud viga, me pole enam märgi Käivita **nurjus**. Nagu siin näete, praeguses käituses on **õnnestus** isegi juhul, kui ühe sammu võrra nurjunud, kuna kirjutasin etappi käsitlema selle tõrke.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Kaks (või rohkem) juhiseid, mis samal ajal käivituma

On mitu toimingute täitmise samal ajal soovitud `runAfter` atribuut peab vastama käitusajal. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Nagu näete mõlemad Ülaltoodud näites `branch1` ja `branch2` on seatud käivitamine `readData`. Selle tulemusena nii nende harude käivitage paralleelselt:

![Samal ajal](./media/app-service-logic-author-definitions/parallel.png)

Saate vaadata mõlemat ajatemplit on identne. 

## <a name="join-two-parallel-branches"></a>Kahe paralleelselt harude liitumine

Saate liituda kaks toimingud, lisades üksuste samal ajal käivitada määratud soovitud `runAfter` atribuudi sarnane kohal.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Samal ajal](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Loendiüksuste vastendus mõne muu konfigureerimine

Järgmise, Oletame, et soovime hoopis sisu sõltuvalt atribuudi väärtus. Loome väärtusi sihtkohta kaardil parameetrina:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

Sel juhul saame esmalt esemete loend, ja seejärel teise etapi otsib kaart, põhineb kategooria, mis on määratletud parameetrina, millist URL-i sisu kaudu. 

Kahe üksuste tähelepanu siin: selle [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funktsiooni abil saab kontrollida, kui kategooria vastab mõnele määratletud teadaolevad kategooria. Teiseks pärast kategooria saamiseks me tõmmata nurksulgude abil kaardi üksuse: `parameters[...]`. 

## <a name="working-with-strings"></a>Stringide töötamine

On erinevaid funktsioone, mis saab töödelda string. Vaatame näide, kus meil on string, mis soovime süsteemi edastada, kuid me ei ole kindel, et märkide kodeering käsitletakse õigesti. Üheks võimaluseks on base64 kodeerida stringile. Siiski vältimiseks pääseks URL-is me asendamiseks paar märki. 

Soovime alamstringi, soovitud tellimuse nime, kuna esmalt 5 märki ei kasutata.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Töö seest:

1. Hankida soovitud [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) sertifikaadiautoriteet isiku nimi, tagastab see tagasi märkide koguarvu

2. Lahutamine 5 (Kuna soovime kuvatakse lühemaks stringi)

3. Tegelikult võtta selle [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Alustame veebisaidil register `5` ja valige stringi ülejäänud.

4. Teisendage see alamstringi, et mõne [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)kõik selle `+` koos märki`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)kõik selle `/` koos märki`_`

## <a name="working-with-date-times"></a>Kuupäeva korda töötamine

Kuupäeva korda võib olla kasulik, eriti siis, kui soovite tõmmata andmete andmeallikast, mis ei toeta loomulikult **päästikute**.  Saate kuupäeva korda aru saada võtab kaua etapid. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

Selles näites me ekstraktimiseks on `startTime` , eelmist toimingut. Siis on saada praeguse kellaaja ja lahutamise ühe teise:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (võib kasutada muude ajaühikute `minutes` või `hours`). Lõpetuseks, saame võrrelda neist kahest väärtusest. Kui esimene on väiksem kui teine, siis see tähendab, et rohkem kui üks teine möödunud tellimuse esmakordselt paigutada. 

Ka teate, et saame kasutada stringi formatters vormindatavate kuupäevi: kasutada päringu stringi [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) soovitud RFC1123 saada. Kõik kuupäev vormindamine [MSDN-is on dokumenteerida](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Juurutamise / kellaaja parameetritega viibite

See on levinud on juurutamise elutsükli, kus teil on mõne arenduskeskkond, lavastus keskkonna ja tootmiskeskkonda. Kõik need on võib soovite sama määratluse, kuid kasutada eri andmebaasidega, näiteks. Samuti, mida soovite kasutada sama määratluse paljude erinevate piirkondade kõrge-saadavus, kuid soovite selle piirkonna andmebaasi rääkida iga loogika rakenduse eksemplari. 

Pange tähele, et see erineb võttes erinevate parameetrite *käitusaja*, et peaksite kasutama funktsiooni `trigger()` funktsioon nagu eespool kuvatud. 

Saate alustada väga lihtne määratlus umbes selline:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Seejärel klõpsake tegelik `PUT` taotlemine loogika rakenduse, saate sisestada parameetri `uri`. Võtke arvesse, kuna seal pole enam see parameeter on nõutav loogika rakenduse last vaikeväärtus.

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Iga keskkonnas seejärel andke muu väärtuse jaoks soovitud `connection` parameeter. 

Vaadake [REST API dokumentatsiooni](https://msdn.microsoft.com/library/azure/mt643787.aspx) kõiki võimalusi loomise ja haldamise loogika rakendused. 
