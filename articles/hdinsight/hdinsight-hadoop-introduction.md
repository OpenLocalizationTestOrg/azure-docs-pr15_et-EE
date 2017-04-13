 <properties
    pageTitle="Sissejuhatus Hadoop - mis on Hadoopi Hdinsightiga? | Microsoft Azure'i"
    description="Saada Sissejuhatus Hadoopi, jaotatud suur andmete töötlemise ja analüüsi ja komponentide Hadoopi ökosüsteemi Hdinsightiga pilve."
    keywords="suur Andmeanalüüs Hadoopi, mis on Hadoopi, hadoop tehnoloogia virnas, hadoop ökosüsteemi tutvustus"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Sissejuhatus Hadoopi ökosüsteemi kohta Windows Azure Hdinsightiga

 Selles artiklis on toodud Windows Azure Hdinsightiga, selle ökosüsteemi ja suur andmete Hadoopi tutvustus. Lisateavet Hadoopi komponendid, levinud terminoloogia ja stsenaariumid suur Andmeanalüüs.

## <a name="what-is-hadoop-on-hdinsight"></a>Mis on Hadoopi Hdinsightiga?

Hadoopi viitab ökosüsteemi avatud lähtekoodi tarkvara, mis on raamistik hajutatud, talletamine ja analüüsi suurte andmekogumite kogumite arvutite kohta. Azure Hdinsightiga Hadoopi komponendid **Hortonworks andmete platvormi (HDP)** jaotuse kättesaadavaks pilves, ja kasutab ja usaldusväärsus ja kättesaadavus hallatavate kogumite sätted.  

Apache Hadoop oli algse avatud lähtekoodi projekti suur andmete töötlemiseks. Järgmine on seotud tarkvara ja Utiliidid käsitletakse Hadoopi tehnoloogia virnas, sh Apache taru, Apache HBase, Apache Spark ja paljud teised osana. Üksikasjad leiate [Ülevaade Hadoopi ökosüsteemi Hdinsightiga sisse](#overview) .

## <a name="what-is-big-data"></a>Mis on suur andmeid?

Suur andmete kirjeldatakse digitaalse teabe, teksti Twitter kanali sensoriteabele tööstusliku seadmetest teavet kliendi sirvimise ja ostud veebisaidil, mis tahes hulk. Suur andmeid saab ajaloo (st salvestatud andmeid) või reaalajas (st voona otse allikas). Suur andmete kogutud-suurenevad mahtu, järjest suurem kiirused, ja seal on laiendamine erinevaid vorminguid.

Suur andmete luureteabena või ülevaate, peate asjakohaste andmete kogumise ja õige küsimusi. Samuti peate veenduma, andmed on kättesaadav, puhastada, analüüsida ja siis esitatakse kasulikul viisil. Mis on, kui suur andmete analüüsi Hadoopi rakenduses Hdinsightiga aitab.

## <a name="overview"></a>Hadoopi ökosüsteemi rakenduses Hdinsightiga ülevaade

Hdinsightiga on pilv jaotuse Apache Hadoop kiiresti laieneb tehnoloogia virnas suur andmeanalüüsi Microsoft Azure'i. See sisaldab rakendusi Apache Spark HBase Storm, siga, taru, Sqoop, Oozie, Ambari ja jne. Samuti integreerub Hdinsightiga (BI) ärianalüüsitööriistade nagu Power BI, Exceli, SQL Server Analysis Services ja SQL serveri aruandlusteenuste.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoopi, HBase, säde, torm ja kohandatud kogumite

Hdinsightiga pakub kobar konfiguratsioone Apache Hadoop, säde, HBase või torm. Või saate [kohandada kogumite skripti toimingute](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoopi**: pakub [HDFS](#hdfs)ja lihtsa [MapReduce](#mapreduce) programmeerimise mudeli andmete paralleelselt töötlemine ja usaldusväärsed andmed.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: paralleelne töötlemine raamistiku, mis toetab-mälu töötlemise suur Andmeanalüüs rakenduste, jõudlust parandada Spark töötab SQL-i, streaming andmed ja masina õ. Vt [Ülevaade: mis on Apache Spark sisse Hdinsightiga?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: NoSQL andmebaasi tugineb Hadoopi, mis pakub muutmälu ja tugev järjepidevuse suurte andmehulkade struktureerimata ja poolstruktuur - ridade potentsiaalselt miljardit korda miljoneid veerud. [HBase Hdinsightiga kohta](hdinsight-hbase-overview.md)ülevaate.

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>**: jaotatud, reaalajas arvutus süsteemi kiire suurte voole andmete töötlemiseks. Torm pakutakse hallatavate kobar Hdinsightiga sisse. Vt [analüüsi reaalajas andurite andmeid torm ja Hadoopi abil](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Näide kohandamine skriptide

Skripti toimingud on skriptide kobar ettevalmistamise ajal käivitada, mis saab installida lisakomponentide klaster. Linux-põhine kogumite, on need Bash skripte.

Järgmine näide skripte on esitatud Hdinsightiga meeskond.

* [Tooni](hdinsight-hadoop-hue-linux.md): suhelda klaster veebirakenduste komplekt. Ainult Linux kogumite.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): mudeli seoseid asjade või töötlemise graafik.

* [R](hdinsight-hadoop-r-scripts-linux.md): avatud lähtekoodi keele- ja keskkonna statistika arvutuste kasutatud seadme õ.

* [Solri](hdinsight-hadoop-solr-install-linux.md): enterprise-skaala otsingu platvormi, mis võimaldab otsingu andmete põhjal.

Oma skripti tegevusi Lisateavet [skripti toimingu arendamiseni Hdinsightiga](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Mis on Hadoopi komponendid ja Utiliidid?

Järgmised komponendid ja Utiliidid on kaasatud Hdinsightiga kogumite.

* **[Ambari](#ambari)**: klaster ettevalmistamise, haldus, jälgimine ja Utiliidid.

* **[Avro](#avro)** (Microsoft .net-i teegi Avro): andmete sariväljaanne Microsoft .net-i keskkonnas.

* **[Taru & HCatalog](#hive)**: liigendatud (SQL) – näiteks päringud ja tabeli ja salvestusruumi haldamine kiht.

* **[Mahout](#mahout)**: scalable masina Õppekeskuse rakendused.

* **[MapReduce](#mapreduce)**: Pärandrakenduse raamistik Hadoopi jaotatud töötlemine-ja ressursihalduse. Lugege teemat [LÕNG](#yarn), edasi-genereerimine ressursside raames.

* **[Oozie](#oozie)**: töövoohaldus.

* **[Phoenix](#phoenix)**: relatsiooniandmebaasist kiht HBase üle.

* **[Siga](#pig)**: lihtsam scripting MapReduce teisenduste jaoks.

* **[Sqoop](#sqoop)**: andmete importimine ja eksportimine.

* **[Tez](#tez)**: võimaldab andmete mahukat protsesside käivitamiseks tõhus skaala.

* **[LÕNG](#yarn)**: osa Hadoopi core teek ja tulevased MapReduce tarkvara raames.

* **[ZooKeeper](#zookeeper)**: koordineerituna protsesside jaotatud spikritest.

> [AZURE.NOTE] Teavet teatud komponendid ja versiooniteabe leiate teemast [Hadoopi komponendid, Versioonimine, ja teenuse pakkumised Hdinsightiga][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari on ettevalmistamise, juhtimiseks ja Apache Hadoop kogumite. See sisaldab on intuitiivne saidikogumi tehtemärk tööriistade ja komplekti API-d, mis peitmine keerukus Hadoopi, lihtsustamine kogumite toimimist. Kogumite pakuvad nii Linuxi-põhiste Hdinsightiga soovitud Ambari web Kasutajaliidese ja Ambari REST API, küll Windowsi-põhiste kogumite REST API alamhulga. Ambari vaadete Hdinsightiga kogumite luba lisandmooduli Kasutajaliidese võimalused.

Vt [haldamine Hdinsightiga kogumite abil Ambari](hdinsight-hadoop-manage-ambari.md) (ainult Linux), [kuvari Hadoopi kogumite Hdinsightiga Ambari API abil sisse](hdinsight-monitor-use-ambari-api.md)ja <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API viide</a>.

### <a name="avro"></a>Avro (Microsoft .NET teegi jaoks Avro)

Microsoft .NET teegi jaoks Avro rakendab Apache Avro tihendatud binaarandmeid andmevahetuse vorming sariväljaanne Microsoft .net-i keskkonnas. <a target="_blank" href="http://www.json.org/">JavaScript Object märke (JSON)</a> kasutab määratleda puhvrina keele interoperability keele diagnostika skeem, ühe keelega seeriasertide tähendust andmeid saab lugeda teise. Vormingu üksikasjalikku teavet leiate selle < siht = _ "tühi" href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro määratlus</a>.
Vormingu Avro faile, mis toetab jaotatud MapReduce programmeerimise mudel. Failid on "splittable" tähendab, et saate otsida mis tahes faili punktis ja lugemise alustamine kindla plokk. Kuidas, lugege teemast [Serialize andmete jaoks Avro teegiga Microsoft .net-i](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Hadoopi jaotatud faili süsteemi (HDFS) on jaotatud failisüsteemi, mis MapReduce ja LÕNG, Hadoop ökosüsteemi keskmes on. HDFS on standardne failisüsteemi Hadoopi kogumite Hdinsightiga kohta.

### <a name="hive"></a>Taru & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache taru</a> on andmete ladu tarkvara tugineb Hadoopi, mis võimaldab teil päringu ja suurte andmekogumite jaotatud salvestusruumi haldamine SQL-nagu keele nimega HiveQL abil. Taru siga, nagu on ka võtmiseks MapReduce peal. Käivitamisel taru tõlgib päringute MapReduce töökohtade sarja. Põhimõtteliselt skannige relatsiooniandmebaasist haldamise süsteem siga, ja seetõttu on veel Liigendatud andmete jaoks asjakohane. Struktureerimata andmed, siga on parem valik. Vaadake [kasutada taru koos Hadoopi Hdinsightiga sisse](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> on tabel ja salvestusruumi haldamine kiht Hadoopi, mis esitab relatsiooniline ülevaate andmete kasutajate jaoks. HCatalog, saate lugeda ja kirjutada failid, mis tahes vormingus, mille saate kirjutada taru SerDe (serializer reafiltrite).

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> on scalable teegi Õppekeskuse algoritmide kohta, mis töötavad Hadoopi kohapeal. Seadme õ rakenduste kasutamise põhimõtted statistika, Õpetage süsteemide teavet andmete põhjal ja kasutada eelmiste tulemuste määratlemiseks tulevaste käitumine. Lugege teemat [Genereeri filmi soovitused, kasutades Mahout Hadoopi kohta](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce on pärand tarkvara raamistik Hadoopi kirjutamiseks partii protsessi suurte andmekogumite paralleelselt rakendusi. MapReduce töö jagab suurte andmekogumite ja korraldab andmete töötlemiseks võti ja väärtuse paarideks.

[LÕNG](#yarn) on Hadoopi edasi-genereerimine ressursi juhataja ja rakenduse raames ja nimetatakse MapReduce 2.0. MapReduce käitatakse LÕNGA kohta.

MapReduce kohta leiate lisateavet teemast <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> Hadoopi viki sisse.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> on Hadoopi töö haldava töövoo koordineerituna süsteem. See on integreeritud Hadoopi virnas ja toetab Hadoopi töö MapReduce, siga, taru ja Sqoop. See saab tööde teatud süsteem, nagu Java programmi või shell skriptide. Lugege teemat [kasutamine Oozie Hadoopi abil](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> on relatsiooniandmebaasist kiht HBase üle. Phoenix sisaldab JDBC draiveri, mis võimaldab kasutajatel päringu ja SQL-i tabelite otse haldamine. Phoenix tõlgib päringud ja muud laused kohalikke NoSQL API kõned – asemel abil MapReduce – võimaldades kiirem rakendusi NoSQL poed. Lugege teemat [kasutamine Apache Phoenix ja orav HBase kogumite abil](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Siga
<a  target="_blank" href="http://pig.apache.org/">Apache siga</a> on üksikasjalik platvorm, mis võimaldab teil abil saate teha keerukate MapReduce teisendused on väga suurte andmekogumite siga Ladina nimega lihtne skript keel. Siga tähendab siga Ladina skriptide nii, et nad käivitate Hadoopi sees. Saate luua kasutaja määratletud funktsioonid (UDF) siga Ladina laiendada. Lugege teemat [kasutamine siga Hadoopi](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> on tööriist, et edastamine hulgiredigeerimiseks Hadoopi ja relatsiooniandmebaasid sellise SQL andmete või muu Liigendatud andmete salvestab, võimalikult tõhusalt. Lugege teemat [kasutamine Sqoop Hadoopi abil](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> on rakenduse raamistik tugineb Hadoopi LÕNG, mis käivitab keerukas, atsüklilised graafikud üldine andmetöötlus. See on võimas ja paindlik järgnev ülesanne MapReduce raamistiku, mis võimaldab andmete mahukat protsessid, nt taru käivitamiseks tõhusamalt skaala. Lugege teemat ["Kasuta Apache Tez jaoks paranenud jõudlus" Kasuta taru ja HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>LÕNG
Apache LÕNG on järgmine tekkimist MapReduce (MapReduce 2.0 või MRv2) ja toetab andmete töötlemise stsenaariumid MapReduce Pakktöötlus suurem skaleeritavus ja reaalajas töötlemine väljaspool. LÕNG pakub ressursside haldamine ja jaotatud rakendus raames. MapReduce käitatakse LÕNGA kohta.

LÕNGA kohta leiate teemast <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (LÕNG)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinaadid protsesside suurte jaotatud spikritest ühiskasutuses hierarhilise nimeruumi andmed registrite (znodes) abil. Znodes sisestada pisut metaandmete koordineerimiseks protsesside vajalik teave: olek, asukoht, konfigureerimine ja jne.

## <a name="programming-languages-on-hdinsight"></a>Programmeerimise keelte Hdinsightiga

Hdinsightiga kogumite--Hadoopi, HBase, Storm ja säde kogumite--toetavad arvu programmeerimise keeled, kuid mõned pole installitud vaikimisi. Teekide puhul kasutage moodulid või pole installitud vaikimisi pakettide skripti toimingut komponendi installimiseks. Lugege teemat [skripti toimingu arendamiseni Hdinsightiga](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Vaikimisi programmeerimisega keelte tugi

Vaikimisi Hdinsightiga kogumite tugi:

* Java

* Python

Täiendavad keeled saab installida skripti toimingute kasutamine: [skripti toimingu arendamiseni Hdinsightiga](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java virtuaalse masina (töötab) keeled

Mitmes keeles peale Java saab käivitada kasutades Java virtuaalse masina (töötab); töötab teatud keelte nõuda lisakomponentide klaster installitud.

Need töötab tähestikul põhinevaid keeli toetab Hdinsightiga kogumite:

* Clojure

* Jython (Python Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoopi kohased keeled

Hdinsightiga kogumite on toetatud järgmised keeled, mis on omased Hadoopi ökosüsteemi:

* Siga Ladina siga projektide jaoks

* HiveQL taru töökohtade ja SparkSQL


## <a name="advantage"></a>Hadoopi pilveteenuste eelised

Azure'i pilveteenuste ökosüsteemi osana Hadoopi rakenduses Hdinsightiga pakub mitmeid eeliseid, nende vahel:

* Hadoopi kogumite automaatse ettevalmistamine. Hdinsightiga kogumite on märksa lihtsam kui käsitsi konfigureerimise Hadoopi kogumite loomine. Lisateavet leiate teemast [sätte Hadoopi kogumite Hdinsightiga sisse](hdinsight-hadoop-provision-linux-clusters.md).

* Tehnika Hadoopi komponendid. Lisateavet leiate teemast [Hadoopi komponendid, Versioonimine, ja teenuse pakkumised Hdinsightiga][component-versioning].

* Kõrge kättesaadavus ja usaldusväärsus kogumite. Üksikasjad leiate [kättesaadavus ja usaldusväärsus Hadoopi kogumite Hdinsightiga sisse](hdinsight-high-availability-linux.md) .

* Tõhus ja ökonoomne andmete salvestusruumi Azure'i bloobimälu, Hadoop-ga ühilduva suvand. Üksikasjad leiate [kasutamine Azure'i bloobimälu Hadoopi Hdinsightiga sisse](hdinsight-hadoop-use-blob-storage.md) .

* Muud Azure teenused, sealhulgas [veebirakenduste](https://azure.microsoft.com/documentation/services/app-service/web/) ja [SQL-andmebaasi](https://azure.microsoft.com/documentation/services/sql-database/)integreerimine.

* Täiendavad VM suurused ja tööta Hdinsightiga kogumite tüübid. Vt [Hadoopi komponendid, Versioonimine, ja teenuse pakkumised Hdinsightiga] [ component-versioning] üksikasju.

* Klaster skaleerimist. Kobar skaleerimist võimaldab teil muuta sõlmed töötava Hdinsightiga kobar arv ilma kustutada või selle uuesti luua.

* Virtuaalse võrgu tugi. Hdinsightiga kogumite saab kasutada Azure virtuaalse võrguga toetamiseks cloud ressursid või hübriidjuurutuse stsenaariumi, mis link cloud ressursid need oma andmekeskuses.

* Madal kirje maksumus. Käivitage [tasuta prooviversioon](https://azure.microsoft.com/free/)või küsige [Hdinsightiga hinnakirjad üksikasjad](https://azure.microsoft.com/pricing/details/hdinsight/).

Lugeda Lisateavet eeliste kohta rakenduses Hdinsightiga Hadoopi, vaadake [Azure funktsioonid lehe Hdinsightiga][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>Hdinsightiga Standard- ja Hdinsightiga Premium

Hdinsightiga pakub suur andmete pilve pakkumisi Standard ja Premium kahe kategooria. Hdinsightiga Standard annab enterprise-skaala kobar, mida ettevõtted saavad kasutada nende suur andmed töökoormus käivitamiseks. Hdinsightiga lisatasu, mis põhineb ning annab analüütiliste Täpsemad ja turvalisus on Hdinsightiga kobar. Lisateavet leiate teemast [Azure Hdinsightiga Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Ressursside kohta rohkem teada suur Andmeanalüüs, Hadoop ja Hdinsightiga

Sissejuhatus Hadoopi pilves ja suur Andmeanalüüs allpool ressurssidega koostamiseks.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoopi dokumentatsioonist Hdinsightiga

* [Hdinsightiga dokumentatsiooni](https://azure.microsoft.com/documentation/services/hdinsight/): dokumentatsiooni leht Hadoopi Windows Azure Hdinsightiga artikleid, videoid ja ressursside lingid.

* [Alustamine rakenduses Hdinsightiga Hadoopi](hdinsight-hadoop-linux-tutorial-get-started.md): kiire alustada õpetuse ettevalmistamise Hdinsightiga Hadoopi kogumite ja käitamise valimi taru päringud.

* [Alustamine rakenduses Hdinsightiga säde](hdinsight-apache-spark-jupyter-spark-sql.md): kiire alustada õpetuse säde kobar loomise ja käitamise interaktiivsed säde SQL-päringud.

* [Kasutage R Server Hdinsightile](hdinsight-hadoop-r-server-get-started.md): R Server Hdinsightiga Premiumi kasutamise alustamine.

* [Sätte Hdinsightiga kogumite](hdinsight-hadoop-provision-linux-clusters.md): saate teada, kuidas ette valmistada mõne Hdinsightiga Hadoopi kobar Azure portaali, Azure'i CLI või Azure PowerShelli kaudu.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Lisateave Apache Hadoop tarkvara teegi raamistiku, mis võimaldab suurte andmekogumite jaotatud töötlemiseks kogumite arvutite üle.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Lisateave arhitektuur ja kujundus Hadoopi jaotatud failisüsteemi, esmased süsteemi Hadoopi rakendused.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce õpetus</a>: Lisateave kirjutamiseks kiiresti töötlemine suure hulga andmete paralleelselt suurtes rühmades Arvuta sõlmed Hadoopi rakenduste programmeerimise raames.


### <a name="microsoft-business-intelligence"></a>Microsoft business intelligence

Tuttavate (BI) ärianalüüsitööriistade – nt Exceli, PowerPivot, SQL Server Analysis Services ja SQL serveri aruandlusteenuste - tuua, analüüsida ja aruande andmete integreeritud Hdinsightiga Power Query lisandmooduli või Microsoft taru ODBC-draiver abil.

Andmeanalüüsis suur aitavad need Ärianalüüsi tööriistade kohta.

* [Hadoopi Power Query Exceli ühendamine](hdinsight-connect-excel-power-query.md): saate teada, kuidas ühendada Exceli Azure Storage konto, mis talletab, kasutades Microsoft Power Query for Excel klaster Hdinsightiga seotud andmete. Windowsi töökoha nõutav. Windowsi või Linuxi põhise kobar töötab.

* [Exceli ühendamine Microsoft taru ODBC draiver Hadoopi](hdinsight-connect-excel-hive-odbc-driver.md): saate teada, kuidas andmete importimine Microsoft taru ODBC-draiver Hdinsightiga. Windowsi töökoha nõutav. Windowsi või Linuxi põhise kobar töötab.

* [Microsofti pilveteenuste platvorm](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Lisateavet Power BI for Office 365, SQL serveri prooviversiooni allalaadimiseks ja SharePoint Server 2013 ja SQL serveri BI häälestamine.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL serveri aruandlusteenused](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
