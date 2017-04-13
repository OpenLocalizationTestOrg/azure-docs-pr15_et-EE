<properties
    pageTitle="Azure'i bloobimälu andmed, mille abil Panda funktsioonid loomine | Microsoft Azure'i"
    description="Kuidas luua andmeid, mis on talletatud Azure'i bloobimälu container Panda Python paketi funktsioone."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Azure'i bloobimälu andmed, mille abil Panda funktsioonid loomine

Selle dokumendi näitab, kuidas luua andmete, mis on talletatud Azure'i bloobimälu container [Pandas](http://pandas.pydata.org/) Python pakett funktsioone. Pärast liigendamine kohta andmete laadimine Panda andmete raami, see näitab, kuidas luua kategooriline funktsioonide Python skriptide abil indikaator väärtused ja binning funktsioone.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]See **menüü** erinevaid funktsioone andmete loomist kirjeldavate teemade lingid. See toiming on juhises [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Eeltingimused

Selles artiklis eeldatakse, et olete loonud Azure'i bloobimälu salvestusruumi konto ja teie andmed on salvestatud. Kui teil on vaja konto häälestamise juhiseid teemast [Azure Storage konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Andmete laadimine Pandas andmete raami
Uurimine ja töödelda andmekomplekt, et peate laadida lähtekoha bloobimälu kohaliku faili, mis saab siis laadida raamis Pandas andmed. Siin on tehke selles protseduuris järgmist.

1. Azure'i andmed Bloobivahemälu Järgmine näidis Python kood bloobimälu teenuse allalaadimine. Asendage kood all muutuja teie määratud väärtusest:

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


2. Andmeid lugeda Pandas andmete-raami allalaaditud faili.

        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nüüd olete valmis, saate andmeid ja luua selle andmehulga funktsioonid.

##<a name="blob-featuregen"></a>Funktsioon genereerimine

Järgmistes näitab, kuidas luua indikaator väärtustega kategooriline funktsioonid ja binning funktsioonid Python skriptide abil.

###<a name="blob-countfeature"></a>Indikaatori väärtus vastavalt funktsiooni genereerimine

Kategooriline funktsioone saab luua järgmiselt:

1. Kontrolli kategooriline veeru jaotuse:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Luua indikaator väärtused iga veeru väärtused

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Liitumine algse andmete paneeli veeru tähis

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Algne muutuja ise eemaldamiseks tehke järgmist.

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Funktsioon genereerimine binning

Binned funktsioonide loomiseks soovime järgmisel viisil:

1. Jada aluse arvuveerg veergude lisamine

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Teisendamine on kahendväärtus muutujate jada binning

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Lõpuks liituda fiktiivsete muutujate tagasi algse andmete paneeli

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Azure'i bloobimälu tagasi andmete kirjutamise ja Azure seadme õppe tarbimine

Pärast proovimist ei andmed ja vajalikud funktsioonid loonud, saate laadida andmed (valimisse või featurized) on Azure Bloobivahemälu ja ta tarbib Azure seadme õ täitke järgmised juhised: Märkus lisafunktsioone loomist Azure seadme õ Studios ka.
1. Kirjutage andmete paneeli kohalik fail.

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Laadi andmed Azure'i bloobimälu järgmiselt:

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
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Nüüd saate andmeid lugeda bloobimälu, nagu on näidatud allpool Kuva Azure seadme õ [Andmete importimine](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) mooduli kasutamise:

![lugeja bloobimälu](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
