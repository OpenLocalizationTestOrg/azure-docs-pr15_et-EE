<properties
    pageTitle="Õpetus: Loo müügivõimaluste ressursihaldur malli abil | Microsoft Azure'i"
    description="Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse Azure'i ressursihaldur malli abil."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Õpetus: Luua müügivõimaluste Kopeeri tegevuse Azure'i ressursihaldur malli abil
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopeerige viisard](data-factory-copy-data-wizard-tutorial.md)
- [Azure'i portaal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure'i ressursihaldur Mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-GA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-I API-GA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Selle õpetuse näidatakse, kuidas luua ja jälgida Azure'i andmed factory, mis on Azure ressursihaldur malli abil. Müügivõimaluste rakenduses andmete factory kopeerib andmete Azure'i bloobimälu Azure SQL-andmebaasiga.

## <a name="prerequisites"></a>Eeltingimused
- Läbida [eeltingimuste ja õpetuse ülevaade](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) ja täitke juhised **nõutav** .
- Järgige juhiseid teemas [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) Azure PowerShelli uusima versiooni installimine arvutisse. Selles õpetuses kasutada PowerShelli juurutamiseks andmete Factory üksused. 
- (valikuline) Leiate Azure'i ressursihaldur mallide kohta leiate [Azure'i ressursihaldur mallide koostamine](../resource-group-authoring-templates.md) .


## <a name="in-this-tutorial"></a>Selles õpetuses

Selles õpetuses loote andmete factory andmete Factory järgmised üksused:

Üksuse | Kirjeldus  
------ | ----------- 
Azure'i lingitud salvestusteenus | Lingid Azure Storage konto andmeid factory. Azure'i salvestusruumi on andmesalve allikas ja Azure SQL-i andmebaas on valamu andmesalve Kopeeri tegevuste õpetuse. Seda määrab sisendandmete Kopeeri tegevuse sisaldava konto salvestusruumi. 
Azure'i SQL-andmebaasi lingitud teenus| SQL Azure'i andmebaasi andmete factory linke. Seda määrab Azure'i SQL-andmebaasiga, mis hoiab Kopeeri tegevuse väljundi andmeid. 
Azure'i bloobimälu Sisestuskeel andmekomplekti | Viitab Azure Storage lingitud teenus. Lingitud teenuse viitab Azure Storage konto ja Azure'i bloobimälu andmekomplekti määrab talletamist, mis hoiab sisendandmete container, kausta ja faili nimi. 
Azure SQL-i väljundi andmekomplekti | Viitab lingitud Azure SQL-i teenus. Lingitud Azure SQL-i teenuse viitab Azure SQL-i server ja Azure SQL-i andmekomplekti saate määrata, mis hoiab andmeid väljundi tabeli nimi. 
Andmete müügivõimaluste | Tulemas on ühe tegevuse tüüp, kopeerida, mis võtab Azure'i bloobimälu andmekomplekti sisendina ja Azure SQL-i andmekomplekti nimega väljund. Kopeeri tegevuse kopeerib andmete mõne Azure'i bloobimälu SQL Azure'i andmebaasi tabelisse.  

Andmete factory võib olla üks või mitu torujuhtmete. Müügivõimaluste võib olla üks või mitu tegevusi. On kahte tüüpi tegevusi: [andmete liikumine](data-factory-data-movement-activities.md) ja [andmete teisendus tegevus](data-factory-data-transformation-activities.md). Selles õpetuses loote müügivõimaluste ühe tegevuse (Kopeeri tegevus).

![Azure'i bloobimälu kopeerimiseks Azure'i SQL-andmebaas](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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

Looge JSON fail nimega **ADFCopyTutorialARM.json** **C:\ADFGetStarted** kausta sisu on järgmine:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
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
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
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
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parameetrite JSON 
Looge JSON fail nimega **ADFCopyTutorialARM-Parameters.json** parameetrite Azure'i ressursihaldur malli sisaldava. 

> [AZURE.IMPORTANT] Määrake nimi ja Azure Storage konto võti **storageAccountName** ja **storageAccountKey** parameetrid.  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Teil võib olla eraldi parameetri JSON failide arengu, testimine ja tootmise keskkondades, mida saate kasutada sama andmete Factory JSON malli abil. Power Shell script abil automatiseerida juurutamisel andmete Factory üksuste nendes keskkondades.  

## <a name="create-data-factory"></a>Andmete factory loomine
1. **Azure'i PowerShelli** käivitamine ja käivitage järgmine käsk:
    - Käivitage `Login-AzureRmAccount` ja sisestage kasutajanimi ja parool, mida kasutate Azure portaali sisse logida.  
    - Käivitage `Get-AzureRmSubscription` kõik selle konto tellimused kuvamiseks.
    - Käivitage `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` valige tellimus, mida soovite töötada. 
2. Käivitage järgmine käsk juurutamiseks andmete Factory üksuste ressursihaldur malli abil loodud samm 1.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Müügivõimaluste jälgimine
1. Azure'i konto kaudu [Azure portaali](https://portal.azure.com) sisse logida.
2. Klõpsake **andmete tehased** vasakul menüüs (või) klõpsake nuppu **rohkem teenuseid** ja seejärel **andmete tehased** **ÄRIANALÜÜSI + ANALYTICS** kategoorias.

    ![Menüü andmed tehased](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. **Andmete tehased** lehel otsida ja leida oma andmete factory. 

    ![Andmete factory otsimine](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klõpsake oma Azure'i andmed factory. Näete andmete factory Avaleht.

    ![Andmete factory Avaleht](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Klõpsake **diagrammi** paani oma andmete factory diagrammi kuvada.

    ![Diagrammivaate andmete factory.](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Topeltklõpsake vaates diagrammi andmekomplekti **SQLOutputDataset**. Näete seda olekut sektorit. Kui koopia toiming on lõpule jõudnud, oleku määrata **valmis**.

    ![Väljundi sektorit valmis olekus](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Kui **olete valmis** olekus on sektorit, veenduge, et andmed kopeeritakse **emp** SQL Azure'i andmebaasi tabelisse.

Leiate juhised, kuidas kasutada Azure portaali labad jälgida müügivõimaluste ja andmekomplektide selles õpetuses olete loonud [kuvari andmekomplektide ja kohaletoimetamisel](data-factory-monitor-manage-pipelines.md) .

Saate jälgida oma andmete torujuhtmete ka jälgimine ja haldamine App. Vt [jälgimine ja haldamine Azure'i andmed Factory torustike jälgimise rakendust](data-factory-monitor-manage-app.md) lisateavet rakenduse kasutamise kohta.


## <a name="data-factory-entities-in-the-template"></a>Andmete Factory üksuste Mall

### <a name="define-data-factory"></a>Andmete factory määratlemine
Saate määratleda andmete factory ressursi halduri malli, nagu on näidatud järgmises näites:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

Funktsiooni dataFactoryName defineeritakse järgmiselt: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

See on kordumatu string põhjal ressursi rühma ID-ga.  

### <a name="defining-data-factory-entities"></a>Andmete Factory üksuste määratlemine
JSON mallis on määratletud andmete Factory järgmised üksused: 

1. [Azure'i lingitud salvestusteenus](#azure-storage-linked-service)
2. [Lingitud SQL Azure'i teenus](#azure-sql-database-linked-service)
3. [Azure'i bloobimälu andmekomplekti](#azure-blob-dataset)
4. [Azure SQL-i andmekomplekti](#azure-sql-dataset)
5. [Andmete müügivõimaluste Kopeeri tegevus](#data-pipeline)

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

Funktsiooni connectionString kasutab storageAccountName ja storageAccountKey parameetrid. Konfiguratsioonifail abil järgmiste parameetrite väärtused. Määratluse kasutab ka muutujate: azureStroageLinkedService ja dataFactoryName määratletud malli. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure'i SQL-andmebaasi lingitud teenus
Selles jaotises Määrake Azure SQL serveri nimi, andmebaasi nimi, kasutajanimi ja kasutaja parooli. Teemast [Azure SQL-i lingitud teenuse](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) JSON atribuutide abil saate määratleda lingitud Azure SQL-i teenuse kohta lisateavet.  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

Funktsiooni connectionString kasutab sqlServerName, databaseName, sqlServerUserName ja sqlServerPassword parameetrite väärtused edastatakse konfiguratsiooni-faili abil. Määratluse kasutab ka järgmiste muutujate mallist: azureSqlLinkedServiceName dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure'i bloobimälu andmekomplekti
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
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL-i andmekomplekti
Saate määrata selle tabeli nimi, SQL Azure'i andmebaas, mida leiate Azure'i bloobimälu kopeeritud andmed. Teemast [Azure SQL-i andmekomplekti atribuutide](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) JSON atribuutide abil saate määratleda on Azure SQL-i andmekomplekti üksikasjad. 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Andmete müügivõimaluste
Müügivõimaluste, mis kopeerib andmete Azure'i bloobimälu andmekomplekti Azure SQL-i andmekomplekti määratleda. JSON elementide abil saate määratleda müügivõimaluste selles näites kirjeldused leiate [Müügivõimaluste JSON](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Malli uuesti kasutamiseks 
Õpetuses, olete loonud malli määratlemiseks andmete Factory üksuste ja malli läbides parameetrite väärtused. Tulemas kopeerib andmete Azure Storage konto kaudu Parameetrid määratud Azure SQL-andmebaasi. Sama malli abil saate juurutada andmete Factory üksuste viibite, parameetri faili iga keskkonna loomine ja kasutada siis, kui selle keskkonnas juurutamine.     

Näide:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Pange tähele, et esimese käsu kasutab parameetri fail soovitud arenduskeskkond, testi keskkonnas teine ja kolmas üks tootmiskeskkonda.  

Saate kasutada ka korduvate ülesannete mall. Näiteks, peate looma palju andmeid tehased koos ühe või mitme torustikud rakendada sama loogika, kuid iga andmete factory kasutab eri Azure salvestusruumi ja Azure'i SQL-andmebaasi kontod. Selle stsenaariumi korral saate sama malli samas keskkonnas (arendaja, testi või tootmise) erinevate parameetri failidega loomine andmete tehased.   

