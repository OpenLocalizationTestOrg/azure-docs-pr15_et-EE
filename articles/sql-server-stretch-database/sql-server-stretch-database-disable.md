<properties
    pageTitle="Keelake venitamine andmebaasi ja tuua tagasi remote andmeid | Microsoft Azure'i"
    description="Saate teada, kuidas keelata venitamine andmebaasi tabelisse ja soovi korral taastada remote andmed."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Keelake venitamine andmebaasi ja tuua tagasi remote andmeid

Tabeli venitamine andmebaasi keelamiseks valige tabeli SQL Server Management Studio **venitamine** . Valige üks järgmistest suvanditest.

-   **Keela | Andmeid tuua tagasi Azure'i**. Kopeeri tabel Azure remote andmeid tagasi SQL serveriga, siis keelata venitamine andmebaasi tabeli. Selle toimingu andmesidekasutusele andmete edastamine kulud ja seda ei saa tühistada.

-   **Keela | Jätke andmete Azure**. Keelake venitamine andmebaasi tabeli.  Loobuda remote Azure'i tabeli andmeid.

Saate kasutada ka Transact\-SQL-i keelamine venitamine andmebaasi tabelisse või andmebaasi.

Pärast venitamine andmebaasi keelamine tabeli andmete migreerimise astmed ja päringutulemite enam pärinevate tulemite kaasamiseks serveri tabel.

Kui soovite lihtsalt peatada andmete migreerimise, lugege teemat [Peata ja Jätka venitamine andmebaasi](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Keelamine venitamine andmebaasi tabelisse või andmebaasi remote objekti kustutamine. Kui soovite kustutada serveri tabeli või kaugandmebaas, peate kukutage see Azure haldusportaali abil. Serveri objekte jätkuvalt kanda Azure kulusid kuni kustutamiseni. Lisateavet leiate teemast [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Keelata venitamine andmebaasi tabelisse

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>SQL Server Management Studio abil saate keelata venitamine andmebaasi tabelisse

1.  Valige tabel, mille jaoks soovite keelata venitamine andmebaasi SQL Server Management Studio Exploreris objekti.

2.  Paremal\-klõpsake ja valige **venitada**, ja seejärel valige üks järgmistest suvanditest.

    -   **Keela | Andmeid tuua tagasi Azure'i**. Kopeeri tabel Azure remote andmeid tagasi SQL serveriga, siis keelata venitamine andmebaasi tabeli. See käsk ei saa tühistada.

        >   [AZURE.NOTE] Remote andmete kopeerimine tabeli Azure SQL serveri tagasi andmesidekasutusele andmete edastamine kulud. Lisateavet leiate teemast [Andmete edastamine hinnad üksikasjade](https://azure.microsoft.com/pricing/details/data-transfers/).

        Kui kõik remote andmed kopeeriti Azure tagasi SQL serveri, venitamine on keelatud tabeli.

    -   **Keela | Jätke andmete Azure**. Keelake venitamine andmebaasi tabeli.  Loobuda remote Azure'i tabeli andmeid.

    >   [AZURE.NOTE] Keelamine venitamine andmebaasi tabeli kustutamine remote andmeid või serveri tabel. Kui soovite kustutada serveri tabel, peate kukutage see Azure haldusportaali abil. Serveri tabeli jätkuvalt kanda Azure kulusid seni, kuni need kustutada. Lisateavet leiate teemast [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Kasutage Transact\-SQL-i keelamine venitamine andmebaasi tabeli

-   Tabeli: Azure'i andmed tagasi keelata venitamine tabeli ja kopeerige serveri SQL serveriga, käivitage järgmine käsk. Kui kõik remote andmed kopeeriti Azure tagasi SQL serveri, venitamine on keelatud tabeli.

    See käsk ei saa tühistada.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Remote andmete kopeerimine tabeli Azure SQL serveri tagasi andmesidekasutusele andmete edastamine kulud. Lisateavet leiate teemast [Andmete edastamine hinnad üksikasjade](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Tabeli venitamine keelamine ja loobuda remote andmed, käivitage järgmine käsk.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Keelamine venitamine andmebaasi tabeli kustutamine remote andmeid või serveri tabel. Kui soovite kustutada serveri tabel, peate kukutage see Azure haldusportaali abil. Serveri tabeli jätkuvalt kanda Azure kulusid seni, kuni need kustutada. Lisateavet leiate teemast [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Andmebaasi venitamine andmebaasi keelamine
Enne andmebaasi saate keelata venitamine andmebaasi, on teil keelata üksikute venitamine venitada andmebaasi\-lubatud andmebaasi tabelid.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>SQL Server Management Studio abil saate andmebaasi venitamine andmebaasi keelamine

1.  Valige SQL Server Management Studio Exploreris objekti andmebaasi, mille jaoks soovite keelata venitamine andmebaasi.

2.  Paremal\-klõpsake ja valige **Tööülesanded**ja valige **venitada**ja valige **keelata**.

>   [AZURE.NOTE] Keelamine venitamine andmebaasi andmebaasi kaugandmebaas ei kustutata. Kui soovite kustutada kaugandmebaas, peate kukutage see Azure haldusportaali abil. Kaugandmebaas on jätkuvalt kanda Azure kulusid seni, kuni need kustutada. Lisateavet leiate teemast [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Kasutage Transact\-SQL-i andmebaasi venitamine andmebaasi keelamiseks
Käivitage järgmine käsk.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Keelamine venitamine andmebaasi andmebaasi kaugandmebaas ei kustutata. Kui soovite kustutada kaugandmebaas, peate kukutage see Azure haldusportaali abil. Kaugandmebaas on jätkuvalt kanda Azure kulusid seni, kuni need kustutada. Lisateavet leiate teemast [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Vt ka

[Muuda andmebaasi suvandite seadmine (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Viige ja venitamine andmebaasi jätkamine](sql-server-stretch-database-pause.md)
