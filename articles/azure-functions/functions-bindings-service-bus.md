<properties
    pageTitle="Azure'i funktsioonide teenuse siini päästikute ja sidumiste | Microsoft Azure'i"
    description="Mõista, kuidas kasutada Azure'i teenus siini päästikute ja sidumiste Azure'i funktsioonid."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i töötab, funktsioonide, sündmuse töötlemiseks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure'i funktsioonide teenuse siini käivitab ja sidumiste järjekorrad, ja teemad

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerida ja koodi Azure'i teenus siini päästikute ja sidumiste Azure'i funktsioonide. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Teenuse siini järjekorda või teema päästik

#### <a name="functionjson"></a>function.JSON

*Function.json* faili saate määrata järgmised atribuudid.

- `name`: Muutuja nimi kasutatakse funktsioon koodi järjekorda või teema või sõnumi järjekorda või teema. 
- `queueName`: Jaoks järjekorra päästiku ainult küsitlus järjekorra nimi.
- `topicName`: Teema kohta käivitamine ainult nime teema küsitlus.
- `subscriptionName`: Teema kohta käivitamine ainult tellimuse nime.
- `connection`: Rakenduse säte, mis sisaldab teenuse siini ühendusstringi nimi. Ühendusstringi peab olema teenuse siini nimeruumi, mitte ainult mõne kindla järjekorda või teema. Kui Ühendusstring ei õiguste haldamine, määrake soovitud `accessRights` atribuut. Kui jätate `connection` tühi, käivitada või sidumine töötavad koos vaikimisi teenuse siini ühendusstringi app funktsioon, mis on määratud AzureWebJobsServiceBus rakenduse säte.
- `accessRights`: Saate määrata ühendusstring saadaval pääsuõigused. Vaikeväärtus on `manage`. Määratud `listen` kasutamisel ühendusstring, mis ei paku õiguste haldamine. Õiguste haldamine vastasel juhul ei suuda teha toiminguid, mis nõuavad ja ülesannete runtime proovida.
- `type`: Peab olema seatud *serviceBusTrigger*.
- `direction`: Peab olema seatud *sisse*. 

Näide *function.json* teenuse siini järjekorda päästik:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# koodi näide, mis töötleb teenuse siini järjekorda sõnumi

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # koodi näide, mis töötleb teenuse siini järjekorda sõnumi

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Töötleb teenuse siini järjekorda sõnumi Node.js koodi näide

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Toetatud andmetüübid

Mis tahes järgmist tüüpi saate sarjadesse teenuse siini järjekorda teade:

* Objekti (alates JSON)
* string
* baitide massiivis 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock käitumine

Käitusaja funktsioone saab sõnum `PeekLock` režiimi ja kõnede `Complete` sõnumit, kui funktsioon lõpule viidud või kõnede `Abandon` funktsiooni nurjumisel. Kui funktsioon töötab pikem kui selle `PeekLock` ajalõpp lukustus uuendatakse automaatselt.

#### <a id="sbpoison"></a>Mürki sõnumite töötlemine

Teenuse siini ei oma mürki sõnumite töötlemise, mis ei saa kontrollida või konfigureeritud Azure funktsioonide konfigureerimise või kood. 

#### <a id="sbsinglethread"></a>Ühe threading

Vaikimisi funktsioonide käitusaja töötleb mitme järjekorra sõnumite samaaegselt. Suunata korraga ainult ühe järjekorda või teema sõnumi töödelda runtime seadmine `serviceBus.maxConcurrrentCalls` 1 *host.json* faili. *Host.json* faili kohta leiate teavet teemast [Meilikausta ülesehitust](functions-reference.md#folder-structure) arendaja viide artikkel ja [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) WebJobs.Script hoidla viki sisse.

## <a id="sboutput"></a>Teenuse siini järjekorda või teema väljund sidumine

#### <a name="functionjson"></a>function.JSON

*Function.json* faili saate määrata järgmised atribuudid.

- `name`: Muutuja nimi kasutatakse funktsioon koodi järjekorda või järjekorra sõnum. 
- `queueName`: Jaoks järjekorda käivitamine ainult nime järjekorra küsitlus.
- `topicName`: Teema kohta käivitamine ainult nime teema küsitlus.
- `subscriptionName`: Teema kohta käivitamine ainult tellimuse nime.
- `connection`: Sama mis siini teenuse käivitamine.
- `accessRights`: Saate määrata ühendusstring saadaval pääsuõigused. Vaikeväärtus on `manage`. Määratud `send` kasutamisel ühendusstring, mis ei paku õiguste haldamine. Muul juhul käitusaja proovida funktsioonide ja ei suuda teha toiminguid, mis nõuavad hallata õigusi, näiteks järjekorrad loomine.
- `type`: Peab olema seatud *serviceBus*.
- `direction`: Peab olema seatud *välja*. 

Näide *function.json* teenuse siini järjekorda sõnumite kirjutamiseks timer päästik kasutamise kohta:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Toetatud andmetüübid

Azure'i funktsioonid saate luua teenuse siini järjekorda sõnumi mis tahes järgmist tüüpi.

* Objekti (alati JSON sõnumi, loob sõnumi null objekti, kui väärtus on null, funktsioon lõppemisel)
* string (loob sõnumi, kui väärtus on tühi, funktsioon lõppemisel)
* baitide massiivis (toimib nagu stringi) 
* `BrokeredMessage`(C#, töötab stringi)

Funktsiooni C# mitme sõnumi loomist saate kasutada `ICollector<T>` või `IAsyncCollector<T>`. Sõnum on loodud, kui helistate funktsiooni `Add` meetod.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# koodi näiteid, et teenuse siini järjekorda sõnumite koostamine

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # koodi näiteks, et teenuse siini järjekorda sõnumi

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Teenuse siini järjekorda sõnumi Node.js koodi näide

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
