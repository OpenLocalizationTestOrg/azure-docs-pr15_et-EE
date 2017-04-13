<properties
    pageTitle="Mis on rakenduses Hdinsightiga HBase? | Microsoft Azure'i"
    description="Sissejuhatus Apache HBase sisse Hdinsightiga NoSQL andmebaasi luua Hadoopi. Lisateavet kasutamise juhtudel ja võrrelda teiste Hadoopi kogumite HBase."
    keywords="bigtable nosql, mis on hbase"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Mis on rakenduses Hdinsightiga HBase: A NoSQL andmebaas, mida pakub BigTable nagu Hadoopi jaoks

Apache HBase on avatud lähtekoodi NoSQL andmebaasi, mis on ehitatud Hadoopi ja pärast Google BigTable modelleerida. HBase pakub muutmälu ja keeruka järjepidevus suurte andmehulkade struktureerimata ja semistructured schemaless andmebaasi peredele veeru järgi.

Andmed salvestatakse tabeli read ja andmete ühes reas on rühmitatud veeru perekonnad. HBase on schemaless andmebaasi mõttes veergude ega neid talletatud andmete tüüpi tuleb määratleda enne nende kasutamist. Avatud lähtekood skaala lineaarses käsitlema terabait andmete tuhandeliste sõlmed. Andmete koondamine, Pakktöötlus ja muud funktsioonid, mis on esitatud hajutatud rakendusi Hadoopi ökosüsteemis saab usaldada.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Kuidas rakendatakse HBase Windows Azure Hdinsightiga?

Hdinsightiga HBase pakutakse hallatavate klaster, mis on integreeritud Azure keskkonnas. Rühmad on konfigureeritud andmete talletamiseks otse Azure'i bloobimälu, mis pakub madal latentsus ja suurem paindlikkus jõudlus ja maksumus valikuid. See võimaldab klientidel luua interaktiivseid veebisaite, mis töötavad suurte andmekogumite, luua teenuseid, mille lõpp-punkti miljoneid sensor ja telemeetria andmete talletamiseks ja Hadoopi töö andmeid analüüsida. HBase ja Hadoopi on hea alates punktides big data Projecti Azure'i; saate need võimaldavad reaalajas rakendusi suurte andmekogumite töötamiseks.

Hdinsightiga rakendamist mõjutab skaala-out arhitektuur HBase automaatne sharding tabelid, loeb ja kirjutab automaatselt Tõrkesiirde tugev järjepidevuse esitada. Jõudluse suurendab-mälu vahemällu loeb ja suure läbilaskevõimega streaming kirjutab. Virtuaalse võrgu ettevalmistamise on saadaval ka Hdinsightiga HBase jaoks. Lisateavet leiate teemast [sätte Hdinsightiga kogumite Azure virtuaalse võrgus] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Kuidas hallatakse andmete Hdinsightiga HBase?

Andmeid saab hallata HBase, kasutades funktsiooni `create`, `get`, `put`, ja `scan` HBase shell käsud. Andmed on kirjutatud andmebaasi, kasutades `put` ja lugeda, kasutades `get`. Funktsiooni `scan` käsku kasutatakse mitme tabeli ridade andmete hankimine. Andmeid saab hallata ka HBase C# API, mis pakub kliendi teek peal HBase REST API abil. Rakenduse HBase andmebaasi saab ka kasutataks taru abil. Sissejuhatus need programmeerimise mudelid, leiate [HBase koos Hadoopi rakenduses Hdinsightiga kasutamise alustamine][hbase-get-started]. Kaasautorluse protsessorit on ka saadaval, võimaldades sõlmed töötlemise majutavate andmebaas.


## <a name="scenarios-use-cases-for-hbase"></a>Stsenaariumid: HBase juhtudel kasutamiseks
Kanoonilise kasutamine juhtum, millised BigTable (ja laiendamine, HBase) on loodud oli veebiotsing. Otsingumootorid koostada registrid, mis veebilehti, mis sisaldavad neid termineid vastendada. Kuid on palju muud kasutamine juhtumeid, et HBase sobib – mitut, mis on selles jaotises loetletud.

- Võti-väärtus pood

    HBase saab kasutada võti väärtus pood ja sobib juhtimine sõnum. Facebooki kasutab HBase oma sõnumisüsteemist üle ja see on väga käepärane talletamine ja Interneti-side haldamine. WebTable kasutab HBase otsimiseks ja tabelid, mis on saadud veebilehtede haldamine.

- Andurite andmeid

    HBase on kasulik hõivamine kogutud artistide eri allikatest pärit andmeid. See hõlmab social analüüsi aeg sarja kuuluv interaktiivsed armatuurlauad trendide ja hinnale ajakohasena hoidmine ja juhtimine auditilogi log. Näiteks Bloomberg kaupleja terminal ja avatud aja sarja andmebaasi (OpenTSDB), mis talletab ja pakub juurdepääsu mõõdikute kogutud serveri süsteemi seisundi kohta.

- Reaalajas päring

    [Phoenix](http://phoenix.apache.org/) on SQL-i päringu mootor Apache HBase jaoks. See on kättesaadav JDBC juht ning võimaldab päringu- ja haldamise HBase tabelid SQL-i abil.

- HBase platvormi

    Rakenduste käitamise kasutades seda on andmesalve HBase peal. Näiteks Phoenix, OpenTSDB, Kiji ja Titan. Rakenduste võib ka integreerida HBase. Näiteks taru siga Solri, Storm, Flume, Impala, säde, Ganglia ja süvitsi minna.


##<a name="next-steps"></a>Järgmised sammud

- [Hadoopi rakenduses Hdinsightiga koos HBase kasutamise alustamine][hbase-get-started]
- [Virtuaalne võrgus Azure Hdinsightiga kogumite ettevalmistamine] [hbase-provision-vnet]
- [Hdinsightiga HBase dispersioonanalüüs konfigureerimine](hdinsight-hbase-geo-replication.md)
- [Analüüsi Twitteri meeleolu koos HBase sisse Hdinsightiga][hbase-twitter-sentiment]
- [Maven abil saate koostada Java rakendusi, mida kasutada HBase Hdinsightiga (Hadoopi)][hbase-build-java-maven]

##<a name="see-also"></a>Vt ka

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Liigendatud andmete jaotatud salvestusruumi süsteemi](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
