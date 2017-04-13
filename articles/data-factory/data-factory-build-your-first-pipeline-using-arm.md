<properties
    pageTitle="Koostada esimese andmete factory (ressursihaldur malli) | Microsoft Azure'i"
    description="Selles õpetuses loote valimi Azure'i andmed Factory kohaletoimetamisel on Azure ressursihaldur malli abil."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Õpetus: Koostada oma esimese Azure'i andmed factory Azure'i ressursihaldur malli abil
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-build-your-first-pipeline.md)
- [Azure'i portaal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShelli](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressursihaldur Mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-GA](data-factory-build-your-first-pipeline-using-rest-api.md)

Selles artiklis malli abil saate ka Azure ressursihaldur luua oma esimese Azure'i andmed factory.

## <a name="prerequisites"></a>Eeltingimused
- Lugege läbi [Õpetuse ülevaate](data-factory-build-your-first-pipeline.md) artikli ja täitke juhised **nõutav** .
- Järgige juhiseid teemas [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) Azure PowerShelli uusima versiooni installimine arvutisse.
- Leiate Azure'i ressursihaldur mallide kohta leiate [Azure'i ressursihaldur mallide koostamine](../resource-group-authoring-templates.md) . 

## <a name="in-this-tutorial"></a>Selles õpetuses
Üksuse | Kirjeldus  
------ | ----------- 
Azure'i lingitud salvestusteenus | Lingid Azure Storage konto andmeid factory. Azure Storage konto hoiab selle valimi tulemas sisend- ja andmeid. 
Hdinsightiga nõudmisel lingitud teenus| Lingid kuvatakse nõudmisel Hdinsightiga klaster abil andmete factory. Klaster on teie jaoks automaatselt loodud andmete töötlemise ja kustutatakse pärast töötlemine on lõpule jõudnud.
Azure'i bloobimälu Sisestuskeel andmekomplekti | Viitab Azure Storage lingitud teenus. Lingitud teenuse viitab Azure Storage konto ja Azure'i bloobimälu andmekomplekti määrab talletamist, mis hoiab sisendandmete container, kausta ja faili nimi. 
Azure'i bloobimälu väljund andmekomplekti | Viitab Azure Storage lingitud teenus. Lingitud teenuse viitab Azure Storage konto ja Azure'i bloobimälu andmekomplekti määrab talletamist, mis hoiab andmeid väljundi container, kausta ja faili nimi. 
Andmete müügivõimaluste | Tulemas on üks tegevuse tüüp HDInsightHive tarbib Sisestuskeel andmekomplekti ja annab tulemiks väljundi andmekomplekti.   

Andmete factory võib olla üks või mitu torujuhtmete. Müügivõimaluste võib olla üks või mitu tegevusi. On kahte tüüpi tegevusi: [andmete liikumine](data-factory-data-movement-activities.md) ja [andmete teisendus tegevus](data-factory-data-transformation-activities.md). Selles õpetuses loote müügivõimaluste ühe tegevuse (Kopeeri tegevus).

Järgmine jaotis pakub täieliku ressursihaldur malli määratlemiseks andmete Factory üksused nii, et saate kiiresti sirvides ja testida mall. Mõistmaks, kuidas iga andmete Factory üksus on määratletud, vt [malli andmete Factory üksuste](#data-factory-entities-in-the-template) lõik.

## <a name="data-factory-json-template"></a>Andmete Factory JSON Mall
Andmete factory määratlemiseks ülataseme ressursihaldur mall on: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Looge JSON fail nimega **ADFTutorialARM.json** **C:\ADFGetStarted** kausta sisu on järgmine:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
        },
        "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
            {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                    }
                }
            },
            {
                "type": "linkedservices",
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    }
                }
            },
            {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Teine näide ressursihaldur malli otsimine loomine on Azure andmete factory [õpetus: müügivõimaluste Kopeeri tegevuse on Azure ressursihaldur malli abil luua](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Parameetrite JSON 
Looge JSON fail nimega **ADFTutorialARM-Parameters.json** parameetrite Azure'i ressursihaldur malli sisaldava.  

> [AZURE.IMPORTANT] Määrake selle parameetri faili nimi ja Azure Storage konto võti **storageAccountName** ja **storageAccountKey** parameetrid. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Teil võib olla eraldi parameetri JSON failide arengu, testimine ja tootmise keskkondades, mida saate kasutada sama andmete Factory JSON malli abil. Power Shell script abil automatiseerida juurutamisel andmete Factory üksuste nendes keskkondades. 

## <a name="create-data-factory"></a>Andmete factory loomine

1. **Azure'i PowerShelli** käivitamine ja käivitage järgmine käsk: 
    - Käivitage `Login-AzureRmAccount` ja sisestage kasutajanimi ja parool, mida kasutate Azure portaali sisse logida.  
    - Käivitage `Get-AzureRmSubscription` kõik selle konto tellimused kuvamiseks.
    - Käivitage `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` valige tellimus, mida soovite töötada. Selle tellimuse peaks olema sama, mis kasutasite Azure'i portaalis.
1. Käivitage järgmine käsk juurutamiseks andmete Factory üksuste ressursihaldur malli abil loodud samm 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Müügivõimaluste jälgimine
 
1.  Pärast sisselogimist [Azure'i portaal](https://portal.azure.com/), klõpsake nuppu **Sirvi** ja valige **andmete tehased**.
        ![Sirvi -> andmete tehased](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Klõpsake **Andmete tehased** labale loodud andmete factory (**TutorialFactoryARM**).   
2.  Klõpsake oma andmete factory **Andmete Factory** labale **skeem**.
        ![Diagrammi paan](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  **Diagrammivaate**näete torujuhtmed ja andmekomplektide kasutatud selles õpetuses ülevaade.
    
    ![Diagrammivaate](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Topeltklõpsake vaates diagrammi andmekomplekti **AzureBlobOutput**. Te näete, et sektorit, mida praegu töödeldakse.

    ![Andmekomplekti](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Kui töötlemine on lõpule jõudnud, näete sektorit **valmis** olekus. Kuvatakse nõudmisel Hdinsightiga kobar loomine kulub tavaliselt millalgi (umbes 20 minutit). Seetõttu oodata müügivõimaluste teha töötlemiseks sektorit **30 minutit** .

    ![Andmekomplekti](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Kui **olete valmis** olekus on sektorit, vaadake **partitioneddata** kausta **adfgetstarted** ümbrises bloobimälus väljundi andmete jaoks.  

Leiate juhised, kuidas kasutada Azure portaali labad jälgida müügivõimaluste ja andmekomplektide selles õpetuses olete loonud [kuvari andmekomplektide ja kohaletoimetamisel](data-factory-monitor-manage-pipelines.md) .

Saate jälgida oma andmete torujuhtmete ka jälgimine ja haldamine App. Vt [jälgimine ja haldamine Azure'i andmed Factory torustike jälgimise rakendust](data-factory-monitor-manage-app.md) lisateavet rakenduse kasutamise kohta. 

> [AZURE.IMPORTANT] Sisestuskeel faili kustutamine kui sektorit on töötlemine õnnestus. Seetõttu sektorit uuesti või õpetuse uuesti teha soovite, laadige inputdata kausta adfgetstarted-ümbrisest Sisestuskeel faili (input.log).

## <a name="data-factory-entities-in-the-template"></a>Andmete Factory üksuste Mall
### <a name="define-data-factory"></a>Andmete factory määratlemine
Saate määratleda andmete factory ressursihaldur Mall, nagu on näidatud järgmises näites:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

Funktsiooni dataFactoryName defineeritakse järgmiselt: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

See on kordumatu string, mis põhineb ressursi rühma ID-ga.  

### <a name="defining-data-factory-entities"></a>Andmete Factory üksuste määratlemine
JSON mallis on määratletud andmete Factory järgmised üksused: 

- [Azure'i lingitud salvestusteenus](#azure-storage-linked-service)
- [Hdinsightiga nõudmisel lingitud teenus](#hdinsight-on-demand-linked-service)
- [Azure'i bloobimälu Sisestuskeel andmekomplekti](#azure-blob-input-dataset)
- [Azure'i bloobimälu väljundi andmekomplekti](#azure-blob-output-dataset)
- [Andmete müügivõimaluste Kopeeri tegevus](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus
Määrake selle jaotise nime ja Azure storage konto võti. Teemast [Azure Storage lingitud teenuse](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON atribuutide abil saate määratleda Azure Storage lingitud teenuse kohta lisateavet. 

      {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureStorage",
          "description": "Azure Storage linked service",
          "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
          }
        }
      }

**ConnectionString** kasutab storageAccountName ja storageAccountKey parameetrid. Konfiguratsioonifail abil järgmiste parameetrite väärtused. Määratluse kasutab ka muutujate: azureStroageLinkedService ja dataFactoryName määratletud malli. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>Hdinsightiga nõudmisel lingitud teenus
Vt artikli [arvutada lingitud teenuste](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) JSON atribuutide abil saate määratleda Hdinsightiga nõudmisel lingitud teenuse kohta lisateavet.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Võtke arvesse järgmist. 

- Andmete Factory loob **Windowsi-põhiste** Hdinsightiga kobar teile eespool JSON. Te oleksite selle **Linuxi-põhiste** Hdinsightiga kobar loomine. Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Võite kasutada **oma Hdinsightiga kobar** asemel kuvatakse nõudmisel Hdinsightiga kobar. Üksikasjad leiate [Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- Hdinsightiga kobar loob **vaikimisi container** bloobimälu, määratud JSON (**linkedServiceName**). Klaster kustutamisel ei kustutata Hdinsightiga selles ümbrises. Selline käitumine on kujundus. Nõudmisel Hdinsightiga lingitud teenusega Hdinsightiga kobar luuakse iga kord, kui mõnda sektorit peab töödeldud, kui ei ole olemasoleva live kobar (**timeToLive**) ja kustutatakse kui töötlemine on lõpule jõudnud.

    Nagu töödeldakse veel sektoritele, näete oma Azure'i bloobimälu palju ümbriste. Kui te ei pea neid tõrkeotsingu tööd, võite kustutage need salvestusruumi kuidagi vähendada. Järgige neid ümbriste nimed mustri: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Näiteks [Microsoft salvestusruumi Exploreri](http://storageexplorer.com/) abil saate kustutada ümbriste Azure'i bloobimälus.

Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .



#### <a name="azure-blob-input-dataset"></a>Azure'i bloobimälu Sisestuskeel andmekomplekti
Saate määrata bloobimälu container, kausta ja faili, mis sisaldab lähteandmeid nimesid. Üksikasjad JSON atribuutide abil saate määratleda mõne Azure'i bloobimälu andmekomplekti kohta leiate [Azure'i bloobimälu andmekomplekti atribuudid](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) . 

      {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

See määratlus kasutab järgmisi määratletud parameeter mall: blobContainer inputBlobFolder, ja inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure'i bloobimälu väljund andmekomplekti
Saate määrata bloobimälu container ja kausta, mis hoiab andmeid väljundi nimed. Üksikasjad JSON atribuutide abil saate määratleda mõne Azure'i bloobimälu andmekomplekti kohta leiate [Azure'i bloobimälu andmekomplekti atribuudid](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

See määratlus kasutab järgmisi määratletud parameeter malli: blobContainer ja outputBlobFolder. 

#### <a name="data-pipeline"></a>Andmete müügivõimaluste
Müügivõimaluste, et muuta andmete taru skripti käivitades mõne nõudmisel Windows Azure Hdinsightiga kobar määratleda. JSON elementide abil saate määratleda müügivõimaluste selles näites kirjeldused leiate [Müügivõimaluste JSON](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Malli uuesti kasutamiseks 
Õpetuses, olete loonud malli määratlemiseks andmete Factory üksuste ja malli läbides parameetrite väärtused. Sama malli abil saate juurutada andmete Factory üksuste viibite, parameetri faili iga keskkonna loomine ja kasutada siis, kui selle keskkonnas juurutamine.     

Näide:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Pange tähele, et esimese käsu kasutab parameetri fail soovitud arenduskeskkond, testi keskkonnas teine ja kolmas üks tootmiskeskkonda.  

Saate kasutada ka korduvate ülesannete mall. Näiteks, peate looma palju andmeid tehased koos ühe või mitme torustikud rakendada sama loogika, kuid iga andmete factory kasutab eri Azure salvestusruumi ja Azure'i SQL-andmebaasi kontod. Selle stsenaariumi korral saate sama malli samas keskkonnas (arendaja, testi või tootmise) erinevate parameetri failidega loomine andmete tehased. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Ressursihaldur malli loomiseks lüüsi
Siin on valimi ressursihaldur malli loomiseks loogilise lüüsi tagasi. Kohapealse arvuti või Azure IaaS VM lüüsi installimine ja lüüsi registreerida andmete Factory teenuse klahvi abil. Üksikasjad leiate [andmete kohapealse ja pilveteenuse vahel liikumine](data-factory-move-data-between-onprem-and-cloud.md) .

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

See mall loob andmete factory, nimega GatewayUsingArmDF nimega lüüsi: GatewayUsingARM. 

## <a name="see-also"></a>Vt ka
| Teema: | Kirjeldus |
| :---- | :---- |
| [Tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) | Selles artiklis on toodud andmete teisendus tegevused (nt Hdinsightiga taru teisendus kasutasite selles õpetuses) loendi Azure'i andmed Factory ei toeta. |
| [Plaanimis- ja täitmise](data-factory-scheduling-and-execution.md) | Selles artiklis selgitatakse plaanimis- ja täitmise aspektide Azure'i andmed Factory rakenduse mudel. |
| [Torujuhtmete](data-factory-create-pipelines.md) | See artikkel aitab teil mõista torujuhtmed ja Azure'i andmed Factory ja kuidas neid kasutada ehitada lõpuni Andmepõhiste töövoogude oma stsenaarium või ettevõtte tegevusi. |
| [Andmekomplektide](data-factory-create-datasets.md) | See artikkel aitab teil mõista andmekogumite Azure'i andmed Factory.
| [Jälgimine ja haldamine torustike jälgimise App abil](data-factory-monitor-manage-app.md) | Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja silumine torujuhtmete jälgimine ja haldamine rakenduse abil. 

  

