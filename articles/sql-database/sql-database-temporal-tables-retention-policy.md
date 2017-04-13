<properties
   pageTitle="Ajalooliste ajaline tabeli koos säilituspoliitika andmete haldamine | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada ajaline säilituspoliitika ajalooliste andmete oma kontrolli all hoidmine."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Ajalooliste ajaline tabeli koos säilituspoliitika andmete haldamine

Ajaline tabelite võib suurendada andmebaasi suurus rohkem kui tavaline tabelid, eriti siis, kui saate säilitada ajalooliste andmeid pikema aja jooksul. Seega on säilituspoliitika ajalooliste andmete jaoks oluline osa kavandamise ja haldamise elutsükli iga ajaline tabel. Ajaline Azure'i SQL-andmebaasi tabelid on lihtne kasutada säilitamise süsteem, mis aitab teil selle ülesande.

Ajalise ajaloo säilitamine võib olla konfigureeritud tasemel üksikute tabel, mis võimaldab kasutajatel luua paindlikke vanade poliitikat. Ajalise säilituspoliitika rakendamine on lihtne: see nõuab ainult üks parameeter seadmiseks ajal tabeli loomise või skeemi muutmine.

Pärast ülesande määratlemist säilituspoliitika, hakkavad Azure'i SQL-andmebaasi regulaarselt kontrollimine, kas ajalooliste ridu, mis on õigus saada automaatse andmete Kettapuhastus. Vastendatud read identifitseerimise ja nende eemaldamist nurjumiskirjet esineda läbipaistev, tausta tööülesande, mis on ajastatud ja käivitage süsteem. Vanuse tingimus ajalugu tabeli read on märgitud tähistav SYSTEM_TIME perioodi lõpu veeru alusel. Kui säilitusperiood, näiteks on seatud kuus kuud, õigus Kettapuhastus Tabeliridade vastama järgmine tingimus:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

Ülaltoodud näites me eeldatakse, et **ValidTo** veeru vastab SYSTEM_TIME lõppu.

##<a name="how-to-configure-retention-policy"></a>Kuidas seadistada säilituspoliitika?

Enne kui saate konfigureerida säilituspoliitika ajaline tabeli, kontrollige esmalt, kas ajaline ajaloo säilitamine on lubatud *andmebaasi tasemel*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Andmebaasi lipp **is_temporal_history_retention_enabled** on vaikimisi sees, kuid kasutajad saavad muuta selle Muuda andmebaasi lause. See automaatselt väärtuseks Väljas pärast [punkti aja taastamine](sql-database-point-in-time-restore-portal.md) . Ajalise ajaloo säilitamine Kettapuhastus andmebaasi lubamiseks Käivita järgmine tekst:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Isegi juhul, kui **is_temporal_history_retention_enabled** on väljas, kuid vähemalt ridade automaatne puhastus ei käivitatakse sel juhul saate konfigureerida säilituspoliitika ajaline tabelite jaoks.


Säilituspoliitika on konfigureeritud tabeli loomise ajal, määrates HISTORY_RETENTION_PERIOD parameetri väärtus:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure'i SQL-andmebaasi saate määrata erinevaid abil säilitusperiood: päeva, nädala, kuude ja aastate. Kui HISTORY_RETENTION_PERIOD puudub, eeldatakse LÕPMATU säilitus. Saate kasutada ka LÕPMATU märksõna otseselt.

Mõnel juhul võib-olla soovite konfigureerida säilituspoliitika pärast tabeli loomist või muutmiseks eelnevalt konfigureeritud väärtus. Sel juhul kasutamine lause ALTER TABLE

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Seadmine SYSTEM_VERSIONING *ei säilitata* säilituspoliitika periood väärtuse väljas. Säte SYSTEM_VERSIONING sees ilma HISTORY_RETENTION_PERIOD määratud konkreetselt tulemuste LÕPMATU säilitusperiood.

Säilituspoliitika hetkeseisu läbi kasutada järgmist päringut, mis ühendab ajaline säilituspoliitika võimaldamine lipp koos säilituspoliitika perioodide üksikud tabelid tasemel andmebaasi:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Kuidas SQL-andmebaasi kustutab vanuses ridade?

Kettapuhastus sõltub nurjumiskirjet index paigutust. See on oluline Pange tähele, et *ainult ajaloo tabelid rühmitatud indeks (B-puu või columnstore) võib olla piiratud säilituspoliitika konfigureeritud*. Tausta tööülesande luuakse kõik ajaline tabelid ja piiratud säilitusperiood vähemalt andmete puhastamine teha.
Kettapuhastus loogika rowstore (B tree) rühmitatud indeks kustutab vähemalt rea väiksemate tükkideks (kuni 10 K) minimeerimine andmebaasi Logi ja I/O alasüsteemi. Kuigi puhastus loogika kasutab nõutav B-puu index, kustutamised read, mis on vanemad kui säilitusperiood ei saa tagada kindlalt järjestuse. Seega *võtta mis tahes sõltuvus rakenduste Kettapuhastus järjestuses*.

Rühmitatud columnstore cleanup ülesanne eemaldab kogu [rea rühmad](https://msdn.microsoft.com/library/gg492088.aspx) korraga (tavaliselt sisaldavad read iga 1 miljonit), mis on väga tõhus, eriti siis, kui andmeid luuakse kiiresti.

![Rühmitatud columnstore säilitus.](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Suurepärane andmete tihendamise ja tõhusa säilituspoliitika puhastus muudab rühmitatud columnstore indeks stsenaariumid parim valik kui teie töökoormus kiiresti loob suur hulk ajaloolisi andmeid. Selle muster on tüüpilised intensiivse [selgituseks töötlemine töökoormus kasutavad ajaline tabelite](https://msdn.microsoft.com/library/mt631669.aspx) muutuste jälituse ja auditeerimine, trendi analüüsi või asjade andmete manustamisest.

##<a name="index-considerations"></a>Registri kaalutlused

Tabelid rowstore rühmitatud indeks cleanup ülesanne nõuab index alustada SYSTEM_TIME perioodi lõpu vastav veerg. Kui sellise pole olemas, ei saa konfigureerida piiratud säilitusperiood:

*Sõnum 13765, tase 16 riigi 1 <br> </br> seadmine piiratud säilitusperiood süsteemi versiooniga ajaline tabeli "temporalstagetestdb.dbo.WebsiteUserInfo" nurjus, kuna nurjumiskirjet 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' ei sisalda nõutud rühmitatud indeks. Kaaluge võimalust luua rühmitatud columnstore või B-puu index alustades veerg, mis vastab SYSTEM_TIME lõppu perioodi ajalugu tabeli.*

See on oluline Pange tähele, et vaikimisi nurjumiskirjet Azure'i SQL-andmebaasi juba loodud on rühmitatud indeks, mis vastab säilituspoliitika jaoks. Kui proovite eemaldada selle indeksi tabeli piiratud säilitusperiood, toiming nurjub järgmine tõrketeade:

*Sõnum 13766, tase 16 riigi 1 <br> </br> ei saa langev rühmitatud indeks 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory', kuna seda kasutatakse automaatne cleanup vähemalt andmed. Kaaluge HISTORY_RETENTION_PERIOD lõpmatu vastava süsteemi versiooniga ajaline tabeli kui teil on vaja kukutage see indeks.*

Rühmitatud columnstore indeks kettapuhastus töötab optimaalselt, kui lisatakse ajalooliste read on tõusvas järjestuses (tellida perioodi lõpus), mis on alati juhul, kui nurjumiskirjet sisustatakse üksnes, SYSTEM_VERSIONIOING süsteem. Kui ajalugu tabeli ridade ei on tellinud perioodi veeru (mis võib juhtuda siis, kui te viiakse varasemate olemasolevate andmete), peaksite looma uuesti rühmitatud columnstore index peal B-puu rowstore indeks, mis on õigesti tellitud optimaalse jõudluse saavutamiseks.

Ärge taastamine rühmitatud columnstore index ajalugu tabeli piiratud säilitusperiood, kuna võivad muutuda, tellimine rea jaotiste loomulikult kehtestanud süsteemi-versioonimise toiming. Kui teil on vaja taastada rühmitatud columnstore indeks nurjumiskirjet teha, et uuesti luua nõuetele vastavuse B-puu index, säilitades tellimine vajalik tavaline andmete puhastamine rowgroups peal. Kui loote olemasoleva ajaloo tabel, mis on rühmitatud veeru indeks tagatud andmeid tellimuse ilma ajalise tabeli rakendada sama lähenemisviisi:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Piiratud säilitusperiood konfigureerimisel rühmitatud columnstore indeks nurjumiskirjet, ei saa luua täiendavaid kobar B-tree registrid selle tabeli:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Ülaltoodud lause execute katse ei õnnestu järgmine tõrketeade:

*Sõnum 13772, tase 16 riigi 1 <br> </br> ei saa luua-rühmitatud indeks ajaline ajalugu tabeli "WebsiteUserInfoHistory" Kuna see on piiratud säilitusperiood ja rühmitatud columnstore indeks.*

##<a name="querying-tables-with-retention-policy"></a>Päringute säilituspoliitika tabelid

Kõik päringud ajaline tabeli automaatselt filtreerida ajalooliste ridade sobitamine piiratud säilituspoliitika vältimiseks ettearvamatuid ja vastuolulised tulemused, kuna vähemalt read saate kustutada cleanup ülesanne, *mis tahes hetkel aega ja järjekorras*.

Järgmisel pildil on kujutatud lihtsa päringu päringu kavandamine:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Päringu leping sisaldab veeru periood (ValidTo) täiendavad filtrid "Rühmitatud indeks skannimine" tehtemärk ajalugu tabeli (esile tõstetud). Selles näites eeldab see 1 kuu säilitusperiood oli määratud WebsiteUserInfo tabeli.

![Säilituspoliitika päringu filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Juhul, kui saate päringu nurjumiskirjet otse, võidakse kuvada ridu, mis on vanemad kui määratud säilituspoliitika aja jooksul, kuid ilma mingit garantiid jaoks korratavad päringutulemite. Järgmisel pildil on kujutatud ajalugu tabeli päringu täitmise kava päringu ilma rakendatud filtreid:

![Päringute ajalugu ilma säilitus filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Sõltuvad teie äriloogika lugemine nurjumiskirjet Lisaks säilitusperiood saad vastuolulised või ootamatuid tulemusi. Soovitatav on kasutada ajaline päringute FOR SYSTEM_TIME klausliga ajaline tabeli andmete analüüsimiseks.

##<a name="point-in-time-restore-considerations"></a>Punkti aja taastamine kaalutlused

Kui loote uue andmebaasi [taastamine olemasoleva andmebaasi kohast ajas](sql-database-point-in-time-restore-portal.md), on keelatud andmebaasi tasemel ajaline säilitus. (**is_temporal_history_retention_enabled** lipu seadmine väljas). Selle funktsiooni abil saate vaadata kõiki ajalooliste ridade korral taastada, ilma muret vähemalt read eemaldatakse enne saate need päringu. Saate kontrollida *Lisaks konfigureeritud säilitusperiood andmeid*.

Öelda, et ajaline tabel on ühe kuu säilituspoliitika määratud ajavahemiku jooksul. Kui teie andmebaas on loodud Premiumi astme, mida oleks võimalik luua andmebaasi koopia andmebaasi olek üles 35 päeva tagasi minevikus. Mis tõhus võimaldab teil ajalooliste ridu, mis on kuni 65 päeva alusel päringute nurjumiskirjet otse analüüsimiseks.

Kui soovite aktiveerida säilituspoliitika ajaline cleanup, käivitage järgmine Transact-SQL-lause pärast punkti aja taastamine:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas kasutada tutvuge ajaline tabelid rakenduste [Ajaline Azure'i SQL-andmebaasi tabelitega töötamise alustamine](sql-database-temporal-tables.md).

Külastage kuulen [tegelik klient ajaline rakendamist edu lugu](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ja vaadake [vahetu ajaline tutvustamise](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)kanali 9.

Üksikasjalikku teavet ajaline tabelid, lugege läbi [MSDN-i dokumendid](https://msdn.microsoft.com/library/dn935015.aspx).
