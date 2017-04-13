<properties 
    pageTitle="Kuidas kasutada AMQP 1.0 .net-i teenuse siini API-ga | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada täiustatud sõnumi Queuing Protodol (AMQP) 1.0 Azure'i .net-i teenuse siini API." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Kuidas kasutada AMQP 1.0 teenuse siini .net-i API-ga

Funktsiooni täiustatud sõnumi järjekord protokolli (AMQP) 1.0 on tõhusa, usaldusväärseid, kaabel taseme sõnumside protokoll, mille abil saate koostada töökindlaid, mitu platvormi, sõnumside rakendused.

Tugiteenuste AMQP 1.0 teenuse siini tähendab, et saate kasutada funktsiooni queuing ja avaldamise/tellimise vahendatud sõnumside funktsioonide erinevatest seadmetest kasutab tõhusa kahendarvu protokolli kaudu. Lisaks saate luua rakendusi asuvaid komponentidest kasutavate keelte, raamistiku ja operatsioonisüsteemid.

Selles artiklis selgitatakse, kuidas teenuse siini vahendajaks sõnumside funktsioonide kasutamiseks (järjekorrad ja avaldamise/tellimise teemade) .net-i rakendustest API teenuse siini .net-i abil. On [companion artiklis](service-bus-java-how-to-use-jms-api-amqp.md) , mis selgitab, kuidas teha standard Java sõnumi teenuse (JMS) API sama abil. Saate need kaks juhendid koos platvormidel sõnumside AMQP 1.0 kasutamise kohta.

## <a name="get-started-with-service-bus"></a>Teenuse siini kasutamise alustamine

Selles artiklis eeldatakse, et teil on juba teenuse siini nimeruumi, mis sisaldab järjekorda nimega "queue1." Kui te ei saa, saate luua nimeruum ja [Azure portaali][]järjekord Teenuse siini nimeruumid ja järjekordade loomise kohta leiate lisateavet teemast [teenuse siini järjekorrad alustamine](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Laadige alla teenuste siini SDK

AMQP 1.0 tugi on saadaval teenuse siini SDK 2.1 või uuem versioon. Saate alla laadida uusima bittide Nugeti [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/)juures.

## <a name="code-net-applications"></a>Koodi .net-i rakendused

Vaikimisi teenuse siini .net-i kliendi teek suhtleb teenuse siini teenuse sihtotstarbeline SOAP-põhiste protokolli abil. AMQP 1.0 vaikimisi asemel kasutama protokolli nõuab konkreetsete konfiguratsiooni teenuse siini ühendusstring, nagu on kirjeldatud järgmises jaotises. Peale selle muudatuse rakenduse koodi ei muutu põhimõtteliselt AMQP 1.0 kasutamisel.

Praeguses versioonis on mõne API funktsioonid, mis pole toetatud AMQP kasutamisel. Neid Toetuseta funktsioonid on loetletud allpool jaotises [Toetuseta funktsioonid ja piirangud](#unsupported-features-and-restrictions). Teatud Täpsemad konfiguratsioonisätted on ka erinevat tähendust AMQP kasutamisel. Nende sätete ükski kasutatakse selle artikli, kuid rohkem üksikasju pole saadaval [teenuse siini AMQP ülevaade](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Kaudu App.config konfigureerimine

See on soovitatav tava, et rakenduste kasutamine App.config konfiguratsioonifail talletamiseks sätted. Teenuse siini rakendused, saate App.config teenuse siini **ConnectionString**talletamiseks. See valimi rakendus kasutab App.config ka teenuse siini sõnumside üksus, mis kasutab nime talletamiseks.

Siin on esitatud App.config näidisfaili:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Teenuse siini ühendusstringi konfigureerimine

**Microsoft.ServiceBus.ConnectionString** sätte väärtus on teenuse siini ühendusstring, mida kasutatakse konfigureerida teenuse siini ühendus. Vorming on järgmine:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Kui `[namespace]` ja `[SAS key]` saadakse [Azure portaali][]. Lisateavet leiate teemast [kasutamine teenuse siini järjekorrad] [].

AMQP kasutamisel ühendusstring on lisatud koos `;TransportType=Amqp`, mis ütleb kliendi teek selle ühenduse loomiseks teenuse siini AMQP 1.0 abil.

### <a name="configure-the-entity-name"></a>Üksuse nimi konfigureerimine

See näide rakendus kasutab funktsiooni `EntityName` **appSettings** osas App.config faili konfigureerida nimi, kellega rakenduse vahetab sõnumite järjekorda seadmine.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Lihtne .net-i rakenduse abil teenuse siini järjekord

Järgmises näites saadab ja võtab ja sealt teenuse siini järjekorda.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Käivitage rakendus

Töötab rakendus annab väljundi vormi:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Mitu platvormi vahel JMS ja .NET sõnumside

Selles teemas ilmnes sõnumite saatmise teenuse siini .net-i abil ka nende meilisõnumite .net-i abil tehke järgmist. Üks klahv eelised AMQP 1.0 on siiski, et see võimaldab rakendusi ehitatakse kirjutatud erinevates keeltes, usaldusväärselt ja täielik kvaliteedikadu vahetatud sõnumite komponentidest.

Eespool kirjeldatud valimi .net-i rakenduse ja võetud companion juhendi leiate [Java sõnumi teenuse (JMS) teenuse siini & AMQP 1.0 API kasutamise kohta](service-bus-java-how-to-use-jms-api-amqp.md), sarnaselt Java rakenduse abil on võimalik vahetada .NET keeleribaja Java sõnumeid. 

Mitu platvormi sõnumside kaudu teenuse siini ja AMQP 1.0 üksikasjade kohta leiate lisateavet teemast [teenuse siini AMQP 1.0 ülevaade](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS .net-i abil

.NET sõnumside näidata JMS:

* Käivitage rakendus .NET valimi ilma mis tahes käsurea argumendid.
* "Sendonly" käsurea argument on Java valimi rakendust käivitada. Selles režiimis ei saa rakenduse sõnumeid kaudu järjekorda, kuvatakse ainult saata.
* Vajutage sisestusklahvi **Enter** paar korda Java rakenduse konsooli, mis põhjusta sõnumeid saata.
* Need sõnumid saadetakse .net-i rakenduse.

### <a name="output-from-jms-application"></a>Väljundi JMS rakendusest

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>.Net-i rakenduse väljund

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.Net-i abil JMS

.NET näidata JMS sõnumside:

* Alustage .net-i valimi rakenduse "sendonly" käsurea argument. Selles režiimis ei saa rakenduse sõnumeid kaudu järjekorda, kuvatakse ainult saata.
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

.Net-i teenuse siini API järgmised funktsioonid on toetatud praegu AMQP kasutamisel:

 * Tehinguid
 * Saata edastamine sihtkoht

Lisateabe saamiseks vt [Mittetoetatavaid funktsioone, piirangud ja käitumist erinevused](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Kokkuvõte

Selles artiklis ilmnes kuidas pääseda juurde teenuse siini vahendajaks sõnumside funktsioonid (järjekorrad ja avaldamise/tellimise teemade) kaudu .NET AMQP 1.0 ja teenuse siini .NET API abil.

Samuti saate teenuse siini AMQP 1.0 muude keelte, sh Java, C, Python ja PHP. Osade keelte abil loodud vahetada sõnumite usaldusväärselt ja täielik AMQP 1.0 kasutamine teenuse siini kvaliteedikadu. Lisateavet leiate teemast [teenuse siini AMQP ülevaade](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete lugenud ülevaate teenuse siini ja AMQP .net-i abil, vaadake lisateabe saamiseks järgmisi linke.

* [Azure'i teenus siini AMQP 1.0 tugi](service-bus-amqp-overview.md)
* [Kuidas kasutada Java sõnumi teenuse (JMS) API teenuse siini & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Kuidas kasutada teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)
 
[Azure'i portaal]: https://portal.azure.com
