<properties 
    pageTitle="Toiminguga silumine ja teenus logib voo Analytics | Microsoft Azure'i" 
    description="Kuidas kasutada voo Analytics Logi" 
    keywords="teenuste logid"
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Silumine voo Analytics töö ja töötamise logide abil

Kõigi Azure'i teenuste pakkumise funktsionaalseid logimine sõnumeid kasutajatele seotud haldustoimingute kirje üksikasjad. Azure'i voo Analytics, seda teavet saab kasutada silumine vaatamine töö olek, töö edenemine, näiteks ja tõrge sõnumid töö edenemist jälgida aja jooksul kaudu käivitada väljund töötlemine.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Toiming logib Azure'i haldusportaal otsimine

Logi juurde kahel viisil:  

- Armatuurlaua voo Analytics töö  
- Azure'i klassikaline portaalis halduse teenused  

## <a name="dashboard-of-the-stream-analytics-job"></a>Armatuurlaua voo Analytics töö

Vastavate logid voo Analytics töö link kuvatakse projekti armatuurlaua vahekaardil. Kui klõpsate linki, seab filtrid, nii, et see näitab uusim logid teatud töö puhul.

  ![Valige halduse teenuste logid](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Teenuste haldus

Käsitsi liikumiseks toiming logid voo analüüsi ja muude teenuste Azure'i klassikaline portaalis:

1.  Klõpsake **Halduse teenused** [Azure'i klassikaline portaalis](https://manage.windowsazure.com).
2.  Valige **Teenuse nimi** **Voo Analytics** **Tüüp** ja töö nime.  

  ![Valige voo Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Auditilogide Azure'i portaalis otsimine ##

Leidmiseks funktsionaalseid logid oma voo Analytics töö Azure'i portaalis, klõpsake nuppu **Sirvi** ja valige **auditilogid**.

  ![Azure'i portaalis valige voo Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

See avab tera nähtaval sündmuste viimase 7 päeva ressursid kõik teie tellimus.  Saate filtreerida kuvamiseks määrake tüübi või aja jooksul sündmused, klõpsates käsku **Filtreeri** .

  ![Azure'i portaalis valige voo Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Log üksikasjade

Saate filtreerida, ajavahemiku ja oleku vaatamiseks logid oma töö.

Azure'i haldusportaal klõpsake valitud sündmuse üksikasjade kuvamiseks akna allosas nuppu **üksikasjad** . 

  ![Valige andmed](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Azure'i portaalis, klõpsake logikirjet näha üksikasjalikku sündmuste sees.

  ![Azure'i portaalis valige üksikasjad](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Sealt saate avada **üksikasjad** tera klõpsates soovitud sündmust.

  ![Azure'i portaalis valige üksikasjad](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Nurjunud töö silumine

Azure'i haldusportaal, klõpsake ikooni Otsi ja tippige "nurjus. See annab tulemuseks kõik logid koos ebaõnnestumist. 

  ![Nurjunud töö silumine](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Azure'i portaalis, saate filtreerida taseme sõnumi vaatamiseks **kriitiline** sündmused.

  ![Azure'i portaali silumine](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Valige mõni ebaõnnestumisi ja **üksikasjad** tõrke kohta lisateabe saamiseks klõpsake nuppu.  Mõned tõrketeated ka teavet selle kohta, kuidas probleemi leevendada. 

  ![Toimingu üksikasjad](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Kui teil on vaja [tugiteenuste](https://azure.microsoft.com/support/options/) või teavet [MSDN-i Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)meeskond, Pange tähele toimingu üksikasjad, spetsiaalselt **Korrelatsiooni ID -d**. 

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
