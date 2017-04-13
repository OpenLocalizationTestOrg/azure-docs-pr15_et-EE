<properties
   pageTitle="Integreerimine Azure muude teenustega Lake andmesalve | Microsoft Azure'i"
   description="Mõista, kuidas Lake andmesalve ühendab Azure muude teenustega"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Lake andmesalve integreerimine Azure muude teenustega

Azure'i andmesalve Lake saab kasutada koos muude Azure'i teenuste suurema hulga stsenaariumid lubamiseks. Järgmises artiklis on loetletud teenused, mida saab integreerida Lake andmesalve.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Kasutage andmete Lake poe Azure Hdinsightiga

Saate [Windows Azure Hdinsightiga](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) kobar, mis kasutab Lake andmesalve HDFS-ühilduv salvestusruumi ette valmistada. Selles versioonis jaoks Hadoopi ja Storm kogumite Windows ja Linux, saate kasutada Lake andmesalve ainult mõne täiendav salvestusruum. Sellise kogumite siiski kasutada Azure salvestusruumi (WASB) salvestusruumi vaikesäte. Siiski for Windows ja Linux HBase kogumite, saate kasutada Lake andmesalve vaikimisi talletamist, ja/või täiendava salvestusruumi.

Ettevalmistamise mõne Hdinsightiga kobar andmete Lake poe juhised leiate teemast:

* [Azure'i portaalis andmete Lake poe mõne Hdinsightiga kobar ettevalmistamine](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Ettevalmistamise mõne Hdinsightiga kobar andmete Lake poe Azure PowerShelli abil](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Kas eelistate videod?** Täitke juhised, kuidas kasutada Lake andmesalve Hdinsightiga kogumite videote vaatamiseks linkidele.

* [Mõne Hdinsightiga kobar Lake andmesalve juurdepääsu loomine](https://mix.office.com/watch/l93xri2yhtp2)
* Kui klaster on häälestatud, [Accessi andmete Lake andmesalve taru ja siga skriptide abil](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Kasutage Lake andmesalve Azure'i andmed Lake Analytics

[Azure'i andmeanalüüsi Lake](../data-lake-analytics/data-lake-analytics-overview.md) võimaldab töötada Big Data cloud skaala. Dünaamiliselt sätted ressursid ja võimaldab teil teha analytics TB või isegi exabait arv Toetatud andmeallikate, üks neist Lake andmesalve on talletatud andmed. Andmeanalüüsi Lake on optimeeritud spetsiaalselt töötamine Azure'i Lake andmesalve - esitada kõrgeima jõudlus ja läbilaskevõime parallelization teile suur andmed töökoormus.

Kasutada andmete Lake Analytics andmete Lake poe juhised leiate teemast [Lake andmeanalüüsi Lake andmesalve kasutamise alustamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Kas eelistate videod?** Täitke juhised, kuidas kasutada Lake andmesalve Hdinsightiga kogumite videote vaatamiseks linkidest.

* [Azure'i andmeanalüüsi Lake ühenduse Azure'i andmesalve Lake](https://mix.office.com/watch/qwji0dc9rx9k)
* [Juurdepääs Azure'i Lake poe kaudu Lake andmeanalüüsi](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Lake andmesalve kasutamine koos Azure'i andmed Factory

[Azure'i andmed Factory](https://azure.microsoft.com/services/data-factory/) abil saate neelata Azure tabelites, Azure'i SQL-andmebaasi, Azure SQL-i andmelao, Azure salvestusruumi plekid ja kohapealse andmebaaside andmeid. Esimese klassi kodanikuks Azure ökosüsteemis olemise Azure'i andmed Factory saab kasutada andmete nende allikas Azure'i andmesalve Lake manustamisest korraldab.

Juhised, kuidas kasutada Azure andmete Factory andmesalve Lake, vaadata [ja sealt Lake andmesalve abil andmete Factory andmete teisaldamine](../data-factory/data-factory-azure-datalake-connector.md).

**Videod uuesti!** Lugege teemat [Andmete korraldamise Azure'i andmed Factory Azure'i andmed Lake poe kaudu](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopeerige andmed Azure salvestusruumi plekid andmesalve Lake

Azure'i andmesalve Lake pakub mõne käsurea tööriista AdlCopy, mis võimaldab kopeerida andmed Azure'i bloobimälu Lake andmesalve kontole. Lisateabe saamiseks lugege teemat [Azure salvestusruumi plekid Lake andmesalve andmeid kopeerida](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Azure'i SQL-andmebaasi ja andmete Lake poe andmete kopeerimine

Saate importida ja eksportida andmeid Azure'i SQL-andmebaasi ja andmete Lake poe Apache Sqoop. Lisateabe saamiseks lugege teemat [Lake andmesalve ja Azure SQL-i andmebaasi Sqoop abil andmete kopeerimine](data-lake-store-data-transfer-sql-sqoop.md).

**Vaadake seda videot,** [Kasutades Apache Sqoop relatsiooniliste andmeallikate ja Azure Lake andmesalve vahel liikumiseks](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Kasutage Lake andmesalve voo Analytics

Saate kasutada Lake andmesalve üks väljundeid voona abil Azure'i voo Analytics andmete talletamiseks. Lisateabe saamiseks vaadake [salvestusruumi Azure'i bloobimälu, andmete Lake salvestuskohta voo andmete kasutamine Azure voo Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Kasutage andmete Lake poe Power BI

Power BI abil saate importida andmeid andmesalve Lake konto analüüsimiseks ja visualiseerida andmeid. Lisateabe saamiseks vaadake [Lake andmesalve andmete analüüsimine kasutamine Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Kasutage Lake andmesalve andmekataloogi abil

Andmete andmete Lake poest saate registreeruma Azure'i Andmekataloogis hõlpsamaks andmete terves asutuses. Lisateabe saamiseks lugege teemat [registreerida Lake andmesalve Azure'i andmekataloogis andmetega](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Vt ka

- [Azure'i andmesalve Lake ülevaade](data-lake-store-overview.md)
- [Lake andmesalve portaali kasutamise alustamine](data-lake-store-get-started-portal.md)
- [Alustamine Lake andmesalve PowerShelli abil](data-lake-store-get-started-powershell.md)  
