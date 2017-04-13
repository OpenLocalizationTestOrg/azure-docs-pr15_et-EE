<properties
   pageTitle="SQL serveri venitus andmebaasi Azure TSQL läbipaistvaid andmete krüptimine (TDE) | Microsoft Azure'i"
   description="SQL serveri venitus andmebaasi Azure TSQL läbipaistvaid andmete krüptimine (TDE)"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Läbipaistva andmete krüptimine (TDE) lubamine venitus andmebaasi Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure'i portaal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Läbipaistev krüptimise (TDE) kaitseb pahatahtlik tegevus ohtu nõudmata rakenduse muudatusi reaalajas krüptimine ja dekrüptimine andmebaasi, seotud varukoopiate ja tehingulogi failide veebisaidil ülejäänud järgige järgmisi juhiseid.

TDE krüptib kogu andmebaasi salvestusruumi sümmeetriline võti nimega andmebaasi krüptovõtme abil. Krüptovõtme andmebaas on kaitstud sisemine Serveri sert. Sisseehitatud Serveri sert on kordumatu iga Azure'i server. Microsoft pöörab need serdid automaatselt iga 90 päeva. TDE üldise kirjelduse leiate [Läbipaistev krüptimise (TDE)].

##<a name="enabling-encryption"></a>Krüptimise lubamine

Lubamiseks on Azure TDE andmebaasi, mis on andmete säilitamise migreeritud venitamine lubatud SQL Serveri andmebaas, tehke järgmised üksused:

1. Azure'i serveris hosting abil Logi sisse, et administraator või põhi andmebaasi **dbmanager** rolli liige andmebaasi *juhtslaidi* andmebaasiga ühenduse loomiseks
2. Andmebaasi krüptimine järgmine lause käivitada.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Krüptimise keelamine

TDE keelamiseks on Azure'i andmebaasi, mis on andmete säilitamise migreeritud venitamine lubatud SQL serveri andmebaasi, teha järgmisi toiminguid:

1. Logi sisse, et administraator või põhi andmebaasi **dbmanager** rolli liige abil *juhtslaidi* andmebaasiga ühenduse loomiseks
2. Andmebaasi krüptimine järgmine lause käivitada.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Kinnitatava krüptimine

Azure'i andmebaasi, mis on andmete säilitamise krüptimise olek migreeritud venitamine lubatud SQL serveri andmebaasi kinnitamiseks tehke järgmised üksused:

1. Logi sisse, et administraator või põhi andmebaasi **dbmanager** rolli liige abil *juhtslaidi* või eksemplari andmebaasiga ühenduse loomiseks
2. Andmebaasi krüptimine järgmine lause käivitada.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Tulemus on ```1``` krüptitud andmebaasi, näitab ```0``` näitab-krüptitud andmebaasi.


<!--Anchors-->
[Läbipaistva andmete krüptimine (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
