<properties 
    pageTitle="AMQP 1.0 tugi teenuse siini liigendatud järjekorrad ja teemade | Microsoft Azure'i" 
    description="Lugege täpsemalt sõnumi järjekord protokolli (AMQP) 1.0 koos teenuse siini liigendatud järjekorrad ja teemade kasutamise kohta." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>Teenuse siini liigendatud järjekorrad ja teemade AMQP 1.0 tugi 

Azure'i teenus siini toetab nüüd täpsemalt sõnumi Queuing protokolli (**AMQP**) 1.0 teenuse siini jaoks **liigendatud järjekorrad ja teemade.**

**AMQP** on avatud-standard sõnumi andmebaasitõrge protokoll, mis võimaldab töötada platvormidel rakendusi kasutada erinevaid keeli. Üldist teavet AMQP toe teenuse siini leiate teemast [AMQP 1.0 teenuse siini tuge](service-bus-amqp-overview.md).

**Partitioned järjekorrad ja Teemad**, nimetatakse ka *sektsioonitud üksused*, pakuvad kõrgema kättesaadavus, töökindluse ja läbilaskevõime kui tavaliste-liigendatud järjekorrad ja teemade. Sektsioonitud üksuste kohta leiate lisateavet teemast [Sektsioonitud sõnumside üksused](service-bus-partitioning.md).

Lisaks AMQP 1.0 nimega Protocol (protokoll) suhtlemiseks sektsioonitud järjekorrad ja teemade võimaldab teil luua koostalitlusvõimeliste rakendusi saate suurema kättesaadavuse, töökindluse, ära ja kogu pakutud teenuse siini liigendatud üksused.

Üksikasjalik kaabel taseme AMQP 1.0 protokolli juhend, mis selgitab, kuidas teenuse siini rakendab ja tugineb OASIS AMQP tehnilised, leiate [Azure'i teenus siini ja sündmuse jaoturi AMQP 1.0 protokolli juhend](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Kasutage AMQP sektsioonitud järjekorrad abil

Järjekorrad on abiks stsenaariumi, mis nõuavad ajalised lahutamine, laadi ühtlustamise, koormusetasakaalustuseks ja lahti ühendada. Järjekorda, tootjad järjekorda sõnumeid saata ja tarbijad sõnumeid saada järjekorda, olukordades, kus sõnumi saate vastu võtta ainult üks kord. Hea näide on varude süsteem esmamüügikohti terminalid avaldada andmete asemel otse, et süsteemi järjekorda. Süsteemi tarbib andmeid seejärel laoseisu haldamise igal ajal. See on mitmeid eeliseid, sh süsteemi ei peaks kogu aeg olema võrgus. Teenuse siini järjekorrad kohta leiate lisateavet teemast [loomine rakendusi, mis kasutavad teenuse siini järjekorrad](service-bus-create-queues.md). 

Täpsemaks sektsioonitud järjekorda suurendab kättesaadavus, töökindlust ja läbilaskevõime rakenduste, nende järjekorrad on liigendatud mitmesse sõnumi maaklerid ja sõnumside poed.     

### <a name="create-partitioned-queues"></a>Sektsioonitud järjekorrad loomine

Sektsioonitud järjekorda [Azure klassikaline portaali][] ja teenuse siini SDK abil saate luua. Sektsioonitud järjekorra loomiseks määrake atribuudi [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) väärtuseks **true** [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) eksemplari. Järgmine kood näitab, kuidas luua teenuse siini SDK abil sektsioonitud järjekorda. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Kasutades AMQP sõnumite saatmiseks ja vastuvõtmiseks

Saate sõnumeid saata ja sõnumeid saada kasutamine AMQP protokolli, seades atribuuti [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) , nagu on näidatud järgmine kood sektsioonitud järjekorda.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Kasutage AMQP sektsioonitud Teemad

Teemade sarnanevad põhimõtteliselt järjekorrad, kuid teemade sama sõnumi koopia saate marsruutida mitu *tellimust*. Teema, tootjad teema sõnumeid saata ja tarbijad tellimuste sõnumeid saada. Laoseisu süsteemi esmamüügikohti stsenaariumi terminalid avaldada andmete teema. Süsteemi saab siis tellimuse sõnumeid. Lisaks jälgimise süsteemi saab saata sama sõnumit mõnda muud tellimust. Teenuse siini teemade ja tellimuste kohta leiate lisateavet teemast [loomine rakenduste kasutavad teenuse siini teemade ja tellimused](service-bus-create-topics-subscriptions.md). 

Sarnaselt järjekordade sektsioonitud teemade täpsemaks suurendada kättesaadavus, töökindlust ja läbilaskevõime rakenduste, nagu järgmisi teemasid ja nende tellimused on liigendatud mitmesse sõnumi maaklerid ja sõnumside poed. 

### <a name="create-partitioned-topics"></a>Sektsioonitud teemade loomine

Sektsioonitud teema [Azure klassikaline portaali][] ja teenuse siini SDK abil saate luua. Sektsioonitud teema loomiseks määrake atribuudi [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) väärtuseks **true** [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) eksemplari. Järgmine kood näitab, kuidas luua teenuse siini SDK abil sektsioonitud teema.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Kasutades AMQP sõnumite saatmiseks ja vastuvõtmiseks

Saate sõnumeid saata ja sõnumeid, mis sektsioonitud teema tellimusest, kasutades AMQP Protocol (protokoll), seades atribuudi [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), nagu on näidatud järgmine kood.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Järgmised sammud

Vaadake lisateavet sektsioonitud sõnumside üksuste kui ka AMQP järgmine täiendav teave.

*    [Sektsioonitud sõnumside üksused](service-bus-partitioning.md)
*    [Täpsemad sõnumi Queuing protokolli (AMQP) versiooni 1.0 OASIS](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Teenuse siini AMQP 1.0 tugi](service-bus-amqp-overview.md)
*    [Azure'i teenus siini ja sündmuse jaoturi protokolli juhendist AMQP 1.0](service-bus-amqp-protocol-guide.md)
*    [Java sõnumi teenuse (JMS) API AMQP 1.0 ja teenuse kasutamise kohta](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Kuidas kasutada AMQP 1.0 teenuse siini .net-i API-ga](service-bus-dotnet-advanced-message-queuing.md)

[Azure'i klassikaline portaal]: http://manage.windowsazure.com
