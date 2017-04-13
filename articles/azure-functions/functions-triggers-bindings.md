<properties
    pageTitle="Azure'i funktsioonide käivitab ja sidumiste | Microsoft Azure'i"
    description="Mõista, kuidas kasutada päästikute ja sidumiste Azure'i funktsioonid."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i funktsioone, funktsioonide, event töötlus, webhooks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure'i funktsioonide päästikute ja seosed tootearendusmaterjal


Selles teemas antakse üldteatmematerjalid päästikute ja seosed. See sisaldab mõningaid täiustatud sidumine funktsioone ja kõik sidumine tüübid ei toeta süntaks.  

Kui otsite üksikasjalikku teavet ümber konfigureerida ja käivitada või sidumine kindlat tüüpi kodeerimise, võite klõpsake ühte allpool loetletud sidumiste või päästik:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Artiklitest Oletame, et olete lugenud [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md)ja [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)või [Node.js](functions-reference-node.md) arendaja viide artikleid.



## <a name="overview"></a>Ülevaade

Päästikute on teie kohandatud koodi käivitamiseks kasutatav sündmuse vastuseid. Need võimaldavad Azure'i platvormi üle või ettevõtete sündmuste vastata. Sidumiste tähistavad oma koodi ühendamiseks soovitud päästik või seotud sisend või väljund andmete vajalikud metaandmed.

Erinevate sidumiste, saate integreerida oma Azure'i funktsioon rakendusega parema ülevaate saamiseks vaadake järgmises tabelis.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Käivitab paremini mõistmiseks ja sidumiste üldiselt Oletagem, et soovite uue üksuse lähevad on Azure Storage järjekorda protsessi mõned koodi käivitada. Azure'i funktsioonid pakub selle toetamiseks lisamispäästiku Azure järjekorda. Peate, jälgida kuhjuda järgmine teave:
 
- Salvestusruumi konto, kui on olemas järjekorda.
- Järjekorra nimi.
- Muutuja nimi, mida teie koodi kasutaks viitamiseks uus üksus, mille jäeti järjekorda.  
 
Järjekorda päästik sidumine sisaldab see teave on Azure funktsiooni. Iga funktsiooni *function.json* fail sisaldab seotud kõik seosed.  Siin on näide *function.json* , mis sisaldab järjekorda käivitamine sidumine. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Teie koodi saata erinevat tüüpi väljundi sõltuvalt sellest, kuidas töödeldakse järjekorra uus üksus. Näiteks soovite kirjutada Azure Storage tabelisse uue kirje.  Selleks saate häälestada ka väljundi sidumine Azure Storage tabelisse. Siin on näide *function.json* , mis sisaldab salvestusruumi tabeli väljundi sidumine, järjekorra päästik koos kasutatud. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Järgmise C# funktsiooni vastaks uue üksuse sellest loobumist sellest kuhjuda ja kirjutab Azure Storage tabelisse uue kasutaja kirje.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Koodi näited ja Azure storage tüübid, mis on toetatud kohta täpsema teabe leiate [Azure'i funktsioonide päästikute ja Azure Storage seosed](functions-bindings-storage.md).


Azure'i portaalis sidumine täpsemate funktsioonide kasutamiseks klõpsake nuppu **Täpsem redaktor** suvand menüüs **integreerida** oma funktsiooni. Täpsem redaktor võimaldab teil muuta *function.json* otse portaalis.

## <a name="dynamic-parameter-binding"></a>Dünaamiline parameeter sidumine 

Oma väljundi sidumine atribuutide seadmine staatilise konfiguratsiooni asemel saate konfigureerida dünaamiliselt seotud andmeid, mis on osa teie päästik Sisestuskeel sidumine sätteid. Kaaluge stsenaarium, kui uued tellimused töödelda abil on Azure Storage järjekorda. Iga uue järjekorra üksus on JSON string, mis sisaldab vähemalt järgmised atribuudid:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Võiksite saata kliendi kontoga Twilio värskendus, et tellimuse võttis sõnumi teksti.  Saate konfigureerida selle `body` ja `to` välja oma Twilio väljund sidumine dünaamiliselt seotud selle `name` ja `mobileNumber` mis olid osa järgmiselt.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Funktsioon kood on nüüd ainult lähtestamine väljundi parameetri järgmiselt. Käitamise ajal väljundi atribuudid on seotud soovitud andmed.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Juhusliku GUID

Azure'i funktsioonid pakub süntaks, luua juhuslik GUID abil seosed. Uue BLOOBIMÄLU koos kordumatu nimi on Azure Storage ümbrises kirjutab väljundi sidumine järgmist süntaksit: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Ühe väljundi esitus

Juhul, kui teie funktsioon kood tagastab ühe väljundi, saate kasutada ka väljundi sidumine nimega `$return` säilitamise füüsilise funktsioon signatuuri koodi. Seda saab kasutada ainult keeles, mis toetavad tagastusväärtus (C#, Node.js, F #). Köitmine oleks umbes järgmine bloobimälu väljundi sidumine on kasutatud järjekorda päästik.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Järgmised C# koodi tagastab väljund rohkem loomulikult kasutamata mõne `out` parameetri funktsioon signatuur.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Sama viisi on näidatud allpool Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # näide on esitatud allpool.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

See saab kasutada ka koos mitme väljundi parameetrid määrates ühe väljundi koos `$return`.


## <a name="route-support"></a>Marsruutimiseks tugi

Kui loote funktsiooni HTTP päästik või WebHook, funktsioon on vaikimisi adresseeritavad ja marsruudi vormi:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Saate kohandada seda teed kasutades valikuline `route` atribuudi HTTP päästik kasutaja sisend sidumine. Näiteks järgmine *function.json* fail määratleb on `route` atribuudi lisamispäästiku HTTP:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Selle konfiguratsiooni kasutamisel on nüüd funktsiooni adresseeritavad järgmised protsessiga asemel algse marsruutimiseks.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

See võimaldab funktsioon koodi toetada kahte parameetrite aadress `category` ja `id`. Saate kasutada mis tahes [Web API marsruutimiseks piirang](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) oma parameetritega. Järgmine funktsioon kood C# kasutab nii parameetrid.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Siin on funktsioon Node.js koodi kasutada sama protsessi parameetrid.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Vaikimisi on kõik funktsioon marsruudib eelkinnitatud *API-ga*. Saate kohandada või eemaldada eesliite abil soovitud `http.routePrefix` atribuudi *host.json* failis. Järgmises näites eemaldab *api* marsruutimiseks eesliite abil *host.json* faili eesliite tühja stringi.

    {
      "http": {
        "routePrefix": ""
      }
    }

Üksikasjalikku teavet selle kohta, kuidas värskendada oma funktsiooni, vaadake, [Kuidas värskendada funktsioon rakenduse failid](functions-reference.md#fileupdate) *host.json* faili. 

Selle konfiguratsiooni lisamisega on nüüd adresseeritavad koos järgmist funktsiooni:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Muud saate konfigureerida *host.json* faili atribuutide kohta leiate teavet teemast [host.json juhend](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Järgmised sammud

Lisateavet järgmistest teemadest:

* [Funktsiooni testimine](functions-test-a-function.md)
* [Funktsiooni skaala](functions-scale.md)
