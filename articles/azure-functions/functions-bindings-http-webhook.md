<properties
    pageTitle="Azure'i funktsioonid HTTP ja webhook sidumiste | Microsoft Azure'i"
    description="Mõista, kuidas kasutada HTTP ja webhook päästikute ja sidumiste Azure'i funktsioonid."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure'i funktsioonid HTTP ja webhook seosed

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas kood HTTP ja webhook päästikute ja Azure funktsioonide seosed ja konfigureerimiseks. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>function.JSON HTTP ja webhook sidumiste jaoks

*Function.json* faili pakub atribuudid, mis on seotud nii koosolekukutsete ja kutsele vastamise.

HTTP-päringu atribuudid:

- `name`: Muutuja nimi, mida kasutatakse funktsioon koodi taotluse objekti (või koosolekukutse kehas Node.js funktsioonide puhul).
- `type`: Peab olema seatud *httpTrigger*.
- `direction`: Peab olema seatud *sisse*. 
- `webHookType`: WebHook päästikute, jaoks sobivad väärtused on *github*, *vaikne*ja *genericJson*. Http päästik, mis on WebHook pole seatud atribuut tühja stringi. WebHooks kohta leiate lisateavet teemast [WebHook päästikute](#webhook-triggers) järgmisest jaotisest.
- `authLevel`: Päästikute WebHook ei kehti. Määrake "funktsioon" nõua API võti "anonüümne" drop API võti nõue või "admin" nõua juhtslaidi API võti. Lisateavet leiate allpool [API võtmed](#apikeys) .

HTTP vastuse atribuudid:

- `name`: Kasutatav funktsioon koodi vastuse objekti Muutuv nimi.
- `type`: Peab olema seatud *http*.
- `direction`: Peab olema seatud *välja*. 
 
Näide *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>Päästikute WebHook

WebHook päästik on HTTP päästik, mis on mõeldud WebHooks järgmised funktsioonid:

* Teatud WebHook pakkujate (praegu GitHub ja vaikne ei toetata), funktsioonide käitusaja kinnitatakse teenusepakkuja allkirja.
* Node.js funktsioone, funktsioonide käitusaja pakub taotluse keha asemel taotluse objekti. On pole teisiti käsitsemise C# funktsioone, sest saate määrata, mis on esitatud, määrates parameetri tüüp. Kui määrate `HttpRequestMessage` saate taotluse objekti. Kui määrate POCO tüüp, proovib funktsioonide käitusaja sõeluda JSON objekti kehas taotluse asustamiseks objekti atribuudid.
* Käivitamiseks on WebHook funktsioon HTTP-päring peab sisaldama mõne API võti. WebHook HTTP päästikute, see nõue on valikuline.

GitHub WebHook häälestamise kohta leiate teemast [GitHub arendaja - WebHooks loomine](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>URL-i funktsiooni käivitamine

Funktsiooni käivitamiseks saadate HTTP-päring URL, mis on funktsioon rakenduse URL-i ja funktsiooni nimi:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API võtmed

Vaikimisi on API võti tuleb lisada mõne funktsiooni HTTP või WebHook käivitamiseks taotlusega HTTP. Võti saab lisada päringu stringi muutuja nimega `code`, või seda saab lisada ka `x-functions-key` HTTP-päis. WebHook funktsioone, saate määrata, et mõne API võti ei pea, seades selle `authLevel` *function.json* faili "anonüümne" atribuut.

Funktsioon rakenduse failisüsteemi kaustas *D:\home\data\Functions\secrets* leiate API väärtused.  Juhtslaidi võti ja funktsiooniklahv seatakse *host.json* faili, nagu on näidatud järgmises näites. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Funktsioon võtme *host.json* saab kasutada mis tahes funktsiooni, käivitamine, kuid ei Käivita keelatud funktsiooni. Juhtslaidi klahvi saab kasutada mis tahes funktsiooni käivitamiseks ja käivitab funktsiooni isegi juhul, kui see on keelatud. Saate konfigureerida funktsiooni nõudma juhtslaidi võti, seades selle `authLevel` atribuudi "administraator". 

Kui *saladusi* kaust JSON faili funktsiooni sama nimi on `key` atribuudi faili saate kasutada ka funktsiooni käivitamiseks ja viitab see funktsioon töötab ainult see võti. Näiteks API võti nimega funktsiooni `HttpTrigger` on määratud *HttpTrigger.json* *saladusi* kausta. Siin on näide:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Kui häälestate WebHook päästik, ei jaga juhtslaidi võti WebHook pakkuja. Kasutage võti, mis töötab ainult funktsiooni, mis on WebHook töötleb.  Juhtslaidi klahvi saab kasutada mis tahes funktsiooni käivitamiseks isegi keelatud funktsioone.

## <a name="example-c-code-for-an-http-trigger-function"></a>Näide C# koodi HTTP päästik funktsioonile 

Näide kood otsib on `name` parameeter päringu string või HTTP koosolekukutse kehasse.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>Näide F # koodi HTTP päästik funktsioonile

Näide kood otsib on `name` parameeter päringu string või HTTP koosolekukutse kehasse.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Vajate mõne `project.json` faili, mis kasutab Nugeti viitamiseks on `FSharp.Interop.Dynamic` ja `Dynamitey` assemblereid umbes järgmine:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Seda kasutatakse Nugeti toomiseks teie sõltuvused ja viitab neid oma skripti.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Näide Node.js kood HTTP päästik funktsioonile 

Selles näites kood otsib on `name` parameeter päringu string või HTTP koosolekukutse kehasse.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Funktsiooni GitHub WebHook näide C# koodi. 

Selles näites kood logib GitHub probleemi kommentaarid.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Funktsiooni GitHub WebHook näide F # koodi.

Selles näites kood logib GitHub probleemi kommentaarid.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Näide Node.js GitHub WebHook funktsioon koodi. 

Selles näites kood logib GitHub probleemi kommentaarid.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
