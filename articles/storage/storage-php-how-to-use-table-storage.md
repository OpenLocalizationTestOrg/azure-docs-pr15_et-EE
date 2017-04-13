<properties
    pageTitle="Kasutamise tabelimälu alates PHP | Microsoft Azure'i"
    description="Saate teada, kuidas PHP tabeli teenuse abil saate luua ja kustutada tabeli lisamine, kustutamine ja päringu tabeli."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Kuidas kasutada alates PHP tabelimälu

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas teha levinud stsenaariumi Azure'i tabeli teenuse abil. Näidiste php kirjutada ja kasutada [Azure SDK php][download]. Stsenaariumid, mis hõlmab kaasata **loomise ja tabeli kustutamine ja lisamine, kustutamine, ja päringute üksuste tabelis**. Azure'i tabeli teenuse kohta leiate lisateavet jaotisest [järgmised toimingud](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP rakenduse loomine

Ainult PHP rakendus, mis kasutab teenust Azure'i tabeli loomise kohta on selle viitamine tunde Azure'i SDK php kaudu oma koodi. Mis tahes arengu tööriistade abil saate luua rakenduse, sh Notepadi.

Sellest juhendist kasutate tabeli teenuse funktsioonid, mis saab helistada PHP rakendusest kohalikult või koodi, mis töötab Azure web roll, töötaja rolli või veebisaiti.

## <a name="get-the-azure-client-libraries"></a>Azure'i kliendi teegid hankimine

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Rakenduse tabeli teenuse konfigureerimine

Azure'i tabeli API-teenuse kasutamiseks peate:

1. Viide autoloader faili, kasutades [require_once] [ require_once] lause ja
2. Viide mis tahes tunnid, võite kasutada.

Järgmises näites on kujutatud, kuidas lisada autoloader fail ja **ServicesBuilder** klassi viidata.

> [AZURE.NOTE] Selles näites (ja muud selle artikli näited) Oletame, et olete installinud PHP kliendi teegid Azure helilooja kaudu. Kui installisite teekide käsitsi, peate viide on <code>WindowsAzure.php</code> autoloader fail.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Allpool toodud näidetes on `require_once` lause kuvatakse alati, kuid ainult vaja, nt käivitada klassid on viidatud.

## <a name="set-up-an-azure-storage-connection"></a>Azure'i salvestusruumi-ühenduse häälestamine

Väärtustada e Azure'i tabeli teenuse klient, peate esmalt sobiva ühendusstringi. Tabeli teenuse ühendusstringi vorming on:

Reaalajas teenuse kasutamiseks:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Juurdepääsu emulaator mälu:

    UseDevelopmentStorage=true


Luua mis tahes Azure'i teenus klient, peate kasutama **ServicesBuilder** klassi. Sa saad:

* edastama ühendusstringi otse selle või
* Kontrollige mitme välised allikad ühendusstringi **CloudConfigurationManager (CCM)** abil:
    * Vaikimisi on tegemist ühe välise andmeallikaga - keskkonna muutujate tugi
    * Saate lisada uue allikad pikendades **ConnectionStringSource** klassi

Siin kirjeldatud näited kantakse otse ühendusstring.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Tabeli loomine

**TableRestProxy** objekti saate luua tabeli **createTable** meetod. Tabeli loomisel saate seada tabeli teenuse ajalõpp. (Tabeli teenuse ajalõpp kohta leiate lisateavet teemast [Säte ajalõpud tabeli teenuse toimingute][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Klõpsake tabelinimede piirangute kohta leiate teemast [tabeli teenuse andmemudeli mõistmine][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Üksuse lisamine tabelisse

Üksuse lisamiseks tabelisse uue **üksuse** objekti loomine ja edastama **TableRestProxy -> insertEntity**. Pange tähele, et üksuse loomisel tuleb teil määrata mõne `PartitionKey` ja `RowKey`. Need on Kordumatud tunnused üksuse ja on väärtused, mida saate kasutataks kiiremini muu üksuse atribuudid. Süsteem kasutab `PartitionKey` jaotab automaatselt selle tabeli üksuste üle palju salvestusruumi sõlmi. Üksused, millel on sama `PartitionKey` sama sõlme talletatakse. (Mitu sama sõlme talletatud üksuste toiminguid teha parem kui üle erinevaid sõlmi talletatud üksuste.) Funktsiooni `RowKey` on kordumatu ID sektsiooni üksuse.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Tabeli atribuudid ja failitüüpide kohta leiate teavet teemast [tabeli teenuse andmemudeli mõistmine][table-data-model].

Klassi **TableRestProxy** pakub kahte alternatiivne üksuste lisamine: **insertOrMergeEntity** ja **insertOrReplaceEntity**. Nende meetodite kasutamiseks Loo uus **üksus** ja edastama parameetrina mõlema meetodi. Igal meetodil on üksuse lisada, kui see pole. Kui üksus on juba olemas, **insertOrMergeEntity** kinnisvara värskendab, kui atribuudid on juba olemas ja lisab uusi atribuute, kui need on olemas, samas kui **insertOrReplaceEntity** täielikult asendab olemasoleva üksuse. Järgmises näites on kujutatud **insertOrMergeEntity**kasutamise kohta. Kui üksus koos `PartitionKey` "tasksSeattle" ja `RowKey` "1" pole veel olemas, lisatakse. Juhul, kui varem lisatud (nagu Ülaltoodud näites), on `DueDate` värskendatakse atribuut, ja `Status` atribuut lisatakse. Funktsiooni `Description` ja `Location` värskendatakse ka atribuudid, kuid väärtustega mis tõhus jätke need samaks. Kui viimase kahe atribuutidest olid ei lisanud, nagu on näidatud, kuid eesmärki üksus on olemas, saavad olemasolevaid väärtusi muudetaks.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Ühe üksuse tuua

**TableRestProxy -> getEntity** meetodit saate alla laadida ühe üksuse jaoks päringute abil oma `PartitionKey` ja `RowKey`. Näites all, sektsiooni võti `tasksSeattle` ja rea `1` edastatakse **getEntity** meetodiga.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Kõigi üksuste sektsiooni tuua

Üksuse päringute ehitatud filtrite abil (leiate lisateavet teemast [päringute tabelid ja üksuste][filters]). Kõigi üksuste partition toomiseks kasutada filtrit "PartitionKey eq *partitsiooni_nimi*". Järgmises näites kujutatakse kõigi üksuste toomiseks soovitud `tasksSeattle` partition saates filtri **queryEntities** meetod.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Sektsiooni üksuste alamhulga tuua

Eelmises näites kasutatakse sama mudelit saab kasutada mis tahes sektsiooni üksuste alamhulga toomiseks. Saate tuua üksuste Alamhulk on määratud kasutate filter (leiate lisateavet teemast [päringute tabelid ja üksuste][filters]). Järgmises näites kujutatakse filtri abil saate tuua kõigi üksuste teatud `Location` ja `DueDate` vähem kui määratud kuupäeva.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Saate tuua alamhulga üksuse atribuudid

Päringu saate tuua üksuse atribuutide alamhulga. Selle meetodi, mida nimetatakse *projektsiooni*, vähendab läbilaskevõime ja parandada päringu jõudluse, eriti suurte üksuste puhul. Atribuut tuuakse määramiseks edastada **päringu -> addSelectField** meetod atribuudi nimi. Saate helistada seda meetodit mitu korda veel atribuute. Pärast käivitamist **TableRestProxy -> queryEntities**, on tagastatud üksuste ainult valitud atribuudid. (Kui soovite tagastada tabeli üksuste alamhulga, filtri kasutamine nagu on näidatud ülaltoodud päringute.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Üksuse värskendamine

Olemasoleva üksuse saate värskendada **üksuse -> Sea_atribuut** ja **üksuse -> addProperty** meetoditega olemi ja seejärel **TableRestProxy -> updateEntity**. Järgmises näites toob üksus, muudab ühe atribuudi, eemaldab teise atribuudi ja lisab uue atribuudi. Pange tähele, et saate eemaldada atribuut määrates selle väärtuseks **null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Üksuse kustutamine

Üksuse kustutamiseks liigu tabeli nimi ja ettevõtte `PartitionKey` ja `RowKey` **TableRestProxy -> deleteEntity** meetodiga.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Pange tähele, et kokkulangevus kontrolli, saate määrata Etag üksuse, kustutatakse peagi meetodil **DeleteEntityOptions -> setEtag** ja läbib **DeleteEntityOptions** objekti **deleteEntity** nimega neljas parameeter.

## <a name="batch-table-operations"></a>Tabeli toimingud

**TableRestProxy -> paketi** meetod võimaldab teil ühe taotluse mitme toimingute käivitada. Mustri siin hõlmab toimingute lisamine **BatchRequest** objekti ja seejärel läbib **BatchRequest** objekti **TableRestProxy -> paketi** meetodiga. Toimingu lisamiseks **BatchRequest** objekti, saate helistada ühte järgmistest meetoditest mitu korda.

* **addInsertEntity** (lisab toimingu insertEntity)
* **addUpdateEntity** (lisab toimingu updateEntity)
* **addMergeEntity** (lisab mergeEntity toiming)
* **addInsertOrReplaceEntity** (lisab toimingu insertOrReplaceEntity)
* **addInsertOrMergeEntity** (lisab toimingu insertOrMergeEntity)
* **addDeleteEntity** (lisab deleteEntity toiming)

Järgmises näites kirjeldatakse, kuidas käivitada **insertEntity** ja **deleteEntity** ühe taotluse.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Partiide tabeli toimingute kohta leiate lisateavet teemast [Läbimiseks üksus rühma tehingud][entity-group-transactions].

## <a name="delete-a-table"></a>Tabeli kustutamine

Lõpuks tabeli kustutamiseks edastama tabeli nime **TableRestProxy -> deleteTable** meetodit.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid Azure'i tabeli teenuse, järgige neid linke salvestusruumi keerukamate tööülesannete kohta.

- Külastage [Azure Storage meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)

Lisateabe saamiseks vt ka [PHP Arenduskeskus](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
