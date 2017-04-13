<properties
    pageTitle="Kasutamine: Node.js bloobimälu | Microsoft Azure'i"
    description="Azure'i bloobimälu (objekti salvestusruumi) koos pilveteenuses talletada struktureerimata andmed."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>



# <a name="how-to-use-blob-storage-from-nodejs"></a>Kuidas kasutada bloobimälu Node.js kaudu

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas teha levinud stsenaariumi bloobimälu abil. Näidiste kirjutada Node.js API kaudu. Stsenaariumid, mis hõlmab kaasata saate üles laadida, loendi, alla laadida ja kustutada plekid.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js rakenduse loomine

Juhised selle kohta, kuidas luua rakenduse Node.js, vt [Node.js veebirakenduse teenuses Azure rakenduse loomine], [luua ja juurutada Node.js rakendus on Azure pilveteenusesse] --Windows PowerShelli või [luua ja juurutada Node.js web Appi abil Web maatriksi Azure].

## <a name="configure-your-application-to-access-storage"></a>Rakenduse salvestusruumi juurdepääsu konfigureerimine

Azure'i salvestusruumi kasutamiseks peate Azure'i salvestusruumi SDK Node.js, mis sisaldab mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuste kogum.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Saada pakett sõlm paketi Manager (NPM) abil

1.  Liikuge kausta, kuhu olete loonud valimi rakenduse käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Unix) abil.

2.  Tippige käsuviiba aken **npm installida azure-salvestusruumi** . Käsu väljund on sarnane koodi järgmises näites.

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

3.  Käsitsi käivitada veendumaks, et käsk **ls** on **sõlm\_moodulid** loodud kausta. Selle kausta sisse leiate **Azure'i-salvestusruumi** paketi, mis sisaldab teeke, mida teil on vaja salvestusruumi juurde.

### <a name="import-the-package"></a>Paketi importimine

Kasutades Notepad või mõne muu tekstiredaktoris, lisage järgmine tekst **server.js** faili rakenduse kui kavatsete kasutada salvestusruumi üles:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Azure'i salvestusruumi-ühenduse häälestamine

Azure'i mooduli loete keskkonna muutujate `AZURE_STORAGE_ACCOUNT` ja `AZURE_STORAGE_ACCESS_KEY`, või `AZURE_STORAGE_CONNECTION_STRING`, Azure salvestusruumi oma kontoga ühenduse loomiseks vajalikku teavet. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe **createBlobService**helistamisel.

Näide säte keskkonna muutujate [Azure portaali](https://portal.azure.com) Azure web app leiate [Node.js web Appi abil Azure'i tabeli teenus].

## <a name="create-a-container"></a>Ümbris loomine

Objekti **BlobService** võimaldab töötada ümbriste ja plekid. Järgmine kood tekitab **BlobService** objekti. Lisage **server.js**ülaosas järgmine tekst:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Pääsete on bloobimälu anonüümselt kasutades **createBlobServiceAnonymous** ja esitada hosti aadress. Näiteks kasutada `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Looge uus ümbris, kasutage **createContainerIfNotExists**. Koodi järgmises näites luuakse uus ümbris, nimega "mycontainer":

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Kui ümbris vastloodud, `result.created` on täidetud. Kui ümbris on juba olemas, `result.created` on väär. `response`sisaldab teavet toimingu ümbris ETag teave.

### <a name="container-security"></a>Container turvalisus

Vaikimisi uue ümbriste on privaatne ja ei pääse anonüümselt. -Ümbrisest avalikuks muutmine nii, et pääseksite anonüümselt, saate seada kõigepealt ümbrist juurdepääsutase **bloobimälu** või **ümbrises**.

* **bloobimälu** – võimaldab anonüümse lugemisõigus bloobimälu sisu ja metaandmed selles pakendi sees, kuid mitte container metaandmete näiteks kõik plekid ümbris sees

* **container** – võimaldab bloobimälu sisu ja metaandmed, samuti container metaandmete anonüümse lugemisõigus

Järgmine kood näide näitab säte **bloobimälu**õiguste tase:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Teise võimalusena saate muuta juurdepääsu tase ümbris **setContainerAcl** abil saate määrata juurdepääsu tase. Järgmises näites koodi muudatusi container juurdepääsu tase.

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Tulem sisaldab teavet toimingu, sh praeguse **ETag** ümbris.

### <a name="filters"></a>Filtrid

Saate rakendada toimingute logiteavet **BlobService**valikuline filtreerimise toimingud. Filtreerimine toimingud võivad hõlmata logimine, automaatselt uuesti proovimine jne. Filtrid on objekte, mis rakendada allkirjaga meetodit.

    function handle (requestOptions, next)

Pärast teeme selle eeltöötlus taotluse suvandid, meetodit peab helistamiseks "järgmine", läbides järgmised allkirjaga pakkumist.

    function (returnObject, finalCallback, next)

See tagasihelistamise ja pärast töötlemist returnObject (vastuse taotluse server), peab funktsiooni tagasihelistamise autonoomsest edasi, kui see on olemas töötlemine filtrid jätkamiseks või lihtsalt autonoomsest finalCallback lõpetamiseks teenuse kutsumise.

Kahe filtrid, mis rakendada uuesti loogika on kaasas Azure'i SDK Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**. Järgmine loob **BlobService** objekti, mis kasutab **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

On kolme tüüpi plekid: blokeerida plekid, lehe plekid ja lisa plekid. Blokeeri plekid luba rohkem tõhus üleslaadimist suure andmed. Lisa plekid on optimeeritud lisa toimingud. Lehe plekid on optimeeritud lugemis-analüüsitoiminguid. Lisateabe saamiseks vt [mõistmine Blokeeri plekid, plekid lisada, ja lehe plekid](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blokeeri plekid

Andmete üleslaadimiseks Blokeeri bloobimälu kasutada järgmist:

* **createBlockBlobFromLocalFile** – loob uue Blokeeri bloobimälu ja lisatud faili sisu

* **createBlockBlobFromStream** – loob uue plokk bloobimälu ja laadib voo sisu

* **createBlockBlobFromText** – loob uue plokk bloobimälu ja laadib stringi sisu

* **createWriteStreamToBlockBlob** – pakub kirjutamine voo, et blokeerida bloobimälu

Järgmine kood näide lisatud **test.txt** faili sisu **myblob**sisse.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Funktsiooni `result` tagastatud nende meetodite sisaldab teavet toimingu, nt **ETag** , kuvatakse bloobimälu.

### <a name="append-blobs"></a>Lisa plekid

Andmete üleslaadimiseks uue lisa bloobimälu kasutada järgmist:

* **createAppendBlobFromLocalFile** – loob uue lisa bloobimälu ja lisatud faili sisu

* **createAppendBlobFromStream** – loob uue lisa bloobimälu ja laadib voo sisu

* **createAppendBlobFromText** – loob uue lisa bloobimälu ja laadib stringi sisu

* **createWriteStreamToNewAppendBlob** – loob uue lisa bloobimälu ja seejärel pakub kirjutada selle voo

Järgmine kood näide lisatud **test.txt** faili sisu **myappendblob**sisse.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Lisamiseks mõne olemasoleva lisa bloobimälu plokk, kasutage järgmist:

* **appendFromLocalFile** - faili sisu lisamine mõne olemasoleva lisa bloobimälu

* **appendFromStream** - mõne olemasoleva lisa bloobimälu voo sisu lisamine

* **appendFromText** - mõne olemasoleva lisa bloobimälu stringi sisu lisamine

* **appendBlockFromStream** - mõne olemasoleva lisa bloobimälu voo sisu lisamine

* **appendBlockFromText** - mõne olemasoleva lisa bloobimälu stringi sisu lisamine

> [AZURE.NOTE] appendFromXXX API-de tuleb teha mõned kliendipoolne valideerimise nurjumise kiire unncessary server kõnede vältimiseks. appendBlockFromXXX ei.

Järgmine kood näide lisatud **test.txt** faili sisu **myappendblob**sisse.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Lehe plekid

Andmete üleslaadimiseks lehe bloobimälu kasutada järgmist:

* **createPageBlob** – loob uue lehe bloobimälu teatud pikkus

* **createPageBlobFromLocalFile** – loob uue lehe bloobimälu ja lisatud faili sisu

* **createPageBlobFromStream** – loob uue lehe bloobimälu ja laadib voo sisu

* **createWriteStreamToExistingPageBlob** – pakub kirjutamine voo, et mõne olemasoleva lehe bloobimälu

* **createWriteStreamToNewPageBlob** – loob uue lehe bloobimälu ja seejärel pakub kirjutada selle voo

Järgmine kood näide lisatud **test.txt** faili sisu **mypageblob**sisse.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Lehe plekid koosnevad 512-baidine 'lehtede". Saate tõrketeate, andmete maht, mis ei ole mitu 512 üleslaadimisel.

## <a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid

Loendi plekid nõus, kasutage **listBlobsSegmented** meetodit. Kui soovite tagastada plekid teatud eesliitega, kasutage **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

Funktsiooni `result` sisaldab ka `entries` ei ole objektid, mis kirjeldavad iga bloobimälu massiivi. Kui kõik plekid ei saa tagastada, on `result` pakub on `continuationToken`, mida võib kasutada teise parameetrina tuua täiendavad kirjed.

## <a name="download-blobs"></a>Laadige alla plekid

Alla laadida soovitud bloobimälu andmeid, kasutage järgmist:

* **getBlobToLocalFile** - kirjutab faili sisu bloobimälu

* **getBlobToStream** - kirjutab bloobimälu sisu andmevoo

* **getBlobToText** - kirjutab bloobimälu sisu string

* **createReadStream** – pakub voo lugeda selle bloobimälu

Järgmine kood näide näitab **getBlobToStream** abil **myblob** bloobimälu sisu alla laadida ja salvestada selle **output.txt** -faili abil:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

Funktsiooni `result` sisaldab teavet bloobimälu, sh **ETag** teavet.

## <a name="delete-a-blob"></a>Kustutamine on bloobimälu

Lõpuks on bloobimälu kustutamiseks helistada **deleteBlob**. Järgmine kood näide kustutab bloobimälu, nimega **myblob**.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Samaaegseid juurdepääs

Samaaegseid juurdepääsu toetamiseks on bloobimälu mitme kliendi või protsessi mitmes eksemplaris, saate kasutada **ETags** või **liisingute**.

* **Etag** – võimaldab tuvastada, mis on bloobimälu või mõni muu protsess on muutnud container

* **Kapitalirendi** - võimaldab saada pikendada, välja arvatud, kirjutamiseks või kustutada mõne bloobimälu juurdepääsu teatud ajavahemiku jaoks

### <a name="etag"></a>ETag

Kasutage ETags, kui soovite lubada mitme kliendi või eksemplaris kirjutamise blokeering bloobimälu või lehe Bloobivahemälu korraga. Funktsiooni ETag võimaldab teil kindlaks teha, kui container või bloobimälu muudeti, kuna algselt loete või loodud, mis võimaldab teil muudatuste sooritatud teise kliendi või protsessi ülekirjutamise vältimiseks.

Saate määrata ETag tingimuste abil valikuline `options.accessConditions` parameeter. Järgmine kood näide ainult lisatud **test.txt** faili kui bloobimälu on juba olemas ja on ETag väärtus ohjata `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Kui kasutate ETags, üldine muster on:

1. Hankida soovitud ETag tulemusena loomine, loendi või too toiming.

2. Kontrollimine, et ETag väärtus pole muudetud toimingute teostamiseks.

Kui väärtus on muudetud – näitab, et mõne muu kliendi või eksemplari bloobimälu või container pärast muudetud hankisite ETag-väärtus.

### <a name="lease"></a>Kapitalirendi

Uue liisingu hankimiseks **acquireLease** meetodil, ning bloobimälu või container, mida soovite saada rendilepingu. Näiteks järgmine kood saab liisingueset **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Järgnevate toimingute **myblob** peab võimaldama selle `options.leaseId` parameeter. ID tagastatakse üürilepingu `result.id` **acquireLease**kaudu.

> [AZURE.NOTE] Vaikimisi on lõpmatu üürilepingu kestus. Saate määrata-lõpmatu kestus (15 ja 60 sekundi) vahel, pakkudes selle `options.leaseDuration` parameeter.

Kasutage rendilepingu eemaldamiseks **releaseLease**. Rendilepingu katkestamine, kuid takistada teistel saamine uue rendilepingu kuni algse kestus on aegunud, kasutage **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Ühiskasutusega juurdepääsu allkirjade töötamine

Ühiskasutusega juurdepääsu allkirjad (SAS) on turvaline viis anda plekid ja ümbriste Varundustöö juurdepääsu võimaldamata oma salvestusruumikonto nimi või võtmed. Piiratud juurdepääsu andmetele, nt lubamisel mobiilirakenduse kaudu juurde pääseda plekid kasutatakse sageli ühiskasutusega juurdepääsu allkirjad.

> [AZURE.NOTE] Kui ka lubate anonüümse juurdepääsu plekid, võimaldavad ühiskasutusega juurdepääsu allkirjade pakuvad lisateavet kontrollitud juurdepääsu, nagu te peate muude.

Usaldusväärsete rakenduste, nt pilvepõhist teenust genereeritud ühiskasutusega juurdepääsu allkirjade abil **generateSharedAccessSignature** , **BlobService**ja pakub seda pooleldi usaldusväärsete ja ebausaldusväärsete rakendus nagu mobiilirakenduse kaudu. Ühiskasutusse antud juurdepääs signatuurid on loodud, kasutades poliitika, mis kirjeldab algus ja lõpu kuupäevad, mil ühiskasutusega juurdepääsu allkirjad kehtivad, samuti ühiskasutusega juurdepääsu allkirjade omanik loa juurdepääsu tase.

Järgmine kood näide loob uue ühiskasutusega juurdepääsu poliitika, mis lubab ühiskasutusega juurdepääsu allkirjade omanikult **myblob** bloobimälu loetuks toiminguid ja aegub 100 minutit pärast selle loomist.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Pange tähele, et host peab teabe ka, kui see on nõutav ühiskasutusega juurdepääsu allkirjade omanik proovib juurde pääseda ümbris.

Seejärel klientrakendusega kasutab ühiskasutusega juurdepääsu allkirjade **BlobServiceWithSAS** vastu on bloobimälu toimingute tegemiseks. Järgmine saab **myblob**teavet.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Kuna ühiskasutusega juurdepääsu allkirjad saadi kirjutuskaitstud juurdepääsu korral, kui proovitakse muutmiseks soovitud bloobimälu, tagastatakse tõrge.

### <a name="access-control-lists"></a>Pääsuloendid

Mõne pääsuloendi (ACL) abil saate SAS Accessi poliitika määramine. See on kasulik, kui soovite lubada mitme kliendi juurdepääsu ümbris, kuid iga kliendi juurdepääsupoliitikaid eri ette.

Mõne ACL rakendatakse abil massiivi juurdepääsupoliitikaid, on seostatud iga poliitika ID-ga. Järgmises näites kood määratleb kahe poliitika, üks "user1" ja 'user2' üks:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Järgmine kood näide saab praeguse ACL **mycontainer**ja lisab seejärel uue poliitika **setBlobAcl**abil. Seda moodust võimaldab:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Kui ACL on määratud, siis saate luua ühiskasutusega juurdepääsu allkirjade poliitika põhjal. Koodi järgmises näites luuakse ühiskasutusega juurdepääsu signatuurid "user2":

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate järgmistest allikatest.

-   [Azure'i salvestusruumi SDK sõlm API viite][]
-   [Azure'i salvestusruumi meeskonna ajaveeb][]
-   [Azure'i salvestusruumi SDK sõlme][] hoidla github
-   [Node.js Arenduskeskus](/develop/nodejs/)
-   [Andmete AzCopy käsurea kasuliku](storage-use-azcopy.md)

[Azure'i salvestusruumi SDK sõlm]: https://github.com/Azure/azure-storage-node

[Azure'i rakendust Service Node.js web appi loomine]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js web Appi abil Azure'i tabeli teenus]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Koostage ja Juurutage Node.js web appi Azure Web maatriksi abil]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Luua ja juurutada Node.js rakendus on Azure pilveteenusesse]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure'i salvestusruumi SDK sõlm API viite]: http://dl.windowsazure.com/nodestoragedocs/index.html
