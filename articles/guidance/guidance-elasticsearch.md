
<properties
   pageTitle="Azure'i suuniste Elasticsearch | Microsoft Azure'i"
   description="Elasticsearch Azure juhised selle kohta."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure juhised selle kohta 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch on väga paindlik avatud lähtekoodi otsingumootori ja andmebaasi. See on kasulik olukorrad, mis on vaja kiiresti analüüsi ja suurte andmekomplektide säilitatava teabe leidmiseks. Juhised vaatab olulisemad aspektid Elasticsearch kobar, keskendudes võimalusi, kuidas optimeerida ja testida konfiguratsiooni kujundamisel silmas pidada.

> [AZURE.NOTE]Juhised eeldab mõne lihtsa tundmine Elasticsearch. Üksikasjalikumat teavet, külastage [Elasticsearch veebisaidi](https://www.elastic.co/products/elasticsearch). 

- **[Töötab Elasticsearch Azure][]** on lühiülevaade Elasticsearch üldist struktuuri ja kirjeldatakse, kuidas rakendada mõne Elasticsearch kobar Azure'i abil. 
- **[Tuning andmete manustamisest jõudlust Elasticsearch Azure][]** kirjeldatakse mõnda Elasticsearch kobar juurutamise ja põhjalikumat analüüsi konfiguratsiooni erinevate suvandite andmete manustamisest kõrge eeldasite silmas pidada.
- **[Tuning andmete liitmise ja päringu jõudluse jaoks Elasticsearch Azure][]** pakub suvandid arutada sügavuti analüüsida otsustada, kuidas optimeerida oma süsteemi jõudluse päringu ja otsida.
- **[Konfigureerida paindlikkuse ja taastamise kohta Elasticsearch Azure][]** kirjeldatakse olulisi funktsioone Elasticsearch kobar, mis aitab vähendada võimalusi andmete kaotsimineku ja laiendatud andmete taastamine korda.
- **[Luua jõudluse testimise keskkond Elasticsearch Azure][]** kirjeldatakse, kuidas häälestada testimiseks jõudluse andmete manustamisest ja päringu töökoormus on Elasticsearch klaster keskkonnas. 
- Töötava jõudluse testide abil JMeter testi lepingute Java koodi kui rongid test näiteks üleslaadimine andmed üheks Elasticsearch kobar ülesandeid täitvate koos rakendatud **[rakendada mõne JMeter testida kavandamine Elasticsearch][]** kokkuvõte.
- **[Juurutamine JMeter rongid proovivõtuseadme katsetamiseks Elasticsearch jõudlust][]** kirjeldatakse, kuidas luua ja kasutada rongid proovivõtja, mida saate luua ja üles laadida andmed on Elasticsearch kobar. See näeb väga paindlik lähenemine laadimine testimine andvate suurt hulka andmeid testimine ilma sõltuvalt välisandmete failid. 
- **[Töötab testide automatiseeritud Elasticsearch paindlikkust][]** Kokkuvõte selles sarjas kasutatud paindlikkust testide käivitamise. 
- **[Töötab testide automatiseeritud Elasticsearch jõudluse][]** Kokkuvõte selles sarjas kasutatud jõudluse testide käivitamise.


[Azure'i Elasticsearch töötavate]: guidance-elasticsearch-running-on-azure.md
[Elasticsearch Azure andmete manustamisest jõudluse häälestamine]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Luua tulemuslikkuse Elasticsearch Azure keskkonna testimine]: guidance-elasticsearch-creating-performance-testing-environment.md
[Elasticsearch JMeter testi lepingu rakendamine]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[JMeter rongid proovivõtuseadme katsetamiseks Elasticsearch jõudlust juurutamine]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Andmete koondamine ja Azure Elasticsearch päringu jõudluse häälestamine]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Konfigureerimine Elasticsearch Azure paindlikkuse ja taastamine]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Töötab testide automatiseeritud Elasticsearch paindlikkust]: guidance-elasticsearch-running-automated-resilience-tests.md
[Töötab testide automatiseeritud Elasticsearch jõudlus]: guidance-elasticsearch-running-automated-performance-tests.md
