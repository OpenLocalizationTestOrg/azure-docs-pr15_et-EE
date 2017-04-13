<properties
    pageTitle="Azure'i funktsioonide Twilio sidumine | Microsoft Azure'i"
    description="Mõista, kuidas kasutada Twilio sidumiste Azure'i funktsioonidega."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure'i funktsioonide Twilio väljund sidumine

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerimine ja kasutamine Twilio sidumiste Azure'i funktsioonidega. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure'i funktsioonide toetab Twilio väljund sidumiste lubamiseks oma funktsioonide SMS tekstsõnumid paar rida koodi ja [Twilio](https://www.twilio.com/) konto. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Azure'i teate jaoturi jaoks function.JSON väljund sidumine

Function.json faili pakub järgmised atribuudid.

- `name`: Muutuja nimi, mida kasutatakse funktsioon koodi Twilio SMS-sõnumiga.
- `type`: peab olema seatud *"twilioSms"*.
- `accountSid`: Rakenduse säte, mis hoiab teie Twilio konto Sid nimi peab olema seatud see väärtus.
- `authToken`: Rakenduse säte, mis hoiab teie Twilio autentimise luba nimi peab olema seatud see väärtus.
- `to`: Selle väärtuseks on seatud SMS tekst saadetud telefoninumber.
- `from`: Selle väärtuseks on seatud SMS tekst saadetud telefoninumber.
- `direction`: peab olema seatud *"välja"*.
- `body`: Väärtus seda saab kasutada kõva koodi SMS-sõnumiga, kui te ei vaja määraks dünaamiliselt kood oma funktsiooni. 

 
Näide function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Näide C# järjekorra päästik koos Twilio väljund sidumine

#### <a name="synchronous"></a>Sünkroonse

See sünkroonse näide koodi lisamispäästiku Azure Storage järjekorda kasutab teksti sõnumi saatmine kliendile, kes tellimuse on välja parameeter.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asünkroonne

See asünkroonne näide koodi lisamispäästiku Azure Storage järjekorra saadab sõnumi teksti kliendi tellimuse.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Näide Node.js järjekorra päästik koos Twilio väljund sidumine

Selles näites Node.js saadab sõnumi teksti kliendi tellimuse.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
