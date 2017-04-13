<properties
    pageTitle="Azure'i SQL-andmebaasi varukoopia põhjal taastamine üheks tabeliks | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i SQL-andmebaasi varukoopia põhjal taastamine üheks tabeliks."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Ühe tabeli Azure SQL-i andmebaasi varukoopia põhjal taastamine

Võivad ilmneda olukorda, kus te eksikombel muudetud mõned andmed SQL-andmebaasis ja nüüd soovite taastada probleemse ühe tabeli. Selles artiklis kirjeldatakse, kuidas ühe tabeli andmebaasi taastamine SQL-andmebaasi [automaatvarunduse](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Ettevalmistavaid toiminguid: nimetab tabeli ümber ja koopia andmebaasi taastamine
1. Leidke tabel Azure SQL-i andmebaasi, mida soovite asendada taastatud koopiaga. Microsoft SQL Management Studio abil nimetab tabeli ümber. Näiteks kui tabeli ümbernimetamine &lt;tabeli nimi&gt;_VANA.

    **Märkus** Blokeeritud vältimiseks veenduge, et ei ole tegevust, töötavate tabel, mis on ümbernimetamine. Kui teil tekib probleeme, veenduge, et selle toimingu käigus hoolduse.

2. Andmebaasi varukoopia taastamiseks punkti aega, mida soovite taastada abil [Punkti-In_Time taastamine](sql-database-recovery-using-backups.md#point-in-time-restore) juhiseid.

    **Märkmete**:
    - Taastatud andmebaasi nime saab DBName + ajatempli vormingus; näiteks **Adventureworks2012_2016-01-01T22-12Z**. See toiming ei kirjuta üle olemasoleva andmebaasi nimi serveris. See on turvalisuse mõõt ja see on mõeldud võimaldavad taastatud andmebaasi veenduge enne nad oma praeguse andmebaasi pukseerida ja pange tootmisotstarbeks taastatud andmebaasi.
    - Kõik jõudluse astme alates lihtsamatest Premium muudatused automaatselt, millal teenus erineva varukoopia säilituspoliitika mõõdikute, olenevalt soovitud taseme:

| DB taastamine | Tavaline taseme | Standardse astme | Premium astme |
| :-- | :-- | :-- | :-- |
|  Ajahetkel taastamine |  Mis tahes taastatava punkti 7 päeva jooksul|Mis tahes taastatava punkti 35 päeva jooksul| Mis tahes taastatava punkti 35 päeva jooksul|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Tabeli kopeerimine taastatud andmebaasi SQL-i andmebaasi migreerimise tööriista abil
1. Laadige alla ja installige [SQL-andmebaasi migreerimine viisard](https://sqlazuremw.codeplex.com).

2. SQL-i andmebaasi migreerimine viisardi lehel **Valige protsessi** avamine, valige **jaotises analüüs/migreerimine andmebaasi**ja seejärel klõpsake nuppu **edasi**.
![SQL-i andmebaasi migreerimine wizard – valige protsess](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. Dialoogiboksis **ühenduse Server** kehtivad järgmised sätted.
 - **Serveri nimi**: teie SQL Azure'i eksemplari
 - **Autentimine**: **SQL serveri autentimine**. Sisestage oma sisselogimisteave.
 - **Andmebaasi**: **juhtslaidi DB (Loetle kõik andmebaasid)**.
 - **Märkus** Vaikimisi viisard salvestab teie sisselogimisteavet. Kui te ei soovi seda, valige **Unustage sisselogimisteavet**.
![SQL-i andmebaasi migreerimine viisardi - valige allikas - samm 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. Dialoogiboksis **Andmeallika valimine** valige taastatud andmebaasi nimi oma allikas jaotise **ettevalmistavaid toiminguid** ja seejärel klõpsake nuppu **edasi**.

    ![SQL-i andmebaasi migreerimine viisardi - valige allikas - etappi 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. Dialoogiboksis **Objektide valimine** suvand **valida kindla andmebaasiobjektide** ja valige, mida soovite migreerida target server table(or tables).
![SQL-i andmebaasi migreerimine wizard – objektide valimine](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. **Skripti viisardi** kokkuvõtteleht, klõpsake nuppu **Jah** kui teilt küsitakse, kas olete valmis luua SQL-skripti kohta. Teil on ka TSQL skripti edaspidiseks kasutamiseks salvestada.
![SQL-i andmebaasi migreerimine wizard – skripti viisardi Kokkuvõte](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. **Tulemite** kokkuvõtteleht, klõpsake nuppu **edasi**.
![SQL-i andmebaasi migreerimine wizard – kokkuvõtte tulemuste](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Lehe **Häälestus Target serveriühenduse** nuppu **Loo ühendus serveriga**ja sisestage üksikasjad järgmiselt:
    - **Serveri nimi**: Target serveri eksemplar
    - **Autentimine**: **SQL serveri autentimine**. Sisestage oma sisselogimisteave.
    - **Andmebaasi**: **juhtslaidi DB (Loetle kõik andmebaasid)**. See suvand on loetletud kõik andmebaasid target serveris.

    ![SQL-i andmebaasi migreerimine wizard – häälestamine Target serveriühenduse](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Klõpsake nuppu **Ühenda**, valige target andmebaas, mida soovite teisaldada tabeli ja seejärel klõpsake nuppu **edasi**. See peaks valmis töö varem loodud script ja näete kopeeritud sihtandmebaasi äsja teisaldatud tabel.

## <a name="verification-step"></a>Kinnitamine sammu
1. Päringu ja testimine veendumaks, et andmed on terve äsja kopeeritud tabel. Pärast kinnitamist, saate kukutage ümbernimetatud tabeli vormi **ettevalmistavaid toiminguid** jaotis. (nt &lt;tabeli nimi&gt;_VANA).

## <a name="next-steps"></a>Järgmised sammud

[SQL-andmebaasi automaatvarunduse](sql-database-automated-backups.md)
