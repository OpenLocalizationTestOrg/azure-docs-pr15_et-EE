<properties
    pageTitle="Taasta kustutatud Azure SQL-andmebaas (Azure'i portaal) | Microsoft Azure'i"
    description="Taastada kustutatud Azure SQL-andmebaasi (Azure'i portaal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Kustutatud portaalis Azure SQL Azure'i andmebaasi taastamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-recovery-using-backups.md)
- [**Taasta kustutatud DB: portaal**](sql-database-restore-deleted-database-portal.md)
- [Taasta kustutatud DB: PowerShelli](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Valige andmebaasi taastamine 

Azure'i portaalis kustutatud andmebaasi taastamine

1.  [Azure'i portaal](https://portal.azure.com), klõpsake nuppu **rohkem teenuseid** > **SQL-i serverid**.
3.  Valige server, kus asunud andmebaasi, mille soovite taastada.
4.  Liikuge kerides jaotiseni **toimingute** oma serveri laba ja valige **Kustutatud andmebaasid**: ![Azure SQL-andmebaasi taastamine](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Valige andmebaas, mille soovite taastada.
6.  Määrake andmebaasi nimi ja klõpsake nuppu **OK**.

    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Järgmised sammud

- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
