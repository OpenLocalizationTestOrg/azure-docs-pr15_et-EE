<properties
    pageTitle="Vertikaalselt mastaapimiseks Azure virtuaalse masina skaala komplektid | Microsoft Azure'i"
    description="Kuidas vertikaalselt mastaapimiseks virtuaalse masina vastuseks jälgimine teatiste Azure automatiseerimine"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Vertikaalne autoscale virtuaalse masina skaala määrab

Selles artiklis kirjeldatakse, kuidas vertikaalselt mastaapimiseks Azure [Virtuaalse masina skaala komplektid](https://azure.microsoft.com/services/virtual-machine-scale-sets/) või ilma reprovisioning. Vertikaalne skaleerimist VMs, mis ei ole skaala kogumitena, vaadake [vertikaalselt mastaapimiseks Azure virtuaalse masina Azure automatiseerimine](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Vertikaalse suuruse muutmise, nimetatakse ka _skaala_ ja _mastaapimiseks_, tähendab, et suureneb või väheneb virtuaalse masina (VM) suurused on töökoormus vastuseks. Võrrelda seda [Horisontaalne mastaapimine](./virtual-machine-scale-sets-autoscale-overview.md)ka edaspidi _välja skaala_ ja _skaala_, kus VMs arv on muudetud sõltuvalt töökoormus.

Reprovisioning tähendab, et mõne olemasoleva VM eemaldada ja asendada see uuega. Kui suurendada või vähendada VMs VM skaala määramine, mõnel juhul soovite olemasoleva VMs suuruse ja andmete säilitamise ajal muudel juhtudel peate juurutada uue VMs uue suuruse. Selle dokumendi hõlmab mõlemal juhul.

Vertikaalne skaleerimist võib olla kasulik järgmistel juhtudel:

- Tugineb virtuaalmasinates teenus on jaotises kasutada (nt nädalavahetustel). VM mahu vähendamiseks saate vähendada igakuised kulud.
- Toime tulla suuremat nõudmisel loomata täiendavaid VMs VM suuruse suurendamine.

Saate häälestada vertikaalne skaleerimist olevat käivitatud põhineb argumendil vastavalt teatiste VM skaala määramine. Kui teatise aktiveeritakse see käivitub on webhook mis käivitab käitusjuhendi, kus saate oma skaala mastaapimiseks seada üles või alla. Vertikaalne skaleerimist saab konfigureerida järgmist:

1. Looge konto Azure automatiseerimine Käivita nimega võimalusega.
2. Azure'i automaatika vertikaalne skaala tegevusraamatud importida VM skaala komplektide tellimuse.
3. Saate lisada oma käitusjuhendi on webhook.
4. Teatise lisada oma VM skaala Kasuta hulga webhook teatis.

> [AZURE.NOTE] Vertikaalne autoscaling toimuda ainult teatud piirides VM suuruses. Võrdlus kirjeldused iga suurus enne ühest teise mastaapimiseks (suurem arv ei tähenda alati suuremaid VM). Saate valida mastaapimiseks suuruses järgmised paari vahele.

>| VM mahud skaleerimise sidumiseks |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Looge konto Azure automatiseerimine Käivita nimega võimalusega

Kõigepealt peate tegema on tegevusraamatud, kasutatud skaala VM skaala seadmine eksemplarid majutavad Azure automatiseerimine konto loomiseks. Hiljuti kasutusele [Azure automatiseerimine](https://azure.microsoft.com/services/automation/) "Run kontona" funktsioon, mis muudab teenuse põhilise üles säte töötab automaatselt selle tegevusraamatud, kasutaja nimel on väga lihtne. Lugege selle artikli alla kohta:

* [Autentida tegevusraamatud Azure'i käivitada nimega kontoga](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Teie tellimus Azure automatiseerimine vertikaalne skaala tegevusraamatud importimine

Tegevusraamatud, vaja vertikaalselt mastaapimiseks oma VM skaala komplektid on juba avaldatud Azure automatiseerimine Käitusjuhendi Galerii. Kui soovite importige need oma tellimuse järgige selles artiklis toodud juhiseid.

* [Azure'i automaatika Käitusjuhendi ja mooduli galeriid](../automation/automation-runbook-gallery.md)

Valige suvand Sirvi Galerii tegevusraamatud käsk:

![Imporditava tegevusraamatud][runbooks]

Kuvatakse tegevusraamatud, mida soovite importida. Valige vastavalt kas soovite vertikaalne skaleerimist koos või ilma reprovisioning käitusjuhendi.

![Tegevusraamatud Galerii][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Teie käitusjuhendi on webhook lisamine

Kui olete importinud tegevusraamatud, tuleb teil lisada mõne webhook käitusjuhendi nii, et seda saab käivitada teatise kaudu VM skaala määramine. Selles artiklis kirjeldatud üksikasjade lisamine webhook loomise oma Käitusjuhendi:

* [Azure'i automaatika webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Veenduge, et saate kopeerida webhook URI enne sulgemist webhook dialoogiboksi, peate selle järgmises jaotises.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Teatise lisamine VM skaala määramine

Allpool on PowerShelli skripti, mis näitab, kuidas lisada teatise VM ulatuse määramiseks. Järgmises artiklis meetermõõdustik tule klõpsake teatise nime hankimine viidata: [Azure'i kuvari autoscaling levinud mõõdikute](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Konfigureerida mõistlikum ajaakna teatise käivitamise vertikaalne suurust ja seotud teenuse peatamine, sageli vältimiseks on soovitatav. Kaaluge võimalust akna 20 – 30 minuti jooksul või rohkem. Kaaluge võimalust horisontaalne mastaapimist, kui teil on vaja katkemise vältimiseks.

Teatiste loomise kohta lisateabe saamiseks vaadake järgmisi artikleid:

* [Azure'i kuvari PowerShelli Lühijuhend näidised](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure'i kuvari platvormidel CLI Kiire alustamine näidised](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Kokkuvõte

Selles artiklis ilmnes lihtsad vertikaalne skaleerimise näited. Järgmiste koosteüksuste - automatiseerimist konto, tegevusraamatud, webhooks, teatiste – saate luua ühenduse rikkaliku erinevaid sündmused ja kohandatud toimingute kogum.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
