<properties
   pageTitle="Päringu poe Azure SQL-andmebaasis opsüsteem"
   description="Siit saate teada, kuidas toimida päringu poe Azure SQL-andmebaasis"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Opsüsteem Azure SQL-andmebaasis päringu pood 

Päringu poe Azure on täielikult hallatud andmebaasi pidevalt kogub ja ajaloolise lisateabe kõik päringud. Mõelge päringu poe nimega sarnane on lennukikujuline flight salvesti oluliselt lihtsustab päringu jõudluse tõrkeotsingu mõlema puhul pilveteenuste ja kohapealse kliendid. Selles artiklis selgitatakse päringu poe Azure teatud elemente. Kasutades eelnevalt kogutud päringu andmed, saate kiiresti diagnoosimine ja jõudluse probleemide lahendamiseks ja seega rohkem aega oma ettevõtte keskendudes. 

Päringu poe on [globaalselt saadaval](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) Azure SQL-andmebaasis November 2015. Päringu poe on jõudluse analüüs ja häälestamine funktsioone, näiteks [SQL-i andmebaasi Advisor ja jõudluse armatuurlaua](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)alus. Selles artiklis avaldamise hetkel töötab päringu poe üle 200 000 kasutaja andmebaaside Azure, mitu kuud, katkestusteta päringu seotud teabe kogumine.

> [AZURE.IMPORTANT] Microsoft on aktiveerimise päringu poe kõigi Azure SQL-i andmebaaside (olemasoleva ja uus). 

## <a name="optimal-query-store-configuration"></a>Optimaalne päringu poe konfigureerimine

Selles jaotises kirjeldatakse optimaalse konfiguratsiooni vaikesätted, mis on loodud päringu salvestamine ja sõltuvad funktsioone, näiteks [SQL-i andmebaasi Advisor ja jõudluse armatuurlaua](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)usaldusväärne töö tagamiseks. Vaikimisi konfiguratsioon on optimeeritud pidev andmete kogumine, mis on minimaalne ajakulu väljas/READ_ONLY Ühendriikides.

| Konfigureerimine | Kirjeldus | Vaikimisi | Kommentaari |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Määrab päringu võib võtta sees z kliendi andmebaasi andmete alale limiit | 100 | Jõustatud uue andmebaasi jaoks |
| INTERVAL_LENGTH_MINUTES | Määratleb ajaakna jooksul kogutud käitusaja statistika päringu režiimi kokku liita ja püsis suurust. Iga aktiivse päringu lepingul on kõige ühe rea selle konfiguratsiooni määratud aja jooksul | 60   | Jõustatud uue andmebaasi jaoks |
| STALE_QUERY_THRESHOLD_DAYS | Ajapõhiste Kettapuhastus poliitika, mis määrab säilitusperiood, nõutud käitusaja statistika ja passiivsed päringud | 30 | Jõustatud uue andmebaasi ja andmebaaside eelmise vaikimisi (367) |
| SIZE_BASED_CLEANUP_MODE | Saate määrata, kas automaatne andmete puhastamine toimub, kui päring poe andmete maht lähenedes limiit | AUTOMAATNE | Jõustatud kõigile andmebaasidele |
| QUERY_CAPTURE_MODE | Saate määrata, kas kõik päringud või alamhulka päringute jälgitakse | AUTOMAATNE | Jõustatud kõigile andmebaasidele |
| FLUSH_INTERVAL_SECONDS | Saate määrata, mis jäädvustata käitusaja statistika hoitakse mällu enne loputamist kettale | 900 | Jõustatud uue andmebaasi jaoks |
||||||

> [AZURE.IMPORTANT] Nende vaikesätete rakendatakse automaatselt viimasel etapil päringu poe aktiveerimine kõik SQL Azure'i andmebaasid (vt enne olulise teabe märge). Pärast üles sellest Azure'i SQL-andmebaasi ei muutuda konfiguratsiooni väärtustele, mille kliendid, juhul, kui nad negatiivselt esmane töökoormus või usaldusväärne tegevuse päringu pood.

Kui soovite jääda oma kohandatud sätteid, abil [Päringu poe suvandeid Muuda andmebaasi](https://msdn.microsoft.com/library/bb522682.aspx) taastada konfiguratsiooni endise. Tutvuge [Päringu poe soovitused](https://msdn.microsoft.com/library/mt604821.aspx) selleks, et saate teada, kuidas algusse valitud optimaalse konfiguratsiooni parameetrid.

## <a name="next-steps"></a>Järgmised sammud

[SQL-i andmebaasi jõudluse tutvustus](sql-database-performance.md)

## <a name="additional-resources"></a>Lisaressursid

Lisateavet vaadake läbi järgmistest artiklitest:

- [Andmebaasi flight salvesti](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Jõudluse kontrollimise poe päringu abil](https://msdn.microsoft.com/library/dn817826.aspx)

- [Päringu poe kasutamise stsenaariumid](https://msdn.microsoft.com/library/mt614796.aspx)

- [Jõudluse kontrollimise poe päringu abil](https://msdn.microsoft.com/library/dn817826.aspx) 
