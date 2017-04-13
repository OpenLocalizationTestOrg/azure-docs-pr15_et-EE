<properties
   pageTitle="Logide kogumise Azure'i diagnostika abil | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas häälestada Azure'i diagnostika logid kogumine teenuse struktuuri kobar, Azure töötab."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Logide kogumise Azure'i diagnostika abil

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Kui teil on mõni Azure teenuse struktuuri kobar, on mõistlik koguda logid kõik sõlmed ühes keskses kohas. Heliprobleemide logid ühes keskses kohas aitab teil analüüsida ja klaster probleemid või rakenduste ja teenuste töötab selle kobar probleemide tõrkeotsing.

Üks võimalus üles laadida ja kogumise logid on lisatud logid Azure Storage Azure'i diagnostika laienduse kasutada. Logide ei ole see kasulik otse salvestusruumi. Kuid mõni väline protsess abil saate lugeda sündmuste salvestusruumi ja paigutada need toode nagu [Log Analytics](../log-analytics/log-analytics-service-fabric.md), [Elastne otsingu](service-fabric-diagnostic-how-to-use-elasticsearch.md)või log sõelumine muu lahendus.

## <a name="prerequisites"></a>Eeltingimused
Nende tööriistade abil saate teha mõned toimingud selles dokumendis:

* [Azure'i diagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure'i pilveteenustega seotud, kuid on hea ja näidetega)
* [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md)
* [Azure'i PowerShelli](../powershell-install-configure.md)
* [Azure'i ressursihaldur klient](https://github.com/projectkudu/ARMClient)
* [Azure'i ressursihaldur Mall](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Logi allikad, mida võiksite kogumise
- **Teenuse struktuuri logid**: nõrgunud platvormi standard sündmuse jälgimine for Windows (ETW) ja EventSource kanaleid. Logide võib olla üks mitut tüüpi:
  - Funktsionaalseid sündmused: logid toiminguid, mis teeb teenuse struktuuri platvormi. Näiteks rakenduste ja teenuste, muutuste sõlm ja versioonitäienduse teabe loomine.
  - [Usaldusväärne osalejate programmeerimisega mudeli sündmused](service-fabric-reliable-actors-diagnostics.md)
  - [Mudeli sündmuste kavandamise usaldusväärsed teenused](service-fabric-reliable-services-diagnostics.md)
- **Rakenduse sündmused**: sündmused nõrgunud oma teenuse kood ja EventSource helper klassi lähtutud Visual Studio mallide abil välja kirjutatud. Kuidas kirjutada logid oma rakenduse kohta leiate lisateavet teemast [kuvari ja teenuste kohalikus arvutis arengu setup diagnoosimine](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Diagnostika laiend juurutamine
Esimene samm kogumise logid on juurutada diagnostika laienduse iga teenuse struktuuri kobar VM. Diagnostika laiend kogub logid iga VM ja lisatud neile salvestusruumi kontole määratud. Juhised sõltuvad pisut kas kasutate Azure portaali või Azure ressursihaldur. Juhised erinevad kas juurutamise on osa kobar loomine või on klaster, mis on juba olemas. Vaatame juhiseid iga stsenaariumi puhul.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Diagnostika laiend kobar loomine portaali kaudu osana juurutamine
Diagnostika laiend juurutamiseks klaster VM kobar loomine osana kasutate diagnostika sätete paani näidatud järgmisel pildil. Usaldusväärne osalejate või usaldusväärsed teenused sündmuse saidikogumi lubamiseks veenduge, et diagnostika väärtuseks on määratud **kohta** (vaikesäte). Kui olete loonud klaster, ei saa neid sätteid muuta portaali kaudu.

![Azure'i diagnostika sätete portaalis kobar loomine](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Azure'i toe meeskonna *jaoks on vaja* tugi logib lahendamiseks, mis tahes teie loodud tugi taotlused. Need logid reaalajas kogutakse ja salvestatakse ühe salvestusruumi konto loodud ressursirühma. Diagnostika sätete konfigureerimine Rakendusetaseme sündmused. Need sündmused kaasata [Usaldusväärne osalejate](service-fabric-reliable-actors-diagnostics.md) sündmused, [Usaldusväärseid teenuseid](service-fabric-reliable-services-diagnostics.md) sündmused ning mõned süsteemi tasemel teenuse struktuuri sündmused talletamise Azure Storage.

Nagu [Elastne otsing](service-fabric-diagnostic-how-to-use-elasticsearch.md) või oma protsessi pääsevad sündmuste salvestusruumi konto kaudu. Praegu kuidagi filtreerimiseks või puhastada sündmusi, mis saadetakse tabel. Kui te ei rakendada protsessi eemaldamiseks tabelist sündmused, kasvab jätkuvalt tabel.

Kui loote klaster portaali kaudu, soovitame tungivalt alla laadida soovitud malli *enne nupu * *OK*** klaster loomiseks. Lisateavet leiate [teenuse struktuuri kobar on Azure ressursihaldur malli abil](service-fabric-cluster-creation-via-arm.md)häälestada. Peate malli muuta hiljem, kuna te ei saa teha muudatusi, kasutades portaali.

Järgmiste juhiste abil saate eksportida portaalist mallid. Neid malle võib olla raskem kasutada, kuna need võib olla tühjad väärtused, mis on puudu nõutav teave.

1. Avage oma ressursirühma.
2. Valige **sätted** kuvamiseks Rühmasätete paneel.
3. Valige **juurutuste** juurutamise ajalugu paani kuvamiseks.
4. Valige juurutamine juurutamise üksikasjade kuvamiseks.
5. Valige **Ekspordi malli** kuvamiseks malli paani.
6. Valige mall, parameetri ja PowerShelli faile sisaldava ZIP-faili eksportimiseks **faili salvestada** .

Pärast eksportimist faile, peate tegema muutmist. Parameters.json faili redigeerimiseks ja eemaldage **adminPassword** element. See põhjustab küsi parooli juurutamise skripti käitamisel. Kui kasutate juurutamise skripti, peate lahendada null parameetrite väärtused.

Allalaaditud malli abil saate värskendada konfiguratsiooni:

1. Kohalikus arvutis kausta sisu ekstraktimiseks.
2. Uue konfiguratsiooni kajastamiseks sisu muuta.
3. Käivitage PowerShelli ja muuta kausta, kuhu ekstraktitud sisu.
4. Käivita **deploy.ps1** ja täitke Tellimuse ID, ressursside rühma nime (kasutage sama nime värskendada konfiguratsiooni) ja juurutamise kordumatu nimi.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Azure'i ressursihaldur juurutada diagnostika laiend osana kobar loomine
Ressursihaldur abil luua klaster, peate enne loomist klaster täielik kobar ressursihaldur malli diagnostika konfiguratsiooni JSON lisamiseks. Pakume diagnostika konfiguratsiooni, lisatakse see osa meie ressursihaldur malli näidised valimi viie-VM kobar ressursihaldur malli. Näete seda galeriis Azure'i näidised selles asukohas: [viie sõlme kobar valimi diagnostika ressursihaldur malli abil](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Diagnostika sätte ressursihaldur malli kuvamiseks avage azuredeploy.json fail ja **IaaSDiagnostics**otsimine. Selle malli abil saate luua klaster, valige **Deploy Azure** nupp Eelmine link saadaval.

Teise võimalusena saate alla laadida ressursihaldur valimi, seda muuta ja luua klaster muudetud malli abil soovitud `New-AzureRmResourceGroupDeployment` käsk Azure PowerShelli aknas. Vt parameetrid, mida te kaotate käsk järgmine kood. PowerShelli abil ressursirühma juurutamise kohta leiate üksikasjalikumat teavet leiate artiklist [Deploy ressursirühma Azure'i ressursihaldur malli abil](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Diagnostika laiend juurutama mõne olemasoleva kobar
Kui teil on olemasolevaid kobar, mis ei sisalda diagnostika juurutatud või kui soovite muuta olemasoleva konfiguratsiooni, saate lisada või värskendada. Ressursihaldur malli loomiseks olemasoleva kobar või alla laadima malli portaalist eespool kirjeldatud kasutatava muuta. Muutke template.json faili, tehes järgmist.

Lisada uue ressursi salvestusruumi malli, lisades jaotisest ressursid.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Järgmisena lisage jaotisse parameetrite vahetult pärast salvestusruumi konto määratlused, vahel `supportLogStorageAccountName` ja `vmNodeType0Name`. Kohatäite teksti *salvestusruumikonto nimi siia* asendada salvestusruumi konto nimi.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Värskendage seejärel soovitud `VirtualMachineProfile` osa template.json faili, lisades järgmine kood laiendid massiivi sees. Kindlasti lisada koma soovitud algusesse või lõppu, olenevalt sellest, kuhu see lisatakse.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Kui muudate template.json faili, nagu on kirjeldatud, uuesti avaldada ressursihaldur mall. Malli eksportimisel faili deploy.ps1 republishes mall. Pärast juurutamist, veenduge, et **ProvisioningState** ei **õnnestunud**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Diagnostika koguda ja vestluste uute EventSource kanalitest pärit värskendamine
Diagnostika koguda logid EventSource uued kanalid, mis tähistab uue rakenduse, mille olete juurutada, teha samad juhised nagu [eelmises jaotises](#deploywadarm) diagnostika häälestamise mõne olemasoleva kobar värskendada.

Värskendage soovitud `EtwEventSourceProviderConfiguration` template.json faili jaoks uued kanalid EventSource enne rakendamist konfiguratsiooni värskendamine abil kirjete lisamiseks jaotis soovitud `New-AzureRmResourceGroupDeployment` PowerShelli käsu. Sündmuse allika nimi on määratletud osa oma kood ServiceEventSource.cs Visual Studio loodud fail.

Näiteks kui teie Sündmuse allikas nimega Minu Eventsource, lisage järgmine kood paigutamiseks üritused minu Eventsource nimega MyDestinationTableName tabelisse.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Koguda jõudluse hinnale või sündmuselogide, muuta ressursihaldur malli loomine [Windowsi virtuaalse masina jälgida ja diagnostika abil mõni Azure ressursihaldur Mall](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)näited. Avaldage ressursihaldur mall.

## <a name="next-steps"></a>Järgmised sammud
Mõistmaks täpsemalt millised sündmused peaks otsima ajal tõrkeotsingu, lugege teemat diagnostika sündmuste kiiratav [Usaldusväärne osalejad](service-fabric-reliable-actors-diagnostics.md) ja [Usaldusväärne teenuste](service-fabric-reliable-services-diagnostics.md)jaoks.


## <a name="related-articles"></a>Seotud artiklid
* [Saate teada, kuidas koguda jõudluse hinnale või logid diagnostika laiendamise abil](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Teenuse struktuuri lahenduse Log Analytics](../log-analytics/log-analytics-service-fabric.md)
