<properties
   pageTitle="Indekseerimise tabelid SQL-i andmebaas | Microsoft Azure'i"
   description="Alustamine rakenduses SQL Azure'i andmebaas indekseerimine tabel."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Indekseerimise tabelid SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Andmetüübid][]
- [Levitamine][]
- [Index][]
- [Partition][]
- [Statistika][]
- [Ajutiste][]

SQL-i andmebaas pakub mitu Indekseerimissuvandid, sh [rühmitatud columnstore registrid][], [rühmitatud registrid ja nonclustered registrid][].  Lisaks pakub see on index võimalus ka [hunnik][].  Selles artiklis antakse ülevaade iga registri tüüp kui ka nõuanded saada oma registrite jõudluse eelised. Vaadake kuidas tabeli loomine SQL-i andmebaas [loomine tabeli süntaks][] üksikasju.

## <a name="clustered-columnstore-indexes"></a>Rühmitatud columnstore registrid

Vaikimisi loob SQL-i andmebaas kui suvandid puuduvad indeks on ära toodud tabeli klaster columnstore register. Rühmitatud columnstore tabelite pakkuda nii andmete pakkimine kõrgeima taseme kui ka parima üldise jõudluse päringu.  Rühmitatud columnstore tabelid on üldiselt paremini rühmitatud indeks või hunnik tabelid ja tavaliselt suurte tabelite jaoks parim valik.  Selliseid rühmitatud columnstore on parim koht alustamiseks, kui olete kindel, kuidas indekseerida tabelit.  

Rühmitatud columnstore tabeli loomiseks lihtsalt määrata asendaja-klauslis rühmitatud COLUMNSTORE indeks või asendaja klausel jätta.

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

On paar stsenaariumid, kus rühmitatud columnstore võib olla hea valik.

- Columnstore tabeleid ei toeta teisene-rühmitatud registrid.  Kaaluge võimalust hoopis hunnik või rühmitatud indeks tabelid.
- Columnstore tabeleid ei toeta nvarchar(max), varchar(max) ja varbinary(max).  Kaaluge võimalust hoopis hunnik või rühmitatud indeks.
- Columnstore tabelite võib olla väiksem tõhusa siirdamiseks andmete jaoks.  Kaaluge hunnik ja isegi Ajutised tabelid.
- Vähem kui 100 miljonit reaga väikesed tabelid.  Kaaluge hunnik tabelid.

## <a name="heap-tables"></a>Hunnik tabelid

Kui andmed on ajutiselt kodulehe, klõpsake SQL-i andmebaas, võib juhtuda, et hunnik tabeli kasutamine teeb üldise protsessi kiirem.  Seda sellepärast, et korrastamine laadimise on kiirem kui index tabeleid ja mõnel juhul vahemälu saab teha järgmised lugemis.  Kui laadite andmed ainult see etapp enne käivitamist rohkem teisendused, hunnik tabeli tabeli laadimine on palju kiirem laadimine rühmitatud columnstore tabeli andmeid. Lisaks[ajutine] [ajutise tabeli]andmete laadimine on laadida ka palju kiiremini laadimine tabeli püsivate salvestusruumi.  

Vähem kui 100 miljonit ridu, väike otsing tabelitega sageli hunnik tabelite oleks.  Kobar columnstore tabelite hakata saavutada optimaalse tihendamise, kui seal on rohkem kui 100 miljonit read.

Hunnik tabeli loomiseks määrake lihtsalt HUNNIK asendaja-klauslis.

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Rühmitatud ja nonclustered registrid

Rühmitatud registrid võib rühmitatud columnstore tabelite paremini, kui ühe rea vaja kiiresti tuua.  Kui ühe või väga vähe rea otsing on vajalik jõudluse äärmiselt kiirus päringuid, kaaluge kobar register või nonclustered indeks.  Kahjuks abil rühmitatud indeks on, et ainult päringuid, mis äärmiselt valikuline filtri kasutamine rühmitatud indeksiveerg kasu.  Filtri muude veergude parandamiseks nonclustered indeks saab lisada muud veerud.  Siiski iga indeks, mis lisatakse tabelisse lisab ruumi nii töötlemise ajal laadimise.

Rühmitatud indeks tabeli loomiseks Määrake asendaja-klauslis lihtsalt rühmitatud indeks.

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Tabeli-rühmitatud indeks lisamiseks lihtsalt kasutage järgmist süntaksit:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] -Rühmitatud indeks luuakse vaikimisi, kui kasutatakse registri loomine. Lisaks-rühmitatud indeks lubatud ainult tabeli rea salvestusruumi (HUNNIK või rühmitatud indeks). Praegu pole lubatud mitte rühmitatud registrid peal rühmitatud COLUMNSTORE indeks.


## <a name="optimizing-clustered-columnstore-indexes"></a>Rühmitatud columnstore registrid optimeerimine

Rühmitatud columnstore tabelid on andmed korraldatud segmente.  Lõigu kõrge kvaliteediga on kriitiline columnstore tabeli päringu optimaalse jõudluse saavutamiseks.  Lõigu kvaliteeti saab mõõta tihendatud rea rühma ridade arv.  Lõigu kvaliteet on kõige optimaalse, kus on vähemalt tihendatud rea kohta 100K ridade rühmitamine ja täitmisel nimega kohta rea rühma lähenemine ridade arv 1 048 576 rida, mis on enamiku read rea rühma võib sisaldada.

Selle all vaate saab luua ja kasutada oma süsteemis arvutada rea kohta Keskmine ridade rühmitamine ja tuvastada, mis tahes optimaalne kobar columnstore registrid.  Viimane veerg vaade loob nimega SQL-lauset, mida saab kasutada oma registrite taastada.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Nüüd, kui olete loonud vaadet, käivitage see päring, millel on vähem kui 100K ridade tabeli rea levirühmadega tuvastamiseks.  Muidugi soovite suurendada lävi 100 k, kui otsite Lisateavet optimaalse lõigu kvaliteet. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Kui käivitate päringu alustamist saate andmeid vaadata ja analüüsi tulemused. Selles tabelis kirjeldatakse, mida teie rea jaotis analüüs otsida.


| Veerg                             | Kuidas kasutada neid andmeid                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Kui tabel on liigendatud, siis võib-olla eeldate näha loendab kõrgema avatud rearühm. Iga sektsiooni jaotus oleks avatud rearühm, mis on seotud teoorias. Tegur see oma analüüsi. Väikese tabeli, mis on antud liigendatud võib optimeeritud, eemaldades eraldamine hoopis nagu see parandaks tihendamise.                                                                        |
| [row_count_total]                  | Tabeli rida kogusumma arvu. Näiteks saate selle väärtuseks ridade tihendatud olekus protsendi arvutamiseks.                                                                      |
| [row_count_per_distribution_MAX]   | Kui kõik read on ühtlaste oleks seda väärtust target ridade arvu, mis levitamise kohta. Võrrelge seda funktsiooni compressed_rowgroup_count väärtus.                                 |
| [COMPRESSED_rowgroup_rows]         | Columnstore vormingus tabeli ridade koguarvust.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Kui Keskmine ridade arv on oluliselt väiksem maksimaalne # ridade rea rühma, siis kaaluge CTAS või muuta indeks uuesti pakkimata andmed                     |
| [COMPRESSED_rowgroup_count]        | Arv rea columnstore vormingus. Kui see arv on väga kõrge seotud tabel on indikaator, et columnstore tihedus on madal.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Ridade kustutatakse loogiliselt columnstore vormingus. Kui arv on kõrge, tabeli suurus suhtes, kaaluge taasloomine sektsiooni või registri taastamine, kui see eemaldab need füüsilise. |
| [COMPRESSED_rowgroup_rows_MIN]     | Kasutage seda AVG ja MAX veergude koos mõista väärtustevahemik oma columnstore rea rühmade jaoks. Väike number üle laadi lävi (joondatud partition jaotuse kohta 102,400) näitab, et Optimeerimised on saadaval andmete laadimine                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Nagu eespool                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Tavaline on avatud rea rühmad. Mõistlik võiks oodata ühe avatud ridade rühmitamine tabelis jaotuse (60) kohta. Liigse arvud näitavad andmete laadimine sektsioonid üle. Kontrollige eraldamine strateegia veendumaks, et see on heli                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Iga rearühm saab on kuni 1 048 576 rida ei. Selle väärtuse abil saate vaadata, kuidas täielik avatud rea rühmad on praegu                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Avatud rühmad näitab, et andmed on tabelisse laadimist käputäis või et eelmise laadi peale ülejäänud read sellesse rühma rida. MIN ja MAX kasutamine, AVG veergude kuvamiseks, kui palju andmeid on avatud l rea rühmad. Väike, tabelid võib 100% kõik andmed! Sel juhul muuta indeks uuesti jõustamine andmed, et columnstore.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Nagu eespool                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Nagu eespool                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Vaadake suletud rea ridade rühmitamine meelerahu kontrollimiseks.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Suletud rea rühmade arv peaks olema madal, kui mis tahes näinud üldse. Suletud rea rühmad saab teisendada tihendatud rowg ane abil muuta registri... Ümber korraldada käsk. Kuid see ei ole tavaliselt nõutav. Suletud rühmad teisendatakse columnstore rea rühmad automaatselt tausta "korteeži mover" protsess.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Suletud rea rühmade peaks olema väga kõrge täide määr. Kui suletud rearühm täide määr on madal, siis on columnstore edasise analüüsimise on nõutav.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Nagu eespool                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Nagu eespool                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL-i taastada tabeli columnstore register                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Põhjustab halba columnstore index kvaliteediga

Kui tabelid on tähistatud lõigu kehva kvaliteedi, mida soovite tuvastada põhjus.  Allpool on mõned muud kehva lõigu quaility levinud põhjused:

1. Kui indeks on ehitatud mälu
2. Suure hulga piirmäära toimingud
3. Väike või käputäis laadi toimingud
4. Liiga palju sektsioonid

Columnstore indeks on oluliselt väiksem kui 1 miljonit optimaalse read rea rühma kohta, võivad põhjustada järgmisi tegureid.  Samuti võivad tekkida ridade asemel tihendatud rearühm delta rearühm avamiseks. 

### <a name="memory-pressure-when-index-was-built"></a>Kui indeks on ehitatud mälu

Tihendatud rea rühma kohta ridade arvu, mis on otseselt seotud rida ja mälu töötlemiseks jaotise rea laiuse.  Kui read on kirjutatud mälu columnstore tabelitele, võib on columnstore lõigu kvaliteet.  Seetõttu on parim seansi, mis teie columnstore index tabelite juurdepääsu nii palju mälu võimalikult kirjutamise.  Kuna seal on loobuda vahel mälu ja kokkulangevus, on juhised õige mälu eraldamine sõltub andmed tabelisse, te olete eraldatud teie süsteemi DWU summa ja kokkulangevus teenindusaegade saate anda seansi, mis on teie tabeli andmete kirjutamise summa igal real.  Hea tava, soovitame alates xlargerc, kui kasutate DW300 või vähem, largerc, kui kasutate DW400 DW600 ja mediumrc, kui kasutate DW1000 ja kohal.

### <a name="high-volume-of-dml-operations"></a>Suure hulga piirmäära toimingud

Suure hulga piirmäära toimingute kohta, mida värskendada ja kustutada read saate lisada soovitud columnstore ebaefektiivsust. See kehtib eriti kui enamik rea jaotises read on muudetud.

- Rea kustutamine tihendatud rearühm ainult loogiliselt märgib rida, kustutada. Rea jääb tihendatud rearühm kuni sektsiooni või tabel luuakse.
- Rea lisamistoimingu lisab rea sisemise rowstore tabelit delta rea rühma. Lisatud rida on ümber columnstore kuni delta rearühm täis ja on märgitud suletuks. Rea rühmad on suletud, kui need jõuavad maksimaalne maht 1 048 576 rida. 
- Rea columnstore vormingus värskendamine töödeldakse loogilise Kustuta ja seejärel lisada. Lisatud rea salvestatakse delta pood.

Batched värskenduse ja lisage toimingud, mis ületavad hulgi lävi 102,400 ridade sektsiooni joondatud jaotuse kirjutatakse columnstore vormingusse. Siiski eeldades, et on paaris jaotuse, peate olema muutmine üle 6.144 miljoni ridade ühekordne selleks. Kui antud sektsiooni ridade arvu joondatud jaotus on väiksem kui 102,400 siis read lähevad delta pood ja kuni piisavalt read on lisatud või muudetud rea rühma sulgemiseks või indeks on taastatud.

### <a name="small-or-trickle-load-operations"></a>Väike või käputäis laadi toimingud

Väike laadib, et meilivoo teenusekomplekti SQL-i andmebaas ka mõnikord nimetatakse nagu käputäis laadimise. Tavaliselt moodustavad need läheduses püsival andmevoo on märkimisväärselt süsteem. Siiski, nagu see voog on pidev lähedal ridade maht ei ole eriti suur. Enamasti andmed on märkimisväärselt jaotises piirmäära nõutav otsene koormus columnstore vormingusse.

Sel juhul on sageli parem maa andmed esmalt Azure'i bloobimälu ja laske koguda enne laadimist. See meetod on sageli tuntud *mikro partiide*.

### <a name="too-many-partitions"></a>Liiga palju sektsioonid

Teine asi, mida arvesse võtta on selle mõju teie rühmitatud columnstore tabelites eraldamine.  Enne eraldamine, SQL-i andmebaas jagab juba andmete 60 andmebaasid.  Andmete täpsemaks eraldamine jagab.  Kui te partition oma andmeid, siis te soovite võtke arvesse, et **iga** sektsiooni peab olema vähemalt 1 miljon ridade kasu klaster columnstore register.  Kui te partition tabeli 100 sektsioonid sisse, siis tabeli peate olema vähemalt 6 miljardit ridade kasu rühmitatud columnstore index (60 jaotuse *100 sektsioonid* 1 miljonit read). Kui teie 100 sektsiooni tabel pole 6 miljardit read, sektsioonid arvu vähendamiseks või kaaluge tabelisse hunnik asemel.

Kui tabelitel on laaditud mõned andmetega, siis tehke selle all samme, et taastada optimaalne kobar columnstore registrite tabelid.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Registrite lõigu kvaliteedi parandamiseks taastamine

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Samm 1: Tuvastamine või kasutaja, mis kasutab õige ressursi klassi

Ühe lõigu kvaliteeti parandada kohe kiiresti on registri taastada.  SQL-i tagastatud ülaltoodud vaates tulemuseks muuta indeks uuesti lause, mida saab kasutada taastada oma registrid.  Kui teie registrid taastamine, kindlasti, et eraldate mälu seansiga, mis loob teie index.  Selle tegemiseks suurendada ressursside klassi kasutaja, mis on vajalikud õigused taastada selle tabeli soovitatav minimaalne registri.  Selle ressursi ainekursuse andmebaasi omanik kasutaja ei saa muuta, nii et kui te pole loonud kasutaja süsteemi, peate esmalt seda teha.  Soovitame vähemalt on xlargerc kui kasutate DW300 või vähem, largerc, kui kasutate DW400 DW600 ja mediumrc, kui kasutate DW1000 ja üle.

Allpool on näide sellest, kuidas määrata rohkem mälu kasutajale nende ressursside klassi suurendades.  Lisateavet ressursi tunnid ja kuidas luua uus kasutaja leiate [kokkulangevus ja töökoormus] [ Concurrency] artikkel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Samm 2: Taastada rühmitatud columnstore indeksid suurema ressursi klassi kasutajaga
Logige sisse kasutajana juhises 1 (nt LoadUser), mis on nüüd kõrgema klassi ressursi kasutamise ja muuta registri laused käivitada.  Pidage meeles, et see kasutaja on ALTER õigus tabelid, kus indeks luuakse uuesti.  Nendes näidetes, kuidas taastada kogu columnstore register või kuidas taastada ühe sektsiooni. Suurte tabelite, oleks rohkem praktiline taastada indeksite ühe sektsiooni korraga.

Võimalusena asemel taastamine registri, võib tabeli kopeerida uude tabelisse [CTAS][]abil.  Milline viis on parim? Suuri andmemahtusid [CTAS][] on tavaliselt kiiremini [Muuta indeks][]. Väiksem andmemahtusid [Muuta indeks][] on hõlpsam kasutada ja ei vaja, saate vahetada tabeli välja.  Vt lisateavet selle kohta, kuidas taastada indeksid koos CTAS **taastamine registrid CTAS ja partition üleminek** all.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Ühenduseta toimingu taastamine registri SQL-i andmebaas on.  Registrite taastamise kohta leiate lisateavet jaotisest muuta indeks uuesti [Columnstore registrite defragmentimist][] ja süntaks teema [Muuta indeks][].
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Samm 3: Veenduge, et rühmitatud columnstore lõigu kvaliteet on paranenud
Käivita päring tuvastatud tabelis kehva segmendi kvaliteedi ja lõigu kvaliteeti kontrollida uuesti parandanud.  Kui lõigu kvaliteet ei parandanud, on võimalik, et tabeli read on eriti lai.  Kaaluge ressursi kõrgema klassi või DWU kui taastamine oma registrid.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Registrite CTAS ja partition üleminek taastamine

Selles näites kasutatakse [CTAS][] ja partition üleminek tabel sektsioon taastada. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Uuesti luua sektsioonid kasutamise kohta lisateabe saamiseks `CTAS`, leiate artiklist [Partition][] .

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks lugege artikleid [Tabeli ülevaade][Ülevaade], [Tabeli andmetüübid][Andmetüübid], [levitamise tabeli][hajuta], [eraldamine tabeli][Partition], [Säilitada tabeli statistika][statistika] ja [Ajutised tabelid][ajutine].  Heade tavade kohta leiate lisateavet artiklist [Andmete ladu SQL-i põhitõed][].

<!--Image references-->

<!--Article references-->
[Ülevaade]: ./sql-data-warehouse-tables-overview.md
[Andmetüübid]: ./sql-data-warehouse-tables-data-types.md
[Levitamine]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Ajutiste]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL-i andmete ladu head tavad]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[LAUSED ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[hunnik]: https://msdn.microsoft.com/library/hh213609.aspx
[rühmitatud registrid ja nonclustered registrid]: https://msdn.microsoft.com/library/ms190457.aspx
[Looge tabel süntaks]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore registrite defragmentimist]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[rühmitatud columnstore registrid]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
