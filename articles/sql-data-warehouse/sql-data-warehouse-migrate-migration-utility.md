<properties
   pageTitle="Migreerimine: Andmete ladu migreerimise kasuliku | Microsoft Azure'i"
   description="SQL-i andmebaas migreerida."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Andmete ladu migreerimise kasuliku (eelvaade)

> [AZURE.SELECTOR]
- [Migreerimise kasuliku allalaadimine][]

Andmete ladu migreerimise kasuliku on mõeldud SQL Azure'i andmebaas SQL serveri ja Azure'i SQL-andmebaasi skeemi ja andmete migreerida tööriist. Skeemi migreerimisel, kaardid tööriist automaatselt vastava skeemi allikast sihtkohta. Pärast skeemi on migreeritud, tööriistade pakub võimalust andmete automaatselt loodud skriptide abil.

Lisaks skeemi ja andmete migreerimise, annab see tööriist võimalus luua ühilduvus aruandeid, mis summarize vastuolude target ja allika eksemplarid, mis takistab sujuv migreerimise.

## <a name="get-started"></a>Alustamine
Nõutav installi, peate BCP käsurea kasuliku migreerimise skripte ja Office'i ühilduvus aruande kuvamiseks. Pärast käivitamine käivitatava, mis laaditakse palutakse teil enne tööriista installitakse standard EULA aktsepteerima.

Lisaks migreerimise Utiliy käivitamiseks peate üks pärast õiguste andmebaas, mida soovite migreerida: andmebaasi loomine, mis tahes andmebaasi muutmine või Kuva kõik määratlus.

### <a name="launching-the-tool-and-connecting"></a>Tööriista käivitamine ja ühendamine
Käivitage tööriist, klõpsates ikooni, mis kuvatakse postituse installi. Tööriista avamisel palutakse kus saate valida oma lähte-kui ka migreerimise tööriista esmakordse lehega. Sel ajal, on toetame SQL Server ja Azure SQL-andmebaas nimega andmeallikate ja SQL-i andmebaas sihtkoht. Pärast seda palutakse serveriga ühenduse loomiseks oma andmeallika täitmine serveri nimi ja autentimine ning seejärel klõpsata käsku "Ühenduse loomine".

Pärast autentimist, tööriist kuvatakse loendi andmebaasidele, mis on olemas serveris, mis on seotud. Migreerimise alustamist, valides andmebaas, mida soovite migreerida ja seejärel klõpsates "Migreerimine valitud".

## <a name="migration-report"></a>Migreerimise aruanne
Valige kontrolli andmebaasi ühilduvust tööriista loob aruannet kokkuvõttev sõnum kõik objekti vastuolude andmebaasi saate migreerida. Laiemas loendi mõnda SQL serveri funktsioone, mida ei ole SQL-i andmebaas leiate meie [migreerimise dokumentatsiooni][]. Pärast aruanne on loodud, siis saab salvestada ja avage aruanne Exceli.

Pange tähele, et migreerimise skeemiga, enamik probleemid tuvastatud "Objekt" saab kohandada, et luba vahetu migreerimine andmeid loomisel. Palun vaadake üle tagamaks, mida soovite teha täiendavaid parandusi enne rakendamist skeemi muudatusi.

## <a name="migrate-schema"></a>Skeemi migreerimine

Pärast ühenduse, valides migreerimine skeemi loob skeemi migreerimise skript valitud tabelite. Selle skripti pordid struktuuri tabeli kaartide kohta rohkem ühilduvad vormide tüübid ja loob turvalisus mandaadi- ja -skeemifailid, kui seda näitab kasutaja migreerimise sätteid. Järgmine kood saab käivitada vastu suunatud SQL-i andmebaas eksemplari, salvestada faili, teie lõikelauale kopeerida või isegi redigeerida-line edasised toimingud enne.  

Nagu eespool mainitud, kui migreerimine skeemi läbivaatus migreerimise muutub, mis tööriist on tehtud tagamaks, et et mõistate täielikult neid.  

## <a name="migrate-data"></a>Andmete migreerimine

Andmete migreerimine suvand klõpsates saate luua BCP skriptide, mis teisaldab esmalt teie andmete kindla failid oma serverisse ja seejärel otse oma SQL-andmebaas. Soovitame seda protsessi vähehaaval andmete teisaldamine ja nimega korduskatsed on pole sisseehitatud ja tõrkeid võivad ilmneda juhul, kui võrguühenduse kaob. Käivitage see, et peate olema installitud BCP käsurea kasuliku ja andmete skeemi peab juba loodud.

Pärast ülaltoodud parameetritel täita peate lihtsalt nuppu Käivita migreerimine ja kogumi pakettide loob teie määratud asukohta. Andmete eksportimine migreerimise allikas tasapinnalise failide ekspordifail käivitama ja käivitama impordifaili andmete importimine SQL-i andmebaas.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete viiakse mõned andmed, lugege teemat Kuidas [arendada][].

<!--Image references-->

<!--Article references-->
[migreerimise dokumentatsioon]: sql-data-warehouse-overview-migrate.md
[töötada]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Migreerimise kasuliku allalaadimine]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
