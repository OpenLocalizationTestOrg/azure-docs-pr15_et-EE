<properties
   pageTitle="SQL-i andmebaas kokkulangevus ja töökoormus haldus | Microsoft Azure'i"
   description="Lahenduste arendamise mõista kokkulangevus ja töökoormus juhtimise SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>SQL-i andmebaas kokkulangevus ja töökoormus haldus

Alal, Microsoft Azure'i SQL-i andmebaas aitab prognoositavad jõudluse esitamisel saate reguleerida kokkulangevus tasemed ja ressursside jaotus nagu mälu ja CPU tähtsusastme. Selles artiklis tutvustatakse saate kokkulangevus ja töökoormus haldamiseks, mis selgitab, kuidas mõlema funktsiooni on rakendatud ja kuidas saate määrata need andmed laos mõisteid. SQL-i andmebaas töökoormus haldamise on mõeldud aitama tugiteenuste mitme kasutaja keskkonnas. See ei ole mõeldud mitme rentniku töökoormus.

## <a name="concurrency-limits"></a>Kokkulangevus piirangud

SQL-i andmebaas võimaldab kuni 1024 samaaegseid ühendusi. 1024 kõik ühendused saate esitada päringuid üheaegselt. SQL-i andmebaas võib optimeerida läbilaskevõime, järjekord, et mõned päringud tagamaks, et iga päringu saab minimaalsete mälu andmine. Queuing ilmneb päringu täitmise ajal. Andmebaasitõrge päringud kui kokkulangevus limiidid on täis, SQL-i andmebaas suurendamiseks saate kokku läbilaskevõime tagamine aktiivse päringuid pääseda äärmiselt vajalik mälu ressursse.  

Kokkulangevus piirangud kehtivad mõistetest: *samaaegseid päringuid* ja *kokkulangevus slots*. Päringu käivitada, peate käivitada nii päringu kokkulangevus limiit ja kokkulangevus pesa eraldatud.

- Samaaegseid päringud on päringute käivitamisel samal ajal. SQL-i andmebaas toetab kuni 32 samaaegseid päringute suuremad DWU suurused.
- Kokkulangevus teenindusaeg DWU põhjal. Iga 100 DWU pakub 4 kokkulangevus teenindusajad. Näiteks on DW100 eraldab 4 kokkulangevus teenindusaegade ja DW1000 eraldab 40. Iga päringu tarbib üks või mitu kokkulangevus pesa, sõltub selle [Ressursi klassi](#resource-classes) päringu. Päringud, mis töötab smallrc ressursi tunni peab olema üks kokkulangevus pesa. Päringud, mis töötavad kõrgema klassi ressursside tarbimine rohkem kokkulangevus teenindusaegu.

Järgmises tabelis kirjeldatakse nii samaaegseid päringuid ja kokkulangevus teenindusaegade erineva DWU suurusega limiidid.

### <a name="concurrency-limits"></a>Kokkulangevus piirangud

|  DWU   | Max samaaegseid päringud  | Jaotatud kokkulangevus teenindusaegade |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Kui on täidetud üks piirmäärasid uutele päringutele ootele ja täide soovitud esimesena sisse, brigaadisuuruste.  Nagu on päringute lõppemist ja päringute ja teenindusaegade arv langeb alla, järjekorras olevad päringud on välja antud. 

> [AZURE.NOTE]  *Valige* päringu käivitamisel dünaamiline haldusvaated () või kataloogi vaadete ei kehti kõiki kokkulangevus. Saate jälgida süsteemi sõltumata käivitamisel seda päringute arv.

## <a name="resource-classes"></a>Ressursi tunnid

Ressursi klassid juhite mälu jaotamine ja antud päringu protsessori spikker. Saate määrata ressursside nelja liiki kasutajale kujul *andmebaasi rollid*. Nelja ressursi klassid on **smallrc**, **mediumrc**, **largerc**ja **xlargerc**. Kasutajate smallrc on toodud väiksema summa mälu ja saate ära kõrgema kokkulangevus. Seevastu xlargerc määratud kasutajad on esitatud suure hulga, ja seetõttu vähem oma päringu käitamise samaaegselt.

Vaikimisi on iga kasutaja väike ressursi ainekursust, smallrc liige. Protseduur `sp_addrolemember` suurendamiseks ressursi ainekursust, kasutada ja `sp_droprolemember` kasutatakse vähendamiseks ressursside klassi. Näiteks suurendab see käsk ressursside klassi loaduser largerc abil:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Hea tava on jäädavalt määrata kasutajatele ressursi tunni asemel muuta nende ressursside tunnid. Näiteks laadimise rühmitatud columnstore tabelitele luua kõrgema kvaliteediga registrite kui eraldatud rohkem mälu. Laadimise juurdepääs suurem mälumahuga tagamiseks kasutaja loomine spetsiaalselt andmete laadimine ja jäädavalt määrata selle kasutaja kõrgema klassi ressursi.

On paar tüüpi päringud, mis ei saa kasu suurem mälumahuga eraldatud. Süsteemi ei Ignoreeri nende ressursside klassi jaotus ja alati käivitada need päringud small ressursi tunni asemel. Kui need päringud käivitada alati small ressursi tunni, ta saab käivitada, kui kokkulangevus teenindusajad on rõhu all ja nad ei tarbimine rohkem teenindusaegu kui vaja. Lisateabe saamiseks vaadake [ressursside klassi erandid](#query-exceptions-to-concurrency-limits) .

Ressursside klassi veel mõned andmed:

- *Muuda rolli* õigusi on vaja muuta ressursside klassi kasutaja.  
- Kuigi kasutaja saate lisada ühe või mitme suurema ressursi tunnid, võtab kasutajad on määratud ressursi kõrgeima klassi atribuute. See tähendab, kui kasutaja on määratud mediumrc nii largerc, ressursside kõrgema klassi (largerc) on ressurss ainekursus kehtima.  
- Ressursside klassi süsteemi administraatoriõigustega kasutaja ei saa muuta.

Üksikasjalikud näiteks vt [muutmine kasutaja ressursside klassi näide](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Mälu jaotamine

On spetsialistid ja miinuste kasutaja ressursside klassi suurendada. Suurendab ressursside klassi kasutaja annab rohkem mälu, mis võivad päringud käivitada kiiremini oma Accessi päringute.  Ressursi kõrgema klassi vähendada ka samaaegne suhtes, mida saab käitada arv. See on vahelist jaotamise suure hulga ühe päringule ja muud päringud, mis on vaja ka mälu jaotuse käivitada üheaegselt, mis võimaldab. Kui üks kasutaja mälu päringu jaotus, kõrge, teiste kasutajate neil pole juurdepääsu päringut, et sama mälu.

Järgmises tabelis kaardid DWU ja ressursside klassi iga jaotuse eraldatud mälu.

### <a name="memory-allocations-per-distribution-mb"></a>Mälu eraldamine jaotuse (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1 600   |
| DW500  |   100   |    400   |    800  |  1 600   |
| DW600  |   100   |    400   |    800  |  1 600   |
| DW1000 |   100   |    800   |  1 600  |  3200   |
| DW1200 |   100   |    800   |  1 600  |  3200   |
| DW1500 |   100   |    800   |  1 600  |  3200   |
| DW2000 |   100   |  1 600   |  3200  |  6 400   |
| DW3000 |   100   |  1 600   |  3200  |  6 400   |
| DW6000 |   100   |  3200   |  6 400  |  12 800  |

Eelnev tabelist näete, et päring töötab soovitud DW2000 xlargerc ressursi tunni oleks juurdepääs 6400 MB mälu iga 60 jaotatud andmebaasid.  SQL-i andmebaas, on 60 jaotuse. Seetõttu on kogu mälu eraldatud päringu antud ressursi tunni arvutamiseks ülaltoodud väärtused tuleks korrutada 60.

### <a name="memory-allocations-system-wide-gb"></a>Mälu jaotus kogu süsteemi (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

Kogu süsteemi mälu jaotuse tabelist näete, et töötavate on DW2000 xlargerc ressursi tunni päringu eraldatakse kokku 375 GB mälu (6400 MB * 60 jaotuse / 1024 teisendamiseks GB) kogu teie SQL-andmebaas.

## <a name="concurrency-slot-consumption"></a>Kokkulangevus pesa tarbimine

SQL-i andmebaas annab rohkem mälu töötab ressursi kõrgema klassi päringud. Mälu on fikseeritud ressursi.  Seetõttu eraldatud päringu kohta rohkem mälu, vähem samaaegseid päringute käivitamiseks. Järgmises tabelis kordab kõik eelmise mõisted ühe vaade, milles kuvatakse kokkulangevus teenindusaegadest, DWU ja pesa, mis on iga ressursside klassi tarbitud arv.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Eraldatud ja tarbimine kokkulangevus teenindusaegade

|  DWU   | Suurim lubatud samaaegseid päringud  | Jaotatud kokkulangevus teenindusaegade | Kasutatavad smallrc Slots |  Kasutatavad mediumrc Slots |  Kasutatavad largerc Slots |  Kasutatavad xlargerc Slots |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Sellest tabelist näete seda SQL-i andmebaas töötab DW1000 eraldab kuni 32 samaaegseid päringud ja 40 kokkulangevus teenindusaegade kokku. Kui kõik kasutajad töötavad smallrc, 32 samaaegseid päringute lubatud Kuna iga päringu peaks olema 1 kokkulangevus pesa. Kui kõik kasutajad on DW1000 olid töötab mediumrc, siis iga päringu eraldatakse 800 MB päring 47 GB mälu kokku eraldatud jagamise kohta ja kokkulangevus oleks piiratud 5 kasutajatele (40 kokkulangevus teenindusaegade / 8 teenindusaegade mediumrc kasutaja kohta).

## <a name="query-importance"></a>Päringu tähtsus

SQL-i andmebaas rakendab ressursside tunnid, kasutades töökoormus rühmad. On kokku kaheksa töökoormus rühmad, mis juhib üle erineva DWU suurusega ressursi tunnid toimimist. Mis tahes DWU SQL-i andmebaas kasutab ainult neli kaheksa töökoormus rühmad. See on mõistlik, kuna iga rühma töökoormus on määratud üks neljast ressursi klassid: smallrc, mediumrc, largerc, või xlargerc. Töökoormus rühmade mõistmine on oluline on see, et mõned nende töökoormus rühmad on seatud suurem *tähtsus*. Tähtsus kasutatakse CPU plaanimine. Käivitage kõrge tähtsusega päringud saavad kolm korda mitme protsessori kui keskmine tähtsaks. Seetõttu määratleda kokkulangevus pesa vastendused CPU prioriteet. Kui päringu tarbib 16 või rohkem pesa, see töötab tähtsaks.

Järgmine tabel näitab iga töökoormus rühma vastendused tähtsus.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Töökoormus jaotises vastendused kokkulangevus teenindusaegade ja tähtsus

| Töökoormus rühmad | Kokkulangevus pesa kaardistamine | MB / jaotuse. | Tähtsus kaardistamine |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Keskmine       |
| SloDWGroupC01   |            2             |         200       |       Keskmine       |
| SloDWGroupC02   |            4             |         400       |       Keskmine       |
| SloDWGroupC03   |            8             |         800       |       Keskmine       |
| SloDWGroupC04   |           16             |       1 600       |       Kõrge         |
| SloDWGroupC05   |           32             |       3200       |       Kõrge         |
| SloDWGroupC06   |           64             |       6 400       |       Kõrge         |
| SloDWGroupC07   |          128             |      12 800       |       Kõrge         |

**Eraldatud ja tarbimine kokkulangevus teenindusaegade** diagrammi, näete, et mõne DW500 kasutab 1, 4, 8 või 16 kokkulangevus slots smallrc, mediumrc, largerc ja xlargerc, vastavalt. Saate vaadata nende väärtuste leidmiseks tähtsus iga ressursside klassi eelmisel diagrammil.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Ressursi tunnid tähtsuse DW500 kaardistamine

| Ressursside klassi | Töökoormus rühma | Kokkulangevus teenindusaegu, mida kasutatakse | MB / jaotuse. | Tähtsus |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Keskmine   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Keskmine   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Keskmine   |
| xlargerc       | SloDWGroupC04  |          16            |        1 600      |   Kõrge     |


Mälu ressursi eraldus üksikasjalikud erinevused ressursihaldur seisukohast vaadata või analüüsimiseks aktiivne ja ajaloolise kasutamist töökoormus rühmade tõrkeotsinguks saate kasutada DMV järgmine päring.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Päringud, mis au kokkulangevus piirangud

Enamik päringute reguleerib ressursi tunnid. Need päringud peab sobivad sees nii samaaegseid päringu ja kokkulangevus pesa piirmäärasid. Kasutaja ei soovi välistamine päringu pesa konkurentsusmudeli.

Järgmistest au korrata, ressursside tunnid:

- LISA-VALIMINE
- VÄRSKENDAMINE
- KUSTUTAMINE
- Valige (kui päringu kasutaja tabelid)
- LAUSED ALTER REGISTER TAASTADA
- LAUSED ALTER INDEX ÜMBER KORRALDADA.
- LAUSED ALTER TABELI TAASTADA
- REGISTRI LOOMINE
- RÜHMITATUD COLUMNSTORE REGISTRI LOOMINE
- LOOGE TABEL AS VALIMINE (CTAS)
- Andmete laadimine
- Andmete liikumine toimuvate järgi soovitud andmete liikumine teenuse Sharepointil

## <a name="query-exceptions-to-concurrency-limits"></a>Päringu erandite kokkulangevus piirangud

Mõned päringud au ressursi ainekursuse, mis on määratud kasutajale. Need piirangud kokkulangevus erandid tehakse, kui mälu ressursse, mis on vajalik mõne kindla käsu sageli madal, kuna käsk on metaandmete toiming. Erandite eesmärk on suurem mälumahuga jaotuse päringuid, mis ei pea neid kunagi vältida. Sellisel juhul kasutatakse alati sõltumata kasutaja määratud tegeliku ressursi liiki vaikimisi small ressursside klassi (smallrc). Näiteks `CREATE LOGIN` kestab alati smallrc. Selle toimingu täitmiseks nõutavad ressursid on liiga nõrk, nii, et see ei tee mõttes päringu kaasatava pesa konkurentsusmudeli.  Need päringud on ka mitte ainult 32 kasutaja kokkulangevus limiit, piiramatu arvu need päringud saate käivitada kuni 1024 seansid, seansi piiri.

Järgmistest au ressursi tunnid:

- Loo või LANGEV tabel
- MUUDA TABELI... LÜLITI, tükeldatud või SEKTSIOONI ühendamine
- LAUSED ALTER INDEX KEELAMINE
- RIPPLOENDI INDEX
- Loo, värskendamine või LANGEV statistika
- TABELI KÄRPIMINE
- LUBA MUUTA
- LOGIN LOOMINE
- LUUA, muuta või LANGEV kasutaja
- Loo, ALTER või LANGEV toiming
- Loo või LANGEV kuvamine
- VÄÄRTUSTE LISAMINE
- Valige süsteemi vaated ja DMVs
- SELGITAGE
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Kasutaja ressursside klassi näiteks muutmine

1. **Loo login:** Ühenduse loomine SQL serveri andmebaasi SQL-i andmebaas majutusteenuse andmebaasi **juhtslaidi** ja käivitada järgmised käsud.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] See on hea mõte põhi andmebaasi SQL Azure'i andmebaas kasutajate jaoks kasutajakonto loomiseks. Juhtslaidi loomine kasutaja saab kasutaja sisselogimine tööriistadega nagu SSMS ilma andmebaasi nimi.  See lubab ka need objekti Exploreri abil saate vaadata kõiki andmebaaside SQL serveris.  Loomise ja kasutajate haldamise kohta leiate lisateavet teemast [Secure andmebaasi SQL-i andmebaas][].

2. **Loomine SQL-i andmebaas kasutaja:** Avage **Andmebaas SQL** -andmebaasiga ühendust ja käivitage järgmine käsk.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Õiguste andmine:** Järgmine näide annab `CONTROL` **SQL-i andmebaas** andmebaasi. `CONTROL`andmebaas on samaväärne db_owner SQL serveris.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Suurendada ressursside klassi:** Suurem töökoormus rollirühma kasutaja lisamiseks kasutada järgmist päringut.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Vähendamiseks ressursside klassi:** Kasutaja eemaldamiseks töökoormus rollirühma kasutada järgmist päringut.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Ei saa smallrc Kasutaja eemaldamine.

## <a name="queued-query-detection-and-other-dmvs"></a>Ootel päringu tuvastamise ja muude DMVs

Saate kasutada funktsiooni `sys.dm_pdw_exec_requests` DMV selgitada päringud, mis ootab kokkulangevus järjekorda. Päringute ootamine kokkulangevus pesa on olek on **peatatud**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Töökoormus haldusrollide saab vaadata `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Järgmine päring näitab, milliseid igale kasutajale on määratud roll.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL-i andmebaas sisaldab järgmist tüüpi ootamine.

- **LocalQueriesConcurrencyResourceType**: päringud, mis istuda väljaspool kokkulangevus pesa raames. Funktsioonid nagu DMV päringuid ja süsteemi `SELECT @@VERSION` on näited kohalikke päringuid.
- **UserConcurrencyResourceType**: päringud, mis istuda sees kokkulangevus pesa raames. Päringute vastu lõppkasutaja tabelite tähistavad näited, kus soovite kasutatakse selle ressursi tüüp.
- **DmsConcurrencyResourceType**: ootab tulenevate andmete liikumine toimingute.
- **BackupConcurrencyResourceType**: see ootamine näitab andmebaasi varundamist. Selle ressursi tüüp väärtus kuni 1. Kui mitu varukoopiaid taotletakse samal ajal, saavad teised järjekorda.

Funktsiooni `sys.dm_pdw_waits` DMV saab taotluse ootab ressursside kuvamiseks.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

Funktsiooni `sys.dm_pdw_resource_waits` DMV kuvatakse ainult ressursside ootab tarbitud päringu. Ressursi ootama aega meetmed ainult aeg ootamine ressursse, mis on esitatud erinevalt signaal ootamine aega, mis on aluseks oleva SQL-i serverite plaanida päringu peale CPU kuluv aeg.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

Funktsiooni `sys.dm_pdw_wait_stats` DMV saab kasutada ootab ajaloolise trendi analüüsimiseks.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Järgmised sammud

Andmebaasi kasutajate ja turvalisuse haldamise kohta leiate lisateavet teemast [Secure andmebaasi SQL-i andmebaas][]. Kuidas suuremat ressursside kohta Lisateavet saate tunnid rühmitatud columnstore index kvaliteeti parandada leiate teemast [taastamine registrid lõigu kvaliteedi parandamiseks].

<!--Image references-->

<!--Article references-->
[Turvaline andmebaasi SQL-i andmebaas]: ./sql-data-warehouse-overview-manage-security.md
[Registrite lõigu kvaliteedi parandamiseks taastamine]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Turvaline andmebaasi SQL-i andmebaas]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
