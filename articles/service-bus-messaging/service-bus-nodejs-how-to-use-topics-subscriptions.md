<properties 
    pageTitle="Kuidas kasutada teenuse siini teemade Node.js | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada teenuse siini teemade ja tellimuste Azure Node.js rakenduse kaudu." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Kuidas kasutada teenuse siini teemade ja tellimused

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Sellest juhendist kirjeldatakse, kuidas kasutada teenuse siini teemade ja tellimuste Node.js rakendustest. Stsenaariumid, mis hõlmab kaasata **teemade ja tellimuste loomise**, **luues tellimuse filtrid**, teema, **sõnumeid tellimusest**, ja **teemad ja tellimuste kustutamine** **sõnumite saatmiseks** . Teemade ja tellimuste kohta leiate lisateavet jaotisest [järgmised toimingud](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Node.js rakenduse loomine

Looge tühi Node.js rakendus. Node.js rakenduste loomise juhised leiate teemast [loomine ja juurutamine Node.js rakendus on Azure veebisaidi], [Node.js pilveteenuses][] WebMatrix või veebisaidi Windows PowerShelli abil.

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Teenuse siini kasutamiseks laadige Node.js Azure. See pakett sisaldab kogum, teegid, mis teenuse siini ülejäänud teenuste suhelda.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Saada pakett sõlm paketi Manager (NPM) abil

1.  Kasutage käsurea liides **PowerShell** (Windows), nt **terminalis** (Mac) või **Bash** (Unix), liikuge kausta, kuhu olete loonud valimi rakenduse.

2.  Tippige käsuviiba aken, mis peaks järgmine väljund **npm installida azure** :

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

3.  Käsitsi käivitada veendumaks, et käsk **ls** on **sõlm\_moodulid** loodud kausta. Selle kausta sisse leiate **Azure'i** paketi, mis sisaldab teenuse siini teemade juurdepääsuks teekide.

### <a name="import-the-module"></a>Mooduli importimine

Kasutades Notepad või mõne muu tekstiredaktoris, lisage järgmine tekst **server.js** faili rakenduse ülaserva:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Teenuse siini-ühenduse häälestamine

Azure'i mooduli loeb keskkonna muutujate AZURE'I\_SERVICEBUS\_NIMERUUM ja AZURE\_SERVICEBUS\_ACCESSI\_võtme jaoks teenuse siini ühenduse loomiseks vajalik teave. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe **createServiceBusService**helistamisel.

Näiteks säte keskkonna muutujate konfiguratsioonifailis on Azure pilveteenuses, vt [Storage Node.js pilveteenuses][].

Näiteks säte keskkonna muutujate [Azure klassikaline portaalis][] Azure veebisait, leiate teemast [Node.js veebirakendust salvestusruumi][].

## <a name="create-a-topic"></a>Teema loomine

Objekti **ServiceBusService** võimaldab töötada teemade. Järgmine kood tekitab **ServiceBusService** objekti. Lisage see **server.js** faili ülaosas pärast lauset azure mooduli importida.

```
var serviceBusService = azure.createServiceBusService();
```

Helistades **createTopicIfNotExists** **ServiceBusService** objekti, tagastatakse määratud teema (olemasolu korral) või luuakse uus teema nimi. Järgmine kood kasutab **createTopicIfNotExists** loomiseks või ühenduse nimega "MyTopic" teema:

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** toetab samuti lisasuvandid, mis võimaldavad teil Alista vaikesätted teema näiteks sõnumi aega live või teema maksimaalne maht. Järgmises näites määrab mahu ülempiir teema 5GB aega, et live 1 minuti jooksul:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtrid

Valikuline filtreerimise toiminguid saab rakendada toimingute logiteavet **ServiceBusService**. Filtreerimine toimingud võivad hõlmata logimine, automaatselt uuesti proovimine jne. Filtrid on objekte, mis rakendada allkirjaga meetodit.

```
function handle (requestOptions, next)
```

Pärast eeltöötlus taotluse suvandid, nõuab `next` läbides järgmised allkirjaga pakkumist:

```
function (returnObject, finalCallback, next)
```

Selle tagasihelistamise ja pärast töötlemist **returnObject** (vastuse taotluse server) tagasihelistamise vajadustele kas autonoomsest edasi, kui see on olemas, jätkake töötlemine filtrid või lihtsalt autonoomsest **finalCallback** muidu sattuda teenuse kutsumise.

Kahe filtrid, mis rakendada uuesti loogika on kaasas Azure'i SDK Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**. Järgmine loob **ServiceBusService** objekti, mis kasutab **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Tellimuste loomine

Teema: tellimuste on loonud ka **ServiceBusService** objekti. Tellimused on nimega ja võib-olla valikuline filter, mis piirab kogumi sõnumid toimetada see tellimus virtuaalse järjekorda.

> [AZURE.NOTE] Tellimused on püsiv ja jätkub kuni ükskõik kumma nad või teema on seotud, kustutatakse. Kui rakenduse sisaldab loogika tellimuse loomiseks, tuleks esmalt kontrollida, kui tellimus juba olemas, **getSubscription** meetodi abil.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luua tellimuse vaikefilter (MatchAll)

**MatchAll** filter on vaikefilter, mida kasutatakse kui filtrit pole määratud, kui luuakse uus tellimus. **MatchAll** filtri kasutamisel paigutatakse kõik sõnumid, mis on avaldatud teema see tellimus virtuaalse järjekorda. Järgmises näites nimega 'AllMessages' tellimuse loob ja kasutab **MatchAll** vaikefilter.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Tellimuste filtritega loomine

Saate luua ka filtrid, mis võimaldavad teil ulatuse, mis on saadetud sõnumite teema ilmuks kindla teemaga seotud tellimuse sees.

Kõige paindlik tellimuste filter on selle **SqlFilter**, mis rakendab SQL92 alamhulga. SQL-i filtrid toimivad atribuutide sõnumid, mis on avaldatud teema. Täpsemat teavet avaldised, mida saab kasutada SQL-i filtriga läbivaatamine [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] süntaks.

Filtrid saate lisada tellimuse **ServiceBusService** objekti **createRule** meetodi abil. See meetod võimaldab uute filtrite lisamine olemasolevale tellimusele.

> [AZURE.NOTE] Kuna vaikefilter rakendatakse kõigi uute tellimused automaatselt, peate eemaldama vaikefilter või **MatchAll** alistab võite määrata filtreid. Saate eemaldada vaikimisi reegel **ServiceBusService** objekti **deleteRule** meetodi abil.

Järgmises näites luuakse nimega tellimuse `HighMessages` koos **SqlFilter** , mis valib ainult sõnumid, mis on suurem kui 3 kohandatud **messagenumber** atribuut:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Samuti järgmises näites luuakse nimega tellimuse `LowMessages` koos **SqlFilter** , mis valib ainult sõnumid, mis on **messagenumber** atribuudi väiksem või võrdne 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Kui sõnum on nüüd saadetud `MyTopic`, see toimetatakse alati tellinud vastuvõtjad on `AllMessages` teema tellimus, ja valikuliselt kohale toimetatud vastuvõtjad tellinud selle `HighMessages` ja `LowMessages` teema tellimused (sõltuvalt sõnumi sisu).

## <a name="how-to-send-messages-to-a-topic"></a>Kuidas saata sõnumeid teema

Sõnumi saatmiseks teenuse siini teema rakenduse peate kasutama **sendTopicMessage** meetodit **ServiceBusService** objekti.
Teenuse siini teemade saadetud sõnumid on **BrokeredMessage** objektid.
**BrokeredMessage** objektid on standardatribuudid (nt **silt** ja **TimeToLive**), kasutatakse hoidke kohandatud rakenduse teatud atribuutide sõnastiku ja keha stringi andmed. Rakenduse saate seada sõnumi kehasse saates stringiväärtus **sendTopicMessage** ja kõik nõutud standard atribuudid on täidetud vaikimisi väärtused.

Järgmises näites näitab, kuidas viis testimiseks 'MyTopic' sõnumeid saata. Pange tähele, et iga sõnumi **messagenumber** atribuudi väärtus muutub kursis iteratsiooni (see määratleb, millised tellimused jõua adressaadini):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Teenuse siini teemade toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Teema sõnumite arv pole piiratud, kuid kokku suurus sõnumid, mille teema on ülempiir. Selles teemas maht on määratletud loomise ajal, 5 GB ülempiir.

## <a name="receive-messages-from-a-subscription"></a>Sõnumeid, mis tellimusest

Tellimuse **receiveSubscriptionMessage** meetodil **ServiceBusService** objekti saadetud meilisõnumid. Vaikimisi kustutatakse tellimuse nagu neid lugeda; Siiski saate lugeda (peek) ja lukustamine sõnumi ilma kustutamine tellimusest, seades valikuline parameeter **isPeekLock** väärtuseks **true**.

Vaikekäitumist lugemise ja kustutage sõnum osana vastuvõtmise toiming on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

Kui **isPeekLock** parameeter on seatud **tõene**, on vastuvõtu muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus.
Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi **deleteMessage** meetodi ning esitada parameetrina soovitud sõnum. **DeleteMessage** meetod on nagu tarbitud sõnumi märkimine ja eemaldamine tellimusest.

Järgmises näites näitab, kuidas sõnumid saab vastu võtta ja **receiveSubscriptionMessage**kasutamist. Näide esmalt saab ja kustutab sõnumi 'LowMessages' tellimus ja seejärel saab sõnumi 'HighMessages' tellimuselt abil **isPeekLock** väärtuseks true. See kustutab sõnumi **deleteMessage**abil:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada **unlockMessage** meetodit **ServiceBusService** objekti. See põhjustab teenuse siini sees tellimuse sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

On ka seotud lukustatud jooksul tellimuse sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse avab sõnumi automaatselt ja teeb selle kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **deleteMessage** meetodit nimetatakse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Töötlemine on vähemalt üks kord**, st iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada, kasutades **MessageId** atribuut, mis samaks saatmiskatsete üle.

## <a name="delete-topics-and-subscriptions"></a>Teemade ja tellimuste kustutamine

Teemade ja tellimused on püsiv ja tuleb selgesõnaliselt [Azure klassikaline portaali][] kaudu või programmiliselt kustutada.
Järgmises näites näitab, kuidas kustutada teema nimega `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Teema kustutamisel kustutatakse ka kõik tellimused, mis on registreeritud teema. Samuti saate tellimuste sõltumatult kustutatud. Järgmises näites on kujutatud, kuidas kustutada tellimuse nimega `HighMessages` kaudu soovitud `MyTopic` teema:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete teenuse siini teemade põhitõdesid õppinud, järgige neid linke lisateabe.

-   Vt [järjekorrad, teemade, ja tellimused][].
-   API viide [SqlFilter][]jaoks.
-   Külastage [Azure'i SDK sõlme][] hoidla github.

  [Azure'i SDK sõlm]: https://github.com/Azure/azure-sdk-for-node
  [Azure'i klassikaline portaal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js pilveteenuses]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Luua ja juurutada Node.js rakendus on Azure veebisaidi]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js pilveteenuses salvestusruum]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js veebirakenduse salvestusruum]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
