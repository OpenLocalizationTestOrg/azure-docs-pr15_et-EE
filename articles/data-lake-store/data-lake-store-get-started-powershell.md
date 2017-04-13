<properties
   pageTitle="Alustamine Lake andmesalve | Azure'i"
   description="Azure'i PowerShelli abil saate luua Lake andmesalve konto ja põhiliste toimingute"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Azure'i Lake andmesalve Azure PowerShelli kaudu alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saate teada, kuidas Azure'i PowerShelli abil saate luua konto Azure andmesalve Lake ja põhiliste toimingute näiteks luua kaustu, üles- ja allalaadimine andmefailid, kustutada oma konto jne. Lake andmesalve kohta leiate lisateavet teemast [Andmesalve Lake ülevaade](data-lake-store-overview.md).

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

* **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

* **Azure'i PowerShelli 1.0 või suurem**. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

## <a name="authentication"></a>Autentimine

Selles artiklis kasutab lihtsam autentimise lähenemine Lake andmesalve, kus teil palutakse sisestada mandaat Azure'i konto. Andmete Lake Store konto ja failisüsteemi reguleerib seejärel juurdepääsutase sisseloginud kasutaja juurdepääsutase. Siiski on veel mitu ka Lake andmesalve kinnitamiseks **lõppkasutaja autentimist** või **teenusest autentimist**. Juhised ja autentida Lisateavet leiate teemast [andmete Lake poe autentimise abil Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Azure'i andmesalve Lake konto loomine

1. Oma töölaualt avamine uues aknas Windows PowerShelli ja sisestage järgmine koodilõigu Azure'i kontosse sisse logida, määrake tellimus ja registreerida Lake andmesalve pakkuja. Kui teil palutakse sisse logida, veenduge, et logite ühe tellimuse admininistrators või omanik.

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Azure'i andmesalve Lake konto on seostatud mõne Azure'i ressursirühma. Alustuseks on Azure ressursirühm loomine.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure'i ressursi rühma loomine] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure'i ressursi rühma loomine")

2. Looge konto Azure andmesalve Lake. Teie määratud nimi peab sisaldama ainult väiketähti ja numbreid.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure'i andmesalve Lake konto loomine] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Azure'i andmesalve Lake konto loomine")

3. Veenduge, et konto loonud.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Väljundi see peaks olema **täidetud**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Oma Azure'i andmed Lake salvestada kataloogi struktuurid loomine

Saate luua kataloogide jaotises konto Azure andmesalve Lake haldamiseks ja andmete talletamiseks.

1. Määrake soovitud juurkaust.

        $myrootdir = "/"

2. Looge uus kaust nimega **mynewdirectory** alusel määratud juurkausta.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Veenduge, et loodud uus kaust.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    See peaks kuva väljund umbes selline:

    ![Veenduge, et kataloogi] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Veenduge, et kataloogi")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Laadi andmed oma Azure'i andmesalve Lake

Saate üles laadida andmete Lake andmesalve otse juurtasemel või kontol loodud kausta. Allpool Koodilõigud näitavad, kuidas Näidisandmete üleslaadimine eelmises tööetapis loodud kataloogi (**mynewdirectory**).

Kui otsite Näidisandmete üles laadida, saate kausta **Kiirabi andmed** [Azure'i andmed Lake Git hoidla](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Faili alla laadida ja talletatakse teie arvutis, nt C:\sampledata kohalik kataloog\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Ümber nimetada, laadige alla ja kustutavad andmeid andmete Lake poest

Faili ümbernimetamine käsuga:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Saate alla laadida faili, kasutage järgmist käsku:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Faili kustutamine käsuga:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Küsimise korral sisestage **Y** üksuse kustutada. Kui teil on rohkem kui ühe faili kustutamiseks, saate sisestada kõik teed, mis on eraldatud komaga.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Azure'i andmesalve Lake konto kustutamine

Järgmise käsu abil saate kustutada Lake andmesalve kontole.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Küsimise korral sisestage **Y** konto kustutada.


## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
