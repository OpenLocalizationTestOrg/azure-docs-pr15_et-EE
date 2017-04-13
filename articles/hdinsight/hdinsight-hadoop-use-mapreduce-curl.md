<properties
   pageTitle="Kasuta MapReduce ja Curl koos Hadoopi rakenduses Hdinsightile | Microsoft Azure'i"
   description="Saate teada, kuidas eemalt käivitage MapReduce töö Hadoopi Hdinsightiga Curl abil."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Käivitage MapReduce töö Hadoopi Hdinsightiga Curl abil

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Selles dokumendis saate teada, kuidas kasutada Curl MapReduce töö tegemiseks Hadoopi Hdinsightiga kobar.

Curl kasutatakse näidata, kuidas saab interaktiivselt kasutada Hdinsightiga töötlemata HTTP päringuid abil saate käivitada MapReduce tööd. See toimib, kasutades WebHCat REST API (varem Templeton) pakutavast Hdinsightiga klaster.

> [AZURE.NOTE] Kui teil on juba tuttavad Hadoopi Linuxi-põhiste serverite kasutamine, kuid teil on uus Hdinsightiga, vt [mida on vaja teada Linux-põhine Hadoopi Hdinsightiga](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Hadoopi Hdinsightiga kobar (Linux või Windowsi-põhise)

* [Curl](http://curl.haxx.se/)

* [JQ](http://stedolan.github.io/jq/)

##<a id="curl"></a>MapReduce tööde Curl abil

> [AZURE.NOTE] Kui kasutate Curl või muu ülejäänud side WebHCat, tuleb autentida taotlused esitada Hdinsightiga kobar administraatori kasutajanime ja parooli. Peate kasutama kobar nime ka URI, mida kasutatakse selle päringuid saata server osana.
>
> Selles jaotises käsud, Asendage **kasutajanimi** kasutaja autentimiseks kobar ja **parooli** kasutajakonto parooli. Asendage **CLUSTERNAME** klaster nime.
>
> [Elementaarne autentimist](http://en.wikipedia.org/wiki/Basic_access_authentication)kasutades on turvatud REST API-ga. Peate taotlusi alati HTTPS abil tagada mandaat saadetakse turvaline server.

1. Mõne käsurea kaudu kasutage kinnitamaks, et saate luua ühenduse Hdinsightiga klaster järgmine käsk:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Peaksite nägema umbes järgmist vastuse:

        {"status":"ok","version":"v1"}

    Selle käsu parameetritest on järgmised:

    * **-u**: näitab kasutajanimi ja parool, taotluse kinnitamiseks
    * **-G**: kui see on GET-päringu

    URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, alguses on sama kõik taotlused.

2. Kasutage MapReduce töö esitada järgmine käsk:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    URI (/ mapreduce/jar) lõppu ütleb WebHCat taotlus hakkavad MapReduce töö ainekursuse jar faili kaudu. Selle käsu parameetritest on järgmised:

    * **-d**: `-G` ei kasutata, nii, et kutse on vaikimisi postituse meetod. `-d`Saate määrata andmete väärtused, mis saadetakse koos.

        * **User.name**: kasutaja, kes on käsu
        * **jar**: jar fail, mis sisaldab tunni olema asukoha parandusfunktsiooni
        * **klassi**: klassi, mis sisaldab MapReduce loogika
        * **ARG**: MapReduce töö; edastatavate argumentide Sel juhul, teksti sisestamine faili ja selle väljundi kasutatavate kataloogis

    See käsk peaks tagastama töö ID, mida saab kasutada töö oleku:

        {"id":"job_1415651640909_0026"}

3. Töö oleku kontrollimine, käsu. Asendage **JOBID** eelmises etapis tagastatud väärtus. Näiteks kui tagastatav väärtus on `{"id":"job_1415651640909_0026"}`, siis oleks selle JOBID `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Kui töö on lõpule jõudnud, olek kuvatakse olema "õnnestus".

    > [AZURE.NOTE] Annab vastuseks Curl taotlus JSON dokument teabega töö; JQ saab tuua ainult oleku väärtus.

4. Kui töö on muutunud, **õnnestus**, võib allalaadimiseks Azure'i bloobimälu töö tulemused. Funktsiooni `statusdir` parameeter, mis on möödunud päring sisaldab väljundfail; asukoht Sel juhul **wasbs: / / / näide/curl**. Selle aadressi talletab väljundi töö **näide/curl** kataloogi vaikimisi salvestusruumi ümbrises kasutatavaid Hdinsightiga klaster.

Saate loendi ja [Azure CLI](../xplat-cli-install.md)abil need failid alla laadida. Kasutage failide loendi **näide/curl**, näiteks järgmine käsk:

    azure storage blob list <container-name> example/curl

Saate alla laadida faili, kasutage järgmist:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Määrake salvestusruumi konto nimi, mis sisaldab soovitud bloobimälu abil soovitud `-a` ja `-k` parameetrid või Sea selle **AZURE'I\_salvestusruumi\_konto** ja **AZURE'I\_salvestusruumi\_ACCESSI\_klahvi** keskkonna muutujate. Vaadake [Hdinsightiga andmete üleslaadimine](hdinsight-upload-data.md) lisateavet.

##<a id="summary"></a>Kokkuvõte

Nagu on näidatud selles dokumendis, saate töötlemata HTTP-päringu käivitamine, jälgimine ja Hdinsightiga klaster taru töökohtade tulemusi vaadata.

ÜLEJÄÄNUD kasutajaliidese, mida kasutatakse selle artikli kohta leiate lisateavet teemast [WebHCat viide](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet MapReduce töökohtade Hdinsightiga kohta:

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)
