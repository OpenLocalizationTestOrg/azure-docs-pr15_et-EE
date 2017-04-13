<properties
    pageTitle="Andmete teisaldamine ja sealt Azure'i bloobimälu abil Python | Microsoft Azure'i"
    description="Andmete teisaldamine ja sealt kasutades Python Azure'i bloobimälu"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Andmete teisaldamine ja sealt kasutades Python Azure'i bloobimälu

Selles teemas kirjeldatakse, kuidas loendi-, üles- ja allalaadimine plekid Python API abil. Python API lähtutud Azure'i SDK, saate teha järgmist.

- Ümbris loomine
- Ümbris laadida soovitud bloobimälu
- Laadige alla plekid
- Loendi a ümbrises plekid
- Kustutamine on bloobimälu

Python API kasutamise kohta lisateabe saamiseks vaadake, [Kuidas kasutada bloobimälu salvestusteenus Python kaudu](../storage/storage-python-how-to-use-blob-storage.md).

Juhised kasutatavaid tehtud Azure'i bloobimälu andmete teisaldamiseks on lingitud siit:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Kui kasutate VM, mis loodi esitatud [andmed teadus virtuaalse masinad Azure](machine-learning-data-science-virtual-machines.md)skriptide, siis AzCopy on juba installitud VM.

> [AZURE.NOTE] Azure'i bloobimälu täieliku tutvustus leiate [Azure'i bloobimälu põhialused](../storage/storage-dotnet-how-to-use-blobs.md) ja [Azure'i bloobimälu teenus](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Eeltingimused

Selle dokumendi eeldab, et teil on Azure tellimuse salvestusruumi konto ja salvestusruumi vastava klahvi konto jaoks. Enne üles/alla andmeid, peate teadma oma Azure storage konto nimi ja konto võti.

- Azure'i tellimuse vaadake [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- Juhised salvestusruumi konto loomine ja konto ja olulise teabe saamiseks vt [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Laadi andmed bloobimälu

Lisage järgmised koodilõigu Python kood, kus soovite programmiliselt juurde Azure Storage ülaservas.

    from azure.storage.blob import BlobService

Objekti **BlobService** võimaldab töötada ümbriste ja plekid. Järgmine kood loob BlobService objekti abil salvestusruumi konto nimi ja konto võti. Konto nimi ja konto võti asendada oma real konto ja võti.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Kasutada andmete üles soovitud bloobimälu järgmistest viisidest:

1. sellele\_Blokeeri\_bloobimälu\_kaudu\_tee (lisatud määratud tee faili sisu)
2. sellele\_block_blob\_kaudu\_faili (lisatud sisu juba avatud faili/voo kaudu)
3. sellele\_Blokeeri\_bloobimälu\_kaudu\_baiti (lisatud on massiiv baiti)
4. sellele\_Blokeeri\_bloobimälu\_kaudu\_tekst (lisatud abil määratud kodeeringut määratud tekstiväärtuse)

Järgmine proovi kood lisatud ümbris kohalik fail:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Järgmine proovi kood, laaditakse kõik failid (välja arvatud kataloogide) bloobimälu kohaliku kataloogis:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Andmete allalaadimine: bloobimälu

Kasutada alla laadida andmete lisamine bloobimälu järgmistest viisidest:
1. saada\_bloobimälu\_et\_tee
2. saada\_bloobimälu\_et\_fail
3. saada\_bloobimälu\_et\_baiti
4. saada\_bloobimälu\_et\_tekst

Need meetodid, mida teha, kui andmete maht ületab 64 MB vajalikud murenemist.

Järgmine proovi kood allalaadimist kohaliku faili sisu lisamine bloobimälu nõus:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Järgmine proovi kood allalaadimist kõik plekid pakendist. Loendi kasutab\_saada loendis Saadaolevad plekid ümbrises plekid ja laadib neid kohalikku kausta.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
