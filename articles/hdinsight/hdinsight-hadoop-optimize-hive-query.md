<properties
   pageTitle="Kiirem täitmist Hdinsightile oma taru päringuid optimeerimine | Microsoft Azure'i"
   description="Saate teada, kuidas optimeerida oma taru päringuid Hadoopi Hdinsightiga sisse."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Hadoopi rakenduses Hdinsightiga taru päringute optimeerimine

Vaikimisi on optimeeritud Hadoopi kogumite jõudluse. Selles artiklis antakse ülevaade mõne enamlevinud taru jõudluse optimeerimine viise, mida saate rakendada meie päringud.

##<a name="scale-out-worker-nodes"></a>Töötaja sõlmed välja skaala

Töötaja sõlmed klaster arvu saate kasutada rohkem mappers ja pärast paralleelselt käivitamiseks. On kaks võimalust, saate suurendada skaala Hdinsightiga välja:

- Sätte ajal saate määrata töötaja sõlmed kasutades Azure portaali, Azure PowerShelli või mitu platvormi käsurea liides arv.  Lisateabe saamiseks vt [sätte Hdinsightiga kogumite](hdinsight-provision-clusters.md). Järgneval pildil Kuva töötaja sõlm konfiguratsioon Azure'i portaalis:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Käitusajal, saate ka muudate klaster uuesti ühte loomata. See on näidatud allpool.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Hdinsightiga ei toeta erinevate virtuaalmasinates kohta leiate lisateavet teemast [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Tez lubamine

[Apache Tez](http://hortonworks.com/hadoop/tez/) on alternatiivseid Andmetäite mootor MapReduce mootori.

![tez_1][image-hdi-optimize-hive-tez_1]


Tez on kiirem, sest:

- Käivitada suunatud atsüklilised graafik (DAG) ühe tööd MapReduce mootor, DAG, mis on väljendatud nõuab iga kriteeriumikogum mappers järgneb pärast kogumit. See põhjustab mitme MapReduce töö olla kedratud iga taru päringu. Tez on selline piirang ja saab töödelda keerukate DAG ühe tööd minimeerida töö käivitus pea kohal.
- **Mittevajalike välditakse kirjutab** Mitme töö on kedratud sama taru päringu MapReduce mootor, kuna iga töö väljund kirjutatakse HDFS vahe andmete jaoks. Kuna Tez minimeeritakse iga taru päringu töökohtade arv on võimalik, et vältida mittevajalike kirjutamine.
- **Minimeerib käivitamis viivitused** Tez suudab paremini mappers alustamiseks läheb vaja arvu ja ka parandades optimeerimine kogu käivitamise viivitus minimeerimiseks.
- **Ümbriste valikut** Kui on võimalik Tez korduvkasutamine ümbriste tagamaks, et latentsuse tõttu ümbriste käivitamine on vähendatud.
- **Pidev optimeerimise abil** Traditsiooniliselt tehtud optimeerimine koostamise ajal. Kuid rohkem teavet sisendeid on saadaval, mis lubavad parem optimeerimine ajal runtime. Tez kasutab pidev optimeerimise abil, mis võimaldab optimeerimine soovitud lepingule käitusaja etappi.

Need kohta lisateabe saamiseks klõpsake [siin](http://hortonworks.com/hadoop/tez/)

Mis tahes taru päringu Tez lubanud eesliidete lisamine päringu alltoodud sätte abil saate teha.

    set hive.execution.engine=tez;

Windowsi-põhiste Hdinsightiga kogumite, Tez lubatud sätte ajal. Proovi Azure PowerShelli skripti ettevalmistamine Hadoopi kobar koos Tez lubatud on järgmine:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux-põhine Hdinsightiga kogumite on Tez vaikimisi sisse lülitatud.
    

## <a name="hive-partitioning"></a>Taru eraldamine

I/O toiming on peamised jõudluse kitsaskoht töötab taru päringud. Kui andmeid, mis peab olema lugemiseks saab vähendada parandada jõudlust. Vaikimisi taru päringute skannige kogu taru tabelid. See on suurepärane päringuid, nagu tabeli skannib, kuid päringuid, mis on vaja ainult skannida andmeid (nt päringud filtreerimine) väike, see loob mittevajalike pea kohal. Taru eraldamine võimaldab taru päringute juurde pääseda ainult vajalikus taru tabeli andmeid.

Taru eraldamine rakendatakse korraldate töötlemata andmete uue kataloogid iga sektsiooni on eraldi directory - kus sektsiooni on kasutaja määratletud. Järgmine diagramm näitab eraldamine taru tabeli veeru *aasta*alusel. Iga-aastane luuakse uus kaust.

![jagamine][image-hdi-optimize-hive-partitioning_1]

Mõned eraldamine asjaoluga:

- **Tehke mitte all partition** - eraldamine veergude ainult mõne väärtusega, võivad põhjustada väga vähe sektsioonid. Näiteks eraldamine soo luua ainult kahe sektsioonid (mees ja naine) luua, seega ainult vähendada latentsus kuni poole.

- **Üle partition** - teises otsas, klõpsake loomise sektsiooni kordumatu väärtus (nt kasutajanimi) veerus põhjustab mitu sektsiooni põhjustab palju rõhku kobar namenode, kuna see on käsitlema kataloogide suurt hulka.

- **Vältida andmete skew** – nii, et kõik partitsioonid on paaris suurus eraldamine võtme hoolikas valimine. Näide on eraldamine sisse *olekusse* võib põhjustada all olevat peaaegu 30 Californias kirjete arvu x, et Vermont populatsiooni vahe tõttu.

Sektsiooni tabeli loomiseks kasutada klausel *Liigendatud* :

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Kui sektsioonitud tabeli on loodud, saate luua staatilise eraldamine või dünaamiline eraldamine.

- **Staatilise eraldamine** tähendab, et teil on juba sharded andmete asjakohane kataloogid ja paluda taru sektsioonid käsitsi directory asukoha alusel. See on esitatud allpool koodilõigu.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dünaamiline eraldamine** tähendab, et soovite taru sektsioonide loomiseks automaatselt teie eest. Kuna me juba loonud eraldamine tabeli vahekausta tabelist, kõik läheb vaja teha on lisada sektsioonitud tabeli andmed, nagu allpool näidatud:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Lisateavet leiate teemast [Liigendatud tabelist](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Kasutage ORCFile vormingut

Taru toetab failivormingutes. Näiteks:

- **Teksti**: see on vaikefailivorming ja töötab enamik stsenaariumid
- **Avro**: sobib hästi interoperability stsenaariumid
- **ORC/parkett**: kõige paremini jõudlus

ORC (optimeeritud rea piklik) vorming on väga tõhus viis taru andmete talletamiseks. ORC võrreldes mõnes muus vormingus, on järgmised eelised:

- keerukate tüübid, sh kuupäeva ja kellaaja ja keerukate ja poolstruktuurandmed tüüpi tugi
- kuni 70% tihendamine
- registrid iga 10 000 rida, mis võimaldavad ridade vahelejätmine
- oluline vähenemine käitusaja täitmise

ORC vorming lubamiseks saate luua esmalt tabeli klausliga *Salvestatud ORC nimega*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Järgmiseks sisestate andmed tabelisse ORC lavastus tabelist. Näiteks:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Lugege lisateavet ORC Vorming klõpsake [siin](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization võimaldab taru töödelda paketi 1024 read koos töötlemise ühe rea haaval asemel. See tähendab, et lihtsad toimingud on kiiremini teha, kuna vähem sisemise koodi käivitada.

Lubamiseks vectorization eesliide oma taru päringu järgmised sätted:

    set hive.vectorized.execution.enabled = true;

Lisateavet leiate teemast [Vectorized päringu täitmise](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Muud võimalused optimeerimine

On veel optimeerimine viise, mida võite kaaluda, näiteks:

- **Taru bucketing:** meetod, mis võimaldab klaster või segmendi suurte määrab andmete päringu jõudluse optimeerimiseks.
- **Liitumine optimeerimine:** optimeerimine taru 's päringu täitmise plaanis tõhustada ühendused ja kasutajale näpunäiteid vähendamiseks. Lisateavet leiate teemast [liitumine optimeerimine](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **Pärast suurendamine**

##<a id="nextsteps"></a>Järgmised sammud
Selles artiklis on õppinud mitu levinud taru päringu optimeerimine võimalust. Lisateabe saamiseks lugege järgmisi artikleid:

- [Kasutage Apache taru sisse Hdinsightiga](hdinsight-use-hive.md)
- [Abil rakenduses Hdinsightiga taru flight viivitus andmete analüüsiks](hdinsight-analyze-flight-delay-data.md)
- [Abil rakenduses Hdinsightiga taru Twitter andmete analüüsiks](hdinsight-analyze-twitter-data.md)
- [Taru päringu konsooli kasutamine rakenduses Hdinsightiga Hadoopi andur andmete analüüsiks](hdinsight-hive-analyze-sensor-data.md)
- [Logide veebisaitidel analüüsimiseks Hdinsightiga taru kasutamine](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
