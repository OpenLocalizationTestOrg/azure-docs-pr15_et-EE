<properties
    pageTitle="Ülevaade: Microsoft Azure mõõdikute | Microsoft Azure'i"
    description="Saate teada, kuidas kohandada Azure jälgimisega seotud diagrammid."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Microsoft Azure mõõdikute ülevaade

Saate jälitada kõigi Azure'i teenuste olulisemad mõõdikud, mille abil saate jälgida seisund, jõudluse, kättesaadavus ja teenuste kasutamist. Saate vaadata nende mõõdikute Azure portaali ja programmiliselt juurde pääseda mõõdikute täiskomplekti saate kasutada ka [REST API -ga](https://msdn.microsoft.com/library/azure/dn931930.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

Teatud teenuste, peate selleks, et näha, mis tahes mõõdikute diagnostika sisse lülitada. Teiste, näiteks virtuaalmasinates, saate mõõdikute elementaarne, kuid peate lubamiseks täielik seada suure sagedusega mõõdikute. Vt lisateavet [diagnostika- ja lubada](insights-how-to-use-diagnostics.md) .

## <a name="using-monitoring-charts"></a>Kasutades jälgimisega seotud diagrammid

Te saate diagrammi, mis tahes mõõdikud neile valite aja jooksul.

1. [Azure portaali](https://portal.azure.com/)nuppu **Sirvi**ja seejärel ressursi teid huvitava jälgimine.

2. **Jälgimine** jaotis sisaldab kõige olulisemad mõõdikud iga Azure'i ressurss. Näiteks web app on **taotlusi ja tõrkeid**, kus nagu virtuaalse masina oleks **CPU protsent** ja **ketta lugemine ja kirjutamine**:  ![lens jälgimine](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Mis tahes diagrammi klõpsamisel kuvatakse **meetermõõdustik** tera. Enne, lisaks graafik, on tabel, mis kuvatakse liitmised parameetrid (nt Keskmine, miinimum- ja maksimumväärtused valitud vahemikus aeg). On väiksem kui ressursi teatiste reeglid.
    ![Argumendil blade](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Kohandada read, mis kuvatakse, klõpsake nuppu diagrammi **redigeerimine** nuppu või argumendil enne käsu **Redigeeri diagrammi** .

5. Enne päringu redigeerimine saate teha kolm toimingut:
    - Ajavahemiku muutmine
    - Aktiveerimine vahelise ilme riba ja joon
    - Valige muu metics ![päringu redigeerimine](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Muutmise aja vahemikus on sama lihtne kui valida mõne muu vahemiku (nt **Tunnis**) ja klõpsake nuppu **Salvesta** tera allosas. Soovi korral saate ka **kohandatud**, mis võimaldab teil valida aega viimase 2 nädala jooksul. Näiteks saate vaadata kogu kaks nädalat, või, eile lihtsalt 1 tund. Tippige tekstiväljale sisestada eri tund.
    ![Kohandatud ajavahemiku](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Allpool ajavahemiku, saate chan valige mis tahes arv mõõdikute kuvamiseks klõpsake diagrammi.

8. Kui klõpsate nuppu Salvesta muudatused salvestatakse see kindla ressurss. Näiteks, kui teil on kaks virtuaalmasinates ja muudate ühe diagrammi, see ei mõjuta teise.

## <a name="creating-side-by-side-charts"></a>Kõrvuti Diagrammide loomine

Võimas kohandus portaalis saate lisada nii palju diagramme, nagu soovite.

1. **…** Klõpsake menüü ülaosas tera **paanide lisamine**.  
    ![Menüü Lisa](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Seejärel saate valige valida diagrammi **Galerii** ekraani paremas servas:  ![Galerii](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Kui te ei näe mõõdiku soovite, saate alati lisada üht eelseadistatud mõõdikute ja **redigeerida** kuvamiseks meetermõõdustik, et peate diagrammi.

## <a name="monitoring-usage-quotas"></a>Piirmäärasid jälgimine

Enamik mõõdikute Kuva trendide aja jooksul, kuid teatud andmed, nt piirmäärasid, on kellaaja punkti teave läve.

Näete piirmäärasid enne ressursid, mis on:

![Kasutus](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Nagu mõõdikute, kus saate [REST API -ga](https://msdn.microsoft.com/library/azure/dn931963.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programmiliselt juurde pääseda täiskomplekti piirmäärasid.

## <a name="next-steps"></a>Järgmised sammud

* [Teatiste vastuvõtmine](insights-receive-alert-notifications.md) iga kord, kui mõõdiku ületab läve.
* [Diagnostika- ja lubada](insights-how-to-use-diagnostics.md) koguda üksikasjalikku suure sagedusega mõõdikute teenust.
* Veendumaks, et teie teenus on saadaval ja reageeri [mastaapimiseks arvu automaatselt](insights-how-to-scale.md) .
* Kui soovite mõista täpselt kuidas oma koodi läbimiseks pilveteenuses [kuvari rakenduse jõudlus](../application-insights/app-insights-azure-web-apps.md) .
* [Rakenduse ülevaated JavaScripti rakenduste ja veebilehtede](../application-insights/app-insights-web-track-usage.md) abil saate kliendi Kasutusanalüüsi veebilehe brauserite kohta.
* [Kuvari-saadavus ja mis tahes veebilehe tundlikkuse](../application-insights/app-insights-monitor-web-app-availability.md) rakenduse ülevaated nii, et saate teada, kui teie leht on alla.
