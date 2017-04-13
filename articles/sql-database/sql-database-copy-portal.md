<properties
    pageTitle="Kopeerige Azure'i portaalis Azure SQL-andmebaasi | Microsoft Azure'i"
    description="Azure'i SQL-andmebaasi koopia loomine"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Azure'i portaalis Azure SQL-andmebaasi kopeerimine

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-copy.md)
- [Azure'i portaal](sql-database-copy-portal.md)
- [PowerShelli](sql-database-copy-powershell.md)
- [T-SQL-IS](sql-database-copy-transact-sql.md)

Järgmised sammud näitavad, kuidas [portaalis Azure](https://portal.azure.com) SQL-andmebaasi kopeerimiseks sama serveri või muu server.

Klõpsake kopeeritavat SQL-andmebaasi vajate järgmist:

- Azure'i tellimuse. Kui teil on vaja Azure tellimuse lihtsalt **Tasuta PROOVIVERSIOON** selle lehe ülaosas nuppu ja siis naaske artiklit lõpuleviimiseks.
- SQL-andmebaasi kopeerida. Kui te ei ole SQL-andmebaasi, luua ühe selles artiklis toodud juhiseid järgides: [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>SQL-andmebaasi kopeerimine

Avage andmebaas, mida soovite kopeerida SQL-i andmebaasi leht:

1.  Avage [Azure'i portaalis](https://portal.azure.com).
2.  Klõpsake nuppu **rohkem teenuseid** > **SQL andmebaase**ja seejärel klõpsake soovitud andmebaas.
3.  Klõpsake lehel SQL-andmebaasi **koopia**.

    ![SQL-andmebaas](./media/sql-database-copy-portal/sql-database-copy.png)

1.  **Kopeerige** lehe on esitatud vaikimisi andmebaasi nimi. Kui soovite, tippige uus nimi (kõik andmebaasid serveris peab olema kordumatu nimi).
2.  Valige **Target serveri**. Target server on, kus on loodud andmebaasi koopia. Saate kopeerida andmebaasi sama server või muu server. Saate luua server või valige olemasoleva serveri loendist. 
3.  Pärast **sihtrakenduse server**, **elastne andmebaasi rakenduskausta**ja **hinnakirjad taseme** suvandite valimise on lubatud. Kui server on, saate sinna kopeerida andmebaasi.
3.  Klõpsake nuppu **OK** , et alustada protsessi koopia.

    ![SQL-andmebaas](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Kopeerimine edenemise jälgimine

- Pärast käivitamist koopia, klõpsake portaali teate üksikasju.

    ![teatis][3]
 
    ![jälgimine][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Veenduge, et andmebaas on reaalajas serveris

- Klõpsake nuppu **rohkem teenuseid** > **SQL andmebaase** ja veenduge, et uus andmebaas on **Online**.


## <a name="resolve-logins"></a>Sisselogimise lahendamine

Lahendamiseks sisselogimise pärast koopia toiming on lõpule jõudnud, lugege teemat [sisselogimise lahendamine](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Järgmised sammud

- Leiate [Azure'i SQL-andmebaasi kopeerimine](sql-database-copy.md) ülevaade kopeerimise Azure'i SQL-andmebaasi.
- Vaadake [Kopeeri Azure'i SQL-andmebaasiga PowerShelli kaudu](sql-database-copy-powershell.md) kopeerimiseks andmebaasi PowerShelli kaudu.
- Vaadake [Kopeeri Azure'i SQL-andmebaasiga abil T-SQL-i](sql-database-copy-transact-sql.md) abil Transact-SQL-i andmebaasi kopeerimiseks.
- [SQL Azure'i andmebaasi turvasätete avariitaastet pärast](sql-database-geo-replication-security-config.md) andmebaasi kopeerimisel eri loogilise server kasutajate ja sisselogimise haldamise kohta leiate teemast.



## <a name="additional-resources"></a>Lisaressursid

- [Sisselogimise haldamine](sql-database-manage-logins.md)
- [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu tegemine](sql-database-connect-query-ssms.md)
- [Andmebaasi eksportimine mõnda BACPAC](sql-database-export.md)
- [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md)
- [SQL-andmebaasi dokumentatsioon](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

