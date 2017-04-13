<properties
    pageTitle="Mastaapimiseks voo Analytics töö läbilaskevõime suurendamiseks | Microsoft Azure'i"
    description="Saate teada, kuidas mastaapimiseks voo Analytics töö konfigureerimise Sisestuskeel sektsioonid, päringu definitsiooni häälestamine ja seadmine töö streaming üksused."
    keywords="andmete voogesitus, streaming andmetöötlus, analytics häälestamine"
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
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Skaala Azure'i voo Analytics töö voo töötlemise läbilaskevõime suurendamiseks

Saate teada, kuidas häälestada analytics töö ja arvutamine *streaming üksuste* voo Analyticsi, kuidas mastaapimiseks voo Analytics töö konfigureerimise Sisestuskeel sektsioonid, analytics päringu definitsiooni häälestamine ja seades töö streaming üksused. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Mis on osa voo Analytics töö?
Voo Analytics töö määratlus sisaldab sisendeid, päringu ja väljund. Sisendeid on pärit kus töö loeb andmevoos, kasutatakse päringu sisendi andmevoos ja väljund on, kus töö saadab töö tulemid.  

Töö nõuab vähemalt üks sisendväärtuste andmeallika data streaming. Voo Sisestuskeel andmeallika saab salvestada Azure'i teenus siini sündmuse jaoturi või Azure'i bloobimälu. Lisateabe saamiseks vt [Sissejuhatus Azure'i voo Analytics](stream-analytics-introduction.md) ja [Azure voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Konfigureerimine voogesituse ühikud
Streaming üksused (SUs) tähistavad materjale ja nõutav on Azure voo Analytics töö arvuti võimsus. SUs võimalda töötlemise võimsus segatud mõõt CPU, mälu, põhineb suhteline sündmuse kirjeldamiseks lugemine ja kirjutamine määra. Iga streaming üksuse vastab umbes 1MB sekundis läbilaskevõime. 

Kui palju SUs valimise vajalike eelkõige töö sõltub partition konfiguratsiooni sisendeid ja päringu määratletud töö. Saate valida kuni oma kvoodi streaming üksuste töö Azure klassikaline portaali kaudu. Iga Azure'i tellimuse vaikimisi on konkreetse piirkonna piirmäära 50 streaming mõõtühikud analytics tööde haldamine ülespoole. Streaming mõõtühikud tellimuste suurendamiseks pöörduge [Microsofti klienditoe poole](http://support.microsoft.com).

Töö saab kasutada streaming ühikute arv sõltub partition sisendeid ja töö määratletud päring. Pange tähele, et voo üksuste jaoks sobiv väärtus tuleb kasutada. Kehtivad väärtused algavad 1, 3, 6 ja siis ülespoole kerimine 6, nagu allpool näidatud.

![Azure'i voo Analytics voo üksuste skaala][img.stream.analytics.streaming.units.scale]

Selles artiklis näitab teile, kuidas arvutada ja häälestada päringu Kasutusanalüüsi tööde läbilaskevõime suurendamiseks.

## <a name="embarrassingly-parallel-job"></a>Piinlikult paralleelselt tööd
Piinlikult paralleelselt tööd on kõige scalable stsenaarium Azure'i voo Analytics meil. Ühe sektsiooni ühe astme päringu sisendi ühendub ühe sektsiooni väljund. Selline vastavus saavutamiseks on vaja mõned asjad:

1.  Kui teie päringu loogika sõltub töötleb päringu samas eksemplaris sama klahvi, peate tagama, et sama sektsiooni sisendit, minge sündmused. Korral sündmuse jaoturi, tähendab, et sündmuse andmed, peab olema **PartitionKey** seadmine või kasutage sektsioonitud saatjad. Puhul bloobimälu, tähendab see, millele saadetakse sündmuste partition samasse kausta. Kui teie päringu loogika ei nõua sama klahvi töödelda sama päringu eksemplari, võite ignoreerida nõue. Näide selle oleks lihtsa select-projekti-filter päringu.  
2.  Kui soovitud andmed on esitatud nagu tuleb Sisestuskeel küljel, tuleb tagada, et teie päring on liigendatud. Selleks saate kasutada **Partition järgi** kõiki juhiseid. Mitme toimingud on lubatud, kuid need kõik peab olema sama klahvi liigendatud. Teine asi, Pange tähele, on see, et praegu eraldamine võti peab olema seatud **PartitionId** on täielikult paralleelselt tööd.  
3.  Praegu ainult sündmuse jaoturi ja bloobimälu tugi sektsioonitud väljundi. Sündmuse jaoturi väljundi, peate konfigureerimiseks tuleb **PartitionId** **PartitionKey** välja. Bloobimälu, teil pole vaja midagi teha.  
4.  Teine asi, Pange tähele, et Sisestuskeel sektsioonid arv peab olema väljundi sektsioonid arv. Bloobimälu väljund ei toeta praegu sektsioonid, kuid see sobib, kuna see pärivad eraldamine kava varustava päringu. Sektsiooni väärtused, mis võimaldaks täielikult paralleelselt tööd näited:  
    1.  8 sündmuse jaoturi sisend sektsioonid ja 8 sündmuse jaoturi väljund sektsioonid
    2.  8 sündmuse jaoturi sisestada sektsioonid ja bloobimälu väljund  
    3.  8 bloobimälu Sisestuskeel sektsioonid ja bloobimälu väljund  
    4.  8 bloobimälu Sisestuskeel sektsioonid ja 8 sündmuse jaoturi väljund sektsioonid  

Siin on mõned näide stsenaariumid, mis on piinlikult paralleelselt.

### <a name="simple-query"></a>Lihtne päring
Sisestusmeetodi – sündmuse jaoturi 8 sektsioonide väljundi – sündmuse jaoturi 8 sektsioonide

**Päringu:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

See päring on lihtne filtri ja sellisel kujul me ei pea muretsema eraldamine saadame sündmuse jaoturi sisend. Te teate, on päringu **Partition järgi** , **PartitionId**, et me täita nõue #2 ülevalt. Selle väljundi jaoks läheb vaja konfigureerida sündmuse jaoturi väljundi töö olema seatud **PartitionId** **PartitionKey** välja. Ühe viimase sisse, Sisestuskeel sektsioonid == väljundi sektsioonid. See topoloogia on piinlikult paralleelne.

### <a name="query-with-grouping-key"></a>Päringu rühmitamise abil
Sisestusmeetodi – sündmuse jaoturi 8 sektsioonide väljundi – bloobimälu

**Päringu:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Selles päringus on rühmitamise klahvi ja sellisel kujul sama võti peab olema sama päringu eksemplari töödelda. See tähendab, et läheb vaja saata meie sündmused sündmused jaoturi sektsioonitud viisil. Mis võti teeme kohta? **PartitionId** on töö loogika mõiste, siis me tegelik võti **TollBoothId**. See tähendab, et seate **PartitionKey** sündmuse andmete saadame sündmuse jaoturi olema **TollBoothId** sündmuse. Päringus on **Partition järgi** , **PartitionId**, nii et meil on hea. Väljund, kuna see on bloobimälu, me ei pea muretsema **PartitionKey**konfigureerimise kohta. Nõue #4, uuesti, see on bloobimälu, et me ei pea muretsema. See topoloogia on piinlikult paralleelne.

### <a name="multi-step-query-with-grouping-key"></a>Mitme toimingu päringu rühmitamise abil ###
Sisestusmeetodi – sündmuse jaoturi 8 sektsioonide väljundi – sündmuse jaoturi 8 sektsioonide

**Päringu:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Selles päringus on rühmitamise klahvi ja sellisel kujul sama võti peab olema sama päringu eksemplari töödelda. Kasutame sama strateegiat eelmise päringuna. Päringus on mitu juhiseid. Kas on iga toimingu **Partition järgi** , **PartitionId**? Jah, nii et meil on hea. Väljund, läheb vaja seada **PartitionKey** **PartitionId** nagu eespool ja me näeme ka on sama arv sektsioonid sisend. See topoloogia on piinlikult paralleelne.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Näidisstsenaariumid, mis pole piinlikult paralleelselt

### <a name="mismatched-partition-count"></a>Sobimatud Partition loendamine ###
Sisestusmeetodi – sündmuse jaoturi 8 sektsioonide väljundi – sündmuse jaoturi 32 sektsioonide

Pole oluline, mida päring on sellisel juhul Kuna sisend partition count! = väljundi partition arv.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Ei ole väljundina sündmuse jaoturi või plekid abil
Sisestusmeetodi – sündmuse jaoturi 8 sektsioonide väljundi – PowerBI

PowerBI väljund ei toeta praegu eraldamine.

### <a name="multi-step-query-with-different-partition-by-values"></a>Mitme toimingu päringu väärtustega eri sektsiooni järgi
Sisestusmeetodi – sündmuse jaoturi 8 sektsioonide väljundi – sündmuse jaoturi 8 sektsioonide

**Päringu:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Nagu näete, kasutab teise etapi **TollBoothId** eraldamine võti. See pole sama, mis kõigepealt ja seetõttu on vaja teha on kaartide segamise. 

Need on mõned näited ja counterexamples voo Analytics tööd, on võimalik saavutada on piinlikult paralleelselt topoloogia ja sellega skaala võimalusi. Töö, mis ei sobi ühte need profiilid, tehakse üksikasjalikult, kuidas maksimaalselt mastaapimiseks mõnda muud kanoonilise voo Analytics stsenaariumid tulevikus värskendusi.

Nüüd jaoks kasutada, on üldised juhised allpool:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Streaming töö üksuste maksimaalne arvutamine
Streaming üksused, mida saab kasutada voo Analytics töö koguarv sõltub juhiseid töö ja arv sektsioonid iga etapi jaoks määratletud päringu arv.

### <a name="steps-in-a-query"></a>Päringu juhised
Päring võib olla üks või mitu juhiseid. Iga toimingu on määratletud abil **asendaja** võtmesõna all päringu. Samuti arvestatakse ainult päring, mis on väljaspool **asendaja** märksõna sammuna, näiteks **Valige** lause järgmine päring:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Eelmise päringus on kaks toimingut.

> [AZURE.NOTE] Selle artikli selgitatakse selle valimi päringu.

### <a name="partition-a-step"></a>Samm Partition

Samm eraldamine nõuab järgmised tingimused:

- Sisendandmete allikas peab olema liigendatud. Lisateabe saamiseks vaadake teemat [Sündmuse jaoturi programmeerimisega juhend](../event-hubs/event-hubs-programming-guide.md).
- **Valige** päringu lause peab lugeda sektsioonitud sisendandmete allikas.
- Päringu jooksul etappi peab olema märksõna **Partition järgi**

Kui päring on liigendatud, Sisestuskeel sündmused on töödeldud ja liidetud rakenduses eraldi sektsiooni rühmad ja väljundeid sündmuste luuakse iga rühma jaoks. Kui ühendatud kokku on soovitav, peate looma teise etapi liigendatud kokkuvõtte abil.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Max streaming üksuste töö arvutamine

Kõigi liigendatud toimingute tegemiseks saate koos skaala kuni kuus streaming mõõtühikud voo Analytics töö. Lisada täiendavad streaming üksused, samm peab olema liigendatud. Iga sektsiooni võib olla kuus streaming üksused.

<table border="1">
<tr><th>Päringu töö</th><th>Max streaming üksuste töö</th></td>

<tr><td>
<ul>
<li>Päring sisaldab sammhaaval.</li>
<li>Samm on liigendatud.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Sisendandmete voo on liigendatud 3.</li>
<li>Päring sisaldab sammhaaval.</li>
<li>Samm on liigendatud.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Päring sisaldab kahte toimingut.</li>
<li>Valimata juhiseid on liigendatud.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Andmesisestuse voo on liigendatud 3.</li>
<li>Päring sisaldab kahte toimingut. Sisestuskeel samm on liigendatud ja teiseks ei ole.</li>
<li>SELECT-lause loeb sektsioonitud sisestatud.</li>
</ul>
</td>
<td>24 (18 sektsioonitud juhised) + 6 – liigendatud juhised</td></tr>
</table>

### <a name="examples-of-scale"></a>Näiteid skaala
Järgmine päring arvutab arvu auto läbi teemaks jaamale kolme ebameeldivusi kolme minuti aknas sees. Selle päringu saate mastaabitud kuni kuus streaming üksused.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Päringu rohkem streaming ühikut kasutamiseks nii andmete voona sisendit ja päringu peab liigendatud. Et andmete voo sektsiooni väärtuseks 3, saate muudetud järgmine päring mastaabitud kuni 18 streaming üksused:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Kui päring on liigendatud, Sisestuskeel sündmuste töödeldakse ja eraldi sektsiooni rühmades kokku liita. Iga rühma jaoks luuakse ka väljundi sündmused. Eraldamine võib põhjustada ootamatuid tulemusi, kui väli **Group by** pole selle voo andmesisestuse sektsiooni võti. Näiteks eelmise valimi päringu **TollBoothId** välja pole Input1 sektsiooni võti. Kassa #1 andmeid saab levinud mitu sektsiooni sisse.

Mitme kirje sama Kassa samas langema aknas auto-pass-kaudu arvu luuakse ja iga Input1 sektsioonide töödeldakse eraldi voo Analytics. Kui võti Sisestuskeel sektsiooni ei saa muuta, saate probleemi määratud täiendav samm partition, lisades näiteks:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Selle päringu saate mastaabitud 24 streaming üksused.

>[AZURE.NOTE] On liitumisel kaks voogu, veenduge, et selle voole on liigendatud sektsiooni võti veerg, mille ühendused, mida teha, ja teil on sama palju sektsioonid nii voogu.


## <a name="configure-stream-analytics-job-partition"></a>Voo Analytics töö partition konfigureerimine

**Kohandada streaming ühiku töö**

1. Logige sisse [haldusportaali](https://manage.windowsazure.com).
2. Klõpsake vasakpoolsel paanil **Voo Analytics** .
3. Klõpsake voo Analytics töö, mida soovite mastaapimiseks.
4. Klõpsake lehe ülaosas **skaala** .

![Azure'i voo Analytics voo üksuste skaala][img.stream.analytics.streaming.units.scale]

Azure'i portaalis juurde skaala sätteid jaotises sätted:

![Azure'i portaalis voo Analytics töö konfigureerimine][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Kuvari töö jõudlus

Haldusportaali abil saate jälgida tööd läbilaskevõime sündmuste sekundis:

![Azure'i voo Analytics jälgida töö][img.stream.analytics.monitor.job]

Arvutamine oodatud läbilaskevõime töökoormus rakenduses sündmuste sekundis. Kui soovitud läbilaskevõime on oodatust, häälestada Sisestuskeel partition, häälestada päringu ja täiendavad voogesituse üksuste lisamine oma töö.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Voo Analytics läbilaskevõime on skaalal - Vaarika Pi stsenaarium


Mõistmaks, kuidas voo analytics töö mastaapimiseks tüüpilised stsenaariumi osas töötlemine läbilaskevõime üle mitme Streaming üksused, siin on katse, mis saadab andurite andmeid (kliendid) sündmuse jaoturiga, töötleb ja saadab teatise või statistika väljund mõne muu sündmuse jaoturi.

Kliendi saadab sünteesitud andurite andmeid sündmuse jaoturi JSON vormingus voo analüüsi ja andmete väljund on ka JSON-vormingus.  Siin on, kuidas näeks Näidisandmete meeldib –  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Päringu: "Saada teatis, kui kerge on välja lülitatud"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Mõõtmine läbilaskevõime: Läbilaskevõime kontekstis on summa sisendandmete töödelda voo Analytics fikseeritud summa (10 minutit). Parim töötlemine läbilaskevõime jaoks sisendandmete saavutamiseks nii andmete voona sisendit ja päringu peab liigendatud. **COUNT()**ka mõõta Sisestuskeel mitmel võistlusalal on töödeldud päringus. Töö lihtsalt ootab Sisestuskeel sündmustest, mis tulevad tagamiseks on iga sektsiooni Sisestuskeel sündmuse keskuse piisavalt sisendandmete (umbes 300MB) arvutisse eellaaditud.  

Allpool on tulemused suureneva Streaming üksuste arvu ja vastava sektsiooni loendab sündmuse jaoturi.  

<table border="1">
<tr><th>Sisestuskeel sektsioonid</th><th>Väljundi sektsioonid</th><th>Streaming ühikud</th><th>Pidev jõudlus
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4,06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![img.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
