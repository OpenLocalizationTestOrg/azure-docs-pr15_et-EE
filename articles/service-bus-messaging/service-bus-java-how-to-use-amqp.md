<properties 
    pageTitle="Java teenuse siini API kasutamine AMQP 1.0 | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure teenuse ja täpsem sõnum Queueing Java sõnumi teenuse (JMS)"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"  
    manager="timlt" 
    editor="" />

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Java sõnumi teenuse (JMS) API AMQP 1.0 ja teenuse kasutamise kohta

Funktsiooni täiustatud sõnumi järjekord protokolli (AMQP) 1.0 on tõhusa, usaldusväärseid, kaabel taseme sõnumside protokoll, mille abil saate koostada töökindlaid, mitu platvormi, sõnumside rakendused. AMQP 1.0 tugi lisati Azure'i teenus siini oktoobris 2012 ja transitioned mai 2013 abil General Availability (GA).

Lisaks AMQP 1.0 tähendab, et on nüüd võimalik kasutada funktsiooni queuing ja avaldamise/tellimise vahendatud sõnumside funktsioonide teenuse siini erinevatest seadmetest kasutab tõhusa kahendarvu protokolli kaudu. Lisaks saate luua rakendusi asuvaid komponentidest kasutavate keelte, raamistiku ja operatsioonisüsteemid.

Õpetused juhend selgitab, kuidas kasutada teenuse siini vahendajaks sõnumside funktsioone (järjekorrad ja avaldamise/tellimise teemade) populaarsed Java sõnumi teenuse (JMS) API standardse Java rakendustest.

## <a name="get-started-with-service-bus"></a>Teenuse siini kasutamise alustamine

Sellest juhendist eeldab, et teil on juba teenuse siini nimeruumi, mis sisaldab nimega **queue1**järjekorda. Kui te ei saa, siis saate [luua nimeruum ja järjekorda](service-bus-create-namespace-portal.md) [Azure portaali](https://portal.azure.com). Lisateavet selle kohta, kuidas luua teenuse siini nimeruumid ja järjekorrad [teenuse siini järjekorrad kasutamise kohta](service-bus-dotnet-get-started-with-queues.md).

### <a name="downloading-the-amqp-10-jms-client-library"></a>Allalaadimine AMQP 1.0 JMS kliendi teek

Kui alla laadida uusima versiooni Apache Qpid JMS AMQP 1.0 kliendi teegi kohta leiate teavet veebisaidilt [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Peate lisama järgmised neli JAR failid arhiivist Apache Qpid JMS AMQP 1.0 jaotuse Java CLASSPATH kui koostamise ja töötab teenus siini JMS rakenduste:

*    Geronimo-jms\_1.1\_spec-1.0.jar
*    qpid-AMQP-1-0-Client-[Version].jar
*    qpid-AMQP-1-0-Client-jms-[Version].jar
*    qpid-AMQP-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Java rakenduste kodeerimine

### <a name="java-naming-and-directory-interface-jndi"></a>Java nimede ja Directory kasutajaliidese (JNDI)

JMS kasutab Java nimede ja Directory kasutajaliidese (JNDI) loogiline nimed ja füüsilise nimed lahutamine loomiseks. Kahte tüüpi JMS objektide nimed lahendatakse JNDI abil: ConnectionFactory ja sihtkoht. JNDI kasutab pakkuja mudeli, kuhu saate ühendada eri kataloogiteenuste nimi eraldusvõime ülesannete käsitlema. Apache Qpid JMS AMQP 1.0 teek on lihtne atribuudid failipõhiste JNDI pakkuja, mis on konfigureeritud järgmistest faili atribuutide abil vormindamine:

```
# servicebus.properties – sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[username]:[password]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Funktsiooni ConnectionFactory konfigureerimine

Kirje abil saate määratleda on **ConnectionFactory** Qpid atribuudid faili JNDI pakkuja on järgmises vormingus:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Kui **[jndi_name]** ja **[ConnectionURL]** on järgmised tähendused:

- **[jndi_name]**: selle ConnectionFactory loogiline nimi. See on nimi, mida lahendatakse Java rakenduse JNDI IntialContext.lookup() meetodi abil.
- **[ConnectionURL]**: A URL-i, mis pakub JMS teegi AMQP ta nõutava teabe.

**ConnectionURL** vorming on järgmine:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Kui **[nimeruum]**, **[kasutajanimi]** ja **[parool]** tähendavad järgmist:

- **[nimeruum]**: The teenuse siini nimeruum.
- **[kasutajanimi]**: The teenuse siini väljaandja nimi.
- **[parool]**: teenuse siini väljaandja võtme URL-kodeeringuga vorm.

> [AZURE.NOTE] Peate URL-i kodeerida parooli käsitsi. Kasulik URL-kodeeringuga kasuliku on saadaval veebisaidil [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Sihtkoht: konfigureerimine

Kirje abil saate määratleda sihtkohta Qpid atribuudid faili JNDI pakkuja on järgmises vormingus:

```
queue.[jndi_name] = [physical_name]
```
või

```
topic.[jndi_name] = [physical_name]
```

Kus **[jndi\_nimi]** ja **[füüsilise\_nimi]** tähendavad järgmist:

- **[jndi_name]**: sihtkohta loogiline nimi. See on nimi, mida lahendatakse Java rakenduse JNDI IntialContext.lookup() meetodi abil.
- **[physical_name]**: rakenduse saadab või saab sõnumeid teenuse siini üksuse nime.

> [AZURE.NOTE] Teenuse siini teema tellimuselt saamisel tuleks füüsilise nime määratud JNDI nime teema. Tellimuse nimi on esitatud püsival tellimuse loomisel JMS rakenduse koodi. [Teenuse siini AMQP 1.0 arendaja juhendi](service-bus-amqp-dotnet.md) leiate täpsemat töötamise teenuse siini teema tellimused: JMS kohta.

### <a name="write-the-jms-application"></a>Kirjutage JMS rakendus

Puuduvad teisiti API-d või võimalused on vaja teenuse siini JMS kasutamisel. Kuid on mõned piirangud, mida käsitletakse hiljem. Nagu mistahes rakendusega JMS esimese asjana nõutav on saama on **ConnectionFactory** ja sihtkohad JNDI keskkonna konfiguratsioon.

#### <a name="configure-the-jndi-initialcontext"></a>JNDI InitialContext konfigureerimine

JNDI keskkonna konfiguratsioon on Hashtable talletatakse konfiguratsiooniteavet sooritab ehitaja javax.naming.InitialContext klassi. Kahe nõutav on Hashtable talletatakse elementide on algse konteksti Factory ja URL-i pakkuja klassi nime. Järgmine kood kuvatakse JNDI keskkonnas kasutada Qpid faili atribuutide konfigureerimine põhjal JNDI pakkuja atribuudid faili nimega **servicebus.properties**abil.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Lihtne JMS rakendus abil teenuse siini järjekord

Järgmine näide programm saadab JMS TextMessages teenuse siini järjekorda JNDI loogiline nimi KUHJUDA ja saab sõnumeid tagasi.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Lookup ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Käivitage rakendus

Töötab rakendus annab järgmised väljundi:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Mitu platvormi vahel JMS ja .NET sõnumside

Sellest juhendist ilmnes kohta ja sealt teenuse siini sõnumite saatmiseks ja vastuvõtmiseks, kasutades JMS. Üks klahv eelised AMQP 1.0 on siiski, et see võimaldab rakendusi ehitatakse kirjutatud erinevates keeltes, usaldusväärselt ja täielik töökindluse vahetatud sõnumite komponentidest.

Eespool kirjeldatud valimi JMS rakendus ja sarnase .net-i rakenduse võetud companion juhendi leiate [AMQP 1.0 .net-i teenuse siini .NET API kasutamine](service-bus-dotnet-advanced-message-queuing.md), abil saate vahetada sõnumite .NET keeleribaja Java. 

Mitu platvormi sõnumside kaudu teenuse siini ja AMQP 1.0 üksikasjade kohta leiate lisateavet teemast [teenuse siini AMQP 1.0 arendaja juhend](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS .net-i abil

.NET sõnumside näidata JMS:

* Käivitage rakendus .NET valimi ilma mis tahes käsurea argumendid.
* "Sendonly" käsurea argument on Java valimi rakendust käivitada. Selles režiimis ei saa rakenduse sõnumeid kaudu järjekorda, kuvatakse ainult saata.
* Vajutage sisestusklahvi **Enter** paar korda Java rakenduse konsooli, mis põhjusta sõnumeid saata.
* Need sõnumid saadetakse .net-i rakenduse.

#### <a name="output-from-jms-application"></a>Väljundi JMS rakendusest

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>.Net-i rakenduse väljund

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.Net-i abil JMS

.NET näidata JMS sõnumside:

* Alustage .net-i valimi rakenduse "sendonly" käsurea argumenti. Selles režiimis ei saa rakenduse sõnumeid kaudu järjekorda, kuvatakse ainult saata.
* Käivitage rakendus Java valimi ilma mis tahes käsurea argumendid.
* Vajutage sisestusklahvi **Enter** paar korda .net-i rakenduse konsooli, mis põhjusta sõnumeid saata.
* Need sõnumid saadetakse Java rakendus.

#### <a name="output-from-net-application"></a>.Net-i rakenduse väljund

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Väljundi JMS rakendusest

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Toetuseta funktsioonid ja piirangud

Järgmised piirangud on olemas, kasutades JMS üle AMQP 1.0 koos teenuse siini, st:

* **Seansi**kohta on lubatud ainult üks **MessageProducer** või **MessageConsumer** . Kui teil on vaja luua mitme **MessageProducers** või **MessageConsumers** rakenduses, neid sihtotstarbeline **seansi** loomine
* Teema: hävivad tellimusi praegu ei toetata.
* **MessageSelectors** pole praegu toetatud.
* Ajutiste üksused, s.o **TemporaryQueue**, **TemporaryTopic** pole praegu toetatud, koos **QueueRequestor** ja **TopicRequestor** API-d, mis neid kasutavad.
* Tehingu seansid ja jaotatud tehingud ei toetata.

## <a name="summary"></a>Kokkuvõte

Selles artiklis ilmnes teenuse siini sõnumside funktsioonid (järjekorrad ja avaldamise/tellimise teemade) kasutamise kohta: Java populaarsed JMS API ja AMQP 1.0 abil.

Samuti saate teenuse siini AMQP 1.0 muude keelte, sh .NET, C, Python ja PHP. Neid erinevates keeltes abil loodud komponendid saate vahetada sõnumeid usaldusväärselt ja täielik töökindluse abil AMQP 1.0 tugi teenuse siini. Lisateabe saamiseks vt [teenuse siini AMQP 1.0 arendaja juhend](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Järgmised sammud

* [Azure'i teenus siini AMQP 1.0 tugi](service-bus-amqp-overview.md)
* [Kuidas kasutada AMQP 1.0 teenuse siini .net-i API-ga](service-bus-dotnet-advanced-message-queuing.md)
* [Teenuse siini AMQP 1.0 arendaja juhend](service-bus-amqp-dotnet.md)
* [Kuidas kasutada teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)
 
