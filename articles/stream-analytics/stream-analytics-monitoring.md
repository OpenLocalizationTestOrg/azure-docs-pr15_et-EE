<properties 
    pageTitle="Mõistmine voo Analytics töö jälgimine | Microsoft Azure'i" 
    description="Mõistmine voo Analytics töö jälgimine" 
    keywords="päringu jälgimine"
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

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Voo Analytics töö jälgimine ja kuidas jälgida päringute mõistmine

## <a name="introduction-the-monitor-page"></a>Sissejuhatus: Lehe jälgimine

Azure haldusportaali ja Azure portaali surface tootluse mõõdikute, mida saab kasutada jälgida ja oma päringu ja töö jõudluse tõrkeotsingut. 

Klõpsake Azure haldusportaali töötava voo Analytics töö kuvamiseks nende mõõdikute vahekaardil **kuvar** . On viivitus kõige 1 minuti jooksul jõudluse mõõdikud, mis on kuvatud lehe jälgimine.  

  ![Armatuurlaua töö jälgimine](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Azure'i portaalis, liikuge voo Analytics töö olete huvitatud näha mõõdikud ja **jälgimis** jaotist.  

  ![Azure'i portaalis jälgimise töö armatuurlaud](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Esimest korda voo Analytics töö luuakse piirkonnas, peate selle piirkonna diagnostika konfigureerimine. Selleks, kuvatakse jaotises **jälgimine** ja **diagnostika** tera klõpsake suvalist kohta. Siin saate lubada diagnostika ja määrake salvestusruumi konto jaoks jälgida andmeid.  

  ![Azure'i portaalis konfigureerimine päringu diagnostika](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Mõõdikute voo Analytics saadaval


| Meetermõõdustik | Määratlus |
|--------|-------------|
| SU % kasutamine | Streaming halvenemiseta kasutamine määratud skaala menüü töö. Jõudma indikaator 80% või kohale, on suur tõenäosus, et sündmuse töötlemiseks seniks või lõpetanud ja edenemise. |
| Sisestuskeel sündmused | Suurt hulka andmeid, mis on saadud voo Analytics töö, sündmuste arv. See saab kinnitada, et sündmuste saadetakse sisendandmete allikas. |
| Väljundi sündmused | Suurt hulka andmeid, mis on saadetud voo Analytics töö suunata väljund sündmuste arv. |
| Tellimuse-ja sündmused | Välja tellimuse, mis on antud korrigeeritud ajatempel, mis põhineb sündmuse tellimine poliitika või lähevad saanud sündmuste arv. See võivad mõjutada konfiguratsiooni akna Tellimuse välja arv hälbe. |
| Andmete teisendamine tõrked | Andmete teisendamine vigade tekkinud voo Analytics töö arv. |
| Käitusaja tõrked | Arv tõrkeid, mis tehakse teostamise voo Analytics töö käigus. |
| Hilinenud Sisestuskeel sündmused | Allikas, mida teha kukkunud või nende ajatempli hiljaks sündmuste arv on kohandatud hilinenud saabumisel hälbe akna sündmuse järjestusega poliitika konfiguratsiooni põhjal. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Azure'i haldusportaal jälgimis kohandamine ##

Kuni 6 mõõdikute saab kuvada diagrammil.

Asemel väljatulemid suhteline väärtused (ainult jaoks iga mõõdiku lõppväärtus) absoluutse väärtuste (Y-telg kuvatud), valige käsk või absoluutne diagrammi kohal.

  ![Päringu kuvari Suhteline absoluutne](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Mõõdikute saab vaadata kuvari diagrammi liitmised on 1 tund, 12 tundi, 24 tundi või 7 päeva.

Ajavahemiku muutmine mõõdikute diagramm kuvatakse, valige 1 tund, 24 tundi või 7 päeva diagrammi kohal.

  ![Päringu kuvari ajaskaala](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Saate seada reeglid saate teavitavad teid meili teel juhuks, kui töö ületab läve määratletud. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Kohandamise Azure'i portaalis jälgimine ##

Saate muuta diagrammi mõõdikud, tüüpi ja ajastada vahemiku Redigeeri diagrammi seadete. Lisateabe saamiseks vaadake, [Kuidas kohandada jälgimiseks](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Azure'i portaali päringu kuvari ajaskaala](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Töö oleku vaatamine

Voo Analytics tööde oleku saab vaadata Azure'i klassikaline portaalis, kus näete tööde loendit. Näete, klõpsake ikooni voo Analytics Azure'i klassikaline portaalis tööde loendi.

| Olek | Määratlus |
|--------|------------|
| Loodud | Töö on loodud, kuid pole käivitatud. |
| Käivitamine | Kasutaja klõpsasite avakuval töö ja töö on käivitamine |
| Töötab | Töö eraldatakse töötlemine sisendit või töödelda sisendi ootamine. Kui töö kuvatakse töötab riik ei tooda väljundit, on tõenäoline, et andmete töötlemise ajaakna on suur või päringu loogika on keeruline. Teise põhjuseks võib olla, et praegu pole kõik andmed, mis saadetakse töö. |
| Peatamine | Kui keegi Stop töö ja töö peatamine. |
| Peatatud | Töö on peatatud. |
| Halvenenud | See olek näitab, et voo Analytics tööd on tekib siirdamiseks tõrked (jaoks ex. Sisend/väljund tõrkeid, töötlemine tõrkeid, teisendamise tõrgete jne). Töö töötab, kuid on palju vigu, mis on loodud. Selle töö arutada klient ja kliendi näete toimingute logid tõrgete kohta. |
| Nurjus. | See näitab, et töö nurjus tõrke tõttu, ja töötlemine on peatatud. Kliendi peab toimingute logid uurida, et silumine tõrked. |
| Kustutamine | See näitab, et töö kustutatakse. |

## <a name="diagnosis"></a>Diagnoosimine

Azure'i haldusportaal töö armatuurlaua teave kohta, kuhu soovite diagnoosimise, st sisendina väljundid ja/või toimingute Logi välja. Saate klõpsata linki valmis sobivasse diagnoosimise vaadata.

  ![Päringu kuvari tõrge](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Klõpsates sisestuskeel või väljund ressursi pakub üksikasjalikke diagnostikateave. See värskendatakse diagnoosimise värskeimaid andmeid töö töötamise ajal.

  ![Päringu diagnostika](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
