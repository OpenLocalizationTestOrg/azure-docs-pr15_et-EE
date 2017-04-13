<properties
   pageTitle="SQL-i andmete ladu õpetuses PolyBase | Microsoft Azure'i"
   description="Siit saate teada, mis PolyBase on ja kuidas seda kasutada andmete hoidmise stsenaariumid."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Laadi andmete PolyBase rakenduses SQL-andmebaas

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Andmete Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Selle õpetuse näitab, kuidas andmete laadimiseks SQL-i andmebaas AzCopy ja PolyBase abil. Kui olete lõpetanud, siis teate, kuidas:

- Andmete kopeerimisel Azure'i bloobimälu AzCopy abil
- Andmete määratlemiseks andmebaasiobjektide loomine
- T-SQL-i päringu laadimiseks andmed

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Eeltingimused

Samm selle õpetuse kaudu, peate

- SQL-i andmebaas andmebaasi.
- Azure'i salvestusruumi konto tüüp Standard kohalikult liigsete salvestusruumi (Standard-LRS) Standard Geo-tarbetud salvestusruumi (Standard-GRS) või Standard-lugemisõigus Geo-tarbetud salvestusruumi (Standard-RAGRS).
- AzCopy käsurea kasuliku. Laadige alla ja installige [uusim versioon AzCopy][] , mis on installitud Microsoft Azure'i salvestusruumi tööriistu.

    ![Azure'i salvestusruumi tööriistad](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Samm 1: Azure'i bloobimälu Näidisandmete lisamine

Andmete laadimine peame Näidisandmete pannakse Azure'i bloobimälu. Selles etapis tuleb meil asustada mõne salvestusruumi Azure'i bloobimälu näidisandmetega. Hiljem kasutame PolyBase näidisandmed andmebaasi SQL-i andmebaas laadimiseks.

### <a name="a-prepare-a-sample-text-file"></a>V-SSE. Teksti näidisfaili ettevalmistamine

Ettevalmistamiseks teksti näidisfaili:

1. Avage Notepad ja järgmised read andmete kopeerimine uue faili. Salvestage see kohaliku ajutiste failide kaust % temp%\DimDate2.txt nimega.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Otsige bloobimälu teenuse lõpp-punkti

Bloobimälu teenuse lõpp-punkti leidmiseks tehke järgmist.

1. Azure'i portaalis valige **Sirvi** > **Salvestusruumi kontod**.
2. Klõpsake salvestusruumi kontot, mida soovite kasutada.
3. Klõpsake labale salvestusruumi konto plekid

    ![Klõpsake plekid](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Salvestage oma bloobimälu teenuse lõpp-punkti URL hiljem.

    ![Bloobimälu teenuse lõpp-punkti](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Azure'i salvestusruumi tootenumbri otsimine

Azure'i salvestusruumi võtme otsimiseks tehke järgmist.

1. Azure'i portaalis valige **Sirvi** > **Salvestusruumi kontod**.
2. Klõpsake salvestusruumi konto, mida soovite kasutada.
3. Valige **Kõik sätted** > **kiirklahvide**.
4. Klõpsake välja Kopeeri ühte oma kiirklahvide lõikelauale kopeerida.

    ![Kopeeri Azure storage võti](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Kopeerige näidisfaili Azure'i bloobimälu

Azure'i bloobimälu oma andmete kopeerimine

1. Avage käsuviip ja muuta kataloogide AzCopy installikaust. See käsk installi vaikekataloogi 64-bitise Windowsi kliendi muutub.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Käivitage järgmine käsk faili üles laadida. Määrake oma bloobimälu teenuse lõpp-punkti URL-i <blob service endpoint URL> ja < azure_storage_account_key > oma Azure storage konto võti.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Vt ka [AzCopy käsurea kasuliku töötamise alustamine][].

### <a name="e-explore-your-blob-storage-container"></a>E. Teie bloobimälu salvestusruumi container uurimine

Faili vaatamiseks saate üles laadida bloobimälu:

1. Minge tagasi oma bloobimälu teenuse tera.
2. Topeltklõpsake jaotises ümbriste, **datacontainer**.
3. Tee uurida oma andmetega, klõpsake kausta **datedimension** ja te näete oma üleslaaditud faili **DimDate2.txt**.
4. Klõpsake atribuutide vaatamiseks **DimDate2.txt**.
5. Pange tähele, et labale bloobimälu atribuudid saate alla laadida või kustutage fail.

    ![Vaate salvestusruumi Azure'i bloobimälu](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Samm 2: Näidisandmetega välise tabeli loomine

Selles jaotises saame luua välise tabeli, mis määratleb näidisandmeid.

PolyBase kasutab välise tabeleid, et Azure'i bloobimälu andmeid. Kuna andmed on salvestatud pole SQL-i andmebaas, tegeleb PolyBase autentimise välisandmete andmebaasi ulatusega mandaadi abil.

Selles juhises näide kasutab neid Transact-SQL-lausete välise tabeli loomiseks.

- Salajane andmebaasi krüptimiseks [loomine juhtslaidi klahv (Transact-SQL-i)][] rakendatud mandaati.
- Autentimis-teabe Azure storage konto jaoks [Luua andmebaasi veebirakendust mandaadi (Transact-SQL-i)][] .
- Azure'i bloobimälu oma asukoha määramiseks [Luua välise andmeallika (Transact-SQL-i)][] .
- Andmete vormingu määramiseks [Luua välise failivorming (Transact-SQL-i)][] .
- Tabeli määratlus ja andmete asukoha määramiseks [Välise tabeli loomine (Transact-SQL-i)][] .

Käivitage see SQL-i andmebaas andmebaasi suhtes. See on nimega DimDate2External dbo skeem, mis viitab DimDate2.txt valimi andmete Azure'i bloobimälu välise tabeli loomine.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


SQL serveri objekti Explorer Visual Studio, saate vaadata välise failivormingus, välise andmeallikaga ja DimDate2External tabel.

![Kas välise tabeli kuvamine](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Samm 3: Andmete laadimine SQL-andmebaas

Kui välise tabeli on loodud, saate laadida andmeid uude tabelisse või lisada olemasolevasse tabelisse.

- Laadite andmed uude tabelisse, käivitage lause [Loomine tabeli AS valimine (Transact-SQL-i)][] . Uue tabeli on päring nimega veerud. Veergude andmetüübid vastavad andmetüüpide välise tabeli määratlus.
- Olemasolevasse tabelisse andmeid laadimiseks kasutamine [Lisa... Valige (Transact-SQL-i)][] lause.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Samm 4: Luua oma äsja laaditud andmete statistika

SQL-i andmebaas ei Loo automaatselt või statistika automaatne uuendamine. Seetõttu kõrge päringu jõudluse saavutamiseks on oluline luua statistika iga tabeli iga veerg pärast esimest laadi. See on oluline statistika pärast olulisi andmeid värskendada.

Selles näites loob ühe veeruga statistika DimDate2 uue tabeli.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Lisateavet leiate teemast [statistika][].  


## <a name="next-steps"></a>Järgmised sammud
Vt täiendavat teavet, mida peaksite teadma, kui teil tekib lahenduse, mis kasutab PolyBase [PolyBase juhend][] .

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[PolyBase juhend]: ./sql-data-warehouse-load-polybase-guide.md
[AzCopy käsurea kasuliku töötamise alustamine]: ../storage/storage-use-azcopy.md
[AzCopy uusim versioon]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[LUUA välise ANDMEALLIKAGA (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[LUUA välise FAILIVORMING (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[Saate luua välise tabeli (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Looge tabel AS valimine (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[LISA … Valige (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Looge JUHTSLAIDI võti (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[LUUA andmebaasi VEEBIRAKENDUST MANDAADI (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
