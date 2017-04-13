<properties
   pageTitle="Hadoopi siga kasutamine rakenduses Hdinsightiga Curl | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Curl siga Ladina töö tegemiseks Hadoopi kobar rakenduses Windows Azure Hdinsightiga."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Käivitage siga töö Hadoopi Hdinsightiga Curl abil

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Selles dokumendis saate teada, kuidas kasutada Curl siga Ladina töö käitamist on Windows Azure Hdinsightiga kobar. Siga Ladina programmeerimiskeel võimaldab teil teisendused, mis on rakendatud sisendandmete andes soovitud väljund kirjeldamiseks.

Curl kasutatakse demonstreerivad seda, kuidas saab interaktiivselt kasutada Hdinsightiga käivitamine, jälgimine ja siga töökohtade tulemeid tuua töötlemata HTTP päringuid abil. See toimib, kasutades WebHCat REST API (varem Templeton), mis on esitatud Hdinsightiga klaster.

> [AZURE.NOTE] Kui teil on juba tuttavad Hadoopi Linuxi-põhiste serverite kasutamine, kuid ei ole Hdinsightiga, vaadake teemat [Linux-põhine Hdinsightiga näpunäiteid](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Windows Azure Hdinsightiga (Hadoopi Hdinsightiga) kobar, (Linux-põhine või Windowsi-põhise)

* [Curl](http://curl.haxx.se/)

* [JQ](http://stedolan.github.io/jq/)

##<a id="curl"></a>Käivitage siga töö Curl abil

> [AZURE.NOTE] Kui WebHCat Curl või muu ülejäänud side kasutamine tuleb autentida taotlused pakkudes Hdinsightiga kobar administraatori kasutajanime ja parooliga. Peate kasutama ka kobar nime osana, on ühtse identifikaator (URL) on kasutatud funktsiooni päringuid saata server.
>
> Selles jaotises käsud, Asendage **KASUTAJANIME** ja kasutajale autentida klaster, ja **parooli** kasutajakonto parooli. Asendage **CLUSTERNAME** klaster nime.
>
> [Elementaarne autentimine](http://en.wikipedia.org/wiki/Basic_access_authentication)on tagada REST API-ga. Peate alati taotlusi Secure HTTP (HTTPS) abil tagada mandaat saadetakse turvaline server.

1. Kasutada käsureal, kinnitamaks, et saate luua ühenduse Hdinsightiga klaster järgmine käsk:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Peaksite nägema umbes järgmist vastuse:

        {"status":"ok","version":"v1"}

    Selle käsu parameetritest on järgmised:

    * **-u**: kasutajanimi ja parool, taotluse kinnitamiseks
    * **-G**: kui see on GET-päringu

    URL-i **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, alguses on sama kõik taotlused. Tee, **/status**, näitab, et kutse on oleku WebHCat (tuntud ka kui Templeton) tagastamiseks server.

2. Kasutage siga Ladina töö klaster esitada järgmine kood:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Selle käsu parameetritest on järgmised:

    * **-d**: Kuna `-G` ei kasutata kutse on vaikimisi postituse meetod. `-d`Saate määrata andmete väärtused, mis saadetakse koos.

        * **User.name**: kasutaja, kes on käsu
        * **käivitada**: The siga Ladina laused käivitada
        * **statusdir**: selle töö oleku kirjutatakse kataloogis

    > [AZURE.NOTE] Teade, et tühikuid siga Ladina laused on asendatud selle `+` märkide kasutamisel koos Curl.

    See käsk peaks tagastama töö ID, mida saab kasutada oleku tööd, näiteks:

        {"id":"job_1415651640909_0026"}

3. Töö oleku kontrollimine, käsu. Asendage **JOBID** eelmises etapis tagastatud väärtus. Näiteks kui tagastatav väärtus on `{"id":"job_1415651640909_0026"}`, siis oleks **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Kui töö on lõpule jõudnud, siis riigi **õnnestus**.

    > [AZURE.NOTE] See Curl taotluse tagastab JavaScript Object märke (JSON) dokumendi töö kohta lisateabe saamiseks ja jq kasutatakse ainult oleku väärtus.

##<a id="results"></a>Tulemuste kuvamine

Kui töö on muutunud, **õnnestus**, võib allalaadimiseks Azure'i bloobimälu töö tulemused. Funktsiooni `statusdir` koos päringuga edastatud parameeter sisaldab väljundfail; asukoht Sel juhul **wasbs: / / / näide/pigcurl**. Selle aadressi talletab väljundi töö **näide/pigcurl** kataloogi vaikimisi salvestusruumi ümbrises kasutatavaid Hdinsightiga klaster.

Saate loendi ja [Azure CLI](../xplat-cli-install.md)abil need failid alla laadida. Kasutage failide loendi **näide/pigcurl**, näiteks järgmine käsk:

    azure storage blob list <container-name> example/pigcurl

Saate alla laadida faili, kasutage järgmist:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Määrake salvestusruumi konto nimi, mis sisaldab soovitud bloobimälu abil soovitud `-a` ja `-k` parameetrid või Sea selle **AZURE'I\_salvestusruumi\_konto** ja **AZURE'I\_salvestusruumi\_ACCESSI\_klahvi** keskkonna muutujate.

##<a id="summary"></a>Kokkuvõte

Nagu on näidatud selles dokumendis, saate töötlemata HTTP päringu käivitamine, jälgimine ja siga töö tulemuste vaatamiseks Hdinsightiga klaster.

ÜLEJÄÄNUD kasutajaliides, mis kasutab sellest artiklist leiate lisateavet teemast [WebHCat viide](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet siga Hdinsightiga:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)
