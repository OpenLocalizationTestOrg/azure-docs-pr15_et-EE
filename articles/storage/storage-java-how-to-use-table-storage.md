<properties
    pageTitle="Kasutamine: Java tabelimälu | Microsoft Azure'i"
    description="Liigendatud andmete talletamiseks pilveteenuses Azure'i tabelimälu, NoSQL andmete poe kaudu."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Kuidas kasutada tabelimälu Java kaudu

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab teile, kuidas teha levinud stsenaariumi salvestusteenus Azure'i tabeli abil. Näidiste Java kirjutada ja kasutada [Azure salvestusruumi SDK Java][]. Stsenaariumid, mis hõlmab kaasata tabeli **loomise**, **loetelu**ja **kustutamise** tabelid, kui ka **lisamise**, **päringute**, **muutmine**ja üksuste **kustutamine** . Tabelite kohta leiate lisateavet jaotisest [järgmised toimingud](#Next-Steps) .

Märkus: Ka SDK on arendajatele, kes kasutavad rakendust Azure Storage Androidi seadmete jaoks saadaval. Lisateavet leiate [Azure'i salvestusruumi SDK Androidi jaoks][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java rakenduse loomine

Sellest juhendist kasutate salvestusruumi funktsioonid, mida saab käivitada Java rakenduses kohalikult või koodi, mis töötab web rolli või töötaja rolli Azure.

Selleks, peate installima soovitud Java Kit (JDK) ja Azure tellimuse Azure storage konto loomine. Kui te olete teinud, peate veenduge, et teie arendamise süsteemi vastab miinimumnõuded ja sõltuvused, mis on loetletud [Azure'i salvestusruumi SDK Java][] hoidla github. Kui teie süsteemi vastab nendele nõuetele, järgite juhiseid allalaadimist ja installimist Azure salvestusruumi teekide Java teie süsteemi selle hoidla. Kui olete lõpetanud need ülesanded, saab luua Java rakendus, mis kasutab selle artikli näited.

## <a name="configure-your-application-to-access-table-storage"></a>Rakenduse tabelimälu juurdepääsu konfigureerimine

Java faili, kuhu soovite kasutada Microsoft Azure storage API-de juurde pääseda tabelite ülaosas impordi järgmistest lisamiseks tehke järgmist.

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Häälestus on Azure storage ühendusstring

Kui Azure storage klient kasutab salvestusruumi ühendusstringi salvestada lõpp-punktid ja identimisteabe juurdepääsuks andmete halduse teenused. Kui kliendi rakendus töötab, peab võimaldama salvestusruumi ühendusstringi järgmises vormingus [Azure portaali](https://portal.azure.com) *account_name* ja *AccountKey* väärtuste loetletud salvestusruumi konto nimi konto salvestusruumi ja esmane kiirklahv kasutamise. Selles näites on näha, kuidas saate deklareerida staatiline väli ühendusstring mahutamiseks:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Rakenduse Microsoft Azure rolli ettenähtud stringile teenuse konfiguratsioonifail *ServiceConfiguration.cscfg*, saab salvestada ja pääseb **RoleEnvironment.getConfigurationSettings** meetod kõne. Siin on näide **säte** element, nimega *StorageConnectionString* teenuse konfiguratsioonifailis ühendusstringi hankimine:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Järgmised näidet Oletame, et olete kasutanud mõnda viisil järgmistest salvestusruumi ühendusstringi toomiseks.

## <a name="how-to-create-a-table"></a>Kuidas: tabeli loomine

**CloudTableClient** objekti võimaldab teil saada viide objektide tabelid ja üksused. Järgmine kood **CloudTableClient** objekti loob ja kasutab seda luua uue **CloudTable** objekti, mis tähistab tabelis nimega "inimesed". (Märkus: on täiendavaid võimalusi luua **CloudStorageAccount** objekte; lisateabe saamiseks leiate [Azure'i salvestusruumi kliendi SDK viide] **CloudStorageAccount** .)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Kuidas: tabelite loendi

Tabelite loendi saamiseks helistage **CloudTableClient.listTables()** meetod on iterable tabeli nimede loendi toomiseks.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Kuidas: üksuse lisamine tabelisse

Üksuste vastendamine Java objektide kohandatud klassi **TableEntity**rakendamise abil. Mugavuse huvides **TableServiceEntity** klassi rakendab **TableEntity** ja peegeldus kasutatakse atribuutide vastendamine juurde ja kinnitamiseks võimalust nimega atribuudid. Üksuse lisamiseks tabeli esmalt luua ainekursuse, mis määratleb teie üksuse atribuutide. Järgmine kood määratleb üksuse ainekursuse, mis kasutab kliendi eesnimi rea võti ja perekonnanimi sektsiooni võti. Koos ettevõtte sektsioon ja rida klahvi kordumatult tabeli üksus. Kiirem kui eri sektsiooni abil saate kasutataks üksuste sama sektsiooni võti.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Tabeli toimingutega üksuste jaoks on vaja **TableOperation** objekti. Selle objekti määratleb tehakse üksus, mis saab teostada **CloudTable** objekti toiming. Järgmine kood loob uue eksemplari **CustomerEntity** klassi mõned kliendiandmete talletamise. Koodi järgmine helistab **TableOperation.insertOrReplace** **TableOperation** objekti üksuse lisamiseks tabeli loomiseks ja uue **CustomerEntity** seostab. Lõpuks nõuab koodi **käivitada** meetod **CloudTable** objektil, "inimesed" tabeli ja selle uue **TableOperation**, mis saadab taotluse salvestusteenus, tabelisse "inimesed" kliendi uue üksuse lisamine või asendamine üksus, kui see on juba olemas.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Kuidas: paketi üksuste lisamine

Saate lisada paketi üksuste teenusesse tabeli ühe kirjutamine toiminguga. Järgmine kood loob **TableBatchOperation** objekti ja seejärel liidab kolme lisada sellele toimingud. Iga lisa toiming on lisatud uus üksus objekti, säte selle väärtused ja seejärel **Lisa** meetodit **TableBatchOperation** objekti üksuse seostamiseks uue lisa toiming. Seejärel koodi kõned **käivitada** **CloudTable** objektil, "inimesed" tabelit ja **TableBatchOperation** objekti, mis saadab salvestusteenus ühe taotluse paketi tabeli toimingute määratlemine.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Mõned asjad, mida paketi toimingute kohta:

- Saate teha kuni 100 lisa, kustutamine, ühendamine, asendada, või ühendada, ja lisada või lisada asendus-ja mis tahes kombinatsioonis ühe paketina.
- Paketi toiming võib olla too toimingut, kui see on ainult paketi.
- Kõigi üksuste ühe paketi toiming peab olema sama sektsiooni võti.
- Paketi toiming on piiratud 4MB andmete last.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Kuidas: kõigi üksuste sektsiooni tuua

Päringu tabeli sektsiooni üksuste jaoks, saate mõne **TableQuery**. Helistage **TableQuery.from** päringu kindla tabeli, mis tagastab määratud tulemitüübi loomiseks. Järgmine kood määrab üksused, kus on "Soe" sektsiooni võti filter. **TableQuery.generateFilterCondition** on abimees filtreid, päringuid loomiseks. **Kui** helistada **TableQuery.from** meetodi abil saate rakendada filtri päringu tagastatud viide. Kõne **CloudTable** objekti **käivitada** päringu käivitamisel tagastab mõne **iteraatori** koos määratud **CustomerEntity** tulemitüüp. Seejärel saate **iteraatori** tagastatud on iga tsükkel tarbimine tulemuste jaoks. Järgmine kood prinditakse iga üksuse päringutulemites konsooli väljad.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Kuidas: tuua vahemiku üksuste sektsiooni

Kui te ei soovi päringu kõik soovitud üksused sektsiooni, saate määrata vahemiku filtri võrdlustehete abil. Järgmine kood ühendab kaks filtrid saada kõigi üksuste partition "Kask", mille rea võti (eesnimi) algab tähega "E" kuni tähestiku. Seejärel prinditakse päringu tulemused. Kui kasutate paketi tabelisse lisatud üksuste lisada selle juhendi, tagastatakse ainult kaks üksust seekord (Ben ja Denise Smith); Jeff Smith ei sisalda.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Kuidas: tuua ühe üksuse

Saate kirjutada päring ühe, kindla üksuse toomiseks. Järgmine kood kõned **TableOperation.retrieve** sektsiooni võti ja rea võtme parameetrite määramiseks kliendi "Jeff Smith" asemel on **TableQuery** loomise ja teevad sama filtrite abil. Kui täidetud, too toiming tagastab ainult ühe üksuse, mitte kogumi. **GetResultAsType** meetod tekitab tulemi ülesande target, **CustomerEntity** objekti tüüp. Kui seda tüüpi ei ühildu päringu jaoks määratud tüüp, visatakse erandi. Tühiväärtus tagastatakse kui ole üksus on partition ja rida olulisi reast. Kiireim viis ühe üksuse toomine tabeli teenuse sektsiooni-ja rida, mis määrab päringu on.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Kuidas: ettevõte muutmine

Üksuse muutmiseks toomiseks tabeli teenuse, muuta isiku objekt ja salvestage tehtud muudatused tagasi tabeli teenuse Asenda või kirjakooste toiming. Järgmine kood muudab olemasoleva kliendi telefoninumber. Järgmine kood kõned asemel nõuab **TableOperation.insert** nagu me ei lisa, **TableOperation.replace**. Tabeli teenuse **CloudTable.execute** nõuab ja üksus on asendatud, kui mõnda muusse rakendusse muutunud see aeg, kuna see rakendus tuua seda. Sel juhul on erand ja üksuse peab tuua, muudetud ja salvestatud uuesti. See loodetav kokkulangevus uuesti muster on levinud jaotatud salvestusruumi süsteem.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Kuidas: päringu alamhulga üksuse atribuudid

Päringu tabeli saate tuua vaid mõned atribuudid üksus. Selle meetodi, mida nimetatakse projektsiooni, vähendab läbilaskevõime ja parandada päringu jõudluse, eriti suurte üksuste puhul. Järgmine kood päringu kasutatakse **Valige** meetodit ainult meiliaadressid üksuste tabelis. Tulemite prognooside kogumise **String** on **EntityResolver**, mis teeb tüübi teisendamise kohta serverist tagastatud üksuste abil. Saate lisateavet projektsiooni [Azure tabelite: Upsert tutvustus ja päringu projektsiooni][]. Pange tähele, et projektsiooni ei toetata kohalikku emulaator, nii et see kood töötab ainult siis, kui tabel teenuse konto abil.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Kuidas: lisamine või asendamine üksus

Sageli soovite üksuse lisamine tabelisse ei tea, kui see on juba olemas tabelis. Toimingu lisa või Asenda võimaldab teha ühe päringu, mis sisestab üksus, kui see on olemas või olemasolev fail asendada, kui see on. Tuginedes eelnevate näited, järgmine kood lisab või üksuse asendab "Walter harf". Kui olete loonud uue üksuse, järgmine kood kutsub **TableOperation.insertOrReplace** meetod. Järgmine kood siis helistab **käivitada** **CloudTable** objektil, lisa ja tabel või asendamine tabeli käivitamine parameetrid. Ainult osa värskendamiseks **TableOperation.insertOrMerge** meetodit saab kasutada hoopis. Pange tähele, et lisa-või-Asenda ei toeta kohalikku emulaator, nii, et see kood töötab ainult siis, kui tabel teenuse konto abil. Saate teada, lisa või Asenda ja lisa-või-Ühenda selles [Azure tabelite: Upsert tutvustus ja päringu projektsiooni][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Kuidas: üksuse kustutamine

Pärast seda, saate üksuse hõlpsalt kustutada. Kui üksus on tuua, kutsuvad **TableOperation.delete** üksuse kustutada. Seejärel helistage **käivitada** **CloudTable** objekti. Järgmine kood laadib ja kustutab kliendi üksus.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Kuidas: tabeli kustutamine

Lõpuks järgmine kood kustutab tabeli salvestusruumi konto. Tabel, mis on kustutatud on saadaval jaoks teatud aja jooksul pärast kustutamise, tavaliselt vähem kui 40 sekundit uuesti luua.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud tabelimälu põhitõdesid, järgmiste linkide kaudu saate teada, kuidas keerukamaid salvestusruumi toiminguid teha.

- [Azure'i salvestusruumi SDK Java][]
- [Azure'i salvestusruumi kliendi SDK viide][]
- [Azure'i salvestusruumi REST API-ga][]
- [Azure'i salvestusruumi meeskonna ajaveeb][]

Lisateabe saamiseks vt ka [Java Arenduskeskus](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure'i salvestusruumi SDK Java]: https://github.com/azure/azure-storage-java
[Azure'i salvestusruumi SDK Androidi jaoks]: https://github.com/azure/azure-storage-android
[Azure'i salvestusruumi kliendi SDK viide]: http://dl.windowsazure.com/storage/javadoc/
[Azure'i salvestusruumi REST API-ga]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure'i tabelid: Upsert ja päringu projektsiooni tutvustus]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
