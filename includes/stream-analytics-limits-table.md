<properties 
   pageTitle="Voo Analytics piirangud tabel"
   description="Kirjeldatakse süsteemi piirangud ja Soovitatavad suurused voo Analytics komponendid ja ühendused."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Piiratud identifikaator | Limiit       | Kommentaarid |
|----------------- | ------------|--------- |
| Maksimumarv Streaming tellimuse piirkonna kohta | 50 | Taotluse streaming mõõtühikud tellimuse lisaks 50 suurendamiseks saate tehtud [Microsofti toe](https://support.microsoft.com/en-us)poole pöördumist. |
| Maksimaalse läbilaskevõime Streaming üksuse | 1MB / s * | Maksimaalse läbilaskevõime kohta SU sõltub seda stsenaariumi. Tegelik läbilaskevõime võib olla väiksem ja sõltub päringu keerukuse ja eraldamine. Täiendavad üksikasjad leiate artikli [skaala Azure'i voo Analytics töö läbilaskevõime suurendamiseks](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maksimum sisendeid töökoha kohta | 60 | On suur piirang 60 sisendeid voo Analytics töökoha kohta. |
| Suurim arv väljundeid töökoha kohta | 60 | On suur piirang 60 väljundeid voo Analytics töökoha kohta. |
| Funktsioonide töö arvu ülempiir | 60 | On suur piirang 60 funktsioonide voo Analytics töökoha kohta. |
| Maksimum töökohtade müüginäitajaid piirkonna kohta | 1500 | Iga tellimuse võib olla kuni 1500 töö geograafilise asukoha kohta. |