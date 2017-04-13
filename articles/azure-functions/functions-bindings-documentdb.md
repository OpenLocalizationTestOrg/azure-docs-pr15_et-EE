<properties
    pageTitle="Azure'i funktsioonide DocumentDB sidumiste | Microsoft Azure'i"
    description="Mõista, kuidas kasutada Azure DocumentDB sidumiste Azure'i funktsioonid."
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
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure'i funktsioonide DocumentDB seosed

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerida ja koodi Azure'i DocumentDB sidumiste Azure'i funktsioonides. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure'i DocumentDB sisestusmeetodi sidumine

Sisestuskeel sidumiste saate alla laadida dokumendi DocumentDB saidikogumi ja edastama otse oma sidumine. Dokumendi ID-d saab määrata päästik, mida kasutada funktsiooni põhjal. C# funktsiooni puhul – kirjesse tehtud muudatused automaatselt saadetakse tagasi kogumise funktsiooni väljumisel edukalt.

#### <a name="functionjson-for-documentdb-input-binding"></a>function.JSON jaoks DocumentDB Sisestuskeel sidumine

*Function.json* faili pakub järgmised atribuudid.

- `name`: Kasutatav funktsioon koodi dokumendi Muutuv nimi.
- `type`: peab olema seatud "documentdb".
- `databaseName`: Andmebaasi, mis sisaldab dokumenti.
- `collectionName`: Selle saidikogumi sisaldav dokument.
- `id`: Dokumendi Id tuua. Selle atribuudi toetab sidumiste sarnane "{queueTrigger}", mis kasutab stringiväärtus järjekorda sõnumi dokumendi ID-ga.
- `connection`: See string peab olema rakenduse sätte väärtuseks DocumentDB konto lõpp-punkti. Kui valite oma konto integreerida menüü, luuakse uus rakendus, millega saate järgmisel kujul, yourAccount_DOCUMENTDB nimega. Kui teil on vaja käsitsi luua rakenduse säte tegelik ühendusstringi peab järgmisel kujul, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "suunas: peab olema seatud *"in"*.

Näide *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure'i DocumentDB Sisestuskeel koodi näide C# järjekorda päästiku
 
Näide function.json ülaltoodud kasutamisel DocumentDB Sisestuskeel sidumine tuua dokumendi ID-ga, mis vastab järjekorda sõnumi string ja andke seda parameetrit "dokument". Kui dokument ei leita, "dokument" parameeter on tühi. Dokumendi värskendatakse siis uus tekst väärtus funktsiooni väljumisel.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure'i DocumentDB Sisestuskeel koodi näide F # järjekorra lisamispäästiku

Näide function.json ülaltoodud kasutamisel DocumentDB Sisestuskeel sidumine tuua dokumendi ID-ga, mis vastab järjekorda sõnumi string ja andke seda parameetrit "dokument". Kui dokument ei leita, "dokument" parameeter on tühi. Dokumendi värskendatakse siis uus tekst väärtus funktsiooni väljumisel.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Vajate mõne `project.json` faili, mis kasutab Nugeti määramiseks soovitud `FSharp.Interop.Dynamic` ja `Dynamitey` pakettide nimega paketi sõltuvusi, nagu siin:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Seda kasutatakse Nugeti toomiseks teie sõltuvused ning viitab neid oma skripti.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure'i DocumentDB Sisestuskeel koodi näide päästiku Node.js järjekord
 
Näide function.json ülaltoodud abil kuvatakse DocumentDB Sisestuskeel sidumine tuua dokumendi ID-ga, mis vastab järjekorda sõnumi string ja andke seda, et selle `documentIn` siduv atribuut. Node.js funktsioonides, ei saadeta värskendatud dokumentide naasmiseks kogumist. Siiski saate saab edastada Sisestuskeel sidumine otse DocumentDB väljundi, köitmine nimega `documentOut` toetama värskendusi. Näites kood värskendused Sisestuskeel dokumendi atribuudi tekst ning seab selle väljundi dokumendi.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure'i DocumentDB väljund seosed

Oma funktsioonide probleemset JSON dokumentide kasutades **Azure DocumentDB dokumendi** Azure'i DocumentDB andmebaasist väljund sidumine. Azure'i DocumentDB kohta lisateabe saamiseks vaadake üle [Sissejuhatus DocumentDB](../documentdb/documentdb-introduction.md) ja [õpetuse alustamine](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>function.JSON jaoks DocumentDB väljund sidumine

Function.json faili pakub järgmised atribuudid.

- `name`: Muutuja nimi, mida kasutatakse funktsioon koodi uus dokument.
- `type`: peab olema seatud *"documentdb"*.
- `databaseName`: Andmebaasi sisaldava saidikogumi, kus luuakse uus dokument.
- `collectionName`: Selle saidikogumi, kus luuakse uus dokument.
- `createIfNotExists`: See on loogikaväärtus, mis näitab, kas selle saidikogumi luuakse, kui see pole. Vaikimisi on *false*. Põhjus, miks see on uus saidikogumid luuakse reserveeritud läbilaskevõime, mis on hinnad mõju. Lisateabe saamiseks külastage [hinnad lehe](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: See string peab olema **Taotlus, milles** on seatud DocumentDB konto lõpp-punkti. Kui valite oma konto **integreerida** menüü, saate luuakse uus rakendus säte nimega, mis viib järgmisel kujul, `yourAccount_DOCUMENTDB`. Kui teil on vaja käsitsi luua rakenduse säte tegelik ühendusstringi peab järgmisel kujul, `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: peab olema seatud *"välja"*. 
 
Näide function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure'i DocumentDB väljundi koodi näide Node.js järjekorda päästik

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Väljundi dokumendi:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure'i DocumentDB väljundi koodi näide F # järjekorda lisamispäästiku

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure'i DocumentDB väljundi koodi näide C# järjekorra päästiku


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure'i DocumentDB väljund koodi näide sätte faili nimi

Kui soovite määrata funktsiooni dokumendi nime, häälestage soovitud `id` väärtus.  Näiteks kui JSON töötaja sisu on jäeti järjekorda järgmine:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Võite kasutada järgmist C# koodi järjekorda päästik funktsiooni: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Võrdväärne F # koodi või tehke järgmist.

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Näide väljund:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
