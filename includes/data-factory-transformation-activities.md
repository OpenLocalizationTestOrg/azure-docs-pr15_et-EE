Azure'i andmed Factory toetab järgmisi teisendus tegevusi, mida saate lisada torujuhtmed kas ükshaaval või ühendatud koos mõne muu tegevuse.

Andmete teisendus tegevus |  Arvutage keskkonnas 
:----------------------- | :--------------------
[Taru](../articles/data-factory/data-factory-hive-activity.md) | Hdinsightiga [Hadoopi] 
[Siga](../articles/data-factory/data-factory-pig-activity.md) | Hdinsightiga [Hadoopi]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | Hdinsightiga [Hadoopi]  
[Hadoopi Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | Hdinsightiga [Hadoopi]
[Arvuti tegevusi Õppekeskuse: paketi täitmise ja Värskenda ressursi](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure'i VM 
[Salvestatud protseduur](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL-i, SQL Azure'i andmebaas või SQL Server |
[Andmete Lake Analytics U-SQL-is](../articles/data-factory/data-factory-usql-activity.md) | Azure'i andmeanalüüsi Lake 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | Hdinsightiga [Hadoopi] või Azure'i paketi
   
> [AZURE.NOTE] 
> Saate MapReduce tegevuste käivitamiseks säde programmide klaster Hdinsightiga säde. Üksikasjad leiate [Azure'i andmed Factory autonoomsest säde programmides](../articles/data-factory/data-factory-spark.md) .
> Saate luua kohandatud tegevuse R skriptide käitamiseks klõpsake Hdinsightiga klaster r installitud. Vaadake [Käivita R skript kasutamine Azure'i andmed Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).