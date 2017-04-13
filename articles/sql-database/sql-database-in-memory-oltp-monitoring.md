<properties
    pageTitle="Jälgimine XTP-mälu salvestusruumi | Microsoft Azure'i"
    description="Hinnangu ja kuvari XTP-mälu salvestusruumi kasutada, võimsuse; tõrke võimsus 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Kuvari-mälu OLTP salvestusruum

[-Mälu OLTP](sql-database-in-memory.md)kasutamisel andmete mälu optimeeritud tabelid ja tabeli muutujate asub-mälu OLTP salvestusruumi. Iga Premium teenuse kiht on suurim lubatud mälu-OLTP mälumaht, mis on [SQL-i andmebaasi teenuse astme artikkel](sql-database-service-tiers.md#service-tiers-for-single-databases). Kui see piirang on ületatud, lisada ja värskendada toimingute alustada ei ole (viga 41823). Sel hetkel on vaja kas kustutamiseks andmete kettaruumi mälu või jõudluse taseme andmebaasi täiendamine.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Määratlemine, kas andmed on mahuks-mälu ots

Määratleda salvestusruumi ots: [SQL-i andmebaasi teenuse astme artiklis](sql-database-service-tiers.md#service-tiers-for-single-databases) salvestusruumi suurtähelukk erinevatele Premium teenuse tasemetele, küsige.

Hindamine mälu nõuded mälu optimeeritud tabeli töötab samal viisil nagu see SQL Server ei Azure SQL-andmebaasis. Kuluda mõni minut üle vaadata selle teema [MSDN](https://msdn.microsoft.com/library/dn282389.aspx)-is.

Pange tähele, et tabel ja muutuv Tabeliridade, samuti registrid, arvestata max kasutaja andmete maht. Lisaks ALTER TABLE tuleb luua uus versioon, mille terve tabel ja selle registrid piisavalt ruumi.

## <a name="monitoring-and-alerting"></a>Jälgimine ja teavitamine

Saate jälgida-mälu kasutamine protsendina soovitud [salvestusruumi piiramise oma jõudluse kiht](sql-database-service-tiers.md#service-tiers-for-single-databases) Azure [portaali](https://portal.azure.com/): 

- Enne andmebaasi, leidke väljal ressursi kasutamise ja klõpsake Redigeeri.
- Valige mõõdiku `In-Memory OLTP Storage percentage`.
- Teatise lisamiseks ressursi kasutamine ruut argumendil tera avamiseks, klõpsake käsku Lisa teatis.

Või kasutada järgmist päringut-mälu kasutamine kuvamiseks tehke järgmist.

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Õige mälu-olukordades - 41823 tõrge

Töötab mälu-tulemused lisamine, värskendamine ja luua toiminguid ei tõrketeatega 41823.

Tõrketeade 41823 näitab mälu optimeeritud tabelid ja tabeli muutujate on ületanud lubatud maht.

Selle tõrke, kas:


- Andmete kustutamine mälu optimeeritud tabelid potentsiaalselt mahalaadimine andmed traditsiooniline, kettapõhiste tabelid; või,
- Võtke kasutusele teenuse kiht üks piisavalt-mälu salvestusruum säilitada tabeli mälu optimeeritud vajalikud andmed.

## <a name="next-steps"></a>Järgmised sammud
Lisaressursid kohta [jälgimise Azure'i SQL-andmebaasi kasutamise dünaamilise halduse vaadete](sql-database-monitoring-with-dmvs.md) kohta
