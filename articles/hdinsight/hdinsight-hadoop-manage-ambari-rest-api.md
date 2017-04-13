<properties
   pageTitle="Jälgimine ja haldamine Hdinsightiga kogumite Apache Ambari REST API abil | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Ambari jälgimine ja haldamine Linux-põhine Hdinsightiga kogumite. Selles dokumendis saate teada, kuidas kasutada Ambari REST API Hdinsightiga kogumite kaasas."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Ambari REST API abil Hdinsightiga kogumite haldamine

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari lihtsustab haldamise ja järelevalve Hadoopi kobar, pakkudes lihtne kasutada kasutajaliides web ja REST API-ga. Ambari lisatud Linux-põhine Hdinsightiga kogumite ja kasutatakse jälgida klaster ja konfiguratsiooni muuta. Selles dokumendis õpite põhitõdesid töötab Ambari REST API-ga, kasutades cURL levinud ülesandeid täitvate.

##<a name="prerequisites"></a>Eeltingimused

* [cURL](http://curl.haxx.se/): cURL on mitu platvormi kasuliku kasutatavate REST API-de käsurea töötamiseks. Selles dokumendis seda kasutatakse suhtlemiseks Ambari REST API-ga.
* [JQ](https://stedolan.github.io/jq/): jq on mitu platvormi käsurea kasuliku JSON dokumentidega töötamiseks. Selles dokumendis seda kasutatakse sõeluda Ambari REST API tagastatud JSON dokumendid.
* [Azure'i CLI](../xplat-cli-install.md): platvormidel käsurea kasuliku Azure'i teenuste töötamiseks.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Mis on Ambari?

[Apache Ambari](http://ambari.apache.org) muudab Hadoopi haldus on lihtne kasutada web UI, mida saab kasutada ette, hallata ja jälgida Hadoopi kogumite pakkudes lihtsamaks. Arendajad saate neid võimalusi integreerida nende rakenduste abil [Ambari REST API-d](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari on esitatud Linux-põhine Hdinsightiga kogumite vaikimisi.

##<a name="rest-api"></a>REST API-GA

Base URI Ambari REST API Hdinsightiga on https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, kus __CLUSTERNAME__ on klaster nime.

> [AZURE.IMPORTANT] Kuigi kobar nime URI (CLUSTERNAME.azurehdinsight.net) osa täielik domeeninimi (FQDN) nimi on väiketähed, muude sündmuste URI on tõstutundlikud. Näiteks kui klaster nimega MyCluster, Järgnevalt on lubatud URI-d.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Järgmised URI-d tagastavad vea, kuna teine esinemiskorra nimi pole õige nii.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Klõpsake Hdinsightiga Ambari ühendamine nõuab HTTPS. Kui ühendus autentimine, peate kasutama administraatori konto nimi (vaikimisi __administraator__) ja registreerimisel klaster on loodud parool.

Järgmine näide kasutab cURL GET-päringu vastu REST API-ga. Asendage klaster administraatori parool __parool__ . Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Vastus on JSON dokument, mis algab järgmine teave:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Kuna see on JSON, on hõlpsam JSON-parseri kasutamine andmete. Näiteks järgmine näide kasutab jq kuvada ainult soovitud `health_report` element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Näide: Saada kobar sõlmed FQDN

Töötades Hdinsightiga, peate teadma kobar sõlm täielik domeeninimi (FQDN). Saate tuua hõlpsalt FQDN erinevad sõlmed klaster, kasutades järgmist:

* __Juhi sõlmed__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Töötaja sõlmed__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper sõlmed__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Teate, et iga nendes näidetes järgivad sama:

1. Päringu komponent, mille soovime teate, et need sõlmed töötab.
2. Saate tuua selle `host_name` elemente, mis sisaldavad sõlmed FQDN.

Funktsiooni `host_components` saatja dokumendi osa sisaldab mitu üksust. Kasutades `.host_components[]`, ja määrate siis tee jooksul elemendi pidev iga üksuse läbi ja tõmmake teatud tee väärtuse. Kui soovite ainult ühe väärtuse, näiteks FQDN esimese kirje, saate tagastada üksuste kogumina ja seejärel valige konkreetse kirje.

    jq '[.host_components[].HostRoles.host_name][0]'

See tagastab esimese FQDN kogumisest.

##<a name="example-get-the-default-storage-account-and-container"></a>Näide: Saan salvestusruumi vaikekonto ja container

Mõne Hdinsightiga kobar loomisel peate kasutama konto Azure salvestusruumi ja bloobimälu container vaikimisi salvestamise klaster. Ambari abil saate pärast loomist klaster seda teavet alla laadida. Näiteks, kui soovite programmiliselt kirjutamise andmed otse ümbris.

Kogumite vaikimisi salvestusruumi WASB URI toob järgmist:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] See tagastab esimese konfiguratsiooni rakendatud server (`service_config_version=1`,) mis sisaldab seda teavet. Kui toote väärtus, mis on muudetud pärast kobar loomist, peate loendi konfiguratsiooni versioonid ja viimati toomiseks.

See tagastab väärtuse sarnane järgmises näites, kus __CONTAINER__ on vaike-ümbrisest ja __ACCOUNT_NAME__ on Azure Storage konto nimi:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Seejärel saate seda teavet [Azure'i CLI](../xplat-cli-install.md) üleslaadimiseks või allalaadimiseks andmete ümbrisest.

1. Hankige ressursirühma salvestusruumi kontole. Salvestusruumi konto nime vormilt Ambari __ACCOUNT_NAME__ asendamiseks tehke järgmist.

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    See tagastab konto ressursi rühma nime.
    
    > [AZURE.NOTE] Kui midagi on tagastatud selle käsu, peate muuta Azure'i CLI Azure'i ressursihaldur režiimi ja käivitage käsk uuesti. Azure'i ressursihaldur lülitumine, kasutage järgmist käsku:
    >
    > `azure config mode arm`
    
2. Saada salvestusruumi konto võti. Asendage __GROUPNAME__ eelmises juhises ressursirühma. Salvestusruumi konto nime __ACCOUNT_NAME__ asendamiseks tehke järgmist.

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Selles näites tagastatakse konto primaarvõti.
    
3. Laadi käsu abil saate talletada faili ümbrises:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Asendage __ACCOUNT_NAME__ salvestusruumi konto nimi. Asendage __ACCOUNTKEY__ tuua varem võti. __FILEPATH__ on soovite üles laadida, kui __BLOBPATH__ on tee ümbrises faili tee.

    Näiteks, kui soovite kuvada Hdinsightiga veebisaidil wasbs://example/data/filename.txt faili, siis __BLOBPATH__ oleks `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Näide: Update Ambari konfigureerimine

1. Saada praeguse konfiguratsiooni, mis salvestab Ambari "soovitud konfiguratsioon":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Selles näites tagastab klaster installitud komponendid JSON dokument, mis sisaldab konfiguratsioonile (tähistatud _sildi_ väärtus). Järgmises näites on tagastatud säde kobar tüüpi andmeid väljavõte.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Selles loendis, peate kopeerimiseks komponendi nimi (nt __säde\_ökonoomsus\_sparkconf__ ja __sildi__ väärtus.
    
2. Järgmise käsu abil saate tuua konfiguratsiooni osa ja silt. Asendage __säde-ökonoomsus-sparkconf__ ja __algse__ osa ja silt, mida soovite tuua konfiguratsiooni.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Curl toob JSON dokumenti ja seejärel jq kasutatakse andmeid muuta, et malli loomine. Malli kasutatakse siis konfiguratsiooni väärtuste lisamine või muutmine. Konkreetselt teeb järgmist.
    
    * Loob kordumatu väärtus, mis sisaldavad stringi "versioon" ja kuupäev, mis on talletatud __newtag__.
    * Loob uue soovitud konfiguratsiooni dokumendi juurkausta.
    * Sisu saab selle `.items[]` massiiv ja lisab selle jaotise __desired_config__ element.
    * Kustutab __href__, __versioon__või __Config__ elemente, esitada uue konfiguratsiooni pole vajadusel järgmisi elemente.
    * Lisab uue __sildi__ elemendi ja määrab väärtusega __versioon ##__. Arvuline osa aluseks on praeguse kuupäeva. Iga konfiguratsiooni peab olema kordumatu sildi.
    
    Lõpuks andmed on salvestatud __newconfig.json__ dokumenti. Dokumendi struktuuris peaks kuvatama sarnane järgmises näites:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Avage __newconfig.json__ dokument ja muuta/lisada väärtused objekti __Atribuudid__ . Järgmises näites muutub __"spark.yarn.am.memory"__ väärtuse __"1 g"__ __"3 g"__ ja, ja lisab uue elemendi jaoks __"spark.kryoserializer.buffer.max"__ väärtusega __"256 m"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Kui olete muudatuste tegemisega lõpule jõudnud, salvestage fail.

4. Järgmise käsu abil saate esitada Ambari värskendatud konfiguratsioon.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    See käsk torud curl taotluse esitatava kobar uue soovitud konfiguratsiooni __newconfig.json__ faili sisu. CURL pärimiseks JSON dokumendi. Selles dokumendis __versionTag__ elemendi peaksid olema samad saatsite versioon ja __configs__ objekt sisaldab konfiguratsiooni muudatusi ei saanud.

###<a name="example-restart-a-service-component"></a>Näide: Taaskäivitage teenuste osa

Sel hetkel, kui vaatate Ambari web UI, säde teenuse näitab tuleb taaskäivitada, enne kui saate uue konfiguratsiooni muudatuste jõustumiseks. Järgmiste juhiste abil saate teenust uuesti.

1. Kasutada järgmisi hooldus režiimis säde teenuse lubamiseks.

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    See käsk saadab JSON dokumendi server (sisalduvad funktsiooni `echo` lause) mis lülitab sisse hooldus režiimis.
    Saate kontrollida, et teenus on nüüd hooldus režiimis järgmised päringuga.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    See väärtus tagastab `"ON"`.

3. Järgmiseks kasutage järgmist teenuse väljalülitamiseks:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    See käsk tagastab vastuse sarnaneb järgmisega.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Funktsiooni `href` selle URI poolt tagastatud väärtus on kasutusel kobar sõlm sisemise IP-aadress. Asendage kasutamiseks selle väljaspool klaster klaster FQDN '10.0.0.18:8080' osa. Näiteks järgmine käsk toob taotluse olekut.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Kui see väärtus tagastab `"COMPLETED"` seejärel kutse on lõpule jõudnud.

4. Kui eelmine taotlus on lõpule jõudnud, kasutage järgmisi teenuse käivitamine.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Pärast teenuse taaskäivitamist on uute konfiguratsioonisätete.

5. Lõpuks hooldustööd režiimi väljalülitamine järgmine abil.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Järgmised sammud

Täieliku viite REST API-ga, vt [Ambari API viide V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Mõned funktsioonid Ambari on keelatud, nagu haldab Hdinsightiga pilveteenuses; näiteks lisamine või eemaldamine klaster hosts või lisada uusi teenuseid.
