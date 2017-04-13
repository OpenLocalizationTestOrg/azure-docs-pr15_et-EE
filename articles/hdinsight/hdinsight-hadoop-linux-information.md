<properties
   pageTitle="Näpunäiteid Hadoopi kasutamine Linux-põhine Hdinsightiga | Microsoft Azure'i"
   description="Kasutada Linux-põhine Hdinsightiga (Hadoopi) kogumite tuttav Linux keskkonnas töötab Azure'i pilves rakendamist näpunäited."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Teavet Linux Hdinsightiga kasutamise kohta

Linux-põhine Windows Azure Hdinsightiga kogumite pakuvad Hadoopi tuttav Linux keskkonnas, töötavad Azure pilveteenuses. Enamik asju, see peaks toimima täpselt nii, nagu mis tahes Hadoop-Linux installi. See dokument nõuab tähelepanu teatud erinevused, mis peaks olema teadlik.

##<a name="prerequisites"></a>Eeltingimused

Selles dokumendis juhiseid paljude kasutada järgmist Utiliidid, mis võib-olla teie arvutisse installitud.

* [cURL](https://curl.haxx.se/) – veebi-põhiste teenuste suhtlemiseks kasutada
* [jq](https://stedolan.github.io/jq/) – kasutatud sõeluda JSON-dokumendid
* [Azure'i CLI](../xplat-cli-install.md) - Azure'i teenuste kaugühenduse teel haldamiseks kasutada

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Domeeninimed

Täielik domeeninimi (FQDN) kasutada ühendamisel klaster Interneti-ühendus on ** &lt;clustername >. azurehdinsight.net** või (ainult SSH) ** &lt;clustername-ssh >. azurehdinsight.net**.

Sees, iga sõlme klaster on nimi, mis on määratud kobar konfigureerimise käigus. Kobar nimede leidmiseks saate lehelt __Hosts__ Ambari Web UI või kasutada järgmist hostide loendi tulu Ambari REST API:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Asendage __parool__ parool administraatori konto ja __CLUSTERNAME__ klaster nimi. JSON dokument, mis sisaldab loendit klaster hosts taastatakse ja seejärel jq tõmbab välja soovitud `host_name` iga host klaster elemendi väärtuse.

Kui teil on vaja leida kindla teenuse sõlme nimi, saab Ambari mõeldud komponendi päringud. Näiteks HDFS nimi sõlm hosts leidmiseks kasutage järgmist.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

See tagastab JSON dokumendi, mis kirjeldab teenust ja seejärel jq tõmbab välja ainult selle `host_name` hosts väärtus.

## <a name="remote-access-to-services"></a>Remote access Services

* **Ambari (web)** - https://&lt;clustername >. azurehdinsight.net

    Autentida kobar administraator kasutaja ja parooli abil ja seejärel logige sisse Ambari. Seda kasutatakse ka kobar administraator kasutaja ja parool.

    Autentimine on Lihttekst - alati HTTPS-i abil tagada, et ühendus on turvaline.

    > [AZURE.IMPORTANT] Ambari jaoks klaster on juurdepääsetav otse Interneti kaudu, mõned funktsioonid tugineb juurdepääs sõlmed kasutatavaid klaster sisemine domeeni nimi. Kuna see on sisemine domeeni nimi, mitte avaldada, saate "server ei leitud" tõrgete kui proovite mõnele funktsioonile juurdepääsuks Interneti kaudu.
    >
    > Kasutage Ambari Web UI täisfunktsionaalsuse kasutamiseks on SSH tunneliga, et puhverserveri web-liikluse kobar pea sõlme. Lugege teemat [kasutamine SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, ja muud web UI's](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** – https://&lt;clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Autentida kobar administraator kasutaja ja parooli abil.
    >
    > Autentimine on Lihttekst - alati HTTPS-i abil tagada, et ühendus on turvaline.

* **WebHCat (Templeton)** – https://&lt;clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Autentida kobar administraator kasutaja ja parooli abil.
    >
    > Autentimine on Lihttekst - alati HTTPS-i abil tagada, et ühendus on turvaline.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net port 22 või 23. Pordi 22 kasutatakse ühenduse esmane headnode, 23 kasutatakse ühenduse teisese. Pea sõlmed kohta leiate lisateavet teemast [kättesaadavus ja usaldusväärsus Hadoopi kogumite Hdinsightiga sisse](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Kobar pea sõlmed pääseb juurde ainult SSH kliendi seadme kaudu. Kui ühendus on loodud, siis pääsete töötaja sõlmed on headnode SSH kaudu.

## <a name="file-locations"></a>Failide asukohad

Hadoopi seotud faile leiate kobar sõlmed veebisaidil `/usr/hdp`. See kaust sisaldab järgmist alamkatalooge.

* __2.2.4.9-1__: selle kausta nimega Hortonworks andmete platvormi number klaster võib olla erinev, kui see siin loetletud Hdinsightiga, kasutavad versiooni jaoks.
* __praeguse__: see kaust sisaldab linke kataloogide __2.2.4.9-1__ Directory ja on olemas, et teil pole vaja tippida versiooninumber (mis võib muutuda,) iga kord, kui soovite faili juurde.

Näide andmete ja JAR failid leiate Hadoopi jaotatud faili süsteemi (HDFS) või Azure'i bloobimälu salvestusruum ' / näide "või" wasbs: / / / näide ".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, Azure'i bloobimälu ja salvestusruumi head tavad

Enamik Hadoopi viimati, HDFS toetavad kohalikku masinad klaster. Kuigi see on tõhusa, võib olla lahenduse pilvepõhist kulukas kui teilt tund või minut Arvuta ressursid.

Hdinsightiga kasutab Azure'i bloobimälu vaikimisi poest, mis pakub järgmised eelised:

* Odavad pikemaks

* Hõlbustusfunktsioonide välisteenused, nt veebisaidid, faili üles/alla laadida Utiliidid, erinevate keele SDK-d ja veebibrauserite kaudu

Kuna see on vaikimisi talletatud Hdinsightiga, tavaliselt pole teil midagi teha seda kasutada. Näiteks järgmine käsk loetletakse **/example/data** kausta, mis on talletatud Azure'i bloobimälu failid:

    hdfs dfs -ls /example/data

Osad käsud nõuda – saate määrata, et kasutate bloobimälu. Nende, saate eesliite käsku **wasb: / /**, või **wasbs: / /**.

Hdinsightiga võimaldab teil klaster bloobimälu salvestusruumi mitmelt seostada. Juurdepääsu andmetele-vaikimisi bloobimälu salvestusruumi konto, saate kasutada vormingu **wasbs: / /&lt;container-name>@&lt;konto nimi >.blob.core.windows.net/**. Näiteks järgmine loetletakse määratud container ja bloobimälu salvestusruumi konto **/example/data** kataloogi sisu:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Millist bloobimälu klaster kasutab?

Kobar loomise ajal valitud kasutama olemasoleva Azure Storage konto ja container, või looge uus. Seejärel, olete ilmselt unustanud selle kohta. Ambari REST API abil saate otsida vaikekonto salvestusruumi ja container.

1. Järgmise käsu abil saate tuua HDFS konfiguratsiooni teabe curl ja filtreerida [jq](https://stedolan.github.io/jq/)abil.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Taastatakse esimese konfiguratsiooni rakendatud server (`service_config_version=1`,) mis sisaldab seda teavet. Väärtus, mis on muudetud pärast kobar loomist toomisel, peate loendisse konfiguratsiooni versioonid ja tuua viimati.

    See tagastavad väärtuse sarnaneb järgmise, kus __CONTAINER__ on vaike-ümbrisest ja __ACCOUNT_NAME__ on Azure Storage konto nimi:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Ressursirühma salvestusruumi konto, võite kasutada [Azure CLI](../xplat-cli-install.md). Järgmine käsk Asenda __ACCOUNT_NAME__ vormilt Ambari salvestusruumi konto nimi:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Tagastab konto ressursi rühma nime.
    
    > [AZURE.NOTE] Kui midagi on tagastatud selle käsu, peate muuta Azure'i CLI Azure'i ressursihaldur režiimi ja käivitage käsk uuesti. Azure'i ressursihaldur režiimi aktiveerimiseks käsu.
    >
    > `azure config mode arm`
    
2. Saada salvestusruumi konto võti. Asendage __GROUPNAME__ eelmises juhises ressursirühma. Salvestusruumi konto nime __ACCOUNT_NAME__ asendamiseks tehke järgmist.

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Tagastab konto primaarvõti.

Leiate Azure'i portaalis salvestusruumi teave:

1. Valige [Azure portaali](https://portal.azure.com/)Hdinsightiga klaster.

2. Valige jaotises __Essentialsi__ __Kõik sätted__.

3. Valige menüü __sätted__ __Azure'i salvestusruumi võtmed__.

4. __Azure'i salvestusruumi klahvid__, valige üks loetletud salvestusruumi kontod. See kuvab salvestusruumi konto teave.

5. Valige võtme ikoon. See kuvab selle konto salvestusruumi klahvid.

### <a name="how-do-i-access-blob-storage"></a>Kuidas pääseb juurde bloobimälu

Kui Hadoopi käsk klaster, kuni on mitmel viisil plekid juurdepääsu.

* [Mac-arvutisse, Linux ja Windows Azure'i CLI](../xplat-cli-install.md): käsurea liides käske Azure'i töötamiseks. Pärast installimist, kasutage funktsiooni `azure storage` käsk salvestusruumi, kasutamise kohta abi saamiseks või `azure blob` bloobimälu kohased käsud.

* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python skript plekid Azure Storage töötamiseks.

* Erinevate SDK-d:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET-I](https://github.com/Azure/azure-sdk-for-net)

* [Salvestusruumi REST API-ga](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Skaleerimist klaster

Funktsioon skaleerimist klaster võimaldab andmete sõlmed kobar, kus töötab Windows Azure Hdinsightiga ilma kustutada ja uuesti luua klaster kasutatavat arvu muutmine.

Saate skaleerimise toiminguid ajal muud tööd teha või protsessid töötavad klaster.

Kobar eri tüüpi mõjutab skaleerimist järgmiselt:

* __Hadoopi__: kui skaala sõlmed klaster arvu teatud teenuste klaster uuesti. See võib põhjustada töö töötab või ootel ebaõnnestuda skaleerimise toimingu sooritamist. Pärast selle toimingu lõpuleviimist, saate uuesti tööd.

* __HBase__: piirkondlike serverid on automaatselt tasakaalustatud on mõne minuti pärast skaleerimise toimingu sooritamist. Käsitsi jääk piirkondliku serverid, tehke järgmist:

    1. Ühenduse Hdinsightiga klaster SSH abil. Hdinsightiga SSH kasutamise kohta lisateabe saamiseks leiate järgmised dokumendid.

        * [Kasutada SSH Hdinsightiga Linux, Unix ja Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Kasutada SSH Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. HBase shell alustamiseks kasutage järgmist:

            hbase shell

    2. Kui HBase shell on laaditud, kasutage järgmist tasakaalustamiseks käsitsi piirkondliku serverid:

            balancer

* __Torm__: kõik töötab Storm topoloogiatest peaks taastub, pärast skaleerimise toimingut sooritatud. See võimaldab topoloogia korrigeerida paralleelsus sätted vastavalt sõlmed klaster uus number. Taastub töötava topoloogiatest, kasutage ühte järgmistest suvanditest:

    * __SSH__: serveriga ja kasutada taastub topoloogia järgmine käsk:

            storm rebalance TOPOLOGYNAME

        Saate määrata ka parameetrite alistamiseks paralleelsus näpunäiteid topoloogia juurde. Näiteks `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` on taaskonfigureerimine topoloogia 5 töötaja protsesside, 3 haldavad isikud sinine – tila komponendi ja 10 haldavad isikud kollane polt komponendi.

    * __Torm UI__: taastub topoloogia Storm Kasutajaliidese abil tehke järgmist.

        1. Avage veebibrauseris, kus CLUSTERNAME on klaster tormi nimi __https://CLUSTERNAME.azurehdinsight.net/stormui__ . Kui kuvatakse vastav viip, sisestage Hdinsightiga kobar administraator (admin) nimi ja parool, mis on määratud klaster loomisel.

        3. Valige topoloogia, mida soovite taastub, valige nupp __taastub__ . Sisestage viivituse enne hooldus toimingu.

Skaleerimist klaster Hdinsightiga kohta täpsema teabe saamiseks vt

* [Hadoopi kogumite rakenduses Hdinsightiga abil Azure portaali haldamine](hdinsight-administer-use-portal-linux.md#scaling)

* [Hadoopi kogumite rakenduses Hdinsightiga haldamine Azure PowerShelli abil](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Kuidas installida tooni (või muude Hadoopi komponent)?

Hdinsightiga on hallatav teenus, mis tähendab, et sõlmed klaster võib hävitatud ja reprovisioned automaatselt Azure kui probleem ei leitud. Seetõttu pole soovitatav on käsitsi installida asju otse kobar sõlmed. Selle asemel kasutada [Hdinsightiga skripti toiminguid](hdinsight-hadoop-customize-cluster.md) , kui peate installima järgmist:

* Teenuse või veebisait, nt säde või Värvitoon.
* Osa, mis nõuab konfiguratsioonimuudatuste mitme sõlmed klaster. Näiteks keskkond, mis on nõutav muutuja loomise logimine kataloogi või konfigureerimise faili loomine.

Skripti toimingud on Bash skripte, mis on parandusfunktsiooni ajal kobar ettevalmistamise ja saab installida ja konfigureerida lisakomponentide klaster. Installimist järgmised komponendid on esitatud näide skriptide.

* [Tooni](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solri](hdinsight-hadoop-solr-install-linux.md)

Oma skripti tegevusi Lisateavet [skripti toimingu arendamiseni Hdinsightiga](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>Jar failid

Kompaktsed jar failid, mis on sisaldavad funktsioone kasutada MapReduce töö või kaudu on toodud mõned Hadoopi tehnoloogiad siga või taru sees. Samal ajal, kui neid saab installida skripti toimingute kasutamine, nad sageli ei nõua mis tahes häälestamise ja saab lihtsalt pärast ettevalmistamise klaster üles laadida ja kasutada otse. Kui soovite makle kindel komponent survives reimaging, klaster, saate talletada WASB jar-faili.

Näiteks, kui soovite kasutada [DataFu](http://datafu.incubator.apache.org/)uusim versioon, saate purki, mis sisaldab projekti allalaadimine ja üleslaadimine Hdinsightiga kobar. Seejärel järgige DataFu dokumentatsiooni kohta, kuidas seda kasutada siga või taru.

> [AZURE.IMPORTANT] Teatud komponendid, mis on eraldi jar failid on varustatud Hdinsightiga, kuid ei tee. Kui otsite kindla osa, saate jälgida otsimiseks klõpsake klaster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Tagastab kõik kattuvad jar failid tee.

Kui klaster pakub juba versiooni komponent jar eraldi failina, kuid soovite kasutada mõnda muud versiooni, saate üles laadida uue versiooni komponent klaster ja proovige oma tööd.

> [AZURE.WARNING] Koos Hdinsightiga kobar komponendid on täielikult toetatud ja Microsoft Support aitab eristada ja nende komponentide seotud probleemide lahendamiseks.
>
> Kohandatud komponendid tugiteenuseid äriliselt mõistlik aidata teil selle probleemi tõrkeotsingu sooritamiseks. See võib põhjustada probleemi lahendamine või palub teil otsimist ja avatud allika tehnoloogiad, kui leitakse sügav teadmised selle tehnoloogia. Näiteks on palju kogukonnafoorumi saite, mida saab kasutada, nt: [MSDN-i Foorum Hdinsightiga](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ka Apache projektide on projekti saitidel [http://apache.org](http://apache.org), näiteks: [Hadoopi](http://hadoop.apache.org/), [säde](http://spark.apache.org/).

## <a name="next-steps"></a>Järgmised sammud

* [Windowsi-põhiste hdinsightist migreerimine Linux-põhine](hdinsight-migrate-from-windows-to-linux.md)
* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)
* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
* [Hdinsightiga MapReduce töö kasutamine](hdinsight-use-mapreduce.md)
