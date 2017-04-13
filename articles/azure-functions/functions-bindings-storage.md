<properties
    pageTitle="Azure'i funktsioonide päästikute ja Azure Storage sidumiste | Microsoft Azure'i"
    description="Mõista, kuidas kasutada Azure Storage käivitab ja sidumiste Azure'i funktsioonid."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i töötab, funktsioonide, sündmuse töötlemiseks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure'i funktsioonide päästikute ja sidumiste Azure Storage

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerida ja koodi Azure Storage päästikute ja sidumiste Azure'i funktsioonides. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure'i salvestusruumi järjekorda päästik

#### <a name="functionjson-for-storage-queue-trigger"></a>function.JSON salvestusruumi järjekorda päästik jaoks

*Function.json* faili saate määrata järgmised atribuudid.

- `name`: Muutuja nimi kasutatakse funktsioon koodi järjekorda või sõnumi järjekorda. 
- `queueName`: Küsitlus järjekorra nimi. Järjekorda nime reegleid, vt [järjekorrad nime andmise ja metaandmete](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Rakenduse säte, mis sisaldab salvestusruumi ühendusstringi nimi. Kui jätate `connection` tühi, käivitab töötavad koos vaikimisi salvestusruumi ühendusstringi app funktsioon, mis on määratud AzureWebJobsStorage rakenduse säte.
- `type`: Peab olema seatud *queueTrigger*.
- `direction`: Peab olema seatud *sisse*. 

Näide *function.json* salvestusruumi järjekorda päästik:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Järjekorda päästik toetatud andmetüübid

Mis tahes järgmist tüüpi saate sarjadesse järjekorda teade:

* Objekti (alates JSON)
* String
* Baitide massiivis 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Järjekorda päästik metaandmed

Saate järjekorra metaandmete oma funktsiooni abil nende nimed:

* expirationTime
* insertionTime
* nextVisibleTime
* ID
* popReceipt
* dequeueCount
* queueTrigger (teine võimalus tuua järjekorda sõnumiteksti stringina)

C# koodi näites toob ja logid järjekorda metaandmete.

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Sõnumite töötlemine mürki järjekord

Sõnumid, mille sisu põhjustab nurjumise funktsiooni nimetatakse *mürki sõnumeid*. Kui funktsioon nurjub, järjekorra sõnum on kustutatud ja lõpuks on peale uuesti, põhjustades tsükli korrata. SDK saab automaatselt katkestada pärast iteratsiooni teatud tsükkel või saate seda teha käsitsi.

SDK helistavad koosolekule funktsiooni 5 korda järjekorda sõnumi töötlemine. Viienda proovige nurjumisel sõnum mürki järjekorda.

Mürki kuhjuda nimega *{originalqueuename}*-mürki. Saate kirjutada funktsiooni protsessi sõnumeid kaudu mürki kuhjuda logimine neid või teade selle käsitsi tähelepanu on vajalik. 

Kui soovite käsitleda mürki sõnumite käsitsi, saate mitu korda on sõnumi peale töötlemiseks, märkides ruudu `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure'i salvestusruumi järjekorda väljund sidumine

#### <a name="functionjson-for-storage-queue-output-binding"></a>salvestusruumi järjekorra function.JSON väljund sidumine

*Function.json* faili saate määrata järgmised atribuudid.

- `name`: Muutuja nimi kasutatakse funktsioon koodi järjekorda või sõnumi järjekorda. 
- `queueName`: Järjekorra nimi. Järjekorda nimetades reegleid, vt [järjekorrad nimetades ning metaandmete](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Rakenduse säte, mis sisaldab salvestusruumi ühendusstringi nimi. Kui jätate `connection` tühi, käivitab töötavad koos vaikimisi salvestusruumi ühendusstringi app funktsioon, mis on määratud AzureWebJobsStorage rakenduse säte.
- `type`: Peab olema seatud *järjekorda*.
- `direction`: Peab olema seatud *välja*. 

Näide *function.json* salvestusruumi järjekorra väljund Köitmine, mis kasutab järjekorda päästik ja kirjutab järjekorda sõnumi:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Järjekorda väljundi sidumine toetatud andmetüübid

Funktsiooni `queue` sidumine saate serialiseerida järjekorda sõnumile järgmist tüüpi:

* Objekti (`out T` C# loob sõnumi null objekti kui parameeter on tühi, funktsioon lõppemisel)
* String (`out string` C# loob järjekorda sõnumi, kui parameetri väärtus on tühi, funktsioon lõppemisel)
* Baitide massiivis (`out byte[]` C# töötab nagu stringi) 
* `out CloudQueueMessage`(C#, töötab stringi) 

C# võite ka siduda `ICollector<T>` või `IAsyncCollector<T>` kus `T` on üks toetatud failitüübid.

#### <a name="queue-output-binding-code-examples"></a>Järjekorda väljundi sidumine koodi näited

C# koodi näites kirjutab ühe väljundi järjekorda sõnumi Sisestuskeel järjekorda iga sõnumi kohta.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

C# koodi näites kirjutab mitu sõnumit, kasutades `ICollector<T>` (kasutada `IAsyncCollector<T>` mõne funktsiooni asünkroonse):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure'i salvestusruumi bloobimälu päästik

#### <a name="functionjson-for-storage-blob-trigger"></a>function.JSON salvestusruumi bloobimälu päästik jaoks

*Function.json* faili saate määrata järgmised atribuudid.

- `name`: Muutuja nimi selle bloobimälu funktsioon koodi kasutatakse. 
- `path`: Tee, mis määrab container jälgimiseks ja soovi korral mustri bloobimälu nimi.
- `connection`: Rakenduse säte, mis sisaldab salvestusruumi ühendusstringi nimi. Kui jätate `connection` tühi, päästiku töötavad koos vaikimisi salvestusruumi ühendusstringi app funktsioon, mis on määratud AzureWebJobsStorage rakenduse säte.
- `type`: Peab olema seatud *blobTrigger*.
- `direction`: Peab olema seatud *sisse*.

Näide *function.json* salvestusruumi bloobimälu päästik, mis on lisatud näidised-workitems ümbris plekid jälgimine:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Bloobimälu päästik toetatud andmetüübid

Funktsiooni bloobimälu saate sarjadesse mõnda järgmist tüüpi sõlm või C# funktsioonide abil:

* Objekti (alates JSON)
* String

C# funktsioonide võite ka siduda mõnda järgmist tüüpi:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Muud tüüpi deserialized [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) järgi 

#### <a name="blob-trigger-c-code-example"></a>Bloobimälu päästik C# koodi näide

C# koodi näites logib iga bloobimälu, mis lisatakse jälgida container sisu.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Bloobimälu päästik nimi mustrite

Saate määrata bloobimälu nime muster soovitud `path` atribuut. Näiteks:

```json
"path": "input/original-{name}",
```

Selle tee leiaks bloobimälu, mis on nimega *Algne-Blob1.txt* *Sisestuskeel* ümbris ja väärtus on `name` muutuja funktsioon koodi oleks `Blob1`.

Teine näide:

```json
"path": "input/{blobname}.{blobextension}",
```

Selle tee leiaks ka bloobimälu, mis on nimega *Algne-Blob1.txt*ja väärtus on `blobname` ja `blobextension` muutujate funktsioon koodi oleks *Algse-Blob1* ja *txt*.

Saate piirata plekid, määrates mustri fikseeritud väärtusega faililaiendit funktsiooni käivitavad tüüpi. Kui seate selle `path` *näidised / {nimi} .png*, käivitab ainult *png* plekid *näidised* ümbrises funktsiooni.

Kui teil on vaja bloobimälu nimed, mis on looksulud nimi nimi mustri määramiseks topeltklõpsake soovitud looksulud. Näiteks, kui soovite leida plekid *piltide* ümbrises, mis on nimed umbes järgmine:

        {20140101}-soundfile.mp3

Kasutage seda funktsiooni `path` atribuut:

        images/{{20140101}}-{name}

Näites kuvatakse `name` muutuja väärtus oleks *soundfile.mp3*. 

#### <a name="blob-receipts"></a>Bloobimälu lugemisteatised

Azure'i funktsioonide käitusaja tagab, et jagamisfunktsiooni bloobimälu päästik saab ehk rohkem kui üks kord sama uus või värskendatud bloobimälu. See, et see säilitades *bloobimälu lugemisteatised* selleks, et kindlaks teha, kui antud bloobimälu versioon on tehtud.

Bloobimälu lugemisteatised on talletatud ümbris nimega *azure-webjobs-hosts* Azure storage kontole määratud AzureWebJobsStorage ühendusstring. Bloobimälu kviitungit on järgmine teave.

* Funktsioon, mis on nõudnud bloobimälu ("*{funktsioon rakenduse nimi}*. Funktsioonid. *{funktsiooni nime}*", näiteks:"functionsf74b96f7. Functions.CopyBlob")
* Container nimi
* Bloobimälu tüüp ("BlockBlob" või "PageBlob")
* Bloobimälu nimi
* Funktsiooni ETag (bloobimälu versiooni identifikaator, nt: "0x8D1DC6E70A277EF")

Kui soovite mõne bloobimälu ümbertöötamine, saate käsitsi bloobimälu teatis jaoks selle bloobimälu *azure-webjobs-hosts* -ümbrisest kustutada.

#### <a name="handling-poison-blobs"></a>Töötlemise mürki plekid

Kui bloobimälu päästik funktsioon nurjub, SDK nõuab see uuesti, juhuks, kui selle põhjuseks ajutine tõrge. Kui sisu on bloobimälu põhjuseks on tõrge, nurjub funktsiooni iga kord, kui püüab selle bloobimälu töötlemine. Vaikimisi SDK nõuab funktsiooni kuni 5 korda antud bloobimälu. Viienda proovige nurjumisel lisab SDK sõnumi nimega *webjobs-blobtrigger-mürki*järjekorda.

Mürki plekid järjekorda sõnum on JSON-objekti, mis sisaldab järgmised atribuudid:

* FunctionId (jaotises vorming *{funktsioon rakenduse nimi}*. Funktsioonid. *{funktsiooni nime}*)
* BlobType ("BlockBlob" või "PageBlob")
* Ümbrisenimi
* BlobName
* ETag (bloobimälu versiooni identifikaator, nt: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Küsitlused jaoks suur ümbriste bloobimälu

Kui bloobimälu ümbris, mis käivitab jälgib sisaldab üle 10 000 plekid, funktsioonide käitusaja skannib logifailid vaadata uusi või muudetud plekid. Seda toimingut ei ole reaalajas; funktsiooni võib saada käivitatud kuni minuti või enam bloobimälu selle loomise järel. Lisaks [salvestusruumi logid luuakse "parima"](https://msdn.microsoft.com/library/azure/hh343262.aspx) alusel; ei ole kindel, et kõik sündmused on kaetud. Teatud tingimustel võib vastamata logid. Kui suur ümbriste bloobimälu käivitab kiirus ja usaldusväärsus piirangud pole aktsepteeritav rakenduse, on soovitatav meetod järjekorda sõnumi loomisel kuvatakse bloobimälu, ning kasutada järjekorda päästik asemel bloobimälu päästik töötlemine on bloobimälu loomine.
 
## <a id="storageblobbindings"></a>Salvestusruumi Azure'i bloobimälu sisend ja väljund seosed

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>function.JSON jaoks salvestusruumi bloobimälu sisend või väljund sidumine

*Function.json* faili saate määrata järgmised atribuudid.

- `name`: Muutuja nimi selle bloobimälu funktsioon koodi kasutatakse. 
- `path`: Tee, mis määrab container bloobimälu kaudu lugeda või kirjutada bloobimälu abil ja soovi korral mustri bloobimälu nimi.
- `connection`: Rakenduse säte, mis sisaldab salvestusruumi ühendusstringi nimi. Kui jätate `connection` tühi, sidumine töötavad koos vaikimisi salvestusruumi ühendusstringi app funktsioon, mis on määratud AzureWebJobsStorage rakenduse säte.
- `type`: Peab olema seatud *bloobimälu*.
- `direction`: Määrake *sisse* või *välja*. 

Näide *function.json* on Storage Bloobivahemälu sisend või väljund sidumine, järjekorda päästik abil saate kopeerida soovitud bloobimälu:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>Sisestusmeetodi bloobimälu ja väljundi toetatud andmetüübid

Funktsiooni `blob` sidumine saate serialiseerida või andmeatribuutide järgmist tüüpi funktsioonide Node.js või C#:

* Objekti (`out T` C# väljundi bloobimälu: loob mõne bloobimälu null objektina, kui parameetri väärtus on null, funktsioon lõppemisel)
* String (`out string` C# väljundi bloobimälu: loob mõne bloobimälu ainult siis, kui päringustringi parameetri on tühi, tagastab funktsioon)

C# funktsioonides, saate ka siduda järgmist tüüpi:

* `TextReader`(ainult sisend)
* `TextWriter`(ainult väljund)
* `Stream`
* `CloudBlobStream`(ainult väljund)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Bloobimälu väljundi C# koodi näide

C# koodi näites kopeerib on bloobimälu, kelle nimi on saadud sõnumile järjekorda.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure'i salvestusruumi tabelid sisestada ja sidumiste väljund

#### <a name="functionjson-for-storage-tables"></a>function.JSON salvestusruumi tabelite jaoks

*Function.json* saate määrata järgmised atribuudid.

- `name`: Muutuja kasutatakse funktsioon koodi sidumine tabeli nimi. 
- `tableName`: Tabeli nimi.
- `partitionKey`ja `rowKey` : kasutatakse koos ühe üksuse C# või sõlm funktsiooni lugemise või kirjutamiseks sõlm funktsioonis ühe üksuse.
- `take`: Lugeda sõlm funktsioonis sisestatud tabeli ridade maksimaalne arv.
- `filter`: Tabeli sõlm funktsioonis sisestatud OData filtriavaldise.
- `connection`: Rakenduse säte, mis sisaldab salvestusruumi ühendusstringi nimi. Kui jätate `connection` tühi, sidumine töötavad koos vaikimisi salvestusruumi ühendusstringi app funktsioon, mis on määratud AzureWebJobsStorage rakenduse säte.
- `type`: Peab olema seatud *tabeli*.
- `direction`: Määrake *sisse* või *välja*. 

Järgmises näites *function.json* kasutab järjekorda päästik lugeda ühe tabeli rida. Funktsiooni JSON pakub raske koodiga partition võtmeväärtuse ja saate määrata, et rea võti pärineb järjekorra sõnum.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Salvestusruumi tabelid sisend ja väljund toetatud andmetüübid

Funktsiooni `table` sidumine saate serialiseerida või andmeatribuutide objektide Node.js või C# funktsioonid. Objektide on RowKey ja PartitionKey atribuudid. 

C# funktsioonides, saate ka siduda järgmist tüüpi:

* `T`Kui `T` rakendab`ITableEntity`
* `IQueryable<T>`(ainult sisend)
* `ICollector<T>`(ainult väljund)
* `IAsyncCollector<T>`(ainult väljund)

#### <a name="storage-tables-binding-scenarios"></a>Salvestusruumi tabelite sidumine stsenaariumid

Tabeli sidumine toetab järgmistel juhtudel:

* Ühe rea C# või sõlm funktsiooni lugeda.

    Määrake `partitionKey` ja `rowKey`. Funktsiooni `filter` ja `take` sel juhul ei kasutata atribuudid.

* Mitme rea C# funktsiooni lugeda.

    Funktsioonide käitusaja pakub mõne `IQueryable<T>` objekti, mis on seotud tabeliga. Tippige `T` peab tulenevad `TableEntity` või rakendada `ITableEntity`. Funktsiooni `partitionKey`, `rowKey`, `filter`, ja `take` atribuudid ei kasutata sel; saate kasutada funktsiooni `IQueryable` objekti tee nõutav filtreerimine. 

* Mitme rea sõlm funktsioonis lugeda.

    Määrake soovitud `filter` ja `take` atribuudid. Ei määrata `partitionKey` või `rowKey`.

* Kirjutage C# funktsiooni üks või mitu rida.

    Funktsioonide käitusaja pakub mõne `ICollector<T>` või `IAsyncCollector<T>` seotud tabeli, kus `T` määrab skeemi üksused, mille soovite lisada. Tavaliselt on tippige `T` tuleneb `TableEntity` või rakendab `ITableEntity`, kuid see ei pea. Funktsiooni `partitionKey`, `rowKey`, `filter`, ja `take` sel juhul ei kasutata atribuudid.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Salvestusruumi tabelite näide: ühe tabeli üksus C# või sõlm lugemine

Eelmise *function.json* faili ühe tabeli üksus lugemise programmi varem töötab C# koodi järgmises näites. Järjekorra sõnum on rea võtmeväärtuse ja tabeli üksuse tüüp, mis on määratletud *run.csx* faili sisse lugeda. Tüüp, mis sisaldab `PartitionKey` ja `RowKey` atribuudid ja tulenevad `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Järgmine kood näide F # töötab ka eelnev *function.json* faili lugeda ühe tabeli üksus.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Järgmises näites sõlm kood töötab ka eelnev *function.json* faili lugeda ühe tabeli üksus.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Salvestusruumi tabelite näide: vt mitme tabeli üksuste C# 

Järgmised *function.json* ja C# koodi näide on järgmine üksuste järjekorda sõnumis määratud sektsioon võti.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# koodi lisab Azure'i salvestusruumi SDK viide, nii et saate tuletada üksuse tüüp `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Salvestusruumi tabelite näide: c tabeli üksuste loomine# 

Järgmises näites *function.json* - ja *run.csx* näitab, kuidas kirjutada tabeli üksuste C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Salvestusruumi tabelite näide: f tabeli üksuste loomine#

Järgmises näites *function.json* ja *run.fsx* näitab, kuidas kirjutada tabeli üksuste F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Salvestusruumi tabelid näide: sõlm tabeli üksuse loomine

Järgmises näites *function.json* ja *run.csx* näitab, kuidas kirjutada tabeli üksuse sõlm.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
