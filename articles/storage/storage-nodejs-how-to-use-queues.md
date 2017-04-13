<properties
    pageTitle="Kasutamise järjekorra salvestusruumi Node.js kaudu | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada teenust Azure järjekorda loomine ja kustutamine järjekordade ja lisada, leida ja sõnumite kustutamine. Näidised Node.js kirjutatud."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Kuidas kasutada järjekorda salvestusruumi Node.js kaudu

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas teha levinud stsenaariumi Microsoft Azure'i kuhjuda teenuse kasutamisel. Näidiste kirjutada Node.js API-ga. Stsenaariumid, mis hõlmab hulka **lisamise**, **piilumist**, **otsimine**ja **kustutamine** järjekorda sõnumeid, samuti **loomine ja kustutamine järjekorrad**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js rakenduse loomine

Looge tühi Node.js rakendus. Node.js rakenduste loomise juhised leiate teemast [Node.js veebirakenduse teenuses Azure rakenduse loomine], [luua ja juurutada Node.js rakendus on Azure pilveteenusesse] Windows PowerShelli või [luua ja juurutada Node.js web Appi abil Web maatriksi Azure].

## <a name="configure-your-application-to-access-storage"></a>Rakenduse Access mäluruumi konfigureerimine

Azure'i salvestusruumi kasutamiseks peate Azure'i salvestusruumi SDK Node.js, mis sisaldab mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuste kogum.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Saada pakett sõlm paketi Manager (NPM) abil

1.  Kasutage käsurea liides **PowerShell** (Windows), nt **terminalis** (Mac) või **Bash** (Unix), liikuge kausta, kuhu olete loonud valimi rakenduse.

2.  Tippige käsuviiba aken **npm installida azure-salvestusruumi** . Käsu väljund sarnaneb järgmises näites.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Käsitsi käivitada veendumaks, et käsk **ls** on **sõlm\_moodulid** loodud kausta. Selle kausta sisse leiate **Azure'i-salvestusruumi** paketi, mis sisaldab teekide talletusmahu juurdepääsuks.

### <a name="import-the-package"></a>Paketi importimine

Kasutades Notepad või mõne muu tekstiredaktoris, lisage järgmine tekst üles **server.js** faili rakenduse kui kavatsete kasutada salvestusruumi:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Mõne Azure Storage ühenduse häälestamine

Azure'i mooduli ei loe keskkonna muutujate AZURE'I\_salvestusruumi\_konto ja AZURE\_salvestusruumi\_ACCESSI\_klahvi või AZURE'I\_salvestusruumi\_ühenduse\_STRINGI Azure storage kontoga ühenduse loomiseks vajalik teave. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe **createQueueService**helistamisel.

Näiteks säte keskkonna muutujate veebisait Azure'i [Azure portaali](https://portal.azure.com) , vt [Node.js web Appi abil Azure'i tabeli teenus].

## <a name="how-to-create-a-queue"></a>Tehke järgmist: Järjekorra loomiseks

Järgmine kood tekitab **QueueService** objekti, mis võimaldab töötada järjekorrad.

    var queueSvc = azure.createQueueService();

Kasutage **createQueueIfNotExists** meetodit, mis tagastab määratud järjekorda, kui see on juba olemas või loob uue järjekorra nimi, kui see pole juba olemas.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Kui järjekorda on loodud, `result.created` on täidetud. Kui järjekorda, `result.created` on väär.

### <a name="filters"></a>Filtrid

Valikuline filtreerimise toiminguid saab rakendada toimingute logiteavet **QueueService**. Filtreerimine toimingud võivad hõlmata logimine, automaatselt uuesti proovimine jne. Filtrid on objekte, mis rakendada allkirjaga meetodit.

    function handle (requestOptions, next)

Pärast teeme selle eeltöötlus taotluse suvandid, peab helistamiseks "järgmine" läbides järgmised allkirjaga pakkumist meetodit.

    function (returnObject, finalCallback, next)

See tagasihelistamise ja pärast töötlemist returnObject (vastuse taotluse server), peab funktsiooni tagasihelistamise autonoomsest edasi, kui see on olemas töötlemine filtrid jätkamiseks või lihtsalt autonoomsest finalCallback muidu sattuda teenuse kutsumise.

Kahe filtrid, mis rakendada uuesti loogika on kaasas Azure'i SDK Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**. Järgmine loob **QueueService** objekti, mis kasutab **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Kuidas: Lisada sõnumi järjekorda

Sõnumi järjekorda, saate lisada **createMessage** meetod uue sõnumi loomine ja lisamine järjekorda.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Juhised: Järgmise sõnumi Piilumine

Saate sõnumi ees järjekorda Piilumine ilma eemaldamine järjekorda, helistades **peekMessages** meetod. Vaikimisi **peekMessages** pisieelvaated veebisaidil ühele sõnumile.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

Funktsiooni `result` sisaldab sõnumi.

> [AZURE.NOTE] On järjekorda pole sõnumeid ei tagasta tõrge **peekMessages** kasutamisel aga pole sõnumeid tagastatakse.

## <a name="how-to-dequeue-the-next-message"></a>Kuidas: Dequeue järgmise sõnumi

Sõnumi töötlemine on kahe etapi toiming.

1. Dequeue sõnum.

2. Sõnumi kustutamine.

Sõnumi dequeue, kasutage **getMessages**. See muudab sõnumite nähtamatu järjekorda, seega pole teiste klientide saab töödelda. Kui teie taotlus on töödeldud sõnumi, helistage **deleteMessage** kustutamiseks järjekorda. Järgmises näites saab sõnumit ja seejärel kustutab selle.

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Vaikimisi peidetud sõnumi ainult 30 sekundit, pärast mida on nähtav teiste klientide. Saate määrata muu väärtuse, kasutades `options.visibilityTimeout` koos **getMessages**.

> [AZURE.NOTE]
> On järjekorda pole sõnumeid ei tagasta tõrge **getMessages** kasutamisel aga pole sõnumeid tagastatakse.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Tehke järgmist: Ootel sõnumi sisu muuta

Saate muuta sisu on sõnumi kohapealne e-abil **updateMessage**järjekorda. Järgmises näites värskendab sõnumi tekst:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Kuidas: Sõnumite Dequeuing lisasuvanditele

On kaks võimalust toomise järjekorda kaudu saate kohandada.

* `options.numOfMessages`-Tuua paketi sõnumite (kuni 32).
* `options.visibilityTimeout`-Määrata rohkem või vähem nähtamatus ajalõpp.

Järgmises näites kasutatakse **getMessages** meetodit ühe kõne 15 sõnumeid saada. Töötleb iga meilisõnumi lisavõimalusi ja seejärel soovitud tsükkel jaoks. Seda määrab ka nähtamatus ajalõpp viis minutit, et kõik sõnumid, mis on tagastatud seda meetodit.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Kuidas: Saada järjekorra pikkus

**GetQueueMetadata** tagastab metaandmeid järjekorda, sh ligikaudne arv sõnumeid järjekorda.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Kuidas: Loendi järjekorrad

Loendi järjekorda toomiseks kasutada **listQueuesSegmented**. Teatud eesliite järgi filtreeritud loendi toomiseks kasutada **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Kui kõik järjekorrad ei saa tagastada, `result.continuationToken` saab kasutada **listQueuesSegmented** esimese parameetrina või **listQueuesSegmentedWithPrefix** teine parameeter rohkem tulemeid tuua.

## <a name="how-to-delete-a-queue"></a>Kuidas: Kustutamine järjekord

Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada järjekorda objekti **deleteQueue** meetod.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Kõik sõnumid järjekorda ilma kustutades selle, kasutage **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Kuidas: ühiskasutusega juurdepääsu allkirjade töötamine

Ühiskasutusega juurdepääsu allkirjad (SAS) on turvaliselt esitada järjekorrad Varundustöö juurdepääsu võimaldamata oma salvestusruumikonto nimi või võtmed. SAS kasutatakse sageli piiratud juurdepääsu oma järjekorrad, nt lubamisel mobiilirakenduse sõnumite edastamiseks.

Usaldusväärsete rakenduste, nt pilvepõhist teenust genereeritud on SAS abil **generateSharedAccessSignature** , **QueueService**ja pakub seda pooleldi usaldusväärsete ja ebausaldusväärsete rakendus. Näiteks mobiilirakenduse kaudu. MUUDE on loodud, kasutades poliitika, mis kirjeldab algus ja lõpp kuupäevad, mille jooksul on kehtiv muude, kui ka SAS omanik loa juurdepääsu tase.

Järgmine näide loob uue ühiskasutusega juurdepääsu poliitika, mis võimaldab SAS omanik järjekorda lisada sõnumeid ja aegub 100 minutit pärast selle loomist.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Pange tähele, et host peab teabe ka, kui see on nõutav SAS omanik proovib juurde pääseda järjekorda.

Seejärel klientrakendusega kasutab muude **QueueServiceWithSAS** suhtes kuhjuda toimingute tegemiseks. Järgmises näites järjekorda loob ja sõnumi.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Kuna muude loodud lisamine Accessi, kui katse lugemine, värskendamine või kustutada sõnumid, tuleks tagastatakse tõrge.

### <a name="access-control-lists"></a>Pääsuloendid

Juurdepääsu juhtimine loend (ACL) abil soovitud SAS Accessi poliitika määramine. See on kasulik, kui soovite lubada mitme kliendi juurde järjekorda, kuid iga kliendi juurdepääsupoliitikaid eri ette.

Mõne ACL rakendatakse abil massiivi juurdepääsupoliitikaid, on seostatud iga poliitika ID-ga. Järgmises näites määratleb kaks poliitikad; üks "user1" ja "user2".

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Järgmises näites saab praeguse ACL **myqueue**ja seejärel lisab uue poliitika **setQueueAcl**abil. Seda moodust võimaldab:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Kui ACL on määratud, siis saate luua on SAS poliitika põhjal. Järgmises näites luuakse uus SAS jaoks "user2":

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud järjekorda salvestusruumi põhitõdesid, järgige neid linke salvestusruumi keerukamate tööülesannete kohta.

-   Leiate [Azure'i salvestusruumi meeskonna ajaveebi][].
-   Külastage [Azure'i salvestusruumi SDK sõlme][] hoidla github.

  [Azure'i salvestusruumi SDK sõlm]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Azure'i rakendust Service Node.js web appi loomine]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web Appi abil Azure'i tabeli teenus]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Luua ja juurutada Node.js rakendus on Azure pilveteenusesse]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
  [Koostage ja Juurutage Node.js web appi Azure Web maatriksi abil]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
