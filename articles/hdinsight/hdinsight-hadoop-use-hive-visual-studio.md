<properties
   pageTitle="Päringu Hadoopi tööriistu taru Visual Studio | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada taru Hadoopi rakenduses Visual Studio Hadoopi tööriistade abil Hdinsightile."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Visual Studio Hdinsightiga tööriistade abil taru päringute sooritamine

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Sellest artiklist saate teada, kuidas esitada taru päringute Hdinsightiga kobar, mis Visual Studio Hdinsightiga tööriistade abil.

> [AZURE.NOTE] Selle dokumendi ei paku HiveQL laused, näidetes kasutatud teha üksikasjalikku kirjeldust. HiveQL, mida selles näites kasutatakse kohta leiate teavet teemast [Kasutamine taru Hadoopi Hdinsightiga abil](hdinsight-use-hive.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist.

* Windows Azure Hdinsightiga (Hadoopi Hdinsightiga) on kobar (Linux või Windowsi-põhise)

* Visual Studio (üks järgmistest versioonidest):

    Visual Studio 2013 ühenduse/Professional/Premium/Ultimate koos [värskendada 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (ühenduse väljaanne Enterprise)

- Visual studio Hdinsightiga tööriistad. Installimine ja konfigureerimine tööriistade kohta lisateabe saamiseks vt [Visual Studio Hadoopi tööriistad Hdinsightiga kasutamise alustamine](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a id="run"></a>Visual Studio Hdinsightiga tööriistade abil taru päringute sooritamine

1. Avage **Visual Studio** ja valige **Uus** > **projekti** > **Hdinsightiga** > **Taru rakendus**. Sisestage selle projekti nimi.

2. Avage **Script.hql** fail, mis on loodud projekti ja kleepige HiveQL järgmistest:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Need teatised järgmisi toiminguid:

    * **Tabeli LANGEV**: kustutab tabeli ja andmefaili, kui tabel on juba olemas.
    * **Välise tabeli loomine**: loob uue tabeli "väline" taru. Välise tabeleid talletada ainult tabeli määratlus taru (andmed on jäänud algsesse asukohta).

        > [AZURE.NOTE] Kui eeldate alusandmete tuleb värskendada välise andmeallikaga (nt automaatne andmete üleslaadimine) või mõni muu MapReduce toiming tuleb kasutada välise tabeleid, kuid tahate alati taru päringute kasutada uusimaid andmeid.
        >
        > Kas pukseerimine on Välistabel **ei** Kustuta andmed tabeli määratlus.

    * **Rea vorming**: taru saate teada, kuidas andmed on vormindatud. Sel juhul iga log väljad on eraldatud ruumi.
    * **Salvestatud AS TEKSTIFAILIDE asukoht**: ütleb taru, kus andmed on salvestatud (nt/andmete kataloogi) ja tekstina talletatud.
    * **Valige**: valige kui veeru **t4** sisaldavad väärtust **[ERROR]**kõigi ridade arv. See peaks tagastama väärtus **3** , sest seal on kolm rida, mis sisaldavad seda väärtust.
    * **INPUT__FILE__NAME nagu "%.log"** - kirjeldatakse, mida me ainult tagasi andmete failidest, mille lõpus on taru. log. See piirab otsingu sample.log faili, mis sisaldab andmeid, ja tagastab andmed muu näiteks andmefailid, mis ei vasta meil määratletud skeemiga hoiab.

3. Tööriistaribal valige **Hdinsightiga kobar** , mida soovite kasutada selle päringu ja valige seejärel **Edasta WebHCat** käivitamiseks aruannete abil WebHCat taru tööd. Samuti saate esitada tööd, kasutades nupp __Käivita HiveServer2 kaudu__ , kui HiveServer2 on saadaval teie kobar versioon. **Taru töö kokkuvõte** kuvatakse ja käivitatud töö teavet. Lingi **värskendamine** abil värskendada töö teave, kuni **Töö olek** muutub **lõpule**.

4. **Töö väljundi** lingi abil saate vaadata selle töö väljund. See peaks kuva `[ERROR] 3`, mis on SELECT-lause poolt tagastatud väärtus.

5. Taru päringute saab käivitada ka ilma projekti loomine. **Server Explorer**, laiendage **Azure'i** > **Hdinsightiga**, paremklõpsake Hdinsightile oma serveri ja seejärel valige **kirjutamine taru päringu**.

6. Lisage **temp.hql** dokumendi HiveQL järgmistest:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Need teatised järgmisi toiminguid:

    * **Looge tabel kui pole olemas**: loob tabeli, kui see pole juba olemas. Kuna **väline** märksõna ei kasutata, see on sisemine tabel, mis on talletatud taru andmebaas ja hallatakse täielikult taru.

        > [AZURE.NOTE] Erinevalt **välise** tabeleid, pukseerimine on sisemine tabel kustutatakse ka aluseks olevad andmed.

    * **Salvestatud AS ORC**: talletatakse andmeid optimeeritud rea veeruformaadis (ORC). See on väga optimeeritud ja tõhusa vorming taru andmete talletamiseks.
    * **Lisa kirjutada... Valige**: valitakse tabelist **log4jLogs** sisaldavate ridade **[ERROR]**, seejärel lisab andmed tabelisse **errorLogs** .

7. Valige tööriistaribal **Edasta** töö käivitamiseks. **Töö oleku** abil saate määratleda töö on lõpule viidud.

8. Veenduge, et töö lõpetatud ja loonud uue tabeli, kasutage **Server Explorer** ja laiendada **Azure'i** > **Hdinsightiga** > Hdinsightiga klaster > **Taru andmebaaside** > ja **vaikeväärtus**. Peaksite nägema **errorLogs** tabel ja **log4jLogs** .

##<a id="summary"></a>Kokkuvõte

Nagu näete, et Hdinsightiga tööriistad Visual Studio pakkuda lihtne viis taru päringute käitamist on Hdinsightiga kobar, töö oleku jälgimine ja tuua väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet rakenduses Hdinsightiga taru kohta:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

Lisateavet Hdinsightiga tööriistad Visual Studio:

* [Alustamine Hdinsightiga tööriistad Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
