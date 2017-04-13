<properties
   pageTitle="Azure'i andmed Factory andmete laadimine | Microsoft Azure'i"
   description="Saate teada, kuidas Azure'i andmed Factory andmete laadimine"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Azure'i andmed Factory andmete laadimine 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Andmete Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Selle õpetuse näitab, kuidas luua müügivõimaluste Azure'i andmed Factory Azure'i salvestusruumi bloobimälu andmete teisaldamiseks SQL Azure'i andmebaas. Järgmiste toimingute abil saate küll:

+ Häälestamine on salvestusruumi Azure'i bloobimälu näidisandmed.
+ Ühenduse Azure'i andmed Factory ressursid.
+ Saate luua müügivõimaluste salvestusruumi plekid andmete teisaldamiseks SQL-i andmebaas.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Enne alustamist

Tutvuge Azure'i andmed Factory, lugege [artikleid Sissejuhatus rakendusse Azure'i andmed Factory][].

### <a name="create-or-identify-resources"></a>Looge või ressursside tuvastamine

Selles õpetuses enne peab teil olema järgmistest allikatest:

   + **Azure'i salvestusruumi bloobimälu**: selles õpetuses kasutab Azure salvestusruumi bloobimälu andmeallikana Azure'i andmed Factory müügivõimaluste ja seega peab teil olema üks saadaval valimi andmete talletamiseks. Kui teil ei ole veel üks, saate teada, kuidas [Loo konto salvestusruumi][].

   + **SQL-i andmebaas**: selles õpetuses viib andmed Azure'i salvestusruumi bloobimälu SQL-i andmebaas ja nii peavad olema andmebaas veebis näidisandmetega AdventureWorksDW laaditud. Kui teil on juba andmebaas, saate teada, kuidas [ette ühte][Create a SQL Data Warehouse]. Kui andmebaas on, kuid ei selle ette näidisandmetega, saate [selle käsitsi laadimine][Load sample data into SQL Data Warehouse].

   + **Azure'i andmed Factory**: Azure'i andmed Factory lõpetab tegelik laadimine ja seega peab teil olema üks, mille abil saate koostada andmete liikumine kohaletoimetamisel. Kui teil ei ole veel üks, saate teada, kuidas luua samm 1 [Alustamine Azure'i andmed Factory (andmete Factory Editor)][].

   + **AZCopy**: peate AZCopy oma Azure'i salvestusruumi bloobimälu oma kohaliku kliendi Näidisandmete kopeerimiseks. Installi juhised leiate teemast [AZCopy dokumentatsiooni][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Samm 1: Azure'i salvestusruumi bloobimälu Näidisandmete kopeerimine

Kui olete valmis tegemist hõlbustavate, olete valmis oma Azure'i salvestusruumi bloobimälu Näidisandmete kopeerimiseks.

1. [Näidisandmete alla laadida][]. Andmed lisatakse veel kolme aastat müügiandmetega AdventureWorksDW näidisandmed.

2. See AZCopy käsu abil saate oma Azure'i salvestusruumi bloobimälu 3 aasta andmete kopeerimiseks.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Samm 2: Azure'i andmed Factory ressursside ühendamine

Nüüd, kui andmed on kohas saame luua Azure'i andmed Factory müügivõimaluste liikuda andmed Azure'i bloobimälu SQL-i andmebaas.

Alustamiseks avage [Azure'i portaal][] ja valige oma andmete factory vasakpoolses menüüs.

### <a name="step-21-create-linked-service"></a>Samm 2.1: Looge lingitud teenus

Oma andmete factory link oma Azure storage konto ja SQL-i andmebaas.  

1. Esmalt registreerimise alustamiseks "Lingitud teenused" jaotises teie andmete factory, klõpsates ja klõpsake "uute andmete talletamiseks." Valige jaotises, valige Azure Storage azure salvestusruumi registreerida oma tüübiks ja seejärel sisestage oma konto nimi ja konto võti nimi.

2. Registreerida SQL-i andmebaas liikuge autori- ja Deploy jaotises Valige "Uus andmesalve" ja "Azure SQL-i andmebaas". Kopeerida ja kleepida selle malli ja seejärel sisestage oma kindla teabe.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Samm 2.2: Määratlemine andmekomplekti

Pärast lingitud teenuste loomist meil andmekogumite määratleda.  Siin selleks, et andmed, mida teie andmebaas on salvestusruumi teisaldamine struktuuri.  Lugege lisateavet loomise kohta

1. Selle protsessi Alustuseks oma andmete factory autori- ja Deploy jaotisse liikumine.

2. Klõpsake nuppu 'Uus andmekomplekti' ja seejärel 'Azure'i bloobimälu' oma andmete factory salvestusruumi linkida.  Saate kasutada funktsiooni all skripti määratlemiseks Azure'i bloobimälu oma andmeid:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Nüüd saame määratleda meie andmekomplekti SQL-i andmebaas. Alustame samal viisil, klõpsates nuppu 'Uus andmekomplekti' ja seejärel "Azure SQL-i andmebaas".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Samm 3: Loomine ja käivitamine oma müügivõimaluste

Lõpetuseks, häälestamine ja Azure'i andmed Factory tulemas läbiviimisel.  See on toiming, mis lõpetab tegelike andmete liikumine.  Täisekraanvaade toimingutest, mida saate täita SQL-i andmebaas ja Azure andmete Factory leiate [siit][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Autori- ja Deploy jaotises nuppu veel käske ja seejärel "Uus müügivõimaluste".  Pärast tulemas loomist saate selle all kood andmed edastada teie andmebaas:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks käivitamiseks kuvamine.

- [Azure'i andmed Factory õppeteema][].
- [Azure SQL-i andmete ladu konnektor][]. See on viide core teema SQL Azure'i andmebaas Azure'i andmed Factory abil.


Nende teemadest Azure'i andmed Factory üksikasjalikku teavet. Azure'i SQL-andmebaasi või Hdinsightiga need arutada, kuid teave kehtib ka SQL Azure'i andmebaas.

- [Õpetus: Azure'i andmed Factory alustamine][] See on core õpetuse Azure'i andmed Factory andmete töötlemiseks. Selles õpetuses loote oma esimese müügivõimaluste, muuta ja analüüsida web logid igakuiselt Hdinsightiga kasutava. Pange tähele, et ei ole Kopeeri tegevust selles õpetuses.
- [Õpetus: Azure'i salvestusruumi bloobimälu andmete kopeerimine Azure'i SQL-andmebaasi][]. Selles õpetuses loote müügivõimaluste Azure'i andmed Factory Azure'i salvestusruumi bloobimälu andmete kopeerimine Azure'i SQL-andmebaasi sisse.

<!--Image references-->

<!--Article references-->
[AZCopy dokumentatsioon]: ../storage/storage-use-azcopy.md
[Azure SQL-i andmete ladu konnektor]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Salvestusruumi konto loomine]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Azure'i andmed Factory (andmete Factory Editor) kasutamise alustamine]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Azure'i andmed Factory tutvustus]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Õppeteema: Andmete kopeerimine Azure'i salvestusruumi bloobimälu Azure SQL-andmebaas]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Õpetus: Azure'i andmed Factory alustamine]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure'i andmed Factory õppeteema]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure'i portaal]: https://portal.azure.com
[Näidisandmete allalaadimine]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
