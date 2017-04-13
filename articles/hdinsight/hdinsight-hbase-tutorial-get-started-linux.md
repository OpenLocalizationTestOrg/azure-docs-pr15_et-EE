<properties
    pageTitle="HBase õpetus: alustamine Linux-põhine HBase kogumite rakenduses Hadoopi | Microsoft Azure'i"
    description="Täitke selle HBase õpetuse Apache HBase Hadoopi rakenduses Hdinsightiga koos kasutamise alustamine. Tabelite loomine HBase kest ja nende päringu abil taru."
    keywords="Apache hbase hbase, Shellis hbase hbase õpetus"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase õpetus: Apache HBase Linux-põhine Hadoopi rakenduses Hdinsightiga koos kasutamise alustamine 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Saate teada, kuidas luua mõne HBase kobar Hdinsightiga, HBase tabelite loomine ja päringu tabelite abil taru. Üldteavet HBase vt [Hdinsightiga HBase ülevaade][hdinsight-hbase-overview].

Teave selles dokumendis on Linux-põhine Hdinsightiga kogumite. Windowsi-põhiste kogumite kohta lisateabe saamiseks kasutage menüü Vaateselektori lehe ülaosas minna.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist õppeteema HBase, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Turvaline Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Looge HBase kobar

Järgmise toimingu kasutab mõni Azure ressursihaldur Mall versioon 3.4 Linux-põhine HBase kobar ja sõltuvad vaikimisi Azure Storage konto loomiseks. Protseduur ja muude kobar loomise võimalused kasutatud parameetrid, vt [loomine Linux-põhine Hadoopi kogumite Hdinsightiga sisse](hdinsight-hadoop-provision-linux-clusters.md).

1. Klõpsake malli avamiseks Azure'i portaalis järgmisel pildil. Mall asub avaliku bloobimälu ümbrises. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. **Kohandatud juurutus** keelest, sisestage järgmine:

    - **Tellimus**: Azure'i tellimuse klaster loomiseks valige.
    - **Ressursirühm**: Azure'i ressursside haldamine uue rühma loomine või olemasoleva kasutamiseks.
    - **Asukoht**: ressursirühma asukoha määramine. 
    - **ClusterName**: Sisestage loodava HBase klaster nimi.
    - **Kobar kasutajanime ja parooli**: login vaikenimi on **administraator**.
    - **SSH kasutajanimi ja parool**: vaikimisi kasutajanimi on **sshuser**.  Saate selle ümber nimetada.
     
    Muud parameetrid on valikuline.  
    
    Iga kobar on Azure'i bloobimälu salvestusruumi konto sõltuvus. Kui kustutate mõne kobar, säilitab andmed salvestusruumi konto. Kobar vaikimisi salvestusruumi konto nimi on "store" lisatud kobar nime. See on kõva malli muutujate jaotises.
        
3. Valige **nõustun eespool tingimused**ja seejärel klõpsake nuppu **osta**. Kulub umbes 20 minutit klaster loomiseks.


>[AZURE.NOTE] Pärast soovitud HBase kobar on kustutatud, saate luua teise HBase kobar, kasutades sama vaikimisi bloobimälu ümbrises. Uue kobar valivad kuni olete loonud algse kobar HBase tabelid. Vältida selliste vastuolude tekkimist, soovitame enne kustutamist klaster keelata HBase tabelid.

## <a name="create-tables-and-insert-data"></a>Tabelite loomine ja andmete sisestamine

Saate SSH ühenduse HBase kogumite ja seejärel kasutage HBase Shell HBase tabelite loomine, lisamine ja päringu andmeid. SSH kasutamise kohta leiate teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Linux, Unix, või OS X](hdinsight-hadoop-linux-use-ssh-unix.md) ja [Kasutage SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Enamik inimesi, kuvatakse andmed tabelina vormindamine:

![Hdinsightiga HBase tabelina esitatud andmed][img-hbase-sample-data-tabular]

Mis on BigTable rakendamise HBase, samad andmed näeb.

![Hdinsightiga HBase bigtable andmed][img-hbase-sample-data-bigtable]

See teeb loogilisem pärast järgmise toimingu lõpetamist.  


**Kasutada HBase shell**

1. Kaudu SSH, käivitage järgmine käsk:

        hbase shell

4. Kahe veeruga peredele on HBase loomiseks tehke järgmist.

        create 'Contacts', 'Personal', 'Office'
        list
5. Mõned andmed lisamiseks tehke järgmist.

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsightiga Hadoopi hbase shell][img-hbase-shell]

6. Ühe rea hankimine

        get 'Contacts', '1000'

    Kuvatakse sama tulemuse nagu skannimine käsu abil, kuna on ainult üks rida.

    HBase tabeli skeemi kohta leiate lisateavet teemast [Sissejuhatus HBase skeemi kujundus][hbase-schema]. Rohkemate käskude HBase leiate [Apache HBase Lühijuhend][hbase-quick-start].

6. Väljuge shell

        exit



**Laadi andmeid hulgi kontaktide HBase tabelisse**

HBase sisaldab tabelitesse andmete laadimine on mitu võimalust.  Lisateavet leiate teemast [hulgi laadimine](http://hbase.apache.org/book.html#arch.bulk.load).


Näidisandmete fail on üles laaditud avaliku bloobimälu container, *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Faili sisu on:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Saate luua tekstifaili ja faili üleslaadimine oma salvestusruumi konto, kui soovite. Juhised leiate teemast, [üleslaadimine andmete Hadoopi töökohta, Hdinsightiga][hdinsight-upload-data].

> [AZURE.NOTE] Seda toimingut kasutatakse kontaktide HBase tabeli viimase toimingu loomist.

1. Kaudu SSH, käivitage järgmine käsk muuta StoreFiles ja hoida Dimporttsv.bulk.output määratud suhteline tee andmefaili:.  Kui teil on HBase Shell, kasutage käsku välju sulgemiseks.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Üles laadida /example/data/storeDataFileOutput andmed tabelisse HBase järgmine käsk:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Saate avada HBase shell ja skannimine käsu abil saate loetleda tabeli sisu.



## <a name="use-hive-to-query-hbase"></a>Päringu HBase taru abil

Taru abil saate teha päringuid HBase tabeli andmeid. Selles jaotises loob taru tabeli, mis HBase tabelisse kaartide ja kasutab seda andmepäringu HBase tabelis.

1. Avage **PuTTY**ja ühenduse klaster.  Vaadake eelmise jaotise juhiseid.
2. Avage taru shell.

       hive
3. Käivitage järgmine HiveQL skript taru tabel, mida saab vastendada HBase tabeli loomiseks. Veenduge, et selles õpetuses enne, kui käivitate selle lause HBase shell abil viidatud näidistabeli loonud.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Käivitage järgmine HiveQL skript andmepäringu HBase tabelis.

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Kasutage HBase REST API-de kasutamine Curl

> [AZURE.NOTE] Kui kasutate Curl või muu ülejäänud side koos WebHCat, tuleb autentida taotlused pakkudes Hdinsightiga kobar administraatori kasutajanime ja parooli. Peate kasutama ka kobar nime osana, on ühtse identifikaator (URL) kasutatakse päringuid serveriga.
>
> Selles jaotises käsud, Asendage **KASUTAJANIME** ja kasutajale autentida klaster, ja **parooli** kasutajakonto parooli. Asendage **CLUSTERNAME** klaster nime.
>
> REST API on tagatud [elementaarautentimine](http://en.wikipedia.org/wiki/Basic_access_authentication)kaudu. Peate alati taotlusi Secure HTTP (HTTPS) abil tagada mandaat saadetakse turvaline server.

1. Kasutada käsureal, kinnitamaks, et saate luua ühenduse Hdinsightiga klaster järgmine käsk:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Peaksite nägema umbes järgmist vastuse:

        {"status":"ok","version":"v1"}

    Selle käsu parameetritest on järgmised:

    * **-u** - kasutajanimi ja parool, taotluse kinnitamiseks.
    * **-G** - näitab, et see on GET-päringu.

2. Kasutage olemasolevaid HBase tabelite loendi järgmine käsk:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Järgmise käsu abil saate HBase uue tabeli loomine kahe veeru peredele:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Skeemi on esitatud JSon-vormingus.

4. Järgmise käsu abil saate lisada mõned andmed:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Peate base64 kodeerida lülitit -d määratud väärtused.  Näide: rakenduses

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: isiklik: nimi
    - Sm9obiBEb2xl: John Dole

    [FALSE-rea-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) saate lisada mitu (batched) väärtus.

5. Kasutage rea saada järgmine käsk:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

HBase ülejäänud kohta leiate lisateavet teemast [Lühijuhend Apache HBase](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Kobar oleku kontrollimine

HBase sisse Hdinsightiga on Web UI kaasas kogumite jälgimiseks. Kasuta kasutajaliides Web, saate taotleda statistika või piirkondade teavet.

SSH saate kasutada ka tunneli kohalikke päringuid, näiteks web nõuab Hdinsightiga klaster. Taotluse seejärel suunatakse installimisega, kui see oli pärineb Hdinsightiga kobar pea sõlme. Lisateabe saamiseks lugege teemat [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Mõne SSH tunneling seansi loomiseks**

1. Avage **PuTTY**.  
2. Kui teil on SSH võti loomisprotsessi ajal oma kasutajakonto loomisel, peate tegema järgmist toimingut valimiseks kasutada, kui autentimine klaster privaatvõti:

    **Kategooria**laiendamiseks **ühendust**, laiendage **SSH**, ja valige **Auth**. Lõpuks klõpsake nuppu **Sirvi** ja valige .ppk fail, mis sisaldab teie privaatvõti.

3. Klõpsake **kategooria**Makrosätted **seansi**.
4. Ekraani kitt seansi võimalust: sisestage järgmised väärtused:

    - **Hosti nimi**: teie Hdinsightiga serveri Host name (või IP-aadress) välja SSH aadress. SSH aadress on teie kobar nime, siis **-ssh.azurehdinsight.net**. Näiteks *mycluster-ssh.azurehdinsight.net*.
    - **Port**: 22. Funktsiooni ssh porti esmane headnode on 22.  
5. Dialoogiboksi vasakul jaotises **kategooria** laiendamiseks **ühendust**, laiendage **SSH**ja klõpsake **tunnelid**.
6. Suvandi juhtimine SSH pordi edastamise vormi järgmise teabe:

    - **Lähteport** - porti klient, mida soovite edastada. Näiteks 9876.
    - **Dünaamiliste** - lubab dünaamiline sokid puhverserveri marsruutimist.
7. Lisamiseks klõpsake nuppu **Lisa** sätteid.
8. Klõpsake, et **avada** dialoogiboksi allservas avamiseks SSH ühendus.
9. Küsimise korral logige sisse kontoga, SSH server. See luua mõne SSH seanssi ja luba selle tunneliga.

**Zookeepers, kasutades Ambari FQDN leidmiseks**

1. Liikuge sirvides https://<ClusterName>.azurehdinsight.net/.
2. Sisestage oma kobar kasutajatunnust konto kaks korda.
3. Klõpsake vasakpoolses menüüs **zookeeper**.
4. Klõpsake ühte kolm **ZooKeeper serveri** lingid Kokkuvõte loendist.
5. **Hostname (hostinimi)**kopeerimine Näiteks zk0 CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Klientprogrammi (Firefox) ja konfigureerimiseks kobar oleku kontrollimine**

1. Avage Firefoxi.
2. Klõpsake nuppu **Ava menüü** .
3. Klõpsake nuppu **Suvandid**.
4. Klõpsake nuppu **Täpsemalt**, klõpsake nuppu **võrk**ja seejärel klõpsake nuppu **sätted**.
5. Valige **käsitsi puhverserveri konfigureerimine**.
6. Sisestage järgmised väärtused:

    - **Host sokid**: localhost
    - **Port**: Kasuta sama porti selle Putty SSH tunneling konfigureeritud.  Näiteks 9876.
    - **Sokid v5**: (valitud)
    - **Remote DNS-i**: (valitud)
7. Klõpsake muudatuste salvestamiseks nuppu **OK** .
8. Liikuge sirvides http://&lt;The FQDN on ZooKeeper >: 60010/juhtslaidi-olek.

Kõrge-saadavus kobar, leiate praeguse aktiivse HBase juhtslaidi sõlme hostib kasutajaliides Web link.

##<a name="delete-the-cluster"></a>Klaster kustutamine

Vältida selliste vastuolude tekkimist, soovitame enne kustutamist klaster keelata HBase tabelid.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Järgmised sammud

Hdinsightiga HBase selles õpetuses, saate teada, kuidas luua mõne HBase kobar ja tabelite loomine ja andmete kuvamiseks nende tabelite HBase kest. Tähtsaimad taru päringu kasutamine HBase tabeli andmeid ja kuidas kasutada HBase C# REST API-de HBase tabeli loomine ja andmete toomiseks tabelist.

Lisateavet leiate järgmistest teemadest.

- [Hdinsightiga HBase ülevaade][hdinsight-hbase-overview]: HBase on Apache, avatud lähtekoodi NoSQL andmebaasi tugineb Hadoopi, mis pakub muutmälu ja tugev järjepidevuse suurte andmehulkade struktureerimata ja semistructured.


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
