<properties
   pageTitle="Andur analüüside Apache Storm ja HBase | Microsoft Azure'i"
   description="Saate teada, kuidas Apache torm virtuaalse võrguga ühenduse. Abil saate Storm HBase protsessi andurite andmeid mõne sündmuse keskuse kaudu ja visualiseerida D3.js."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Andur analüüside Apache Storm, sündmuse jaoturi ja HBase sisse Hdinsightiga (Hadoopi) 

Saate teada, kuidas kasutada Apache Storm Hdinsightiga protsessi andurite andmeid Azure'i sündmuse keskuse kaudu, säilitage Apache HBase Hdinsightiga sisse ja visualiseerida D3.js töötab Azure Web App abil.

Selles dokumendis kasutatava malli Azure'i ressursihaldur näitab, kuidas luua mitme Azure'i ressursid ressursirühma. Täpsemalt, loob see on Azure virtuaalse võrgu, kaks Hdinsightiga kogumite (Storm ja HBase) ja Azure Web App. Node.js rakendamine reaalajas web armatuurlaua automaatselt juurutatakse web appi.

> [AZURE.NOTE] Teave selle dokumendi ja antud näites on testitud Linux-põhine Hdinsightiga 3.3 ja 3.4 kobar versioonide abil.

## <a name="prerequisites"></a>Eeltingimused

* Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Teil pole vaja olemasoleva Hdinsightiga kobar; juhised selle dokumendi loob järgmistest allikatest:
    >
    > * Muu Azure virtuaalse kaudu
    > * Tormi kohta Hdinsightiga kobar (Linuxi põhine 2 töötaja sõlmed)
    > * Mõne HBase kohta Hdinsightiga kobar (Linuxi põhine 2 töötaja sõlmed)
    > * Azure'i Web App, mis hostib web armatuurlaud

* [Node.js](http://nodejs.org/): seda kasutatakse eelvaate web armatuurlaual kohalikult oma arenduskeskkond.

* [Java ja JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): torm topoloogia välja.

* [Maven](http://maven.apache.org/what-is-maven.html): saab koostada ja projekti koostamine.

* [Git](http://git-scm.com/): kasutada projekti allalaadimiseks GitHub.

* __SSH__ kliendi: ühendamiseks Linux-põhine Hdinsightiga rühmad. Hdinsightiga SSH kasutamise kohta leiate lisateavet teemast järgmised dokumendid.

    * [Windows kliendi Hdinsightiga SSH kasutamine](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH kasutamine Hdinsightiga kliendilt Linux, Unix või Mac-arvutisse](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Peate ka juurdepääsu soovitud `scp` käsk, mida kasutatakse teie kohaliku arenduskeskkond ja Hdinsightiga klaster SSH abil failide kopeerimiseks.

## <a name="architecture"></a>Arhitektuur

![arhitektuur skeem](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Selles näites koosneb järgmistest osadest.

* **Azure'i sündmuse jaoturi**: sisaldab andmeid, mis on kogutud andurid. Selles näites on rakenduse eeldusel, et genereeritud andmed.

* **Torm Hdinsightiga kohta**: andmete reaalajas töötlemine pakub sündmuse keskuse kaudu.

* **Klõpsake Hdinsightiga HBase**: püsivate NoSQL andmesalve pakub andmete pärast tormi töötluse.

* **Azure'i virtuaalse võrguteenuse**: võimaldab turvaline tormi kohta Hdinsightiga ja HBase kohta Hdinsightiga kogumite vahelise andmevahetuse.

    > [AZURE.NOTE] Virtuaalse võrk on nõutav kasutada Java HBase kliendi API, nagu see on avatud, üle avaliku lüüsi HBase kogumite jaoks. Installimise HBase ja torm kogumite virtuaalse samasse võrku võimaldab Storm kobar (või mõni muu virtuaalse võrgus, süsteem) otse juurdepääsemiseks HBase kliendi API abil.

* **Armatuurlaua veebisaidi**: näidisarmatuurlaual, et andmete reaalajas diagrammid.

    * Veebisaidi rakendatakse Node.js, et seda saab käivitada kliendi opsüsteeme testimiseks või seda saab kasutada Azure veebisaitide.

    * [Socket.IO](http://socket.io/) kasutatakse reaalajas suhtlemine Storm topoloogia ja veebisaidi jaoks.

        > [AZURE.NOTE] See on ka rakendamise üksikasjad. Saate kasutada mis tahes side raamistik, nt töötlemata WebSockets või SignalR.

    * [D3.js](http://d3js.org/) kasutatakse põhinema andmeid, mis saadetakse veebisaidile.

> [AZURE.IMPORTANT] Kahe kogumite on nõutav, ei ole toetatud meetodit torm nii HBase ühe Hdinsightiga kobar loomiseks.

Topoloogia loeb andmeid sündmuse keskuse kaudu, kasutades [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) klassi ja kirjutab andmete HBase [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) klassi abil. Suhtlus veebisaidi saab teha [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java)abil.

Topoloogia skeem on järgmine.

![topoloogia skeem](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] See on väga lihtsustatud vaade topoloogia. Käitusajal, näiteks iga osa luuakse iga sektsiooni sündmuse jaoturi, mis on lugeda. Need eksemplarid levitatakse üle sõlmed klaster ja andmeid marsruuditakse nende vahel järgmiselt:
>
> * Funktsiooni parseri tila andmetega on koormus tasakaalustatud.
> * Andmed on parseri armatuurlaua ja HBase on rühmitatud seadme ID-d nii, et sõnumid samasse seadmesse flow alati sama osa.

### <a name="topology-components"></a>Topoloogia komponendid

* **EventHub tila**: tila on Apache Storm versioon 0.10.0 esitatud ja uuem versioon.

    > [AZURE.NOTE] Selles näites kasutatakse sündmuse jaoturi tila nõuab torm Hdinsightiga kobar versioon 3.3 või 3.4. Sündmuse jaoturi tormi kohta Hdinsightiga vanema versiooniga kasutamise kohta leiate teemast [protsessi sündmuste Azure'i sündmuse jaoturi torm Hdinsightiga kohta](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: andmed, mida väljastab tila on töötlemata JSON ja aeg-ajalt rohkem kui üks sündmus on kiiratav korraga. See polt demonstreerib kiiratava tila andmete lugemine ja eraldavad andmevoo uue nimega korteeži, mis sisaldab mitme välja.

* **DashboardBolt.java**: see näitab, kuidas kasutada Socket.io kliendi teek Java saatmiseks andmete reaalajas web armatuurlaud.

Selles näites kasutatakse [Flux](https://storm.apache.org/releases/0.10.0/flux.html) raames, et topoloogia määratlus sisaldub YAML failid. On kaks.

* __ei – hbase.yaml__ – kasutage seda faili testimisel topoloogia oma arenduskeskkond. See ei kasuta HBase komponendid, kuna te ei pääse HBase Java API kaudu väljaspool virtuaalse võrgu klaster elu sisse.

* __koos hbase.yaml__ – kasutage seda faili topoloogia juurutamisel Storm kobar. Kuna see töötab HBase kobar virtuaalse samasse võrku kasutab HBase komponendid.

## <a name="prepare-your-environment"></a>Keskkonna ettevalmistamiseks

Enne selle tegemiseks peate looma Azure'i sündmuse jaoturi, mis loeb Storm topoloogia.

### <a name="configure-event-hub"></a>Sündmuse jaoturi konfigureerimine

Sündmuse jaoturi on selles näites andmeallikas. Järgmiste juhiste abil saate luua uue sündmuse jaoturi.

1. [Azure'i portaalis](https://portal.azure.com)valige **+ Uus** -> __Asjade Interneti__ -> __Sündmuse jaoturi__.

2. Enne __Namespace luua__ , tehke järgmist.

    1. Sisestage __nimi__ nimeruumi.
    2. Valige hinnakirjad taseme. __Tavaline__ piisab selles näites.
    3. Valige Azure __tellimuse__ kasutada.
    4. Valige ressursi olemasolevasse rühma või looge uus.
    5. Valige sündmuse jaoturi __asukoht__ .
    6. Valige __armatuurlaud Kinnita__ja seejärel klõpsake nuppu __Loo__.

3. Loomisprotsessi on lõpule jõudnud, kuvatakse teie nimeruumi sündmuse jaoturi tera. Valige avanevas __+ Lisa sündmus jaoturi__. Enne __Sündmuse keskuse loomine__ , sisestage nimi __sensordata__ ja seejärel valige __Loo__. Jätke muude väljadel sätte vaikeväärtust.

4. Valige oma nimeruum keelest sündmuse jaoturi __Sündmuse jaoturi__. Valige __sensordata__ kirje.

5. Valige sündmuse jaoturi sensordata keelest __jagatud juurdepääsupoliitikaid__. __+ Lisa__ link abil saate lisada järgmised poliitikad:


  	| Poliitika nimi | Nõuded |
  	| ----- | ----- |
  	| seadmetes | Saatmine |
  	| torm | Kuulake |

5. Valige mõlema poliitika ja kirjutage __PRIMAARVÕTME__ väärtus. Peate väärtus nii poliitikate tulevaste juhiseid.

## <a name="download-and-configure-the-project"></a>Laadige alla ja projekti konfigureerimine

Kasutage järgmist GitHub projekti alla.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Kui käsk on lõpule jõudnud, on teil järgmised kataloogi struktuuri:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Selle dokumendi sisestage üksikasjad koodi kaasatud selle näidis; siiski koodi täielikult kommenteerinud.

Avage fail **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** ja järgmised read oma sündmuse jaoturi teabe lisamine:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] Selles näites eeldab __torm__ sihitute poliitika, mis sisaldab __kuulata__ taotluste nime ja oma sündmuse jaoturi nimega __sensordata__.

 Salvestage fail pärast selle teabe lisada.

## <a name="compile-and-test-locally"></a>Koostada ja testida kohalikult

Enne testimist, peate käivitama armatuurlaua kuvamiseks topoloogia väljund ja andmete talletamiseks sündmuse keskuse loomiseks.

> [AZURE.IMPORTANT] See topoloogia HBase osa ei ole aktiivne katsetamiseks HBase kobar Java API-ga, ei pääse väljaspool Azure virtuaalse võrku, mis sisaldab rühmad.

### <a name="start-the-web-application"></a>Veebirakenduse käivitamine

1. Avage uus Käsuviip või terminalis ja muuta kataloogide **hdinsightiga eventhub/näidisarmatuurlaual**ja seejärel järgmise käsu abil saate installida sõltuvuste nõutud veebirakenduse.

        npm install

2. Veebirakenduse alustamiseks kasutage järgmist käsku:

        node server.js

    Peaksite nägema järgmine teade:

        Server listening at port 3000

2. Avage veebibrauser ja sisestage **http://localhost:3000 /** aadressina. Peaksite nägema Järgmine leht:

    ![Web armatuurlaud](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Jätke selle Käsuviip või terminalis open. Pärast katsetamine, kasutage klahvikombinatsiooni Ctrl + c. peatamiseks veebiserver.

### <a name="start-generating-data"></a>Genereeriv andmed

> [AZURE.NOTE] Selle jaotise juhised kasutada Node.js nii, et neid saab kasutada mis tahes platvormi. Muu keele näiteid leiate **SendEvents** kataloogi.

1. Avage uus Käsuviip, shell või terminalis ja muuta kataloogide **hdinsightiga eventhub – näiteks/SendEvents/nodejs**, siis sõltuvuste nõutud rakenduse installimiseks kasutada järgmine käsk:

        npm install

2. Avage **app.js** fail tekstiredaktoris ja lisage hankisite varem sündmuse jaoturi teave:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] Selles näites eeldab, et kasutamist __sensordata__ nimeks sündmuse jaoturi ja __seadmete__ poliitika, mis on __saata__ taotluste nimeks.

2. Kasutage sündmuse keskuses uusi kirjeid lisada järgmine käsk:

        node app.js

    Peaksite nägema väljundi mitu rida, mis sisaldavad andmeid, mis on saadetud sündmuse jaoturi. Need kuvatakse järgmine:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Käivitage topoloogia

2. Avage uus Käsuviip, shell või __Hdinsightiga eventhub näide/TemperatureMonitor__terminal ja muutke kataloogid ja topoloogia käivitage järgmine käsk abil.

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Kui kasutate PowerShelli, kasutage selle asemel järgmist:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Kui on Linux-Unixi-OS X süsteemi ja on [installitud torm oma arenduskeskkond](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), saate järgmised käsud:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Käivitub topoloogia, mis on määratletud __ei-hbase.yaml__ faili kohaliku režiimis. __Dev.properties__ failis sisalduvate väärtuste sisestage sündmuse jaoturi ühenduse teave. Käivitatud topoloogia loeb kirjed sündmuse keskuse kaudu ja neile saadetakse teie kohalikus arvutis töötab armatuurlaud. Peaksite nägema read kuvatakse web armatuurlaual järgmine:

    ![armatuurlaua andmetega](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Armatuurlaua töötamise kasutada funktsiooni `node app.js` käsk eelmisi juhiseid uute andmete saatmiseks sündmuse jaoturi kaudu. Kuna temperatuur väärtused on CEIP genereeritud, värskendama graafik temperatuur suurte muutuste näitamiseks.

    > [AZURE.NOTE] Peate olema __Hdinsightiga eventhub – näiteks/SendEvents/Nodejs__ kataloogis kasutamisel on `node app.js` käsk.

3. Pärast kontrollimist, et see toimib, peatage topoloogia, kasutades klahvikombinatsiooni Ctrl + C. Saate peatada kohaliku veebiserver ka klahvikombinatsiooni Ctrl + C.

## <a name="create-a-storm-and-hbase-cluster"></a>Looge torm ja HBase kobar

Selleks, et käivitada topoloogia Hdinsightiga ja HBase polt lubamiseks peate looma uue Storm kobar ja HBase kobar. Selle jaotise juhised [Azure'i ressursihaldur malli](../resource-group-template-deploy.md) kasutamiseks luua uus Azure virtuaalse võrgu ja torm ja HBase kobar virtuaalse võrgus. Malli ka Azure Web App loob ja kasutab seda armatuurlaud koopia.

> [AZURE.NOTE] Virtuaalse võrgu kasutatakse nii, et töötavate Storm kobar topoloogia saate otse suhelda HBase klaster HBase Java API abil.

Ressursihaldur malli selles dokumendis asub avaliku bloobimälu ümbrises veebisaidil __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Azure'i sisse logida ja avage Mall, ressursihaldur Azure'i portaalis järgmist nuppu.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Keelest **Parameetrid** sisestage järgmine:

    ![Hdinsightiga parameetrid](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: seda väärtust kasutatakse torm base nimeks ja HBase kogumite. Näiteks sisestamise __hdi__ loob nimega __torm-hdi__ Storm klaster ning HBase kobar, mis nimega __hbase-hdi__.
    * __CLUSTERLOGINUSERNAME__: torm ja HBase rühmad administraatori kasutajanime.
    * __CLUSTERLOGINPASSWORD__: administraator kasutaja parooli torm ja HBase rühmad.
    * __SSHUSERNAME__: The SSH kasutaja torm ja HBase rühmad luua.
    * __SSHPASSWORD__: torm ja HBase rühmad SSH kasutaja parooli.
    * __Asukoht__: piirkond, mis on loodud rühmad.
    
    Klõpsake nuppu parameetrid salvestamiseks nuppu __OK__ .
    
3. __Ressursirühm__ jaotise abil saate luua uue ressursirühma või valige olemasoleva.

4. Valige __Ressursi rühma asukoht__ rippmenüü, kui valitud __asukoha__ parameetri samasse asukohta.

5. Valige __õiguslikult__ja seejärel nuppu __Loo__.

6. Lõpetuseks, märkige ruut __Kinnita armatuurlaua__ ja seejärel valige __Loo__. Rühmad luua kulub umbes 20 minutit.

Kui ressursid on loodud, suunatakse höövlitera ressursirühm, mis sisaldab kogumite ja web armatuurlaud.

![Ressursi rühma blade vnet ja kogumite](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Märkate, et nimede Hdinsightiga rühmad on __Torm-BASENAME__ ja __hbase-BASENAME__, kus BASENAME on andsite malli nimi. Kasutage nende nimed hiljem toimingutes ühendamisel rühmad. Samuti võtke arvesse, et armatuurlaua saidi nimi on __basename-armatuurlaud__. Kasutage seda hiljem armatuurlaud kuvamisel.

## <a name="configure-the-dashboard-bolt"></a>Armatuurlaua polt konfigureerimine

Selleks, et saata andmed juurutatud web app armatuurlaud, peate muutma __dev.properties__ faili järgmine rida:

    dashboard.uri: http://localhost:3000

Muuda `http://localhost:3000` et `http://BASENAME-dashboard.azurewebsites.net` ja salvestage fail. Asendage __BASENAME__ andsite eelmises etapis base nimi. Saate kasutada ka armatuurlaua ja vaatamiseks URL-i varem loodud ressursirühma.

## <a name="create-the-hbase-table"></a>HBase tabeli loomine

Andmete talletamiseks HBase, saame tuleb esmalt luua tabeli. Üldiselt soovite eelnevalt luua ressursse, mis torm tuleb kirjutada, kui proovite luua ressursside sees torm topoloogia võib põhjustada mitme, jaotatud eksemplari loomise sama ressurss kood. Ressursside väljaspool topoloogia loomine ja kasutada lihtsalt Storm lugemis- ja analüüsi jaoks.

1. Kasutada SSH ühenduse HBase klaster SSH kasutaja ja parooli esitatud malli kobar loomise ajal. Näiteks kui abil ühendamine funktsiooni `ssh` käsku, saate kasutada järgmist süntaksit:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    Asendage selle käsu loomisel kobar ja __BASENAME__ andsite base nimega sisestatud SSH kasutajanimi __kasutajanimi__ . Küsimise korral sisestage parool SSH kasutaja jaoks.

2. SSH seansi, käivitage HBase shell.

        hbase shell
    
    Kui kest on laaditud, näete mõne `hbase(main):001:0>` küsimus.

3. HBase kest, sisestage järgmine käsk andur andmete talletamiseks tabeli loomiseks.

        create 'SensorData', 'cf'

4. Veenduge, et tabel on loodud, kasutades järgmine käsk:

        scan 'SensorData'
        
    See tagastab teabe umbes järgmine näide, mis näitab, et tabelis on 0 rida.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Sisestage HBase shell sulgemiseks järgmist:

        exit

## <a name="configure-the-hbase-bolt"></a>HBase polt konfigureerimine

Kirjutamise HBase torm kobar, peate sisestama HBase polt HBase klaster konfiguratsiooni andmed. Lihtsaim viis selleks on alla laadida __hbase-site.xml__ klaster ja lisada selle oma projekti. Samuti peate mitu sõltuvused __pom.xml__ faili, mis laadida torm-hbase osa ja sõltuvuste nõutud kommenteerige välja.

> [AZURE.IMPORTANT] Peate alla laadima torm-hbase.jar faili kohta oma Storm klõpsake Hdinsightiga kobar 3.3 või 3.4 kobar; See versioon on koostatud töötamiseks HBase 1.1.x, mida kasutatakse HBase Hdinsightiga 3.3 ja 3.4 kogumite. Kui kasutate torm-hbase komponent mujalt, võib tuleb koostada vastu HBase varasem versioon.

### <a name="download-the-hbase-sitexml"></a>Laadige alla hbase-site.xml

Käsuviip, kasutada SCP __hbase-site.xml__ faili alla laadida klaster. Järgmises näites, asendage SSH kasutaja loomisel kobar ja __BASENAME__ base nimega andsite varem sisestatud __kasutajanimi__ . Küsimise korral sisestage parool SSH kasutaja jaoks. Asendada selle `/path/to/TemperatureMonitor/resources/hbase-site.xml` koos TemperatureMonitor projekti selle faili tee.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

See laadib __hbase-site.xml__ määratud tee.

### <a name="download-and-install-the-storm-hbase-component"></a>Laadige alla ja installige torm-hbase komponent

1. Käsuviip, kasutada SCP __torm-hbase.jar__ faili alla laadida Storm kobar. Järgmises näites, asendage SSH kasutaja loomisel kobar ja __BASENAME__ base nimega andsite varem sisestatud __kasutajanimi__ . Küsimise korral sisestage parool SSH kasutaja jaoks.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Seda alla laadida faili nimega `storm-hbase-####.jar`, kus ## on see torm versiooninumber. Märkige see arv, kui seda kasutatakse hiljem.

2. Järgmine käsk abil saate installida selle osa kohaliku Maven hoidlasse sisse oma arenduskeskkond. See võimaldab Maven leidmiseks paketi projekti koostamisel. Asendage __####__ numbriga versioon sisaldab faili nime.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Kui kasutate PowerShelli, kasutage järgmist käsku:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Torm-hbase komponent projekti lubamine

1. Avage fail __TemperatureMonitor/pom.xml__ ja kustutage järgmised read.

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Kustutada ainult need kaks rida; Kustutage kõik read nende vahel.
    
    See võimaldab mitu komponendid, mis on vaja suhtlemisel HBase hbase poldi abil.

2. Järgmised read otsida ja asendada __####__ torm-hbase faili alla laadida varasem versioon arvu.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Versiooninumber peab vastama kasutasite installimisel komponendi kohaliku Maven hoidla, nagu Maven kasutab seda teavet komponendi laadida, kui projekt hoone versioon.

2. Salvestage fail __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Koostage, paketi ja Juurutage lahendus Hdinsightiga

Kasutage oma arenduskeskkond juurutada Storm topoloogia torm kobar toimige järgmiselt.

1. Kataloogist __TemperatureMonitor__ kasutada uue koostamine ja projekti JAR paketi loomine järgmine käsk:

        mvn clean compile package

    See loob fail nimega **TemperatureMonitor-1,0-SNAPSHOT.jar** projekti kataloogis **Target (sihtkoht)** .

2. Kasutada scp klaster Storm __TemperatureMonitor-1,0-SNAPSHOT.jar__ fail üles. Järgmises näites, asendage SSH kasutaja loomisel kobar ja __BASENAME__ base nimega andsite varem sisestatud __kasutajanimi__ . Küsimise korral sisestage parool SSH kasutaja jaoks.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] See võib kuluda mitu minutit üles laadida faili, kui see on mitu megabaiti.

    Kasutage scp faili üles laadida __dev.properties__ , kuna see sisaldab teavet, mida kasutatakse ühenduse sündmuse jaoturi ja armatuurlaud.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Kui failid on üles laaditud, ühenduse klaster SSH abil.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. SSH seansi, kasutage alustamiseks topoloogia järgmine käsk.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Käivitub topoloogia __dev.properties__ faili topoloogia määratluse faili __koos hbase.yaml__ ja konfiguratsiooni väärtuste kasutamine.

3. Pärast topoloogia on hakanud, avage brauser avaldatud Azure veebisaiti, siis kasutage funktsiooni `node app.js` käsk sündmuse jaoturi andmeid saata. Peaksite nägema web armatuurlaud teabe kuvamiseks värskendada.

    ![Armatuurlaua](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase andmete kuvamine

Kui olete saatnud topoloogia abil andmete `node app.js`, ühenduse HBase ja veenduge, et andmed on kirjutatud mõnes varasemas versioonis loodud tabeli järgmiste juhiste abil.

1. Ühenduse loomine HBase kobar SSH abil.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. SSH seansi, käivitage HBase shell.

        hbase shell
    
    Kui kest on laaditud, näete mõne `hbase(main):001:0>` küsimus.

2. Tabeli ridade kuvamine:

        scan 'SensorData'
        
    See peaks tagastama teavet sarnaneb järgmise, mis näitab, et tabelis on 0 rida.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Selle toimingu skannimine tulemuseks ainult kuni 10 ridade tabelist.

## <a name="delete-your-clusters"></a>Teie kogumite kustutamine

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Kogumite, salvestusruumi ja veebirakenduse korraga kustutamiseks Kustuta ressursirühm, mis sisaldab neid.

## <a name="next-steps"></a>Järgmised sammud

Nüüd saate õppinud, kuidas kasutada Storm sündmuse jaoturi andmeid lugeda, talletage see HBase sisse ja visualiseerida teave on väliste armatuurlaual Socket.io ja D3.js abil.

* Rohkem näiteid Storm topoloogiatest koos Hdinsightile järgmistest teemadest.

    * [Näide topoloogiatest Storm Hdinsightiga kohta](hdinsight-storm-example-topology.md)

* Apache Storm kohta leiate lisateavet teemast [Apache Storm](https://storm.incubator.apache.org/) saidile.

* HBase Hdinsightiga kohta leiate lisateavet teemast [HBase Hdinsightiga ülevaade](hdinsight-hbase-overview.md).

* Socket.io kohta leiate lisateavet teemast [socket.io](http://socket.io/) saidile.

* D3.js kohta leiate lisateavet teemast [D3.js - andmete juhitud dokumendid](http://d3js.org/).

* Java topoloogiatest loomise kohta leiate teavet teemast [arendamise Java topoloogiatest Apache Storm Hdinsightiga kohta](hdinsight-storm-develop-java-topology.md).

* .NET topoloogiatest loomise kohta leiate teavet teemast [arendamise C# topoloogiatest Apache Storm Hdinsightiga Visual Studio abil sisse](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
