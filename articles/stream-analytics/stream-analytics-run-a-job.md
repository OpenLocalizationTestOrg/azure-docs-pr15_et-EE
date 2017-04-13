<properties 
    pageTitle="Kuidas alustada streaming töö voo Analytics | Microsoft Azure'i" 
    description="Kuidas streaming töö Azure'i voo Analytics | Õppekeskuse tee lõigu."
    keywords="streaming tööde haldamine"
    documentationCenter=""
    services="stream-analytics"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Azure'i voo Analytics voogesituse töö käivitamise

Kui tööd, päringu ja väljundi kõik määratud on voo Analytics töö alustamiseks.

Oma töö alustamiseks tehke järgmist.

1.  Klõpsake Azure klassikaline portaalis armatuurlaualt töö **alustamiseks** klõpsake lehe allosas.

    ![Töö alustamine nupp](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Azure'i portaalis, klõpsake nuppu **alustada** oma töö lehe ülaosas.

    ![Azure'i portaali töö alustamine nupp](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Määrake kindlaks teha, millal seda tööd alustada tooda väljundit **Käivitamine** väljundväärtuse. Tööd, mis pole varem alustatud vaikesäte on **Töö alguskellaaeg**, mis tähendab, et töö hakkab kohe andmete töötlemiseks. Samuti saate määrata **kohandatud** aja varem (ajalooliste andmete tarbimine) või tulevase (viivitada kuni tulevaste töötlemine). Juhul kui tööd on varem alustatud ja lõpetanud, **On peatatud viimast** suvand on saadaval väljundi viimasest töö jätkamiseks ja andmete kaotsimineku vältimiseks.  

    ![Alusta voogesituse töö aeg](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure'i portaali Alusta voogesituse töö aeg](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Valiku salvestamiseks. Töö olek muutub *algus* ja liigub varsti *töötab* pärast töö on alustanud. Saate **Teate jaoturi**toimingu **käivitamine** edenemist jälgida:

    ![streaming töö edenemine](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure'i portaalis streaming töö edenemine](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
