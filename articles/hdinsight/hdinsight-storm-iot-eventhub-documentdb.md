<properties
 pageTitle="Töötlemine andur sõidukiandmete Apache torm kohta Hdinsightiga | Microsoft Azure'i"
 description="Saate teada, kuidas sõiduki anduri andmeid sündmuse jaoturi abil Apache Storm Hdinsightile töötlemine. Lisage mudeli andmete DocumentDB ja talletada salvestusruum väljund."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Azure'i sündmuse jaoturi abil Apache Storm Hdinsightile sõiduki anduri andmeid töötlemine

Saate teada, kuidas sõiduki anduri andmeid Azure'i sündmuse jaoturi abil Apache Storm Hdinsightile töötlemine. Selles näites loeb andurite andmeid Azure'i sündmuse jaoturi, rikastab andmed poolt viitamine Azure'i DocumentDB talletatavad andmed ja lõpuks salvestab andmed üheks Azure Storage Hadoopi fail süsteemi (HDFS) abil.

![Hdinsightiga ja Internet, asjade arhitektuur diagramm](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Ülevaade

Andurid lisamine sõidukite võimaldab seadmete probleemide ajalooliste Andmetrendide põhjal prognoosida, samuti põhinevat mustri Kasutusanalüüsi tulevaste versioonide täiustamiseks. Kuigi traditsiooniline MapReduce Pakktöötlus saate kasutada analüüsiks, peate saama laadimiseks kiire ja tõhus loomine andmete kõik sõidukitelt Hadoopi enne MapReduce töötlemine võib tekkida. Lisaks võite teha analüüsi jaoks kriitilise tõrke teed (mootori temperatuur, pidurid jne) reaalajas.

Azure'i sündmuse jaoturi ehitatakse käsitlema suuri loodud andurid andmete maht, ja Apache Storm Hdinsightiga kohta saab laadimise ja töödelda andmeid enne selle üheks HDFS (tagatud Azure Storage) MapReduce töötlemiseks.

##<a name="solution"></a>Lahendus

Mootori temperatuur, ümbritsev temperatuur ja kiiruse telemeetria andmed on salvestatud andurid, siis saadetakse koos auto sõiduki ID Number (VIN) ja ajatempel sündmuse jaoturi. Sealt Storm topoloogia, mis töötab mõne Apache Storm klõpsake Hdinsightiga kobar loeb andmeid, töötleb ja salvestab HDFS.

Töötlemise ajal kasutatakse VIN Azure'i DocumentDB mudeli teabe toomiseks. See on lisatud andmevoos enne, kui see on salvestatud.

Kasutatakse Storm topoloogia komponendid on:

* **EventHubSpout** - loeb andmeid Azure'i sündmuse jaoturi kaudu

* **TypeConversionBolt** – teisendab JSON-stringi sündmuse jaoturi sisaldav mootori temperatuur, ümbritsev temperatuur, kiiruse, VIN ja ajatempli eraldi andmeväärtusi korteeži

* **DataReferencBolt** – otsib sõiduki mudeli kasutamine VIN DocumentDB kaudu

* **WasbStoreBolt** - salvestab andmed HDFS (Azure Storage)

See lahendus skeem on järgmine:

![torm topoloogia](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] See on lihtsustatud skeemi ja lahendus iga komponendi jaoks võivad kehtida mitmes eksemplaris. Näiteks levitatakse üle sõlmed Storm Hdinsightiga kobar kohta mitmes eksemplaris topoloogias iga osa.

##<a name="implementation"></a>Rakendamine

Täielik, automatiseeritud lahendus on saadaval [Hdinsightiga-Storm-näited](https://github.com/hdinsight/hdinsight-storm-examples) hoidla github osana. Selles näites kasutamiseks järgige [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Järgmised sammud

Veel näide Storm topoloogiatest, vt [näide topoloogiatest Storm Hdinsightiga kohta](hdinsight-storm-example-topology.md).
