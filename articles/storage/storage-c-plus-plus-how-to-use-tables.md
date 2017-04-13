<properties
    pageTitle="Kuidas kasutada tabelimälu (C++) | Microsoft Azure'i"
    description="Liigendatud andmete talletamiseks pilveteenuses Azure'i tabelimälu, NoSQL andmete poe kaudu."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Kuidas kasutada tabelimälu C++:

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade  
Sellest juhendist näitab teile, kuidas teha levinud stsenaariumi salvestusteenus Azure'i tabeli abil. Näidised kirjutatakse C++ ja [Azure salvestusruumi kliendi teek C++ jaoks](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)kasutada. Stsenaariumid, mis hõlmab kaasata **loomine ja kustutamine tabeliks** ja **tabeli üksuste töötamine**.

>[AZURE.NOTE] Sellest juhendist eesmärgid Azure'i salvestusruumi kliendi teek c++ versioon 1.0.0 ja kohal. Soovitatav versioon on salvestusruumi kliendi teek 2.2.0, mis on saadaval [Nugeti](http://www.nuget.org/packages/wastorage) või [GitHub](https://github.com/Azure/azure-storage-cpp/)kaudu.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>C++ rakenduse loomine  
Sellest juhendist kasutate salvestusruumi funktsioonid, mida saab käitada C++ rakenduses. Selleks, peate installige Azure'i salvestusruumi kliendi teek C++ ja Azure tellimuse Azure storage konto loomiseks.  

Azure'i salvestusruumi kliendi teek c++ installimiseks saate järgmistest viisidest:

-   **Linux:** Järgige lehel [Azure'i salvestusruumi kliendi teek C++ README jaoks](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windowsi:** Klõpsake Visual Studio **Tööriistad > Nugeti Package Manager > Package Manager konsooli**. [Nugeti Package Manager konsooli](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) tippige järgmine käsk ja vajutage sisestusklahvi Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Rakenduse tabelimälu juurdepääsu konfigureerimine  
Lisage järgmine sisaldavad C++ faili, kuhu soovite kasutada Azure storage API-de juurde pääseda tabelite ülaossa.  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Mõne Azure storage ühendusstringi häälestamine  
Kui Azure storage klient kasutab salvestusruumi ühendusstringi salvestada lõpp-punktid ja identimisteabe juurdepääsuks andmete halduse teenused. Kui kliendi rakendus töötab, peate sisestama salvestusruumi ühendusstringi järgmises vormingus. [Azure portaali](https://portal.azure.com) *account_name* ja *AccountKey* väärtuste loetletud salvestusruumi konto nimi konto salvestusruumi ja salvestusruumi kiirklahv kasutamiseks. Salvestusruumi kontod ja kiirklahvide kohta leiate teavet teemast [Azure kohta salvestusruumi kontod](storage-create-storage-account.md). Selles näites on näha, kuidas saate deklareerida staatiline väli ühendusstring mahutamiseks:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Kontrollimiseks rakenduse kohaliku Windowsi-põhises arvutis, saate kasutada Azure [salvestusruumi emulaator](storage-use-emulator.md) [Azure'i SDK](https://azure.microsoft.com/downloads/)installitud. Salvestusruumi emulaator on kasuliku, mis jäljendab Azure'i bloobimälu, järjekord ja tabel teenuseid teie kohaliku arengu arvutis saadaval. Järgmises näites kirjeldatakse, kuidas saate deklareerida staatiline väli hoida oma kohalikku emulaator ühendusstring.  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Azure'i salvestusruumi emulaator käivitamiseks klõpsake nuppu **Start** või vajutage Windowsi klahvi. Hakake tippima **Azure salvestusruumi emulaator**, ja seejärel valige **Microsoft Azure'i salvestusruumi emulaator** rakenduste loendist.  

Järgmised näidet Oletame, et olete kasutanud mõnda viisil järgmistest salvestusruumi ühendusstringi toomiseks.  

## <a name="retrieve-your-connection-string"></a>Teie ühendusstringi toomiseks  
Saate **cloud_storage_account** tunni tähistada kontoteabe salvestusruumi. Ühendusstringi salvestusruumi salvestusruumi kontoteabe toomiseks saate sõeluda meetod.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Järgmiseks saada viide **cloud_table_client** ainekursuse, nagu see võimaldab teil saada viide objektide tabelid ja üksused, mis on talletatud salvestusteenus tabel. Järgmine kood loob **cloud_table_client** objekti me tuua kohal salvestusruumi konto objekti abil:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Tabeli loomine
**Cloud_table_client** objekti võimaldab teil saada viide objektide tabelid ja üksused. Järgmine kood **cloud_table_client** objekti loob ja kasutab seda uue tabeli loomine.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse
Üksuse lisamiseks tabelisse uue **table_entity** objekti loomine ja andke seda **table_operation::insert_entity**. Järgmine kood kasutab kliendi eesnimi rea võti ja perekonnanimi sektsiooni võti. Koos ettevõtte sektsioon ja rida klahvi kordumatult tabeli üksus. Üksuste sama sektsiooni võti saate kasutataks kiiremini kui eri sektsiooni abil, kuid mitmesuguse partition klahvide abil võimaldab suurem paralleelselt skaleeritavus. Lisateavet leiate teemast [Microsoft Azure'i salvestusruumi jõudlus ja skaleeritavus kontroll-loend](storage-performance-checklist.md).

Järgmine kood loob uue eksemplari **table_entity** mõned kliendiandmete talletamise. Koodi järgmine helistab **table_operation::insert_entity** **table_operation** objekti üksuse lisamiseks tabeli loomiseks ja uue üksuse tabeli seostab. Lõpuks kood kõned täita meetod **cloud_table** objekti. Ja uue **table_operation** saadab tabeli teenuse kliendi uue üksuse lisamiseks tabeli "inimesed".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Paketi üksuste lisamine
Saate lisada paketi üksuste teenusesse tabeli ühe kirjutamine toiminguga. Järgmine kood loob **table_batch_operation** objekti ja lisab seejärel kolme lisada sellele toimingud. Iga toimingu lisa lisatud uus üksus objekti, säte selle väärtused ja seejärel lisa meetod **table_batch_operation** objekti üksuse seostamiseks uue lisa toiming. Seejärel **cloud_table.execute** nimetatakse toimingu käivitada.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Mõned asjad, mida paketi toimingute kohta:  

-   Saate teha kuni 100 lisa, Kustuta, kooste, Asenda, lisa või Ühenda ja lisa või Asenda kõik koos ühe paketina.  
-   Paketi toiming võib olla too toimingut, kui see on ainult paketi.  
-   Kõigi üksuste ühe paketi toiming peab olema sama sektsiooni võti.  
-   Paketi toiming on piiratud 4-MB andmete last.  

## <a name="retrieve-all-entities-in-a-partition"></a>Kõigi üksuste sektsiooni tuua
Päringu tabeli sektsiooni kõigi üksuste jaoks, kasutage **table_query** objekti. Järgmises näites kood määrab üksused, kus on "Soe" sektsiooni võti filter. Selles näites prinditakse iga üksuse päringutulemites konsooli väljad.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Selles näites päringu toob kõik üksused, mis vastavad määratud filtrikriteeriumide. Kui teil on suur tabelid ja tuleb alla laadida tabeli üksuste sageli, soovitame teil salvestada oma andmed Azure storage plekid hoopis.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Vahemiku üksuste sektsiooni tuua
Kui te ei soovi päringu kõik soovitud üksused sektsiooni, saate määrata vahemikus, kombineerides sektsiooni võtme filter filtri rea tootenumbri abil. Järgmine kood näide kasutab saada kõik üksused partition soe kus rea võti (eesnimi) algab tähega "E" tähestikus varem ja seejärel prinditakse päringutulemite kaks filtreid.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Ühe üksuse tuua
Saate kirjutada päring ühe, kindla üksuse toomiseks. Järgmine kood kasutab **table_operation::retrieve_entity** määramiseks kliendi "Jeff Smith". See meetod tagastab ainult ühe üksuse, mitte kogumi ja tagastatud väärtus on **table_result**. Kiireim viis ühe üksuse toomine tabeli teenuse sektsiooni-ja rida, mis määrab päringu on.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Asendage üksus
Üksuse asendamiseks toomiseks tabeli teenuse, üksuse objekti muuta ja seejärel salvestage tehtud muudatused tagasi tabeli teenus. Järgmine kood muudab olemasoleva kliendi telefoninumber ja e-posti aadress. Järgmine kood kasutab asemel nõuab **table_operation::insert_entity**, **table_operation::replace_entity**. See põhjustab üksus täielikult asendada server, kui üksuse serveris muutnud, kuna see toodud, millisel juhul toiming nurjub. See tõrge on takistada oma rakenduse tahtmatult ülekirjutamise üks osa rakenduse vahel otsimiseks ja update tehtud muudatuse. Tõrke proper töötlemine on tuua üksuse uuesti, tehke soovitud muudatused (kui see on kehtiv) ja seejärel toimingu teise **table_operation::replace_entity** . Järgmises jaotises näitab teile, kuidas seda käitumist alistada.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Lisa-või Asenda üksus
**table_operation::replace_entity** toimingute nurjub, kui üksus on muudetud, kuna see on serverist alla laadida. Lisaks peate esmalt, et **table_operation::replace_entity** serverist edukaks tuua üksus. Mõnikord, aga te ei tea, kui üksus on olemas serveris ja praegusi väärtusi, salvestatakse see on oluline – oma värskendus peaks kirjutada need kõik. Selleks kasutage **table_operation::insert_or_replace_entity** toiming. See toiming lisab üksus, kui see pole olemas või see asendab, kui seda ei, olenemata sellest, kui toimus viimast värskendust. Järgmises näites kood Jeff Smith kliendi üksus on endiselt tuua, kuid siis salvestatakse see taas serveri kaudu **table_operation::insert_or_replace_entity**. Kirjutatakse vahel otsimiseks ja värskendamist üksusele tehtud värskendusi.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Päringu alamhulga üksuse atribuudid  
Päringu tabeli saate tuua vaid mõned atribuudid üksus. Järgmine kood päringu kasutatakse **table_query::set_select_columns** meetodit ainult meiliaadressid üksuste tabelis.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Päringute mõne üksuse atribuudid on tõhustamiseks, kui toomine kõik atribuudid.

## <a name="delete-an-entity"></a>Üksuse kustutamine
Pärast seda, saate üksuse hõlpsalt kustutada. Kui üksus on tuua, helistage **table_operation::delete_entity** kustutamiseks üksusega. Seejärel helistage **cloud_table.execute** meetod. Järgmine kood toob ja kustutab üksuse sektsiooni võti, "Soe" ja rea klahvi, "Jeff".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Tabeli kustutamine
Lõpuks järgmine kood näide kustutab tabeli salvestusruumi konto. Tabel, mis on kustutatud on saadaval olevat uuesti loodud pärast selle aja jooksul.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete õppinud tabelimälu põhitõdesid, järgige neid linke Lisateavet Azure Storage:  

-   [Kuidas kasutada bloobimälu C++:](storage-c-plus-plus-how-to-use-blobs.md)
-   [Kuidas kasutada järjekorda salvestusruumi C++:](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure'i salvestusruumi ressursid c++ loend](storage-c-plus-plus-enumeration.md)
-   [Salvestusruumi kliendi teek C++ teatmematerjalide](http://azure.github.io/azure-storage-cpp)
-   [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
