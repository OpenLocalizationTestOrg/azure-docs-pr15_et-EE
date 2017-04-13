<properties
    pageTitle="Automaatse skaleerimist ja virtuaalse masina mastaapimiseks komplektid | Microsoft Azure'i"
    description="Lugege teemat diagnostika- ja autoscale ressursid automaatselt mastaapida virtuaalmasinates kogumi skaala."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automaatse skaleerimist ja virtuaalse masina mastaapimiseks komplektid

Automaatset mastaapimist, virtuaalmasinates skaala kogumis on loomist või kustutamist masinad määramine vastavalt vajadusele jõudluse nõuetele vastavus. Töö kasvab, rakendus võib olla vaja tõhusalt teha toiminguid selleks täiendavaid ressursse.

Automaatse skaleerimist on automatiseeritud protsessi, et aitab lihtsustada haldus pea kohal. Vähendades kohal, pole vaja pidevalt jälgimine süsteemi jõudluse või otsustada, kuidas hallata ressursid. Skaleerimist on elastne protsess. Veel ressursse saab lisada kui laadi suureneb, kuid nimega vajadusel saab eemaldada väheneb ressursside kulude minimeerimine ja säilitada jõudluse taseme.

Häälestage automaatne skaleerimist skaalal mõni Azure ressursihaldur Mall, Azure PowerShelli, Azure'i CLI või Azure portaali abil.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Häälestamine skaleerimist teguriga ressursihaldur mallide kasutamine

Selle asemel juurutamine ja hallata eraldi iga ressursi rakenduse, kasutada malli, mis kasutab ühe koordineeritud kasutusel kõik ressursid. Malli, rakenduse ressursid on määratletud ja juurutamise parameetrid on määratud viibite. Mall sisaldab JSON ja avaldised, mille abil saate koostada oma juurutamiseks väärtused. Lisateabe saamiseks vaadake [loome Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md).

Määrake malli, võimsuse elementi:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Võimsus tuvastab virtuaalmasinates määramine arv. Saate muuta käsitsi võimsus juurutamine malli teine väärtus. Kui juurutate muuta ainult võimsus malli, saate lisada ainult SKU elemendi värskendatud mahuga.

Automaatselt muuta oma skaala autoscaleSettings ressurss ja diagnostika laiend kombinatsiooni abil võimsus.

### <a name="configure-the-azure-diagnostics-extension"></a>Azure'i diagnostika laiend konfigureerimine

Automaatse skaleerimist saab teha ainult kui mõõdikute saidikogumi õnnestus iga virtual arvutisse skaala määramine. Azure'i diagnostika laiend pakub diagnostika- ja võimaluste mõõdikute saidikogumi vajadustele autoscale ressursi. Saate installida laiendamine ressursihaldur malli osana.

Selles näites muutujate, mida kasutatakse konfigureerida diagnostika laiend mall:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parameetrite on saadaval, kui mall on juurutatud. Selles näites on olemas andmete talletuskoht salvestusruumi konto nimi ja nime ulatuse määramine koguda andmeid. Selles näites Windows Server kogutakse ka ainult lõime Count jõudluse näidiku. Saadaval jõudluse hinnale Windowsi või Linuxi saab kasutada diagnostika teabe kogumiseks ja saab lisada laiend konfigureerimine.

Selles näites määratluse laiendamine mall:

    "extensionProfile": {
      "extensions": [
        {
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
              "storageAccount": "[variables('diagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
              "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Diagnostika laiend käitamisel kogutakse andmed tabelisse, mis asub teie määratud salvestusruumi konto. Tabelis WADPerformanceCounters leiate kogutud andmete:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>AutoScaleSettings ressursikeskuse konfigureerimine

Ressursi autoscaleSettings kasutab seda teavet diagnostika pikendamist otsustada, kas suurendamiseks või vähendamiseks virtuaalmasinates skaala määramine arv.

Selles näites on kujutatud ressursi konfiguratsiooni malli:

    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

Ülaltoodud näites luuakse kaks reeglid skaleerimise Automaattoimingute määratlemine. Esimese reegli määratleb skaala-out toimingu ja teise reegli määratleb skaala toiming. Need väärtused on toodud reegleid:

- **metricName** – see väärtus on sama, mis on määratletud wadperfcounter muutuja pikendamise diagnostika jõudluse näidiku. Ülaltoodud näites kasutatakse lõime Count näidiku.  
- **metricResourceUri** – see väärtus on ressursiidentifikaator virtuaalse masina skaala komplekti. See identifikaator sisaldab ressursirühma nimi, ressursi pakkuja nimi ja nime ulatuse määramine skaala.
- **timeGrain** – väärtuseks granulaarsus mõõdikute, mis on kogunud. Eelmises näites on ühe minuti kogutud andmed. Seda väärtust kasutatakse koos timeWindow.
- **statistiline** – see väärtus määratleb, kuidas mõõdikud on ühendatud mahutamiseks automaatselt skaleerimise toiming. Võimalikud väärtused on: Keskmine, Min ja Max.
- **timeWindow** – see väärtus on lahtrivahemik, kus on eksemplari andmete kogumise aja. Peab olema 5 minutit kuni 12 tundi.
- **timeAggregation** – see väärtus määratleb, kuidas peaks kogutud andmete kombineerida aja jooksul. Vaikeväärtus on keskmine. Võimalikud väärtused on: Keskmine, miinimum, maksimum, viimase, kokku, loendus.
- **korraldaja** – väärtuseks võrdlemiseks argumendil andmed ja piirmäär kasutatav tehtemärk. Võimalikud väärtused on: võrdub, NotEquals ületas, GreaterThanOrEqual, vähem kui, LessThanOrEqual.
- **lävi** – see väärtus on väärtus, mis käivitab toimingu skaala. Kindlasti pakuvad piisavaid vahe lävi skaala-out toimingu ja skaala toimingu piirmäär. Kui seate sama väärtused, prognoosib süsteemi pidev muutmine, mis takistab skaleerimise tegevuse rakendamiseks. Näiteks säte nii 600 Teemad eelmises näites, ei tööta.
- **suunas** – see väärtus määratleb toiming, mille võetaks lävi väärtuse saavutamiseks. Võimalikud väärtused on suurendada või vähendada.
- **Tippige** – väärtuseks toimingut, mis peaks toimuma ja peab olema seatud ChangeCount tüüp.
- **väärtuse** – selle väärtuseks on arv virtuaalmasinates, mis on lisatud või sealt skaala määramine. Väärtus peab olema 1 või suurem.
- **cooldown** – see väärtus on pärast viimast toimingut skaleerimise ootama, enne järgmise toimingu toimumise aeg. Väärtus peab olema üks minut ja üks nädal.

Sõltuvalt Jõudluseloenduri kasutate, mõned elemendid malli konfiguratsiooni kasutatakse erinevalt. Eelnevast näitest jõudluse näidiku on lõime Count, lävi väärtus on 650 skaala-out toimingu ja lävi väärtus on 550 skaala toimingute jaoks. Kui kasutate näiteks % protsessori aja vastuolus, lävi väärtuseks on seatud protsent CPU hõivatus, mis määratleb skaleerimise toiming.

Koormus loomisel virtuaalmasinates, mis käivitab skaleerimise toimingu määramine võimsus suurendatakse malli väärtuse. Näiteks skaalal kogum, kus on seatud võimsus 3 ja skaala toimingu väärtuse väärtuseks 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Laadi loomise Keskmine jutulõnga count 650 läve minna põhjustav:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Skaala-out toiming käivitatakse määramine võimsus suurendatakse ühte välja käivitava:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

Ja virtuaalse masina lisatakse skaala määramine:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Pärast cooldown 5 minuti jooksul, kui Teemad masinad keskmine arv jääb 600, mõnes muus arvutis on lisatud määramine. Kui Keskmine jutulõnga count jääb all 550, skaala määramine võimsus on vähendatud üks ja masina eemaldatakse määramine.

## <a name="set-up-scaling-using-azure-powershell"></a>Häälestamine skaleerimist Azure PowerShelli abil

Näiteid PowerShelli abil häälestada autoscaling vaatamiseks vaadake [Azure kuvari PowerShelli Kiire alustamine näidised](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Häälestamine skaleerimist Azure'i CLI abil

Näiteid Azure'i CLI abil häälestada autoscaling vaatamiseks vaadake [Azure'i kuvari platvormidel CLI Kiire alustamine näidised](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Azure'i portaalis skaleerimist häälestamine

Näide Azure portaali abil häälestada autoscaling vaatamiseks vaadake [virtuaalse masina skaala seadmine loomine abil Azure portaali](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Uurige skaleerimise toimingud

- [Azure'i portaalis]() – saate praegu piiratud hulgal teavet portaalis.
- [Azure'i Resource Explorer]() – see tööriist on parim oma skaala määramine hetkeseisu uurimine. Järgige selle tee ja näete eksemplari vaate loodud ulatuse määramine: tellimuste > {tellimuse} > resourceGroups > {ressursirühma} > pakkuja > Microsoft.Compute > virtualMachineScaleSets > {skaala määramine} > virtualMachines
- Azure'i PowerShelli – kasutage see käsk saada teavet:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Ühenduse jumpbox virtuaalse masina täpselt samamoodi, nagu teeksite mis tahes muu seadme ja siis pääsete juurde kaugühenduse teel virtuaalmasinates Määrake üksikute protsesside jälgimiseks skaala.

## <a name="next-steps"></a>Järgmised sammud

- Vaadata soovite näha, kuidas luua seadistatud automaatset mastaapimist konfigureeritud skaala näiteks [automaatselt mastaapida masinad virtuaalse masina skaala määramine](virtual-machine-scale-sets-windows-autoscale.md) .
- Azure'i kuvar jälgimise [Azure kuvari PowerShelli Kiire alustamine näidised](../monitoring-and-diagnostics/insights-powershell-samples.md) funktsioonide näiteid
- Lisateavet teatis funktsioone [kasutada saatmine e-posti ja webhook teatiste Azure'i monitoris autoscale toiminguid](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Kuidas [saata e-posti ja webhook teatiste Azure'i monitoris auditilogid kasutamine](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Lugege [Täpsemalt autoscale stsenaariumid](./virtual-machine-scale-sets-advanced-autoscale.md).
