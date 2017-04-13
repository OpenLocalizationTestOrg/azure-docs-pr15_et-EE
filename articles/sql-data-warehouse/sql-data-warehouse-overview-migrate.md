<properties
   pageTitle="Teie lahendus migreerimine SQL-i andmebaas | Microsoft Azure'i"
   description="Migreerimise juhiseid SQL Azure'i andmebaas platvormi teie lahendus toomiseks."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Teie lahendus migreerimine SQL-andmebaas

SQL-i andmebaas on jaotatud andmebaasi süsteem, mis elastically skaala teie vajadustele. Säilitada nii jõudlus ja skaala, rakendatakse kõiki funktsioone SQL serveri SQL-i andmebaas sees. Järgmistes teemades migreerimise puudutada mõned olulised tegurid teie lahendus migreerimine SQL-i andmebaas. Andmete ladu skaala kujundamise tutvustab erinevate mustrite ja seega traditsiooniline võimalustest pole alati kõige paremini kujundus. Võite leida seetõttu, et kohandada oma olemasoleva lahenduse tagab teile ära jaotatud platvormi Saadaolevad SQL-i andmebaas.

See on oluline meeles pidada, et SQL-i andmebaas on põhjal Microsoft Azure'i platvormi. Seetõttu migreerimise osa võib hõlmata ka andmeedastuse pilveteenusesse. Andmeedastus on omaette teema ja tuleks kaaluda; eriti siis, kui mahu suurendamine. Andmete edastamiseks ja andmete laadimine on eraldi teemad.

## <a name="migration-guidance"></a>Migreerimise juhised

Veenduge, et olete läbi lugenud artiklitest mõistate mõne toote erinevusi ja olulise põhimõtet enne migreerimise tagamiseks kaudu.

- [Oma skeemi migreerimine][]
- [Andmete migreerimine][]
- [Migreerimine oma kood][]

## <a name="next-steps"></a>Järgmised sammud

KASS (klient nõu meeskonnatöö) on ka hea SQL-i andmebaas juhised mis need avaldada ajaveebide kaudu.  Heitke pilk oma artikli täiendavad juhised migreerimise [SQL Azure'i andmebaas praktikas Migrating andmed][] .

<!--Image references-->

<!--Article references-->
[Oma skeemi migreerimine]: sql-data-warehouse-migrate-schema.md
[Andmete migreerimine]: sql-data-warehouse-migrate-data.md
[Migreerimine oma kood]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migreerimine SQL Azure'i andmebaas praktikas andmed]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
