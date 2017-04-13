<properties 
    pageTitle="Ülevaade: Azure'i sündmuse jaoturi API-de | Microsoft Azure'i"
    description="Kokkuvõte leiate ülevaate põhilistest sündmuse jaoturi .net-i klient API-d."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Sündmuse jaoturi API ülevaade

Selles artiklis on toodud mõned sündmuse jaoturi .net-i klient API võti. Kahte kategooriasse: haldus ja käitusaja API-d. Käitusaja API-de koosnevad kõik toimingud, mis on vaja saata ja vastu võtta sõnumi. Toimingute võimaldavad teil on sündmuse jaoturi üksuse olekus loomine, värskendamine ja kustutamine üksustele haldamine.

Jälgimisega seotud stsenaariumid hõlmavad juhtimis- ja käitusaja. Üksikasjalikku teavet dokumendid .net-i API-de kohta vaadake [Teenuse siini .net-i](https://msdn.microsoft.com/library/azure/mt419900.aspx) ja [EventProcessorHost API](https://msdn.microsoft.com/library/azure/mt445521.aspx) viited.

## <a name="management-apis"></a>API-de haldamine

Järgmised haldamise toimingute tegemiseks peab teil olema sündmuse jaoturi nimeruumi õiguste **haldamine** :

### <a name="create"></a>Loomine

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Värskendamine

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Kustutamine

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Käitusaja API-d

### <a name="create-publisher"></a>Publisheri loomine

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Sõnumi avaldamine

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Tarbija loomine

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Sõnumi maht peab olema

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Sündmuse protsessor host API-d

Nende API-d pakuvad paindlikkust töötaja protsessid, mis võib muutuda pole saadaval, jagades shards saadaval töötajate üle.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

Kasutajaliidese [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) defineeritakse järgmiselt:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Järgmised sammud

Sündmuse jaoturi stsenaariumid kohta lisateabe saamiseks külastage järgmisi linke:

- [Mis on Azure sündmuse jaoturi?](event-hubs-what-is-event-hubs.md)
- [Sündmuse jaoturi ülevaade](event-hubs-overview.md)
- [Sündmuse jaoturi programmeerimisega juhend](event-hubs-programming-guide.md)
- [Sündmuse jaoturi koodinäiteid](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

.Net-i API viited on siin.

- [Teenuse siini ja sündmuse jaoturi .NET API viited](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Sündmuse protsessor host API viide](https://msdn.microsoft.com/library/azure/mt445521.aspx)
