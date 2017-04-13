<properties
   pageTitle="Hadoopi Sqoop kasutamine rakenduses Hdinsightiga Curl | Microsoft Azure'i"
   description="Saate teada, kuidas Sqoop tööde Hdinsightiga abil Curl kaugühenduse teel edastamiseks."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Hadoopi rakenduses Curl Hdinsightiga käivitada Sqoop tööde haldamine

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saate teada, kuidas kasutada Curl Sqoop töö Hadoopi klaster Hdinsightiga sisse.

Curl saab näidata, kuidas saab interaktiivselt kasutada Hdinsightiga abil töötlemata HTTP päringuid käivitamine, jälgimine ja Sqoop töökohtade tulemeid tuua. See toimib, kasutades WebHCat REST API (varem Templeton) pakutavast Hdinsightiga klaster.

> [AZURE.NOTE] Kui teil on juba tuttavad Hadoopi Linuxi-põhiste serverite kasutamine, kuid ei ole Hdinsightiga, lugege [teavet Hdinsightiga Linux kasutamise kohta](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Hadoopi Hdinsightiga kobar (Linux või Windowsi-põhise)

* [Curl](http://curl.haxx.se/)

* [JQ](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Esitage Sqoop töö Curl abil

> [AZURE.NOTE] Kui kasutate Curl või muu ülejäänud side koos WebHCat, tuleb autentida taotlused pakkudes Hdinsightiga kobar administraatori kasutajanime ja parooli. Peate kasutama ka kobar nime osana, on ühtse identifikaator (URL) kasutatakse päringuid serveriga.
>
> Selles jaotises käsud, Asendage **KASUTAJANIME** ja kasutajale autentida klaster, ja **parooli** kasutajakonto parooli. Asendage **CLUSTERNAME** klaster nime.
>
> REST API on tagatud [elementaarautentimine](http://en.wikipedia.org/wiki/Basic_access_authentication)kaudu. Peate alati taotlusi Secure HTTP (HTTPS) abil tagada mandaat saadetakse turvaline server.

1. Kasutada käsureal, kinnitamaks, et saate luua ühenduse Hdinsightiga klaster järgmine käsk:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Peaksite nägema umbes järgmist vastuse:

        {"status":"ok","version":"v1"}

    Selle käsu parameetritest on järgmised:

    * **-u** - kasutajanimi ja parool, taotluse kinnitamiseks.
    * **-G** - näitab, et see on GET-päringu.

    URL-i **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, alguses on sama kõik taotlused. Tee, **/status**, näitab, et tagastada olek WebHCat (tuntud ka kui Templeton) on taotluse server. 

2. Kasutage järgmist esitada sqoop töö.


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Selle käsu parameetritest on järgmised:

    * **-d** - Kuna `-G` ei kasutata kutse on vaikimisi postituse meetod. `-d`Saate määrata andmete väärtused, mis saadetakse koos.

        * **user.name** - kasutaja, kus töötab käsk.

        * **käsk** - The Sqoop käsk käivitada.

        * **statusdir** - kataloog, mis selle töö oleku kirjutatakse.

    See käsk peaks tagastama töö ID, mida saab kasutada töö oleku.

        {"id":"job_1415651640909_0026"}

3. Töö oleku kontrollimine, käsu. Asendage **JOBID** eelmises etapis tagastatud väärtus. Näiteks kui tagastatav väärtus on `{"id":"job_1415651640909_0026"}`, siis oleks **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Kui töö on lõpule jõudnud, siis riigi **õnnestus**.

    > [AZURE.NOTE] See Curl pärimiseks JavaScript Object märke (JSON) dokument teabega töö; JQ saab tuua ainult oleku väärtus.

4. Pärast töö on muutunud, **õnnestus**, võib allalaadimiseks Azure'i bloobimälu töö tulemused. Funktsiooni `statusdir` koos päringuga edastatud parameeter sisaldab väljundfail; asukoht Sel juhul **wasbs: / / / näide/curl**. Selle aadressi talletab väljundi töö **näide/curl** kataloog vaikimisi salvestusruumi container kasutatavaid Hdinsightiga klaster.

    Saate loendi ja [Azure CLI](../xplat-cli-install.md)abil need failid alla laadida. Kasutage failide loendi **näide/curl**, näiteks järgmine käsk:

        azure storage blob list <container-name> example/curl

    Saate alla laadida faili, kasutage järgmist:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Kas peate määrama salvestusruumi konto nimi, mis sisaldab soovitud bloobimälu abil soovitud `-a` ja `-k` parameetrid või Sea soovitud **AZURE\_salvestusruumi\_konto** ja **AZURE\_salvestusruumi\_ACCESSI\_klahvi** keskkonna muutujate. Vt < href = "hdinsightiga-üles-data.md" target = "_blank" Lisateavet.

##<a name="limitations"></a>Piirangud

* Hulgi ekspordi - ja Linux-põhine Hdinsightiga, kasutatakse Microsoft SQL Server või Azure'i SQL-andmebaasi andmete eksportimine Sqoop konnektor ei toeta praegu hulgi lisab.

* Partiide - ja Linux-põhine Hdinsightiga, kasutades funktsiooni `-batch` vahetada, kui lisatakse, Sqoop täidab mitme lisab asemel partiide lisa toimingud.

##<a name="summary"></a>Kokkuvõte

Nagu on näidatud selles dokumendis, saate töötlemata HTTP päringu käivitamine, jälgimine ja Sqoop töö tulemuste vaatamiseks Hdinsightiga klaster.

ÜLEJÄÄNUD liides kasutab sellest artiklist leiate lisateavet teemast <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API juhend</a>.

##<a name="next-steps"></a>Järgmised sammud

Üldteavet taru koos Hdinsightiga:

* [Hadoopi Hdinsightiga Sqoop kasutamine](hdinsight-use-sqoop.md)

Teavet teiste mooduste kohta, saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

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




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


