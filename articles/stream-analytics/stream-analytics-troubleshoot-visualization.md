<properties
    pageTitle="Visualiseerimine ja voo Analytics töö tõrkeotsing | Microsoft Azure'i"
    description="Saate teada, kuidas visualiseerida voo Analytics töö müügivõimaluste iseteenindusliku tõrkeotsingu diagnostika skeem funktsiooni abil."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualiseerimine ja tõrkeotsing voo Analytics tööde haldamine

Voo Analytics, nagu tehnoloogiad pilvepõhist tõrkeotsing on mõnikord vaja uurida, miks tööd ei tööta oodatud väljundi (või mis tahes väljundi, et asi). Seda silmas pidades voo Analytics pakub võimalus visualiseerimine voogesituse töö jaoks. See on ka mugav modelleerimine tööriist ja on küljel kasu nende nõuab dokumentatsiooni oma töö.

Visualiseeringu paanil sisendeid on nähtavad ka täidetakse päring ja seejärel Kõik väljundid konfigureeritud. Probleeme ühenduvuse või konfiguratsiooni võib muutuda selgemaks ja see võib olla kasulik konfiguratsioonist visuaali kuvamiseks.

## <a name="using-the-diagnosis-diagram-tool"></a>Diagnostika skeem tööriista abil

See visualizer juurdepääsemiseks lihtsalt klõpsake tera "Settings" nupu "Diagnoosimise skeemi" selle voo Analytics töö.

![Stream-Analytics-Troubleshoot-Visualization-DIAGNOSIS-Diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Iga sisestus- ja väljundi on värv kodeeritud tähistamiseks praegune seisund komponendi, nagu allpool näidatud.

![Stream-Analytics-Troubleshoot-Visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Kui kasutaja soovib vaadata mõista andmete liikumisele töö sees vahe päringuetappe, visualiseerimine tööriista annab ülevaate päringu jaotus selle osa juhiseid ja meilivoo jada. Iga päringuetapi klõpsamisel kuvatakse vastav jaotis päringu redigeerimine paan, nagu on näidatud. 

![Stream-Analytics-Troubleshoot-Visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
