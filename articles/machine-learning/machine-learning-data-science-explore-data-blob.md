<properties 
    pageTitle="Azure'i bloobimälu koos Pandas andmete uurimine | Microsoft Azure'i" 
    description="Kuidas uurida andmeid, mis on talletatud Azure'i bloobimälu container Pandas abil." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Azure'i bloobimälu koos Pandas andmete uurimine

Selles dokumendis antakse ülevaade andmete, mis on talletatud Azure'i bloobimälu container [Pandas](http://pandas.pydata.org/) Python pakett uurimine.

**Menüü** viivat teemadele, kus kirjeldatakse, kuidas erinevate salvestusruumi keskkonnas: andmete uurimine tööriistade abil. See toiming on [Andmete teadus protsess]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on:

* Loodud Azure storage konto. Kui vajate juhiseid, lugege teemat [Azure Storage konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account)
* Salvestatud andmete Azure'i bloobimälu salvestusruumi konto. Kui vajate juhiseid, lugege teemat [liikumine andmete ja sealt Azure Storage](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Pandas DataFrame andmete laadimine
Analüüsimine ja töödelda andmekomplekt, peate esmalt laadida lähtekoha bloobimälu kohaliku faili, mille saab seejärel laadida Pandas DataFrame. Siin on tehke selles protseduuris järgmist.

1. Azure'i andmed Bloobivahemälu koos järgmine Python kood näidis bloobimälu teenuse allalaadimine. Järgmine kood muutuja asendamine oma kindlad väärtused: 

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

##<a name="blob-dataexploration"></a>Andmete uurimine Pandas kasutamise näited

Siin on mõned näited viisid andmete uurimine Pandas abil.

1. **Ridade ja veergude arv** uurimine 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Kontrolli** esimese või viimase mõned **read** järgmine andmekomplekti:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Märkige ruut iga veeru imporditud, kasutades järgmist proovi kood **andmetüüp**
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. **Lihtsa statistika** jaoks andmehulga veerud kontrollida järgmiselt.
 
        dataframe_blobdata.describe()
    
5. Vaadake iga veeru väärtus kirjete arv järgmiselt.

        dataframe_blobdata['<column_name>'].value_counts()

6. **Puuduvate väärtuste loendamine** võrreldes kirjete järgmine proovi kood iga veeru tegelik arv

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Kui teil on vastava veeru **puuduvate väärtuste** andmeid, saate kukutage need järgmiselt:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Teine võimalus puuduvate väärtuste asendamine on funktsiooni režiim.
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. **Histogrammi** diagrammi, aluste arv abil saate diagrammile muutuja jaotuse loomine 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Vaadake **korrelatsiooni** vahel muutujate abil on scatterplot või sisseehitatud korrelatsiooni funktsiooni abil

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
