<properties 
    pageTitle="Teenuse siini ja Java koos AMQP 1.0 | Microsoft Azure'i"
    description="AMQP teenuse siini kaudu Java abil"
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

# <a name="use-service-bus-from-java-with-amqp-10"></a>Teenuse siini kaudu Java AMQP 1.0 kasutamine

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Java sõnumi teenuse (JMS) on standardne API Java platvormi sõnumi rakendusse vahevara töötamiseks. Microsoft Azure'i teenus siini on testitud koos AMQP 1.0 vastavalt projekti Apache Qpid välja töötatud JMS kliendi teek. See teek toetab täielik JMS 1.1 API ja saab kasutada mis tahes AMQP 1.0 ühilduv teenus. Selle stsenaariumi toetatakse ka [Teenuse siini Windows Server](https://msdn.microsoft.com/library/dn282144.aspx) (kohapealse teenuse siini). Lisateavet leiate teemast [AMQP teenuse siini Windows Server][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Laadige alla Apache Qpid AMQP 1.0 JMS kliendi teek

Lisateabe saamiseks allalaadimine Apache Qpid JMS AMQP 1.0 kliendi teek uusim versioon, külastage [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

Peate lisama järgmised neli JAR failid arhiivist Apache Qpid JMS AMQP 1.0 jaotuse Java CLASSPATH kui koostamise ja töötab teenus siini JMS rakenduste:

-   Geronimo-jms\_1.1\_spec-[versioon] laiendid

-   qpid-AMQP-1-0-Client-[Version].jar

-   qpid-AMQP-1-0-Client-jms-[Version].jar

-   qpid-AMQP-1-0-Common-[Version].jar

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Teenuse siini järjekorrad, teemasid ja tellimused: JMS töötamine

### <a name="java-naming-and-directory-interface-jndi"></a>Java nimede ja Directory kasutajaliidese (JNDI)

JMS kasutab Java nimede ja Directory kasutajaliidese (JNDI) loogiline nimed ja füüsilise nimed lahutamine loomiseks. Kahte tüüpi JMS objektide nimed lahendatakse JNDI abil: **ConnectionFactory** ja **sihtkoht**. JNDI kasutab pakkuja mudeli, kuhu saate ühendada eri kataloogiteenuste nimi eraldusvõime ülesannete käsitlema. Apache Qpid JMS AMQP 1.0 teek on lihtne atribuudid failipõhiste JNDI pakkuja, mis on konfigureeritud tekstifaili abil.

Qpid atribuudid faili JNDI pakkuja on konfigureeritud, kasutades järgmises vormingus faili atribuudid:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>Ühenduse factory konfigureerimine

Kirje abil saate määratleda on **ConnectionFactory** Qpid atribuudid faili JNDI pakkuja on järgmises vormingus:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Kui `[jndi\_name]` ja `[ConnectionURL]` tähendavad järgmist:

| Nimi            | Tähendus                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | Ühenduse factory loogiline nimi. See nimi on lahendatud Java rakenduse abil soovitud JNDI `IntialContext.lookup()` meetod. |   |   |   |   |
| `[ConnectionURL]` | URL, mis pakub JMS teegi AMQP ta nõutava teabe.                                                      |   |   |   |   |

Vormingu ühenduse URL on järgmine:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Kui `[namespace]`, `[username]`, ja `[password]` tähendavad järgmist:

| Nimi          | Tähendus                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | Teenuse siini nimeruumi saadud [Azure portaali][].                      |   |   |   |   |
| `[username]`  | Teenuse siini SAS võtme nimi saadud [Azure portaali][].                    |   |   |   |   |
| `[password]`  | Teenuse siini SAS võti saadud [Azure portaali][]URL-kodeeringuga vorm. |   |   |   |   |

> [AZURE.NOTE] Peate URL-i kodeerida parooli käsitsi. Kasulik URL-i kodeering kasuliku on saadaval veebisaidil [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

Näiteks kui saadud teabe portaalis on järgmine:

| Namespace:   | test.ServiceBus.Windows.net                  |
|--------------|----------------------------------------------|
| Väljaandja nimi: | RootManageSharedAccessKey                                        |
| Väljaandja võti:  | ABCDEFG |

Klõpsake, et määratleda **ConnectionFactory** objekti nimega `SBCONNECTIONFACTORY`, konfiguratsiooni string on järgmised:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Sihtkoht: konfigureerimine

Kirje, mis määratleb sihtkohta Qpid atribuudid faili JNDI pakkuja on järgmises vormingus:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Kui `[jndi\_name]` ja `[physical\_name]` tähendavad järgmist:

| Nimi              | Tähendus                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | Sihtkoha loogiline nimi. See on nimi on lahendatud Java rakenduse abil soovitud JNDI `IntialContext.lookup()` meetod. |
| `[physical\name]` | Rakendus saadab või saab sõnumeid teenuse siini üksuse nimi.                                                  |

Võtke arvesse järgmist.

- Funktsiooni `[physical\name]` väärtus võib olla teenuse siini järjekorda või teema.
- Teenuse siini teema tellimuselt saamisel tuleks füüsilise nime määratud JNDI nime teema. Tellimuse nimi on esitatud püsival tellimuse loomisel JMS rakenduse koodi.
- Samuti on võimalik teenuse siini teema tellimuse käsitleda JMS järjekorda. Seda moodust annab mitmeid eeliseid: sama vastuvõtja koodi saab kasutada järjekorrad ja teema tellimused ja kõik aadressiteabe (teema ja tellimuse nimed) on externalized faili atribuudid.
- Teenuse siini teema tellimuse käsitleda JMS järjekorda, kirje atribuudid faili peaks olema vormi: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Loogika JMS sihtkoha nimega "Teema", mida saab vastendada teenuse siini teema nimega "Teema1" määratlemiseks oleks kirje failis atribuudid järgmiselt:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Saata sõnumeid, kasutades JMS

Järgmine kood näitab, kuidas teenuse siini teema sõnumeid saata. Eeldatakse, et `SBCONNECTIONFACTORY` ja `TOPIC` **servicebus.properties** konfiguratsioonifailis on määratletud eelmises jaotises kirjeldatud.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Meilisõnumite abil JMS

Järgmine kood kuvatakse `how` saada sõnumi teema teenuse siini tellimusest. Eeldatakse, et `SBCONNECTIONFACTORY` ja teema on määratletud **servicebus.properties** konfiguratsioonifail eelmises jaotises kirjeldatud. Samuti eeldatakse, et tellimuse nimi on `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Juhised koostamise töökindlaid rakendusi

JMS spetsifikatsioon määratleb, kuidas erandi lepingu API meetodite ja rakenduse kood tuleks alati kirjutada erandite käsitlema. Siin on mõned muud punktid kaaluda erandi käsitsemise.

-   Mõne **ExceptionListener** registreerima JMS ühenduse **connection.setExceptionListener**abil. See võimaldab mõnda muud klienti teavitama asünkroonselt probleem. See teatis on eriti oluline ühendusi, mis kasutavad ainult sõnumid, mis ei ole muul viisil teavet, et ei ole oma ühenduse. **ExceptionListener** nimetatakse, kui seal on probleem AMQP aluseks ühendus, seansil või lingi. Sellisel juhul tuleks rakenduse programmi uuesti luua **JMS ühendus**, **seansi**, **MessageProducer** ja **MessageConsumer** objektide algusest peale ise luua.

-   Veenduge, et sõnum on edukalt saadetud kaudu on **MessageProducer** teenuse siini üksusele, veenduge, et rakendus on konfigureeritud on **qpid.sync\_avaldamine** süsteemi atribuutide kogum. Saate seda teha, käivitage programm ja selle **-Dqpid.sync\_avaldamine = true** Java VM suvand rakenduse käivitamisel käsurea seadmine. Valitud suvand konfigureerib tulu ei saada kõne seni, kuni on saadud kinnitus, mis sõnumi aktsepteerinud teenuse siini teegis. Probleemi ilmnemisel saada selle toimingu käigus tõstetakse on **JMSException** . On kaks võimalikku põhjust. 
    1. Kui probleem on teenuse siini lükake need tagasi kindla sõnumi saatmise tõttu, siis kuvatakse tõstetakse **MessageRejectedException** erand. See tõrge on ajutine, või mõni probleem sõnumi tõttu. Soovitatav toiming on avaldab teha mitu toimingut mõne loogika tagasi välja. Kui probleem ei lahene, siis sõnum tuleks loobuda viga kohalikult sisse loginud. Ei ole vaja uuesti luua **Ühendus JMS**, **seansil**või **MessageProducer** objektide sellisel juhul. 
    2. Kui probleem on teenuse siini sulgemata AMQP tõttu, siis kuvatakse tõstetakse **InvalidDestinationException** erandi. See võib olla põhjuseks on ajutine probleem, või sõnumi üksus on kustutatud. Mõlemal juhul **JMS ühendus**, **seansi**ja **MessageProducer** objektide uuesti luua. Kui kuvatakse tõrketeade oli ajutine, siis seda toimingut lõpuks ei viida edukalt. Kui üksus on kustutatud, kuvatakse tõrge eemaldatakse jäädavalt.

## <a name="messaging-between-net-and-jms"></a>Sõnumside .net-i ja JMS vahel

### <a name="message-bodies"></a>Sõnumi keha

JMS määratleb viit tüüpi: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**ja **TextMessage**. Ühe sõnumi tüüp, [BrokeredMessage][]pole teenuse siini .net-i API-ga.

#### <a name="jms-to-service-bus-net-api"></a>Teenuse siini .NET API JMS

Järgmistes jaotistes näitab, kuidas tarbimine sõnumite iga JMS sõnumitüübid .net-i kaudu. Näide **ObjectMessage** ei ole kaasatud, on **ObjectMessage** sisu sisaldab sarjadesse jaotatav objekti sisse Java programmeerimiskeel, mis ei ole Rienso .net-i rakendus.

##### <a name="bytesmessage"></a>BytesMessage

Järgmine kood näitab, kuidas keha **BytesMessage** objekti abil teenuse siini .net-i API-de kasutamine.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

Järgmine kood näitab, kuidas tarbimine **MapMessage** objekti keha teenuse siini .net-i API-de abil. Järgmine kood seni, kuni kaart, kuvades väärtuse iga elemendi nime ja elementide.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

Järgmine kood näitab, kuidas keha **StreamMessage** objekti abil teenuse siini .net-i API-de kasutamine. Järgmine kood on ära toodud kõik selle voo koos nende tüüpi üksused.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

Järgmine kood näitab, kuidas keha **TextMessage** objekti abil teenuse siini .net-i API-de kasutamine. Järgmine kood kuvatakse sõnumi sisalduvad tekstistring.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Teenuse siini .net-i API-de JMS abil

Järgmistes jaotistes näitab, kuidas .net-i rakenduse saab luua teade, mis on saadud iga JMS sõnumi tüüpide JMS. Näide **ObjectMessage** ei ole kaasatud, on **ObjectMessage** sisu sisaldab sarjadesse jaotatav objekti sisse Java programmeerimiskeel, mis ei ole Rienso .net-i rakendus.

##### <a name="bytesmessage"></a>BytesMessage

Järgmine kood näitab, kuidas luua [BrokeredMessage][] objekti lisamine **BytesMessage**JMS kliendi kätte .net-i.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

Järgmine kood näitab, kuidas luua [BrokeredMessage][] objekti lisamine **StreamMessage**JMS kliendi kätte .net-i.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

Järgmine kood näitab, kuidas sisu on **TextMessage** kasutamine API teenuse siini .net-i abil. Järgmine kood kuvatakse sõnumi sisalduvad tekstistring.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Teenuserakenduse atribuutide

####<a name="jms-to-service-bus-net-apis"></a>Teenuse siini .net-i API-de JMS

JMS sõnumite toetavad rakenduse atribuutide järgmist tüüpi: **kahendväärtus**, **bait**, **lühike**, **int**, **pikk**, **hõljumine**, **kahekordsete**ja **String**. Java järgmine kood kuvatakse sõnumi atribuutide määramine, kasutades iga tüüpi atribuuti.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

Klõpsake teenuse siini .NET API viiakse sõnumi rakenduse atribuutide [BrokeredMessage][] **atribuutide** kogum. Järgmine kood näitab, kuidas rakenduse atribuutide JMS kliendi saadud Sõnumi lugemiseks.

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

Järgmine tabel näitab, kuidas JMS atribuudi tüüpi kaart .NET atribuudi tüüpi.

| JMS atribuudi tüüp | .Net-i atribuudi tüüp |
|-------------------|--------------------|
| Bait              | sbyte              |
| Täisarv           | int                |
| Hõljumine             | hõljumine              |
| Kahekordne            | kahekordne             |
| Kahendmuutuja           | bool               |
| String            | string             |

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

Järgmine kood Java näitab, kuidas rakenduse atribuutide teenuse siini .net-i kliendi saadud Sõnumi lugemiseks.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

Järgmine tabel näitab, kuidas .NET atribuudi tüüpi kaart JMS atribuudi tüüpi.

| .Net-i atribuudi tüüp | JMS atribuudi tüüp | Märkmete                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bait               | UnsignedByte      | -                                                                                                                                                                      |
| sbyte              | Bait              | -                                                                                                                                                                     |
| char               | Märk         | -                                                                                                                                                                     |
| lühike              | Lühike             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| int                | Täisarv           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| pikk               | Pikk              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| hõljumine              | Hõljumine             | -                                                                                                                                                                     |
| kahekordne             | Kahekordne            | -                                                                                                                                                                     |
| Decimal            | BigDecimal        | -                                                                                                                                                                     |
| bool               | Kahendmuutuja           | -                                                                                                                                                                     |
| GUID               | UUID              | -                                                                                                                                                                     |
| string             | String            | -                                                                                                                                                                     |
| Kuupäev ja kellaaeg           | Kuupäev              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks vastendatud AMQP tüüp:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Kuuline ajavahemik           | DescribedType     | Timespan.Ticks vastendatud AMQP tüüp:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri vastendatud AMQP tüüp:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Standardse päised

Järgmistes tabelites on kuidas JMS Standard päised ja [BrokeredMessage][] standardatribuudid on vastendatud AMQP 1.0 abil.

#### <a name="jms-to-service-bus-net-apis"></a>Teenuse siini .net-i API-de JMS

| JMS              | Teenuse siini .net-i               | Märkmete                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Pole praegu saadaval        | Teenuse siini toetab vaid püsival sõnumid; näiteks DeliveryMode.PERSISTENT, olenemata sellest, mis on määratud.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Sõnumi. TimeToLive            | Teisendamine                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Vaikimisi on JMSMessageID kodeeritud kahendarvu AMQP sõnumi vorm. .Net-i kliendi teek saamist kahendarvu sõnumi-id, teisendab stringi kujutis baitide Unicode'i väärtuste põhjal. Asemel JMS teegi kasutamiseks stringi sõnumi ID-d, lisage soovitud "binary-messageid = false" päringu parameetrite JNDI ConnectionURL stringi. Näide: “amqps://[username]:[password]@[namespace].servicebus.windows.net? binary-messageid = false ". |
| JMSPriority      | Pole praegu saadaval        | Teenuse siini ei toeta sõnumi prioriteet.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Pole praegu saadaval        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Sõnumi. OutboundSMTPServeri parameetrid               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Teisendamine                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms tüüp"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Teenuse siini .net-i API-de JMS abil

| Teenuse siini .net-i        | JMS              | Märkmete                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Pole praegu saadaval |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Teisendamine              |
| Sildi                   | n/a              | Pole praegu saadaval |
| MessageId               | JMSMessageID     | -                         |
| OutboundSMTPServeri parameetrid                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | n/a              | Pole praegu saadaval |
| ScheduledEnqueueTimeUtc | n/a              | Pole praegu saadaval |
| SeansiId               | n/a              | Pole praegu saadaval |
| TimeToLive              | JMSExpiration    | Teisendamine              |
| Kui soovite                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Toetuseta funktsioonid ja piirangud

Järgmised piirangud on olemas üle AMQP 1.0 koos teenuse siini JMS kasutamisel:

-   Seansi kohta on lubatud ainult üks **MessageProducer** või **MessageConsumer** . Kui soovite mitme **MessageProducer** või **MessageConsumer** objekti loomine rakenduses, luua sihtotstarbeline seansid neid.

-   Teema: hävivad tellimusi praegu ei toetata.

-   **MessageSelector** objekte ei toetata.

-   Ajutiste sihtkohta; näiteks **TemporaryQueue** või **TemporaryTopic**, ei toetata, koos **QueueRequestor** ja **TopicRequestor** API-d, mis neid kasutavad.

-   Tehingu seansid ei toetata.

-   Jaotatud tehingud ei toetata.

## <a name="next-steps"></a>Järgmised sammud

Kas olete valmis saada lisateavet? Külastage järgmisi linke:

- [Teenuse siini AMQP ülevaade]
- [Klõpsake teenuse siini Windows Server AMQP]

[Klõpsake teenuse siini Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Teenuse siini AMQP ülevaade]: service-bus-amqp-overview.md
[Azure'i portaal]: https://portal.azure.com
