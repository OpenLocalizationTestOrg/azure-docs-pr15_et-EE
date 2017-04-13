<properties 
    pageTitle="Teenuse siini järjekorrad kasutamine php | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure teenuse siini järjekorrad. Koodinäiteid php kirjutada." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Kuidas kasutada teenuse siini järjekorrad

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Sellest juhendist näitab, kuidas kasutada teenuse siini järjekorrad. Näidised on kirjutatud PHP ja kasutada [Azure SDK php](../php-download-sdk.md). Stsenaariumid, mis hõlmab kaasake **loomise järjekorrad**, **sõnumite saatmine ja vastuvõtt**ja **järjekorrad kustutamine**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>PHP rakenduse loomine

Ainult PHP rakendus, mis kasutab teenust Azure'i bloobimälu loomiseks on [Azure SDK php](../php-download-sdk.md) kaudu oma koodi tunde viitamine. Mis tahes arengu tööriistade abil saate luua rakenduse või Notepadi.

> [AZURE.NOTE] Installi PHP peab olema installitud ja lubatud [OpenSSL laiendamine](http://php.net/openssl) .

Sellest juhendist kasutate teenuse funktsioonid, mis saab helistada PHP rakendusest kohalikult või koodi, mis töötab Azure web roll, töötaja rolli või veebisaiti.

## <a name="get-the-azure-client-libraries"></a>Azure'i kliendi teekide hankimine

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Teenuse siini kuhjuda API-de kasutamiseks tehke järgmist.

1. Viide autoloader faili, kasutades [require_once] [ require_once] lause.
2. Viide mis tahes tunnid, võite kasutada.

Järgmises näites on kujutatud, kuidas lisada autoloader fail ja **ServicesBuilder** klassi viidata.

> [AZURE.NOTE] Selles näites (ja muud selle artikli näited) eeldatakse, et olete installinud PHP kliendi teegid Azure helilooja kaudu. Kui installisite teekide käsitsi või PIRNIPUUDE paketina, viidatava **WindowsAzure.php** autoloader fail.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Allpool toodud näidetes on `require_once` lause kuvatakse alati, kuid ainult vaja, nt käivitada klassid on viidatud.

## <a name="set-up-a-service-bus-connection"></a>Teenuse siini-ühenduse häälestamine

Väärtustada teenuse siini klient, peab olema esmalt sobiva ühendusstringi järgmises vormingus:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Kui **lõpp-punkti** on tavaliselt vormingu `[yourNamespace].servicebus.windows.net`.

Mis tahes Azure'i teenus kliendi loomiseks kasutage **ServicesBuilder** ainekursuse. Sa saad:

* Liigu otse selle ühendusstring.
* Kontrollige mitme välised allikad ühendusstringi **CloudConfigurationManager (CCM)** abil:
    * Vaikimisi on tegemist ühe välise andmeallikaga - keskkonna muutujad tugi
    * Saate lisada uue allikad pikendades **ConnectionStringSource** klassi

Siin kirjeldatud näited kantakse otse ühendusstring.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Kuidas: järjekorra loomiseks

Saate toiminguid teha halduse teenuse siini järjekorrad **ServiceBusRestProxy** ainekursuse kaudu. **ServiceBusRestProxy** objekt on ehitatud **ServicesBuilder::createServiceBusService** factory meetodil sobiv ühenduse string, mis kapseloi Turbeloa õiguste haldamiseks.

Järgmises näites kujutatakse väärtustada on **ServiceBusRestProxy** ja kõne- **ServiceBusRestProxy -> createQueue** nimega järjekorra loomiseks `myqueue` sees on `MySBNamespace` teenuse nimeruumi:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
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

> [AZURE.NOTE] Saate kasutada funktsiooni `listQueues` meetodi `ServiceBusRestProxy` kontrollida, kui määratud nimega järjekorda juba algselt nimeruumi objektid.

## <a name="how-to-send-messages-to-a-queue"></a>Kuidas: sõnumeid saata järjekorda

Sõnumi saatmiseks teenuse siini järjekord teie rakendus nõuab **ServiceBusRestProxy -> sendQueueMessage** meetod. Järgmine kood näitab, kuidas saada sõnum on `myqueue` varem loodud järjekorda soovitud `MySBNamespace` teenuse nimeruumi.

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
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Sõnumid saadetakse (ja vastuvõetud:) teenuse siini järjekorrad on **BrokeredMessage** klassi eksemplarid. **BrokeredMessage** objektid on standardne viise (nt **getLabel**, **getTimeToLive**, **setLabel**ja **setTimeToLive**) ja atribuudid, mida kasutatakse kohandatud rakenduse kohased atribuudid ja keha suvalise rakenduse andmete mahutamiseks.

Teenuse siini järjekorrad toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Järjekorda sõnumite arv pole piiratud, kuid ei ülempiir suuruse järjekorda sõnumite kohta. Selles järjekorras suurus on 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Kuidas sõnumeid vastu järjekord

Parim viis järjekorda sõnumeid vastu on **ServiceBusRestProxy -> receiveQueueMessage** meetodi kasutamise kohta. Kaks erinevat režiimi saate sõnumeid vastu võtta: **ReceiveAndDelete** (vaikimisi) ja **PeekLock**.

Kui **ReceiveAndDelete** režiimi kasutades saate on ühe-shot toiming - st kui teenus siini saab sõnum loetuks taotluse järjekorda, see märgib sõnumid tarbitud ja tagastab selle rakenduse. **ReceiveAndDelete** režiim on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

**PeekLock** režiimis, sõnumi muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla tarbitud, lukustab muude tarbijate takistamine seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi saates vastuvõetud sõnumi **ServiceBusRestProxy -> deleteMessage**. Kui teenus siini näeb **deleteMessage** kõne, see nagu tarbitud sõnumi märkimine ja eemaldamine järjekorda.

Järgmises näites on kujutatud, kuidas sõnumit vastu võtta ja töödeldud **PeekLock** režiimis (mitte vaikerežiim).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Kuidas: rakendus jookseb ja loetavad sõnumid

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada **unlockMessage** meetodit vastuvõetud sõnumi (asemel **deleteMessage** meetod). See põhjustab teenuse siini sees kuhjuda sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

Olemas on ka seotud lukustatud jooksul kuhjuda sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **deleteMessage** kutse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Vähemalt ühe korra töötlemine**; mis on iga sõnumi töötlemise vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumi. Kui seda stsenaariumi ei luba dubleeritud töötlemine, seejärel lisada täiendavad loogika taotlusi käsitleda sõnumite kohaletoimetamise on soovitatav. See on sageli saavutada sõnumi, mis jääb samaks üle saatmiskatsete **getMessageId** meetodi abil.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini järjekorda, leiate lisateavet [järjekorrad, teemade, ja tellimused][] .

Lisateavet leiate teemast ka [PHP Arenduskeskus](/develop/php/).

[Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
