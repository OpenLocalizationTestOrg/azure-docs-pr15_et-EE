<properties 
   pageTitle="Andmete stsenaariumid, mis hõlmavad andmete Lake poe | Microsoft Azure'i" 
   description="Mõista erinevaid stsenaariumeid lähemalt ja tööriistade abil, millised andmed saate märkimisväärselt, töödeldud alla laadida ja visualiseerimise andmete Lake poes" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Azure'i andmesalve Lake kasutamise suur andmete nõuded

Suur töötlemine on neli põhietapid:

* Reaalajas või pakkidena andmesalve söömisega suurte andmemahtude
* Andmete töötlemiseks
* Andmete allalaadimine
* Andmete visualiseerimine

Selles artiklis vaatame need etapid Lake andmesalve Azure'i suvandid ja suur andmeid oma vajaduste saadaolevate tööriistade suhtes.

## <a name="ingest-data-into-data-lake-store"></a>Andmete neelata andmete Lake poest

Selles jaotises tõsta esile eri allikatest andmeid ja erineval viisil, milles andmeid võib manustada Lake andmesalve kontole.

![Neelata andmeid andmesalve Lake] (./media/data-lake-store-data-scenarios/ingest-data.png "Neelata andmeid andmesalve Lake")

### <a name="ad-hoc-data"></a>Sihtotstarbelise andmete

See tähistab väiksema andmekogumi, mis on kasutatud prototüüpimiseks suur andmete rakenduse. On võimalust söömisega sihtotstarbelise andmete sõltuvalt andmete allikas.

| Andmeallikas        | Neelata abil                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Kohalikus arvutis     | <ul> <li>[Azure'i portaal](/data-lake-store-get-started-portal.md)</li> <li>[Azure'i PowerShelli](data-lake-store-get-started-powershell.md)</li> <li>[Azure'i platvormidel CLI](data-lake-store-get-started-cli.md)</li> <li>[Visual Studio andmete Lake tööriista abil](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Salvestusruumi Azure'i bloobimälu | <ul> <li>[Azure'i andmed Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy tööriist](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp töötavate Hdinsightiga kobar](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Voona andmed

See on andmeid, mis võib olla loodud erinevatest allikatest, nt rakendused, seadmed, andurid jne. Andmed võib manustada Lake poodi erinevaid tööriistu. Nende tööriistade tavaliselt jäädvustada ja töödelda sündmuse – sündmuse alustel reaalajas, ja kirjutage siis sündmuste pakettidena Lake andmesalve sisse, et ta saab edasi töödelda. 

Järgnevalt on tööriistu, mille abil saate:
 
* [Azure'i voo Analytics] (.. / voo-analytics-andmete-lake-väljundi) - sündmuste märkimisväärselt sisse sündmuse jaoturi saab kirjutada Azure'i andmed Lake Azure'i andmesalve Lake väljund abil.
* [Azure Hdinsightiga Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) - te saate kirjutamise andmed otse Lake andmesalve Storm kobar.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – saate saadud sündmuse jaoturi sündmused ja siis kirjutada selle Lake andmesalve [Andmete Lake poe .NET SDK](data-lake-store-get-started-net-sdk.md)abil.

### <a name="relational-data"></a>Relatsiooniliste andmete

Samuti saate andmeallika andmete relatsioonandmebaasidest. Aja jooksul, relatsiooniandmebaasid koguda andmed, mis annab olulise ülevaate kui töödeldud – suured andmete kohaletoimetamisel. Järgmised tööriistade abil saate nende andmete viimine Lake andmesalve.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure'i andmed Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web server logiandmed (laadi kohandatud rakenduste)

Seda tüüpi andmekomplekti nimetatakse konkreetselt, sest web serveri logifailide andmeanalüüsi on levinud suur andmete rakenduste puhul kasutada ja nõuab suurel hulgal logifailid Lake andmesalve üles laadida. Järgmised tööriistade abil saate kirjutada oma skriptide või rakendused üles laadida neid andmeid.

* [Azure'i platvormidel CLI](data-lake-store-get-started-cli.md)
* [Azure'i PowerShelli](data-lake-store-get-started-powershell.md)
* [Azure'i andmesalve Lake .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure'i andmed Factory](../data-factory/data-factory-data-movement-activities.md)

Üleslaadimine veebi serveri logifailide andmeid, ja ka muud tüüpi andmeid (nt social tundeid andmeid) üles, on hea lähenemine kirjutada oma kohandatud skriptide/rakendustes, sest see kaudu saate oma andmeid üleslaadimise komponent suuremat suur andmete rakenduse osana. Mõnel juhul järgmine kood võib kuluda skripti või lihtsa käsurea kasuliku. Muudel juhtudel võib kasutada koodi ärirakendusele või lahenduse suur töötlemise integreerida.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Windows Azure Hdinsightiga kogumite seotud andmete

Kobar enamiku Hdinsightiga (Hadoopi HBase, torm) toetada Lake andmesalve salvestusruumi andmete talletuskoht. Hdinsightiga kogumite pääsevad andmetele juurde: Azure'i salvestusruumi plekid (WASB). Parema jõudluse tagamiseks saate andmed kopeerida seostatud klaster Lake andmesalve kontole WASB. Järgmised tööriistade abil saate andmed kopeerida.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy teenus](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure'i andmed Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Andmed salvestatakse kohapealne või IaaS Hadoopi kogumite

Suurte andmehulkade salvestatakse olemasoleva Hadoopi kogumite kohalikult HDFS kasutamine arvutites. Hadoopi rühmad võib olla mõni kohapealne juurutus või võib olla mõni IaaS kobar Azure. Põhjusi võib olla nõuete selliseid andmete kopeerimine Azure'i andmesalve Lake ühekordne lähenemisviisi või korduva mood. On erinevaid võimalusi, mille abil saate selle saavutamiseks. Allpool on alternatiive ja seotud kompromisse loendit.

| Lähenemisviisi  | Üksikasjad | Eelised   | Kaalutlused  |
|-----------|---------|--------------|-----------------|
| Kopeerige andmed otse Hadoopi kogumite Azure'i andmesalve Lake Azure'i Factory (ADF) abil | [ADF toetab HDFS andmeallikana](../data-factory/data-factory-hdfs-connector.md) | ADF out-of-box toetab HDFS ja esimese klassi end-to-end haldus ja jälgimine | Nõuab Andmehalduslüüsi kasutusele võtta kohapealne või IaaS kobar |
| Andmete eksportimine Hadoopi failidena. Seejärel kopeerige failid Azure'i Lake andmesalve kasutamise korral süsteem.                                   | Azure'i andmesalve Lake abil saate faile kopeerida: <ul><li>[OS Windows Azure'i PowerShelli](data-lake-store-get-started-powershell.md)</li><li>[Azure'i platvormidel CLI Windows OS](data-lake-store-get-started-cli.md)</li><li>Kohandatud rakenduse mis tahes andmete Lake poe SDK abil</li></ul> | Kiire alustamine. Saate teha kohandatud üles                                                   | Mitme järgmist toimingut, mis hõlmab mitme tehnoloogiad. Halduse ja jälgimise kasvab olla keeruline, arvestades kohandatud tööriistade ajas |
| Hadoopi andmete kopeerimine Azure Storage Distcp abil. Klõpsake andmete kopeerimine Azure Storage Lake andmesalve kasutamise korral süsteem. | Saate kopeerida andmeid Azure Storage Lake andmesalve abil: <ul><li>[Azure'i andmed Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy tööriist](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp tööta Hdinsightiga kogumite kohta](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Saate valida avatud lähtekoodi tööriistu. | Mitme järgmist toimingut, mis hõlmab mitme tehnoloogiad |

### <a name="really-large-datasets"></a>Väga suurte andmekogumite

Üleslaadimise andmekomplektide, et mitu TB vahemik, kasutades eespool kirjeldatud võib mõnikord olla aeglane ja kulukas. Sellisel juhul saate suvanditest.

* **Azure'i ExpressRoute abil**. Azure'i ExpressRoute saate luua privaatne seoseid Azure andmekeskuste ja taristu oma ettevõttes. See pakub usaldusväärne suvand suurte andmehulkade ülekandmiseks. Lisateavet leiate [Azure'i ExpressRoute dokumentatsioonist](../expressroute/expressroute-introduction.md).


* **Andmete üleslaadimine "Ühenduseta"**. Kui Azure'i ExpressRoute abil ei ole võimalik mingil põhjusel, saate saata oma andmetega, mis on Azure andmekeskuse kõvaketta draivid [Azure impordi/ekspordi teenuse](../storage/storage-import-export-service.md) . Teie andmed on esmalt Azure salvestusruumi plekid üles laadida. Seejärel saate [Azure'i andmed Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) või [AdlCopy tööriista](data-lake-store-copy-data-azure-storage-blob.md) Azure salvestusruumi plekid andmete kopeerimine Lake andmesalve.

    >[AZURE.NOTE] Kuigi abil teenuse impordi/ekspordi faili suurused kettale, mille saadate Azure andmekeskuse ei tohi olla suurem kui 200 GB.


## <a name="process-data-stored-in-data-lake-store"></a>Lake andmesalve säilitatavate andmete töötlemiseks

Kui andmed on saadaval Lake andmesalve käivitada analüüsi toetatud suur andmete rakenduste andmeid. Praegu saate Windows Azure Hdinsightiga ja Azure andmeanalüüsi Lake andmete analüüsi tööd käitamist Lake andmesalve talletatud andmed. 

![Lake andmesalve andmete analüüsimine] (./media/data-lake-store-data-scenarios/analyze-data.png "Lake andmesalve andmete analüüsimine")

Saate vaadata järgmistes näidetes.

* [Luua mõne Hdinsightiga kobar Lake andmesalve nimega salvestusruum](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Andmete allalaadimine andmete Lake poest

Võite ka alla laadida või teisaldage andmed Azure'i andmesalve Lake stsenaariumid näiteks:

* Teisaldage andmed muude hoidlate oma olemasolevate andmete töötlemise gaasijuhtmetega liides. Näiteks soovite teisaldada andmeid andmesalve Lake Azure'i SQL-andmebaasi või asutusesisese SQL serveriga.
* Andmete allalaadimine IDE keskkonnas töötlemise ajal rakenduse prototüüpide kohalikku arvutisse.

![Sealt andmeid andmesalve Lake] (./media/data-lake-store-data-scenarios/egress-data.png "Sealt andmeid andmesalve Lake")

Sellisel juhul saate teha ühte järgmistest suvanditest.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure'i andmed Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Saate järgmistest meetoditest kirjutada oma skripti/taotluse andmete andmete Lake poest alla laadida.

* [Azure'i platvormidel CLI](data-lake-store-get-started-cli.md)
* [Azure'i PowerShelli](data-lake-store-get-started-powershell.md)
* [Azure'i andmesalve Lake .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Lake andmesalve andmete visualiseerimine

Mixi teenuste abil saate luua visuaalne anda Lake andmesalve talletatud andmed.

![Visualiseeri andmete andmesalve Lake] (./media/data-lake-store-data-scenarios/visualize-data.png "Visualiseeri andmete andmesalve Lake")

* [Azure'i andmed Factory andmesalve Lake SQL Azure'i andmebaas andmete teisaldamiseks](../data-factory/data-factory-data-movement-activities.md#supported-data-stores) abil saate alustada
* Pärast seda, saate [integreerida Power BI koos SQL Azure'i andmebaas](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) luua visuaalse ettekujutuse.
