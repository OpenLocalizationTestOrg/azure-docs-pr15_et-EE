<properties
    pageTitle="Andmete teisaldamine ja sealt Azure'i bloobimälu | Microsoft Azure'i"
    description="Andmete teisaldamine ja sealt Azure'i bloobimälu"
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
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Andmete teisaldamine ja sealt Azure'i bloobimälu

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Juhised kasutatavaid tehtud Azure'i bloobimälu andmete teisaldamiseks on lingitud siit:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Millist meetodit on teie jaoks sobivaim oleneb stsenaariumist. [Täiustatud analüüsi Azure seadme õppe stsenaariumid](machine-learning-data-science-plan-sample-scenarios.md) artikkel aitab määratleda ressursid, peate andmete teadus töövoogude täiustatud analüüsi käigus kasutada mitmesuguseid.

> [AZURE.NOTE] Azure'i bloobimälu täieliku tutvustus leiate [Azure'i bloobimälu põhialused](../storage/storage-dotnet-how-to-use-blobs.md) ja [Azure'i bloobimälu teenus](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Teise võimalusena saate kasutada [Azure'i andmed Factory](https://azure.microsoft.com/services/data-factory/) . 

- loomine ja müügivõimaluste, mis laadib andmed Azure'i bloobimälu, plaanimine 
- andke seda avaldatud Azure seadme õ veebiteenuse, 
- Ennustav tulemused, vastuvõtmine ja 
- tulemite salvestusruumi üles laadida. 

Lisateabe saamiseks vt [loomine sõnastikupõhise torujuhtmete Azure'i andmed Factory ja Azure seadme õ abil](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Eeltingimused

Selle dokumendi eeldab, et teil on Azure tellimuse salvestusruumi konto ja salvestusruumi vastava klahvi konto jaoks. Enne üles/alla andmeid, peate teadma oma Azure storage konto nimi ja konto võti.

- Azure'i tellimuse vaadake [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- Juhised salvestusruumi konto loomine ja konto ja olulise teabe saamiseks vt [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md).
