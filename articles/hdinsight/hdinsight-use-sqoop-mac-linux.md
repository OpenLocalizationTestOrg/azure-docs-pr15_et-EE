<properties
    pageTitle="Hadoopi Sqoop kasutamine Linux-põhine Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas käivitada Sqoop importimine ja eksportimine Linux-põhine Hadoopi Hdinsightiga kobar ja Azure SQL-andmebaasi vahel."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Hadoopi sisse Hdinsightiga (SSH) Sqoop kasutamine

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saate teada, kuidas kasutada Sqoop importimine ja eksportimine Linux-põhine Hdinsightiga kobar ja Azure'i SQL-andmebaasi või SQL serveri andmebaasi vahel.

> [AZURE.NOTE] Selles artiklis toodud juhiseid kasutada SSH ühenduse loomiseks Linux-põhine Hdinsightiga kobar. Windowsi kliendid kasutada ka Azure PowerShelli ja Hdinsightiga .NET SDK Linux-põhine kogumite Sqoop koos töötada. Avage nende artiklite Vaateselektori menüü abil.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **A Hadoopi kobar Hdinsightiga sisse** ja __Azure'i SQL-andmebaasi__: selle dokumendi juhised põhinevad kobar ja dokumendi [loomine kobar ja SQL-andmebaasi](hdinsight-use-sqoop.md#create-cluster-and-sql-database) abil loodud andmebaasi. Kui teil on juba Hdinsightiga kobar ja SQL-andmebaasiga, saate need väärtused selles dokumendis kasutatud asendada.
- **Töökoha**: SSH kliendi arvuti.

##<a name="install-freetds"></a>Installige FreeTDS

1. Ühenduse Linux-põhine Hdinsightiga kobar SSH abil. Loomisel kasutatav aadress on `CLUSTERNAME-ssh.azurehdinsight.net` ja pordi on `22`.

    Ühenduse loomiseks Hdinsightiga SSH kasutamise kohta lisateabe saamiseks vt järgmised dokumendid

    * **Linux, Unix või OS X kliendid**: vt [ühenduse loomine Linux-põhine Hdinsightiga kobar Linux, OS X või Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windowsi kliendid**: vt [ühenduse loomine Linux-põhine Hdinsightiga kobar Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Järgmise käsu abil saate installida FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    SQL-i andmebaasiga ühenduse loomiseks kasutatakse FreeTDS mitmes etapis.

##<a name="create-the-table-in-sql-database"></a>SQL-andmebaasis tabeli loomine

> [AZURE.IMPORTANT] Kui kasutate Hdinsightiga kobar- ja SQL-andmebaasi [loomine kobar](hdinsight-use-sqoop.md)ja SQL-andmebaasi juhiseid abil loodud, Ignoreeri selles jaotises toodud juhised osana juhiseid dokument on loodud andmebaas ja tabel.

1. Hdinsightiga SSH ühenduse kaudu kasutada ühenduse SQL-andmebaasi server ja Kreeta tabelit, mida kasutatakse järgmist ülejäänud järgmine käsk:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Saate väljundi umbes järgmine:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Juures olevat `1>` Küsi, sisestage järgmised read:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Kui soovitud `GO` esitatakse, eelmise laused hinnatakse. Esmalt luuakse **mobiledata** tabel ja seejärel rühmitatud indeks lisatakse see (nõutav SQL-andmebaasi).

    Veenduge, et tabel on loodud kasutada järgmist:

        SELECT * FROM information_schema.tables
        GO

    Peaksite nägema väljundi umbes järgmine:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Sisestage `exit` juures olevat `1>` kiiret tsql kasuliku väljumiseks.

##<a name="sqoop-export"></a>Sqoop eksportimine

3. Kaudu SSH ühendus Hdinsightiga, se, kinnitamaks, et Sqoop näevad SQL-andmebaasi järgmine käsk:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    See peaks tagastama andmebaaside, sh varem loodud andmebaasi **sqooptest** loendit.

4. Kasutage andmete eksportimine **hivesampletable** **mobiledata** tabelisse järgmine käsk:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    See teeb Sqoop **sqooptest** andmebaasiga, SQL-andmebaasiga ühenduse loomiseks ja andmete eksportimine on **wasbs: / / / taru/ladu/hivesampletable** ( *hivesampletable*füüsilise failid) **mobiledata** tabelisse.

5. Pärast käsu on lõpule jõudnud, kasutades TSQL andmebaasiga ühenduse loomiseks järgmist kasutamine

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Kui ühendus on loodud, kasutage kinnitamaks, et andmed on eksporditud **mobiledata** tabelisse järgmistest:

        SELECT * FROM mobiledata
        GO

    Peaksite nägema loetletakse tabelis andmeid. Tippige `exit` tsql kasuliku sulgemiseks.

##<a name="sqoop-import"></a>Sqoop importimine

1. Järgmine abil saate importida andmeid tabelist **mobiledata** SQL-andmebaasis, kuni soovitud **wasbs: / / / õpetused/usesqoop/importeddata** Hdinsightiga kataloogi:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Imporditud andmete on väljad, mis on eraldatud tabeldusmärgi ja read lõpetatakse uue rea märk.

2. Kui importimine on lõppenud, kasutada uut kataloogis loendi andmed järgmine käsk:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>SQL serveri kasutamine

Samuti saate importida ja eksportida andmed SQL Serverist oma andmekeskuse või majutatud Azure virtuaalse masina Sqoop. SQL-andmebaas ja SQL serveri kasutamise erinevused on:

* Nii Hdinsightiga ja SQL serveri peavad olema sama Azure virtuaalse võrgus

    > [AZURE.NOTE] Hdinsightiga toetab ainult asukoht põhineb virtuaalse võrgu ja see ei tööta praegu osaleja rühma-põhiste virtuaalse võrgustikke.

    Kui kasutate SQL serveri oma andmekeskuses, tuleb konfigureerida virtuaalse võrgu *- saidilt* või *saidi punkti*.

    > [AZURE.NOTE] **Punkti saidi** virtuaalne võrkude, SQL Server peab töötama VPN klient konfigureerimisrakenduse, mis on saadaval Azure virtuaalse võrgukonfiguratsioon **armatuurlaud** .

    Lisateavet leiate Azure'i virtuaalse võrgu vt [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md).

* SQL Server peab olema konfigureeritud lubama SQL-i autentimist. Lisateavet leiate teemast [valimine soovitud autentimisrežiim](https://msdn.microsoft.com/ms144284.aspx)

* Kui teil konfigureerida SQL serveri allalaadimise kinnitamiseks. Lisateabe saamiseks vaadake [tõrkeotsingu ühenduse SQL serveri andmebaasi mootor](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Peate looma **sqooptest** andmebaasi SQL serveri abil nagu **SQL Server Management Studio** või **tsql** kasuliku - abil Azure'i CLI ainult tööta Azure SQL-andmebaas

    Sarnanevad TSQL laused **mobiledata** tabeli loomiseks kasutatud SQL-i andmebaasi, välja arvatud loomise on clusterd index – seda pole vaja SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Kui ühendate SQL serveri hdinsightist, tuleb kasutada SQL serveri IP-aadress, kui olete konfigureerinud on domeeninime süsteemi (DNS) nimede Azure virtuaalse võrgus. Näiteks:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Piirangud

* Hulgi ekspordi - ja Linux-põhine Hdinsightiga, kasutatakse Microsoft SQL Server või Azure'i SQL-andmebaasi andmete eksportimine Sqoop konnektor ei toeta praegu hulgi lisab.

* Partiide - ja Linux-põhine Hdinsightiga, kasutades funktsiooni `-batch` vahetada, kui lisatakse, Sqoop täidab mitme lisab asemel partiide lisa toimingud.

##<a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud, kuidas kasutada Sqoop. Lisateavet leiate järgmistest teemadest.

- [Oozie kasutamine Hdinsightiga][hdinsight-use-oozie]: kasutamine Sqoop toimingu Oozie töövoos.
- [Analüüsida viivitus lennuandmetega abil Hdinsightiga][hdinsight-analyze-flight-data]: flight analüüsimiseks kasutada taru viivitamine andmed ja Azure SQL-andmebaasi andmete eksportimine Sqoop abil.
- [Laadi andmed Hdinsightiga][hdinsight-upload-data]: Otsige muid viise andmete üleslaadimiseks Hdinsightiga/Azure'i bloobimälu.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
