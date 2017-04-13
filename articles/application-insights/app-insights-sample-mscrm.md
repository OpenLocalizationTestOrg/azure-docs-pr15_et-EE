<properties 
    pageTitle="Juhend: Jälgimine Microsoft Dynamics CRM-i rakenduse ülevaated" 
    description="Telemeetria toomine Microsoft Dynamics CRM Online'i abil rakenduse ülevaated. Samm-sammult juhendi häälestamise saada andmeid, visualiseerimine ja ekspordi." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Juhend: – Unsafe for Microsoft Dynamics CRM Online'i abil rakenduse ülevaated telemeetria

Selles artiklis kirjeldatakse, kuidas telemeetria andmete toomiseks [Microsoft Dynamics CRM Online'i](https://www.dynamics.com/) abil [Visual Studio rakenduse ülevaated](https://azure.microsoft.com/services/application-insights/). Selgitame kogu protsessi rakenduse ülevaated skripti lisamine rakenduse, andmete ja andmete visualiseerimine.

>[AZURE.NOTE] [Sirvige valimi lahendus](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Rakenduse ülevaated lisamine uude või olemasolevasse CRM Online'i eksemplari 

Rakenduse jälgida, saate lisada ka rakenduse ülevaateid SDK rakenduse. SDK saadab telemeetria [Rakenduse ülevaated portaali](https://portal.azure.com), kus saate kasutada meie võimsate analüüsiriistade ja diagnostikatööriistu või andmete eksportimine salvestusruumi.

### <a name="create-an-application-insights-resource-in-azure"></a>Azure on rakenduse ülevaated ressurss loomine

1. Saada [konto Microsoft Azure](http://azure.com/pricing). 
2. [Azure'i portaali](https://portal.azure.com) sisse logida ja lisada uue ressursi rakenduse ülevaated. See on, kus teie andmeid töödeldakse ja kuvada.

    ![Klõpsake nuppu +, rakenduse ülevaated arendaja teenused.](./media/app-insights-sample-mscrm/01.png)

    ASP.net-i valida rakenduse tüüp.

3. Avage vahekaart Kiirkäivituse ja avage skripti kood.

    ![](./media/app-insights-sample-mscrm/03.png)

**Koodileht lahti jätta** ajal teha järgmine juhis mõnes muus brauseriaknas. Peate koodi varsti. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>JavaScripti web ressursi Microsoft Dynamics CRM-i loomine

1. Avage oma CRM Online'i eksemplari ja logige sisse administraatoriõigustega.
2. Avatud Microsoft Dynamics CRM-i sätted, kohandusi, süsteemi kohandamine

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Saate luua JavaScripti ressursi.

    ![](./media/app-insights-sample-mscrm/07.png)

    Pange nimi, valige **Script (JScripti)** ja avage tekstiredaktor.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopeeri kood rakenduse ülevaated. Kopeerimise ajal veenduge, et ignoreerida skripti sildid. Vaadake allpool pildil:

    ![](./media/app-insights-sample-mscrm/09.png)

    Kood sisaldab instrumentation klahvi, mis tuvastab teie teadmisi ressurssi.

5. Salvesta ja avalda.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Dokumendi vormid

1. Avage Microsoft CRM Online'i vormi konto

    ![](./media/app-insights-sample-mscrm/11.png)

2. Avage vorm atribuudid

    ![](./media/app-insights-sample-mscrm/12.png)

3. Saate lisada JavaScripti web ressursi loodud

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Salvestamine ja avaldamine vormi kohandused.


## <a name="metrics-captured"></a>Jäädvustatud mõõdikud

Nüüd olete seadistanud telemeetria jäädvustada vormi. Iga kord, kui seda kasutatakse, saadetakse teie rakenduse ülevaated ressursi andmed.

Siit leiate näited andmed, mida näete.

#### <a name="application-health"></a>Rakenduse seisund

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Brauseri erandid:

![](./media/app-insights-sample-mscrm/17.png)

Klõpsake diagrammi, et saada rohkem üksikasju:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Kasutus

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Brauserid

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geograafiline asukoht

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Kollased lehe vaate taotlus

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Proovi kood

[Sirvige proovi kood](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Saate teha ka põhjalikult analüüsida, kui [Microsoft Power BI andmed eksportida](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Valimi Microsoft Dynamics CRM-i lahendus

[Siin on rakendatud Microsoft Dynamics CRM-i valimi lahendus] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Lisateave

* [Mis on rakenduse ülevaated?](app-insights-overview.md)
* [Rakenduse ülevaated veebilehtedel](app-insights-javascript.md)
* [Lisateavet näidiseid ja juhendavad tutvustused](app-insights-code-samples.md)

 
