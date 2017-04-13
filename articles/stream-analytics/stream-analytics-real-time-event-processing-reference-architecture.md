<properties 
    pageTitle="Reaalajas sündmuse voo analüüs sündmuse töötlemiseks | Microsoft Azure'i" 
    description="Siit saate teada, kuidas Azure'i teenuseid saate reaalajas event töötlus ja analüüsi, mis võimaldab siduda." 
    keywords="reaalajas töötlemine, event töötlus, viide arhitektuur"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Viide arhitektuur: Microsoft Azure voo Analytics töötlemine reaalajas sündmuse

Viide arhitektuur reaalajas sündmuse töötlemiseks Azure'i voo Analytics eesmärk üldise näidis Microsoft Azure'i lahendusena service (PaaS) voo töötlemiseks reaalajas platvormi kasutamise kohta.

## <a name="summary"></a>Kokkuvõte

Traditsiooniliselt analytics lahenduste põhjal võimaluste ETL (ekstrakti, transformatsioon, laadi) ja võrgupõhised andmebaasid, nt enne analüüsi andmete talletuskoht. Muutmisega nõuded, sealhulgas kiiresti saabuvate rohkem andmeid, on see olemasoleva mudel lükkamine piiri. Üks lahendus on võimalus analüüsida andmete voogu enne salvestusruumi teisaldada ja ei ole uue võimaluse, lähenemine on antud üle kõik valdkonna otsinguvertikaalide suuresti vastu. 

Microsoft Azure'i pakub mõne analytics tehnoloogiaid, mis võivad massiivi muu lahendus stsenaariumid ja nõuded olulisel kataloogi. Valides Azure'i teenuste-lõpuni lahenduse juurutamiseks võib olla keeruline antud laius pakkumisi. See paberi eesmärk on kirjeldatud võimaluste ja koostoimimist erinevate Azure teenuseid, mis toetavad sündmuse streaming lahenduse. Selgitatakse ka mõned stsenaariumid, kus kliendid saavad kasutada seda tüüpi lähenemine.

## <a name="contents"></a>Sisu

- Ametlik Kokkuvõte
- Reaalajas Analytics tutvustus
- Kuvandi Azure reaalajas andmed.
- Reaalajas Analytics tavastsenaariumid
- Arhitektuur ja komponendid
    - Andmeallikad
    - Andmete integreerimine kiht
    - Reaalajas Analytics kiht
    - Andmete salvestusruumi kiht
    - Esitluse / tarbimine Layer
- Kokkuvõte

**Autor:** Charles Feddersen lahenduse arhitekti ülevaateid andmekeskuse tipptasemel, Microsoft Corporation

**Avaldatud:** Jaanuar 2015

**Paranduse:** 1.0

**Allalaadimine:** [Reaalajas sündmuse Microsoft Azure'i voo Analytics töötlemine](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
