<properties 
    pageTitle="Teenuse siini ja Python AMQP 1.0 | Microsoft Azure'i"
    description="Teenuse siini kaudu Python funktsiooniga AMQP."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>Teenuse siini kaudu Python funktsiooniga AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Prootonpumba-Python on Python keele siduv prootonpumba-C; Seega prootonpumba-Python rakendataks ümbrise läheduses mootori rakendatud C.

## <a name="download-the-proton-client-library"></a>Laadige alla prootonpumba kliendi teek

Saate alla laadida prootonpumba-C ja selle seotud sidumiste (sh Python) [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Allalaadimine on lähtekoodi kujul. Koodi loomiseks järgige sisalduvad allalaaditud pakett.

Võtke arvesse, et kirjutamise ajal SSL-i tugiteenuste prootonpumba-c ainult Linux opsüsteemide jaoks. Kuna siini Azure'i teenus nõuab SSL-i kasutamine, prootonpumba-C (ja keel sidumiste) saab kasutada ainult juurde pääseda Linux teenuse siini sel ajal. Töö lubamiseks prootonpumba-C SSL-iga Windows on käimas seega kontrollige sageli värskendusi tagasi.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Teenuse siini järjekorrad, teemade ja tellimused: Python töötamine

Järgmine kood näitab, kuidas saata ja vastu võtta sõnumeid teenuse siini sõnumside üksus.

### <a name="send-messages-using-proton-python"></a>Kasutades prootonpumba-Python sõnumite saatmine

Järgmine kood näitab, kuidas sõnumi saatmiseks teenuse siini sõnumside üksus.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Meilisõnumite abil prootonpumba-Python

Järgmine kood näitab, kuidas teile sõnumi teenuse siini sõnumside üksus.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>.Net-i ja prootonpumba-Python sõnumside

### <a name="application-properties"></a>Teenuserakenduse atribuutide

#### <a name="proton-python-to-service-bus-net-apis"></a>Prootonpumba-Python teenuse siini .net-i API-d

Prootonpumba-Python sõnumite toetavad rakenduse atribuutide järgmist tüüpi: **int**, **pikk**, **hõljumine**, **uuid**, **bool**, **string**. Python järgmine kood kuvatakse sõnumi atribuutide määramine, kasutades iga tüüpi atribuuti.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

Teenuse siini .NET API, viiakse sõnum rakenduse atribuutide [BrokeredMessage][] **atribuutide** kogum. Järgmine kood näitab, kuidas rakenduse atribuutide Python kliendi saadud Sõnumi lugemiseks.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Järgmises tabelis kaardid Python atribuudi tüüpi .NET atribuudi tüüpi.

| Python atribuudi tüüp | .Net-i atribuudi tüüp |
|----------------------|--------------------|
| int                  | int                |
| hõljumine                | kahekordne             |
| pikk                 | Int64              |
| UUID                 | GUID               |
| bool                 | bool               |
| string               | string             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Teenuse siini .net-i API-de prootonpumba-Python abil

Tippige [BrokeredMessage][] toetab rakenduse atribuutide järgmist tüüpi: **bait**, **sbyte**, **char**, **lühike**, **ushort**, **int**, **uint**, **pikk**, **ulong**, **hõljumine**, **kahekordsete**, **kümnendkohti**, **bool**, **Guid**, **stringi**, **Uri**, **kuupäev ja kellaaeg**, **DateTimeOffset**, ja **kuuline ajavahemik**. Järgmine .net-i kood kuvatakse iga tüüpi atribuudi abil [BrokeredMessage][] objekti atribuutide määramine.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Järgmine kood Python näitab, kuidas rakenduse atribuutide teenuse siini .net-i kliendi saadud Sõnumi lugemiseks.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

Järgmises tabelis kaardid .NET atribuudi tüüpi Python atribuudi tüüpi.

| .Net-i atribuudi tüüp | Python atribuudi tüüp | Märkmete                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bait               | int                  | -                                                                                                                                                                     |
| sbyte              | int                  | -                                                                                                                                                                     |
| char               | char                 | Prootonpumba-Python klassi                                                                                                                                                 |
| lühike              | int                  | -                                                                                                                                                                     |
| ushort             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| pikk               | int                  | -                                                                                                                                                                     |
| ulong              | pikk                 | Prootonpumba-Python klassi                                                                                                                                                 |
| hõljumine              | hõljumine                | -                                                                                                                                                                     |
| kahekordne             | hõljumine                | -                                                                                                                                                                     |
| Decimal            | String               | Kümnendkohtade prootonpumba praegu ei toeta.                                                                                                                     |
| bool               | bool                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Prootonpumba-Python klassi                                                                                                                                                 |
| string             | string               | -                                                                                                                                                                     |
| Kuupäev ja kellaaeg           | ajatempli            | Prootonpumba-Python klassi                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | DateTimeOffset.UtcTicks vastendatud AMQP tüüp:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Kuuline ajavahemik           | DescribedType        | Timespan.Ticks vastendatud AMQP tüüp:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Uri.AbsoluteUri vastendatud AMQP tüüp:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standardatribuudid.

Järgmistes tabelites on prootonpumba-Python standard sõnumi atribuudid ja [BrokeredMessage][] standard sõnumi atribuutide vastendust.

#### <a name="proton-python-to-service-bus-net-apis"></a>Prootonpumba-Python teenuse siini .net-i API-d

| Prootonpumba-Python        | Teenuse siini .net-i         | Märkmete                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| püsival              | n/a                      | Teenuse siini toetab ainult püsival sõnumeid.               |
| Priority (prioriteet)             | n/a                      | Teenuse siini toetab vaid ühe sõnumi prioriteet.      |
| TTL                  | Message.TimeToLive       | Teisendamine prootonpumba-Python TTL on määratletud millisekundites. |
| esimese\_omandaja      | n/a                      | -                                                           |
| kohaletoimetamise\_loendamine      | n/a                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| kasutaja\_id             | n/a                      | -                                                           |
| meiliaadress              | Message.To               | -                                                           |
| Teema              | Message.Label            | -                                                           |
| vasta\_abil            | Message.ReplyTo          | -                                                           |
| korrelatsiooni\_id      | Message.CorrelationID    | -                                                           |
| sisu\_tüüp        | Message.ContentType      | -                                                           |
| sisu\_kodeerimine    | n/a                      | -                                                           |
| aegumise\_aeg         | n/a                      | -                                                           |
| loomise\_aeg       | n/a                      | -                                                           |
| rühma\_id            | Message.SessionId        | -                                                           |
| rühma\_jada      | n/a                      | -                                                           |
| vasta\_et\_rühma\_id | Message.ReplyToSessionId | -                                                           |
| vormindamine               | n/a                      | -                                                           |

| Teenuse siini .net-i        | Prootonpumba                       | Märkmete                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.content\_tüüp        | -                                                           |
| CorrelationId           | Message.Correlation\_id      | -                                                           |
| EnqueuedTimeUtc         | n/a                          | -                                                           |
| Sildi                   | Message.Subject              | -                                                           |
| MessageId               | Message.ID                   | -                                                           |
| OutboundSMTPServeri parameetrid                 | Message.Reply\_abil            | -                                                           |
| ReplyToSessionId        | Message.Reply\_et\_rühma\_id | -                                                           |
| ScheduledEnqueueTimeUtc | n/a                          | -                                                           |
| SeansiId               | Message.Group\_id            | -                                                           |
| TimeToLive              | Message.TTL                  | Teisendamine prootonpumba-Python TTL on määratletud millisekundites. |
| Kui soovite                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Järgmised sammud

Kas olete valmis saada lisateavet? Külastage järgmisi linke:

- [Teenuse siini AMQP ülevaade]
- [Klõpsake teenuse siini Windows Server AMQP]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[Klõpsake teenuse siini Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx

[Teenuse siini AMQP ülevaade]: service-bus-amqp-overview.md
