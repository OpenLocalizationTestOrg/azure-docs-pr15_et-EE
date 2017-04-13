<properties
    pageTitle="SQL-i andmebaasi jõudluse häälestamine näpunäited | Microsoft Azure'i"
    description="Näpunäiteid jõudluse häälestamine Azure SQL-andmebaasis hindamise ja parandamise kohta."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL-i jõudluse parandamine, andmebaasi jõudluse häälestamine: SQL-i jõudluse häälestamine näpunäiteid, SQL-i andmebaasi jõudluse häälestamine"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>SQL-andmebaasi jõudluse häälestamine näpunäited
Saate muuta [teenuse kiht](sql-database-service-tiers.md) ühe andmebaasi või suurendada eDTUs mõne elastne andmebaasi pool igal ajal jõudluse parandamiseks, kuid soovite leida võimalusi parandada ja optimeerida päringu jõudluse esmalt. Puuduvad registrite ja halvasti optimeeritud päringute kirjeldame mõningaid levinud põhjuseid kehva andmebaasi jõudluse. Sellest artiklist leiate juhised jõudluse häälestamine SQL-andmebaasis.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Juhised annavad ja viimistleda andmebaasi jõudlus
1.  [Azure portaali](https://portal.azure.com), käsku **SQL-i andmebaasid**, valige andmebaas ja otsige lähenemas rohkem ressursse jälgimis diagrammi abil. Vaikimisi kuvatakse DTU tarbimine. Klõpsake nuppu **Redigeeri** väärtused ja ajavahemiku muutmiseks.
2.  Kasutage [Päringu jõudluse ülevaate](sql-database-query-performance.md) hindamaks, kasutades DTUs päringuid ja seejärel kasutage [SQL-i andmebaasi Advisor](sql-database-advisor.md) soovitused loomise ja registrite pukseerimine, parameterizing päringute ja probleemide skeemi probleemide kuvamiseks.
3.  Saate saada jõudluse parameetrid reaalajas dünaamiline haldusvaated () ja laiendatud sündmusi (Xevents) SSMS päringu pood. Lugege üksikasjalikku jälgimiseks ja näpunäited häälestamine [jõudluse juhiseid teemas](sql-database-performance-guidance.md) .


    > [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Veel ressursse andmebaasi jõudluse parandamiseks
1.  Ühe andmebaaside jaoks, saate [muuta teenuse astme](sql-database-scale-up.md) nõudmisel andmebaasi jõudluse parandamiseks.
2.  Mitme andmebaaside, kaaluge [elastne andmebaasi kaustu](sql-database-elastic-pool-guidance.md) mastaapimiseks ressursid automaatselt.
