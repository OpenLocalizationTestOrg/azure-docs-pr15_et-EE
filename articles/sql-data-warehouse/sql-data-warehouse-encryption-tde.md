<properties
   pageTitle="Läbipaistva andmete krüptimine SQL-i andmebaas (portaal) | Microsoft Azure'i"
   description="SQL-i andmebaas läbipaistvaid andmete krüptimine (TDE)"
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

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Töö alustamine koos läbipaistev krüptimist (TDE) rakenduses SQL-andmebaas

> [AZURE.SELECTOR]
- [Turvalisuse ülevaade](sql-data-warehouse-overview-manage-security.md)
- [Autentimine](sql-data-warehouse-authentication.md)
- [Krüptimise (portaal)](sql-data-warehouse-encryption-tde.md)
- [Krüptimise (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Nõutav Permssions

Läbipaistev krüptimise (TDE) lubamiseks peate olema administraator või dbmanager rolli liige.

## <a name="enabling-encryption"></a>Krüptimise lubamine

SQL-i andmebaas TDE lubamiseks tehke järgmist:

1. Avage andmebaas, [Azure'i portaal](https://portal.azure.com)
2. Andmebaasi tera, klõpsake nuppu **sätted**
3. Valige suvand **läbipaistev andmete krüptimine**![][1]
4. Valige säte **kohta**![][2]
5. Valige **Salvesta**
![][3]  

## <a name="disabling-encryption"></a>Krüptimise keelamine

SQL-i andmebaas TDE keelamiseks tehke järgmist:

1. Avage andmebaas, [Azure'i portaal](https://portal.azure.com)
2. Andmebaasi tera, klõpsake nuppu **sätted**
3. Valige suvand **läbipaistev andmete krüptimine**![][1]
4. Valige säte **väljalülitamine**![][4]
5. Valige **Salvesta**
![][5]  

## <a name="encryption-dmvs"></a>Krüptimise DMVs

Järgmised DMVs abil saab kinnitada krüptimise:

- [sys.Databases]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
