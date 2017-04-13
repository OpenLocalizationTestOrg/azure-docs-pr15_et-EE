<properties
   pageTitle="Laadida suurte andmehulkade Lake andmesalve ühenduseta abil | Microsoft Azure'i"
   description="Azure'i salvestusruumi plekid andmete kopeerimine Lake andmesalve AdlCopy tööriista abil"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Azure'i impordi ja ekspordi teenust kasutada ühenduseta eksemplar Lake andmesalve andmete jaoks

Selles artiklis tutvustame teile suur andmekomplektide kopeerimine (> 200GB) Azure'i andmed Lake salvestuskohta võrguühenduseta meetoditega, nagu [Teenuse Azure impordi/ekspordi](../storage/storage-import-export-service.md). Täpsemalt näide selle artikli fail on 339,420,860,416 baiti umbes 319GB vaba. Vaatame kõne selle faili 319GB.tsv.

Azure'i impordi/ekspordi teenus võimaldab teil turvaliselt edastamiseks suurte andmehulkade Azure'i bloobimälu kõvaketta draivid on Azure andmekeskuse saatmine.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Storage konto**.

- **Azure'i andmeanalüüsi Lake konto (valikuline)** – vt [Alustamine Azure'i andmeanalüüsi Lake](../data-lake-analytics/data-lake-analytics-get-started-portal.md) juhised, kuidas luua Lake andmesalve konto.


## <a name="preparing-the-data"></a>Andmete ettevalmistamine

Enne impordi/ekspordi teenuse kasutamist tuleb me murda andmefaili olema edastatud **üheks eksemplarid, mis on väiksem kui 200 GB** suuruseid. See on import tööriist ei tööta üle 200GB faile. Kud, selles õpetuses me jagada faili osa 100GB baiti. Saate teha seda hõlpsalt [Cygwin](https://cygwin.com/install.html)abil. Cygwin toetab Linuxi käsk, mis võimaldab kasutajatel hõlpsasti toiming. Meie puhul kasutame järgmine käsk.

    split -b 100m 319GB.tsv

Tükeldatud toiming loob failide nimede allpool.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Andmetega ketast ettevalmistamine

Järgige juhiseid [kasutades Azure impordi/ekspordi](../storage/storage-import-export-service.md) teenust (jaotises **ettevalmistamine oma draivid** ) ette valmistada oma kõvaketas. Siin on üldised voo kohta, kuidas valmistada ketas.

1. Hankida arvuti kõvakettale, mis vastab kasutatava teenuse Auzre impordi/ekspordi.

2. Määratlege Azure Storage konto, kus saab andmed kopeeritakse üks kord selle si saadetakse Azure data Centeri kaudu.

3. Kasutage [Azure impordi/ekspordi vahend](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), käsurea kasuliku. Siin on näide koodilõigu tööriista kasutamise kohta.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Leiate [Azure'i impordi/ekspordi teenuse abil](../storage/storage-import-export-service.md) veel valimi Koodilõigud kohta, kuidas kasutada **Tööriista Azure impordi/ekspordi**.

4. Ülaltoodud käsk loob kausta Päevik faili määratud asukohas. Kasutate selle töölehe faili loomiseks on töö [Azure klassikaline portaali](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Mõne importimine töö loomine

Nüüd saate luua importimine töö, mis (jaotises **Loo importimine töö** ) [abil Azure impordi/ekspordi](../storage/storage-import-export-service.md) teenust juhiste abil. Selle impordi töö ja muud üksikasjad pakuvad ka töölehe faili loodud on kettadraivide koostamise ajal.

## <a name="physically-ship-the-disks"></a>Füüsilise saata selle ketast

Saate nüüd füüsilise saadate ketast Azure andmekeskuse, kus andmed kopeeritakse üle Azure'i salvestusruumi plekid andsite importimine töö loomisel. Lisaks töö loomisel kui ise otsustanud pakkuda hiljem jälitusteabe saate nüüd saate minna tagasi tööpäevad impordi ja värskendatud jälgimise numbri.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Andmete kopeerimine Azure salvestusruumi plekid Azure andmesalve Lake

Kui importimine töö olek kuvatakse lõpule viidud, saate kontrollida, kas andmed on saadaval Azure salvestusruumi plekid oli määratud. Seejärel saate andmeid Azure'i salvestusruumi plekid liikumiseks Azure'i andmesalve Lake mitmesuguseid viise. Kõigi saadaolevate suvandite andmete üleslaadimine, vt [Lake andmesalve Ingesting andmeid](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

Selles jaotises meil teile pakkuda JSON määratlused, mille abil saate luua ka Azure'i andmed Factory müügivõimaluste andmete kopeerimine. Saate neid JSON määratlusi [Azure portaali](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)kaudu.

### <a name="source-linked-service-azure-storage-blob"></a>Andmeallika lingitud teenuse (salvestusruumi Azure'i bloobimälu)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Target (sihtkoht) lingitud teenuse (Azure'i Lake andmesalve)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Sisestuskeel andmehulgas.
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Väljundi andmehulgas.
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Müügivõimaluste (Kopeeri tegevus)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
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
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Azure'i andmed Factory andmete teisaldamiseks Azure'i salvestusruumi bloobimälu ja Azure andmesalve Lake kasutamise kohta leiate lisateavet teemast [salvestusruumi Azure'i bloobimälu, Azure'i andmed Lake Store Azure'i andmed Factory abil andmete teisaldamine](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Töötajatel andmefailid Azure'i Lake poes

Me failiga, mis on 319GB suuruseid ja katki see failidesse väiksem nii, et saab üle Azure impordi/ekspordi teenuse kasutamise alustamine. Nüüd, kui andmed on Azure Lake poes, saame reconstrcut faili algse suuruse. Saate seda teha järgmised Azure PowerShelli cmldts.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
