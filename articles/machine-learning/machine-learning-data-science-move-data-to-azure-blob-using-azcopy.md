<properties
    pageTitle="Andmete teisaldamine ja sealt Azure'i bloobimälu abil AzCopy | Microsoft Azure'i"
    description="Andmete teisaldamine ja sealt abil AzCopy Azure'i bloobimälu"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Andmete teisaldamine ja sealt abil AzCopy Azure'i bloobimälu

AzCopy on mõeldud üles, alla ja andmete kopeerimine ja Microsoft Azure'i bloobimälu faili ja tabelimälu käsurea kasuliku.

Juhised AzCopy ja täiendava teabe installimine funktsiooniga Azure'i platvormi, vt [AzCopy käsurea kasuliku töötamise alustamine](../storage/storage-use-azcopy.md).

Juhised kasutatavaid tehtud Azure'i bloobimälu andmete teisaldamiseks on lingitud siit:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Kui kasutate VM, mis loodi esitatud [andmed teadus virtuaalse masinad Azure](machine-learning-data-science-virtual-machines.md)skriptide, siis AzCopy on juba installitud VM.

> [AZURE.NOTE] Azure'i bloobimälu täieliku tutvustus leiate [Azure'i bloobimälu põhialused](../storage/storage-dotnet-how-to-use-blobs.md) ja [Azure'i bloobimälu teenus](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Eeltingimused

Selle dokumendi eeldab, et teil on Azure tellimuse, salvestusruumi konto ja salvestusruumi vastava klahvi konto jaoks. Enne üles/alla andmeid, peate teadma oma Azure storage konto nimi ja konto võti.

- Azure'i tellimuse vaadake [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- Juhised salvestusruumi konto loomine ja konto ja olulise teabe saamiseks vt [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>AzCopy käsud

AzCopy käskude käivitamiseks avage käsuviiba aken ja liikuge arvutis, kus asub käivitatava AzCopy.exe AzCopy installikaust. 

Põhilised käsud AzCopy süntaks on:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Saate lisada oma tee AzCopy installi asukoht ja seejärel käivitage käsud, mis tahes kataloogist. Vaikimisi installitakse AzCopy *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* või *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Failide üleslaadimine on Azure'i bloobimälu

Faili üleslaadimine käsuga:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Faile alla laadida ka Azure'i bloobimälu

Faili alla laadida ka Azure'i bloobimälu, kasutage järgmist käsku:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Azure'i ümbriste plekid vahetada

Azure'i ümbriste plekid vahetada, kasutage järgmist käsku:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Näpunäiteid AzCopy

> [AZURE.TIP]   
> 1. Kui lisatud failide **üleslaadimise** */S* faile rekursiivselt. Selle parameetri ilma failid alamkatalooge ei üles.  
> 2. Faili **allalaadimine** */S* otsib container rekursiivselt, kuni kõik failid määratud kausta ja selle alamkatalooge või kõik failid, mis vastavad määratud mustrile antud kataloogi ja selle alamkatalooge, laaditakse.  
> 3.  Te ei saa määrata **teatud bloobimälu faili** alla laadida parameetriga *statistikast/allikas* . Allalaadimiseks mingit kindlat tüüpi failiga, määrake allalaadimiseks */Pattern* parameetriga bloobimälu faili nimi. **/S** parameetrit saab kasutada on AzCopy faili nimi mustri rekursiivselt otsida. Ilma mustri parameetrit AzCopy allalaadimist kõik selle kausta faile.
