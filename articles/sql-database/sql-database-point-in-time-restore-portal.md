<properties
    pageTitle="Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajas (Azure'i portaal) | Microsoft Azure'i"
    description="Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajal."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Seisu Azure'i portaalis eelmises punktis Azure SQL-andmebaasi taastamine


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-recovery-using-backups.md)
- [Punkti õigel ajal taastamine: PowerShelli](sql-database-point-in-time-restore-powershell.md)

Selles artiklis kirjeldatakse andmebaasi taastamine varasemasse ajas: [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md) Azure'i portaalis.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>SQL-andmebaasi taastamiseks eelmises punktis ajas

Valige andmebaasi taastamine Azure'i portaalis.

1.  Avage [Azure'i portaalis](https://portal.azure.com).
2.  Valige ekraani vasakus servas **rohkem teenuseid** > **SQL andmebaase**.
3.  Klõpsake andmebaasi, mille soovite taastada.
4.  Teie andmebaasi lehe ülaosas valige **Taasta**.

    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  **Taastamine** lehel Valige kuupäev ja kellaaeg (UTC aega) andmebaasi taastamine ja seejärel klõpsake nuppu **OK**.

    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Taastetoimingu jälgimine

1. Pärast nupu **OK klõpsamist** eelmises etapis, lehe paremas ülanurgas ikooni teatis ja klõpsake **taastamine SQL-andmebaasi** teatise üksikasju.

    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Avaneb taastamine SQL-i andmebaasi leht, kus Taasta olekuteavet. Võite klõpsata nuppu reaüksuse üksikasjalikumat teavet.

    ![Azure'i SQL-andmebaasi taastamine](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Järgmised sammud

- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
