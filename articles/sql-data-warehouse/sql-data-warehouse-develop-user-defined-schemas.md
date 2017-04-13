<properties
   pageTitle="Kasutaja määratletud skeemid SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid rakenduse SQL Azure'i andmebaas Transact-SQL-i skeemid arendamise lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Kasutaja määratletud skeemid SQL-andmebaas

Rakenduse piirmäärad töökoormus, domeeni või turvalisus põhinev loomiseks kasutada traditsiooniline andmete ladu sageli eraldi andmebaasid. Näiteks võivad traditsiooniline SQL Serveri andmebaas on vaheandmebaasi, andmete ladu andmebaasi ja kõik andmed mart andmebaasid. See topoloogia iga andmebaasi toimib töökoormus ja turvalisus piiri arhitektuur.

Seevastu töötab SQL-i andmebaas kogu andmete ladu liitmisel ühe andmebaasi. Rist andmebaasi ühendused on lubatud. Seetõttu eeldab SQL-i andmebaas, et kõik tabelid, mis kasutavad ladu salvestatud ühe andmebaasi.

> [AZURE.NOTE] SQL-i andmebaas ei toeta rist andmebaasipäringud jätmiseks. Seetõttu tuleb läbi vaadata andmete ladu rakendusi, mis seda mudelit kasutada.

## <a name="recommendations"></a>Soovitused

Need on soovitused konsolideerimisele töökoormus, Turve, domeeni ja funktsionaalne piirmäärad kasutaja määratletud skeemide abil

1. Ühe SQL-i andmebaas andmebaasi abil saate käivitada oma kogu andmete ladu töökoormus
2. Olemasolevate andmete ladu keskkonna ühe SQL-i andmebaas andmebaasi kasutama konsolideerimine
3. **Kasutaja määratletud skeemid** esitada varem rakendatud andmebaaside abil äärist kasutada.

Kui kasutaja määratletud skeemid ei ole kasutatud varem on teil puhtalt lehelt. Lihtsalt vana andmebaasi nimi alusena kasutada kasutaja määratletud skeemid SQL-i andmebaas andmebaasi jaoks.

Kui skeemid juba kasutanud, siis on teil mitu võimalust:

1. Eemaldage pärand skeemi nimesid ja alustada puhtalt lehelt
2. Säilitada pärand skeemi nimed ette nurksulgudes pärand skeemi nimi tabeli nimi
3. Rakendades vaadete üle tabeli eest skeemiga uuesti luua vana skeemi struktuuri säilitamise pärand skeemi nimed.

> [AZURE.NOTE] Klõpsake esimese kontrolli suvand 3 võib tunduda kõige pilkupüüdev suvand. Kurat on üksikasjad. Vaadete lugeda ainult SQL-i andmebaas. Andmeid või tabelivormingut muudatused oleks vaja teha base tabeli. Suvand 3 lisatakse ka teie süsteemi kiht vaated. Võite mõtlema see täiendavad kui kasutate vaadete oma arhitektuuri juba.


### <a name="examples"></a>Näited:

Kasutaja määratletud skeemid põhjal andmebaasinimed rakendada.

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Säilitada pärand skeemi nimed ette nurksulgudes need tabeli nimi. Kasutage skeemid töökoormus äärist.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Säilitamise pärand skeemi nimed vaadete kasutamine

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Skeemi strateegia muudatustest peab andmebaasi mudelit ülevaade. Paljudel juhtudel võimalik lihtsustada mudelit skeemi tasemel õiguste määramine. Kui täpsema õigused on nõutavad, siis saate kasutada andmebaasi rollid.

## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
