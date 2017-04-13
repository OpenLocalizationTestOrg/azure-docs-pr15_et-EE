<properties
    pageTitle="Azure'i tabelimälu kaudu Node.js kasutamine | Microsoft Azure'i"
    description="Liigendatud andmete talletamiseks pilveteenuses Azure'i tabelimälu, NoSQL andmete poe kaudu."
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


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Kuidas kasutada Node.js: Azure'i tabelimälu

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Selles teemas kirjeldatakse, kuidas teha levinud stsenaariumi Azure'i tabeli teenuse kasutamise Node.js rakenduses.

Koodi näited selles teemas Oletagem, et teil on juba Node.js rakenduse. Azure Node.js rakenduste loomise kohta leiate teemast mis tahes järgmistest teemadest:

- [Azure'i rakendust Service Node.js web appi loomine](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Koostage ja Juurutage Node.js web Appi abil WebMatrix Azure](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Luua ja juurutada Node.js rakendus on Azure pilveteenusesse](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (Windows PowerShelli abil)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Rakenduse juurdepääsu Azure Storage konfigureerimine

Azure Storage kasutamiseks peate Azure'i salvestusruumi SDK Node.js, mis sisaldab mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuste kogum.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Installige pakett sõlm paketi Manager (NPM) abil

1.  Kasutage käsurea liides, nt **PowerShell** (Windows), **Terminal** (Mac) või **Bash** (Unix) ja liikuge kausta, kus lõite rakenduse.

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

**Server.js** faili oma rakenduse algusse järgmine kood lisamiseks tehke järgmist.

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Azure'i salvestusruumi-ühenduse häälestamine

Azure'i mooduli ei loe keskkonna muutujate AZURE'I\_salvestusruumi\_konto ja AZURE\_salvestusruumi\_ACCESSI\_klahvi või AZURE'I\_salvestusruumi\_ühenduse\_STRINGI Azure storage kontoga ühenduse loomiseks vajalik teave. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe **TableService**helistamisel.

Näiteks säte keskkonna muutujate veebisait Azure'i [Azure portaali](https://portal.azure.com) , vt [Node.js web Appi abil Azure'i tabeli teenus].

## <a name="create-a-table"></a>Tabeli loomine

Järgmine kood **TableService** objekti loob ja kasutab seda uue tabeli loomine. Lisage järgmine **server.js**ülaosas.

    var tableSvc = azure.createTableService();

Kõne **createTableIfNotExists** loob uue tabeli nimi, kui see pole juba olemas. Järgmises näites luuakse uus tabel nimega "mytable", kui see pole juba olemas:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

Funktsiooni `result.created` on `true` kui luuakse uus tabel, ja `false` kui tabel on juba olemas. Funktsiooni `response` sisaldab teavet taotluse.

### <a name="filters"></a>Filtrid

Valikuline filtreerimise toiminguid saab rakendada toimingute logiteavet **TableService**. Filtreerimine toimingud võivad hõlmata logimine, automaatselt uuesti proovimine jne. Filtrid on objekte, mis rakendada allkirjaga meetodit.

    function handle (requestOptions, next)

Pärast teeme selle eeltöötlus taotluse suvandid, meetodit peab helistamiseks "järgmine", läbides järgmised allkirjaga pakkumist.

    function (returnObject, finalCallback, next)

See tagasihelistamise ja pärast töötlemist returnObject (vastuse taotluse server), peab funktsiooni tagasihelistamise autonoomsest edasi, kui see on olemas töötlemine filtrid jätkamiseks või lihtsalt autonoomsest finalCallback teisiti teenuse kutsumise lõpetamiseks.

Kahe filtrid, mis rakendada uuesti loogika on kaasas Azure'i SDK Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**. Järgmine loob **TableService** objekti, mis kasutab **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse

Üksuse lisamiseks esmalt luua objekti, mis määratleb oma üksuse atribuudid. Kõigi üksuste peab sisaldama **PartitionKey** ja **RowKey**, mis on kordumatu identifikaatoreid üksus.

* **PartitionKey** - määratleb sektsiooni, mis on talletatud üksus

* **RowKey** – see tuvastab kordumatult sektsiooni soovitud üksus

Nii **PartitionKey** ja **RowKey** peavad olema stringi väärtuse. Lisateavet leiate teemast [tabeli teenuse andmemudeli mõistmine](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Järgmises näites määratlemisel üksus. Pange tähele, et **tähtaeg identifikaatorist** on määratletud teatud tüüpi **Edm.DateTime**. Täpsustades tüüp on valikuline ja tüübid on tuletatud kui mitte määratud.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Olemas on ka **ajatempli** välja iga kirje, mis on määratud Azure, kui üksus on lisatud või uuendatud.

**EntityGenerator** abil saate luua üksused. Järgmises näites luuakse sama tööülesande üksuse **entityGenerator**abil.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Üksuse lisamiseks tabeli edastama üksuse objekti **insertEntity** meetod.

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Kui toiming õnnestus, `result` sisaldab [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) lisatud kirje ja `response` sisaldab teavet toiming.

Näide vastus:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Vaikimisi **insertEntity** ei tagasta lisatud üksuse osana selle `response` teavet. Kui kavatsete selle üksuse muude toimingute tegemiseks või soovite vahemälu teabe, võib olla kasulik on see osa, tagastatakse funktsiooni `result`. Saate seda teha, võimaldades **echoContent** järgmiselt:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Üksuse värskendamine

On saadaval värskendada olemasoleva üksuse mitut meetodit.

* **replaceEntity** - värskendab olemasoleva üksuse, asendades selle

* **mergeEntity** - värskendab olemasoleva üksuse, uue kinnisvarahindade ühendatakse olemasolevate üksus

* **insertOrReplaceEntity** - värskendab olemasoleva üksuse, asendades selle. Kui ole üksus on olemas, lisatakse uus

* **insertOrMergeEntity** - värskendab olemasoleva üksuse, uue kinnisvarahindade ühendatakse olemasolevate. Kui ole üksus on olemas, lisatakse uus

Järgmises näites näitab värskendamine üksuse **replaceEntity**abil:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Vaikimisi üksuse värskendamine ei kontrolli kuvamiseks, kui andmete värskendamise on muudetud varem mõni muu protsess. Toetama samaaegseid värskendused:
>
> 1. Saada ETag värskendamist objekti. See osa, tagastatakse funktsiooni `response` üksus seotud toiming ja saate tuua läbi `response['.metadata'].etag`.
>
> 2. Üksuse värskenduse toimingu täites lisage ETag teave varem allalaaditud uus üksus. Näiteks:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Värskenduse toimingu. Kui üksus on muudetud, kuna te tuua ETag-väärtus, nt muus eksemplaris oma rakenduse mõne `error` tagastatakse, märkides taotluse määratud Värskenda tingimus ei rahuldatud.

**ReplaceEntity** ja **mergeEntity**, kui üksus, mida on värskendatud pole olemas, siis värskendamist ei õnnestu. Seetõttu kui soovite talletada sõltumata üksus on juba olemas, kasutada **insertOrReplaceEntity** või **insertOrMergeEntity**.

Funktsiooni `result` eduka värskenduse sisaldab **Etag** värskendatud üksuse toimingud.

## <a name="work-with-groups-of-entities"></a>Üksuste jaotistega töötamine

Mõnikord on mõistlik esitada mitut toimingute tagamiseks atomic töötlemine server partii. Saavutada, et kasutada **TableBatch** klassi paketi loomine ja seejärel kasutage **executeBatch** meetodit **TableService** batched toimingute tegemiseks.

 Järgmises näites demonstreerib esitamise kaks üksust partii:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Eduka paketi toimingute puhul `result` sisaldab teavet iga toimingu paketi.

### <a name="work-with-batched-operations"></a>Töötamine batched toimingud

Paketti lisatud toimingute saate tutvuda kuvamine soovitud `operations` atribuut. Samuti saate järgmistest meetoditest töötamiseks toimingud:

* **tühjendage** - kustutab kõik toimingud hulgast

* **getOperations** - saab toimingu pakett

* **hasOperations** – tagastab väärtuse true, kui pakett sisaldab toimingud

* eemaldab **removeOperations** - toiming

* **maht** - paketi arv, tagastab

## <a name="retrieve-an-entity-by-key"></a>Üksuse tuua vajutamisega

**PartitionKey** ja **RowKey**konkreetset üksust, kasutage **retrieveEntity** meetod.

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Kui see toiming on lõpule viidud, `result` sisaldab üksus.

## <a name="query-a-set-of-entities"></a>Päringu määratud üksuste

Päringu tabeli, kasutage **TableQuery** objekti luua päringu avaldis, kasutades järgmist:

* **Valige** - väljad, tagastatakse päringu põhjal

* **kus** - kus klausel

    * **ja** - mõne `and` kui-tingimus

    * **või** - on `or` kui-tingimus

* **üla** - toomiseks üksuste arv


Järgmises näites koostab päring, mis tagastavad ülemise viis üksust koos mõne PartitionKey "hometasks".

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Kuna **Valige** ei kasutata, tagastatakse kõik väljad. Tabeli päring täitmiseks kasutada **queryEntities**. Järgmine näide kasutab selle päringu üksuste tulu "mytable".

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Edu korral `result.entries` massiivi üksused, mis vastavad päring sisaldab. Kui päring ei saa tagastada kõik üksused, `result.continuationToken` on mitte -*null* ja saab kasutada **queryEntities** kolmas parameeter rohkem tulemeid tuua. Algse päringu jaoks kasutada *null* kolmas parameeter.

### <a name="query-a-subset-of-entity-properties"></a>Päringu alamhulga üksuse atribuudid

Päringu tabeli saate tuua vaid mõned väljad üksus.
See vähendab läbilaskevõime ja parandada päringu jõudluse, eriti suurte üksuste puhul. **Valige** -klauslit ja edastama tagastatakse väljade nimed. Näiteks järgmine päring tagastab ainult **kirjelduse** ja **tähtaeg identifikaatorist** väljad.

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Üksuse kustutamine

Saate kustutada selle sektsiooni ja rida klahvide abil üksus. Selles näites sisaldab **task1** objekti **RowKey** ja **PartitionKey** väärtusi üksuse kustutada. Klõpsake objekti edastatakse **deleteEntity** meetod.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Kaaluge ETags üksuste kustutamisel tagamaks, et üksuse pole muudetud mõni muu protsess. ETags kasutamise kohta lisateabe saamiseks vt [värskendada üksus](#update-an-entity) .

## <a name="delete-a-table"></a>Tabeli kustutamine

Järgmine kood kustutab tabeli salvestusruumi konto.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Kui te pole kindel, kas tabel on olemas, kasutage **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Kasutada jätku sõned

Kui teil on suure hulga tulemuste tabelid päringu, otsige jätku sõned. Domeenil võib olla suurte andmehulkade oma päringu, mis ei pruugi mõista, kui te ei loo äratuntavaks jätkamiseks luba korral saadaval.

Tulemite objekti ajal päringute selliste üksuste komplektides on `continuationToken` atribuudi korral, nt luba. Seejärel saate seda päringut tehes jätkata, liigutage kursorit sektsioon ja tabeli üksused.

Kui päringu, võib continuationToken parameetri ette päringu objekti eksemplari ja tagasihelistamise funktsiooni vahel:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Kui te kontrolli selle `continuationToken` objekti, leiate atribuudid nagu `nextPartitionKey`, `nextRowKey` ja `targetLocation`, mida saab kasutada kordamiseks läbi kõik tulemused.

Olemas on ka jätku valimi Azure'i salvestusruumi Node.js repo github sees. Otsige `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Ühiskasutusega juurdepääsu allkirjade töötamine

Ühiskasutusega juurdepääsu allkirjad (SAS) on turvaliselt juurdepääsust Varundustöö tabelite võimaldamata oma salvestusruumikonto nimi või võtmed. SAS kasutatakse sageli piiratud juurdepääsu andmetele, nt lubamisel päringu kirjete mobiilirakenduse kaudu.

Usaldusväärsete rakenduste, nt pilvepõhist teenust genereeritud on SAS abil **generateSharedAccessSignature** , **TableService**ja pakub seda pooleldi usaldusväärsete ja ebausaldusväärsete rakendus nagu mobiilirakenduse kaudu. MUUDE on loodud, kasutades poliitika, mis kirjeldab algus ja lõpp kuupäevad, mille jooksul on kehtiv muude, kui ka SAS omanik loa juurdepääsu tase.

Järgmine näide loob uue ühiskasutusega juurdepääsu poliitika, mis võimaldab SAS omanik päringusse ("r") tabeli ja aegub 100 minutit pärast selle loomist.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Pange tähele, et host peab teabe ka, et see on nõutav SAS omanik proovib juurde pääseda tabeli.

Seejärel klientrakendusega kasutab muude **TableServiceWithSAS** tabeli toimingute tegemiseks. Järgmises näites ühendab tabeli ja täidab päringu.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Kuna muude loodud päringu juurdepääsu, kui katse lisamine, värskendamine või üksuste kustutamine, tuleks tagastatakse tõrge.

### <a name="access-control-lists"></a>Pääsuloendid

Juurdepääsu juhtimine loend (ACL) abil soovitud SAS Accessi poliitika määramine. See on kasulik, kui soovite lubada mitme kliendi tabeli juurde, kuid iga kliendi juurdepääsupoliitikaid eri ette.

Mõne ACL rakendatakse abil massiivi juurdepääsupoliitikaid, on seostatud iga poliitika ID-ga. Järgmises näites määratleb kahe poliitika, üks "user1" ja 'user2' üks:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Järgmises näites saab praeguse ACL **hometasks** tabeli ja seejärel lisab uue poliitika, kasutades **setTableAcl**. Seda moodust võimaldab:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Kui ACL on määratud, siis saate luua on SAS poliitika põhjal. Järgmises näites luuakse uus SAS jaoks "user2":

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate järgmistest allikatest.

-   [Azure'i salvestusruumi meeskonna ajaveebi][].
-   [Azure'i salvestusruumi SDK sõlme][] hoidla github.
-   [Node.js Arenduskeskus](/develop/nodejs/)

  [Azure'i salvestusruumi SDK sõlm]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web Appi abil Azure'i tabeli teenus]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
