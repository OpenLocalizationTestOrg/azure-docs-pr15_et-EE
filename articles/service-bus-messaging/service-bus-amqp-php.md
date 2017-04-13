<properties 
    pageTitle="Teenuse siini ja PHP koos AMQP 1.0 | Microsoft Azure'i"
    description="Teenuse siini alates PHP funktsiooniga AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Teenuse siini alates PHP abil AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Prootonpumba-PHP on PHP keele siduv prootonpumba-C; Seega prootonpumba-PHP rakendataks ümbrise läheduses mootori rakendatud C.

## <a name="downloading-the-proton-client-library"></a>Prootonpumba kliendi teek allalaadimine

Saate alla laadida prootonpumba-C ja selle seotud sidumiste (sh PHP) [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Allalaadimine on lähtekoodi kujul. Koodi loomiseks järgige allalaaditud pakett sisaldas.

> [AZURE.IMPORTANT] Kirjutamise ajal on saadaval Linuxi operatsioonisüsteemid ainult SSL-i tugiteenuste prootonpumba-c. Kuna siini Azure'i teenus nõuab SSL-i kasutamine, prootonpumba-C (ja keel sidumiste) saab kasutada ainult juurde pääseda Linux teenuse siini sel ajal. Töö lubamiseks prootonpumba-C SSL-iga Windows on pooleli, seega kontrollige sageli värskendusi tagasi.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Teenuse siini järjekorrad, teemade ja tellimuste alates PHP töötamine

Järgmine kood näitab, kuidas saata ja vastu võtta sõnumeid teenuse siini sõnumside üksus.

### <a name="sending-messages-using-proton-php"></a>Sõnumite saatmiseks prootonpumba-PHP abil

Järgmine kood näitab, kuidas sõnumi saatmiseks teenuse siini sõnumside üksus.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Sõnumite prootonpumba-PHP abil

Järgmine kood näitab, kuidas teile sõnumi teenuse siini sõnumside üksus.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>.Net-i ja prootonpumba-PHP sõnumside

### <a name="application-properties"></a>Teenuserakenduse atribuutide

#### <a name="protonphp-to-service-bus-net-apis"></a>Teenuse siini .net-i API-de ProtonPHP

Prootonpumba-PHP sõnumite toetavad rakenduse atribuutide järgmist tüüpi: **täisarv**, **kahekordsete**, **kahendväärtus**, **stringi**ja **objekti**. Järgmine kood PHP kujutatakse, kasutades järgmist tüüpi atribuuti iga sõnumi atribuutide seadmine.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

Klõpsake teenuse siini .NET API viiakse sõnumi rakenduse atribuutide [BrokeredMessage][] **atribuutide** kogum. Järgmine kood näitab, kuidas rakenduse atribuutide PHP kliendi saadud Sõnumi lugemiseks.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

Järgmises tabelis kaardid PHP atribuudi tüüpi .NET atribuudi tüüpi.

| PHP atribuudi tüüp | .Net-i atribuudi tüüp |
|-------------------|--------------------|
| täisarv           | int                |
| kahekordne            | kahekordne             |
| kahendmuutuja           | bool               |
| string            | string             |
| objekti            | Objekti             |

#### <a name="service-bus-net-apis-to-php"></a>Teenuse siini .net-i API-de php

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
```

Järgmine kood PHP näitab, kuidas rakenduse atribuutide teenuse siini .net-i kliendi saadud Sõnumi lugemiseks.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

Järgmises tabelis kaardid .NET atribuudi tüüpi PHP atribuudi tüüpi.

| .Net-i atribuudi tüüp | PHP atribuudi tüüp | Märkmete                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bait               | täisarv           | -                                                                                                                                                                     |
| sbyte              | täisarv           | -                                                                                                                                                                     |
| char               | Char              | Prootonpumba-PHP klassi                                                                                                                                                    |
| lühike              | täisarv           | -                                                                                                                                                                     |
| ushort             | täisarv           | -                                                                                                                                                                     |
| int                | täisarv           | -                                                                                                                                                                     |
| uint               | Täisarv           | -                                                                                                                                                                     |
| pikk               | täisarv           | -                                                                                                                                                                     |
| ulong              | täisarv           | -                                                                                                                                                                     |
| hõljumine              | kahekordne            | -                                                                                                                                                                     |
| kahekordne             | kahekordne            | -                                                                                                                                                                     |
| Decimal            | string            | Kümnendkohtade prootonpumba praegu ei toeta.                                                                                                                     |
| bool               | kahendmuutuja           | -                                                                                                                                                                     |
| GUID               | UUID              | Prootonpumba-PHP klassi                                                                                                                                                    |
| string             | string            | -                                                                                                                                                                     |
| Kuupäev ja kellaaeg           | täisarv           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks vastendatud AMQP tüüp:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Kuuline ajavahemik           | DescribedType     | Timespan.Ticks vastendatud AMQP tüüp:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri vastendatud AMQP tüüp:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standardatribuudid.

Järgmistes tabelites on prootonpumba-PHP standard sõnumi atribuudid ja [BrokeredMessage][] standard sõnumi atribuutide vastendust.

| Prootonpumba-PHP           | Teenuse siini .net-i         | Märkmete                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Püsival              | n/a                      | Teenuse siini toetab ainult püsival sõnumeid.          |
| Priority (prioriteet)             | n/a                      | Teenuse siini toetab vaid ühe sõnumi prioriteet. |
| TTL                  | Message.TimeToLive       | Teisendamine prootonpumba-PHP TTL on määratletud millisekundites.   |
| esimese\_omandaja      | -                          | -                                                          |
| kohaletoimetamise\_loendamine      | -                          | -                                                          |
| ID                   | Message.Id               | -                                                          |
| kasutaja\_id             | -                          | -                                                          |
| Meiliaadress              | Message.To               | -                                                          |
| Teema              | Message.Label            | -                                                          |
| vasta\_abil            | Message.ReplyTo          | -                                                          |
| korrelatsiooni\_id      | Message.CorrelationId    | -                                                          |
| sisu\_tüüp        | Message.ContentType      | -                                                          |
| sisu\_kodeerimine    | n/a                      | -                                                          |
| aegumise\_aeg         | Message.ExpiresAtUTC     | -                                                          |
| loomise\_aeg       | n/a                      | -                                                          |
| rühma\_id            | Message.SessionId        | -                                                          |
| rühma\_jada      | -                          | -                                                          |
| vasta\_et\_rühma\_id | Message.ReplyToSessionId | -                                                          |
| Vormindamine               | n/a                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Teenuse siini .net-i API-de prootonpumba-php

| Teenuse siini .net-i        | Prootonpumba-PHP                                             | Märkmete                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Sõnumi -\>sisu\_tüüp                                | -                                                        |
| CorrelationId           | Sõnumi -\>korrelatsiooni\_id                              | -                                                        |
| EnqueuedTimeUtc         | Sõnumi -\>marginaalid [x-valida-enqueued-time]             | -                                                        |
| Sildi                   | Sõnumi -\>teema                                      | -                                                        |
| MessageId               | Sõnumi -\>id                                           | -                                                        |
| OutboundSMTPServeri parameetrid                 | Sõnumi -\>vasta\_abil                                    | -                                                        |
| ReplyToSessionId        | Sõnumi -\>vasta\_et\_rühma\_id                         | -                                                        |
| ScheduledEnqueueTimeUtc | Sõnumi -\>marginaalid ["x-valida-ajastatud-nimekirja lisamine-kellaaeg"] | -                                                        |
| SeansiId               | Sõnumi -\>rühma\_id                                    | -                                                        |
| TimeToLive              | Sõnumi -\>ttl                                          | Teisendamine prootonpumba-PHP TTL on määratletud millisekundites. |
| Kui soovite                      | Sõnumi -\>aadress                                      | -                                                        |

## <a name="next-steps"></a>Järgmised sammud

Kas olete valmis saada lisateavet? Külastage järgmisi linke:

- [Teenuse siini AMQP ülevaade]
- [Klõpsake teenuse siini Windows Server AMQP]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[Klõpsake teenuse siini Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
[Teenuse siini AMQP ülevaade]: service-bus-amqp-overview.md
