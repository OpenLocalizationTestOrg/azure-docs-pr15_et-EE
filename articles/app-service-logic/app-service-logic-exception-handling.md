<properties
   pageTitle="Loogika rakenduste erandi töötlemine | Microsoft Azure'i"
   description="Siit saate teada, mustrite tõrge ja Azure loogika rakendustega töötlemise erand"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Loogika rakenduste viga ja erandi töötlemine

Loogika rakenduste pakub suurel hulgal erinevaid tööriistad ja mustrid tagada teie integratsioon on robustne ja olles vastu ebaõnnestumist.  Üks probleeme, mis tahes integreerimine arhitektuuri on tagada, et tööseisakute või sõltuvad süsteemide probleeme käsitletakse õigesti.  Loogika rakenduste teeb töötlemise tõrked esimese klassi kogemusi, annab teile tööriistad, mida vajate erandid ja sees oma töövoogude tõrgete kohta.

## <a name="retry-policies"></a>Proovige poliitika

Kõige olulisemaid erand ja tõrge töötlemine on uuesti poliitika.  Selle poliitika määratleb, kui toiming peaks uuesti, kui algne koosolekukutse aegus või nurjus (mis tahes kutse, mille tulemuseks on 429 või 5xx vastus).  Vaikimisi kõik toimingud uuesti 4 korda kursor 20 sekundi järel.  Jah, kui esimene kutse saanud on `500 Internal Server Error` vastust töövoo engine peatab 20 sekundit ja proovi kutse uuesti.  Kui pärast kõigi korduskatsed vastus on endiselt erand või tegevusetuse, töövoo jätkab ja märkida toimingu oleku nimega `Failed`.

Saate konfigureerida **sisendina** teatud toimingu uuesti poliitika.  Proovi uuesti poliitika saab konfigureerida proovida nii palju 4 korda ühe tunni jooksul intervalle.  Kõigi üksikasjadega Sisestuskeel atribuutide leidub [MSDN-is][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Kui soovite HTTP toimingu uuesti 4 korda ja oodake 10 minutit iga katse vahel on teil järgmised määratlus.

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Toetatud süntaksi kohta lisateabe saamiseks vaata [uuesti-poliitika jaotist MSDN-is][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>RunAfter atribuudi kinni püüdma tõrked

Iga loogiline rakenduse toiming kinnitab, milliseid toiminguid on vaja enne hakkab toimingu lõpuleviimiseks.  Teil võib võrrelda selle oma töövoo etappide tellimine.  See tellimine nimetatakse funktsiooni `runAfter` atribuudi toimingu määratlus.  See on objekti, mis kirjeldab, milliseid toiminguid ja toimingu olekuid täitma toimingu.  Vaikimisi on seatud kõik toimingud, mis on lisatud designer `runAfter` eelmises etapis kui eelmises etapis oli `Succeeded`.  Siiski saate kohandada seda väärtust tule toimingud, kui eelnenud toiminguid `Failed`, `Skipped`, või need väärtused võimalike kogum.  Kui soovite üksuse lisamiseks määratud teenuse siini teema pärast teatud toimingu `Insert_Row` nurjub, saate kasutada järgmisi `runAfter` konfigureerimine:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Teade kuvatakse `runAfter` kui atribuudi väärtuseks on seatud soovitud `Insert_Row` toiming on `Failed`.  Toimingu käivitamiseks, kui toiming olek on `Succeeded`, `Failed`, või `Skipped` oleks süntaks:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Toimingud, mida pärast ei ole eelmise toimingu käivitada ja lõpule viia, tähistatakse `Succeeded`.  Selline käitumine tähendab, et kui te edukalt saagi kõik ebaõnnestumist töövoo ise Käivita on märgitud `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Otsinguulatuste ja tulemusi hinnata toimingud

Sarnane kuidas käivitada pärast üksikuid toiminguid, saate ka rühmitada toimingud koos sees [ulatus](app-service-logic-loops-and-scopes.md) – toimivad loogiline rühmitamine toimingud.  Otsinguulatuste on kasulikud loogika rakenduse toimingute korraldamiseks ja läbimiseks liitväärtuse hinnangute ulatus oleku kohta.  Pärast kõigi toimingute ulatus on lõpule viidud saate ise ulatuse olek.  Ulatus olek määratakse kriteeriumidega, mis kestab – kui lõplik toiming on täitmise kontoris on `Failed` või `Aborted` olek on `Failed`.

Saate `runAfter` mille ulatus on märgitud `Failed` tule teatud toimingute jaoks mis tahes piires ilmnenud tõrkeid.  Pärast nurjub, mille ulatus võimaldab teil luua ühe toimingu kinni püüdma tõrkeid, kui *mis tahes* toimingute piires nurjuda.

### <a name="getting-the-context-of-failures-with-results"></a>Saada kontekstis tõrkeid tulemustega

Püüdmine tõrkeid, mille ulatus on väga kasulik, kuid võite ka konteksti mõista täpselt nurjus, toimingud ja vigu või tagastatud olek koode.  Funktsiooni `@result()` töövoo funktsioon pakub kontekstis üheks tulemus kõik toimingud, mille ulatus sees.

`@result()`võtab ühe parameetri, ulatuse nimi ja tagastab massiivi kõik toimingu tulemused ulatuse piirides.  Need toimingu objektid sisaldavad sama nimega atribuudid on `@actions()` objekti, sh toimingu algusaja, toimingu lõppkellaaeg, toimingu olek, toimingu sisendina, toimingu korrelatsiooni ID-d ja toimingu väljund.  Saate hõlpsalt siduda mõne `@result()` toimida on `runAfter` sees, mille ulatus kontekstis kõik toimingud, mida ei saanud saata.

Kui soovite käivitada toimingut *iga* toimingu ulatuses mis `Failed`, saate siduda `@result()` **[Filter massiiv](../connectors/connectors-native-query.md)** toimingu ja **[ForEach](app-service-logic-loops-and-scopes.md)** tsükkel.  See võimaldab teil filtreerida toimingud, mida ei saanud tulemuste massiivi.  Saate teha filtreeritud tulem massiiv ja iga rikkumise, kasutades **ForEach** tsükkel toimingu sooritamine.  Siin on näide allpool, millele järgneb üksikasjaliku selgituse.  Selles näites saadab HTTP POST-taotluse vastuse keha kõik toimingud, mida ei saanud piires `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Mis toimub üksikasjaliku selgituse leiate siit.

1. **Filtri massiiv** toimingu filtreerimiseks on `@result('My_Scope')` tulemi kõik toimingud sees`My_Scope`
1. **Filtri massiiv** on mis tahes `@result()` üksus, mille olek on võrdne `Failed`.  See filtreerib kõik toimingu tulemuste massiivi `My_Scope` ainult nurjunud toimingu tulemuste massiivi abil.
1. Väljundeid **Filtreeritud massiivi** **iga** toimingut teha.  See viib läbi soovitud toimingu *iga* nurjus toimingu tulem me filtreeritud kohal.
    - Kui ühe ulatusega, mida ei saanud toimingud toimingu soovitud `foreach` läheks ainult üks kord.  Nurjunud mitmekaupa põhjustaks ühe toimingu nurjumise kohta.
1. Saata mõne HTTP POST on `foreach` üksuse vastuse keha, või `@item()['outputs']['body']`.  Funktsiooni `@result()` üksuse kujund on sama, mis on `@actions()` kujundamine ja sõeluda samal viisil.
1. Ka kaks kohandatud päised nurjunud toiming nimega `@item()['name']` ja nurjunud käivitada kliendi ID jälgimise `@item()['clientTrackingId']`.

Siin on näide ühe viide; `@result()` üksus.  Saate vaadata selle `name`, `body`, ja `clientTrackingId` atribuudid sõeluda eeltoodud näites.  Tuleb märkida väljaspool, mis on `foreach`, `@result()` tagastab massiivi neid objekte.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Ülaltoodud avaldiste abil teha erinevate mustrite töötlemise erand.  Võite valida üks erand töötlemise väljapoole, mida aktsepteerib tõrkeid ja Eemalda kogu filtreeritud massiiv toimingu käivitada soovitud `foreach`.  Võite ka muud kasulikud atribuudid on `@result()` vastuse eespool näidatud.

## <a name="azure-diagnostics-and-telemetry"></a>Azure'i diagnostika- ja telemeetria

Ülaltoodud mustrite on suurepärane viis käsitlema tõrgete ja erandid kestab sees, kuid saate tuvastada ja vastata tõrgete sõltumatu ise Käivita.  [Azure'i diagnostika](app-service-logic-monitor-your-logic-apps.md) võimaldab saata kõigi (sh kõik Käivita ja toimingu olekud) töövoosündmused Azure Storage konto või mõne Azure'i sündmuse jaoturi.  Saate jälgida logid ja mõõdikute või avaldada need kõik eelistate hinnata Käivita olekud jälgimise tööriista.  Võimalikud üheks võimaluseks on voona üheks [Voo Analytics](https://azure.microsoft.com/services/stream-analytics/)kõigi sündmuste Azure'i sündmuse keskuse kaudu.  Voo Analytics kirjutamise reaalajas päringute välja mis tahes kõrvalekaldeid, keskmiste või tõrkeid diagnostika logid kaudu.  Voo Analytics saate hõlpsalt väljund teistest andmeallikatest, nt järjekorrad, teemade, SQL-i, DocumentDB ja Power BI.

## <a name="next-steps"></a>Järgmised sammud
- [Kuidas üks kliendi kuvamiseks koostada töökindlaid tõrketöötluse loogika rakendustega](app-service-logic-scenario-error-and-exception-handling.md)
- [Loogika rakenduste näited ja stsenaariumid](app-service-logic-examples-and-scenarios.md)
- [Saate teada, kuidas luua automatiseeritud juurutuste loogika rakenduste](app-service-logic-create-deploy-template.md)
- [Kujundamine ja juurutamine Visual Studio loogika rakendused](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9