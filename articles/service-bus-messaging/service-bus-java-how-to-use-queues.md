<properties
    pageTitle="Teenuse siini järjekorrad kasutamine Java | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure teenuse siini järjekorrad. Koodinäiteid kirjutatud Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Kuidas kasutada teenuse siini järjekorrad

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini järjekorrad. Näidiste Java kirjutada ja kasutada [Azure SDK Java][]. Hõlmatud stsenaariumide hulka **loomise järjekorrad**, **sõnumite saatmine ja vastuvõtt**ja **järjekorrad kustutamine**.

## <a name="what-are-service-bus-queues"></a>Mis on teenuse siini järjekorrad?

Teenuse siini järjekorrad toetavad **vahendatud sõnumside** side mudel. Kui kasutate järjekorrad, komponentide jaotatud rakendus ei otse omavahel suhelda; selle asemel nad vahetada sõnumeid järjekorda, mis on vahendajaks (ta) kaudu. Sõnumi tootja (saatja) käed sõnumi järjekorda välja ja seejärel jätkub töötlemine.
Asünkroonselt, sõnumi tarbija (vastuvõtja) tõmbab sõnumi kuhjuda ja töötleb. Tootja ei pea ootama vastuse tarbija jätkamiseks töötlemine ja sõnumite saatmine. Järjekorrad pakkuda **esimese, esimese välja FIFO** kohaletoimetamise ühe või mitme konkureerivate tarbijatele. Seega sõnumid on tavaliselt vastu ja töödelda vastuvõtjad järjestuses, kus need olid varem lisatud kuhjuda ja iga sõnumi on saanud ja vaid ühe sõnumi tarbija töödelda.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Teenuse siini järjekorrad on üldotstarbeline tehnoloogia, mida saab kasutada erinevaid stsenaariumeid lähemalt.

- Suhtlemine mitmekihilise Azure rakenduse web ja töötaja rollid.
- Suhtlemine kohapealse rakendused ja Azure majutatud rakendused hübriid lahendus.
- Suhtlemine jaotatud rakendus töötab kohapealse eri ettevõtted või ettevõttes osakondade komponendid.

Abil järjekorrad võimaldab teil hõlpsam välja rakenduste mastaapimiseks ja lubada paindlikkust oma arhitektuur.

## <a name="create-a-service-namespace"></a>Teenuse nimeruumi loomine

Azure'i teenus siini järjekorrad kasutamine alustamiseks peate esmalt looma nimeruumi. Nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks.

Nimeruumi loomiseks tehke järgmist.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Veenduge, et olete installinud [Azure'i SDK Java][] enne selle valimi koostamise. Kui kasutate Eclipse, saate installida [Azure tööriistakomplekt Eclipse][] Azure'i SDK Java sisaldava. Seejärel saate lisada **Microsoft Azure'i teekide Java** projekti:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Lisage järgmine `import` laused Java faili ülaosas:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Järjekorra loomiseks

Toimingute jaoks teenuse siini järjekorrad saab teostada **ServiceBusContract** klassi kaudu. **ServiceBusContract** objekt on ehitatud vastav konfiguratsiooni, mis kapseloi SAS luba õigustega haldamiseks ja **ServiceBusContract** klassi on ainus Azure'i suhtlus.

Klassi **ServiceBusService** pakub võimalust luua, loetlemine ja kustutada järjekorrad. Järgmises näites on kuvatud, kuidas saab kasutada **ServiceBusService** objekti nimega "TestQueue" nimega "HowToSample" nimeruumi järjekorra loomiseks:

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

On sisse **QueueInfo** meetodid, mis võimaldavad korraldada järjekorra atribuutide (nt: seadmiseks aeg live (TTL) vaikeväärtuse rakendamise järjekorda saadetud sõnumitele). Järgmises näites kujutatakse nimega järjekorra loomiseks `TestQueue` maksimaalse suurusega 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Pange tähele, et saate **listQueues** meetodit **ServiceBusContract** objektide kontrollida, kui määratud nimega järjekorda juba algselt teenuse nimeruumi.

## <a name="send-messages-to-a-queue"></a>Sõnumite saatmiseks järjekord

Sõnumi saatmiseks teenuse siini järjekorda rakenduse hangib **ServiceBusContract** objekti. Järgmine kood näitab, kuidas saata sõnum on `TestQueue` järjekorda varem loonud soovitud `HowToSample` nimeruum:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Sõnumite saatmiseks ja saadud teenuse siini järjekorrad on [BrokeredMessage][] klassi eksemplarid. [BrokeredMessage][] objektid on standardatribuudid (nt [silt](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) sõnastiku, hoidke kohandatud rakenduse teatud atribuutide kasutatava ja keha suvalise rakenduse andmeid. Rakenduse saate seada sõnumi kehasse, läbides sarjadesse jaotatav objekti [BrokeredMessage][]ehitaja ja vastav serializer kasutatakse seejärel serialiseerida objekti. Teise võimalusena saate sisestada **java. IO. InputStream** objekti.

Järgmises näites näitab, kuidas viis testida sõnumite saatmiseks on `TestQueue` **MessageSender** me saadud eelmise koodilõigu:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Teenuse siini järjekorrad toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Järjekorda sõnumite arv pole piiratud, kuid ei ülempiir suuruse järjekorda sõnumite kohta. Selles järjekorras maht on määratletud loomise ajal, 5 GB ülempiir.

## <a name="receive-messages-from-a-queue"></a>Saadud sõnumite järjekord

Esmane viis järjekorda sõnumeid vastu on kasutada **ServiceBusContract** objekti. Saadud sõnumite saate töötada kaks erinevat režiimi: **ReceiveAndDelete** ja **PeekLock**.

Kui **ReceiveAndDelete** režiimi kasutades saate on ühe-shot toiming - st kui teenus siini saab sõnum loetuks taotluse järjekorda, see märgib sõnumid tarbitud ja tagastab selle rakenduse. **ReceiveAndDelete** režiimi (mis on vaikimisi režiim) on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks.
Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

**PeekLock** režiimis, kuvatakse muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi helistades vastuvõetud sõnumi **kustutamine** . Kui teenus siini näeb **kustutada** kõne, see nagu tarbitud sõnumi märkimine ja eemaldamine järjekorda.

Järgmises näites näitab, kuidas sõnumid saab vastu võtta ja töödeldud **PeekLock** režiimis (mitte vaikerežiim). Järgmises näites ei silmuses ja töötleb sõnumeid nende saabumisel üheks meie "TestQueue".

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada **unlockMessage** meetodit vastuvõetud sõnumi (asemel **deleteMessage** meetod). See põhjustab teenuse siini sees kuhjuda sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

Olemas on ka seotud lukustatud jooksul kuhjuda sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (nt juhul, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **deleteMessage** kutse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Töötlemine on vähemalt üks kord**, st iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada sõnumi, mis jääb üle saatmiskatsete konstandi **getMessageId** meetodi abil.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini järjekorda, leiate lisateavet [järjekorrad, teemade, ja tellimused][] .

Lisateavet leiate teemast [Java Arenduskeskus](/develop/java/).


  [Azure'i SDK Java]: http://azure.microsoft.com/develop/java/
  [Azure'i tööriistakomplekt Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

