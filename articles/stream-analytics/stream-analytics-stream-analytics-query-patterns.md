<properties
    pageTitle="Päringu levinud mustreid voo Analytics näited | Microsoft Azure'i"
    description="Levinud Azure'i voo Analytics päringu mustrite "
    keywords="päringu näited"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Päringu levinud voo Analytics mustreid näited

## <a name="introduction"></a>Sissejuhatus

Päringute Kasutusanalüüsi Azure'i voo on väljendatud SQL-like päringukeele, mis on Lühijuhend [voo Analytics päringu keel](https://msdn.microsoft.com/library/azure/dn834998.aspx) .  See artikkel kirjeldab põhjal reaal stsenaariumid mitme levinud päring mustrite lahendusi.  See on tööd ja kasutab ka edaspidi mudeleid pidevalt värskendada.

## <a name="query-example-data-type-conversions"></a>Päringu näide: andmete andmetüüpide teisendamise
**Kirjeldus**: määratleda atribuutide liiki Sisestuskeel voogu.
Näiteks auto paksus peagi Sisestuskeel voogu nagu stringide ja tuleb teha INT ümber KOKKUVÕTTEKS.

**Sisestusmeetodi**:

| Tegemine | Aeg | Weight (kaal) |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Väljundi**:

| Tegemine | Weight (kaal) |
| --- | --- |
| Honda | 3000 |

**Lahendus**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Selgitus**: CAST lause kasutamine paksus välja määramiseks selle tüüpi (loendi kuvamiseks toetatud andmetüübid [siin](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Päringu näide: abil meeldib/ei meeldi muster sobitamine
**Kirjeldus**: Kontrollige, kas välja väärtus sündmus vastab mustri näiteks saatja litsentsi plaatide, mis algavad a ja lõppevad 9

**Sisestusmeetodi**:

| Tegemine | LicensePlate | Aeg |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Väljundi**:

| Tegemine | LicensePlate | Aeg |
| --- | --- | --- |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Lahendus**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Selgitus**: Veenduge, et LicensePlate välja väärtus algab a, siis on null või mis tahes märgistring, ja lõpeb 9 LIKE lause abil. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Päringu näide: määrake loogika eri juhtudel väärtuste (juhul lauseid)
**Kirjeldus**: pakkuda muu arvutuse teatud kriteeriumide alusel välja.
Näiteks kirjelduse stringi mitu auto möödunud sama teha, kus on 1.

**Sisestusmeetodi**:

| Tegemine | Aeg |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Väljundi**:

| CarsPassed | Aeg |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotad | 2015-01-01T00:00:10.0000000Z |

**Lahendus**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Selgitus**: The juhul klausel võimaldab meil pakkuda muu arvutuse (käesoleval juhul auto liitväärtuse aknas arv) teatud kriteeriumide alusel.

## <a name="query-example-send-data-to-multiple-outputs"></a>Päringu näide: mitme väljundeid andmete saatmine
**Kirjeldus**: mitme väljundi sihtkohtade ühe töö andmeid saata.
Näiteks lävi vastavalt teatise andmeid analüüsida ja arhiivida kõik sündmused bloobimälu

**Sisestusmeetodi**:

| Tegemine | Aeg |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Tegemine | Aeg |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Tegemine | Aeg | Count |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Lahendus**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Selgitus**: The sisse klausel ütleb voo Analytics andmete kirjutamiseks selle lause väljundeid mis.
Esimene päring on läbiv me ei saanud, et saaksime nimega ArchiveOutput väljund andmed.
Teise päringu ei mõne lihtsa liitmise ja filtreerimise ja saadab tulemused järgmise etapi infoturbeküsimustes süsteem.
*Märkus*: mitme väljundi aruannetes saate ka taaskasutada tulemuste CTEs (st koos lauseid) – see on lisatud kasu avamine vähem lugejatel sisendandmete allikas.
Näiteks 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Päringu näide: Üheste väärtuste loendamine
**Kirjeldus**: voo kindla aja jooksul kuvatavate väljade Kordumatute väärtuste arvu loendamiseks.
Näiteks mitu kordumatu tegemine auto läbi teemaks boksi 2 teine aken?

**Sisestusmeetodi**:

| Tegemine | Aeg |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Väljund:**

| Count | Aeg |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Lahendus:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Selgituse:** Me algse liitmise kordumatu muudab oma arv üle akna saada.
Seejärel teeme liitmise mitu muudab meil sai – antud kõik Kordumatud väärtused aknas saada sama ajatempli, siis teine koondamine akna peab olema minimaalne, mitte liita 2 windows kõigepealt kaudu.

## <a name="query-example-determine-if-a-value-has-changed"></a>Päringu näide: kindlaks teha, kui väärtus on muutunud#
**Kirjeldus**: eelmise väärtus kindlaks teha, kui see on erinev, kui praegune väärtus on näiteks eelmise auto teemaks teel sama teha, kui praegune auto vaadata?

**Sisestusmeetodi**:

| Tegemine | Aeg |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Väljundi**:

| Tegemine | Aeg |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Lahendus**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Selgitus**: kasutamise LAG sisend üheks korraks voona ühe sündmuse tagasi ja tee väärtuse. Seejärel võrrelge seda teha praeguse sündmuse kohta ja väljund sündmus, kui need on erinevad.

## <a name="query-example-find-first-event-in-a-window"></a>Päringu näide: otsing esimese sündmuse aknas
**Kirjeldus**: Otsi esimene auto iga 10 minuti järel?

**Sisestusmeetodi**:

| LicensePlate | Tegemine | Aeg |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Väljundi**:

| LicensePlate | Tegemine | Aeg |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Lahendus**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Nüüd vaatame muutmine probleemi ja Otsi esimene auto eriti teha iga 10 minuti järel.

| LicensePlate | Tegemine | Aeg |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Lahendus**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Päringu näide: otsing viimase sündmuse aknas
**Kirjeldus**: Find viimase auto iga 10 minuti järel.

**Sisestusmeetodi**:

| LicensePlate | Tegemine | Aeg |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Väljundi**:

| LicensePlate | Tegemine | Aeg |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Lahendus**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Selgitus**: päringusse on kaks toimingut – esimene leiab uusim ajatempli opsüsteemis windows 10 minuti. Teise etapi liitub esimest Päringutulemid koos algse voo leidmiseks sündmuste sobitamine viimase ajatemplid iga aknas. 

## <a name="query-example-detect-the-absence-of-events"></a>Päringu näide: tuvastada sündmuste
**Kirjeldus**: märkige ruut, et voo ei ole väärtust, mis vastab teatud kriteeriumidele.
Näiteks 2 järjestikust auto kaudu sama tehke sisestanud teemaks tee 90 sekundi pärast?

**Sisestusmeetodi**:

| Tegemine | LicensePlate | Aeg |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | DEF-987 | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI-345 | 2015-01-01T00:00:04.0000000Z |

**Väljundi**:

| Tegemine | Aeg | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA-999 | ABC-123 | 2015-01-01T00:00:01.0000000Z |

**Lahendus**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Selgitus**: kasutamise LAG sisend üheks korraks voona ühe sündmuse tagasi ja tee väärtuse. Seejärel võrrelge seda teha praeguse sündmuse kohta ja väljund sündmus, kui need on samad ja LAG abil saate andmeid eelmise auto.

## <a name="query-example-detect-duration-between-events"></a>Päringu näide: Tuvasta sündmuste kestus
**Kirjeldus**: leida antud sündmuse kestus. Antud mõne web Click Stream määratleda ajakulu funktsiooni.

**Sisestusmeetodi**:  
  
| Kasutaja | Funktsioon | Sündmuse | Aeg |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Alustamine | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Lõpeta | 2015-01-01T00:00:08.0000000Z |
  
**Väljundi**:  
  
| Kasutaja | Funktsioon | Kestus |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Lahendus**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Selgitus**: kasutamine viimase funktsioon laadida viimase kellaaja väärtuse, kui sündmuse tüüp on "Start". Pange tähele, et viimase funktsioon kasutab PARTITION [kasutaja], näitamaks, et tulemus arvutatakse kordumatu kasutaja kohta.  Päring on 1 tund kuni lävi aja erinevus "Start" ja "Stop" sündmuste jaoks, kuid seda konfigureerida as vajadusele (piiratud kestus (tund, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Päringu näide: Tuvasta kestuse tingimus.
**Kirjeldus**: teada, kui kaua tingimus ilmnenud jaoks.
Oletame näiteks, et viga, mille tulemuseks on vale on vähemalt kõik auto (üle 20 000 naela) – soovime arvutada viga kestus.

**Sisestusmeetodi**:

| Tegemine | Aeg | Weight (kaal) |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Väljundi**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Lahendus**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Selgitus**: kasutamine LAG Sisestuskeel voo kuvamine 24 tundi ja otsida eksemplari, kus ulatuvad StartFault ja StopFault, kaal < 20000.

## <a name="query-example-fill-missing-values"></a>Päringu näide: täitke puuduvate väärtuste
**Kirjeldus**: sündmusi, mis on puuduvate väärtuste voo, jaoks aedvili voo sündmuste regulaarse intervalliga.
Näiteks saate luua sündmuse iga 5 sekundi järel, mis esitab kõige viimati vaadelda andmepunkti.

**Sisestusmeetodi**:

| t | väärtus |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

**Väljasta (esimese 10 read)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Lahendus**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Selgitus**: selle päringu luua sündmuste iga 5 teine ja väljund võttis enne viimast sündmusega. [Hopping aken] (https://msdn.microsoft.com/library/dn835041.aspx "Hopping aken - Azure'i voo Analytics") kestus määrab päringu näeb uusima sündmus (selles näites 300 sekundi) leidmiseks kui kaugele tagasi.


## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
