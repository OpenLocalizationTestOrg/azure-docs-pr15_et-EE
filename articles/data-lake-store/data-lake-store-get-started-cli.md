<properties
   pageTitle="Alustamine Lake andmesalve platvormidel käsurea liides abil | Microsoft Azure'i"
   description="Azure'i platvormidel käsurea abil luua Lake andmesalve konto ja põhiliste toimingute"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Azure'i Lake andmesalve Azure'i käsurea kaudu alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saate teada, kuidas Azure'i käsurea liides abil saate luua konto Azure andmesalve Lake ja põhiliste toimingute näiteks luua kaustu, üles- ja allalaadimine andmefailid, kustutada oma konto jne. Lake andmesalve kohta leiate lisateavet teemast [Andmesalve Lake ülevaade](data-lake-store-overview.md).

Azure'i CLI rakendatakse Node.js. Seda saab kasutada mis tahes platvorm, mis toetab Node.js, sh Windowsi, Maci ja Linux. Azure'i CLI on avatud. Lähtekoodi hallatakse GitHub, <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>juures. Selles artiklis käsitletakse ainult Lake andmesalve Azure'i CLI abil. Üldised juhised, kuidas kasutada Azure CLI, leiate [Azure'i CLI kasutamine] [azure-command-line-tools].


##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Azure'i CLI** - vt [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) installi ja konfiguratsiooni teavet. Veenduge, et te taaskäivitage arvuti pärast installimist CLI.

## <a name="authentication"></a>Autentimine

Selles artiklis kasutab lihtsam autentimise lähenemine Lake andmesalve, kui logite kasutajana lõppkasutaja. Andmete Lake Store konto ja failisüsteemi reguleerib seejärel juurdepääsutase sisseloginud kasutaja juurdepääsutase. Siiski on veel mitu ka Lake andmesalve kinnitamiseks **lõppkasutaja autentimist** või **teenusest autentimist**. Juhised ja autentida Lisateavet leiate teemast [andmete Lake poe autentimise abil Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Azure'i tellimuse sisse logida

1. Järgige juhiseid dokumenteerida [Azure tellimuse: Azure'i käsurea liides (Azure'i CLI) ühenduse](../xplat-cli-connect.md) ja ühenduse oma tellimuse abil soovitud `azure login` meetod.

2. Loend tellimused, mis on seotud teie konto abil soovitud `azure account list` käsk.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Eespool nimetatud väljund **Azure-sub-1** on praegu lubatud ja muude tellimus on **Azure-sub-2**. 

3. Valige tellimus, millega soovite töötada, klõpsake jaotises. Kui soovite töötada jaotises tellimuse Azure-sub-2, kasutage funktsiooni `azure account set` käsk.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Azure'i andmesalve Lake konto loomine

Avage käsuviip, shell või terminal seanss ja käivitage järgmised käsud.

2. Lülitumine Azure'i ressursihaldur abil järgmine käsk:

        azure config mode arm


5. Saate luua uue ressursirühma. Sisestage järgmine käsk parameetrite väärtused, mida soovite kasutada.

        azure group create <resourceGroup> <location>

    Kui asukoha nimes on tühikud, pange see jutumärgid. Näiteks "Ida-USA 2".

5. Looge konto Lake andmesalve.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Kaustade loomine andmete Lake poes

Saate luua kaustu jaotises konto Azure andmesalve Lake haldamiseks ja andmete talletamiseks. Looge kaust nimega "mynewfolder" Lake andmesalve juurtasemel järgmise käsu abil.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Näiteks:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Laadi andmed teie andmesalve Lake

Saate üles laadida andmete Lake andmesalve otse juurtasemel või kausta, mis on loodud kontol. Allpool Koodilõigud näitavad, kuidas üles laadida Näidisandmete eelmises tööetapis loodud kausta (**mynewfolder**).

Kui otsite Näidisandmete üles laadida, saate kausta **Kiirabi andmed** [Azure'i andmed Lake Git hoidla](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Faili alla laadida ja talletatakse teie arvutis, nt C:\sampledata kohalik kataloog\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Näiteks:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Lake andmesalve failide loend

Kasutage järgmist käsku faili Lake andmesalve konto.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Näiteks:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Väljund see peaks olema umbes järgmine:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Ümber nimetada, laadige alla ja kustutavad andmeid andmete Lake poest

* **Faili ümber nimetada**, kasutage järgmist käsku:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Näiteks:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Alla laadida faili**, kasutage järgmist käsku. Veenduge, et teie juba määratud sihtkohta tee on olemas.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Näiteks:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Faili kustutamine**, kasutage järgmist käsku:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Näiteks:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Küsimise korral sisestage **Y** üksuse kustutada.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Vaadata kausta pääsuloendi andmesalve Lake

Järgmise käsu abil saate vaadata selle ACL-ID Lake andmesalve kausta. Praeguses versioonis saab ACL määrata ainult andmed Lake salve. Jah, alati on parameeter path root (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Näiteks:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Lake andmesalve konto kustutamine

Järgmise käsu abil saate kustutada Lake andmesalve konto.

    azure datalake store account delete <dataLakeStoreAccountName>

Näiteks:

    azure datalake store account delete mynewdatalakestore

Küsimise korral sisestage **Y** konto kustutada.


## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
