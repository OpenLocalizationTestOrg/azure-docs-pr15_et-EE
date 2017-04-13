<properties
    pageTitle="Kuidas alustada tabelimälu ja Visual Studio ühendatud teenused (ASP.net-i 5) | Microsoft Azure'i"
    description="Kuidas alustada Azure'i tabelimälu ASP.net-i 5 Projectis Visual Studio pärast salvestusruumi Visual Studio abil kontoga ühenduse ühendatud teenused"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Kuidas alustada Azure'i tabelimälu ja Visual Studio ühendatud teenused

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas kasutuselevõtt Azure'i tabelimälu Visual Studios, kui olete loonud või viidatud Azure storage konto 5 ASP.net-i projekti Visual Studio **Ühendatud teenuste lisamine** dialoogiboksi kaudu.

Azure'i tabeli salvestusteenus võimaldab Liigendatud andmete talletamiseks. Teenus on NoSQL andmesalve, mida aktsepteerib autenditud kõnede ja sellest väljaspool Azure pilve. Azure'i tabelid on optimaalne liigendatud, mitte relatsiooniliste andmete talletamiseks.

**Ühendatud teenuste lisamine** toimingu installib vastav NuGet-paketid juurdepääsemiseks Azure storage projektis ja lisab ühendusstringi salvestusruumi konto konfigureerimine failide.

Rohkem üldist teavet Azure'i tabelimälu kasutamise kohta leiate teemast [Alustamine Azure'i tabelimälu .net-i abil](storage-dotnet-how-to-use-tables.md).

Enne alustamist peate esmalt konto salvestusruumi tabeli loomiseks. Näitame teile koodi Azure'i tabeli loomiseks tehke järgmist. Samuti näitame teile põhitabel ja üksuse toiminguid, nagu lisamine, muutmine, lugemispaani ja tabeli üksuste lugemise sooritamiseks. Näidiste kirjutada c\# koodi ja Azure salvestusruumi kliendi teek .net-i jaoks kasutada.

**Märkus** - API-d, mis sooritavad ASP.net-i 5 kõned Azure salvestusruum välja osa on asünkroonne. Lisateabe saamiseks vaadake [Asünkroonne programmeerimine asünkroonse ja ootame](http://msdn.microsoft.com/library/hh191443.aspx) . Alljärgnev kood eeldab asünkroonse programmeerimise meetodite kasutatakse.

## <a name="access-tables-in-code"></a>Accessi tabelid kood

ASP.net-i 5 projektide tabelid, peate lisada mis tahes C# allika failidele juurdepääs Azure'i tabelimälu järgmist.

1. Veenduge, et C# faili ülaosas nimeruumi andmed sisaldavad järgmisi **abil** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Saada **CloudStorageAccount** objekti, mis tähistab kontoteabe salvestusruumi. Järgmine kood abil saate selle oma salvestusruumi ühendusstringi ja Azure teenuse konfigureerimine salvestusruumi kontoteavet.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Märkus** - kasutada kõiki ülaltoodud kood koodi ette järgmist näidet.

3. Saada **CloudTableClient** objekti viide tabeli objektide teie salvestusruumi konto.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Saada **CloudTable** viide objekti viide kindla tabeli ja üksused.

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Tabeli loomine kood

Azure'i tabeli loomiseks lisage lihtsalt **CreateIfNotExistsAsync()**kõne.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse

Üksuse lisamiseks tabeli loomiseks ainekursuse, mis määratleb teie üksuse atribuutide. Järgmine kood määratleb mõne üksuse klassi nimetatakse **CustomerEntity** , mis kasutab kliendi eesnimi rea võti ja perekonnanimi sektsiooni võti.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Seotud üksuste tabeli toimingud on valmis abil **CloudTable** objekti saate varasemas versioonis loodud "Accessi tabelid kood." Objekti **TableOperation** tähistab toimingut tuleb teha. Koodi järgmises näites kujutatakse **CloudTable** objekti ja **CustomerEntity** objekti loomine. Toimingu koostada luuakse klientide üksuse lisamiseks tabeli lisamine **TableOperation** . Lõpuks käivitatakse helistades CloudTable.ExecuteAsync toiming.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Paketi üksuste lisamine

Saate lisada mitu üksuste ühe toimingu tabelisse. Järgmine kood näide loob kahe üksuse objektid ("Jeff soe" ja "Ben kask"), lisatakse **TableBatchOperation** objekti, meetodil **lisamine** ja käivitab toimingu, helistades CloudTable.ExecuteBatchAsync.

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Kõigi üksuste sektsiooni hankimine
Päringu tabeli kõigi üksuste sektsiooni, kasutage **TableQuery** objekti. Järgmises näites kood määrab üksused, kus on "Soe" sektsiooni võti filter. Selles näites prinditakse iga üksuse päringutulemites konsooli väljad.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a>Ühe üksuse hankimine
Saate kirjutada saada ühe, kindla üksuse päringu. Järgmine kood kasutab **TableOperation** objekti määramiseks kliendi nimega "Ben soe". See meetod tagastab ainult ühe üksuse, mitte kogumi ja **TableResult.Result** tagastatud väärtus on **CustomerEntity** objekti. Kiireim viis ühe üksuse toomine **tabeli** teenuse sektsiooni-ja rida, mis määrab päringu on.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Üksuse kustutamine
Kui te ei leia seda saate üksuse kustutada. Järgmine kood otsib kliendi üksust nimega "Ben soe" ja kui see leiab see, kustutab selle.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
