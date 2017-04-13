<properties
    pageTitle="Azure'i teenuse seisund Azure'i kuvari logid abil jälgida | Microsoft Azure'i"
    description="Uurida, millal on Azure kogenud jõudluse degradeerumine või teenuse segadusi. "
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Azure'i teenuse seisund abil Azure'i kuvari logid jälgimine

Azure'i publicizes iga kord, kui seal on teenuse katkemise või jõudluse degradeerumine. Saate sirvida need sündmused Azure portaali ja programmiliselt juurde pääseda sündmuste täiskomplekti saate kasutada ka [REST API -ga](https://msdn.microsoft.com/library/azure/dn931927.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Teenuse seisund logid tellimuse sirvimine

1. [Azure'i portaali](https://portal.azure.com/)sisse logima.

2. Klõpsake **Avaleht** peaksite paan nimega **teenuse seisund**. Klõpsake seda.

    ![Avaleht](./media/insights-service-health/Insights_Home.png)

3. Kõigi piirkondade Azure loendi kuvamine Klõpsake mis tahes regiooni avab tegevuste Logi päringu, mis näitab teenusejuhtumitega, mis on mis tahes tellimuste mõjutada viimase 24 tunni jooksul.

    ![Tegevuste Logi tellimuse teenuse seisund](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Näete üksikute juhtum üksikasjad, klõpsates tabeli sündmusega.

5. Saate muuta **kuuline ajavahemik** kuvamiseks kauem aega.

## <a name="next-steps"></a>Järgmised sammud

* [Kuvari-saadavus ja mis tahes veebilehe tundlikkuse](../application-insights/app-insights-monitor-web-app-availability.md) rakenduse ülevaated nii, et saate teada, kui teie leht on alla.
