<properties
    pageTitle="Azure'i SQL-andmebaasi taastamine automaatvarunduse (Azure'i portaal) | Microsoft Azure'i"
    description="Azure'i SQL-andmebaasi taastamine automaatvarunduse (Azure'i portaal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Azure'i SQL-andmebaasi taastamine Azure'i portaalis Automaatne varundamine


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-recovery-using-backups.md#geo-restore)
- [Geo-taastamine: PowerShelli](sql-database-geo-restore-powershell.md)

Selles artiklis näidatakse, kuidas taastada andmebaasi uus server on [automaatse varundamise](sql-database-automated-backups.md) [Geo-taastamine](sql-database-recovery-using-backups/.md#geo-restore) Azure'i portaalis.

## <a name="select-a-database-to-restore"></a>Valige andmebaasi taastamine

Azure'i portaalis andmebaasi taastamiseks tehke järgmist.

1.  Avage [Azure'i portaalis](https://portal.azure.com).
2.  Klõpsake ekraani vasakus servas valige **+ Uus** > **andmebaaside** > **SQL-andmebaasi**:

    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Valige allikas **varukoopia** ja valige varukoopia, mille soovite taastada. Määrake andmebaasi nimi, mida soovite taastada, andmebaasi server ja klõpsake nuppu **Loo**.
  
    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-geo-restore-portal/geo-restore.png)

Jälgida olekut taastetoimingu ülemise lehe paremas ülanurgas ikooni teatis. 


## <a name="next-steps"></a>Järgmised sammud

- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
