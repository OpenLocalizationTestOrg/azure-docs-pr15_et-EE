<properties
    pageTitle="PowerShelli kasutamine Azure teenuste teatiste loomine | Microsoft Azure'i"
    description="PowerShelli abil saate luua Azure teatised, mis käivitab teatisi või automatiseerimise, kui teie määratud tingimustele."
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

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>PowerShelli kasutamine Azure teenuste teatiste loomine

> [AZURE.SELECTOR]
- [Portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas luua PowerShelli kaudu Azure'i teatised.  

Saate teatise jälgimisega seotud mõõdikud või sündmuste oma Azure'i teenuste kohta.

- **Argumendil väärtused** - teatis päästikute, kui määratud mõõtühiku väärtus ületab läve määrate suunda. Mis on see käivitab nii kui tingimus on täidetud esmalt ja seejärel hiljem kui see tingimus on ei ole enam täidetud.    
- **Tegevuste ja sündmuste** - teatise käivitab *iga* sündmuse, või ainult siis, kui sündmuste arv ilmneda.

Saate konfigureerida tehke järgmist, kui see käivitab teatis.

- Meiliteatiste saatmine teenuseadministraator ja kaasadministraatorite
- täiendavad meilisõnumid teie määratud e-posti saata.
- kõne on webhook
- Käivitage täitmine on Azure käitusjuhendi (ainult portaalist Azure).

Saate konfigureerida ja teatiste reeglite kasutamise kohta teabe saamine

- [Azure'i portaal](insights-alerts-portal.md)
- [PowerShelli](insights-alerts-powershell.md)
- [käsurea liides (CLI)](insights-alerts-command-line-interface.md)
- [Azure'i kuvari REST API-ga](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Lisateabe saamiseks võite alati tippida ```get-help``` ja seejärel soovitud kohta abi PowerShelli käsk.

## <a name="create-alert-rules-in-powershell"></a>PowerShelli teatiste reeglite loomine

1. Azure'i sisse logida.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Loendi tellimustest teil on saadaval. Veenduge, et töötate õige tellimus. Kui pole, määrake õige abil väljund `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Loendi kehtivad ressursirühma, kasutage järgmist käsku:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Reegli loomiseks peate on mitu olulisemad teabe esmalt. 
    - Ressursi **Ressursi ID -d** , mida soovite teatise seadistamine
    - **Argumendil määratlused** selle ressursi jaoks saadaval

    Üks võimalus ressursi ID-d on kasutada Azure portaali. Eeldades, et ressurss on juba loonud, valige see portaalis. Järgmise tera, valige *Atribuudid* jaotises *sätted* . RESSURSI ID-d on järgmine tera välja. Teine võimalus on kasutada [Azure ressurssi Explorer](https://resources.azure.com/).

    Näide ressursi id web app on

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Saate kasutada `Get-AzureRmMetricDefinition` argumendil määratlused teatud ressursi loendi kuvamiseks.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Järgmine näide loob tabel koos meetermõõdustik nimi ja selle väärtuseks meetermõõdustik üksus.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Get-MetricDefinitions käitades on saadaval jaoks Get-AzureRmMetricDefinition täieliku loendi.


5. Järgmises näites luuakse teatise veebisait ressursi. Teatiste päästikute, kui ta saab pidevalt mis tahes liikluse 5 minutit ja uuesti, kui ta saab pole liikluse 5 minutit.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Luua webhook või saata meilisõnumeid, kui teatise käivitab, esmalt luua e-posti ja/või webhooks. Seejärel kohe luua reegli hiljem sildiga - toimingud ja nagu on näidatud järgmises näites. Te ei saa seostada webhook või e-kirju juba loonud PowerShelli kaudu.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Luua teatise, mis käivitab tegevuse Logi teatud tingimus, kasutage käske järgmisel kujul.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    -OperationName vastab sündmuste logi tegevuse tüüp. Näiteks *Microsoft.Compute/virtualMachines/delete* ja *microsoft.insights/diagnosticSettings/write*.

    PowerShelli käsku [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) abil võimalik operationNames loendi. Vaheldumisi, saate Azure portaali tegevuse Logi päringu ja leida kindla eelmiste toimingute kohta, mida soovite teatise luua. Pildi log vaate sõbralik nimede toimingud. Kirje JSON otsida ja tõmmake OperationName väärtus.   

8. Veenduge, et teatiste on loodud õigesti vaadates üksikuid reegleid.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Kustuta teatised. Need käsud kustutage reeglid, mis on selles artiklis varem loodud.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Järgmised sammud

* [Ülevaate Azure jälgimine,](monitoring-overview.md) sh tüüpi teavet saate koguda ja jälgida.
* Lisateavet [teatiste seadistamine webhooks](insights-webhooks-alerts.md).
* Lisateave [Azure'i automaatika tegevusraamatud](..\automation\automation-starting-a-runbook.md).
* Siit saate [ülevaate kogumiseks diagnostikalogid](monitoring-overview-of-diagnostic-logs.md) koguda üksikasjalikku suure sagedusega mõõdikute teenust.
* Siit saate [ülevaate mõõdikute saidikogumi](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
