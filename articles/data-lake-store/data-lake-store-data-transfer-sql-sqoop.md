<properties 
   pageTitle="Kopeerige andmed andmesalve Lake-ja Azure SQL-i andmebaasi Sqoop abil | Microsoft Azure'i"
   description="Azure'i SQL-andmebaasi ja andmete Lake poe andmete kopeerimiseks kasutada Sqoop" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopeerige andmed Lake andmesalve ja Azure SQL-i andmebaasi Sqoop abil

Saate teada, kuidas kasutada Apache Sqoop Azure'i SQL-andmebaasi ja andmete Lake poe vahel andmete importimine ja eksportimine.
 

## <a name="what-is-sqoop"></a>Mis on Sqoop?

Suur andmete rakendused on loomulikus valik poolstruktuur- ja struktureerimata andmed, nt logid ja failide töötlemiseks. Siiski võib ka olla vaja struktureeritud andmeid, mis on talletatud relatsioonandmebaasidest.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) on loodud pilvepõhised andmebaasid ja suur andmete talletuskoht, nt Lake andmesalve vahel andmete edastamiseks tööriist. Relatsiooniandmebaasist haldamise süsteemi (RDBMS) näiteks Azure'i SQL-andmebaasi andmete importimiseks Lake andmesalve saate seda kasutada. Saate siis muuta ja suur andmed töökoormus kasutamine andmete analüüsimiseks ja seejärel eksportige andmed tagasi mõne RDBMS. Selles õpetuses kasutate Azure'i SQL-andmebaasi relatsiooniline andmebaasi importida või eksportida kaudu.
 

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- **Luba Azure tellimuse** Lake andmesalve avaliku eelvaade. Vaadake [juhiseid](data-lake-store-get-started-portal.md#signup). 
- **Windows azure Hdinsightiga kobar** Lake andmesalve kontole. Vaadake teemat [mõne Hdinsightiga kobar andmete Lake poe loomine](data-lake-store-hdinsight-hadoop-use-portal.md). Selles artiklis eeldatakse, et teil on Hdinsightiga Linuxi klaster, Lake andmesalve juurdepääsu.
- **Azure'i SQL-andmebaas**. Juhised, kuidas luua, leiate teemast [Azure SQL-andmebaasi loomine](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Kas saate teada, videote abil kiiresti?

[Vaadake seda videot](https://mix.office.com/watch/1butcdjxmu114) kohta, kuidas kopeerida andmeid Azure'i salvestusruumi plekid ja Lake andmesalve DistCp abil.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Azure'i SQL-andmebaasi valimi tabelite loomine

1. Et alustada, luua kahe valimi tabeli Azure'i SQL-andmebaasi. [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) või Visual Studio abil Azure SQL-i andmebaasiga ühenduse loomiseks ja seejärel käivitage järgmistele küsimustele.

    **Tabel1 loomine**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Tabel2 loomine**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. **Tabel1**, lisage mõningaid näidisandmeid. **Tabel2** tühjaks jätta. Me ei impordi andmed **Tabel1** Lake andmesalve. Siis me ei ekspordi andmed Lake andmesalve **Tabel2**. Käivitage järgmised koodilõigu.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop on Hdinsightiga kobar-juurdepääsu abil andmesalve Lake

Mõne Hdinsightiga kobar juba Sqoop paketid saadaval. Kui olete konfigureerinud Hdinsightiga kobar kasutamiseks Lake andmesalve soovitud täiendav salvestusruum, saate Sqoop (ilma konfiguratsiooni muudatusi) importida või eksportida andmed relatsiooniandmebaasist (näites Azure'i SQL-andmebaasi) vahel ja Lake andmesalve konto. 

1. Selles õpetuses on endale loodud Linuxi kobar, seega peaksite kasutama SSH ühenduse klaster. Vt [ühenduse loomine Linux-põhine Hdinsightiga kobar](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Kontrollige, kas pääsete Lake andmesalve konto klaster. Käivitage järgmine käsk SSH viip:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    See peaks andma failide ja kaustade Lake andmesalve konto loendit.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Azure'i SQL-andmebaasi andmete importimine andmesalve Lake

3. Liikuge kausta, kus on saadaval Sqoop. Tavaliselt on see tuleb `/usr/hdp/<version>/sqoop/bin`. 

4. Andmete importimine **Tabel1** Lake andmesalve kontole. Kasutage järgmist süntaksit:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Pange tähele, et **sql-andmebaasi serveri nimi** nime kohatäide, kus töötab Azure SQL-andmebaasi server. **SQL-andmebaasi nime** kohatäide tegelik andmebaasi nimi.

    Näiteks

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Veenduge, et andmed üle Lake andmesalve kontole. Käivitage järgmine käsk:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Peaksite nägema järgmine väljund.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Iga **osa – m -** * fail vastab tabelis andmeallika rea * *Tabel1**. Saate vaadata osa sisu – m -* faile, et kontrollida.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Andmete eksportimine Lake andmesalve Azure SQL-andmebaas

6. Andmete eksportimine Lake andmesalve konto tühja tabeliga, **Tabel2**, Azure SQL-andmebaasis. Kasutage järgmist süntaksit.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Näiteks

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Veenduge, et andmed on üles laaditud SQL-andmebaasi tabelisse. [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) või Visual Studio abil Azure SQL-andmebaasiga ühenduse loomiseks ja seejärel käivitage järgmine päring.

        
        SELECT * FROM TABLE2

    See peaks olema järgmine väljund.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Vt ka

- [Azure'i salvestusruumi plekid andmete kopeerimine andmesalve Lake](data-lake-store-copy-data-azure-storage-blob.md)
- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
