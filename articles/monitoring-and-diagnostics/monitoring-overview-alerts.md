<properties
    pageTitle="Ülevaade Microsoft Azure'i teatised | Microsoft Azure'i"
    description="Teatiste võimaldab teil jälgida Azure ressursi mõõdikute, sündmuste või logid ja kohaletoimetamisest teie määratud tingimuste."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Ülevaade Microsoft Azure'i teatised


Selles artiklis kirjeldatakse, milliseid teatisi on oma eelised ja kuidas neid kasutamise alustamine.  

## <a name="what-are-alerts"></a>Mis on teatised?
Teatiste on jälgimise Azure ressurssi mõõdikute, sündmuste, või logid ja seejärel teatatud, kui tingimus on täidetud määratud meetodit.

Saate teatisi põhjal:

- **Argumendil väärtused**: teemasse käivitab, kui määratud mõõtühiku väärtus lõikab, mis teil määrata, kas suunas. Mis on see käivitab nii kui tingimus on täidetud esmalt ja seejärel hiljem kui see tingimus on ei ole enam täidetud.
- **Tegevuste sündmuste logi**: teemasse käivitab iga sündmuse või ainult siis, kui sündmuste arv ilmneda.

## <a name="alerts-in-different-azure-services"></a>Azure'i teenuste teatised

Teatiste on saadaval erinevad teenused, sh:

- **Rakenduse ülevaated**: lubab web testi ja mõõdiku teatised. Vt [Rakenduse ülevaated teatised määrata](../application-insights/app-insights-alerts.md) ja [kuvari-saadavus ja mis tahes veebisaidile reageerimine](../application-insights/app-insights-monitor-web-app-availability.md).
- **Log Kasutusanalüüsi (Operations Management Suite)**: võimaldab diagnostikalogid, et Log Kasutusanalüüsi marsruutimise. Operations Management Suite võimaldab meetermõõdustik, log ja muude teatis. Lisateabe saamiseks vt [Log Kasutusanalüüsi teatised](../log-analytics/log-analytics-alerts.md).  
- **Azure'i kuvari**: võimaldab põhineb argumendil väärtused nii tegevuste ja sündmuste teatised. Azure'i kuvari sisaldab [Azure'i kuvari REST API -ga](https://msdn.microsoft.com/library/dn931943.aspx).  Lisateabe saamiseks vt [kasutamine Azure portaali, PowerShelli, või käsurea liides teatiste loomine](insights-alerts-portal.md).

## <a name="alert-actions"></a>Teatiste toimingud
Saate konfigureerida teatise teha järgmist:

- Saada meiliteatisi teenuse administraator, kaasadministraatorite või e-posti aadressid, teie määratud.
- Helistage webhook, mille abil saate käivitada toiminguid täiendavate automatiseerimine.

 ![Teatiste ülevaade](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Järgmised sammud

Saate teavet teatiste reeglid ja konfigureerimine nende abil:

- [Azure'i portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [Käsurea liides (CLI)](insights-alerts-command-line-interface.md)
- [Azure'i kuvari REST API-ga](https://msdn.microsoft.com/library/azure/dn931945.aspx)
