<properties
   pageTitle="Azure'i bloobimälu andmete laadimine SQL-i andmebaas (PolyBase) | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada PolyBase andmete Azure'i bloobimälu laadimiseks SQL-i andmebaas. Asetage mõned tabelid avalikest andmetest skeemi Contoso jaemüügi andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Azure'i bloobimälu andmete laadimine SQL-i andmebaas (PolyBase)

> [AZURE.SELECTOR]
- [Andmete Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Azure'i bloobimälu andmete laadimine SQL Azure'i andmebaas PolyBase ja T-SQL-i käskude abil. 

Hoidke sõnum lihtne, et selles õpetuses laadib kahe tabeli avaliku Azure'i salvestusruumi bloobimälu Contoso jaemüügi andmebaas skeemiga võtta. Täielik andmehulga laadimiseks käivitage Microsoft SQL Server näidised hoidla [laadimine täielik Contoso jaemüügi andmebaas][] näide.

Selles õppetükis saate küll:

1. Azure'i bloobimälu laadida PolyBase konfigureerimine
2. Avalike andmete laadimine andmebaasi
3. Kui laadi on lõpule jõudnud, tehke Optimeerimised.


## <a name="before-you-begin"></a>Enne alustamist
Selle õpetuse käivitamiseks peate Azure'i konto, mis on juba andmebaasi SQL-i andmebaas. Kui teil pole veel seda, lugege teemat [loomine SQL-i andmebaas][].

## <a name="1-configure-the-data-source"></a>1. andmeallika konfigureerimine

PolyBase kasutab T-SQL-i väliste objektide asukoht ja välisandmete atribuudid. SQL-i andmebaas on salvestatud välise objekti määratlusi. Andmed on salvestatud väliselt.

### <a name="11-create-a-credential"></a>1.1. Mandaati loomine

Kui laadite Contoso avalikest andmetest **Jäta see toiming vahele** . Avalikest andmetest turvaline juurdepääs pole vaja, kuna see on juba kättesaadav kõigile.

**Ärge jätke see juhis vahele** , kui kasutate selle õpetuse mallina laadimise oma andmetega. Mandaati kaudu pääsevad andmetele juurde, järgmise skripti abil saate luua andmebaasi ulatusega identimisteavet, ja seejärel kasutage seda andmeallika asukoha määratlemisel.


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
```

Jätkake juhisega 2.

### <a name="12-create-the-external-data-source"></a>1.2. Välise andmeallika loomine

Selle [Välise ANDMEALLIKA loomine][] käsu abil saate talletada asukoha andmed ja andmete tüüpi. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Kui valite oma Azure'i bloobimälu säilitus avalikuks muutmine, pidage meeles, et andmete omanikuna saate tuleb tasuda andmete sealt kulude andmete lahkumisel andmekeskuse. 

## <a name="2-configure-data-format"></a>2. andmete vormindamine konfigureerimine

Andmed salvestatakse teksti failide Azure'i bloobimälu ja iga välja on eraldatud eraldaja. Käivitage see [Luua välise FAILIVORMINGUS][] käsk teksti failid andmete vormingu määramiseks. Contoso andmed on tihendamata ja Triibu eraldatud.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. välise tabelite loomine

Nüüd, kui olete määratlenud andmete allikas ja failivormingusse, olete valmis looma välise tabeleid. 

### <a name="31-create-a-schema-for-the-data"></a>3,1. Skeemi andmete jaoks luua. 

Koht Contoso andmete talletamiseks andmebaasi loomiseks skeem.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3,2. Saate luua välise tabeleid. 

Käivitage see skript DimProduct ja FactOnlineSales välise tabeleid loomiseks. Kõik teeme siin on veergude nimed ja andmetüübid ja sidumine asukoht ja Azure'i bloobimälu salvestusruumi vormingus. SQL-i andmebaas on talletatud määratluse ja andmed on endiselt Azure'i salvestusruumi bloobimälu.

**Asukoha** parameeter on Azure salvestusruumi bloobimälu juurkausta kaustas. Iga tabeli on mõnda muusse kausta.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. andmete laadimine
On välisandmetele juurdepääsuks erineval viisil.  Saate päringu andmed otse välise tabeli, andmete laadimine uue andmebaasi tabelite või väliste andmete lisamine tabeleid.  


### <a name="41-create-a-new-schema"></a>4.1. Uue skeemi loomine

CTAS loob uue tabeli, mis sisaldab andmeid.  Esmalt looge skeemi contoso andmete jaoks.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Uue tabeli andmete laadimine

Azure'i bloobimälu kaudu andmete laadimine ja salvestage see sees oma andmebaasi tabelisse, kasutage lause [Loomine tabeli AS valimine (Transact-SQL-i)][] . Laadimise koos CTAS mõjutab äsja loodud tugevalt tipitud välise tabeleid. Kasutage andmete laadimiseks uue tabeli ühe [CTAS][] lause tabeli kohta. 

CTAS loob uue tabeli ja kuvab selle tulemused select-lause. CTAS määratleb olema sama veergude ja andmetüüpide select-lause tulemitena uue tabeli. Kui valite välise tabelist kõik veerud, saab uue tabeli veergude ja andmetüüpide välise tabeli koopia.

Selles näites me luua dimensiooni nii nagu räsi jaotatud tabelite fact tabeli. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 Laadi jälgimiseks

Saate jälgida oma laadi dünaamiline haldusvaated () kasutamine edenemist. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. optimeerimine columnstore tihendamine

Vaikimisi SQL-i andmebaas salvestab tabeli klaster columnstore register. Pärast koormus on lõpule jõudnud, mitte võib osa andmeridade tihendada on columnstore.  On mitmeid põhjused, miks see võib juhtuda, et. Lisateavet leiate teemast [haldamine columnstore registrid][].

Päringu jõudlus ja columnstore tihendamise pärast koormus optimeerimiseks taastada tabeli jõustamine columnstore registri tihendamiseks kõik read. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Säilitades columnstore registrite kohta leiate lisateavet artiklist [haldamine columnstore registrid][] .

## <a name="6-optimize-statistics"></a>6. optimeerimine statistika

See on kõige parem luua ühe veeruga statistika kohe pärast soovitud laadi. On statistika järgmisi võimalusi. Kui loote ühe veeruga statistika iga veeru see võib võtta kaua aega, et taastada kõik statistika. Kui teate, et teatud veergude ei kavatse olla päringu predicates, saate luua statistika nende veergude vahele jätta.

Kui otsustate luua ühe veeruga statistika iga iga tabeli veerust, saate kasutada salvestatud protseduur proovi kood `prc_sqldw_create_stats` [statistika][] artiklis.

Järgmises näites on hea alguspunkt statistika loomine. See loob ühe veeruga statistika iga veerg tabelis mõõde ja klõpsake iga liitumist veeru fact tabelite. Saate alati lisada ühe või mitme veeru statistika muude fact tabeli veergude uuem versioon.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Saavutamise lukustamata!

Avalikest andmetest on laaditud edukalt SQL Azure'i andmebaas. Hästi tehtud!

Nüüd saate alustada päringute tabelite, päringute umbes selline:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Järgmised sammud
Täielik andmete Contoso jaemüügi andmebaas laadimiseks kasutada skripti veel arengu näpunäiteid leiate teemast [SQL-i andmebaas arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[Looge SQL-i andmebaas]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL-i andmebaas arengu ülevaade]: sql-data-warehouse-overview-develop.md
[columnstore registrite haldamine]: sql-data-warehouse-tables-index.md
[Statistika]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[VÄLISE ANDMEALLIKA LOOMINE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[LUUA VÄLISE FAILIVORMING]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Looge tabel AS valimine (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Laadi täielik Contoso jaemüügi andmebaas]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
