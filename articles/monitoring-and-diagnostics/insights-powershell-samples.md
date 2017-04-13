<properties
    pageTitle="Azure'i kuvari PowerShelli Lühijuhend näidised. | Microsoft Azure'i"
    description="PowerShelli kasutamine Azure kuvari funktsioonide nagu autoscale, teatiste, webhooks ja otsimise logid."
    authors="kamathashwin"
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure'i kuvari PowerShelli Lühijuhend näited

Selles artiklis näidatakse te Kuulake PowerShelli käske aitavad Azure kuvari funktsioone. Azure'i jälgimine võimaldab teil AutoScale pilveteenustega, Virtuaalmasinates ja veebirakenduste ja teatiste saatmine või kõne web URL-ide väärtuste konfigureeritud telemeetria andmete põhjal.

>[AZURE.NOTE] Azure'i kuvar on kuni 25 Sept 2016 mis kutsuti "Azure ülevaateid" uus nimi. Siiski soovitud nimeruumid ja seega allpool käsud sisaldavad siiski "ülevaateid".

## <a name="set-up-powershell"></a>PowerShelli häälestamine
Kui te pole seda veel teinud, seadistamiseks PowerShelli teie arvutis. Lisateavet leiate teemast [PowerShelli installimine ja konfigureerimine](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Selle artikli näited

Selle artikli näited illustreerivad Azure'i kuvari cmdlettide kasutamise. Saate vaadata ka kogu loendit Azure kuvari PowerShelli cmdletid veebisaidil [Azure'i kuvari (ülevaateid) cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).


## <a name="sign-in-and-use-subscriptions"></a>Logige sisse ja kasutage tellimused

Azure'i tellimuse esmalt sisse logida.

```PowerShell
Login-AzureRmAccount
```

Selleks saate sisse logida. Kui te ei tee, oma konto kuvatakse TenantId ja vaikimisi tellimuse Id. Kõik Azure cmdlettide töötavad vaikimisi tellimuse kontekstis. Teil on juurdepääs tellimuste loendi vaatamiseks käsu.

```PowerShell
Get-AzureRmSubscription
```

Saate muuta oma töö kontekstis eri tellimusse järgmine käsk.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Auditilogide tellimuse tuua
Kasutage funktsiooni `Get-AzureRmLog` cmdlet-käsk.  Allpool on mõned levinud näited.

Kannete toomine selle kellaaeg/kuupäev esitamiseks:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Saada vahel kuupäev/kellaaeg valikut Logi kirjed:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Mõne kindla ressursirühma saamiseks Logi kirjed.

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Saamiseks teatud ressursi pakkuja vahel kuupäeva/kellaaja valikut Logi kirjed.

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Saada kõik logi kannete teatud helistaja:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Auditilogi toob viimase 1000 sündmuste järgmine käsk:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`toetab palju muid parameetreid. Lugege teemat funktsiooni `Get-AzureRmLog` Lisateavet viide.

>[AZURE.NOTE] `Get-AzureRmLog`ainult pakub 15 päeva ajalugu. **-MaxEvents** parameetriga saate päringu viimase N sündmuste 15 päeva jooksul. Kasutage Accessi sündmuste üle 15 päeva, REST API-ga või SDK (C# valimi SDK abil). Kui kaasate **StartTime**, siis vaikeväärtust, milleks on **EndTime** miinus üks tund. Kui kaasate **EndTime**, vaikeväärtus on praeguse kellaaja. Kõik ajad on UTC-vormingus.

## <a name="retrieve-alerts-history"></a>Saate tuua teatiste ajalugu
Kõigi teatiste sündmuste vaatamiseks saate teha päringuid Azure'i ressursihaldur logid abil järgmised näited.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Teatiste kindla reegli ajaloo kuvamiseks saate kasutada funktsiooni `Get-AzureRmAlertHistory` cmdlet-käsk, läbides reegli ressursi ID-d.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Funktsiooni `Get-AzureRmAlertHistory` cmdlet-käsk toetab parameetritele. Lisateavet leiate teemast [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Teavet teatiste reeglid tuua
Kõik järgmised käsud toimivad nimega "montest" ressursirühma.

Reegli atribuutide vaatamiseks tehke järgmist.

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Saate tuua kõik teatised ressursirühma:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Saate tuua kõigi teatiste reeglite seadmine target ressursi. Näiteks kõigi teatiste reegleid seada VM.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`toetab muud parameetrid. Lisateabe saamiseks vaadake [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Teatiste reeglite loomine
Saate kasutada funktsiooni `Add-AlertRule` cmdlet luua, värskendada või reegli keelamine.

Saate luua e-posti ja webhook atribuudid abil `New-AzureRmAlertRuleEmail` ja `New-AzureRmAlertRuleWebhook`vastavalt. Reegli cmdlet-käsk määrata neid toiminguid reegli atribuudile **toimingud** .

Järgmine jaotis sisaldab näide, mis näitab, kuidas luua reegli parameetritele.

### <a name="alert-rule-on-a-metric"></a>Reegli sisse mõõdiku
Järgmises tabelis kirjeldatakse parameetrite ja mõõdiku abil teatise loomiseks kasutatud väärtused.


|parameetri|väärtus|
|---|---|
|Nimi|  simpletestdiskwrite|
|Selle reegli asukoht|   Ida-USA|
|ResourceGroup| Montest|
|TargetResourceId|  /subscriptions/S1/resourceGroups/Montest/Providers/Microsoft.compute/virtualMachines/testconfig|
|MetricName teatise, mis on loodud|   \PhysicalDisk (_Total) \Disk kirjutab sekundis. Lugege teemat funktsiooni `Get-MetricDefinitions` all cmdleti kohta, kuidas tuua täpse argumendil nimed|
|tehtemärk|  Ületas|
|Piirmäärast (arv sekundis sisse selle mõõdiku)|    1|
|WindowSize (hh:mm:ss vorming)|  00:05:00|
|lugeja (meetermõõdustik, mis kasutab keskmine arv, kui statistiline)|  Keskmine|
|kohandatud e-kirju (stringi massiiv)|'foo@example.com','bar@example.com'|
|Saada e-posti omanikud, kaasautorid ja lugejad|    -SendToServiceOwners|

E-posti toimingu loomine

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Webhook toimingu loomine

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

CPU % meetermõõdustik klassikaline VM reegli loomine

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Reegli tuua

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Lisa teatis cmdlet-käsk värskendab ka reegel reegli juba olemas antud atribuudid. Reegli kustutamiseks, sisaldavad parameetri **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Auditilogi logisündmuse Hoiata

>[AZURE.NOTE] See funktsioon on siiski eelvaade.

Selle stsenaariumi korral kuvatakse saatke meilisõnum, kui veebisait on edukalt alustatud tellimuse ressursi jaotises *abhingrgtest123*.

Reegli e-posti häälestamine

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Webhook reegli häälestamine

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Klõpsake sündmust reegli loomine

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Reegli tuua

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

Funktsiooni `Add-AlertRule` cmdlet lubab muude parameetritele. Lisateavet leiate teemast [Lisa-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Teatiste jaoks saadaval mõõdikute loendi hankimine
Saate kasutada funktsiooni `Get-AzureRmMetricDefinition` cmdlet-käsk kõik mõõdikud teatud ressursi loendi kuvamiseks.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Järgmises näites luuakse tabel koos meetermõõdustik nimi ja selle üksuse.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Täieliku loendi jaoks `Get-AzureRmMetricDefinition` on saadaval veebisaidil [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Saate luua ja hallata AutoScale sätted
Ressursi, nt Web Appi VM, pilveteenuses või VM skaala seadmine võib olla ainult üks autoscale säte, mis on konfigureeritud.
Siiski saate iga autoscale säte on mitu profiili. Näiteks üks põhinevad skaala profiili ja teine ajakava jaoks vastavalt profiili. Iga profiili võib olla mitu reeglid, mis on konfigureeritud. Autoscale kohta leiate lisateavet teemast [Autoscale rakendus](../cloud-services/cloud-services-how-to-scale.md).

Siin on toimingud, kasutame.

1. Saate luua reeglid.
2. Saate luua profiili ja profiilid vastendamise varem loodud reeglid.
3. Valikuline: Looge teatiste autoscale atribuudid webhook ja e-posti konfigureerimise abil.
4. Luua mõne sätte autoscale, vastendades profiilid ja teatised, et eelmiste juhiste järgi loodud ressurss sihtfail nimega.

Järgmised näited kujutavad, kuidas saate luua mõne Autoscale sätte seadmine Windowsi operatsioonisüsteemi põhjal, kasutades CPU kasutamise mõõdiku VM skaala.

Esmalt looge reegel skaala-out, näiteks arvu suurendamine.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Järgmiseks luua reegli, et skaala sisse, kuvatakse eksemplari count vähenda.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Looge reeglid profiili.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Webhook atribuudi loomine.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Looge autoscale sätte, sh e-posti ja varem loodud webhook atribuudi teatis.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Lõpuks luua autoscale säte eespool loodud profiili lisamiseks.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Autoscale sätete haldamise kohta leiate lisateavet teemast [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Autoscale ajalugu
Järgmises näites kirjeldatakse, kuidas saate vaadata tehtud autoscale ja teatiste sündmused. Auditilogi log otsingu abil saate vaadata Autoscale ajalugu.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Saate kasutada funktsiooni `Get-AzureRmAutoScaleHistory` cmdlet hankida AutoScale ajalugu.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Lisateavet leiate teemast [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Mõne sätte autoscale andmete kuvamine
Saate kasutada funktsiooni `Get-Autoscalesetting` cmdlet hankida Lisateavet autoscale säte.

Järgmises näites kuvatakse ressursi jaotises "myrg1 kõigi autoscale sätete üksikasjad.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Järgmises näites on kujutatud kõik autoscale sätete ressursi rühma 'myrg1 ja spetsiaalselt autoscale säte nimega "MyScaleVMSSSetting" üksikasjad.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Mõne sätte autoscale eemaldamine
Saate kasutada funktsiooni `Remove-Autoscalesetting` cmdlet-käsu kustutamine on autoscale säte.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Hallata profiilid auditilogid jaoks

Saate luua *Logi profiili* ja oma auditilogid andmete eksportimine salvestusruumi konto ja saate konfigureerida andmete säilitamise seda. Soovi korral saate ka voona andmeid oma sündmuse jaoturi. Pange tähele, et see funktsioon on praegu eelvaates ja te saate luua ainult ühe Logi profiili tellimuse kohta. Saate luua ja hallata profiilid saate kasutada koos oma praeguse tellimuse järgmised cmdlet-käsud. Soovi korral saate ka teatud tellimuste. Kuigi PowerShelli on vaikimisi praegune tellimus, saate muuta selle abil `Set-AzureRmContext`. Saate konfigureerida auditilogid marsruutimiseks andmetega salvestusruumi konto või sündmuse jaoturi tellimuse sees. Andmed on kirjutatud bloobimälu failidena JSON-vormingus.

### <a name="get-a-log-profile"></a>Logi profiili hankimine
Olemasoleva Logi profiilid toomiseks, kasutage funktsiooni `Get-AzureRmLogProfile` cmdlet-käsk.

### <a name="add-a-log-profile-without-data-retention"></a>Andmete säilitamine ilma Logi profiili lisamine

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Logi profiili eemaldamine

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Logi profiili andmete säilitamise ja lisamine

Saate määrata atribuudi **- RetentionInDays** arvu päevade on positiivne täisarv, kus andmed säilivad.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Säilitus- ja EventHub koos lisamine
Lisaks andmete marsruutimine salvestusruumi kontole, ka voogesituseks selle sündmuse jaoturiga. Pange tähele, et seda eelvaateversiooni ja talletamist konto konfigureerimine on kohustuslik, kuid sündmuse jaoturi konfiguratsiooni pole kohustuslik.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigureerida diagnostika logid
Palju Azure teenuseid pakkuda täiendavad logid ja telemeetria, sh Azure võrgu turberühmad, tarkvara koormus soolise, võti Vault Azure'i otsing teenuseid, ja loogika rakendused ja ta saab konfigureerida Azure Storage konto andmed salvestada. Seda toimingut saab teha ainult ressursi tasemel ja salvestusruumi konto peaks olema sama piirkonna sihtkoht ressursi kus diagnostika säte on konfigureeritud Esita.

### <a name="get-diagnostic-setting"></a>Saada diagnostika seade

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnostika seade keelamine

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Diagnostika ilma säilitus sätet lubamine

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Diagnostika säilitus sätet lubamine

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Diagnostika säilituspoliitika teatud log kategooria, milles lubamine

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
