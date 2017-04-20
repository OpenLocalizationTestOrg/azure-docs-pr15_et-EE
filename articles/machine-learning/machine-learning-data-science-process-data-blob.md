<properties 
    pageTitle="Azure Kämp andmeid täiustatud Analytics | Microsoft Azure" 
    description="Andmete töötlemise Azure bloobimälu." 
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

#<a name="heading"></a>Täiustatud Analytics andmete töötlemise Azure Kämp

Käesolevas dokumendis käsitletakse Avastades andmed ja andmed salvestatakse Azure bloobimälu teeniva funktsioone. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Laadida andmeid Pandas andmed raami
Et avastada ja manipuleerida andmekogumit, peate laadida allikast Kämp kohaliku faili, mida saab seejärel laadida Pandas andmed raami. Siin on selle toimingu jaoks tehke järgmist:

1. Lae andmed Azure Bloobivahemälu koos Python näidiskoodis Kämp teenust kasutades. Asenda oma eriväärtuste muutuja kood allpool: 

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


2. Pandas andmed-raami allalaaditud failist andmeid lugeda.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nüüd olete valmis, saate andmeid ja luua tuvastatavate funktsioonid.


##<a name="blob-dataexploration"></a>Andmete uurimine

Siin on mõned näited Pandas abil andmete uurimiseks:

1. Kontrollida ridade ja veergude arv 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Kontrollida ees- või mõned read andmekogumis alljärgnevalt:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Kasutades näidiskoodis imporditi iga veeru andmetüübi kontrollimine.
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Kontrollige põhilised stats esitatud andmete veerud järgmiselt.
 
        dataframe_blobdata.describe()
    
5. Vaata kirjete iga veeru väärtus järgmiselt

        dataframe_blobdata['<column_name>'].value_counts()

6. Krahv puuduvate väärtuste arv igas veerus kasutades näidiskoodis versus

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Kui teil puuduvad väärtused kindlas veerus andmed, lohistad neid järgmiselt:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Teine võimalus asendada puuduvad väärtused on mode funktsioon:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Luua histogrammi krundi kasutamise aluste arv kanda muutuja jaotus 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Vaata korrelatsioonide vahel muutujad on scatterplot või sisseehitatud korrelatsioonifunktsiooni

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Funktsioon põlvkonna
    
Me saame luua funktsioone kasutades Python järgmiselt:

###<a name="blob-countfeature"></a>Näitaja väärtus vastavalt funktsiooni põlvkonna

Kategooriline funktsioone saab luua järgmiselt:

1. Kontrollida kategooriline veeru jaotus:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Näitajate väärtusi luua iga veeru väärtuse

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Liitu näitaja veeru andmete algne raam 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Eemaldada originaali muutuja, ise:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Funktsioon põlvkonna binning

Tekitama binned funktsioonid, me järgmisel viisil:

1. Lisada veerge bin arvuveerg jada
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Teisendada binning on boolean muutujate jada

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Lõpetuseks Liitu näiva muutujate naasta andmete Algne raami

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Andmete kirjutamise tagasi Azure Kämp ja Azure'i masinõpet tarbivad

Pärast proovimist ei andmed ja loodud vajalikke funktsioone, võite saata andmed (proovid või featurized) on Azure Bloobivahemälu ja ta tarbib Azure'i masinõppe alltoodud: pange tähele lisavõimalusi loomist Azure masin õppe Studio samuti. 
1. Kohaliku faili kirjutada andmeid raam

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Üles andmeid Azure Kämp järgmiselt:

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

3. Nüüd andmete lugemiseks Azure'i masinõppe [Impordiandmete] Kämp Action[ import-data] moodul, nagu on näidatud ekraani alla:
 
![lugeja Kämp][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
