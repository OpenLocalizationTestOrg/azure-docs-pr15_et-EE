<properties
    pageTitle="Azure'i funktsioonide sündmuse jaoturi sidumiste | Microsoft Azure'i"
    description="Mõista, kuidas kasutada Azure sündmuse jaoturi sidumiste Azure'i funktsioonid."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure'i funktsioonide sündmuse jaoturi seosed

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerida ja koodi [Azure'i sündmuse jaoturi](../event-hubs/event-hubs-overview.md) sidumiste Azure'i funktsioonide. Azure'i funktsioonide kasutajatugi päästik ja väljundi sidumiste Azure'i sündmuse jaoturi.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Kui olete kasutanud Azure'i sündmuse jaoturi, leiate [Azure'i sündmuse jaoturi ülevaade](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure'i sündmuse jaoturi käivitamine sidumine

Azure'i sündmuse jaoturi lisamispäästiku saab kasutada saadetud sündmuse jaoturi sündmuse voo sündmusele reageerimine. Lugemisõigus sündmuse jaoturi päästik sidumine häälestamiseks peab teil olema.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Sündmuse jaoturi jaoks function.JSON käivitamine sidumine

Azure'i sündmuse jaoturi lisamispäästiku *function.json* faili saate määrata järgmised atribuudid.

- `type`: Peab olema seatud *eventHubTrigger*.
- `name`: Muutuja nimi kasutatakse funktsioon koodi sündmuse jaoturi sõnumi. 
- `direction`: Peab olema seatud *sisse*. 
- `path`: Jaoturi sündmuse nimi.
- `consumerGroup`: See on mõni valikuline atribuudi seadmiseks [tarbija rühma](../event-hubs-overview.md#consumer-groups) sündmused keskuses tellida. Kui välja jäetud, kuvatakse `$Default` kasutatakse tarbija rühma. 
- `connection`: Säte, mis rakenduse nimi sisaldab nimeruumi, mis asub sündmuse jaoturi ühendusstring. Kopeerige see ühendusstring, klõpsates **Ühendusteabe** nuppu nimeruumi, mitte sündmuse jaoturi ise.  Selle ühendusstringi peab olema vähemalt lugemise õigused päästiku aktiveerimine.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure'i sündmuse jaoturi päästik C# näide
 
Näide function.json ülaltoodud kasutamisel sündmuse sõnumi kehasse logitakse C# funktsioon koodi kasutamise allpool:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Azure'i sündmuse jaoturi päästik F # näide

Näide function.json ülaltoodud kasutamisel sündmuse sõnumi kehasse logitakse F # funktsioon koodi kasutamise allpool:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure'i sündmuse jaoturi päästik Node.js näide
 
Näide function.json ülaltoodud kasutamisel sündmuse sõnumi kehasse logitakse Node.js funktsioon koodi allpool:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure'i sündmuse jaoturi väljund sidumine

Mõne Azure'i sündmuse jaoturi väljundi sidumine kasutatakse kirjutamise sündmuste sündmuse jaoturi sündmuse voo. Saada luba sündmuse jaoturiga sündmuste kirjutada selle peab teil olema. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Sündmuse jaoturi jaoks function.JSON väljund sidumine

*Function.json* fail on Azure sündmuse jaoturi väljund sidumine saate määrata järgmised atribuudid:

- `type`: Peab olema seatud *eventHub*.
- `name`: Muutuja nimi kasutatakse funktsioon koodi sündmuse jaoturi sõnumi. 
- `path`: Jaoturi sündmuse nimi.
- `connection`: Säte, mis rakenduse nimi sisaldab nimeruumi, mis asub sündmuse jaoturi ühendusstring. Kopeerige see ühendusstring, klõpsates **Ühendusteabe** nuppu nimeruumi, mitte sündmuse jaoturi ise.  See ühendusstringi peab olema sõnumi saatmiseks jaoturi sündmuse voo saatmise õigused.
- `direction`: Peab olema seatud *välja*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure'i sündmuse jaoturi C# koodi näide väljundi sidumine
 
Järgmine C# näide funktsioon kood näitab kirjutamise sündmuse voogu sündmuse jaoturi sündmus. Selles näites tähistab sündmuse jaoturi väljund sidumine eespool näidatud rakendatud C# timer päästik.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure'i sündmuse jaoturi F # koodi näide väljundi sidumine

Järgmine F # näide funktsioon kood näitab kirjutamise sündmuse voogu sündmuse jaoturi sündmus. Selles näites tähistab sündmuse jaoturi väljund sidumine eespool näidatud rakendatud C# timer päästik.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure'i sündmuse jaoturi Node.js koodi näide väljundi sidumine
 
Järgmine kood Node.js näide funktsioon näitab kirjutamise sündmuse voogu sündmuse jaoturi sündmus. Selles näites tähistab sündmuse jaoturi väljund sidumine eespool näidatud rakendatud Node.js timer päästik.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
