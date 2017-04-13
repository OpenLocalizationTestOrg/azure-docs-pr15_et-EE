<properties 
    pageTitle="Andmete teisendus: Protsess ja transformatsioon andmete | Microsoft Azure'i" 
    description="Saate teada, kuidas andmeid või Azure'i andmed Factory Hadoopi, seadme õ või Azure Lake andmeanalüüsi protsessi andmeid muuta." 
    keywords="andmete teisendus, andmete töötlemise muuta andmete teisendus tegevus"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Muuta andmete Azure'i andmed Factory
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Ülevaade 
Selles artiklis selgitatakse andmete teisendus tegevuste Azure'i andmed Factory, et abil saate muuta ja kasutatakse teie toorandmetega prognoose ja muude võimalustega. Teisendus tegevuse käivitab IT-keskkond, näiteks Windows Azure Hdinsightiga kobar või mõne Azure'i paketi. Üksikasjalikku teavet iga teisendus tegevuse pakub linke artiklitele.
 
Andmete Factory toetab järgmisi andmete teisendus tegevusi, mida saate lisada [torujuhtmed](data-factory-create-pipelines.md) kas ükshaaval või ühendatud muu tegevuse abil.

> [AZURE.NOTE] Üksikasjalikud juhised leiate teemast [loomine koos taru teisendus müügivõimaluste](data-factory-build-your-first-pipeline.md) artiklis koos selgituse.  

## <a name="hdinsight-hive-activity"></a>Hdinsightiga taru tegevus
Andmete Factory teel Hdinsightiga taru tegevuse aktiveeritakse taru päringute ise või nõudmisel Windowsi/Linuxi-põhiste Hdinsightiga kobar. Vt [Taru tegevuse](data-factory-hive-activity.md) artikkel üksikasjalikku teavet selle kohta. 

## <a name="hdinsight-pig-activity"></a>Hdinsightiga siga tegevus
Andmete Factory teel Hdinsightiga siga tegevuse aktiveeritakse siga päringute ise või nõudmisel Windowsi/Linuxi-põhiste Hdinsightiga kobar. Vt [Siga tegevuse](data-factory-pig-activity.md) artikkel üksikasjalikku teavet selle kohta. 

## <a name="hdinsight-mapreduce-activity"></a>Hdinsightiga MapReduce tegevus
Andmete Factory teel Hdinsightiga MapReduce tegevuse aktiveeritakse MapReduce programmi ise või nõudmisel Windowsi/Linuxi-põhiste Hdinsightiga kobar. Vt [MapReduce tegevuse](data-factory-map-reduce.md) artikkel üksikasjalikku teavet selle kohta.

Saate MapReduce tegevuste käivitamiseks säde programmide klaster Hdinsightiga säde. Üksikasjad leiate [Azure'i andmed Factory autonoomsest säde programmides](data-factory-spark.md) .

## <a name="hdinsight-streaming-activity"></a>Tegevuste Hdinsightiga Streaming
Hdinsightiga Streaming tegevuse andmete Factory müügivõimaluste aktiveeritakse Hadoopi Streaming programmi ise või nõudmisel Windowsi/Linuxi-põhiste Hdinsightiga kobar. Saate vaadata üksikasjalikku teavet selle kohta [Hdinsightiga Streaming tegevust](data-factory-hadoop-streaming-activity.md) .

## <a name="machine-learning-activities"></a>Seadme õppe toimingud
Azure'i andmed Factory võimaldab teil kerge vaevaga luua nii torustikud ennustav avaldatud Azure seadme õ veebiteenuse kasutamiseks. [Paketi täitmise tegevuste](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) kasutamine on Azure andmete Factory müügivõimaluste, saate autonoomsest masina õ veebiteenuse teha prognoose paketi andmeid.

Aja jooksul prognoosmudelite masina õppe, hinded katsete vaja koolitada ümber, kasutades uue Sisestuskeel kogumid. Kui olete teinud ümberõpet, mida soovite värskendada hinded veebiteenuse retrained masina õ mudeliga. Saate [Värskendada ressursside tegevuse](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) äsja koolitatud mudeliga veebiteenuse värskendada.  

[Kasutage seadme õppimise](data-factory-azure-ml-batch-execution-activity.md) kohta vaadake teavet nende masina õppe toimingud. 

## <a name="stored-procedure-activity"></a>Salvestatud protseduur tegevus
Saate kasutada SQL serveri salvestatud protseduur tegevuse andmete Factory teel autonoomsest salvestatud protseduuri järgmised andmed poed: Azure'i SQL-andmebaasi, SQL Azure'i andmebaas, SQL serveri andmebaasi teie ettevõtte või mõne Azure VM. Vt artiklit [Salvestatud protseduur tegevuse](data-factory-stored-proc-activity.md) .  

## <a name="data-lake-analytics-u-sql-activity"></a>Andmete Lake Analytics U-SQL-i tegevus
Andmete Lake Analytics U-SQL-i tegevus käivitatakse U-SQL-i skripti on Azure andmeanalüüsi Lake kobar. Vt [Andmete Analytics U-SQL-i tegevus](data-factory-usql-activity.md) artikkel üksikasju. 

## <a name="net-custom-activity"></a>.Net-i kohandatud tegevuse
Kui teil on vaja muuta nii, et ei toeta andmete Factory andmeid, saate luua kohandatud tegevuse oma andmete töötlemise loogika ja kasutada tegevuse teel. Saate konfigureerida kohandatud .NET tegevuste käivitamiseks teenuse Azure'i paketi või mõne Windows Azure Hdinsightiga kobar. Vt artiklit [kasutada kohandatud tegevuste](data-factory-use-custom-activities.md) . 

Saate luua kohandatud tegevuse R skriptide käitamiseks klõpsake Hdinsightiga klaster r installitud. Vaadake [Käivita R skript kasutamine Azure'i andmed Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Arvutage keskkonnas
Saate luua lingitud teenuse Arvuta keskkonnas, ja seejärel kasutage lingitud teenuse teisendus tegevuse määratlemisel. On kahte tüüpi andmete Factory ei toeta Arvuta keskkonnas. 

1. **Nõudmisel**: sel juhul IT-keskkond on täielikult hallata andmete Factory. See on automaatselt loodud teenuse andmete Factory enne, kui tööd on protsess andmeid ja eemaldada, kui töö on valmis. Saate konfigureerida ja Varundustöö juhtelemendisätted nõudmisel Arvuta keskkonna töö täitmise, kobar haldus ja eellaadimisel toimingud. 
2. **Kuvage teie enda**: sel juhul saate registreeruda oma IT-keskkond (nt Hdinsightiga klaster) andmete Factory lingitud teenust. IT-keskkond haldab teie ja andmete Factory teenuse kasutab seda tegevuste käivitamiseks. 

Vt [Arvutada lingitud teenuste](data-factory-compute-linked-services.md) artikkel kohta arvutada teenuste andmete Factory ei toeta. 


## <a name="summary"></a>Kokkuvõte
Azure'i andmed Factory toetab andmete teisendus järgmiste tegevuste ja Arvuta keskkonnas tegevuste jaoks. Tegevuste teisendus saate lisada torujuhtmed kas ükshaaval või muu tegevuse abil ühendatud.

Andmete teisendus tegevus |  Arvutage keskkonnas 
:----------------------- | :--------------------
[Taru](data-factory-hive-activity.md) | Hdinsightiga [Hadoopi] 
[Siga](data-factory-pig-activity.md) | Hdinsightiga [Hadoopi]  
[MapReduce](data-factory-map-reduce.md) | Hdinsightiga [Hadoopi]  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md) | Hdinsightiga [Hadoopi]
[Arvuti tegevusi Õppekeskuse: paketi täitmise ja Update ressurss](data-factory-azure-ml-batch-execution-activity.md) | Azure'i VM 
[Salvestatud protseduur](data-factory-stored-proc-activity.md) | Azure SQL-i, SQL Azure'i andmebaas või SQL Server |
[Andmete Lake Analytics U-SQL-is](data-factory-usql-activity.md) | Azure'i andmeanalüüsi Lake 
[DotNet](data-factory-use-custom-activities.md) | Hdinsightiga [Hadoopi] või Azure'i paketi
   

