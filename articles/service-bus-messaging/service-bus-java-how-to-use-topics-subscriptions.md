<properties
    pageTitle="Teenuse siini teemade kasutamine Java | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure teenuse siini teemade ja tellimused. Java rakenduste koodinäiteid kirjutada."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Kuidas kasutada teenuse siini teemade ja tellimused

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Sellest juhendist kirjeldatakse, kuidas kasutada teenuse siini teemade ja tellimused. Näidiste Java kirjutada ja kasutada [Azure SDK Java][]. Stsenaariumid, mis hõlmab kaasake **teemade ja tellimuste loomise**, **tellimuse filtrite loomise**, **teemale sõnumite saatmine**, **sõnumeid tellimusest**ja **teemade ja tellimuste kustutamine**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Mis on teenuse siini teemade ja tellimuste?

Teenuse siini teemade ja tellimuste tugi on *avaldada/Telli* sõnumside side mudel. Teemade ja tellimuste kasutamisel komponendid jaotatud rakendus pole otse omavahel suhelda; selle asemel nad vahetada sõnumeid teema, mis on vahendajaks kaudu.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Erinevalt teenuse siini järjekorrad, kus iga sõnumi töödeldakse ühe tarbija teemade ja tellimuste pakuvad teatis, kasutades avalda/tellimise mustri kujul "üks-mitmele". On võimalik registreerida mitu tellimust teemale. Sõnumi saatmisel teema see on seejärel kättesaadav iga tellimuse pidet/protsessi sõltumatult.

Teema tellimust sarnaneb virtuaalse järjekorda, mida saab teema saadetud sõnumite koopiad. Soovi korral saate registreerida filtri reeglid teema kohta tellimuse alusel, mis võimaldab filtri/piirata sõnumid, mida soovite teema on saanud, millised tellimused teema.

Teenuse siini teemade ja tellimuste võimaldavad mastaapimiseks töödelda väga suure hulga sõnumite üle väga suure hulga kasutajate ja rakendused.

## <a name="create-a-service-namespace"></a>Teenuse nimeruumi loomine

Azure teenuse siini teemade ja tellimuste kasutamise alustamiseks tuleb esmalt luua teenuse nimeruumi. Nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks.

Nimeruumi loomiseks tehke järgmist.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Veenduge, et olete installinud [Azure'i SDK Java][] enne selle valimi koostamise. Kui kasutate Eclipse, saate installida [Azure tööriistakomplekt Eclipse][] Azure'i SDK Java sisaldava. Seejärel saate lisada **Microsoft Azure'i teekide Java** projekti:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Java faili ülaosas impordi järgmistest lisamiseks tehke järgmist.

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Azure'i teekide Java lisada oma Koosta tee ja lisada selle oma projekti juurutamise komplekteerimine.

## <a name="create-a-topic"></a>Teema loomine

Toimingute jaoks teenuse siini teemade saab teostada **ServiceBusContract** klassi kaudu. **ServiceBusContract** objekt on ehitatud vastav konfiguratsiooni, mis kapseloi SAS luba õigustega haldamiseks ja **ServiceBusContract** klassi on ainus Azure'i suhtlus.

**ServiceBusService** klassi pakub võimalust luua, loetlemine ja teemade kustutamine. Järgmises näites on kujutatud, kuidas **ServiceBusService** objekti saab luua nimega teema `TestTopic`, nimetatakse nimeruumi koos `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

On sisse **TopicInfo** meetodid, mis võimaldavad atribuutide määramine teema (nt: seada aja live (TTL) vaikeväärtus peaks rakenduma teema saadetud sõnumid). Järgmises näites kujutatakse luua nimega teema `TestTopic` maksimaalse suurusega 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Pange tähele, et saate **listTopics** meetodit **ServiceBusContract** objektide kontrollida, kui määratud nimega teema on juba olemas teenuse nimeruumi sees.

## <a name="create-subscriptions"></a>Tellimuste loomine

Teemade tellimused on loonud ka **ServiceBusService** klassi. Tellimused on nimega ja võib-olla valikuline filter, mis piirab kogumi, sõnumid, mis on möödas see tellimus virtuaalse järjekorda.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luua tellimuse vaikefilter (MatchAll)

**MatchAll** filter on vaikefilter, mida kasutatakse kui filtrit pole määratud, kui luuakse uus tellimus. **MatchAll** filtri kasutamisel paigutatakse kõik sõnumid, mis on avaldatud teema see tellimus virtuaalse järjekorda. Järgmises näites nimega "AllMessages" tellimuse loob ja kasutab **MatchAll** vaikefilter.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Tellimuste filtritega loomine

Saate luua ka filtrid, mis võimaldavad teil ulatus, mille jooksul kindla teemaga seotud tellimuse ilmub teema saadetud sõnumid.

Kõige paindlik tellimuste filter on selle [SqlFilter][], mis rakendab SQL92 alamhulga. SQL-i filtrid toimivad atribuutide sõnumid, mis on avaldatud teema. SQL-i filtriga kasutatavate avaldiste kohta lisateabe saamiseks vaadake [SqlFilter.SqlExpression][] süntaks.

Järgmises näites luuakse nimega tellimuse `HighMessages` [SqlFilter][] objektiga, mis valib ainult sõnumid, mis on suurem kui 3 kohandatud **MessageNumber** atribuut:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Samuti järgmises näites luuakse nimega tellimuse `LowMessages` [SqlFilter][] objektiga, mis valib ainult sõnumid, mis on **MessageNumber** atribuudi väiksem või võrdne 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Kui sõnum on nüüd saadetud `TestTopic`, see toimetatakse alati tellinud vastuvõtjad on `AllMessages` tellimus ja valikuliselt kohale toimetatud vastuvõtjad tellinud soovitud `HighMessages` ja `LowMessages` tellimused (sõltuvalt sõnumi sisu).

## <a name="send-messages-to-a-topic"></a>Sõnumite saatmiseks teema

Sõnumi saatmiseks teenuse siini teema rakenduse hangib **ServiceBusContract** objekti. Järgmine kood näitab, kuidas saata sõnum on `TestTopic` varem loodud teema on `HowToSample` nimeruum:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Teenuse siini teemade saadetud sõnumid on juhtumeid [BrokeredMessage][] klassi. [BrokeredMessage][] *objektid on standardne meetodite kogum (näiteks * *setLabel* * ja * *TimeToLive**), kasutatakse kohandatud rakenduse kohased atribuudid sõnastiku ja keha suvalise rakenduse andmeid. Rakenduse saate seada sõnumi kehasse, läbides sarjadesse jaotatav objekti ehitaja [BrokeredMessage][]ja asjakohased * *DataContractSerializer* * kasutatakse seejärel serialiseerida objekti. Teine võimalus on * *java.io.InputStream** on saadaval.

Järgmises näites näitab, kuidas viis testida sõnumite saatmiseks on `TestTopic` **MessageSender** me saadud ülaltoodud koodilõigu.
Pange tähele, kuidas iga sõnumi **MessageNumber** atribuudi väärtus muutub kursis iteratsiooni (see määratleb, millised tellimused jõua adressaadini):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Teenuse siini teemade toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Teema sõnumite arv pole piiratud, kuid kokku suurus sõnumid, mille teema on ülempiir. Selles teemas maht on määratletud loomise ajal, 5 GB ülempiir.

## <a name="how-to-receive-messages-from-a-subscription"></a>Kuidas sõnumeid, mis tellimusest

Tellimuse sõnumeid vastu, tuleb kasutada **ServiceBusContract** . Saadud sõnumite saate töötada kaks erinevat režiimi: **ReceiveAndDelete** ja **PeekLock**.

Kui **ReceiveAndDelete** režiimi kasutades saate on ühe-shot toiming - st kui teenus siini saab sõnumi loetuks, see märgib sõnumid tarbitud ja tagastab selle rakenduse. **ReceiveAndDelete** režiim on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

**PeekLock** režiimis, kuvatakse muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi helistades vastuvõetud sõnumi **kustutamine** . Kui teenus siini näeb **kustutada** kõne, see nagu tarbitud sõnumi märkimine ja eemaldamine teema.

Järgmises näites näitab, kuidas sõnumid saab vastu võtta ja töödeldud **PeekLock** režiimis (mitte vaikerežiim). Järgmises näites teostab esitatavaks ja töötleb sõnumite "HighMessages" tellimus ja väljub siis, kui seal on enam sõnumeid (teise võimalusena selle võib olla määratud ootama uute sõnumite jaoks).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada **unlockMessage** meetodit vastuvõetud sõnumi (asemel **deleteMessage** meetod). See põhjustab teenuse siini avada sõnumi teema sees ja saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus jaoks kättesaadavaks teha.

On ka seotud sõnumi teema sees lukustatud ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **deleteMessage** kutse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Töötlemine on vähemalt üks kord**, st iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada sõnumi, mis jääb üle saatmiskatsete konstandi **getMessageId** meetodi abil.

## <a name="delete-topics-and-subscriptions"></a>Teemade ja tellimuste kustutamine

Teemade ja tellimuste kustutamiseks peamine viis on kasutada **ServiceBusContract** objekti. Teema kustutamisel kustutatakse ka kõik tellimused, mis on registreeritud teema. Samuti saate tellimuste sõltumatult kustutatud.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini järjekorda, leiate lisateavet [teenuse siini järjekorrad, teemade, ja tellimused][] .

  [Azure'i SDK Java]: http://azure.microsoft.com/develop/java/
  [Azure'i tööriistakomplekt Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Teenuse siini järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
