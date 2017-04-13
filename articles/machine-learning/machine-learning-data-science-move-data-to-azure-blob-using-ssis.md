<properties
    pageTitle="Andmete teisaldamine või: Azure'i bloobimälu SSIS konnektorite abil | Microsoft Azure'i"
    description="Andmete teisaldamine või sealt Azure'i bloobimälu SSIS konnektorite abil."
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

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Andmete teisaldamine või sealt Azure'i bloobimälu SSIS konnektorite abil

[SQL Server Integration Services Feature Packi Azure](https://msdn.microsoft.com/library/mt146770.aspx) pakub andmete protsessi andmed salvestatakse Azure'i Azure ja kohapealse andmeallikate vahel ühenduse Azure'i komponendid.

Juhised kasutatavaid tehtud Azure'i bloobimälu andmete teisaldamiseks on lingitud siit:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Kui kliendid on teisaldatud kohapealsed andmed pilve pääsevad mis tahes Azure'i teenuse võimendada Azure tehnoloogiad komplekti täielik õigus. See võib kasutada näiteks Azure seadme õ või mõne Hdinsightiga kobar.

See on tavaliselt tuleb kõigepealt jaoks [SQL-i](machine-learning-data-science-process-sql-walkthrough.md) ja [Hdinsighti](machine-learning-data-science-process-hive-walkthrough.md) juhendavad tutvustused.

Arutelu kanoonilise stsenaariumi, mis SSIS ärivajaduste levinud hübriid integreerimine juhtudel tegemiseks kasutada, leiate [veel koos SQL Server Integration Services Feature Packi Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) ajaveebi.

> [AZURE.NOTE] Azure'i bloobimälu täieliku tutvustus leiate [Azure'i bloobimälu põhialused](../storage/storage-dotnet-how-to-use-blobs.md) ja [Azure'i bloobimälu teenus](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Eeltingimused

Selles artiklis kirjeldatud toimingute, peab teil olema Azure tellimuse ja Azure storage konto häälestanud. Teate oma Azure storage konto nimi ja konto võti üles ega alla laadida andmeid.

- **Azure'i tellimus**on häälestatud, lugege teemat [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- Juhised **salvestusruumi konto** loomine ja konto ja olulise teabe saamiseks vt [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md).


**SSIS-i konnektorid**kasutamiseks tuleb alla laadida

- **SQL Server 2014 või 2016 Standardne (või uuem)**: installi sisaldab SQL serveri Integreerimisteenused.
- **Microsoft SQL Server 2014 või 2016 Integration Services Paketita Azure**: neid saab alla laadida, [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) ja [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) lehtedelt.

> [AZURE.NOTE] SSIS SQL serveriga on installitud, kuid pole kaasatud Express versioon. Millised rakendused on kaasatud SQL serveri eri versioonide kohta lisateabe saamiseks vt [SQL serveri versioonide](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Koolitusmaterjalid SSIS-i kohta, leiate [SSIS käed kohta koolitus](http://www.microsoft.com/download/details.aspx?id=20766)

Teavet, kuidas saada üles-töötab abil luua lihtsa eraldamine, teisendamiseks ja laadi (ETL) paketid, vt SISS [SSIS-i õpetus: lihtsa ETL paketi loomine](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Laadige alla NYC takso andmekomplekti  
Käesolevas näites kirjeldatud siin kasutada avalikult kättesaadavaks andmekomplekti-- [NYC takso reisi](http://www.andresmh.com/nyctaxitrips/) andmekomplekti. Andmekomplekti koosneb umbes 173 miljonit taksosõidud NYC aasta 2013. On kahte tüüpi andmetega: reisi üksikasjad andmed ja hind andmed. Nagu iga kuu faili, meil 24 faile kõikidest, millest igaüks on umbes 2GB tihendamata.


## <a name="upload-data-to-azure-blob-storage"></a>Laadi andmed Azure'i bloobimälu
Andmeid, kasutades funktsiooni SSIS-i funktsioonide pakett asutusesisesest Azure'i bloobimälu teisaldamiseks kasutame eksemplari [**Azure'i bloobimälu üleslaadimine tööülesande**](https://msdn.microsoft.com/library/mt146776.aspx), joonisel:

![konfigureerimine-andmete-teadus-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Siin kirjeldatakse tööülesande kasutab parameetrid:


Väli|Kirjeldus|
----------------------|----------------|
**AzureStorageConnection**|Saate määrata olemasoleva Azure Storage ühenduse Manager või loob uue, mis viitab Azure storage konto, mis viitab sellele, kus bloobimälu failid on majutatud.|
**BlobContainer**|Määrab nime bloobimälu ümbrisest reguleerivad nimega plekid üles laaditud faile.|
**BlobDirectory**|Määrab bloobimälu kausta nimega Blokeeri bloobimälu üleslaaditud faili talletuskoht. Bloobimälu kaust on virtuaalse hierarhiat. Kui soovitud bloobimälu juba olemas, i a asendatakse see.|
**LocalDirectory**|Saate määrata kohalikku kausta, mis sisaldab faile üles laadida.|
**Faili nimi**|Saate määrata failide mustriga määratud nime valimiseks filtri nimi. Näiteks MySheet\*.xls\* sisaldab faile nagu MySheet001.xls ja MySheetABC.xlsx|
**TimeRangeFrom/TimeRangeTo**|Saate määrata Vahemikufiltri aeg. Failide pärast *TimeRangeFrom* ja enne *TimeRangeTo* kaasatakse.|


> [AZURE.NOTE] **AzureStorageConnection** mandaadi peavad olema õige ja **BlobContainer** olemas enne ülekandmist proovitakse kohale toimetada.

## <a name="download-data-from-azure-blob-storage"></a>Andmete allalaadimine: Azure'i bloobimälu

Andmete allalaadimine: Azure'i bloobimälu asutusesisese SSIS salvestusruumi, kasutage [Azure'i bloobimälu üleslaadimine tööülesande](https://msdn.microsoft.com/library/mt146779.aspx)eksemplari.

##<a name="more-advanced-ssis-azure-scenarios"></a>Täpsemate SSIS-Azure'i stsenaariumid
Me märkus siin SSIS paketita võimaldab keerukamaid puhul saabuvaid pakendit tööülesannete koos. Näiteks bloobimälu andmed võib siseneda otse mõne Hdinsightiga kobar, mille väljund saanud alla tagasi on bloobimälu ja seejärel kohapealne salvestusruumi. SSIS-i saate kasutada taru ja siga töökohtade Hdinsightiga kobar, mis täiendavad SSIS konnektorite abil:

- Käivitage taru skripti on Windows Azure Hdinsightiga kobar SSIS-i abil, kasutage [Azure Hdinsightiga taru ülesanne](https://msdn.microsoft.com/library/mt146771.aspx).
- Käivitage siga skripti on Windows Azure Hdinsightiga kobar SSIS-i abil, kasutage [Azure Hdinsightiga siga ülesanne](https://msdn.microsoft.com/library/mt146781.aspx).
