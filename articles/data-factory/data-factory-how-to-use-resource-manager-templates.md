<properties 
    pageTitle="Kasutage ressursihaldur Mallid andmete Factory | Microsoft Azure'i" 
    description="Saate teada, kuidas luua ja Azure ressursihaldur mallide abil saate luua andmete Factory üksused." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Mallide abil saate luua Azure'i andmed Factory üksused

## <a name="overview"></a>Ülevaade
Diagrammide korduvkasutamine sama muster kasutamisel Azure'i andmed Factory teie andmete integreerimise vajadustele, võite ise üle viibite või rakendada sama ülesande korduvas sees sama lahendust. Mallide abil saate rakendada ja hallata järgmisi olukordi lihtne viisil. Azure'i andmed Factory Mallid on käepärane stsenaariumi, mis hõlmavad korduvkasutatavuse ja kordamine.
 
Kaaluge võimalust olukorda, kus organisatsioon on 10 tootmisprotsessi seadmete kogu maailmas. Logide kaudu iga on salvestatud eraldi asutusesisese SQL serveri andmebaasi. Ettevõtte soovib koostada ühe andmebaas erakorralist analytics pilveteenuses. Samuti soovib on sama loogika, kuid muu konfiguratsioone arengu, keskkondade. 

Sel juhul peab tööülesande korrata üle 10 andmete tehased iga tootmisprotsessi taimest jaoks sama keskkonnas, kuid erinevaid väärtusi. Tegelikult on **kordamine** . Templating võimaldab võtmiseks see üldise vool (, millel on sama tegevuse iga andmete factory torujuhtmetes), kuid kasutab iga tootmisprotsessi taimest eraldi parameetri faili.

Lisaks nagu organisatsiooni soovib Juurutage need 10 andmete tehased mitu korda viibite, malle saate kasutada seda **korduvkasutatavuse** eraldi parameetri failid arengu, keskkondade kasutades.

## <a name="templating-with-azure-resource-manager"></a>Azure'i ressursihaldur templating
[Azure'i ressursihaldur Mallid](../azure-resource-manager/resource-group-overview.md#template-deployment) on suurepärane viis templating rakenduses Azure'i andmed Factory saavutamiseks. Ressursihaldur Mallid määratlevad taristu ja Azure lahenduse konfiguratsiooni JSON faili kaudu. Kuna Azure'i ressursihaldur Mallid töötada kõik/kõige Azure teenused, seda suuresti saab hallata oma Azure varade kõik ressursid. Vaadake lisateavet mallide ressursihaldur üldiselt [loome Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md) . 

## <a name="tutorials"></a>Õpetused
Lugege üksikasjalikke juhiseid, et luua andmete Factory üksuste ressursihaldur mallide abil järgmised juhised:

- [Õpetus: Azure'i ressursihaldur malli abil andmete kopeerimiseks Müügivõimaluste loomine](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Õpetus: Azure'i ressursihaldur malli abil luua andmete töötlemise müügivõimaluste](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Andmete Factory Mallid github
Vaadake Azure Lühijuhend järgmised Mallid github: 

- [Andmete factory Azure'i bloobimälu andmete kopeerimine Azure'i SQL-andmebaasi loomine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Luua andmete factory taru tegevuse Windows Azure Hdinsightiga kobar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Andmete factory Salesforce'i andmete kopeerimine Azure plekid loomine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Julgelt jagada oma Azure'i andmed Factory Mallid veebisaidil [Azure'i Kiirkäivituse](https://azure.microsoft.com/documentation/templates/). Vaadake [osakaal juhend](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) väljatöötamisel malle, mida saab jagada selle hoidla kaudu. 

Järgmistes jaotistes üksikasjade määratlemine andmete Factory ressursid ressursihaldur malli. 

## <a name="defining-data-factory-resources-in-templates"></a>Määratleda andmete Factory ressursid Mallid
Andmete factory määratlemiseks ülataseme mall on:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Andmete factory määratlemine

Saate määratleda andmete factory ressursihaldur Mall, nagu on näidatud järgmises näites:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

Funktsiooni dataFactoryName "muutujad" on määratletud järgmiselt:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Lingitud teenuste määratlemine 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


[Lingitud salvestusteenus](data-factory-azure-blob-connector.md#azure-storage-linked-service) või [Arvutada lingitud teenuste](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) kohta vaadake teavet soovite juurutada teatud lingitud teenuse JSON atribuutide. "Ei leia dependsOn" parameeter saate määrata vastavate andmete factory nime. Lingitud teenuse Azure Storage määratlemisel näide on esitatud järgmine JSON määratlus:

### <a name="define-datasets"></a>Andmekomplektide määratlemine

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Vaadake [toetatud andmete](data-factory-data-movement-activities.md#supported-data-stores-and-formats) üksikasjade JSON atribuute soovite juurutada teatud andmekomplekti tüüp. Märkus "ei leia dependsOn" parameeter määrab nimi ja vastavaid andmeid factory salvestusruumi lingitud teenus. Näide määratlemine andmekomplekti tüüpi Azure'i bloobimälu on kujutatud järgmises JSON määratluse:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Torujuhtmete määratlemine

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Vaadake [torujuhtmete määratlemise](data-factory-create-pipelines.md#pipeline-json) kohta JSON atribuutide teatud müügivõimaluste ja tegevused, mida soovite kasutada. Pange tähele, et "ei leia dependsOn" parameeter määrab andmete factory nime ja mis tahes vastav lingitud teenuseid või andmekomplektide. Müügivõimaluste, mis kopeerib andmete Azure'i bloobimälu Azure'i SQL-andmebaasi näide on esitatud järgmine JSON koodilõigu:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Parameterizing andmete Factory Mall
Parimad tavad parameterizing, leiate [Azure'i ressursihaldur mallide loomise head tavad](../resource-manager-template-best-practices.md#parameters) artikli jaoks. Üldiselt peaks minimeeritud parameetri kasutamine, eriti siis, kui muutujaid saab kasutada hoopis. Ainult anda parameetrid järgmistel juhtudel:

- Keskkonna erinevad sätted (näide: arengu, testimine ja tootmise)
- Saladusi (nt paroolid)

Kui teil on vaja tõmmata saladusi [Azure'i klahvi](../key-vault/key-vault-get-started.md) hoidlast juurutamisel Azure'i andmed Factory üksuste mallide kasutamine, määrake **võtme hoidla** - ja **salajane nime** nagu on näidatud järgmises näites:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Kuigi eksportimise malle olemasolevad andmed tehased praegu veel ei toetata, on töötab. 


