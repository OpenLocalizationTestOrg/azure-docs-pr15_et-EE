<properties
    pageTitle="Päringute Kasutusanalüüsi voo teatiste häälestamine | Microsoft Azure'i"
    description="Mõistmine voo Analytics teavitamine"
    keywords="teatiste häälestamine"
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Azure'i voo Analytics töö teatiste häälestamine

## <a name="introduction-monitor-page"></a>Sissejuhatus: Kuvari leht

Saate häälestada teatisi käivitab teatise mõõdiku jõuab teie määratud tingimusele.

Näiteks "kui väljundi sündmuste viimase 15 minutit on < 100, e-posti teatise saatmine e-posti id: xyz@company.com”.

Reegleid saab häälestada mõõdikute portaali kaudu või võib olla konfigureeritud [programmiliselt](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) Logi andmete üle.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Häälestage teatised Azure'i klassikaline portaali kaudu

On kaks võimalust häälestada teatised Azure'i klassikaline portaalis.  

1.  Vahekaardil **kuvar** voo Analytics oma tööd  
2.  Toimingute Logi teenuste haldus  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Teatise kaudu portaali töö vahekaardil kuvar häälestamine

1.  Valige vahekaardil kuvar mõõdiku armatuurlaua all olevat nuppu **Lisa reegel** ja reeglite häälestamise.  

    ![Armatuurlaua](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Nimi ja kirjeldus teatise määratlemine  

    ![Reegli loomine](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Sisestage piirmäärad, teatiste hindamise akna ja teatise toimingud  

    ![Tingimuste määratlemine](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Häälestage teatised kaudu tehtavad toimingud logid

1.  Minge halduse teenused [Azure klassikaline portaali](https://manage.windowsazure.com)vahekaarti **teatised** .  
2.  Klõpsake nuppu **Lisa reegel**  

    ![Kriteeriumid](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Määratle nimi ja kirjeldus teatise. Valige teenuse tüüp ja töö nimi nimeks teenuse "Voo Analytics".  

    ![Teatise määratlemine](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Azure'i portaalis teatiste häälestamine ##

Azure'i portaalis, sirvige voo Analytics töö olete huvitatud teavitamine sisse ja klõpsake nuppu jaotises **jälgimine** .  **Meetermõõdustik** tera, mis avab, klõpsake käsku **Lisa teatis** .

  ![Azure'i seadistamine](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Saate oma reegli nimi ja valige kirjeldus, mis kuvatakse meilisõnumi teatis.

Kui valite mõõdikute saate valida tingimus ja mõõdiku piirmäära väärtus.

  ![Azure'i portaali valige meetermõõdustik](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Leiate Azure'i portaalis teatiste konfigureerimise kohta rohkem üksikasju, [teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
