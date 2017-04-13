<properties
    pageTitle="Siit saate teada, mis on taru ja kuidas kasutada HiveQL | Microsoft Azure'i"
    description="Teavet Apache taru ja kuidas seda kasutada koos Hadoopi Hdinsightiga sisse. Saate valida, kuidas oma taru töö ja analüüsida Apache log4j näidisfaili HiveQL abil."
    keywords="hiveql, mis on taru"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Kasutage taru ja HiveQL Hadoopi sisse Hdinsightiga Apache log4j näidisfaili analüüsimiseks

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Selles õpetuses kuvatakse saate teada, kuidas kasutada Apache taru rakenduses Hadoopi Hdinsightiga ja valida, kuidas oma taru töö käivitamiseks. Samuti saate teada, HiveQL ja kuidas analüüsida Apache log4j näidisfaili kohta.

##<a id="why"></a>Mis on taru ja miks seda kasutada?
[Apache taru](http://hive.apache.org/) on andmete ladu süsteemi Hadoopi, mis võimaldab andmete kokkuvõtet, päringute ja andmete analüüsimine, kasutades HiveQL (sarnane SQL päringukeele). Taru saab kasutada andmete interaktiivseks analüüsimiseks või töötlemise tööd korduvkasutatava paketi loomiseks.

Taru saate projekti struktuuri suuresti struktureerimata andmed. Pärast ülesande määratlemist struktuuri, saate päringu andmeid teadmata Java või MapReduce taru. **HiveQL** (taru query language) võimaldab teil kirjutada laused, mis on sarnane T-SQL-i abil.

Taru mõistab töötamise struktureeritud ja poolstruktuurandmeid andmeid, nt tekstifailidest, kus väljad on piiratud kindlaid märke. Taru toetab samuti keerukas või ebakorrapärase Liigendatud andmete jaoks kohandatud **serializer/deserializers (SerDe)** . Lisateabe saamiseks vaadake, [Kuidas kasutada kohandatud JSON SerDe koos Hdinsightiga](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Kasutaja määratletud funktsioonid (UDF)

Taru saab **kasutaja määratletud funktsioonid (UDF)**kaudu pikendada. Soovitud UDF võimaldab teil rakendada funktsioone või loogika, mis pole kerge vaevaga kujundatud HiveQL. Näitena kasutades taru UDF-ID, vaadake järgmist.

* [Kasutage Java kasutaja määratletud funktsioon koos taru](hdinsight-hadoop-hive-java-udf.md)

* [Kasutades Python taru ja Hdinsightiga siga](hdinsight-python.md)

* [Kasutage C# taru ja Hdinsightiga siga](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Kuidas lisada kohandatud taru UDF Hdinsightiga](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Kohandatud taru UDF näide teisendamiseks taru ajatempli kuupäeva/kellaaja vormingud](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Sisemise tabelite vs välise tabeleid taru

On mõned asjad, mida peate teadma taru sisemine tabeli ja välise tabeli kohta.

- **Tabeli loomine** käsk loob on sisemine tabel. Vaike-ümbrisest peab asuma andmefaili.
- **Tabeli loomine** käsu viib andmefaili /hive/ladu/<TableName> kausta.
- **Välise tabeli loomine** käsk loob mõne välise tabeli. Vaikimisi väljastpoolt võib asuda andmefaili.
- **Välise tabeli loomine** käsu ei liigu andmefaili.
- **Välise tabeli loomine** käsu ei luba kõik kaustad asukohta. See on põhjus, miks muudab õpetuse sample.log faili koopia.

Lisateavet leiate teemast [Hdinsightiga: taru sise- ja välise tabeleid sissejuhatav][cindygross-hive-tables].


##<a id="data"></a>Näidisandmete kasutada Apache log4j faili kohta

Selles näites kasutatakse *log4j* näidisfaili, mis on talletatud veebisaidil **/example/data/sample.log** bloobimälu salvestusruumi ümbrises. Iga log failis koosneb rida väljad, mis sisaldab soovitud `[LOG LEVEL]` välja kuvamiseks tüüp ja raskusaste, näiteks:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Eelmises näites Logi tase on viga.

> [AZURE.NOTE] Saate luua ka log4j faili [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logimine tööriista abil ja seejärel faili üleslaadimine bloobimälu ümbrises. Vaadake [Hdinsightiga andmete üleslaadimine](hdinsight-upload-data.md) olevaid juhiseid. Koos Hdinsightiga Azure'i bloobimälu kasutamise kohta leiate lisateavet teemast [Kasutamine Azure'i bloobimälu Hdinsightiga](hdinsight-hadoop-use-blob-storage.md).

Azure'i bloobimälu, mis kasutab Hdinsightiga failisüsteemi talletatakse näidisandmeid. Hdinsightiga pääsevad plekid **wasb** eesliite abil salvestatud failid. Näiteks sample.log failile juurdepääsu, saate kasutada järgmist süntaksit:

    wasbs:///example/data/sample.log

Kuna Azure'i bloobimälu on vaikimisi salvestusruumi jaoks Hdinsightiga, võite kasutada ka faili, kasutades **/example/data/sample.log** HiveQL kaudu.

> [AZURE.NOTE] Funktsiooni süntaks **wasbs: / / /**, kasutatakse vaikimisi salvestusruumi ümbris Hdinsightiga klaster talletatud failidele juurde pääseda. Kui määrasite täiendavat salvestusruumi kontod, kui teil on ette valmistatud klaster ning soovite nende kontodega salvestatud failidele juurde pääseda, pääsete andmed, määrates container nime ja salvestusruumi konto aadress, näiteks **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Valimi töö: projekti veerud peale eraldatud andmed

Järgmistest HiveQL on projekti veerud peale eraldatud andmeid, mis on talletatud on **wasbs: / / / / näidisandmetega** directory:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Eelmises näites HiveQL laused järgmisi toiminguid:

* __Määrake hive.execution.engine=tez;__: määrab Tez kasutada mootori täitmise. MapReduce Tez asemel saate sisestada päringu jõudluse suurenemine. Tez kohta leiate lisateavet jaotisest [Kasutamine Apache Tez jaoks paranenud jõudlus](#usetez) .

    > [AZURE.NOTE] See on ainult nõutav, kui kasutate Windowsi-põhiste Hdinsightiga kobar; Tez on vaikimisi täitmise mootor Linux-põhine Hdinsightiga jaoks.

* **Tabeli LANGEV**: kustutab tabeli ja andmefaili, kui tabel on juba olemas.
* **Välise tabeli loomine**: loob uue **välise** tabeli taru. Välise tabeleid talletada ainult tabeli määratlus taru; andmed on jäetud algse vormingu ja selle algsesse asukohta.
* **Rea vorming**: taru saate teada, kuidas andmed on vormindatud. Sel juhul iga log väljad on eraldatud ruumi.
* **Salvestatud AS TEKSTIFAILIDE asukoht**: ütleb taru, kus andmed on salvestatud (nt/andmete kataloogi) ja tekstina talletatud. Andmeid saab ühte faili või mitme faili kataloogi raames laiali.
* **Valige**: valib kõik read, kus veerus **t4** sisaldab väärtust **[ERROR]**arv. See peaks tagastama väärtus **3** , sest seal on kolm rida, mis sisaldavad seda väärtust.
* **INPUT__FILE__NAME nagu "%.log"** - kirjeldatakse, mida me ainult tagasi andmete failidest, mille lõpus on taru. log. See piirab otsingu sample.log faili, mis sisaldab andmeid, ja tagastab andmed muu näiteks andmefailid, mis ei vasta meil määratletud skeemiga hoiab.

> [AZURE.NOTE] Välise tabeleid tuleks kasutada eeldate alusandmete tuleb värskendada välise andmeallikaga, nt automaatne andmete üleslaadimine või mõne muu MapReduce toimingut, kui soovite alati taru päringute kasutada uusimaid andmeid.
>
> Pukseerimine on Välistabel ei **ole** andmed kustutada, vaid kustutab tabeli määratlus.

Pärast välise tabeli loomise, kasutatakse järgmistest **sisemise** tabeli loomiseks.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Need teatised järgmisi toiminguid:

* **Looge tabel kui pole olemas**: loob tabeli, kui see pole juba olemas. Kuna **väline** märksõna ei kasutata, see on sisemine tabel, mis on talletatud taru andmebaas ja hallatakse täielikult taru.
* **Salvestatud AS ORC**: talletatakse andmeid optimeeritud rea piklikku (ORC) vormingus. See on väga optimeeritud ja tõhusa vorming taru andmete talletamiseks.
* **Lisa kirjutada... Valige**: valib **log4jLogs** tabeli, mis sisaldab **[ERROR]**ja seejärel lisab andmed tabelisse **errorLogs** read.

> [AZURE.NOTE] Erinevalt välise tabeleid, pukseerimine on sisemine tabel kustutatakse ka aluseks olevad andmed.

##<a id="usetez"></a>Paranenud jõudlus Apache Tez kasutamiseks

[Apache Tez](http://tez.apache.org) on raamistiku, mis võimaldab andmete intensiivne rakendusi, nt taru käivitamiseks tõhusamalt skaala. Taru toetab töötavate Tez Hdinsightiga uusimat versiooni. Tez on vaikimisi Linux-põhine Hdinsightiga kogumite jaoks.

> [AZURE.NOTE] Tez on praegu vaikimisi välja lülitatud jaoks Windowsi-põhiste Hdinsightiga kogumite ja see peab olema lubatud. Ära Tez, seatakse taru päringu järgmine väärtus:
>
> ```set hive.execution.engine=tez;```
>
>See saab esitada päringu kohta alusel, pannes selle oma päringu alguses. Saate ka seda, määrates selle konfiguratsiooni klaster loomisel vaikimisi klaster olla. Lisateavet leiate rühmades [ettevalmistamise Hdinsightiga](hdinsight-provision-clusters.md).

[Taru Tez kujundus dokumentidega](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) sisaldavad üksikasjad rakendamist valikuid ja häälestamise konfiguratsioone arvu.

Abi silumine töö parandusfunktsiooni abil Tez, Hdinsightiga pakub järgmised web UIs, mis võimaldavad teil kuvada Tez töökohtade üksikasjad:

* [Windowsi-põhiste Hdinsightiga Tez UI kasutamine](hdinsight-debug-tez-ui.md)

* [Kasutage Linux-põhine Hdinsightiga Ambari Tez vaade](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Saate valida, kuidas HiveQL töö

Hdinsightiga saab käitada HiveQL tööde haldamine, kasutades mitmesuguseid viise. Järgmise tabeli abil saate otsustada, milline viis teile sobib, siis järgige lühiülevaade linki.

| **Kasutage seda** , kui soovite...                                                     | .. .an **interaktiivsed** shell | ... **Pakktöötlus** | .. ei ole see **kobar operatsioonisüsteem** | .. .minu **Kliendi operatsioonisüsteem** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Taru kuvamine](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Mis tahes (brauseri põhine) |
| [Linnulennutee käsk (alates on SSH seansi)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X või Windows        |
| [Taru käsk (alates on SSH seansi)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X või Windows        |
| [Curl](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux või Windows                          | Linux, Unix, Mac OS X või Windows        |
| [Päringu konsooli](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Mis tahes (brauseri põhine)                            |
| [Visual Studio Hdinsightiga tööriistad](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux või Windows                          | Windows                                  |
| [Windows PowerShelli](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux või Windows                          | Windows                                  |
| [Kaugtöölaua](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Töötab taru tööde haldamine Windows Azure Hdinsightiga asutusesisese SQL Server Integration Services abil

SQL serveri Integration Services (SSIS) abil saate käivitada taru töö. Azure'i Paketita ssisoledb pakub järgmised komponendid, mis Hdinsightiga taru töö töötada.


- [Azure Hdinsightiga taru ülesanne][hivetask]
- [Azure'i tellimuse haldur][connectionmanager]


Lugege lisateavet Azure Paketita ssisoledb [siin][ssispack].


##<a id="nextsteps"></a>Järgmised sammud

Nüüd, kui olete õppinud, mis taru on ja kuidas seda kasutada koos Hadoopi Hdinsightiga sisse, järgmiste linkide abil muul viisil töötada Windows Azure Hdinsightiga uurimine.


- [Laadi andmed Hdinsightiga][hdinsight-upload-data]
- [Kasutage siga Hdinsightiga][hdinsight-use-pig]
- [Hdinsightiga Sqoop kasutamine](hdinsight-use-sqoop.md)
- [Hdinsightiga Oozie kasutamine](hdinsight-use-oozie.md)
- [Hdinsightiga MapReduce töö kasutamine][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
