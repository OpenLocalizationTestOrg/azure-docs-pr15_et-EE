<properties 
    pageTitle="Teenuse siini teemade kasutamine php | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada teenuse siini teemade PHP Azure." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Kuidas kasutada teenuse siini teemade ja tellimused

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini teemade ja tellimuste. Näidised on kirjutatud PHP ja kasutada [Azure SDK php](../php-download-sdk.md). Stsenaariumid, mis hõlmab kaasake **teemade ja tellimuste loomise**, **tellimuse filtrite loomise**, **teemale sõnumite saatmine**, **sõnumeid tellimusest**ja **teemade ja tellimuste kustutamine**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>PHP rakenduse loomine

Ainult nõue loomise PHP rakendus, mis kasutab teenust Azure'i bloobimälu on viide tunde [Azure'i SDK php](../php-download-sdk.md) kaudu oma koodi. Mis tahes arengu tööriistade abil saate luua rakenduse või Notepadi.

> [AZURE.NOTE] Installi PHP peab olema installitud ja lubatud [OpenSSL laiendamine](http://php.net/openssl) .

Selles artiklis kirjeldatakse, kuidas kasutada teenuse funktsioonid, mida saab kutsuda PHP rakendusest kohalikult või koodi, mis töötab Azure web roll, töötaja rolli või veebisaiti.

## <a name="get-the-azure-client-libraries"></a>Azure'i kliendi teekide hankimine

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Teenuse siini API-de kasutamiseks tehke järgmist.

1. Viide autoloader faili, kasutades [require_once] [ require-once] lause.
2. Viide mis tahes tunnid, võite kasutada.

Järgmises näites on kujutatud, kuidas lisada autoloader fail ja **ServiceBusService** klassi viidata.

> [AZURE.NOTE] Selles näites (ja muud selle artikli näited) eeldatakse, et olete installinud PHP kliendi teegid Azure helilooja kaudu. Kui installisite teekide käsitsi või PIRNIPUUDE paketina, viidatava **WindowsAzure.php** autoloader fail.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Järgmistes näidetes on `require_once` lause kuvatakse alati, kuid ainult vaja, nt käivitada klassid on viidatud.

## <a name="set-up-a-service-bus-connection"></a>Teenuse siini-ühenduse häälestamine

Kliendi teenuse siini väärtustada peab olema esmalt sobiva ühendusstringi järgmises vormingus:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Kus `Endpoint` on tavaliselt vormingu `https://[yourNamespace].servicebus.windows.net`.

Mis tahes Azure'i teenus kliendi loomiseks kasutage **ServicesBuilder** ainekursuse. Sa saad:

* Liigu otse selle ühendusstring.
* Kontrollige mitme välised allikad ühendusstringi **CloudConfigurationManager (CCM)** abil:
    * Vaikimisi on see ühe välise andmeallikaga - keskkonna muutujate tugi.
    * Saate lisada uue allikad pikendades **ConnectionStringSource** klassi.

Siin kirjeldatud näited edastatakse otse ühendusstring.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Teema loomine

Toimingute jaoks teenuse siini teemade **ServiceBusRestProxy** klassi kaudu, saate seda teha. **ServiceBusRestProxy** objekt on ehitatud **ServicesBuilder::createServiceBusService** factory meetodil sobiv ühenduse string, mis kapseloi Turbeloa õiguste haldamiseks.

Järgmises näites kujutatakse väärtustada on **ServiceBusRestProxy** ja kõne- **ServiceBusRestProxy -> createTopic** luua nimega teema `mytopic` sees on `MySBNamespace` nimeruum:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Saate kasutada funktsiooni `listTopics` meetodi `ServiceBusRestProxy` objektide kontrollida, kui määratud nimega teema on juba olemas teenuse nimeruumi sees.

## <a name="create-a-subscription"></a>Tellimuse loomine

Teema: tellimuste on loonud ka **ServiceBusRestProxy -> createSubscription** meetodit. Tellimused on nimega ja võib-olla valikuline filter, mis piirab kogumi, sõnumid, mis on möödas see tellimus virtuaalse järjekorda.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luua tellimuse vaikefilter (MatchAll)

**MatchAll** filter on vaikefilter, mida kasutatakse kui filtrit pole määratud, kui luuakse uus tellimus. **MatchAll** filtri kasutamisel paigutatakse kõik sõnumid, mis on avaldatud teema see tellimus virtuaalse järjekorda. Järgmises näites nimega "mysubscription" tellimuse loob ja kasutab **MatchAll** vaikefilter.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Tellimuste filtritega loomine

Saate häälestada ka filtrid, mille abil saate määrata, millised sõnumid saadetakse teema peaks kuvatakse mõne kindla teemaga seotud tellimuse pärast. Kõige paindlik tellimuste filter on selle **SqlFilter**, mis rakendab SQL92 alamhulga. SQL-i filtrid toimivad atribuutide sõnumid, mis on avaldatud teema. SqlFilters kohta leiate lisateavet teemast [SqlFilter.SqlExpression atribuudi][sqlfilter].

> [AZURE.NOTE] Iga tellimuse reegel töötleb sissetulevate sõnumite sõltumatult, nende tulemi sõnumeid lisamine tellimus. Lisaks iga uus tellimus on vaikimisi **reegel** objekti filtriga, mis lisab kõik sõnumid teema tellimusega. Teade kuvatakse ainult vastenduvate filtrisse, peate eemaldama vaikimisi reegel. Saate eemaldada vaikimisi reegel, kasutades funktsiooni `ServiceBusRestProxy->deleteRule` meetod.

Järgmises näites luuakse nimega **HighMessages** koos **SqlFilter** , mis valib ainult sõnumid, mis on suurem kui 3 (kohta leiate teavet [teema sõnumeid saata](#send-messages-to-a-topic) sõnumeid kohandatud atribuutide lisamine) kohandatud **MessageNumber** atribuut tellimus:

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Teate, et see kood tuleb kasutada mõne täiendavad nimeruum: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Järgmises näites luuakse samuti tellimuse nimega **LowMessages** koos **SqlFilter** , mis valib ainult sõnumid, mis on **MessageNumber** atribuudi väiksem või võrdne 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Nüüd, kui sõnum saadetakse soovitud `mytopic` teema, see on alati esitatud vastuvõtjad tellinud on `mysubscription` tellimus, ja valikuliselt kohale toimetatud vastuvõtjad tellinud soovitud `HighMessages` ja `LowMessages` tellimused (sõltuvalt sõnumi sisu).

## <a name="send-messages-to-a-topic"></a>Sõnumite saatmiseks teema

Teenuse siini teema sõnumi saatmiseks teie rakendus nõuab **ServiceBusRestProxy -> sendTopicMessage** meetod. Järgmine kood näitab, kuidas saada sõnum on `mytopic` varem loodud teema on `MySBNamespace` teenuse nimeruumi.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Teenuse siini teemade saadetud sõnumid on juhtumeid **BrokeredMessage** klassi. **BrokeredMessage** objektid on standardatribuudid ja meetodite (nt **getLabel**, **getTimeToLive**, **setLabel**ja **setTimeToLive**), samuti atribuudid, mida saate kasutada hoidke kohandatud rakenduse kohased atribuudid. Järgmises näites kujutatakse 5 testi sõnumite saatmiseks on `mytopic` varem loodud teema. Lisada kohandatud atribuuti kasutatakse meetodit **Sea_atribuut** (`MessageNumber`) iga sõnumi. Pange tähele, et selle `MessageNumber` atribuudi väärtus muutub igale sõnumile (saate selle väärtuse kindlaks teha, millised tellimused vastu võtta, nagu on näidatud jaotise [Loo tellimuse](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Teenuse siini teemade toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Teema sõnumite arv pole piiratud, kuid kokku suurus sõnumid, mille teema on ülempiir. See teema mahu ülempiir on 5 GB. Kvootide kohta leiate lisateavet teemast [teenuse siini piirmäärasid][].

## <a name="receive-messages-from-a-subscription"></a>Sõnumeid, mis tellimusest

Parim viis sõnumeid saada tellimus on **ServiceBusRestProxy -> receiveSubscriptionMessage** meetodi kasutamise kohta. Saadud sõnumite saate töötada kaks erinevat režiimi: **ReceiveAndDelete** (vaikimisi) ja **PeekLock**.

Kui **ReceiveAndDelete** režiimi kasutades saate on ühe-shot toiming; See tähendab, kui teenus siini saab sõnumi loetuks taotluse tellimus, see märgib sõnumi nagu tarbitud ja tagastab selle rakenduse. **ReceiveAndDelete** režiim on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

**PeekLock** režiimis, sõnumi muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi saates vastuvõetud sõnumi **ServiceBusRestProxy -> deleteMessage**. Kui teenus siini näeb **deleteMessage** kõne, see nagu tarbitud sõnumi märkimine ja eemaldamine järjekorda.

Järgmises näites kujutatakse saada ja töödelda sõnumi **PeekLock** režiimis (mitte vaikerežiim). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kuidas: rakendus jookseb ja loetavad sõnumid

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada **unlockMessage** meetodit vastuvõetud sõnumi (asemel **deleteMessage** meetod). See põhjustab teenuse siini sees kuhjuda sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

Olemas on ka seotud lukustatud jooksul kuhjuda sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **deleteMessage** kutse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Vähemalt ühe korra töötlemine**; mis on iga sõnumi töötlemise vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumi. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele tuleks lisada täiendavad loogika rakendustele sõnumite kohaletoimetamise käsitlema. See on sageli saavutada sõnumi, mis jääb samaks üle saatmiskatsete **getMessageId** meetodi abil.

## <a name="delete-topics-and-subscriptions"></a>Teemade ja tellimuste kustutamine

Teema või tellimuse kustutamiseks kasutage funktsiooni- **ServiceBusRestProxy -> deleteTopic** või **ServiceBusRestProxy -> deleteSubscripton** meetodite vastavalt. Pange tähele, et teema kustutamine kustutatakse ka kõik tellimused, mis on registreeritud teema.

Järgmises näites on kujutatud, kuidas kustutada teema nimega `mytopic` ja selle registreeritud tellimused.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

**DeleteSubscription** meetodi abil saate tellimuse sõltumatult kustutada:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini järjekorda, leiate lisateavet [järjekorrad, teemade, ja tellimused][] .

[Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Teenuse siini kvoote]: service-bus-quotas.md
