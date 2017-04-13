<properties
   pageTitle="Töötamine taru kohta Hdinsightiga (Hadoopi) Linnulennutee abil | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada SSH ühenduse loomine Hadoopi kobar Hdinsightiga sisse ja seejärel interaktiivseks esitada taru päringute Linnulennutee abil. Linnulennutee on kasuliku üle JDBC HiveServer2 töötamiseks."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Hadoopi rakenduses Hdinsightiga Linnulennutee taru kasutamine

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Sellest artiklist saate teada, kuidas kasutada Secure Shell (SSH) ühenduse Linux-põhine Hdinsightiga kobar ja seejärel interaktiivseks esitada taru päringute [Linnulennutee](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) käsurea tööriista abil.

> [AZURE.NOTE] Linnulennutee kasutab JDBC taru loomiseks. Taru JDBC kasutamise kohta lisateabe saamiseks vt [ühenduse loomine taru taru JDBC draiveri kasutamine Windows Azure Hdinsightiga kohta](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Mõne Linuxi-põhiste Hadoopi Hdinsightiga kobar.

* SSH kliendi. Linux, Unix ja Mac OS-is peaks tulema SSH kliendi. Windowsi kasutajad peab klient, nt [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)alla laadida.

##<a id="ssh"></a>Kasutajaga SSH

Ühenduse täielik domeeninimi (FQDN), Hdinsightiga klaster SSH käsu abil. FQDN saab teie andsite kobar, siis **. azurehdinsight.net**. Näiteks järgmine soovite ühendada nimega **myhdinsight**klaster.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Kui sertifikaadi võti SSH autentimise** Hdinsightiga kobar loomisel, võib-olla peate määrama privaatvõti asukoha kliendi arvutisse:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Kui SSH autentimise parooli** loomisel Hdinsightiga kobar, peate sisestama parooli küsimise.

Hdinsightiga SSH kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Linux, OS X-ja Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitt (Windowsi-põhiste kliendid)

Windows ei paku sisseehitatud SSH klient. Soovitame kasutada **kitt**, mille saate alla laadida [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Käsuga Linnulennutee

1. Kui ühendus on loodud, kasutage Linnulennutee alustamiseks järgmist:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    See käivitage Linnulennutee klient ja ühenduse JDBC URL-i. Siin `localhost` kasutatakse, kuna HiveServer2 töötab nii pea sõlme klaster ning meil töötab Linnulennutee otse sisse esmane headnode.
    
    Kui käsk on lõpule jõudnud, jõuate mõne `jdbc:hive2://localhost:10001/>` küsimus.

3. Tavaliselt algavad Linnulennutee käsud on `!` märk, näiteks `!help` kuvab spikker. Samas on `!` sageli välja. Näiteks `help` töötab ka.

    Kui vaatate abi, te teate `!sql`, mida kasutatakse käivitada HiveQL laused. Siiski HiveQL on nii levinud, et saate jätate eelmise `!sql`. Järgmised kaks laused on täpselt sama tulemuse; kus on kuvatud tabelid taru kaudu praegu saadaval:
    
        !sql show tables;
        show tables;
    
    Uus klaster, loetletakse ainult ühest tabelist: __hivesampletable__.

4. Järgmine abil saate kuvada soovitud hivesampletable skeemi:

        describe hivesampletable;
        
    Tagastab järgmine teave:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Kuvatakse tabelis veerud. Kuigi me võib teha mõned päringud andmete suhtes, hoopis täiesti uue tabeli loomine demonstreerib taru andmete laadimine ja rakendamine skeem.
    
5. Sisestage nimega **log4jLogs** koos Hdinsightiga kobar Näidisandmete abil uue tabeli loomiseks järgmistest:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Need teatised järgmisi toiminguid:

    * **Tabeli DROP** - kustutab tabeli ja selle andmefaili juhuks, kui tabel on juba olemas.
    * **Välise tabeli loomine** – loob uue tabeli "väline" taru. Välise tabeleid talletada ainult taru tabeli määratlus. Andmed on jäänud algsesse asukohta.
    * **Rea vorming** – taru saate teada, kuidas andmed on vormindatud. Sel juhul iga log väljad on eraldatud ruumi.
    * **Salvestatud AS TEKSTIFAILIDE asukoht** – ütleb taru, kus andmed on salvestatud (nt/andmete kataloogi) ja see, et tekstina.
    * **Valige** - valib kõik read, kus veerus **t4** sisaldab väärtust **[ERROR]**arv. See peaks tagastama väärtus **3** , kui palju on kolm rida, mis sisaldavad seda väärtust.
    * **INPUT__FILE__NAME nagu "%.log"** - kirjeldatakse, mida me ainult tagasi andmete failidest, mille lõpus on taru. log. Tavaliselt ainult teil andmeid sama skeemi samas kaustas kui päringud taru, aga selles näites logifaili on salvestatud andmeid mõnes muus vormingus.

    > [AZURE.NOTE] Kui eeldate alusandmete värskendada välise andmeallikaga, nagu on automaatne andmete üleslaadimine või mõne muu MapReduce toiming, kuid tahate alati taru päringute uusimaid andmeid kasutada tuleks kasutada välise tabeleid.
    >
    > Kas pukseerimine on Välistabel **ei** Kustuta andmed tabeli määratlus.
    
    Selle käsu väljund peaks olema umbes järgmine:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Linnulennutee väljumiseks kasutada `!quit`.

##<a id="file"></a>Käivitage HiveQL faili

Linnulennutee saate kasutada ka faili, mis sisaldab HiveQL laused käivitamiseks. Järgmiste juhiste abil saate faili loomine ja seejärel käivitage see Linnulennutee abil.

1. Järgmise käsu abil saate luua uus fail nimega __query.hql__:

        nano query.hql
        
2. Kui redaktori avab, kasutage järgmist faili sisu. See päring on **errorLogs**nimega "sisemine" uue tabeli loomine

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Need teatised järgmisi toiminguid:

    * **Looge tabel kui pole olemas** - loob tabeli, kui see pole juba olemas. Kuna **väline** märksõna ei kasutata, see on sisemine tabel, mis on talletatud taru andmebaas ja hallatakse täielikult taru.
    * **Salvestatud AS ORC** - salvestab andmed optimeeritud rea piklikku (ORC) vormingus. See on väga optimeeritud ja tõhusa vorming taru andmete talletamiseks.
    * **Lisa kirjutada... Valige** - klõpsab ridade **log4jLogs** tabeli, mis sisaldavad **[ERROR]**, seejärel lisab andmed tabelisse **errorLogs** .
    
    > [AZURE.NOTE] Erinevalt välise tabeleid, pukseerimine on sisemine tabel kustutatakse ka aluseks olevad andmed.
    
3. Faili salvestamiseks kasutage __klahvikombinatsiooni Ctrl__+ ___X__, seejärel sisestage __Y__ja lõpuks __Enter__.

4. Järgmine abil saate käivitada faili, kasutades Linnulennutee. Asendage nimi, mis on saadud varem pea sõlme ja __parooli__ administraatori konto parool __hostname (HOSTINIMI)__ :

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] Funktsiooni `-i` parameeter käivitab Linnulennutee, töötab teksti query.hql faili ja jääb Linnulennutee veebisaidil on `jdbc:hive2://localhost:10001/>` küsimus. Samuti saate käivitada faili, kasutades funktsiooni `-f` parameeter, mis tagastab Bash fail on töödeldud.

5. Veenduge, et **errorLogs** tabel on loodud, abil järgmine lause tulu **errorLogs**kõiki ridu.

        SELECT * from errorLogs;

    Kolm andmeread tuleks tagastada, kõik sisaldavad **[ERROR]** veeru t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Lisateavet leiate teemast Linnulennutee Ühenduvus

Juhised selle dokumendi kasutamine `localhost` ühenduse HiveServer2 kobar headnode töötab. Saate kasutada ka hostname või täielik domeeninimi on headnode vaja need täiendavad juhised protsess (juhiseid, et leida hostname või FQDN). Kasutades `localhost` piisab Linnulennutee kasutamisel on headnode.

Kui teil on mõni serva sõlm klaster, Linnulennutee installinud, peate ühenduse hostname (hostinimi) või selle headnode FQDN kasutada.

Kui teil on Linnulennutee väljaspool klaster kliendi installitud, saate ühendada järgmise käsu abil. Asendage __CLUSTERNAME__ klaster Hdinsightiga nime. Asendage (HTTP login) administraatori konto parool __parool__ .

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Pöörake tähelepanu sellele parameetrite/URI erinevas kui töötab otse soovitud headnode või soovitud serva sõlme klaster sees. Seda sellepärast, et ühenduse klaster Interneti kaudu kasutab avaliku lüüsi, mis marsruudib liikluse pordi 443. Ka mitu muud teenused puutuvad avaliku lüüsi pordi 443 kaudu, kui ühenduse otse URI erineb. Interneti kaudu ühendamise korral peab ka autentimiseks seansi esitada parool.

##<a id="summary"></a><a id="nextsteps"></a>Järgmised sammud

Nagu näete, Linnulennutee käsu abil on lihtne interaktiivseks käitamist on Hdinsightiga kobar taru päringud.

Üldteavet taru sisse Hdinsightiga:

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

