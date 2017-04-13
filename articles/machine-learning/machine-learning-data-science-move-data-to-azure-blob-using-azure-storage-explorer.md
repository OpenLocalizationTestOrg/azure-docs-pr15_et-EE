<properties 
    pageTitle="Andmete teisaldamine ja sealt Azure'i bloobimälu Azure'i salvestusruumi Exploreriga | Microsoft Azure'i" 
    description="Andmete teisaldamine ja sealt Azure'i salvestusruumi Exploreriga Azure'i bloobimälu" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Andmete teisaldamine ja sealt Azure'i salvestusruumi Exploreriga Azure'i bloobimälu

Azure'i salvestusruumi Explorer on tasuta tööriista Microsoft, mis võimaldab töötada Azure'i andmed, mille Windows, macOS ja Linux. Selles teemas kirjeldatakse, kuidas selle abil üles- ja allalaadimine: Azure'i bloobimälu andmed. Tööriista saate alla laadida [Microsoft Azure'i salvestusruumi Explorer](http://storageexplorer.com/).

Juhised kasutatavaid tehtud Azure'i bloobimälu andmete teisaldamiseks on lingitud siit:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Kui kasutate VM, mis loodi esitatud [andmed teadus virtuaalse masinad Azure](machine-learning-data-science-virtual-machines.md)skriptide, siis Azure salvestusruumi Explorer on juba installitud VM.
 
> [AZURE.NOTE] Azure'i bloobimälu täieliku tutvustus leiate [Azure'i bloobimälu põhitõed](../storage/storage-dotnet-how-to-use-blobs.md) ja [Azure'i bloobimälu teenus](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Eeltingimused

Selle dokumendi eeldab, et teil on Azure tellimuse salvestusruumi konto ja salvestusruumi vastava klahvi konto jaoks. Enne üles/alla andmeid, peate teadma oma Azure storage konto nimi ja konto võti. 

- Azure'i tellimuse vaadake [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- Juhised salvestusruumi konto loomine ja konto ja olulise teabe saamiseks vt [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md). Märkige üles kiirklahv salvestusruumi konto jaoks, nagu see võti Azure'i salvestusruumi Exploreri tööriistaga selle kontoga ühenduse loomiseks peate.
- Azure'i salvestusruumi Exploreri tööriista saab alla laadida [Microsoft Azure'i salvestusruumi Explorer](http://storageexplorer.com/). Aktsepteerige vaikesätted installi ajal.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Azure'i salvestusruumi Exploreriga 

Järgmised toimingud dokument üles/alla laadida Azure'i salvestusruumi Exploreriga andmete kohta. 

1.  Käivitage Microsoft Azure'i salvestusruumi Explorer.
2.  Avab viisardi **... kontosse sisse logida** , valige **Azure'i konto** sätted, seejärel **Lisa konto** ja teil sisestada mandaat. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Avab **Azure Storage ühenduse loomine** viisardi, valige **Azure storage ühenduse** ikoon. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Sisestage kiirklahv Azure storage konto **ühenduse Azure Storage** viisard ja seejärel **edasi**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Sisestage salvestusruumikonto nimi väljale **konto nimi** ja seejärel valige **edasi**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Nüüd olema lisatud salvestusruumi lisatud kontole. Salvestusruumi konto bloobimälu container loomiseks paremklõpsake sõlme **Bloobimälu ümbriste** selle konto, valige **Loomine bloobimälu ümbris**ja sisestage nimi.
7. Andmete üleslaadimiseks ümbris valige siht-ümbrisest ja klõpsake nuppu **üles laadida** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klõpsake **…** välja **failide** paremal, valige üks või mitu faili failisüsteemi kaudu üles laadida ja **üles laadida** failide üleslaadimise alustamiseks klõpsake.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Andmed, valides soovitud bloobimälu vastavate ümbrises allalaadimiseks ja klõpsake nuppu **Laadi alla**laadida. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


