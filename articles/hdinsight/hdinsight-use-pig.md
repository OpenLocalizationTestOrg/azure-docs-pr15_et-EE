<properties
   pageTitle="Hadoopi siga kasutamine Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada siga Hadoopi Hdinsightiga."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Kasutage siga Hadoopi Hdinsightiga

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache siga](http://pig.apache.org/) on platvorm loomise programmide Hadoopi üksikasjalik tuntud *siga Ladina*keele abil. Siga on Java alternatiiv *MapReduce* lahenduste loomise ja see on kaasatud opsüsteemi Windows Azure Hdinsightiga.

Selles artiklis saate teada, kuidas saate kasutada siga koos Hdinsightiga.

##<a id="why"></a>Miks kasutada siga?

Üks probleeme, kasutades MapReduce Hadoopi andmete töötlemiseks rakendab oma töötlemise loogika ainult kaart ja vähendamine funktsiooni abil. Keerukate töötlemiseks peate murda töötlemine mitu MapReduce toimingud, mis on ühendatud sageli koos soovitud tulemuse saavutamiseks.

Sunnib saate kasutada ainult kaart ja funktsioonide vähendada, mitte siga võimaldab määratleda töötlemine teisendused, kus andmed kirjutatakse kaudu, andes soovitud väljund reana.

Siga Ladina keele saate kirjeldada andmete voogu töötlemata panuse ühe või mitme teisendused, andes soovitud väljundi kaudu. Siga Ladina programmide tehke see põhimõtteliselt.

- **Laadi**: vt andmete manipuleerida failisüsteemi kaudu
- **Muuta**: töödelda andmed
- **Dump või poe**: väljund andmete Kuva või salvestada see töötlemine

Siga Ladina toetab samuti kasutaja määratletud funktsioonid (UDF), mis võimaldab teil autonoomsest välise komponendid, et loogika, mis on raske siga Ladina mudeli.

Siga Ladina kohta leiate lisateavet teemast [siga Ladina viide käsitsi 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) ja [siga Ladina viide käsitsi 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Näiteks UDF-ID abil siga, leiate järgmised dokumendid.

* [Kasutamine DataFu Hdinsightiga siga koos](hdinsight-hadoop-use-pig-datafu-udf.md) – DataFu on kasulik UDF-ID haldab Apache kogum

* [Python kasutamine siga ja taru sisse Hdinsightiga](hdinsight-python.md)

* [Kasutage C# taru ja Hdinsightiga siga](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Näidisandmete kohta

Selles näites kasutatakse *log4j* näidisfaili, mis on talletatud veebisaidil **/example/data/sample.log** bloobimälu salvestusruumi ümbrises. Iga log faili sees koosneb rida väljad, mis sisaldab soovitud `[LOG LEVEL]` välja kuvamiseks tüüp ja raskusaste, näiteks:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Eelmises näites Logi tase on viga.

> [AZURE.NOTE] Saate luua ka log4j faili [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logimine tööriista abil ja seejärel oma bloobimälu faili üleslaadimine. Vaadake [Hdinsightiga andmete üleslaadimine](hdinsight-upload-data.md) olevaid juhiseid. Koos Hdinsightiga plekid Azure salvestusruumi kasutamise kohta leiate lisateavet teemast [Kasutamine Azure'i bloobimälu Hdinsightiga](hdinsight-hadoop-use-blob-storage.md).

Azure'i bloobimälu, mis kasutab Hdinsightiga failisüsteemi Hadoopi kogumite talletatakse näidisandmeid. Hdinsightiga pääsevad plekid **wasb** eesliite abil salvestatud failid. Näiteks sample.log failile juurdepääsu, saate kasutada järgmist süntaksit:

    wasbs:///example/data/sample.log

Kuna WASB on vaikimisi salvestusruumi jaoks Hdinsightiga, võite kasutada ka faili **/example/data/sample.log** siga Ladina abil.

> [AZURE.NOTE] Funktsiooni süntaks **wasbs: / / /**, kasutatakse vaikimisi salvestusruumi ümbris Hdinsightiga klaster talletatud failidele juurde pääseda. Kui määrasite täiendavat salvestusruumi kontod, kui teil on ette valmistatud klaster ning soovite nende kontodega salvestatud failidele juurde pääseda, pääsete andmed, määrates container nime ja salvestusruumi konto aadress, näiteks: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Valimi töö kohta

Järgmine siga Ladina töö laadib **sample.log** faili Hdinsightiga klaster vaikimisi salvestusruumist. Seejärel teeb teisendused, mille tulemuseks arvu, mitu korda iga taseme log ilmnenud sisendandmete sarja. Tulemite tavaväärtuse STDOUT sisse.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Järgmisel pildil on kujutatud jaotus, mida iga teisenduse teeb andmetega.

![Muutusi graafiline kujutis][image-hdi-pig-data-transformation]

##<a id="run"></a>Töö siga Ladina

Hdinsightiga saab käitada siga Ladina tööde haldamine, kasutades mitmesuguseid viise. Järgmise tabeli abil saate otsustada, milline viis teile sobib, siis järgige lühiülevaade linki.

| **Kasutage seda** , kui soovite...                                   | .. .an **interaktiivsed** shell | ... **Pakktöötlus** | .. ei ole see **kobar operatsioonisüsteem** | .. .minu **Kliendi operatsioonisüsteem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X või Windows        |
| [Curl](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux või Windows                          | Linux, Unix, Mac OS X või Windows        |
| [.NET SDK Hadoopi jaoks](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux või Windows                          | Windowsi (vähemalt praegu)                        |
| [Windows PowerShelli](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux või Windows                          | Windows                                  |
| [Kaugtöölaua](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Töötab siga tööde haldamine Windows Azure Hdinsightiga asutusesisese SQL Server Integration Services abil

SQL serveri Integration Services (SSIS) abil saate käivitada siga töö. Azure'i Paketita ssisoledb pakub järgmised komponendid, mis Hdinsightiga siga töö töötada.


- [Azure Hdinsightiga siga ülesanne][pigtask]
- [Azure'i tellimuse haldur][connectionmanager]


Lugege lisateavet Azure Paketita ssisoledb [siin][ssispack].


##<a id="nextsteps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas kasutada siga Hdinsightiga, kasutage järgmisi linke uurida muul viisil töötada Windows Azure Hdinsightiga.

* [Laadi andmed Hdinsightiga][hdinsight-upload-data]
* [Hdinsightiga taru kasutamine][hdinsight-use-hive]
* [Hdinsightiga Sqoop kasutamine](hdinsight-use-sqoop.md)
* [Hdinsightiga Oozie kasutamine](hdinsight-use-oozie.md)
* [Hdinsightiga MapReduce töö kasutamine][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
