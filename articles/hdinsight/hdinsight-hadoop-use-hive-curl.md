<properties
   pageTitle="Hadoopi taru kasutamine rakenduses Hdinsightiga Curl | Microsoft Azure'i"
   description="Saate teada, kuidas siga tööde abil Curl Hdinsightiga kaugühenduse teel edastamiseks."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Hadoopi rakenduses Hdinsightiga Curl koos taru päringute sooritamine

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Selles dokumendis saate teada, kuidas kasutada Curl taru päringute käitamist Hadoopi Windows Azure Hdinsightiga kobar.

Curl kasutatakse näidata, kuidas saab interaktiivselt kasutada Hdinsightiga töötlemata HTTP päringuid käivitamine, jälgimine ja toomiseks taru päringute abil. See toimib, kasutades WebHCat REST API (varem Templeton) pakutavast Hdinsightiga klaster.

> [AZURE.NOTE] Kui teil on juba tuttavad Hadoopi Linuxi-põhiste serverite kasutamine, kuid ei ole Hdinsightiga, vt [mida on vaja teada Hadoopi Linux-põhine Hdinsightiga](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Hadoopi Hdinsightiga kobar (Linux või Windowsi-põhise)

* [Curl](http://curl.haxx.se/)

* [JQ](http://stedolan.github.io/jq/)

##<a id="curl"></a>Käivitage taru päringute abil Curl

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

    URL-i **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, alguses on sama kõik taotlused. Tee, **/status**, näitab, et tagastada olek WebHCat (tuntud ka kui Templeton) on taotluse server. Soovi korral võite taru versiooni abil järgmine käsk:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    See peaks tagastama vastuse umbes järgmine:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Nimega **log4jLogs**uue tabeli loomiseks kasutada järgmist:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Selle käsu parameetritest on järgmised:

    * **-d** - Kuna `-G` ei kasutata kutse on vaikimisi postituse meetod. `-d`Saate määrata andmete väärtused, mis saadetakse koos.

        * **user.name** - kasutaja, kus töötab käsk.

        * **käivitada** - The HiveQL laused käivitada.

        * **statusdir** - kataloog, mis selle töö oleku kirjutatakse.

    Need teatised järgmisi toiminguid:

    * **Tabeli DROP** - kustutab tabeli ja andmefaili, kui tabel on juba olemas.

    * **Välise tabeli loomine** – loob uue tabeli "väline" taru. Välise tabeleid säilitada tabeli määratlus taru. Andmed on jäänud algsesse asukohta.

        > [AZURE.NOTE] Kui eeldate alusandmete värskendada välise andmeallikaga, nagu on automaatne andmete üleslaadimine või mõne muu MapReduce toiming, kuid tahate alati taru päringute kasutada uusimaid andmeid tuleb kasutada välise tabeleid.
        >
        > Kas pukseerimine on Välistabel **ei** Kustuta andmed tabeli määratlus.

    * **Rea vorming** – taru saate teada, kuidas andmed on vormindatud. Sel juhul iga log väljad on eraldatud ruumi.

    * **Salvestatud AS TEKSTIFAILIDE asukoht** – ütleb taru, kus andmed on salvestatud (nt/andmete kataloogi) ja see, et tekstina.

    * **Valige** - valib kõik read, kus veerus **t4** sisaldab väärtust **[ERROR]**arv. See peaks tagastama väärtus **3** , kui palju on kolm rida, mis sisaldavad seda väärtust.

    > [AZURE.NOTE] Teade, et tühikute HiveQL laused on asendatud selle `+` märkide kasutamisel koos Curl. Pakutud väärtused, mis sisaldavad tühik, nt eraldaja, ei saa asendada `+`.

    * **INPUT__FILE__NAME nagu "% 25.log"** – seda ainult otsingu kasutamine failide lõpus on piirangud. log. Kui see puudub, proovib taru otsimiseks selle kataloogi ja selle alamkatalooge, sh faile, mis ei vasta selle tabeli jaoks määratletud veeru skeemiga kõik failid.

    > [AZURE.NOTE] Pange tähele, et 25% on URL-i kodeeritud vormi %, seega on tegelik seisund `like '%.log'`. % On olevat URL-ina kodeeritav, käsitletakse seda URL-ide teisiti märgina.

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

6. Uue nimega **errorLogs**"sisemine" tabeli loomiseks kasutada järgmistest:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Need teatised järgmisi toiminguid:

    * **Looge tabel kui pole olemas** - loob tabeli, kui see pole juba olemas. Kuna **väline** märksõna ei kasutata, see on sisemine tabel, mis on talletatud taru andmebaas ja hallatakse täielikult taru.

        > [AZURE.NOTE] Erinevalt välise tabeleid, pukseerimine on sisemine tabel kustutatakse ka aluseks olevad andmed.

    * **Salvestatud AS ORC** - salvestab andmed optimeeritud rea piklikku (ORC) vormingus. See on väga optimeeritud ja tõhusa vorming taru andmete talletamiseks.
    * **Lisa kirjutada... Valige** - klõpsab ridade **log4jLogs** tabeli, mis sisaldavad **[ERROR]**, seejärel lisab andmed tabelisse **errorLogs** .
    * **Valige** - valib kõik read **errorLogs** uuest tabelist.

7. Kasutage töö ID tagastada töö oleku vaatamine. Kui see on õnnestus, kasutage Azure'i CLI for Mac, Windows ja Linux, nagu on kirjeldatud eelnevalt alla laadida ja tulemusi vaadata. Väljund peaks sisaldama kolm rida, mis sisaldavad **[ERROR]**.


##<a id="summary"></a>Kokkuvõte

Nagu on näidatud selles dokumendis, saate töötlemata HTTP päringu käivitamine, jälgimine ja taru töö tulemuste vaatamiseks Hdinsightiga klaster.

ÜLEJÄÄNUD liides kasutab sellest artiklist leiate lisateavet teemast <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat viide</a>.

##<a id="nextsteps"></a>Järgmised sammud

Üldteavet taru koos Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

Teavet teiste mooduste kohta, saate töötada Hadoopi Hdinsightiga:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

Kui kasutate Tez taru, leiate teavet silumine järgmised dokumendid.

* [Windowsi-põhiste Hdinsightiga Tez UI kasutamine](hdinsight-debug-tez-ui.md)

* [Kasutage Linux-põhine Hdinsightiga Ambari Tez vaade](hdinsight-debug-ambari-tez-view.md)

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


