<properties
    pageTitle="Ühilduvus tase, kuidas hinnata | Microsoft Azure'i"
    description="Juhised ja tööriistu, kindlaks teha, millised ühilduvus on parim Azure'i SQL-andmebaasi või Microsoft SQL serveri andmebaasi"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Täiustatud päringu jõudluse ühilduvus tase 130 Azure SQL-andmebaasis


SQL Azure'i andmebaas töötab läbipaistev andmebaaside tuhandetele paljude erinevate tasemete, säilitamise ja tagada kõigi oma klientidele tagasiühilduvuse vastava versiooni Microsoft SQL Server!

Seetõttu ei takista klientidele, kes mis tahes olemasolevaid andmebaase muuta uusim ühilduvuse tasemel kasu uue päringu optimeerija ja päringu protsessor funktsioonid. Ajaloo meeldetuletuseks joonduse SQL-i versioonide ühilduvust vaikeõigusetasemed on järgmised:

- 100: SQL Server 2008 ja SQL Azure'i andmebaasi V11.
- 110: SQL Server 2012 ja SQL Azure'i andmebaasi V11.
- 120: SQL Server 2014 ja SQL Azure'i andmebaasi V12.
- 130: SQL Server 2016 ja Azure SQL-i andmebaasi V12.


> [AZURE.IMPORTANT] Alates **juunist – 2016**Azure SQL andmebaasis on vaikimisi ühilduvuse taseme 130 asemel 120 **äsja loodud** andmebaaside jaoks.
> 
> Enne juuni 2016 andmebaase kuvatakse *ei* mõjuta ja kas säilitada oma praeguse ühilduvus (100, 110 või 120). Azure'i SQL-andmebaasi versiooni V11 v12 ei ole oma ühilduvuse taseme migreerimine andmebaasides muutunud kas.


Selles artiklis uurime ühilduvuse taseme 130 ja kuidas seda kasutada nende eeliste eelised. Me tegeleme päringu jõudluse olemasoleva SQL-i rakenduste võimalikud küljel-mõju.


## <a name="about-compatibility-level-130"></a>Ühilduvuse taseme 130 kohta


Esmalt, kui soovite leida ühilduvus praeguse andmebaasi, käivitada järgmine Transact-SQL-lause.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Enne selle muudatuse tasemele 130 juhtub **äsja** loodud andmebaaside jaoks, loome läbi vaadata selle muudatuse, mis on väga lihtsa päringu näited kaudu ja vaadake, kuidas igaüks saab kasu.

Päringu töötlemine relatsioonandmebaasidest võib olla väga keerukas ja võib põhjustada palju infotehnoloogia ja matemaatika omast kujundus valikuid ja käitumist. Selle dokumendi sisu on teadlikult lihtsustatud, et tagada, et igaüks, kellel on mõned minimaalne tehnilise tausta mõista mõju ühilduvuse taseme muutmine ja määrata, kuidas see kasulik rakenduste.

Vaatame, mis toob ühilduvuse taseme 130 tabel pilgu.  Rohkem üksikasju leiate [ühilduvusega](https://msdn.microsoft.com/library/bb510680.aspx)tasemel Muuda andmebaasi (Transact-SQL-i), kuid siin on lühike kokkuvõte.

- Toimingu lisa-select-lause Insert võib olla mitmetoimeline või võib olla paralleelselt leping, kuigi enne selle toimingu oli ühelõimelised.
- Mälu optimeeritud tabel ja tabeli muutujate päringute nüüd on paralleelsete lepingute enne selle toimingu on ka ühelõimelised ajal.
- Ühe muutujaga statistika mälu optimeeritud tabeli nüüd saate proovida ja on automaatselt värskendada. Vt [mis on uut rakenduses andmebaasimootor:-mälu OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) rohkem üksikasju.
- Paketi režiimi v/s rea režiim muudab veeru poe indeksid
  - Järjestab veeru poe index tabelis on nüüd kogumitena.
  - Nüüd tegutseda akendesüsteemide agregaadid kogumitena, nt TSQL LAG/juhi laused.
  - Päringute veeru poe tabelites mitme erinevate punktide töötada kogumitena.
  - Päringud, mis on käivitatud DOP = 1 või järjestikune leping ka käivitada kogumitena.
- Viimati, võimsus hinnang täiustused tulevad tegelikult ühilduvuse taseme 120, aga neile, töötab ühilduvus väiksem (nt 100 või 110), teisaldamist ühilduvuse taseme 130 toob need parandused ja need saavad ka rakenduste päringu täitmise.


## <a name="practicing-compatibility-level-130"></a>Ühilduvuse taseme 130 harjutamine


Esimese vaatame saaksin tabelid, registrid ja juhuslike andmete loodud harjutada mõni järgmised uued võimalused. Azure'i SQL-andmebaasi alusel või SQL Server 2016 saab teostada TSQL skripti näited. Siiski Azure SQL-andmebaasi loomisel veenduge, et valite vähemalt P2 andmebaasi sest teil on vaja vähemalt paar valdkond luba lõimtöötlusele ja nende funktsioonide kasu.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nüüd on mõned päringu töötlemine funktsioonid, mis tulevad ühilduvuse taseme 130 vaadata.


## <a name="parallel-insert"></a>Paralleelne lisamine


Käivitamisel TSQL laused allpool käivitab toimingu lisa ühilduvuse taseme 120 ja 130, mis vastavalt käivitab toimingu INSERT ühe threaded mudeli (120) ja mitmetoimeline mudelis (130).


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Paluda tegelik päringu lepingut, vaadates selle graafiliselt või selle XML-sisu, saate määrata, millised võimsus hinnang on funktsioon Esita. Vaadates selle lepingu-kõrvuti joonisel 1, näeme selgelt, et täitmise veeru poe lisamine läheb järjestikune 120 samal ajal 130. Lisaks tähele, et iteraatori ikoon näitab kahe paralleelselt nooled, mis illustreerivad asjaolu, et nüüd iteraatori täitmise 130 lepingus muudatuse on tõepoolest paralleelselt. Kui teil on suur lisa toimingute lõpule viimiseks, paralleelne, on teie käsutuses andmebaasi, core arvu lingitud täidab paremini; kuni 100 korda kiirem, sõltuvalt teie olukord!


*Joonis 1: Lisa toiming vahetatakse järjestikune samal ajal ühilduvus tase 130.*


![Joonis 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>JÄRJESTIKUNE kogumitena


Samuti teisaldada ühilduvuse taseme 130 töötlemise ajal andmeridade võimaldab Pakktöötlus režiim. Esmalt režiimi toimingud on saadaval ainult kui teil on poe veeruindeksi kohas. Teiseks partii tavaliselt tähistab ~ 900 read, ja kasutab koodi loogika optimeeritud mitmesoonelised CPU, suurem mälumahuga läbilaskevõime ja otse mõjutab veeru poe võimaluse tihendatud andmed. Järgmistest tingimustest SQL Server 2016 saate protsessi ~ 900 read korraga 1 rea ajal asemel ja seetõttu üldist üldkulu toiming on nüüd ühiskasutuses kogu paketi, vähendamise maksumus Real üldine. See ühiskasutusega hulk kombineeritud veeru poe tihendamise põhiliselt vähendab valige paketi režiimi tegevusega seotud latentsus. Saate otsida rohkem üksikasju veeru poe kohta ja partii [Columnstore registrite juhendi](https://msdn.microsoft.com/library/gg492088.aspx)režiim.


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Nagu allpool näha, järgides päringu lepingud – kõrvuti joonisel 2, saame artiklist töötlemine režiim on muutunud ühilduvuse tasemel, ja seetõttu täitmisel päringute nii ühilduvuse taseme täielikult eemaldada, näeme et enamik töötlemise ajal kulub rea režiimis (86%) võrreldes kogumitena (14%), kus 2 pakettidena töödelda. Andmekomplekti suurendada, suureneb kasu.


*Joonis 2: Valige toiming muudatuste kaudu järjestikune kogumitena ühilduvuse taseme 130.*


![Joonis 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Kogumitena sortimine täitmise kohta


Sarnane ülaltoodud rakendatud sortida, kuid kogumitena (ühilduvuse taseme 130) rea režiimi (ühilduvuse taseme 120) üleminek parandab jõudlust sortida põhjustel.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Nähtav-kõrvuti joonisel 3, näeme, et sortida rea režiimis tähistab 81% maksumus, ajal kogumitena moodustab ainult 19% maksumus (vastavalt 81% ja ise sortimine 56%).


*Joonis 3: Sortida vahetatakse rea kogumitena ühilduvuse taseme 130.*


![Joonis 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Ilmselt need näidised sisaldada ainult kümneid tuhandeliste read, mis on midagi, vaadates andmed enamikus SQL-i serverid tänapäeval. Lihtsalt project need miljonid ridade asemel ja see tõlkimise võimaldamiseks täitmise säästa iga päev oma töökoormus laadi ootel mitu minutit.


## <a name="cardinality-estimation-ce-improvements"></a>Arvutite hinnang (CE) täiustused


Kasutusele SQL Server 2014, mis tahes andmebaasiga ühilduvuse taseme 120 või üle teeb uue võimsus hinnang kasutada funktsioone. Sisuliselt võimsus hinnang on loogika abil määrata, kuidas käivitada SQL serveri päringu põhjal selle prognoositud kulud. Hinnang arvutatakse sisend statistika seotud objektidega seotud päring. Praktiliselt üksikasjalik, võimsus hindamine funktsioonide on rea count hinnangulise koos jaotuse väärtused, erinevate väärtuse arvu, teavet ja dubleeritud loendab sisalduvate tabelite ja objektide päringus viidatud. Kuidas need hinnangud valesti, võib põhjustada mittevajalike ketta I/O tõttu mälu toetused (st TempDB lekete) või järjestikune lepingu täitmise valiku üle paralleelselt lepingu täitmise, nimi mõne. Lõpetuseks vale hinnangulise võib kaasa tuua mõne üldise jõudluse vähenemine päringu täitmise. Teisel pool parem hinnangulise täpsemaid hinnanguid, viib parema päringu täitmised!

Nagu enne mainitud, päringu optimeerimine ja hinnangulise on keeruline küsimus, kuid kui soovite lisateavet päringu lepingud ja arvutite prognoos, saate dokumendi [optimeerida oma päringu](https://msdn.microsoft.com/library/dn673537.aspx) plaanid koos SQL serveri 2014 arvutite prognoos süvitsi Dive viidata.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Millised võimsus hinnang praegu kasutada?


Määratlemiseks klõpsake jaotises millist võimsus hinnang päringute töötavad ainult kasutame päringu proovide allpool. Pange tähele, et näide käivitavad jaotises ühilduvuse taseme 110, vana võimsus hindamine funktsioonide kasutamiseks.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Pärast Andmetäite on lõpule jõudnud, klõpsake linki XML-i ja pilk esimese iteraatori atribuutide, nagu allpool näidatud. Märkus nimetatakse praegu määratud 70 CardinalityEstimationModelVersion atribuudi nimi. Ei tähenda, SQL Server 7.0 versioon (see seatakse 110 on nähtavad TSQL aruannete ülaltoodud) on määratud andmebaasi ühilduvuse taseme, et väärtust 70 lihtsalt tähistab pärand võimsus hinnang funktsioonid saadaval alates SQL Server 7.0, mis pole suuri muudatusi kuni SQL serveri 2014 (on kaasas ühilduvuse taseme 120).


*Joonis 4: Selle CardinalityEstimationModelVersion on seatud 70 kasutamisel ühilduvuse taseme 110 või all.*


![Joonis 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Teise võimalusena saate muuta 130 ühilduvuse ja LEGACY_CARDINALITY_ESTIMATION, määrata sees [muuta andmebaasi rakendatud](https://msdn.microsoft.com/library/mt629158.aspx)konfiguratsiooni abil uue võimsus hinnang funktsiooni keelamiseks. See on täpselt sama, mis kasutamisel uusima päringutöötluse ühilduvuse taseme 110 võimsus hinnang funktsioon seisukohast abil. Nii saate uue päringu töötlemise funktsioonid, mis tulevad uusim ühilduvuse taseme (st kogumitena) kasu, kuid ikka toetuvad vana võimsus hinnang funktsioonid vajaduse korral.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Lihtsalt teisaldada ühilduvuse taseme 120 või 130 võimaldab võimsus hinnang uued funktsioonid. Sellisel juhul vaikimisi CardinalityEstimationModelVersion seatakse vastavalt 120 või 130 nagu allpool näha.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Joonis 5: Selle CardinalityEstimationModelVersion on seatud 130 ühilduvuse taseme 130 kasutamisel.*


![Joonis 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Tunnistajaks võimsus hinnang erinevused


Nüüd vaatame Käivita veidi keerulisem päringu seotud INNER JOIN koos WHERE-klauslis mõned predicates ja Heitkem pilk rida count hinnangu vana võimsus hinnang funktsiooni esmalt.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Selle päringu tõhus täitmine tagastab 200,704 read, kuigi rida hinnangu ja vana võimsus hindamine funktsioonide nõuded 194,284 ridu. Ilmselt, nagu mainitud, tulemused rida count sõltub sageduse käivitasite eelmise näidised, mis kuvab ikka ja jälle veebisaidil iga Käivita tabeleid. Ilmselt predicates päringus on ka mõju tegelik hinnang peale tabeli kujundit andmete sisu, ja kuidas neid andmeid tegelikult oleksid omavahel.


*Joonis 6: Rida count hinnang on 194,284 või 6000 rida välja oodatud 200,704 ridadest.*


![Joonis 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Samal viisil Vaatame nüüd päringu sama võimsus hinnang uute funktsioonidega.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Vaadates on allpool, nüüd näeme, et rida on 202,877, või palju täpsem ja vana võimsus hinnang kõrgem.

*Joonis 7: Rida count hinnang on nüüd 202,877 asemel 194,284.*


![Joonis 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


Tegelikult tulem on 200,704 read (kuid kõik sõltub sageduse kas käivitage eelmiste valimite päringud, kuid veelgi olulisem, kuna selle TSQL kasutab RAND() lause, tagastatakse tegelike väärtuste võib erineda korraga järgmise). Seetõttu, näiteks uus võimsus hinnang ei parem töökoht hindamine ridade arvu, kuna 202,877 on palju lähemal 200,704, kui 194,284! Lõpuks, kui soovite muuta, et võrdse top WHERE-klausli (mitte ">" näiteks), see võib teha hinnangulise vahele ja uue arvutite funktsiooni veelgi erinevad sõltuvalt sellest, kui palju vastab pääseb.

Ilmselt sel juhul on ~ 6000 ridade välja tegelik Tähista palju andmeid teatud olukordades. Nüüd on palju suuremad see miljoneid ridu mitmest tabelist ja keerukamate päringute ja aeg-ajalt hinnang saab välja lülitada miljoneid ridu, mistõttu valimise üles valesti täitmise leping või mälu annab TempDB lekete ja nii rohkem i/o, et paluda Transponeeri.

Kui teil on võimalus, tava võrdlus oma kõige tüüpilised päringud ja andmekomplektide, ja vaadake ise, kui suur osa uute ja vanade hinnangulise mõjutab, kuigi mõned lihtsalt muutuvad rohkem välja tegelikult või mõne teistele vaid lihtsalt lähemale tegelik ridade loendab tegelikult tagastatud kogumitena tulemi. Kõik sõltub kuju päringute SQL Azure'i andmebaasi omadused, laadi ja suuruse ja teie andmekomplektide statistikud on saadaval nende kohta. Kui lõite just teie Azure'i SQL-andmebaasi eksemplari, on päringu optimeerija koostamiseks oma teadmisi nullist asemel diagrammide korduvkasutamine statistika eelmise päring käivitatakse valmistatud. Jah, hinnangud on väga kontekstipõhine ja eriomased peaaegu iga server ja rakenduse olukord. See on tähtis meeles pidada!


## <a name="some-considerations-to-take-into-account"></a>Mõned kaalutlused arvesse võtta


Kuigi enamik töökoormus kasu ühilduvuse taseme 130, enne vastuvõtmist ühilduvuse taseme jaoks tootmiskeskkonnast, on teil põhimõtteliselt 3 võimalust.

1. Ühilduvus tasemele 130 teisaldada ja vaadake, kuidas asjad, mida teha. Juhuks, kui märkate, et mõned regressioon, saate lihtsalt tagasi algse tasemele ühilduvuse taseme määramine või säilitada 130 ja ainult tagant võimsus hinnang uuesti pärand režiimis (nagu eespool kirjeldatud, see eraldi võib käsitleda probleemi).
2. Põhjalikult testida olemasolevaid rakendusi koormusega sarnast tootmise, viimistlemiseks ja kinnitage enne tootmise jõudlus. Probleemide korral nagu ülal, saate alati naasta algse ühilduvuse taseme või vastupidises järjekorras võimsus hinnang pärand režiimis.
3. Lõplik valik ja viimase võimalus neid küsimusi, nagu on kasutada päringu pood. See on soovitatav suvand tänase! Aidata oma päringuid jaotises ühilduvus analüüsi tase 120 või all võrreldes 130, ei saa soovitame teil piisavalt päringu poe kasutamiseks. Päringu poe on saadaval Azure SQL-i andmebaasi V12 uusim versioon ja see on mõeldud aitama teil päringu jõudluse tõrkeotsingu. Mõelge päringu poe andmebaasi kogumine ja esitamine kõik päringud ajaloolise lisateabe flight salvesti. See lihtsustab jõudluse kohtumeditsiini vähendades aega diagnoosimine ja probleemide lahendamiseks. Rohkem teavet leiate [päringu poe: andmebaasi flight salvesti](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


At üksikasjalik, kui teil juba on andmebaase, mis töötab ühilduvuse tasemel 120 või alla ja paigutada mõned neist 130 või kuna teie töökoormus automaatselt ettevalmistamise uute andmebaaside, millest saab kiiresti vaikimisi 130, võtke arvesse järgmisi selle:

- Enne uue ühilduvuse tasemel valmistamisel muutmist, lubage päringu pood. Lisateabe saamiseks võib viidata [muuta andmebaasi ühilduvusrežiim ja kasutada päringu pood](https://msdn.microsoft.com/library/bb895281.aspx) .
- Järgmiseks testige kõik kriitilised töökoormus andmeid ja päringute tootmise-like keskkond ja Võrdle jõudlus kogenud ning mille päringu poe kaudu. Kui teil tekib teatud regressioon, saate tuvastada päringu poe taandus päringute ja kasutada sunnib suvand päringu poest leping (ehk leping kinnitamine). Sellisel juhul lõplikult jääda ühilduvuse taseme 130 ning te kasutate endise päringu plaan soovitatud päringu pood.
- Kui soovite kasutada uute funktsioonide ja võimaluste kohta Azure'i SQL-andmebaasi, (mis töötab SQL Server 2016), kuid on tundlik muutuste ühilduvuse taseme 130, viimase võimalusena, võiksite kaaluda sunnib ühilduvuse taseme taas sellele tasemele, mis sobib teie töökoormus Muuda andmebaasi lauset abil. Kuid esmalt olema teadlik päringu poe leping kinnitamine suvandit on teie parim valik, kuna ei kasuta 130 põhiliselt viibib SQL serveri varasem versioon tasemel funktsioone.
- Kui teil on mitu andmebaaside kestvad rentnikuga rakendusi, tuleb värskendada ettevalmistamise loogika andmebaasi taseme ühtsete ühilduvuse tagamiseks üle kõik andmebaasid; äsja ettevalmistatud ja vana neist. Oma rakenduse töökoormus jõudlus olla tundlik asjaolu, et kõik andmebaasid töötavad erinevate tasanditel ja seetõttu võib olla vaja töötada samamoodi oma klientidele kogu tahvli ühilduvuse taseme järjepidevuse üle kõik andmebaasi. Pange tähele, et see ei ole mandaat, on tõesti sõltub kuidas mõjutab rakenduse ühilduvuse tasemele.
- Viimati, võimsus hinnang kohta ja muuta selle ühilduvust enne jätkamist valmistamisel, nagu on soovitatav testida oma tootmise töökoormus uue tingimustel kindlaks teha, kui rakenduse kasu võimsus hinnang täiustused.


## <a name="conclusion"></a>Kokkuvõte


Kõik SQL Server 2016 täiustused kasu Azure'i SQL-andmebaasi abil saate oma päringu täitmised selgelt parandada. Samamoodi nagu-on! Muidugi, nagu mis tahes uus funktsioon õigesti hinnata tuleb teha täpse tingimused, mille teie andmebaasi töökoormus toimib kõige paremini. Kogemus näitab, et enamik töökoormus oodatakse käivituma vähemalt läbipaistev ühilduvuse taseme 130, samal ajal uue päringu töötlemise funktsioonid ja uue võimsus hinnang kasutamine. Mida ütles reaalselt alati on mõned erandid ja tehke proper tähtaja hoolikus on oluline hinnata, kindlaks teha, kui palju saate kasutada järgmisi täiustusi. Ja uuesti, on suureks abiks see töö saab päringu poe!

Nagu SQL Azure'i areneb, võite oodata ühilduvuse taseme 140 tulevikus. Kui aeg on sobiv, alustame räägi, mis toob selle tulevaste ühilduvuse taseme 140, just nagu me lühidalt siin milline ühilduvus 130 toob täna.

Nüüd, vaatame unusta, alates juunist 2016, Azure'i SQL-andmebaasi muutub vaikimisi ühilduvuse taseme 120 130 äsja loodud andmebaaside jaoks. Arvestage!


## <a name="references"></a>Viited


- [Mis on uut rakenduses andmebaasimootor](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Ajaveeb: Päringu poe: flight andmete salvesti andmebaasi Borko Novakovic juuni 8 2016 abil](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [Laused ALTER ühilduvuse tasemele (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [MUUDA ANDMEBAASI JÄÄVATES KONFIGUREERIMINE](https://msdn.microsoft.com/library/mt629158.aspx)

- [SQL Azure'i andmebaasi V12 ühilduvus tase 130](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Päringu optimeerimine lepingute SQL serveri 2014 arvutite prognoos](https://msdn.microsoft.com/library/dn673537.aspx)

- [Columnstore registrite juhend](https://msdn.microsoft.com/library/gg492088.aspx)

- [Ajaveeb: Täiustatud päringu jõudluse ühilduvuse taseme 130 Azure SQL-andmebaasis, Alain Lissoir, võite 6 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
