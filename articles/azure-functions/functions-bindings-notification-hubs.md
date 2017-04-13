<properties
    pageTitle="Azure'i funktsioonide teate jaoturi sidumine | Microsoft Azure'i"
    description="Mõista, kuidas kasutada Azure teate jaoturi sidumine Azure'i funktsioonid."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure'i funktsioonide teate jaoturi väljund sidumine

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerida ja koodi Azure'i teate jaoturi sidumiste Azure'i funktsioonides. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Oma funktsioonide saate saata Tõuketeatiste mõned koodiread konfigureeritud Azure teate jaoturi abil. Siiski Azure'i teate jaoturi peab olema konfigureeritud jaoks platvormi teatised teenused (PNS) soovite kasutada. Konfigureerimine on Azure teate jaoturi ja klientrakendustes, et registreerida, et saada teatisi arendamise kohta leiate lisateavet teemast [teatise jaoturi töötamise alustamine](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) ja teie target kliendi platvormi ülaosas nuppu.

Teatised, mis on saadetud saab kohalikke teatised või malli teatised. Kohalikke teatised target teatud teatise platvormi konfigureeritud selle `platform` väljundi sidumine atribuut. Malli teatise saab suunata mitme kaudu.   

## <a name="notification-hub-output-binding-properties"></a>Teatis jaoturi väljundi sidumine atribuudid

Function.json faili pakub järgmised atribuudid.

- `name`: Muutuja nimi, mida kasutatakse funktsioon koodi jaoturi teavitussõnumi.
- `type`: peab olema seatud *"notificationHub"*.
- `tagExpression`: Sildi avaldiste abil saate määrata, teatised kogumisse seadmed, teatisi, mis vastavad sildi avaldis registreerunud.  Lisateavet leiate teemast [marsruutimine ja sildi avaldiste](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Azure'i portaalis teatise jaoturi ressursi nimi.
- `connection`: Ühendusstringi see peab olema **Taotlus, milles** ühendusstringi määramine teie teate jaoturi *DefaultFullSharedAccessSignature* väärtus.
- `direction`: peab olema seatud *"välja"*. 
- `platform`: Atribuudi platvormi näitab teatis platvormi oma teatise eesmärgid. Peab olema üks järgmistest väärtustest:
    - `template`: See on vaikimisi platform kui atribuudi platform puudub väljundi sidumine kaudu. Malli teatised saate kasutada mis tahes platvormi konfigureeritud Azure teate jaoturi suunata. Saata rist platvormi teatiste on Azure teate jaoturi üldiselt mallide kasutamise kohta leiate lisateavet teemast [Mallid](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Apple'i Lükketeatiste teenus. Konfigureerimise kohta lisateabe saamiseks vt teate jaoturi APNs-i ja saanud teate kliendi rakenduses [saatmine Tõuketeatiste iOS Azure'i teatis jaoturi abil](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Seadme Amazon sõnumside](https://developer.amazon.com/device-messaging). ADM teate jaoturi konfigureerimine ja saavad Kindle rakenduse kohta leiate lisateavet teemast [Teatise jaoturi Kindle rakenduste kasutamise alustamine](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud sõnumside](https://developers.google.com/cloud-messaging/). Firebase'i pilve sõnumid, mis on uue versiooni GCM, toetatakse ka. Konfigureerimise kohta lisateabe saamiseks teate jaoturi GCM/FCM ja rakenduse Android klienti, saavad vaadata [saatmine Tõuketeatiste Android Azure'i teatis jaoturi abil](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows Push teatise Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) Windows platvormil suunamise. Windows Phone 8.1 ja uuemate toetab ka WNS. Konfigureerimise kohta lisateabe saamiseks leiate teate jaoturi WNS ja saanud teate Universal platvormil (UWP) rakenduses [teatis jaoturi jaoks Windowsi Universal platvormi rakendused töötamise alustamine](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsofti Lükketeatiste teenus](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). See platvorm toetab Windows Phone 8 ja Windows Phone varasemates platvormid. Konfigureerimise kohta lisateabe saamiseks vt teate jaoturi MPNS ja saavad Windows Phone rakenduses [saatmine Tõuketeatiste koos Azure teatis jaoturi kohta Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Näide function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Teatis jaoturi stringi häälestamine

Teatis jaoturi väljundi Köitmine kasutamiseks tuleb konfigureerida ühendusstringi jaoturi jaoks. Seda saab teha menüü *integreerida* , valides oma teate jaoturi või luua uue eksemplari. 

Ühendusstringi jaoks mõne olemasoleva jaoturi saate lisada ka käsitsi, lisades ühendusstringi jaoks *DefaultFullSharedAccessSignature* teie teate jaoturi. See ühendusstringi pakub funktsioon juurdepääsuõigus teatise saatmine. *DefaultFullSharedAccessSignature* ühenduse stringiväärtus pääseb juurde oma teatise jaoturi ressursi Azure'i portaalis peamised tera nuppu **klahvid** . Teie jaoturi jaoks ühendusstringi käsitsi lisamiseks tehke järgmist: 

1. **Funktsioon rakenduse** enne Azure portaali, klõpsake **funktsioon App sätted > Avage rakendus Teenusesätted**.

2. Klõpsake **sätete** labale **Rakenduse sätted**.

3. Liikuge kerides jaotiseni **ühendusstringi** ja lisage nimega kirje jaoks teie teate jaoturi *DefaultFullSharedAccessSignature* väärtus. Saate muuta **kohandatud**tüüpi.
4. Viide oma väljundi sidumiste ühenduse stringi nime. Sarnaselt **MyHubConnectionString** kasutada eeltoodud näites.

## <a name="native-notification-examples"></a>Kohalikke teatis näited

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APNs-i kohalikke teatiste järjekorda päästikute C#

Selles näites on kujutatud kohalikke APNs-i teatise saatmine määratletud [Microsoft Azure'i teatis jaoturi teegi](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) kasutamise kohta. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM kohalikke teatiste järjekorda päästikute C#

Selles näites on kujutatud kohalikke GCM teatise saatmine määratletud [Microsoft Azure'i teatis jaoturi teegi](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) kasutamise kohta. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS kohalikke teatiste järjekorda päästikute C#

Selles näites kujutatakse saatmiseks kohalikke WNS töölauateatise määratletud [Microsoft Azure'i teatis jaoturi teegi](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) abil. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Malli teatise näited

#### <a name="template-example-for-nodejs-timer-triggers"></a>Malli Node.js timer päästikute näide 

Selles näites saadab valitud isikule teatise [malli registreerimine](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , mis sisaldab `location` ja `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Ajastiteenuse päästikute F # malli näide

Selles näites saadab valitud isikule teatise [malli registreerimine](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , mis sisaldab `location` ja `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Välja parameetriga malli näide 

Selles näites saadab valitud isikule teatise [malli registreerimine](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , mis sisaldab soovitud `message` häälestusfaili malli.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Malli näide asünkroonne funktsiooniga

Kui kasutate asünkroonne kood, parameetreid, pole lubatud. Sel juhul kasutada `IAsyncCollector` teie malli teatise tagastamiseks. Järgmine kood on näide asünkroonne ülaltoodud kood. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Malli näide, mis kasutab JSON 

Selles näites saadab valitud isikule teatise [malli registreerimine](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , mis sisaldab soovitud `message` häälestusfaili malli abil kehtiv JSON-stringi.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Malli näide, mis kasutab teatise jaoturi Raamatukogu tüübid

Selles näites on määratletud [Microsoft Azure'i teatise jaoturi teegi](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)kasutamise kohta. 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
