<properties 
    pageTitle="Teenuse siini järjekorrad kasutamine Node.js | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure teenuse siini järjekorrad Node.js rakenduse kaudu." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Kuidas kasutada teenuse siini järjekorrad

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini järjekorrad Node.js. Näidised kirjutada JavaScript ja kasutage mooduli Node.js Azure. Stsenaariumid, mis hõlmab kaasake **loomise järjekorrad**, **sõnumite saatmine ja vastuvõtt**ja **järjekorrad kustutamine**. Järjekorrad kohta leiate lisateavet jaotisest [järgmised toimingud](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Node.js rakenduse loomine

Looge tühi Node.js rakendus. Juhised selle kohta, kuidas luua rakenduse Node.js, lugege teemat [loomine ja juurutamine Node.js rakenduse Azure'i veebisaidile][], või [Node.js pilveteenuses][] Windows PowerShelli kaudu.

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Azure'i teenus siini kasutamiseks alla laadida ja kasutada Node.js Azure. See pakett sisaldab kogum, teegid, mis teenuse siini ülejäänud teenuste suhelda.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Saada pakett sõlm paketi Manager (NPM) abil

1. **Windows PowerShelli jaoks Node.js** käsuviiba aken liikumiseks kasutage funktsiooni **c:\\sõlm\\sbqueues\\WebRole1** kaust, mille lõite rakenduse valimi.

2. Tippige käsuviiba aken, mis peaks väljund sarnaneb järgmisega **npm installida azure** :

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3. Käsitsi käivitada veendumaks, et käsk **ls** on **sõlm\_moodulid** loodud kausta. Selle kausta sisse leiate **Azure'i** paketi, mis sisaldab teenuse siini järjekorrad juurdepääsuks teekide.

### <a name="import-the-module"></a>Mooduli importimine

Kasutades Notepad või mõne muu tekstiredaktoris, lisage järgmine tekst **server.js** faili rakenduse ülaserva:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Azure'i teenus siini-ühenduse häälestamine

Azure'i mooduli loeb keskkonna muutujate AZURE'I\_SERVICEBUS\_NIMERUUM ja AZURE\_SERVICEBUS\_ACCESSI\_klahvi saamiseks teenuse siini ühenduse loomiseks vajalik teave. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe **createServiceBusService**helistamisel.

Näiteks säte keskkonna muutujate konfiguratsioonifailis on Azure pilveteenuses, vt [Storage Node.js pilveteenuses][].

Näiteks säte keskkonna muutujate [Azure klassikaline portaalis][] Azure veebisait, leiate teemast [Node.js veebirakendust salvestusruumi][].

## <a name="create-a-queue"></a>Järjekorra loomiseks

Objekti **ServiceBusService** võimaldab töötada teenuse siini järjekorrad. Järgmine kood tekitab **ServiceBusService** objekti. Lisage see **server.js** faili ülaosas pärast lauset Azure mooduli importida.

```
var serviceBusService = azure.createServiceBusService();
```

Helistades **createQueueIfNotExists** **ServiceBusService** objekti, tagastatakse määratud järjekorda (kui see on olemas), või on loodud uue järjekorra nimi. Järgmine kood kasutab **createQueueIfNotExists** loomiseks või ühenduse nimega kuhjuda `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** toetab samuti veel suvandeid, mis lubavad teil Alista vaikesätted järjekorda näiteks sõnumi aega otse- või järjekorra suurus. Järgmises näites määrab maksimaalne järjekorra suurus 5 GB ja kellaaeg live (TTL) väärtust 1 minuti jooksul:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filtrid

Valikuline filtreerimise toiminguid saab rakendada toimingute logiteavet **ServiceBusService**. Filtreerimine toimingud võivad hõlmata logimine, automaatselt uuesti proovimine jne. Filtrid on objekte, mis rakendada allkirjaga meetodit.

```
function handle (requestOptions, next)
```

Pärast teeme selle eelse töötlemiseks taotluse suvandid, peab kõne meetodit `next`, läbides järgmised allkirjaga pakkumist:

```
function (returnObject, finalCallback, next)
```

See tagasihelistamise ja pärast töötlemist **returnObject** (vastuse taotluse server), kuvatakse tagasihelistamise peab kas autonoomsest `next` kui see on olemas jätkata töötlemine filtrid või lihtsalt autonoomsest `finalCallback`, mis lõpeb teenuse kutsumise.

Kahe filtrid, mis rakendada uuesti loogika on kaasas Azure'i SDK Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**. Järgmine loob **ServiceBusService** objekti, mis kasutab **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Sõnumite saatmiseks järjekord

Sõnumi saatmiseks teenuse siini järjekord teie rakendus nõuab **sendQueueMessage** meetod **ServiceBusService** objekti. Sõnumid saadetakse (ja vastuvõetud:) teenuse siini järjekorrad on **BrokeredMessage** objektid ja on sõnastiku, hoidke kohandatud rakenduse kohased atribuudid kasutatava ja keha suvalise rakenduse andmete standardatribuudid (nt **silt** ja **TimeToLive**). Rakenduse saate seada sõnumi kehasse, läbides stringi nagu sõnum. Mõni nõutav standardatribuudid täidetakse vaikeväärtustega.

Järgmises näites näitab, kuidas test sõnumi saatmine nimega järjekorda `myqueue` **sendQueueMessage**abil:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Teenuse siini järjekorrad toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Järjekorda sõnumite arv pole piiratud, kuid ei ülempiir suuruse järjekorda sõnumite kohta. Selles järjekorras maht on määratletud loomise ajal, 5 GB ülempiir. Kvootide kohta leiate lisateavet teemast [teenuse siini piirmäärasid][].

## <a name="receive-messages-from-a-queue"></a>Saadud sõnumite järjekord

Saadetud meilisõnumid **receiveQueueMessage** meetodil **ServiceBusService** objekti järjekorda. Vaikimisi kustutatakse kuhjuda nagu neid lugeda; Siiski saate lugeda (peek) ja lukustamine sõnumi ilma kustutamine järjekorda seadmisega valikuline parameeter **isPeekLock** väärtuseks **true**.

Vaikekäitumist lugemise ja kustutage sõnum osana vastuvõtmise toiming on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

Kui **isPeekLock** parameeter on seatud **tõene**, on vastuvõtu muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi **deleteMessage** meetodi ning esitada parameetrina soovitud sõnum. **DeleteMessage** meetod on nagu tarbitud sõnumi märkimine ja eemaldamine järjekorda.

Järgmises näites näitab, kuidas saada ja töödelda sõnumite **receiveQueueMessage**abil. Näide esmalt saab ja kustutab sõnumi, ja seejärel saab sõnumi abil **isPeekLock** väärtuseks **true,**ja seejärel kustutab sõnumi **deleteMessage**abil.

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada **unlockMessage** meetodit **ServiceBusService** objekti. See põhjustab teenuse siini sees kuhjuda sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

Olemas on ka seotud lukustatud jooksul kuhjuda sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (nt juhul, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **deleteMessage** meetodit nimetatakse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Töötlemine on vähemalt üks kord**, st iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada, kasutades **MessageId** atribuut, mis samaks saatmiskatsete üle.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet järjekorrad, leiate järgmistest teemadest.

-   [Järjekorrad, teemasid ja tellimused][]
-   [Azure'i SDK sõlme][] hoidla github
-   [Node.js Arenduskeskus](/develop/nodejs/)

  [Azure'i SDK sõlm]: https://github.com/Azure/azure-sdk-for-node
  [Azure'i klassikaline portaal]: http://manage.windowsazure.com
  
  [Node.js pilveteenuses]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
  [Luua ja juurutada Node.js rakenduse Azure'i veebisaidile]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js pilveteenuses salvestusruum]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js veebirakenduse salvestusruum]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Teenuse siini kvoote]: service-bus-quotas.md
 
