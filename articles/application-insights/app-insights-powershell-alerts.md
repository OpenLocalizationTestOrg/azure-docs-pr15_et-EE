<properties
    pageTitle="PowerShelli kasutamine rakenduse ülevaated teatiste seadmine"
    description="Automatiseerida konfigureerimine rakenduse ülevaated saada e-kirju argumendil muudatuste kohta."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>PowerShelli kasutamine rakenduse ülevaated teatiste seadmine

[Visual Studio rakenduse ülevaated](app-insights-overview.md) [teatiste](app-insights-alerts.md) konfiguratsiooni automatiseerida.

Lisaks saate [määrata oma vastuse teatise automatiseerimiseks webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Ühekordse häälestamine

Kui te pole kasutanud PowerShelli enne Azure'i tellimus:

Installige Azure'i PowerShelli mooduli arvutisse, kuhu soovite selle skriptide käitamiseks.

 * Installige [Microsoft Web platvormi Installer (v5 või uuem versioon)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Kasutage seda Microsoft Azure'i PowerShelli installimine


## <a name="connect-to-azure"></a>Azure'i ühendamine

Azure'i PowerShelli ja luua [ühenduse tellimuse](../powershell-install-configure.md)alustamiseks tehke järgmist.

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Saada teatisi

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Teatise lisamine


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Näide 1

Saada mulle kui serveri reageerimine HTTP 5 minuti jooksul, mis on aeglasem kui 1 teise. Minu rakenduse ülevaated ressursi nimetatakse IceCreamWebApp ja see on ressursirühm Fabrikam. Ma olen Azure tellimuse omanik.

GUID on tellimuse ID (mitte rakenduse instrumentation võti).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Näide 2

Mul on rakendus, mille kasutada [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) teatada mõõdiku nimega "salesPerHour." Saada e-posti kolleegidele, kui "salesPerHour" langeb alla 100, mis on 24 tunni jooksul.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Mõõdiku teatatud teise jälitus kõne nagu TrackEvent või trackPageView [mõõtühikute parameetri](app-insights-api-custom-events-metrics.md#properties) abil saab kasutada sama reegel.

## <a name="metric-names"></a>Argumendil nimed

Argumendil nimi | Kuva nimi | Kirjeldus
---|---|---
`basicExceptionBrowser.count`|Brauseri erandid|Count tabamatu erandite visati brauseris.
`basicExceptionServer.count`|Serveri erandid|Count töötlemata erandi visanud rakendus
`clientPerformance.clientProcess.value`|Kliendi töötlemise ajal|Dokumendi viimase bait vastuvõtmine kuni DOM on laaditud vahelise aja. Asünkroonse taotlusi siiski võib töötlemine.
`clientPerformance.networkConnection.value`|Ühenduse loomine laadimise võrgu ajal| Brauser suunab võrguga aeg. Kui vahemällu võib olla 0.
`clientPerformance.receiveRequest.value`|Vastuvõtmise aega| Kellaaeg brauseri kutse saatmist hakkamist vastuse vahel.
`clientPerformance.sendRequest.value`|Saatke kutse aeg| Aega, mida brauseris kutse saatmiseks.
`clientPerformance.total.value`|Brauseri laadimise ajal|Kasutaja taotlus kuni DOM, laadilehte, skripte ja pildid laaditakse aeg.
`performanceCounter.available_bytes.value`|Saadaoleva mäluga|Füüsilise mälu protsessi või süsteemi jaoks kohe saadaval.
`performanceCounter.io_data_bytes_per_sec.value`|Protsessi IO määr|Kokku baiti sekundis lugeda ja kirjutada faile, võrgu ja seadmed.
`performanceCounter.number_of_exceps_thrown_per_sec`|erandi määr|Visati sekundis erandid.
`performanceCounter.percentage_processor_time.value`|Protsessi CPU|Möödunud aeg kõik protsessi Teemad juhiste täitmise protsessori jaoks kasutamiseks rakenduste protsessi protsent.
`performanceCounter.percentage_processor_total.value`|Protsessori aeg|Aeg, mille protsessor veedab-jõude Teemad protsent.
`performanceCounter.process_private_bytes.value`|Protsessi privaatne baiti|Ainult määratud jälgida protsesse mälu.
`performanceCounter.request_execution_time.value`|ASP.net-i taotluse täitmisaeg|Viimati tehtud taotluse täitmisaeg.
`performanceCounter.requests_in_application_queue.value`|ASP.net-i taotlused täitmise järjekorras|Rakenduse taotluse järjekorda pikkus.
`performanceCounter.requests_per_sec`|ASP.net-i taotluse määr|Kõigi rakenduste sekundis ASP.net-i taotluste määr.
`remoteDependencyFailed.durationMetric.count`|Sõltuvus tõrked|Arv on nurjunud kõned tehtud server rakenduse välised ressursid.
`request.duration`|Serveri vastuse aeg|HTTP-taotluse vastu ja viimistlus vastuse saatmist vahelise aja.
`request.rate`|Taotluse määr|Kõigi rakenduste sekundis taotluste määr.
`requestFailed.count`|Nurjunud taotlusi|Count, HTTP päringuid, mille tulemuseks vastuse koodi > = 400
`view.count`|Lehe vaated|Kliendi kasutaja taotluste veebilehe arv. Sünteetiline liikluse on välja filtreeritud.
{oma kohandatud argumendil nimi}|{Argumendil nimi}|Teie argumendil väärtus teatatud [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) või [mõõtmed parameeter kõne jälgimine](app-insights-api-custom-events-metrics.md#properties).

Erinevate telemeetria moodulid saadetakse mõõdikud:

Argumendil rühma | Koguja mooduli
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>Vaade | [Brauseri JavaScripti](app-insights-javascript.md)
performanceCounter | [Jõudluse](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Sõltuvus](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
Klõpsake koosolekukutses<br/>requestFailed|[Serveri taotlus](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Saate [oma vastuse teatise automatiseerida](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure'i helistab teie valitud veebiaadress, kui tõstetakse teatise.

## <a name="see-also"></a>Vt ka


* [Skripti konfigureerida rakenduse ülevaated](app-insights-powershell-script-create-resource.md)
* [Saate luua rakenduse ülevaated ja veebiressursid test](app-insights-powershell.md)
* [Microsoft Azure'i diagnostika ühendamise rakenduse ülevaate saamiseks automatiseerimine](app-insights-powershell-azure-diagnostics.md)
* [Oma vastuse teatise automatiseerimine](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
