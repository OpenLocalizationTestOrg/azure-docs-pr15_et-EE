<properties 
    pageTitle="Andmete Factory - .NET API muutmine Logi | Microsoft Azure'i" 
    description="Kirjeldab server muudatused, funktsiooni täiendusi ja veaparandused jne... .net-i API Azure'i andmed Factory teatud versioonis." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure'i andmed Factory - .NET API muutmine Logi 
Sellest artiklist leiate teavet muudatuste kohta Azure'i andmed Factory SDK abil konkreetsete versiooni. Viimane Nugeti pakett leiate Azure'i andmed Factory [siin](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Versiooni 4.11.0
Funktsiooni täiendusi:

- Lisatud on lingitud teenuse järgmist tüüpi:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Andmekomplekti järgmist tüüpi on lisatud: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Järgmist tüüpi Kopeeri allikas on lisatud:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Versiooni 4.10.0
- Tekstivorming on lisatud valikuline järgmised atribuudid:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Lisatud on lingitud teenuse järgmist tüüpi:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Andmekomplekti järgmist tüüpi on lisatud:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Järgmist tüüpi Kopeeri allikas on lisatud:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- AzureMLBatchExecutionActivity [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) atribuudi lisamine 
    - Luba, läbides mitu web Azure seadme õ katse teenuse lähteandmed


## <a name="version-491"></a>4.9.1 versioon

### <a name="bug-fix"></a>Vea parandamine

- Taunimine [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)WebApi põhinev autentimine.

## <a name="version-490"></a>Versiooni 4.9.0

### <a name="feature-additions"></a>Funktsiooni täiendusi

- Saate lisada CopyActivity [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) ja [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) atribuudid. Vt lisateavet funktsiooni [etapiviisiline Kopeeri](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Vea parandamine

- Tutvustage ülekoormuse [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) meetod, mis võtab [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) eksemplar.
- [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) ja [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) CopySink valikuliseks märkimine

## <a name="version-480"></a>Versiooni 4.8.0

### <a name="feature-additions"></a>Funktsiooni täiendusi
- Valikuline järgmised atribuudid on lisatud Kopeeri tegevuse tüüp lubamiseks Kopeeri jõudluse häälestamine:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Versiooni 4.7.0

### <a name="feature-additions"></a>Funktsiooni täiendusi
* Lisatud uus StorageFormat tüüp [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) tüüp faile kopeerida optimeeritud rea veeruformaadis (ORC).
* Saate lisada SqlDWSink [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) ja PolyBaseSettings atribuudid.
    * Võimaldab kasutada PolyBase andmete kopeerimiseks mõnda SQL-i andmebaas.

## <a name="version-461"></a>4.6.1 versioon

### <a name="bug-fixes"></a>Veaparandusi
* Parandab loetelu tegevuse windows HTTP-päring.
    * Eemaldab taotluse last ressursi rühma nimi ja andmete factory nimi.

## <a name="version-460"></a>Versiooni 4.6.0

### <a name="feature-additions"></a>Funktsiooni täiendusi

- Järgmised atribuudid on lisatud [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Andmekomplektide](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- Järgmised atribuudid on lisatud [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Lisatud uus [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) tippige [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) tüübi määratlemine andmekomplektide, kelle andmeid on JSON-vormingus. 

## <a name="version-450"></a>Versioon 4.5.0

### <a name="feature-additions"></a>Funktsiooni täiendusi
* Lisatud [Tegevused tegevuse akna](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Lisatud meetodid toomiseks tegevuse windows filtritega põhinevate üksus (ehk teisisõnu öeldes andmete tehased, andmekomplektide, torustike ja tegevused).
* Lisatud on lingitud teenuse järgmist tüüpi: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Andmekomplekti järgmist tüüpi on lisatud: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Järgmist tüüpi Kopeeri allikas on lisatud:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Versiooni 4.4.0

### <a name="feature-additions"></a>Funktsiooni täiendusi

- Järgmised lingitud konto tüüp on lisatud nagu andmeallikate ja valamud Kopeeri tegevuste jaoks.
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Teemast [Azure SAS lingitud salvestusteenus](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) kontseptuaalset teavet ja näited. 

## <a name="version-430"></a>Versiooni 4.3.0

### <a name="feature-additions"></a>Funktsiooni täiendusi

- Järgmised lingitud teenuse tüüpi haven on lisatud andmeallikas Kopeeri tegevused:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Vaadake [HDFS abil andmete Factory andmete teisaldamine](data-factory-hdfs-connector.md) kontseptuaalset teavet ja näited. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Vaadake [teisaldamine kaudu ODBC andmeid talletab abil Azure'i andmed Factory](data-factory-odbc-connector.md) kontseptuaalset teavet ja näited. 

## <a name="version-420"></a>Versioon 4.2.0

### <a name="feature-additions"></a>Funktsiooni täiendusi

- Järgmised uue tegevuse tüüp on lisatud: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Tegevuse kohta leiate üksikasjalikumat teavet teemast [värskendamine Azure'i ML mudelite Update ressurss tegevuse abil](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Valikuline atribuut [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) on lisatud [AzureMLLinkedService klassi](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- Klassi [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) on lisatud [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) ja [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) atribuudid. 
- Luba ajalõpud kliendi kõnede andmete Factory teenuse konfigureerimine. 


## <a name="version-410"></a>Versiooni 4.1.0

### <a name="feature-additions"></a>Funktsiooni täiendusi
* Lisatud on lingitud teenuse järgmist tüüpi: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Järgmiste tegevuste on lisatud: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Andmekomplekti järgmist tüüpi on lisatud: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Lähte- ja valamu järgmiste tegevuste Kopeeri on lisatud:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Versioonis 4.0.1

### <a name="breaking-changes"></a>Suurte muudatuste
Järgmiste ümber nimetada. Uute nimede olid tunnid algse nimesid enne 4.0.0. 
 
4.0.0 nimi | 4.0.1 nimi
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Versiooni 4.0.0

### <a name="breaking-changes"></a>Suurte muudatuste



- Tunnid/liideste ümber nimetada.

| Vana nimi | Uus nimi |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabel | [Andmekomplekti](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- **Loendi** meetodid tulemusteni leheküljed kohe. Kui vastus sisaldab **NextLink** atribuut on tühi, peab toomise järgmisel lehel seni, kuni kõik lehed on tagastatud jätkamiseks klientrakendust.  Siin on näide: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Loendi** müügivõimaluste API tagastab ainult asemel üksikasjadega müügivõimaluste kokkuvõte. Näiteks tegevuste müügivõimaluste Kokkuvõte sisaldada ainult nimi ja tüüp.

### <a name="feature-additions"></a>Funktsiooni täiendusi
- Klassi [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) toetab kaks uut atribuudid **SliceIdentifierColumnName** ja **SqlWriterCleanupScript**, toetavad idempotent Kopeeri asukohta SQL Azure'i andmebaas. Artiklist [SQL Azure'i andmebaas](data-factory-azure-sql-data-warehouse-connector.md) , täpsemalt [süsteemi 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) ja [süsteem 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) jaotisi, atribuutidest üksikasjad.

- Nüüd toetame töötab salvestatud protseduur vastu Azure'i SQL-andmebaasi ja Azure SQL-i andmebaas allikad Kopeeri tegevuse käigus. [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) ja [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) klassid on järgmised atribuudid: **SqlReaderStoredProcedureName** ja **StoredProcedureParameters**. Vt artiklitest [Azure'i SQL-andmebaasi](data-factory-azure-sql-connector.md#sqlsource) ja [Azure SQL-i andmebaas](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) Azure.com atribuutidest üksikasjad.  