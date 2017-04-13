<properties
    pageTitle="Azure'i portaalis Azure'i SQL-andmebaasi haldamine | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure portaali haldamiseks relatsiooniandmebaasist Azure'i portaalis pilveteenuses."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Azure'i portaalis Azure SQL-i andmebaaside haldamine


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShelli](sql-database-manage-powershell.md)

[Azure'i portaalis](https://portal.azure.com/) võimaldab teil luua, jälgimine ja haldamine SQL Azure'i andmebaasid ja serverid. See artikkel annab kiirülevaate kirjelduse ja lingid põhitoimingud rohkem üksikasju.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Azure SQL-i andmebaasid, serverid ja kaustu vaatamine

Saadaolevate SQL-andmebaasi teenuste, klõpsake nuppu **rohkem teenuseid**ja tippige väljale Otsing **SQL** :

![SQL-andmebaas](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Kuidas luua või SQL Azure'i andmebaasid vaadata?

**SQL-andmebaase** tera avamiseks käsku **SQL-andmebaase**, ja seejärel klõpsake andmebaasi, mida soovite töötada või klõpsake nuppu **+ Lisa** SQL-andmebaasi loomiseks. Lisateavet leiate teemast [SQL-andmebaasi minutiga, kasutades Azure portaali loomine](sql-database-get-started.md).


![SQL-i andmebaasid](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Kuidas luua või Azure SQL-i serverite vaadata?

**SQL-i serverite** tera avamiseks käsku **SQL-i serverid**, ja seejärel nuppu server, mida soovite töötada või klõpsake loomiseks nuppu **+ Lisa** SQL server. Lisateavet leiate teemast [SQL-andmebaasi minutiga, kasutades Azure portaali loomine](sql-database-get-started.md).

![SQL-i serverid](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Kuidas luua või SQL-i elastne kaustu vaadata?

**SQL-i elastne kaustu** tera avamiseks käsku **SQL-i elastne kaustu**, ja seejärel klõpsake kausta, mida soovite töötada või klõpsake nuppu **+ Lisa** loomiseks on. Lisateavet leiate teemast [loomine mõne elastne andmebaasi rakenduskausta Azure'i portaalis](sql-database-elastic-pool-create-portal.md).

![SQL-i elastne kaustu](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Kuidas värskendada või SQL-andmebaasi vaatesätted?

Saate vaadata või värskendada oma andmebaasi sätted, klõpsake SQL-i andmebaasi enne soovitud seade.


![SQL-i andmebaasi sätted](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Kuidas leiavad SQL andmebaase serveri täielik nimi?

Oma andmebaase serveri nime kuvamiseks klõpsake nuppu **Ülevaade** **SQL-andmebaasi** enne ja serveri nimi:


![SQL-i andmebaasi sätted](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Kuidas hallata reegleid tulemüüri juurdepääsu minu SQL server ja andmebaas?

Vaatamine, loomine või värskendamine tulemüüri reeglid, **SQL-andmebaasi** enne klõpsake **serveri tulemüüri seadmine** . Lisateavet leiate teemast [Azure SQL-andmebaasi server taseme tulemüüri reegli Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md).


![tulemüüri reeglid](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Kuidas muuta oma SQL-i andmebaasi taseme või jõudluse teenusetaseme?


SQL-andmebaasi teenuse taseme või jõudluse taseme värskendamiseks klõpsake nuppu **SQL-andmebaasi** enne **hinnakirjad taseme (skaala DTUs)** . Lisateavet leiate teemast [teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) SQL-andmebaasi](sql-database-scale-up.md).


![hinnad astme](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Kuidas seadistada auditeerimine ja ohtu tuvastamine SQL-i andmebaasi?

Auditi- ja ohtu tuvastamine SQL-i andmebaasi konfigureerimiseks nuppu **Valemiauditi ja ohtu tuvastamine** **SQL-andmebaasi** enne. Lisateavet leiate teemast [Alustamine SQL-andmebaasi auditeerimine](sql-database-auditing-get-started.md)ja [SQL-i andmebaasi ohtude tuvastamise alustamine](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Kuidas seadistada SQL-i andmebaasi masking dünaamilise?

Dünaamiliste andmete masking SQL-i andmebaasi konfigureerimiseks klõpsake nuppu **masking dünaamilise** **SQL-andmebaasi** enne. Lisateavet leiate teemast [Alustamine SQL-i andmebaasi dünaamiliste andmete Masking](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Kuidas seadistada SQL-i andmebaasi läbipaistvaid andmete krüptimise (TDE)?

Läbipaistva andmete krüptimine SQL-i andmebaasi konfigureerimiseks klõpsake nuppu **läbipaistvaid andmete krüptimine** **SQL-andmebaasi** enne. Lisateavet leiate teemast [Lubamine TDE andmebaasi portaalis](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Kuidas vaadata või SQL-andmebaasi max suurust muuta?

Saate vaadata või muuta selle suurust SQL-andmebaasi, klõpsake **SQL-andmebaasi** enne **andmebaasi suurus** . Värskendage andmebaasi maksimaalne maht, muutes taseme või jõudluse teenusetaseme. Lisateavet leiate teemast [teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) SQL-andmebaasi](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Kuidas jälgimine ja SQL-andmebaasi jõudluse parandamiseks?

Jälgimine ja SQL-andmebaasi omaduste jõudluse parandamiseks klõpsake **jõudluse ülevaade** **SQL-andmebaasi** enne. Lisateavet leiate teemast [SQL-i andmebaasi jõudluse ülevaate](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Kuidas seadistada Geo-Dispersioonanalüüs?

SQL-andmebaasi Geo-Dispersioonanalüüs seadistamiseks nuppu **SQL-andmebaasi** enne **Geo-Dispersioonanalüüs** . Lisateavet leiate teemast [Konfigureerimine Geo-Dispersioonanalüüs Azure SQL-i andmebaasi Azure'i portaalis](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Kuidas Tõrkesiirde geo kopeeritud SQL-andmebaasiga?

Geo kopeeritud teisese Tõrkesiirde, **Geo-Dispersioonanalüüs** **SQL-andmebaasi** enne ja siis klõpsake **Tõrkesiirde**. Lisateavet leiate teemast [algatada kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Azure'i portaalis](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Kuidas kopeerida SQL-andmebaasi?

SQL-andmebaasi kopeerimiseks klõpsake nuppu **Kopeeri** **SQL-andmebaasi** enne. Lisateavet leiate teemast [Azure portaalis Azure SQL-andmebaasi kopeerida](sql-database-copy-portal.md).


![SQL-i andmebaasi sätted](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Kuidas ma Azure'i SQL-andmebaasiga BACPAC faili arhiivida?

BACPAC SQL-i andmebaasi loomiseks klõpsake **SQL-andmebaasi** enne nuppu **ekspordi** . Lisateavet leiate teemast [arhiivi Azure'i SQL-andmebaasiga BACPAC faili Azure'i portaalis](sql-database-export.md).


![SQL-i andmebaasi eksportimine](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Kuidas taastada SQL-andmebaasi eelmise punkti aega?

SQL-andmebaasi taastamiseks klõpsake raadionuppu **Taasta** **SQL-andmebaasi** enne. Lisateavet leiate teemast [Azure SQL-andmebaasi eelmise punkti Azure portaali ajal taastada](sql-database-point-in-time-restore-portal.md).


![SQL-i andmebaasi sätted](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Kuidas luua Azure'i SQL-andmebaasiga BACPAC faili?

SQL-andmebaasi BACPAC faili loomiseks klõpsake enne **SQL serveri** **andmebaasi Import** . Lisateavet leiate teemast [Azure SQL-andmebaasi loomiseks BACPAC faili importimine](sql-database-import.md).


![SQL serveri](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Kuidas taastada kustutatud SQL-andmebaasi?

Kustutatud SQL-andmebaasi taastamiseks klõpsake raadionuppu **Kustutatud andmebaaside** **SQL serveri** enne (SQL server, kus asunud andmebaas, mida on kustutatud). Lisateavet leiate teemast [Azure portaalis kustutatud SQL Azure'i andmebaasi taastamine](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Kuidas kustutada SQL-andmebaasi?

SQL-andmebaasi kustutamiseks valige **SQL-andmebaasi** enne **kustutada** . 

![SQL-i andmebaasi sätted](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Lisaressursid

- [SQL-andmebaas](sql-database-technical-overview.md)
- [Jälgimine ja haldamine on elastne andmebaasi rakenduskausta Azure'i portaalis](sql-database-elastic-pool-manage-portal.md)
