<properties
   pageTitle="Alustamine ajaline tabelid SQL Azure'i andmebaasi | Microsoft Azure'i"
   description="Saate teada, kuidas alustada Azure'i SQL-andmebaasi ajaline tabelite abil."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Ajaline Azure SQL-andmebaasi tabelitega töötamise alustamine

Ajalise tabelid on uus funktsioon programmeerimisega Azure'i SQL-andmebaasi, mis võimaldab teil jälgida ja analüüsida teie andmete jaoks kohandatud kodeerimise vajamata muudatuste täieliku ajaloo. Ajalise tabelite säilitada andmete tihedalt seotud aja kontekstis nii, et seda võidakse tõlgendada salvestatud faktide kehtivaks ainult teatud aja jooksul. Selle atribuudi ajaline tabelite võimaldab tõhusa ajapõhiste analüüsi ja andmete uuendus saamisega teadmisi.

##<a name="temporal-scenario"></a>Ajalise stsenaarium

Selles artiklis iseloomustab toiminguid, mida tuleb kasutada ajaline tabelite rakenduse stsenaariumi puhul. Oletame, et soovite jälgida kasutaja tegevuste uuel veebisaidil, mis on välja töötatud nullist või olemasoleva veebisaidi, mida soovite laiendada kasutaja tegevuste Analytics. Lihtsustatud näites me oletame, et aja jooksul külastatud veebilehtede arv on näitaja, mis tuleb registreerimise ning jälgida veebisaidi andmebaasi, mis on majutatud Azure'i SQL-andmebaasi. Kasutaja tegevuste ajalooliste analüüsi eesmärk on saada sisendina ümberkujundamiseks veebisaidi külastajad ja parem kogemus.

Selle stsenaariumi andmebaasi mudel on väga lihtne – kasutaja tegevuste meetermõõdustik on esindatud ühe täisarvuväli **PageVisited**, ja on jäädvustatud koos kasutaja profiili põhiteabe. Lisaks ajaline analüüsiks oleks hoiate sarja rida iga kasutaja jaoks, kus igal real on teatud kasutaja külastatud teatud aja jooksul lehtede arv.

![Skeemi](./media/sql-database-temporal-tables/AzureTemporal1.png)

Õnneks pole vaja luua mis tahes peegeldav rakenduse selle tegevuse teabe haldamiseks. Ajalise tabelitega selle protsessi on automatiseeritud - annab teile täielik paindlikkus veebisaidi kujundus ja rohkem aega keskenduda ise Andmeanalüüs ajal. Tagada, et **WebSiteInfo** tabel on konfigureeritud on ainus tuleb teha [ajaline süsteemi versiooniga](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Allpool on kirjeldatud täpsete juhiste ajaline tabelite kasutada seda stsenaariumi.

##<a name="step-1-configure-tables-as-temporal"></a>Samm 1: Konfigureerida tabelite ajaline

Sõltuvalt sellest, kas olete alates uue või olemasoleva rakenduse täiendamine, kuvatakse ajaline tabelite loomine või muuta olemasolevaid, lisades ajaline atribuute. Üldiselt juhul stsenaariumist võib olla kombinatsiooni need kaks võimalust. Toimingu need [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) või muude Transact-SQL-i arengu tööriista abil.


> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Uue tabeli loomine

Kasutada kontekstimenüüst kirje "Uus süsteemi versiooniga tabel" SSMS objekti Exploreri avamiseks päringuredaktori ajaline tabeli malli script ja asustada malli "Määrata väärtuste jaoks malli parameetrid" (Ctrl + Shift + M) abil:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

Valitud SSDT, "Ajaline tabel (süsteemi-versiooniga)" malli uute üksuste lisamisel andmebaasi projekt. Mis avage tabel designer ja abil saate hõlpsalt määrata tabeli paigutus.

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Samuti saate ajaline tabeli loomine, määrates Transact-SQL-lausete otse, nagu on näidatud järgmises näites. Pange tähele, et iga ajaline tabeli kohustuslikud osad perioodi määratluse ja SYSTEM_VERSIONING klausel viitega teise kasutaja tabeli, mis salvestab rida ajaloolisi versioone:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Süsteemi versiooniga ajaline tabeli loomisel luuakse automaatselt lisatud nurjumiskirjet vaikimisi konfiguratsioon. Vaikimisi nurjumiskirjet sisaldab rühmitatud B-puu indeks perioodi veergude (algus, lõpp) lehe tihendamine lubatud. Selle konfiguratsiooni puhul on optimaalne enamik stsenaariumid, kus kasutatakse ajaline tabelid, eriti jaoks [andmete kontrollimine](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

Käesoleval juhul, eesmärk sooritada ajapõhiste trendi analüüsi üle rohkem andmeid ajalugu ja suurem andmekogumi, nii, et ajalugu tabeli salvestusruumi valik on rühmitatud columnstore register. Rühmitatud columnstore pakub hea tihendamise ja jõudluse analytical päringuid. Ajalise tabelite annab teile paindlikkuse konfigureerimiseks registrid praeguse ja ajaline tabelid täielikult sõltumatult. 

**Märkus**: Columnstore registrite saadaval ainult premium teenuse kiht.

Järgmise skripti näitab, kuidas vaikimisi index ajalugu tabeli saab muuta rühmitatud columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Ajalise tabelid on esindatud objekti Exploreri teatud ikoon tuvastamise hõlbustamiseks, ajal kuvatakse selle tabeli ülataseme.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Olemasolev tabel ajaline muuta

Vaatame katta alternatiivne stsenaarium, kus WebsiteUserInfo tabel juba olemas, kuid pole loodud muutuste ajalugu säilitada. Sel juhul saate lihtsalt laiendada olemasolevasse tabelisse saada ajaline, nagu on näidatud järgmises näites:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Samm 2: Käivitumise oma töökoormus

Ajalise tabelite eelis on, et teil pole vaja muuta või kohandada oma veebisaidi mis tahes viisil teha muutuste jälituse. Kui loodud, ajaline tabelite läbipaistev püsida eelmistes Real iga kord, kui muudatusi sooritada oma andmete. 

Selleks, et kasutada automaatse muutuste jälituse kindla stsenaariumi, vaatame lihtsalt värskendage veeru **PagesVisited** iga kord, kui kasutaja lõpetab oma seansi veebisaidil.

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

See on oluline Pange tähele, et värskenduspäringu ei vaja teada täpse kellaaja tegelik toiming ilmnemise ega kuidas andmeid säilitatakse tulevaste analüüsi jaoks. Azure'i SQL-andmebaasi töödeldakse automaatselt nii aspekte. Järgmine diagramm näitab, kuidas andmeid on loodud igal värskendamisel.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Samm 3: Ajalooliste andmeanalüüside

Nüüd, kui ajaline süsteemi-Versioonimine on lubatud, ajalooliste Andmeanalüüs on ainult ühe päringu eemale. Selles artiklis anname mõned näited selle aadressi analüüsi tavastsenaariumid – kogu teave, Avasta klausliga [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) kasutusele erinevaid võimalusi.

Ülemine 10 kasutajat, tellitud külastatud veebilehtede tunni taguse seisuga arvu kuvamiseks käivitage see päring:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Saab hõlpsasti muuta selle päringu analüüsiks kohapealseid päevast, tagasi, kuu tagasi või mis tahes hetkel varem soovite.

Eelmisele päevale lihtsa statistilise analüüsi tegemine, kasutage järgmises näites:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Otsing mõne kindla kasutaja tegevuse teatud aja jooksul, kasutage SISALDAS IN-klauslit:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Pildi visualiseering on eriti mugav ajaline päringuid, nagu saate kuvada ja -trendide mustreid intuitiivne viis väga lihtne:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Päevakohaste tabeli skeemi

Tavaliselt, peate ajaline tabeli skeemi muuta, kui teete rakenduste arendamiseni. Selleks lihtsalt käivitage tavaline ALTER TABLE laused ja Azure SQL-andmebaasi õigesti ütleb muutuste ajalugu tabelisse. Järgmise skripti näitab, kuidas saate lisada täiendavad atribuut jälgimise:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Samuti saate muuta veeru määratlus oma töökoormus on aktiivne.

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Lisaks saate eemaldada veeru, mida te enam ei vaja.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Teise võimalusena muuta ajaline tabeli skeemi ühenduses (ühendusega režiimi) või andmebaasi projekti (ühenduseta režiim) uusim [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) abil.

##<a name="controlling-retention-of-historical-data"></a>Ajalooliste andmete säilitamine juhtimine

Süsteemi versiooniga ajaline tabelitega, võib nurjumiskirjet suurendada andmebaasi suurus rohkem kui tavaline tabelid. Suur ja järjest nurjumiskirjet võib muutuda nii puhas salvestusruumi ning jõudlust määramise tõttu käibemaksu ajaline päringute probleemi. Seega arendamise säilituspoliitika andmete haldamise ajalugu tabeli andmed on oluline osa kavandamise ja haldamise elutsükli iga ajaline tabel. Azure'i SQL-andmebaasi, on teil järgmised võimalused ajaline tabelis ajalooliste andmete haldamise.

- [Tabeli jagamine](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Kohandatud Kettapuhastus skripti](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Järgmised sammud

Ajalise tabelite kohta leiate üksikasjalikumat teavet leiate [MSDN-i dokumendid](https://msdn.microsoft.com/library/dn935015.aspx).
Külastage kanali 9 [tegelik klient ajaline implemenation edu lugu](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ja vaadake [vahetu ajaline tutvustamise](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
