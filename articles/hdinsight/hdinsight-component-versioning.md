<properties
    pageTitle="Mis on saadaval koos mõne Hdinsightiga kobar erinevate osade? | Microsoft Azure'i"
    description="Hdinsightiga toetab mitut käivituva Hadoopi kobar komponendid ja versioonid. Vt ka Hadoopi ja HortonWorks andmete platvormi (HDP) jaotuse versioonides toetatud."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="what-are-the-different-hadoop-components-available-with-hdinsight"></a>Mis on erinevate Hadoopi komponendid Hdinsightiga saadaval?

Lisateave pakutud Hdinsightiga kui ka muu Hadoopi komponendid kaasas Hdinsightiga versioonide muu teenuse tase.

## <a name="hdinsight-standard-and-hdinsight-premium"></a>Hdinsightiga Standard- ja Hdinsightiga Premium

Azure Hdinsightiga pakub suur andmete pilve pakkumisi kahte kategooriasse: **Standard** ja **Premium**. Jaotis järgmises tabelis on loetletud funktsioone, mis on saadaval **ainult Premium osana**. Funktsioonid, mis pole konkreetselt nimetatakse tabelis siin on saadaval Standard osana.

>[AZURE.NOTE] Hdinsightiga lisatasu pakub praegu saadaval ainult Linuxi kogumite eelvaade ja.

| Funktsioon Hdinsightiga Premium | Kirjeldus |
|--------------|---------------|
| Domeeni ühendatud Hdinsightiga kogumite       | Ettevõtte tasemel turve Azure Active Directory (AAD) domeenide Hdinsightiga kogumite liita. Saate konfigureerida töötajate loendi oma ettevõttelt, kes saab autentida Azure Active Directory Hdinsightiga kobar sisselogimiseks kaudu. Ettevõtte administraator saab konfigureerida ka rollipõhise juurdepääsu reguleerimine taru turvalisuse abil [Apache Ranger](http://hortonworks.com/apache/ranger/), seega juurdepääsu piiramine andmed ainult siis, kui vaja. Lõpetuseks, saate admin audit andmed töötajate ja juurdepääsu juhtimine poliitikad, seega saavutada kõrge oma ettevõtte ressursside haldamine tehtud muudatused. Lisateavet leiate teemast [konfigureerimine domeeni ühendatud Hdinsightiga kogumite](hdinsight-domain-joined-configure).

### <a name="cluster-types-supported-for-premium"></a>Toetatud Premiumi kobar tüübid

Järgmises tabelis on loetletud Hdinsightiga kobar tüüp ja Premium tugi maatriks.

| Kobar tüüp | Standard  | Premium |
|--------------|---------------|--------------|
| Hadoopi       | Jah           | Jah (ainult Hdinsightiga 3.5)         |
| Säde        | Jah           | Ei          |
| HBase        | Jah           | Ei           |
| Torm        | Jah           | Ei           |
| Interaktiivne taru (eelvaade) | Jah | Ei |
| R Server (eelvaade) | Jah | Ei |

Selles tabelis värskendatakse, kui mitu kobar kaasatakse Hdinsightiga Premium.

### <a name="pricing-and-sla"></a>Hinnad ja SLA

Hinnad ja SLA Hdinsightiga Premiumi Lisateavet [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Hadoopi komponendid, mis on saadaval eri Hdinsightiga versioonidega

Azure Hdinsightiga toetab mitut Hadoopi kobar versiooni, igal ajal juurutatud. Iga versioon valik loob konkreetse versiooni Hortonworks andmete platvormi (HDP) jaotuse ja komponentide, mis sisalduvad selle jaotuse. Järgmises tabelis on loetletud seostatud Hdinsightiga kobar versioonide komponent versioonid. Pange tähele, et kasutada Windows Azure Hdinsightiga kobar vaikeversiooniks praegu 3.4 ja seisuga 09/14/2016, võttes aluseks HDP 2.4.

> [AZURE.NOTE] Teenuse vaikeversiooniks võib ette teatamata muuta. Soovitame määrata versioon, kui loote kogumite, kasutades PowerShelli .NET SDK/Azure ja Azure CLI, kui teil on versiooni sõltuvus. 

Komponent|Hdinsightiga versiooni 3.5 | Hdinsightiga versioon 3.4 (vaikeväärtus) | Hdinsightiga versioon 3.3 | Hdinsightiga versioon 3,2 |Hdinsightiga versioon 3,1 |Hdinsightiga versiooni 3.0|
---|---|---|---|---|---|---
Hortonworks andmete platvormi|2,5|2.4|2.3|2.2|2.1.7|2.0|
Apache Hadoop ja LÕNG|2.7.3|2.7.1|2.7.1|2.6.0|2.4.0|2.2.0|
Apache Tez|0.7.0|0.7.0|0.7.0 | 0.5.2|0.4.0||
Apache siga|0.16.0|0.15.0|0.15.0|0.14.0|0.12.1|0.12.0|
Apache taru & HCatalog|1.2.1.2.5|1.2.1|1.2.1|0.14.0|0.13.1|0.12.0|
Apache HBase |1.1.2|1.1.2|1.1.1|0.98.4|0.98.0||
Apache Sqoop|1.4.6|1.4.6|1.4.6|1.4.5|1.4.4|1.4.4|1.4.3
Apache Oozie|4.2.0|4.2.0|4.2.0|4.1.0|4.0.0|4.0.0|
Apache Zookeeper|3.4.6|3.4.6|3.4.6|3.4.6|3.4.5|3.4.5|
Apache torm|1.0.1|0.10.0|0.10.0|0.9.3|0.9.1||
Apache Mahout|0.9.0+|0.9.0+|0.9.0+|0.9.0|0.9.0||
Apache Phoenix|4.7.0|4.4.0|4.4.0|4.2.0|4.0.0.2.1.7.0-2162||
Apache Spark|1.6.2 + 2.0 (ainult Linux)|1.6.0 (ainult Linux)|1.5.2 (Linux ainult/eksperimentaalsed järk)|1.3.1 (ainult Windows)|||

**Praeguse komponent versiooniteave**

Seostatud Hdinsightiga kobar versioonide komponent versioonid võivad vahetada tulevikus värskenduste Hdinsightiga. Üks võimalus saadaval komponendid ja kontrollida, millist versiooni kasutatakse klaster on kasutada Ambari REST API-ga. Käsk **GetComponentInformation** saab hankida teavet teenuste osa. Lisateavet leiate teemast [Ambari dokumentatsiooni][ambari-docs]. Teine võimalus saada see teave on kaugtöölaua klaster sisse logida ja sisu on "C:\apps\dist\" directory otse.


**Väljalaskemärkmed**

Teemast [Hdinsightiga vabastage märkmete](hdinsight-release-notes.md) väljalaskemärkmed täiendavad Hdinsightiga uusimad versioonid.


## <a name="supported-hdinsight-versions"></a>Toetatud Hdinsightiga versioonid
Järgmises tabelis on loetletud Hdinsightiga on praegu saadaval versioonides, vastavate Hortonworks andmete platvormi versioonid, et nad ja nende Väljalaske kuupäevad. Kui teada, tugi aegumine ja taunimine kuupäevad on olemas. Võtke arvesse järgmist.

* Vaikimisi on väga kättesaadav kogumite koos kahe pea sõlme juurutatud Hdinsightiga 2.1 ja üle. Nad ei ole saadaval Hdinsightiga 1,6 kogumite.
* Kui soovitud tugi on lõppenud konkreetse versiooni jaoks, ei pruugi olla saadaval Azure portaali kaudu. Järgmine tabel näitab, milline on saadaval Azure klassikaline portaalis. Kobar versioonid endiselt saadaval abil soovitud `Version` käsk Windows PowerShelli [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) ja selle taunimine kuupäevani .NET SDK parameeter.

Hdinsightiga versioon|HDP versioon|VM OS|Kõrge-saadavus|Väljaandmisaeg|Azure'i portaalis saadaval|Aegumiskuupäev tugi|Taunimine kuupäev
---|---|---|---|---|---|---|---
HDI 3.5 | HDP 2,5| Ubuntu 16 | Jah | 9/30/2016 | Jah | 
HDI 3.4|HDP 2.4|Ubuntu 14.0.4 LTS|Jah|03/29/2016|Jah|12/29/2016|2018/1/9|
HDI 3.3|HDP 2.3|Ubuntu 14.0.4 LTS või Windows Serveri 2012R2|Jah|12/02/2015|Jah|27-06-2016|07/31/2017
HDI 3,2|HDP 2.2|Ubuntu 12.04 LTS või Windows Serveri 2012R2|Jah|18-2-2015|Jah|3/1/2016|04/01/2017
HDI 3,1|HDP 2.1|Windows Serveri 2012R2|Jah|6/24/2014|Ei|18-05-2015|30-06-2016
HDI 3.0|HDP 2.0|Windows Serveri 2012R2|Jah|02/11/2014|Ei|09/17/2014|30-06-2015
HDI 2.1|HDP 1.3|Windows Serveri 2012R2|Jah|28-10-2013|Ei|05/12/2014|05/31/2015
HDI 1,6|HDP 1.1||Ei|28-10-2013|Ei|04/26/2014|05/31/2015

**Vaikelokaadisättest kogumite kasutuselevõtt**

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Teenusetaseme lepingu Hdinsightiga kobar versioonid

SLA määratletakse "Tugi aken". Tugiteenuste akna viitab aeg, mis on Hdinsightiga kobar versioon toetab Microsofti klienditeenindus ja tugi. Mõne Hdinsightiga klaster on väljaspool tugi akna kui selle versiooni on **Tugi aegumiskuupäeva** viimase praeguse kuupäeva. Toetatud Hdinsightiga kobar versioonide loendi leiate ülaltoodud tabelis. Tugiteenuste aegumiskuupäeva antud Hdinsightiga version (kui saadaval on uuem versioon X + 1) X arvutatakse hiljem kohta:  

- Valem 1: Lisada 180 päeva kuupäeva Hdinsightiga kobar versioon ilmus X.
- Valem 2: Lisada 90 päeva kuupäeva Hdinsightiga kobar versioon X + 1 (X pärast hilisem versioon) on kättesaadav portaalis.

**Taunimine kuupäev** on kuupäev, pärast mida kobar versiooni ei saa luua Hdinsightiga.

> [AZURE.NOTE] Azure'i Külastajate OS pere 4, mis kasutab 64-bitine versioon Windows Server 2012 R2 ja toetab .NET Framework 4.0, 4.5, 4.5.1 ja 4.5.2 käivitada Windowsi-põhiste Hdinsightiga kobar (sh versioon 2.1, 3.0, 3,1, 3,2 ja 3.3).

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks vabastage märkmete seostatud Hdinsightiga versioonid##

* Hdinsightiga kobar versioon 3.4 kasutab Hadoopi jaotuse, [Hortonworks andmete platvormi 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)põhjal. See on **vaikimisi** Hadoopi kobar loodud portaali kasutamisel.



* Hdinsightiga kobar versioon 3.3 kasutab Hadoopi jaotuse, [Hortonworks andmete platvormi 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)põhjal.
    * Apache Storm väljalaskemärkmed on saadaval [siin](https://storm.apache.org/2015/11/05/storm0100-released.html).
    * Apache taru väljalaskemärkmed on saadaval [siin](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).

* Hdinsightiga kobar versioon 3,2 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 2.2]põhineva[hdp-2-2].  

    * Väljalaskemärkmed teatud Apache komponentide - [taru 0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450) [0,14 siga](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [LÕNG 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [levinud](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).


* Hdinsightiga kobar version 3,1 kasutab Hadoopi jaotuse, mis põhineb [Hortonworks andmete platvormi 2.1.7][hdp-2-1-7]. Enne 11/7/2014 põhinevad [Hortonworks andmete platvormi 2.1.1]loodud Hdinsightiga 3,1 kogumite[hdp-2-1-1].

* Hdinsightiga kobar versiooni 3.0 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 2.0]põhineva[hdp-2-0-8].

* Hdinsightiga kobar versioon 2.1 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 1.3]põhineva[hdp-1-3-0].

* Hdinsightiga kobar versioon 1,6 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 1.1]põhineva[hdp-1-1-0].


[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
