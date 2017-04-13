<properties 
   pageTitle="Kasutage Apache Phoenix ja Hdinsightiga orav | Microsoft Azure'i" 
   description="Teada, kuidas kasutada Apache Phoenix Hdinsightiga ja kuidas installida ja konfigureerida ühenduse mõne HBase kobar sisse Hdinsightile oma töökoha orav." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Kasutage Apache fööniks Linux-põhine HBase le Hdinsightiga  

Teada, kuidas kasutada [Apache Phoenix](http://phoenix.apache.org/) Hdinsightiga ja SQLLine kasutamise kohta. Phoenix kohta leiate lisateavet teemast [Phoenix 15 minutit või vähem](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Phoenix grammatika, leiate teemast [Phoenix grammatika](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Hdinsightiga Phoenix-versiooni leiate teemast [mis on uut rakenduses Hadoopi kobar versioonide pakutavast Hdinsightiga?] [hdinsight-versions].

##<a name="use-sqlline"></a>Kasutage SQLLine
[SQLLine](http://sqlline.sourceforge.net/) on käsurea kasuliku SQL-i käivitada. 

###<a name="prerequisites"></a>Eeltingimused
Enne SQLLine kasutamiseks peab teil olema järgmised:

- **A HBase kobar Hdinsightiga sisse**. Klaster sätte HBase teavet leiate teemast [Alustamine rakenduses Hdinsightiga Apache HBase][hdinsight-hbase-get-started].
- **Ühenduse HBase kobar Remote'i töölaua-protokolli**. Juhised leiate teemast [haldamine Hadoopi kogumite Hdinsightiga, kasutades Azure klassikaline portaali sisse][hdinsight-manage-portal].


Kui loote ühenduse mõne HBase kobar, peate selle Zookeepers ühendamiseks. Iga Hdinsightiga kobar on 3 Zookeepers. 

**Kui soovite teada saada Zookeeper hosti nimi**

1. Liikuge sirvides Ambari avamiseks **https://<ClusterName>. azurehdinsight.net**.
2. HTTP (klaster) kasutajanimi ja parool, sisestage Logi sisse.
3. Klõpsake vasakpoolses menüüs **ZooKeeper** . Näete peab 3 **ZooKeeper Server** loetletud.
4. Valige üks loetletud **ZooKeeper Server** . Otsige kokkuvõttepaanil, **hostname (hostinimi)**. See on sarnane *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine kasutamine**

1. Ühenduse klaster SSH abil. Lisateabe saamiseks vaadake [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Linux, Unix, või OS X](hdinsight-hadoop-linux-use-ssh-unix.md) või [Kasutage SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md) sõltuvalt teie klientarvuti OS.

2. Käivitage kaudu SSH, SQLLine käivitamiseks järgmised käsud:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Käivitage järgmised käsud HBase tabeli loomiseks ja lisage siis mõned andmed:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Lisateavet leiate teemast [SQLLine käsitsi](http://sqlline.sourceforge.net/#manual) ja [Phoenix grammatika](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Järgmised sammud
Selles artiklis on õppinud, kuidas kasutada Apache Phoenix Hdinsightiga.  Lisateavet leiate teemast

- [Hdinsightiga HBase ülevaade][hdinsight-hbase-overview]: HBase on Apache, avatud lähtekoodi NoSQL andmebaasi tugineb Hadoopi, mis pakub muutmälu ja tugev järjepidevuse suurte andmehulkade struktureerimata ja semistructured.
- [Ettevalmistamise HBase kogumite Azure virtuaalse võrgus][hdinsight-hbase-provision-vnet]: virtuaalse võrgu integreerimise, saate HBase kogumite juurutatud rakenduste virtuaalse samasse võrku nii, et rakendused ei saa suhelda HBase otse.
- [HBase konfigureerimine paljundamine Hdinsightiga](hdinsight-hbase-geo-replication.md): saate teada, kuidas konfigureerida HBase dispersioonanalüüs üle kahe Azure andmekeskuste. 
- [Analüüsida Twitter meeleolu koos HBase sisse Hdinsightiga][hbase-twitter-sentiment]: saate teada, kuidas teha reaalajas [meeleolu analüüsi](http://en.wikipedia.org/wiki/Sentiment_analysis) suurte Hadoopi kobar sisse Hdinsightiga HBase abil.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
