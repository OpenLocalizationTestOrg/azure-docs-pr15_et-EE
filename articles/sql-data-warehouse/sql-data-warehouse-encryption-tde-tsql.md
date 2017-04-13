<properties
   pageTitle="Läbipaistva andmete krüptimine SQL-i andmebaas (T-SQL-i) | Microsoft Azure'i"
   description="Läbipaistva andmete krüptimine (TDE) SQL-i andmebaas (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Töö alustamine läbipaistev andmete krüptimise (TDE)


> [AZURE.SELECTOR]
- [Turvalisuse ülevaade](sql-data-warehouse-overview-manage-security.md)
- [Autentimine](sql-data-warehouse-authentication.md)
- [Krüptimist (portaal)](sql-data-warehouse-encryption-tde.md)
- [Krüptimise (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Nõutav Permssions

Läbipaistev krüptimise (TDE) lubamiseks peate olema administraator või dbmanager rolli liige.

## <a name="enabling-encryption"></a>Krüptimise lubamine

SQL-i andmebaas TDE lubamiseks tehke järgmist

1. *Juhtslaidi* sisse login, mis on administraator või põhi andmebaasi **dbmanager** rolli liige abil andmebaasi majutava serveri andmebaasiga ühenduse loomiseks
2. Andmebaasi krüptimine järgmine lause käivitada.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Krüptimise keelamine

SQL-i andmebaas TDE keelamiseks tehke järgmist

1. Logi sisse, et administraator või põhi andmebaasi **dbmanager** rolli liige abil *juhtslaidi* andmebaasiga ühenduse loomiseks
2. Andmebaasi krüptimine järgmine lause käivitada.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Peatatud SQL-andmebaas peab enne muudatuste tegemist TDE sätted jätkata.

## <a name="verifying-encryption"></a>Kinnitatava krüptimine

Kinnitamiseks krüptimise olek SQL-i andmebaas, järgige alltoodud juhiseid:

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

## <a name="encryption-dmvs"></a>Krüptimise DMVs  

- [sys.Databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
