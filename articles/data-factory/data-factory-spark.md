<properties 
    pageTitle="Autonoomsest Azure'i andmed Factory säde programmides" 
    description="Saate teada, kuidas kasutada säde programmides Azure'i andmed factory, mis MapReduce tegevuse abil." 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Autonoomsest andmete Factory säde programmides
## <a name="introduction"></a>Sissejuhatus
Andmete Factory teel MapReduce tegevuse abil saate käivitada säde programmide kohta klaster Hdinsightiga säde. Vt [MapReduce tegevuse](data-factory-map-reduce.md) artikkel enne selle artikli lugemist tegevuse kasutamise kohta lisateabe saamiseks. 

## <a name="spark-sample-on-github"></a>Github säde näidis
[Säde - andmete Factory valimi github](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) näitab, kuidas MapReduce tegevuse autonoomsest säde programmi abil. Säde programm kopeerib ühe Azure'i bloobimälu container andmed lihtsalt teise. 

## <a name="data-factory-entities"></a>Andmete Factory üksused
**Säde-ADF src ADFJsons** kaust sisaldab failide andmete Factory üksuste (lingitud teenused, andmekomplektide, müügivõimaluste).  

Selles näites on kaks **lingitud teenuste** : Azure'i salvestusruumi ja Azure Hdinsighti. Määrake oma Azure storage nimi ja väärtused **StorageLinkedService.json** ning clusterUri, kasutajanime ja parooli **HDInsightLinkedService.json**.

Selles näites on kaks **andmekomplektide** : **input.json** ning **output.json**. Need failid asuvad **andmekomplektide** kausta.  Nende failide tähistavad sisend- ja andmekomplektide MapReduce tegevuste

Valimi torujuhtmete **ADFJsons/müügivõimaluste** kausta otsimine Vaadake üle müügivõimaluste, et aru saada, kuidas rakendada säde programmi MapReduce tegevuse abil. 

MapReduce tegevuse konfigureeritud autonoomsest **com.adf.sparklauncher.jar** **adflibs** ümbrises Azure salvestusruumi (määratud soovitud StorageLinkedService.json) sisse. Lähtekoodi see programm on säde-ADF/src/põhi/java/com/adf/kausta ja see nõuab säde edastamine ja käivitada säde tööd. 

> [AZURE.IMPORTANT] 
> Lugege läbi [README. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) uusimaid ja täiendava teabe enne valimi abil. 
>  
> Seda moodust oma Hdinsightiga säde kobar abil saate autonoomsest säde programmide MapReduce tegevuse abil. Abil kuvatakse nõudmisel Hdinsightiga kobar ei toetata.   


## <a name="see-also"></a>Vt ka
- [Taru tegevus](data-factory-hive-activity.md)
- [Siga tegevus](data-factory-pig-activity.md)
- [MapReduce tegevus](data-factory-map-reduce.md)
- [Hadoopi Streaming tegevus](data-factory-hadoop-streaming-activity.md)
- [R skriptide autonoomsest](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
