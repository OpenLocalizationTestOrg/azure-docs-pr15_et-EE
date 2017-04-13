<properties 
    pageTitle="Näidisandmete Azure Bloobivahemälu salvestusruumi | Microsoft Azure'i" 
    description="Azure'i bloobimälu näidisandmed" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Näidisandmete Azure Bloobivahemälu salvestusruum


Selle dokumendi hõlmab valimite andmed salvestatakse Azure'i bloobimälu allalaadimist programmiliselt ja seejärel proovide kirjutatud Python toimingute abil.

**Miks teie näidisandmed?**
Kui kavatsete analüüsida andmekomplekti on suur, on tavaliselt on hea mõte kasutada alla valimiga väiksem, kuid esindaja ja paremini hallataval mahu vähendamiseks andmed. See hõlbustab andmete mõistmine, avastamine ja funktsioon matemaatika erifunktsioonid. Selle rolli Cortana Analytics protsess on kiire prototüüpimine töötlemise funktsioonidest ja seadme õppe mudelid lubamine.

**Menüü** allpool näidisandmed erinevate salvestusruumi keskkonnas, kuidas kirjeldavate teemade lingid. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Selles ülesandes valimite on samm [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Laadi alla ja alla näidisandmed
1. Järgmine näidis Python kood bloobimälu teenuse kasutamise Azure'i bloobimälu alla laadida andmed: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. Andmeid lugeda Pandas andmete-raami faili üles laadida.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Down valimiga andmete abil soovitud `numpy`'s `random.choice` järgmiselt:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Nüüd saate töötada ülaltoodud andmete raami 1% valimi täpsemaks uurimiseks ja funktsioon genereerimine.

##<a name="heading"></a>Andmete üles- ja lugeda Azure seadme õpetused

Järgmine proovi kood abil saate down valimit andmeid ning kasutada seda otse Azure'i ML:

1. Kirjutage andmete paneeli kohalik fail.

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Kohaliku faili üleslaadimine on Azure'i bloobimälu abil järgmine kood:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Lugege andmed: Azure'i bloobimälu, kasutades Azure ML [Andmete importimine](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , nagu on näidatud järgmisel pildil.
 
![lugeja bloobimälu](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
