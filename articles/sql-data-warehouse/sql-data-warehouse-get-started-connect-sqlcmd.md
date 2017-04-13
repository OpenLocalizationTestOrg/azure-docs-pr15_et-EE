<properties
   pageTitle="Päringu SQL Azure'i andmebaas (sqlcmd) | Microsoft Azure'i"
   description="Päringud käsurea kasuliku sqlcmd SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Päringu SQL Azure'i andmebaas (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure'i masina õpetused](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Juhendis kasutab [sqlcmd][] käsurea kasuliku päringu Azure SQL-i andmebaas.  

## <a name="1-connect"></a>1. ühenduse loomine

Alustamine [sqlcmd][], avage käsuviip ja sisestage **sqlcmd** järgneb SQL-i andmebaas andmebaasi ühendusstringi. Ühendusstringi nõuab järgmiste parameetrite abil tehke järgmist.

+ **Server (-S):** Server kujul `<`serveri nimi`>`. database.windows.net
+ **Andmebaasi (-d):** Andmebaasi nimi.
+ **Luba kommenteeritud identifikaatorite (-I):** Pakutud identifikaatorite peab olema lubatud ühenduse SQL-i andmebaas eksemplari.

Kui soovite kasutada SQL serveri autentimine, peate lisama kasutajanime ja parooli parameetrid:

+ **Kasutaja (– A):** Serveri kasutaja vorm `<`kasutaja`>`
+ **Parooli (-P):** Kasutaja parooli.

Näiteks võib teie ühendusstringi välja umbes selline:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory integreeritud autentimise kasutamiseks tuleb kõigepealt lisada Azure Active Directory Parameetrid:

+ **Azure Active Directory autentimine (-G):** kasutada Azure Active Directory autentimine

Näiteks võib teie ühendusstringi välja umbes selline:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Peate lubamiseks [Azure Active Directory autentimise](sql-data-warehouse-authentication.md) autentida Active Directory abil.

## <a name="2-query"></a>2. päringu

Pärast ühenduse, saate välja mis tahes toetatud Transact-SQL-i avaldised vastu eksemplari.  Selles näites esitatakse päringud interaktiivne režiim.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Nende järgmise näited Kuidas käivitada päringute kogumitena suvandiga -Q või toru oma SQL-sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Järgmised sammud

Lisateavet üksikasjad sqlcmd saadaolevate suvandite kohta leiate [sqlcmd dokumentatsiooni][sqlcmd] .

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
