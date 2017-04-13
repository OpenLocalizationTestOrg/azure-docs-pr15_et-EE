<properties
    pageTitle="Määrab Autoscale Windowsi virtuaalse masina skaala | Microsoft Azure'i"
    description="Autoscaling häälestamine on virtuaalse masina skaala määra Windows Azure'i PowerShelli kaudu"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automaatselt mastaapida masinad kogumi virtuaalse masina skaala

Virtuaalse masina skaala komplektid hõlbustavad juurutada ja hallata identse virtuaalmasinates kogumina. Skaala komplektid pakkuda väga paindlik ja kohandatav Arvuta kiht hyperscale rakenduste ja nad ei toeta Windowsi platvormi pildid, Linux platvormi pildid, kohandatud pilte ja laiendid. Skaala komplekti kohta leiate lisateavet teemast [Virtuaalse masina skaala määrab](virtual-machine-scale-sets-overview.md).

Selle õpetuse näidatakse, kuidas luua Windowsi virtuaalmasinates skaala ja automaatselt mastaapida masinad määramine. Ulatuse määramine ja skaleerimist teguriga on Azure ressursihaldur malli loomine ja juurutamine Azure PowerShelli abil häälestada loomine Mallide kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](../resource-group-authoring-templates.md). Automaatse skaleerimist skaala komplekti kohta leiate lisateavet teemast [automaatse suuruse muutmise ja virtuaalse masina skaala määrab](virtual-machine-scale-sets-autoscale-overview.md).

Selles artiklis juurutamist järgmised ressursid ja laiendid:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Ressursihaldur ressursside kohta leiate lisateavet teemast [Azure arvutada, võrku, ja salvestusruumi pakkujate jaotises Azure'i ressursihaldur](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Samm 1: Azure'i PowerShelli installimine

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) Azure PowerShelli uusima versiooni installimise, valides tellimuse ja Azure sisselogimise kohta teavet.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Samm 2: Looge ressursirühma ja salvestusruumi konto

1. **Ressursi rühma loomine** – kõik ressursid tuleb kasutusele võtta ressursirühma. [Uus-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) abil saate luua nimega **vmsstestrg1**ressursirühma.

2. **Loo konto salvestusruumi** – see salvestusruumi konto on, kus mall on salvestatud. [Uus-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) abil saate luua salvestusruumi konto nimega **vmsstestsa**.

## <a name="step-3-create-the-template"></a>Samm 3: Malli loomine
Mõni Azure ressursihaldur Mall võimaldab teil juurutada ja hallata Azure ressursid koos JSON kirjeldus ressursid ja seotud juurutamise parameetrite abil.

1. Saate oma lemmik redaktoris faili C:\VMSSTemplate.json loomine ja algse JSON liigendada toetamiseks mall.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parameetrite alati need pole kohustuslikud, kuid need võimaldavad malli juurutamisel väärtuste sisestamiseks. Lisage need parameetrid jaotises malli lisatud parameetrite emaelement.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Määrake eraldi juurdepääsu skaala masinad kasutatava virtuaalse masina nimi.
    - Kui mall on salvestatud salvestusruumi konto nimi.
    - Arv virtuaalmasinates loomiseks algselt skaala määramine.
    - Nimi ja klõpsake soovitud virtuaalmasinates administraatorikonto parooli.
    - Ulatuse määramine toetamiseks loodud ressursid eesliite nimi.

3. Muutujaid saab kasutada malli määrata väärtused, mis võivad muutuda sageli või väärtused, mis on vaja luua kombinatsiooni parameetrite väärtused. Lisage need muutujad jaotises malli lisatud muutujate emaelement.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - DNS-i nimed, mida kasutatakse võrgu liidesed.
    - IP address nimed ja eesliidete tähised virtuaalse võrgu-ja alamvõrku.
    - Nimede ja identifikaatorite virtuaalse võrgu laadimine koormusetasakaalustusteenuse ja võrgu liidesed.
    - Salvestusruumi kontonimed skaala masinad seotud konto määramine.
    - Diagnostika laiend, kuhu on installitud on virtuaalmasinates sätted. Diagnostika pikendamise kohta leiate lisateavet teemast [loomine Windowsi virtuaalse masina jälgida ja diagnostika Azure'i ressursihaldur malli abil](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Malli lisatud ressursid emaelement ressurssi salvestusruumi konto lisamine. See mall kasutab esitatavaks loomiseks soovitatav viis salvestusruumi kontod operatsioonisüsteemi ketast ja Diagnostikaandmete talletamiseks. Nende kontode toetab kuni 100 virtuaalmasinates skaala komplekti, mis on suurim. Iga salvestusruumi konto nimi on tähe olekutähiste, määratletud muutujad koos eesliite esitate parameetrid malli abil.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Lisage virtuaalse võrguressurssi. Lisateavet leiate teemast [Ressursi pakkuja](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Lisage avaliku IP-aadressi ressursse, mida kasutatakse laadi koormusetasakaalustusteenuse ja võrgu liidese.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Saate lisada laadi koormusetasakaalustusteenuse ressursi kasutatava skaala määramine. Lisateabe saamiseks lugege teemat [Azure ressursihaldur tugi laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Lisage eraldi virtuaalse masina kasutatavat võrguressurssi kasutajaliidese. Kuna masinad skaala komplekti pole avaliku IP-aadressi kaudu, luuakse eraldi virtuaalse masina sama virtuaalse võrgu kaugühenduse teel Accessi masinad.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Ulatuse määramine samasse võrku eraldi virtuaalse masina lisada.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"        
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. Virtuaalse masina skaala seadmine ressursi lisamine ja määrake diagnostika laiend, kuhu on installitud kõik virtuaalmasinates skaala määramine. Paljud selle ressursi sätted on sarnased virtuaalse masina ressursiga. Peamised erinevused on võimsus element, mis määrab arvu virtuaalmasinates skaala määramine ja upgradePolicy, mis täpsustab, kuidas virtuaalmasinates tehtud värskendused. Ulatuse määramine on loodud, kuni kõik salvestusruumi kontod luuakse määratletud ei leia dependsOn element.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
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
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Lisage autoscaleSettings ressurss, mis määratleb, kuidas skaala määramine reguleerib põhjal protsessori kasutus masinad skaala määramine.

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
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Selles õppetükis oluliste need väärtused:

    - **metricName** – see väärtus on sama, mis jõudluse näidiku, mida me määratletud wadperfcounter muutuja. Muutuja kasutamisel diagnostika laiend kogub selle **Processor(_Total)\% protsessori aja** counter.
    - **metricResourceUri** – see väärtus on ressursiidentifikaator virtuaalse masina skaala komplekti.
    - **timeGrain** – väärtuseks granulaarsus mõõdikute, mis on kogunud. Selle malli väärtuseks ühe minuti.
    - **statistiline** – see väärtus määratleb, kuidas mõõdikud on ühendatud majutada automaatse skaleerimise toiming. Võimalikud väärtused on: Keskmine, Min ja Max. Selle malli kogutakse Keskmine kokku CPU hõivatus on virtuaalmasinates.
    - **timeWindow** – see väärtus on lahtrivahemik, kus on eksemplari andmete kogumise aja. Peab olema 5 minutit kuni 12 tundi.
    - **timeAggregation** – see väärtus määratleb, kuidas peaks kogutud andmete kombineerida aja jooksul. Vaikeväärtus on keskmine. Võimalikud väärtused on: Keskmine, miinimum, maksimum, viimase, kokku, loendus.
    - **korraldaja** – väärtuseks võrdlemiseks argumendil andmed ja piirmäär kasutatav tehtemärk. Võimalikud väärtused on: võrdub, NotEquals ületas, GreaterThanOrEqual, vähem kui, LessThanOrEqual.
    - **lävi** – see väärtus on väärtus, mis käivitab toimingu skaala. Selle malli masinad lisatakse kui keskmine CPU hõivatus masinad määramine seas on üle 50% skaala.
    - **suunas** – see väärtus määratleb toiming, mille võetaks lävi väärtuse saavutamiseks. Võimalikud väärtused on suurendada või vähendada. Selle malli suurendatakse virtuaalmasinates skaala määramine arvu piirmäär on üle 50% määratletud ajaakna.
    - **Tippige** – väärtuseks toimingut, mis peaks toimuma ja peab olema seatud ChangeCount tüüp.
    - **väärtuse** – selle väärtuseks on arv virtuaalmasinates, mis on lisada või eemaldada skaala määramine. Väärtus peab olema 1 või suurem. Vaikeväärtus on 1. Selle malli seadmine masinad skaala arv suureneb 1 juhul, kui piirmäär on täidetud.
    - **cooldown** – see väärtus on pärast viimast toimingut skaleerimise ootama, enne järgmise toimingu toimumise aeg. Väärtus peab olema üks minut ja üks nädal.

12. Salvestage mallifail.    

## <a name="step-4-upload-the-template-to-storage"></a>Samm 4: Malli üleslaadimine salvestusruum

Malli saate üles laadida, kui teate, et nime ja primaarvõti salvestusruumi konto, mille lõite juhises 1.

1.  Microsoft Azure'i PowerShelli aknas määrata muutuja, mis määrab nime salvestusruumi konto, mille lõite juhises 1.

            $storageAccountName = "vmstestsa"

2.  Määrake muutuja, mis määrab salvestusruumi konto primaarvõti.

            $storageAccountKey = "<primary-account-key>"

    Saate selle klahvi klõpsates võtme ikooni kuvamisel salvestusruumi konto ressursi Azure'i portaalis.

3.  Salvestusruumi konto konteksti objekti, mis on valideerimiseks toimingute salvestusruumi konto luua.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Container salvestamiseks malli loomine

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Uus ümbris malli faili üleslaadimiseks.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Juhis 5: Malli

Nüüd, kui olete loonud malli, saate alustada ressursse. Selle käsu abil saate alustada protsessi:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Kui vajutate sisestada, palutakse esitada teile määratud muutujate väärtused. Sisestage järgmised väärtused.

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Kulub umbes 15 minutit edukalt kasutatavad vahendid.

>[AZURE.NOTE] Portaali võimalus abil saate juurutada ressursside. Kasutage seda linki: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Samm 6: Kuvari ressursid

Saate teavet nende meetoditega virtuaalse masina skaala komplektid:

 - Azure'i portaal - praegu saate piiratud hulgal teavet portaalis.
 - [Azure'i Resource Explorer](https://resources.azure.com/) - see tööriist on parim oma skaala määramine hetkeseisu uurimine. Järgige selle tee ja näete eksemplari vaate loodud skaala määramine:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure'i PowerShelli – kasutage see käsk saada teavet:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Ühenduse eraldi virtuaalse masina täpselt samamoodi, nagu teeksite mis tahes muu seadme ja siis pääsete juurde kaugühenduse teel virtuaalmasinates Määrake üksikute protsesside jälgimiseks skaala.

>[AZURE.NOTE] Täieliku REST API skaala komplekti kohta teabe saamiseks leiate [Virtuaalse masina skaala määrab](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Juhis 7: Eemaldamine ressursside

Kuna ostmisega kasutada Azure ressursse, on alati hea tava kustutamine ressursse, mis ei ole enam vaja. Teil pole vaja iga ressursi eraldi kustutada ressursirühma. Saate kustutada ressursirühma ja kustutatakse kõik selle ressursid.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Kui soovite säilitada oma ressursirühm, saate kustutada ainult seadmine skaala.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Järgmised sammud

- Ulatuse määramine, et teie loodud teabe [haldamine virtuaalmasinates virtuaalse masina skaala Set](virtual-machine-scale-sets-windows-manage.md)abil hallata.
- Lisateavet vertikaalne skaleerimist teguriga [vertikaalne autoscale koos virtuaalse masina skaala komplekti](virtual-machine-scale-sets-vertical-scale-reprovision.md) läbivaatamine
- Azure'i kuvar jälgimise [Azure kuvari PowerShelli Kiire alustamine näidised](../monitoring-and-diagnostics/insights-powershell-samples.md) funktsioonide näiteid
- Lisateavet teate funktsioone [kasutada autoscale toiminguid saatmine e-posti ja webhook teatiste Azure'i monitoris](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Siit saate teada, kuidas [saata e-posti ja webhook teatiste Azure'i monitoris auditilogid kasutamine](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
