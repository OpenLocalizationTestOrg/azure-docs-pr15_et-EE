<properties
    pageTitle="Alustamine Azure'i tabelimälu .net-i abil | Microsoft Azure'i"
    description="Liigendatud andmete talletamiseks pilveteenuses Azure'i tabelimälu, NoSQL andmete poe kaudu."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Azure'i tabelimälu kasutades .net-i kasutamise alustamine

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Azure'i tabelimälu on teenus, mis talletab NoSQL Liigendatud andmete pilveteenuses. Tabelimälu on võti/atribuut poe schemaless leiduva kujundusega asendada. Kuna tabelimälu on schemaless, on lihtne, kui teie rakendus areneb edasi vajab andmete kohandamiseks. Juurdepääs andmetele on kiire ja kulutõhus igasuguseid rakendusi. Tabelimälu on tavaliselt märkimisväärselt madalam kulu traditsiooniline SQL-i jaoks sarnaseid andmemahtusid.

Tabelimälu abil saate talletada paindlik andmekomplektide, nt veebirakenduste, aadressiraamatuid, seadme kohta ja muud tüüpi metaandmed, mis teie teenus nõuab kasutajaandmeid. Saate salvestada mis tahes üksuste arv tabeli ja salvestusruumi konto võib sisaldada mis tahes arv tabelite salvestusruumi konto tööjõudlusega ulatuses.

### <a name="about-this-tutorial"></a>Selle õpetuse kohta

Selle õpetuse näitab, kuidas kirjutada mõne levinud stsenaariumi abil Azure'i tabelimälu, sh loomise ja tabeli kustutamine ja lisamine, värskendamine, kustutamine ja päringute tabeliandmete .net-i koodi.

**Hinnanguline aega:** 45 minutit

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure'i salvestusruumi kliendi teek .net-i jaoks](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure'i Configuration Manager .net-i jaoks](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure storage konto](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Veel näidised

Täiendavad näiteid tabelimälu abil leiate [Azure'i Tabelimälu .NET-is töötamise alustamine](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Saate alla laadida rakenduse valimi ja käivitage see või sirvida kood github.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Nimeruumi deklaratsiooni lisamine

Lisage järgmine tekst `using` laused ülaosas olevat `program.cs` faili:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Ühendusstringi sõeluda

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Kliendi teenuse tabeli loomine

**CloudTableClient** klassi abil saate tuua tabelite ja üksused, mis on talletatud tabelimälu. Siin on üks viis, kuidas luua teenuse kliendi:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Nüüd olete valmis kirjutada koodi, mis loeb andmeid ja kirjutab andmed tabelimäluga.

## <a name="create-a-table"></a>Tabeli loomine

Selles näites kirjeldatakse, kuidas tabeli loomiseks, kui see pole juba olemas.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse

Üksuste vastendamine C\# objektid, kasutades kohandatud klassi tuletatud **TableEntity**. Üksuse lisamiseks tabeli loomine ainekursuse, mis määratleb teie üksuse atribuutide. Järgmine kood määratleb üksuse ainekursuse, mis kasutab kliendi eesnimi rea võti ja perekonnanimi sektsiooni võti. Koos ettevõtte sektsioon ja rida klahvi kordumatult tabeli üksus. Kiirem kui eri sektsiooni abil saate kasutataks üksuste sama sektsiooni võti, kuid mitmesuguse partition klahvide abil võimaldab tegevuste paralleelselt suurem skaleeritavus.  Atribuut, mida tuleb säilitada tabeli teenuse, atribuut peab olema avaliku atribuudi toetatud tüüp, mis seab nii `get` ja `set`.
Ka oma üksuse tüüp *peab* seada parameetri-vähem ehitaja.

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

Tabeli üksuste hõlmavad tehete **CloudTable** objekti varem loodud jaotises "Looge uus tabel" kaudu. Teostatav toiming on esindatud **TableOperation** objekti.  Koodi järgmises näites on kujutatud **CloudTable** objekti ja seejärel **CustomerEntity** objekti loomine.  Toimingu koostada luuakse **TableOperation** objekti klientide üksuse lisamiseks tabelisse.  Lõpuks käivitatakse helistades **CloudTable.Execute**toiming.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Paketi üksuste lisamine

Paketi üksuste saate lisada tabelisse ühe kirjutamine toiminguga. Mõne muu märkmeid toimingud:

-  Saate teha värskendused, kustutab ja lisab sama ühe paketi toiminguga.
-  Ühe paketi toiming võib sisaldada kuni 100 üksused.
-  Kõigi üksuste ühe paketi toiming peab olema sama sektsiooni võti.
-  See on võimalik teha päringu paketi toiminguga, peab olema ainult toimingu paketi.

<!-- -->
Järgmine kood näide loob kahe üksuse objektid ja lisab iga **TableBatchOperation** **lisamine** meetodi abil. Seejärel **CloudTable.Execute** nimetatakse toimingu käivitada.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

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
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Kõigi üksuste sektsiooni tuua

Päringu tabeli sektsiooni kõigi üksuste jaoks, kasutage **TableQuery** objekti.
Järgmises näites kood määrab üksused, kus on "Soe" sektsiooni võti filter. Selles näites prinditakse iga üksuse päringutulemites konsooli väljad.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Vahemiku üksuste sektsiooni tuua

Kui te ei soovi päringu kõik soovitud üksused sektsiooni, saate määrata vahemikus, kombineerides sektsiooni võtme filter filtri rea tootenumbri abil. Järgmine kood näide kasutab saada kõik üksused partition soe kus rea võti (eesnimi) algab tähega "E" tähestikus varem ja seejärel prinditakse päringutulemite kaks filtreid.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Ühe üksuse tuua

Saate kirjutada päring ühe, kindla üksuse toomiseks. Järgmine kood kasutab **TableOperation** määramiseks kliendi "Ben soe".
See meetod tagastab ainult ühe üksuse, mitte kogumi ja **TableResult.Result** tagastatud väärtus on **CustomerEntity** objekti.
Kiireim viis ühe üksuse toomine tabeli teenuse sektsiooni-ja rida, mis määrab päringu on.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Asendage üksus

Üksuse värskendamiseks toomiseks tabeli teenuse, üksuse objekti muuta ja seejärel salvestage tehtud muudatused tagasi tabeli teenus. Järgmine kood muudab olemasoleva kliendi telefoninumbrit. Asemel videokõned **lisada**järgmine kood kasutab **asendada**. See põhjustab üksus täielikult asendada server, kui üksuse serveris muutnud, kuna see toodud, millisel juhul toiming nurjub.  See tõrge on takistada oma rakenduse tahtmatult ülekirjutamise üks osa rakenduse vahel otsimiseks ja update tehtud muudatuse.  Tõrke proper töötlemine on tuua üksuse uuesti, tehke soovitud muudatused (kui see on kehtiv) ja seejärel toimingu teise **asendada** .  Järgmises jaotises näitab teile, kuidas seda käitumist alistada.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Lisa-või Asenda üksus

**Asendage** toimingute nurjub, kui üksus on muudetud, kuna see on serverist alla laadida.  Lisaks peate esmalt, et **asendada** selle toimingu serverist edukaks tuua üksus.
Mõnikord, aga te ei tea, kui üksus on olemas serveris ja praegusi väärtusi, salvestatakse see on oluline. Teie värskendus peaks kirjutada need kõik.  Selleks kasutage toimingut **InsertOrReplace** .  See toiming lisab üksus, kui see pole olemas või see asendab, kui seda ei, olenemata sellest, kui toimus viimast värskendust.  Järgmises näites kood Ben soe kliendi üksus on endiselt tuua, kuid siis salvestatakse see taas serveri kaudu **InsertOrReplace**.  Kirjutatakse üksuse toomine ja Värskenda toimingute vahel tehtud värskendusi.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Päringu alamhulga üksuse atribuudid

Tabeli päringu saate tuua üksus üksuse atribuutide asemel vaid mõned atribuudid. Selle meetodi, mida nimetatakse projektsiooni, vähendab läbilaskevõime ja parandada päringu jõudluse, eriti suurte üksuste puhul. Järgmine kood päring tagastab ainult meiliaadressid üksuste tabelis. Selleks **DynamicTableEntity** ja ka **EntityResolver**päringu abil. Lisateavet leiate teemast kohta projektsiooni kohta [Upsert tutvustus ja päringu projektsiooni ajaveebi postitamine][]. Pange tähele, et projektsiooni ei toeta kohalikku emulaator, nii, et see kood töötab ainult siis, kui kasutate tabelis teenuse konto.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Üksuse kustutamine

Üksuse kustutada saab hõlpsalt pärast seda, kasutades sama muster näidatud värskendamise üksus.  Järgmine kood laadib ja kustutab kliendi üksus.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Tabeli kustutamine

Lõpuks järgmine kood näide kustutab tabeli salvestusruumi konto. Tabel, mis on kustutatud on saadaval olevat uuesti loodud pärast selle aja jooksul.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Üksuste lehtede asünkroonselt tuua

Kui loete palju üksusi ja te soovite protsessi/Kuva üksuste nagu tuuakse need asemel nende kõigi juurde naasmiseks, saate otsida üksuste segmenditud päringu abil. Selles näites kujutatakse tulemeid lehtede abil mustri asünkroonse ootavad, et täitmise on blokeeritud, kui olete oodanud suure hulga tulemite tagastamiseks. .NET-is asünkroonse-ootame mustri kasutamise kohta leiate lisateavet teemast [asünkroonne programmeerimine asünkroonse ja ootame (C# ja Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud tabelimälu põhitõdesid, järgige neid linke salvestusruumi keerukamate tööülesannete kohta:

- Vaadake veel tabeli salvestusruumi proovi [Azure'i Tabelimälu .NET-is töötamise alustamine](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Tabeli teenuse viide dokumentatsioonist saadaval API-de kohta kõigi üksikasjade vaatamiseks tehke järgmist.
    - [Salvestusruumi kliendi teek .net-i viide](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API viide](http://msdn.microsoft.com/library/azure/dd179355)
- Saate teada, kuidas lihtsustada koodi kirjutamise töötamiseks Azure Storage [Azure'i WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) abil
- Saate vaadata funktsiooni juhikute lisasuvandid Azure andmete säilitamise kohta.
    - [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md) struktureerimata andmed salvestada.
    - [Ühenduse loomine SQL-andmebaasiga, kasutades .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) relatsiooniliste andmete talletamiseks.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Kasutusele Upsert ja päringu projektsiooni ajaveebipostitus]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
