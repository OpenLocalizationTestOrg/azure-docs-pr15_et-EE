<properties
   pageTitle="SQL-i andmebaas taastamine | Microsoft Azure'i"
   description="Andmebaasi taastamine suvandid SQL Azure'i andmebaas andmebaasi taastamisel ülevaade."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>SQL-i andmebaas taastamine

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Portaal][]
- [PowerShelli][]
- [ÜLEJÄÄNUD][]

SQL-i andmebaas pakub kohalikke ja geograafiliste taastab oma andmete ladu katastroofi osana funktsioonid. Andmete ladu varukoopiate abil saate taastada oma andmebaas taastamine punkti esmane piirkonnas või kasutage geograafilise liigne varukoopiate taastamine geograafiliste muule alale. Selles artiklis selgitatakse taastada andmebaas üksikasjad.

## <a name="what-is-a-data-warehouse-restore"></a>Mis on andmete ladu taastamine?

Andmete ladu taastamine on uus andmebaas, olemasoleva varukoopia põhjal loodud või kustutatud andmebaas. Taastatud andmebaas loob uuesti andmebaas varundatud kindlal ajal. SQL-i andmebaas on jaotatud süsteem, luuakse andmete ladu taastamine varukoopia põhjal palju faile, mis on talletatud Azure plekid. 

Andmebaasi taastamine on mis tahes äri järjepidevus ja avariijärgne taastamine strateegia oluline osa, kuna see loob uuesti juhusliku rikkumise või kustutamise pärast andmete.

Lisateavet leiate teemast:

-  [Varukoopiate SQL-andmebaas](sql-data-warehouse-backups.md)
-  [Äritegevuse järjepidevuse ülevaade](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Andmepunktide ladu taastamine

Kasutades Azure Premium Storage kasu, SQL-i andmebaas kasutab Azure salvestusruumi bloobimälu hetktõmmiseid varundamise esmane andmebaas. Iga hetktõmmise on taastamine punkti, mis tähistab hetktõmmis alustamine aeg. Andmebaas taastamiseks valige Taasta punkti ja anda käsku Taasta.  

SQL-i andmebaas taastab alati varukoopia uus andmebaas. Saate säilitada taastatud andmebaas ja praegune nimi või kustutada üks neist. Kui soovite asendada praegune andmebaas taastatud andmebaas, võite selle ümber nimetada.

Kui soovite taastada kustutatud või peatatud andmebaas, saate [luua tugi Piletite](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Geograafilise liigne taastamine

Kui kasutate geograafilise liigne salvestusruumi, saate taastada [andmepunktipaaride andmekeskuse](../best-practices-availability-paired-regions.md) teises regioonis asuva geograafiliste andmebaas. Andmebaas on taastatud viimase päeva varukoopia põhjal. 

## <a name="restore-timeline"></a>Ajaskaala taastamine

Andmebaasi saab taastada mis tahes saadaval taastamine punkti seitsme viimase päeva jooksul. Hetktõmmiseid käivitage iga neli kaheksa tundi ja on saadaval seitse päeva. Kui hetktõmmise on üle seitsme päeva, aegumist ja selle taastamine punkti pole enam saadaval.

## <a name="restore-costs"></a>Kulude taastamine

Salvestusruumi tasu taastatud andmebaas on arve Azure Premium Storage määra. 

Kui andmebaas on taastatud peatamiseks ostmisega Azure Premium Storage määr salvestusruum. On see eelis, pausi, mitte ostmisega DWU arvuti ressursse.

SQL-i andmebaas hindade kohta lisateabe saamiseks vt [SQL-i andmete ladu hinnad](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Kasutusalad taastamine

Peamine andmete ladu taastamine on andmete taastamine pärast juhuslike andmete kaotsimineku või rikutud.

Saate andmete ladu taastamine varukoopia rohkem kui seitsme päeva säilitada. Kui taastatakse varukoopia, saate andmebaas on võrgus ja saate peatada lõputult salvestamiseks Arvuta kulud. Peatatud andmebaasi andmesidekasutusele salvestusruumi kulude Azure Premium Storage määra. 

## <a name="related-topics"></a>Seotud teemad

### <a name="scenarios"></a>Stsenaariumid

- Business järjepidevuse ülevaate leiate teemast [äritegevuse järjepidevuse ülevaade](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Andmete ladu taastamise teostamiseks taastada abil.

- Azure'i portaalis leiate [Azure'i portaalis andmebaas taastamine](sql-data-warehouse-restore-database-portal.md)
- PowerShelli cmdlet-käskude vaadata, [taastada andmebaas PowerShelli cmdlet-käskude kasutamine](sql-data-warehouse-restore-database-powershell.md)
- Viige API-de kohta leiate teemast [taastamine abil REST API-de andmebaas](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Ülevaade]: ./sql-data-warehouse-restore-database-overview.md
[Portaal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShelli]: ./sql-data-warehouse-restore-database-powershell.md
[ÜLEJÄÄNUD]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
