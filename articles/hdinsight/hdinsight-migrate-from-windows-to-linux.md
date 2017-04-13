<properties
pageTitle="Linux-põhine Hdinsightiga hdinsightist Windowsi-põhiste migreerimine | Azure'i"
description="Saate teada, kuidas migreerimiseks Windowsi-põhiste Hdinsightiga kobar Linux-põhine Hdinsightiga kobar."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Windowsi-põhiste Hdinsightiga kobar migreerida Linux-põhine kobar

Ajal Windowsi-põhiste Hdinsightiga abil on lihtne kasutada Hadoopi pilves, võite avastada, et teil on vaja Linuxi-põhiste kobar ära tööriistad ja teie lahendus jaoks vajalike. Hadoopi ökosüsteemis paljud asjad on välja töötatud Linuxi-põhiste süsteemide ja mõned ei pruugi olla saadaval jaoks Windowsi-põhiste Hdinsightiga. Lisaks palju raamatuid, videoid ja muud koolitusmaterjalid endale, et kasutate Linuxi Hadoopi töötamisel.

Selles dokumendis on toodud üksikasjad Windows ja Linux Hdinsightiga ja juhised, kuidas migreerida olemasoleva töökoormus Linux-põhine arvutikobaras erinevuste kohta.

> [AZURE.NOTE] Hdinsightiga kogumite Ubuntu pikaajaline tugiteenuste kasutamine (LTS) operatsioonisüsteemi sõlmed klaster. Hdinsightiga, Saadaval Ubuntu versiooni koos teiste osade versioonimise teavet leiate artiklist [Hdinsightiga komponent versioonid](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migreerimisjärgsete toimingute tegemine

Migreerimise üldine töövoog on järgmine.

![Migreerimise töövoo joonis](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Lugege selle dokumendi muudatused, mida võib olla vaja, kui saate olemasoleva töövoo, töö jne migreerimine Linux-põhine kobar mõista iga jaotist.

2.  Looge Linux-põhine kobar testi ja kvaliteedi assurance keskkonnas. Linux-põhine kobar loomise kohta leiate lisateavet teemast [loomine Linux-põhine kogumite Hdinsightiga sisse](hdinsight-hadoop-provision-linux-clusters.md).

3.  Olemasolevate projektide, andmeallikate ja valamud kopeerida uude keskkonda. Vt lisateavet jaotisse testi keskkonna andmete kopeerimine.

4.  Testida valideerimine veenduge, et teie töö õigesti töötada uus klaster.

Kui te kontrollida, kas kõik toimib ootuspäraselt, ajakava tööseisakute migreerimise jaoks. See tööseisakute ajal teha järgmisi toiminguid.

1.  Salvestatud kohalikult kobar sõlmed siirdamiseks andmed varundada. Näiteks, kui teil on pea sõlme talletatud andmed.

2.  Windowsi-põhiste kobar kustutada.

3.  Saate luua Linux-põhine kobar, kasutades sama vaikimisi andmesalve Windowsi-põhiste kobar kasutatud. See võimaldab uue kobar jätkata tööd oma olemasoleva tootmisandmed suhtes.

4.  Mis tahes varundatud siirdamiseks andmeid importida.

5.  Alusta tööd/jätkata töötlemine abil uue kobar.

### <a name="copy-data-to-the-test-environment"></a>Andmete kopeerimine testi keskkonnas

On palju võimalusi Kopeerige andmed ja -tööde haldamine, aga kaks selles jaotises kirjeldatud on lihtsam meetodid otse failide teisaldamine testi kobar.

#### <a name="hdfs-dfs-copy"></a>HDFS DFS kopeerimine

Hadoopi HDFS käsu abil saate otse andmete kopeerimine talletamist jaoks olemasoleva tootmise klaster salvestusruumi jaoks uus test klaster täitke järgmised juhised.

1. Teave salvestusruumi konto ja vaikimisi container olemasoleva klaster jaoks. Saate seda teha järgmised Azure PowerShelli skripti abil.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Järgige Hdinsightiga dokumendi luua uue testimiskeskkonnas rühmades luua Linux-põhine. Enne loomist klaster lõpetada ja valige selle asemel **Valikuline konfigureerimine**.

3. Valige keelest valikuline konfigureerimine **Lingitud salvestusruumi kontod**.

4. Valige **Lisa salvestusruumi klahvi**ja vastava viiba kuvamisel valige salvestusruumi konto tagastatud PowerShelli skripti juhises 1. **Valige** klõpsake iga tera neid sulgeda. Lisaks saate luua klaster.

5. Kui klaster on loodud, ühendage see kasutades **SSH.** Kui olete tuttav Hdinsightiga SSH abil, vt ühte järgmistest artiklitest.

    * [SSH kasutamine Linux-põhine Hdinsightiga Windowsi kliendid](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH kasutamine Linux-põhine Hdinsightiga Linux, Unix ja Mac kliendid](hdinsight-hadoop-linux-use-ssh-unix.md)

6. SSH seansi, kasutage lingitud salvestusruumi konto failide kopeerimine uue salvestusruumi vaikekonto järgmine käsk. Asendage CONTAINER ja konto container ja konto teabe tagastatud PowerShelli skripti juhises 1. Asendage tee andmed andmete faili tee.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Kui test keskkond pole kataloogi struktuuri, mis sisaldab andmeid, saate luua järgmise käsu abil.

        hdfs dfs -mkdir -p /new/path/to/create

    Funktsiooni `-p` vahetamine võimaldab luua kõik kataloogid tee.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Otsest Kopeeri Azure Storage plekid vahel

Teise võimalusena võite kasutada funktsiooni `Start-AzureStorageBlobCopy` Azure PowerShelli cmdleti plekid väljaspool Hdinsightiga salvestusruumi kontode vahel kopeerida. Lisateavet leiate teemast Azure plekid Azure PowerShelli kasutamine Azure Storage osas haldamiseks.

##<a name="client-side-technologies"></a>Kliendipoolne tehnoloogiad

Üldiselt kliendipoolne tehnoloogiad nagu [Azure PowerShelli cmdletid](../powershell-install-configure.md), [Azure'i CLI](../xplat-cli-install.md) või [.NET SDK Hadoopi](https://hadoopsdk.codeplex.com/) jätkab tööd sama Linux-põhine kogumite, nagu nad toetuvad REST API-d, mis on sama nii kobar OS tüüpide lõikes.

##<a name="server-side-technologies"></a>Serveripoolne tehnoloogiad

Migreerimine serveripoolne komponendid, mis on Windows teatud annab järgmise tabeli juhiseid.

| Kui kasutate selle tehnoloogia... | Seda toimingut teha … |
| ----- | ----- |
| **PowerShelli** (serveripoolne skriptide, sh skripti toiminguid kasutatakse kobar loomise ajal) | Kui Bash skripte kirjutada. Skripti toimingud leiate [kohandamine Linux-põhine Hdinsightiga skripti toimingud](hdinsight-hadoop-customize-cluster-linux.md) ja [skripti toimingu arengu Linuxi-põhine Hdinsightiga](hdinsight-hadoop-script-actions-linux.md). |
| **Azure'i CLI** (Serveripoolsed skriptid) | Azure'i CLI on saadaval Linuxi, see ei tule eelinstallitud Hdinsightiga kobar pea sõlmed. Kui vajate serveripoolne skriptimine, [installige Azure'i CLI](../xplat-cli-install.md) lisateabe saamiseks vt platvormide Linux-põhine installimise kohta. |
| **.Net-i komponendid** | .NET ei toeta täielikult Linux-põhine Hdinsightiga kogumite. Linux-põhine Storm klõpsake Hdinsightile kogumite loodud pärast 2017/10/28 tugi C# Storm topoloogiatest abil SCP.NET raames. Täiendavate tugiteenuste .net-i lisatakse tulevikus värskendusi. |
| **Win32 komponendid või muu ainult Windowsi tehnoloogia** | Juhised sõltub osa või tehnoloogia; teil on võimalik, et leida versioon, mis ühildub Linux või peate võib-olla alternatiivse lahenduse või kirjutada selle osa. |

##<a name="cluster-creation"></a>Kobar loomine

Sellest jaotisest leiate teavet kobar loomine erinevuste kohta.

### <a name="ssh-user"></a>SSH kasutaja

Linux-põhine Hdinsightiga kogumite kasutavad Kaugpöördusteenuse kobar sõlmi **Secure Shell (SSH)** protokolli. Erinevalt Remote'i töölaua jaoks Windowsi-põhiste kogumite, enamik SSH kliendid ei paku graafiline kasutusvõimalused, kuid selle asemel pakub käsurea, mis võimaldab käivitada käsud klaster. Osa kliente (nt [MobaXterm](http://mobaxterm.mobatek.net/),) pakuvad graafiline fail süsteemi brauseri Lisaks mõne käsurea remote.

Kobar loomise ajal peab võimaldama SSH rakendust ja kas **parooli** või **avaliku võtme serdi** autentimist.

Soovitame kasutada avaliku võtme serdi, nagu oleks turvalisem parooliga. Serdi autentimist töötab genereerimine allkirjastatud era/avalik võti paari ja siis esitada avalik võti klaster loomisel. SSH abil serveriga klient privaatvõti pakub ühenduse autentimist.

Hdinsightiga SSH kasutamise kohta leiate lisateavet teemast:

- [Windowsi klientide Hdinsightiga SSH kasutamine](hdinsight-hadoop-linux-use-ssh-windows.md)

- [SSH kasutamine Hdinsightiga Linux, Unix ja OS X kliendid](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Kobar kohandamine

**Skripti toiminguid** kasutatakse Linux-põhine kogumite peab olema kirjutatud Bash skripti. Ajal skripti toimingute abil saab kobar loomise ajal jaoks Linux-põhine kogumite tegemist võib olla ka kasutatud teha kohandus pärast klaster on loodud ja töötab. Lisateabe saamiseks vt [kohandamine Linux-põhine Hdinsightiga skripti toimingud](hdinsight-hadoop-customize-cluster-linux.md) ja [skripti toimingu arengu Linuxi-põhine Hdinsightiga](hdinsight-hadoop-script-actions-linux.md).

Teine kohandamise funktsioon on **bootstrap**. Windowsi kogumite, see võimaldab teil määrata asukoha teeke kasutamiseks koos taru. Pärast kobar loomist, on need teegid automaatselt saab kasutada koos taru päringud, ilma et oleks vaja kasutada `ADD JAR`.

Alglaaduri jaoks Linux-põhine kogumite ei näe seda funktsiooni. Selle asemel kasutada skripti toimingu dokumenteerida [lisamine taru teekide kobar loomise ajal](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuaalne võrkude

Windowsi-põhiste Hdinsightiga kogumite ainult töötamine klassikaline virtuaalse võrke, samas Linuxi-põhiste Hdinsightiga kogumite nõuda ressursihaldur virtuaalne võrkude. Kui teil on ressursid klassikaline virtuaalse võrgus, mis tuleb ühendada Linux-Hdinsightiga kobar, vt [ühenduse klassikaline virtuaalse võrgu ressursihaldur Virtual võrku](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Konfiguratsiooni nõuded Azure virtuaalse võrkude kasutamist Hdinsightiga kohta leiate lisateavet teemast [laiendamine Hdinsightiga võimaluste virtuaalse võrgu kaudu](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Haldus ja jälgimine

Paljud web UIs olete kasutanud Windowsi-põhiste Hdinsightiga, nt varasem töökogemus või lõng UI, kus on saadaval Ambari kaudu. Lisaks Ambari taru vaate võimaldab taru päringuid veebibrauseri kaudu käivitada. Ambari Web UI on saadaval veebisaidil https://CLUSTERNAME.azurehdinsight.net kogumite Linux-põhine.

Ambari töötamise kohta leiate lisateavet teemast järgmised dokumendid:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API-ga](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari teatised

Ambari on teatiste süsteemi, mis võivad teile klaster võimalikud probleemid. Teatiste kuvatakse kollane või punane kirjete Ambari Web UI, kuid te saate alla laadida ka need REST API kaudu.

> [AZURE.IMPORTANT] Ambari teatised näitavad, et seal *võib* olla probleem, pole see seal *on* . Näiteks võidakse kuvada teatise, et HiveServer2 ei pääse juurde, ehkki sellele juurde pääsevad tavalisel viisil.
>
> Mitme teatiste rakendatakse nimega intervall vastavalt päringute teenuse vastu ja oodata vastust teatud aja jooksul. Nii, et teatise ei tähenda, et teenus on alla, lihtsalt, et see ei annaks tulemuste oodatud aja jooksul.

Üldiselt tuleks hinnata, kas teatise on toimunud pikema perioodi peegeldab kasutaja probleemid, mis on varem teatatud klaster enne selle toimingu või.

##<a name="file-system-locations"></a>Süsteemi failiasukohtade

Linuxi kobar failisüsteemi on Windowsi-põhiste Hdinsightiga kogumite teisiti sätestatud. Otsi levinud failide järgmise tabeli abil.

| Mul on vaja leida... | See asub... |
| ----- | ----- |
| Konfigureerimine | `/etc`. Näiteks`/etc/hadoop/conf/core-site.xml` |
| Logifailide | `/var/logs` |
| Hortonworks andmete platvormi (HDP) | `/usr/hdp`. On kaks kataloogide asub siin, üks, mis on HDP praegune versioon (nt `2.2.9.1-1`,) ja `current`. Funktsiooni `current` kataloogi viitu failid ja kataloogid, mis asub kaustas versioon sisaldab ja on mugav juurdepääs alates versioon HDP failide arv muutub HDP versioon on uuendatud. |
| Hadoopi-streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Üldiselt kui teate faili nime, saate järgmine käsk SSH seanss faili tee leidmiseks:

    find / -name FILENAME 2>/dev/null

Samuti saate metamärkide abil faili nime. Näiteks `find / -name *streaming*.jar 2>/dev/null` tee naasmiseks mis tahes jar failid, mis sisaldavad sõna streaming failinime osana.

##<a name="hive-pig-and-mapreduce"></a>Taru, siga ja MapReduce

Siga ja MapReduce töökoormus on väga sarnased Linux-põhine kogumite kohta - peamine erinevus kui kaugtöölaua abil saate ühendada Windowsi-põhiste kobar ja tööde käitamine kasutatavat SSH kogumite Linux-põhine.

- [Kasutage siga SSH](hdinsight-hadoop-use-pig-ssh.md)

- [Kasutage MapReduce koos SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Taru

Järgmine tabel annab juhiseid migreerimine oma taru töökoormus.

| Klõpsake Windowsi-põhiste, ma kasutan... | Linux põhisesse... |
| ----- | ----- |
| **Taru redaktor** | [Taru Ambari vaates](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Kui soovite lubada Tez | Tez on vaikimisi täitmise mootori jaoks Linux-põhine kogumite, määramine lause ei ole enam vaja. |
| CMD failid või skripte kui taru töö osa serveris | Bash skriptide kasutamine |
| `hive`kaugtöölaua käsk | Kasutage [Linnulennutee](hdinsight-hadoop-use-hive-beeline.md) või [taru SSH seanss](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Torm

| Klõpsake Windowsi-põhiste, ma kasutan... | Linux põhisesse... |
| ----- | ----- |
| Torm armatuurlaud | Armatuurlaua torm pole saadaval. Viiside esitada topoloogiatest [Deploy ja hallata Storm topoloogiatest Linux-põhine Hdinsightiga kohta](hdinsight-storm-deploy-monitor-topology-linux.md) vt |
| Torm kasutajaliides | Torm UI on saadaval veebisaidil https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio abil luua, juurutada ja hallata C# või hübriid topoloogiatest | Visual Studio saab luua, juurutada ja hallata C# (SCP.NET) või hübriid topoloogiatest Linux-põhine tormi Hdinsightiga kogumite, mis on loodud pärast 2017/10/28 kohta. |

##<a name="hbase"></a>HBase

Klõpsake Linux-põhine kogumite, znode HBase puudub `/hbase-unsecure`. Peate seadma selle mõne Java kliendi konfiguratsiooni rakendusi, mis kasutavad kohalikke HBase Java API-ga.

Vt [koostada Java-põhine HBase rakenduse](hdinsight-hbase-build-java-maven.md) näide klient, mis määrab selle väärtuse.

##<a name="spark"></a>Säde

Säde kogumite on saadaval opsüsteemi Windows – kogumite eelvaate; siiski väljaande, säde on saadaval ainult selliste kogumite Linux-põhine. On pole migreerimise tee Windowsi-põhiste säde eelvaade kobar väljaanne Linux-põhine säde kobar.

##<a name="known-issues"></a>Teadaolevad probleemid

### <a name="azure-data-factory-custom-net-activities"></a>Azure'i andmed Factory kohandatud .NET tegevuste

Azure'i andmed Factory kohandatud .NET tegevuste Linux-põhine Hdinsightiga kogumite praegu ei toetata. Selle asemel saab kasutada üht järgmistest meetoditest rakendada kohandatud tegevuste osana oma ADF kohaletoimetamisel.

-   Azure'i paketi rakenduskausta .NET tegevuste käivitamine Vaadake jaotist kasutamine Azure'i paketi lingitud teenuse [kasutamise kohandatud](../data-factory/data-factory-use-custom-activities.md#AzureBatch) tegevuste on Azure andmete Factory müügivõimaluste

-   Rakendada tegevuse MapReduce tegevus. Lisateabe saamiseks vaadake [Andmete Factory autonoomsest MapReduce programmides](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Rea lõpud

Üldiselt kasutada rea lõpud Windowsi-põhiste süsteemide CRLF, Linuxi-põhiste jaoks kasutada LF. Kui aedvili või oodata, andmete CRLF rea lõpud, peate muuta tootjate ja tarbijate töötamiseks LF rea lõpus.

Näiteks Azure PowerShelli abil päringu Hdinsightiga Windowsi-põhiste klaster tagastada CRLF andmeid. Tagastab sama päring koos Linux-põhine kobar LF. Paljudel juhtudel see järjekord oluline andmete tarbija, kuid tuleb uurida enne Linux-põhine kobar migreerimine.

Kui teil on skripte, mis käivitatakse otse sisse Linux-kobar sõlmed (nt Python skript taru või MapReduce töö), tuleks alati kasutate LF rea lõpus. Kui kasutate CRLF, võidakse kuvada tõrkeid, kui skriptid Linux-põhine klaster.

Kui teate, et skriptide ei sisalda stringe manustatud CR märki, saate muuta joone lõpud, kasutades ühte järgmistest:

-   **Kui teil on skriptide, mille kavatsete klaster üleslaadimise kohta**, järgmistest PowerShelli abil saate muuta joone lõpud CRLF LF enne üleslaadimist skripti klaster.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Kui teil on, mis on juba kasutatav klaster salvestusruum skriptide**, saate Linux-põhine kobar SSH seanss järgmine käsk muuta skripti.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Järgmised sammud

-   [Saate teada, kuidas Linux-põhine Hdinsightiga kogumite loomine](hdinsight-hadoop-provision-linux-clusters.md)

-   [Ühenduse loomine Windowsi kliendi kaudu SSH Linuxi-põhiste arvutikobaras](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Ühenduse loomine Linux-põhine kobar, kasutades SSH kliendi Linux, Unix või Mac-arvutisse](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Linux-põhine kobar, kasutades Ambari haldamine](hdinsight-hadoop-manage-ambari.md)
