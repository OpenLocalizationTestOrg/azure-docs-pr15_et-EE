<properties
   pageTitle="SQL-i andmebaas varukoopiate | Microsoft Azure'i"
   description="Teavet SQL-i andmebaas sisseehitatud andmebaasi varundamine, mis võimaldavad teil taastamine punkti või muu geograafilise piirkonna Azure SQL-i andmebaas taastada."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>Varukoopiate SQL-andmebaas

SQL-i andmebaas pakub kohalikke ja geograafiliste varukoopiate oma andmete ladu varukoopia võimaluste osana. Nendeks on Azure salvestusruumi bloobimälu hetktõmmiseid ja geograafilise liigne salvestusruumi. Teie andmebaas taastamiseks taastamine punkti esmane piirkonna või taastamiseks teises regioonis asuva geograafiliste andmete ladu varukoopiate abil. Selles artiklis selgitatakse varukoopiaid SQL-i andmebaas üksikasjad.

## <a name="what-is-a-data-warehouse-backup"></a>Mis on varukoopia andmete ladu?

Varukoopia andmete ladu on andmed, mida saate taastada andmebaas konkreetse aja.  SQL-i andmebaas on jaotatud süsteem, varukoopia andmete ladu sisaldab palju faile, mis on talletatud Azure plekid. 

Andmebaasi varundamine on mis tahes äri järjepidevus ja avariijärgne taastamine strateegia oluline osa, sest andmete kaitsmine juhusliku rikkumise või kustutamise kaudu. Lisateavet leiate teemast [Business järjepidevuse ülevaade](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Andmete koondamine

SQL-i andmebaas kaitseb teie andmeid, andmete talletamise kohalikult liigsete (LRS) Azure Premium mälu. Funktsioon Azure Storage talletab mitme sünkroonse eksemplari andmed kohaliku andmekeskuse selleks, et tagada läbipaistev andmekaitse, kui seal on lokaliseeritud ebaõnnestumist. Andmete koondamine tagab, et teie andmed võib jääda taristu küsimustega ketta ebaõnnestumist. Andmete koondamine tagab järjepidevuse abil veaks salliv ja väga kättesaadav taristu.

Lisateavet:

- Azure Premium mälu, lugege teemat [Azure Premium Storage tutvustus](../storage/storage-premium-storage.md).
- Kohalik liigsete salvestusruumi, lugege teemat [Azure Storage kopeerimine](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure'i bloobimälu salvestusruumi hetktõmmiseid

Kasutades Azure Premium Storage kasu, SQL-i andmebaas kasutab Azure salvestusruumi bloobimälu hetktõmmiseid varundamise kohalik andmebaas. Saate taastada andmebaas hetktõmmise taastamine punkti. Hetktõmmiseid käivitage vähemalt iga kaheksa tundi ja on saadaval seitse päeva.  

Lisateavet:

- Azure'i bloobimälu pilte, lugege teemat [loomine bloobimälu hetktõmmise](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Geograafilise liigne varukoopiate

Iga 24 tunni järel SQL-i andmebaas talletatakse täielik andmebaas kindlad. Täielik andmebaas on loodud mängu ajal, viimase hetktõmmis. Standardse salvestusruumi kuulub geograafilise liigne salvestusruumi konto koos lugemisõigus (RA GRS). Funktsiooni Azure'i salvestusruumi RA-GRS tiražeerib varukoopia failid on [seotud data Centeri kaudu](../best-practices-availability-paired-regions.md). Geo-kopeerimist tagab saate taastada andmebaas juhuks, kui tõmmised ei pääse juurde teie peamine regioonis. 

See funktsioon on vaikimisi sisse lülitatud. Kui te ei soovi kasutada geograafilise liigne varukoopiate, saate valida. 

>[AZURE.NOTE] Azure'i salvestusruumi, viitab Termini *kopeerimine* failide kopeerimine ühest kohast teise. Hoides mitme teisene andmebaaside sünkroonitud esmane andmebaasi viitab SQL's *andmebaasi tiražeerimine* . 

Lisateavet:
- Geograafilise liigne salvestusruumi, lugege teemat [Azure Storage kopeerimine](../storage/storage-redundancy.md).
- RA-GRS salvestusruumi, lugege teemat [lugemisõigus – geograafilise liigne](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Andmete ladu varukoopia ajakava ja säilitusperiood

SQL-i andmebaas loob hetktõmmiseid teie võrgus andmete ladu iga neli kaheksa tundi ja hoiab iga hetktõmmise seitse päeva. Saate taastada ühendusega andmebaasi taastamine punktiga seitsme viimase päeva jooksul. 

Viimase hetktõmmise käivitamisel vaatamiseks selle päringu käitamist online SQL-i andmebaas. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Kui vajate rohkem kui seitsme päeva hetktõmmise, saate taastada taastamine punkti uus andmebaas. Kui taastamine on lõpule jõudnud, käivitatakse SQL-i andmebaas hetktõmmiseid luua uus andmebaas. Kui te ei tee muudatusi uus andmebaas, hetktõmmiste püsida tühja ja seetõttu hetktõmmise kulu on minimaalne. Samuti võib Viige hiirekursor andmebaasi SQL-i andmebaas takistada hetktõmmiseid luua.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Mis juhtub minu varukoopia säilitamise ajal minu andmebaas on peatatud?

SQL-i andmebaas luua hetktõmmiseid ja ei lõpe hetktõmmiseid ajal andmebaas on peatatud. Kui andmebaas on peatatud hetktõmmise vanus ei muuda. Andmebaas on veebis, mitte kalendri päeva päevade arv põhineb hetktõmmise säilitamist.

Näiteks kui hetktõmmise käivitab oktoober 1 kell 4 ja andmebaas on peatatud oktoober 3, 4 kell, hetktõmmis on kaks päeva vana. Iga kord, kui andmebaas sisaldab võrguühenduse taastamisel hetktõmmis on kaks päeva vana. Kui andmebaas on veebis oktoober 5 kell 4, hetktõmmis on kaks päeva vana ja 5 päeva jääb.

Kui andmebaas sisaldab võrguühenduse taastamisel, SQL-i andmebaas kasutataks uue hetktõmmiseid ja lõpeb hetktõmmiseid luua, kui neil pole üle seitsme päeva andmed.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Kui kaua on kaotatud andmebaas säilitatakse?
Kui andmebaas katkeb, andmebaas ja tõmmised on salvestatud seitse päeva ning seejärel eemaldada. Mis tahes salvestatud taastamine punktide saate taastada andmebaas.

> [AZURE.IMPORTANT] Kui kustutate loogilise SQL serveri eksemplar, kõigile andmebaasidele, mis kuuluvad näiteks kustutatakse ka ja ei saa taastada. Te ei saa taastada kustutatud server.

## <a name="data-warehouse-backup-costs"></a>Andmete ladu varukoopia kulud

Teie peamine andmebaas ja seitse päeva Azure'i bloobimälu pilte kogumaksumuse ümardatakse lähima TB. Näiteks kui teie andmebaas on 1,5 TB ja tõmmised kasutada 100 GB, on arve andmete Azure Premium Storage määrade 2 TB. 

>[AZURE.NOTE] Iga hetktõmmise on tühi algselt ja kasvab muudate esmase andmebaas. Kõik pilte suurenemine suurus andmete ladu muutmine. Seetõttu salvestusruumi kulud hetktõmmiseid kasvata määras muutmine.

Kui kasutate geograafilise liigne salvestusruumi, kuvatakse eraldi salvestusruumi tasuta. Arve esitatud geograafilise liigne salvestusruumi standardmäär lugemisõigus – geograafiliselt liigsete salvestusruumi (RA GRS).

SQL-i andmebaas hindade kohta lisateabe saamiseks vt [SQL-i andmete ladu hinnad](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Kasutades andmebaasi varundamine

SQL-i ladu varufailide esmane kasutamine on taastamiseks mõne punkti Taasta andmebaas säilitamise aja jooksul.  

- SQL-i andmebaas taastamiseks vaadake [taastamine SQL-i andmebaas](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Seotud teemad

### <a name="scenarios"></a>Stsenaariumid

- Business järjepidevuse ülevaate leiate teemast [äritegevuse järjepidevuse ülevaade](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Andmebaas taastamiseks vaadata, [taastada SQL-i andmebaas](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

