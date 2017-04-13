<properties
   pageTitle="Levitamise tabelid SQL-i andmebaas | Microsoft Azure'i"
   description="Alustamine levitamise tabelid SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Levitamise tabelid SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Andmetüübid][]
- [Levitamine][]
- [Index][]
- [Partition][]
- [Statistika][]
- [Ajutiste][]

SQL-i andmebaas on oluliselt paralleelne töötlemine (MPP) jaotatud andmebaasi süsteem.  Jagada andmed ja töötlemine võimalus üle mitme sõlmed, SQL-i andmebaas pakkuda suur skaleeritavus - kaugemale mis tahes ühe süsteem.  Ühiskasutuse jagada oma andmeid oma SQL-i andmebaas on üks kõige olulisemaid tegureid optimaalse jõudluse saavutamiseks.   Võti optimaalse jõudluse tagamiseks on minimeerimine andmete liikumine ja omakorda võti minimeerimine andmete liikumine on õige jaotuse strateegia valimine.

## <a name="understanding-data-movement"></a>Ülevaade andmete liikumine

MPP süsteemis on iga tabeli andmed jagatud üle aluseks oleva andmebaasidest.  Kõige optimeeritud päringute MPP abil saab edasi lihtsalt läbi üksikute jaotatud andmebaasid ja muid andmebaase pole omavaheliste käivitada.  Näiteks Oletame, et teil on andmebaasi müügiandmeid, mis sisaldab tabeleid, müügi ja klientidega.  Kui teil on oma Müügitabeli liituda klientide tabeli päringu ja te jagatakse müügi ja klientide tabelite üles kliendi number, panna iga kliendi eraldi andmebaasi, mis tahes päringud, mis liitumine ja klienditeenindus lahendada iga andmebaasis, kellel ei ole teadmisi muid andmebaase.  Seevastu kui teie müügiandmeid jagatud tellimuse number ja klientide arvuga kliendiandmeid, klõpsake mis tahes antud andmebaasi ei ole vastavaid andmeid iga kliendi ja seega kui soovisite liituda oma müügiandmeid kliendiandmeid, peate iga kliendi andmete toomine muid andmebaase.  Selles näites teise andmete liikumine oleks vaja ilmneda kliendi andmete teisaldamiseks müügiandmeid, nii, et saate ühendada kaks tabelit.  

Andmete liikumine ei ole alati halb, mõnikord on vaja lahendada päringu.  Kuid kui selle lisatööd ära hoida, loomulikult päring kestab kiiremini.  Andmete liikumine kõige sagedamini tekib tabeli- või liitmised teostamise.  Sageli peate tegema mõlemat, nii, et samal ajal, kui teil on võimalik, et optimeerida üks stsenaarium, nt ühenduse, peate andmete liikumine muude stsenaariumi, nt liitmise lahendamiseks.  Aga on aru saada, mis on vähem tööd.  Enamikul juhtudel levitamise suurte faktitabelite tihti ühendatud veeru on kõige tõhusam meetod vähendada enamik andmete liikumine.  Andmete ühendamine veergude levitamise on palju ühist vähendada andmete liikumine, kui andmete liitmise kaasatud veergude levitamise.

## <a name="select-distribution-method"></a>Valige jaotuse.

Varjatult SQL-i andmebaas jagab andmete 60 andmebaasid.  Iga üksiku andmebaasi nimetatakse **jaotuse**.  Kui andmed on laaditud igasse tabelisse, SQL-i andmebaas on teada, kuidas oma andmeid jagada üle nende 60 jaotuse.  

Jaotuse on määratletud tabeli tasemel ja praegu on kaks võimalust:

1. **Round jaan** , mis levitada andmeid ühtlaselt, kuid juhusliku ID-ga.
2. **Räsi jaotatud** mis jaotab räsi väärtuste ühe veeru põhjal

Vaikimisi, kui määrate andmete jaotuse meetodi, tabeli jagatakse **round jaan** jaotuse meetodi abil.  Juhul, kui olete saanud keerukamaid oma rakendamist, te soovite kaaluge **räsi jaotatud** tabelite andmete liikumine, mis omakorda optimeerida päringu jõudluse minimeerimiseks.

### <a name="round-robin-tables"></a>Round jaan tabelid

Round jaan meetodil levitamise andmed on väga palju, kuidas see kõlab.  Kui teie andmed on laaditud, saadetakse iga rea lihtsalt järgmise jaotuse.  See meetod levitamise andmed alati juhusliku ID-ga jaotab andmed väga ühtlaselt kõigis selle jaotuse.  Mis on, on sortimine, käigus round jaan, mis paigutab teie andmed on valmis.  Round jaan jaotuse mõnikord nimetatakse juhusliku räsi seetõttu.  Round-jaan jaotatud tabel ei ole vaja mõista andmeid.  Seetõttu Round-jaan tabelite sageli teha hea laadimise sihtkohtade.

Vaikimisi, kui ei jaotuse meetod on valitud, round jaan jaotuse meetodit kasutatakse.  Ajal ringi jaan tabelid on lihtne kasutada, kuna andmeid levitatakse CEIP kogu see tähendab, et süsteemi ei saa tagada mis jaotuse süsteemis on iga rea kohta.  Selle tulemusena süsteemi vahel peab autonoomsest andmete liikumine toiming andmete paremaks korraldamiseks enne selle võib lahendada päringu.  See lisatööd aeglustada oma päringuid.

Kaaluge Round jaan jaotuse tabeli järgmistel juhtudel:

- Kui lihtne lähtepunktina alustamine
- Kui ei ole selge liitumist võti
- Kui ei ole hea testversiooni veeru räsi tabeli levitamine
- Kui tabel ei jaga levinud Liitu klahvi teiste tabelitega
- Kui ühendus on väiksem kui muude Query ühendused
- Kui tabel on ajutine vahekausta tabelis

Mõlemad näited loob Round jaan tabeli:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Round Jaan on vaikimisi tabeli tüüp on esile toodud teie DDL on parimaks peetakse nii, et teie tabeli paigutus jälgitaks teistele Eemalda.

### <a name="hash-distributed-tables"></a>Räsi jaotatud tabelid

Tabelite levitamine **räsi jaotatud** algoritmi abil saate parandada jõudlust palju stsenaariumid vähendades andmete liikumine päringu ajal.  Jaotatud räsi tabelid on tabelid, mis on jagatud jaotatud andmebaase, üks veerg, mis aktiveeritakse loob rakendus algoritmi kasutamine.  Jaotuse veerg on, mis määrab, kuidas andmed on jagatud jaotatud andmebaaside üle.  Funktsiooni räsi kasutab jaotuse veeru read määramiseks jaotuse.  Loob rakendus algoritmi ja tulemuseks jaotuse on tarkadeks.  Mis on sama väärtuse sama andmetüübiga kuvatakse alati on sama jaotuse.    

Selles näites on jaotatud ID tabeli loomiseks tehke järgmist.

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Valige veerg jaotuse.

Kui otsustate tabeli **räsi** levitada, peate ühe jaotuse veeru valimiseks.  Jaotuse veeru valimisel on kolm põhilist mõjutavad tegurid.  

Valige üks veerg, mis on:

1. Ei värskendata
2. Hajutamine andmeid, vältida andmete skew
3. Andmete liikumine minimeerimine

### <a name="select-distribution-column-which-will-not-be-updated"></a>Valige jaotuse veerg, mis ei värskendata

Jaotuse veerud pole värskendatav, seetõttu, valige veeru staatilisi väärtusi.  Kui veerg on vaja värskendada, see pole üldjuhul hea jaotuse testversiooni.  Kui ilmneb juhul, kui peate värskendama jaotuse veeru, saate seda teha esmalt kustutada rida ja seejärel uue rea lisamine.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Valige jaotuse veerg, mis kuvatakse hajutamine andmed

Kuna hajutatud süsteemi sooritab ainult nii kiiresti kui selle parema jaotuse, on oluline jagada töö ühtlaselt üle selle jaotuse kogu süsteemis tasakaalustatud täitmise saavutamiseks.  Kus asub andmed iga jaotuse põhineb töö jagatakse jaotatud süsteemi viisi.  See on väga oluline valida õige jaotuse veeru levitada andmeid, et iga jaotus on võrdne töö ja võtab samal ajal oma töö osa lõpuleviimiseks.  Töö jagatakse ka kogu süsteemis, saabuv andmed on jaotuse.  Kui andmete saabuv pole ühtlaste, kutsume selle **andmete skew**.  

Andmete ühtlaselt jagada ja vältida andmete skew, võtke arvesse järgmist oma jaotuse veeru valimisel:

1. Valige veerg, mis sisaldab palju erinevaid väärtusi.
2. Vältige levitamise erinevate väärtuste veergudes andmeid. 
3. Vältige levitamise andmete veergude kõrge sagedus tühiväärtusi.
4. Vältige levitamise kuupäevaveergude andmeid.

Kuna iga väärtuse on räsitud 60 jaotuse 1, saavutada isegi levitamist kas soovite valige veerg, mis on väga kordumatu ja sisaldab rohkem kui 60 kordumatuid väärtusi.  Kaaluge illustreerimiseks, juhul kui veerg on ainult 40 kordumatuid väärtusi.  Kui see veerg on valitud jaotuse võti, selle tabeli andmed oleks maa 40 jaotuse, jättes 20 jaotuse andmed ja ei töödelda teha.  Seevastu 40 jaotuse oleks rohkem tööd teha, kui andmed on ühtlaselt rohkem kui 60 jaotuse.  See stsenaarium on näide andmeid skew.

MPP süsteemis iga päringuetapi ootab kõik jaotuse oma töö osa lõpuleviimiseks.  Kui üks jaotuse teeb rohkem tööd, kui teised, siis on üldiste müügijaotustega tutvumine ressursi on põhiosas raisatud lihtsalt ootel hõivatud jaotuse.  Kui töö on ühtlaselt laiali kõik jaotuse, kutsume selle **skew töötlemine**.  Töötlemine skew põhjustab päringu käivitamiseks aeglasem kui kui töökoormus saab olema ühtlaselt kõigi selle jaotuse.  Andmete skew viib skew töötlemine.

Vältige levitamise tugevalt nullable veergu, nagu tühjad väärtused kuvatakse kõik maa sama jaotuse. Klõpsake veeru kuupäev levitamise põhjuseks võib olla ka töötlemine skew kuna kõik andmed sisestatud kuupäevale maa sama jaotuse. Kui mitu kasutajat, on tööd päringute kõigi filtrite samal kuupäeval, siis ainult 1 60 jaotuse teeme kogu töö alates antud kuupäevast on ainult üks levitamise kohta. Selle stsenaariumi korral käivitatakse päringute tõenäoliselt 60 korda aeglasem kui andmeid võrdselt jagunevad üle kõik selle jaotuse. 

Kui veerge hea testversiooni olemas, siis kaaluge round jaan nimega jaotuse.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Valige jaotuse veerg, mille minimeerida andmete liikumine

Minimeerimine, valides õige jaotuse veeru andmete liikumine on üks kõige olulisem strateegiad optimeerimine jõudluse oma SQL-i andmebaas.  Andmete liikumine kõige sagedamini tekib tabeli- või liitmised teostamise.  Veerud, mida kasutatakse `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` ja `HAVING` kõik oleks **hea** klauslitest räsi jaotuse leviloendite. 

Teiseks, veerud on `WHERE` klausel ära **ei** tee hea räsi veeru leviloendite puhul kuna nad piirata, millised jaotuse osalemine päringu, põhjustades töötlemine skew.  Hea veerg, mis võib olla ahvatlev jagada, kuid tihti, võivad põhjustada selle töötlemine skew näide on veeru kuupäev.

Üldiselt, kui teil on suur fact tabeleid sageli ühenduse, saad jõudluse jagades ühele Liitu veerge mõlemas tabelis.  Kui teil on tabel, mis on ühendatud kunagi suur fact teises tabelis, otsige veerud, mis on sagedamini selle `GROUP BY` klausel.

On paar klahv kriteeriumid, mis peavad olema täidetud vältida andmete liikumise ajal ühenduse.

1. Seotud tabelite ühendus peab olema jaotatud **ühe** osalemine ühendust veergude räsi.
2. Andmetüüpidega veergude ühendamine peab vastama nii tabelite vahel.
3. Võrdub ärisuhetes tuleb liita veerud.
4. Liitmistüübi ei pruugi mõne `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Tõrkeotsinguteavet skew

Kui tabeli andmeid levitatakse räsi jaotuse meetodil on võimalus, et mõned jaotuse on moonutatud ebaproportsionaalselt rohkem andmeid, kui teised on. Liigse andmete skew võib mõjutada päringu jõudluse, kuna lõplik koos jaotatud päringu peate ootama pikima töötava jaotuse lõpuleviimiseks. Olenevalt andmete skew peate selle lahendamiseks.

### <a name="identifying-skew"></a>Skew tuvastamine

Lihtne viis kindlaks tabeli nagu moonutatud on kasutada `DBCC PDW_SHOWSPACEUSED`.  See on väga kiire ja lihtne viis, mis on talletatud iga 60 jaotamine andmebaasi tabeli ridade arvu kuvamiseks.  Pidage meeles, et tasakaalustatud jõudluse tagamiseks jaotatud tabeli read peaks olema ühtlaselt kõik üldiste müügijaotustega tutvumine.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Juhul, kui teete päringu SQL Azure'i andmebaas dünaamilise halduse vaadete (DMV), saate teha üksikasjalikum analüüs.  Alustamiseks luua abil [Tabeli ülevaade][ülevaate] artiklist SQL [dbo.vTableSizes][] vaade.  Kui vaade on loodud, käivitage see kindlaks teha, millised tabelid on rohkem kui 10% andmete skew.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Andmete skew lahendamine

Mitte kõik skew on piisav alus Paranda.  Mõnel juhul saate tabeli teatud päringute jõudlus üles andmete skew kahju.  Otsustada, kui te peaksite lahendada andmete skew tabelis, peate mõistma võimalikult palju andmemahtudega ja päringute kohta rakenduses oma töökoormus.   Üks võimalus vaadata skew mõju on kasutada juhiseid artiklis [Päringu jälgimise][] skew päringu jõudluse ja spetsiaalselt lõpuleviimiseks klõpsake üksikute üldiste müügijaotustega tutvumine võtta kaua päringutele mõju mõju jälgimiseks.

Andmete levitamine on asi leida õige ülejäänud andmed skew minimeerimine ja minimeerimine andmete liikumine vahel. Neid saab vastu eesmärgid ja mõnikord te soovite säilitada andmete skew vähendamiseks andmete liikumine. Näiteks kui jaotuse veerg on sageli ühiskasutusse antud veeru ühendused ja liitmised, saate kuvatakse olema minimeerimine andmete liikumine. Andmete liikumisel kasu võib üles mõju oleks skew andmed. 

Tüüpilised viis andmete skew lahendamiseks on uuesti luua erinevate jaotuse veeruga tabel. Kuna ei ole võimalik olemasolevasse tabelisse, võimalus muuta tabeli jaotuse jaotuse veerg muuta seda, et see on [CTAS] [] uuesti luua.  Siin on kaks näidet, kuidas lahendada skew andmeid:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Näide 1: Uuesti luua uue jaotuse veeruga tabel

Selles näites kasutatakse [CTAS], [] uuesti luua erinevate räsi jaotuse veeruga tabel. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Näide 2: Looge round jaan jaotusega tabel uuesti

Selles näites kasutatakse [CTAS], [] round jaan asemel räsi jaotuse tabel uuesti luua. Selle muudatuse toodavad isegi andmete jaotuse hinnaga suurema andmete liikumine. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Järgmised sammud

Tabeli kujundus kohta lisateabe saamiseks vt [hajuta][], [Index][], [Partition][], [Andmetüübid][], [statistika][] ja [Ajutised tabelid][ajutine] artikleid.

Head tavad ülevaate leiate teemast [SQL-i andmete ladu head tavad][].


<!--Image references-->

<!--Article references-->
[Ülevaade]: ./sql-data-warehouse-tables-overview.md
[Andmetüübid]: ./sql-data-warehouse-tables-data-types.md
[Levitamine]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Ajutiste]: ./sql-data-warehouse-tables-temporary.md
[SQL-i andmete ladu head tavad]: ./sql-data-warehouse-best-practices.md
[Päringu jälgimine]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
