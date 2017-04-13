<properties 
   pageTitle="Azure'i Lake andmeanalüüsi Azure'i käsurea liidest kasutades alustamine | Microsoft Azure'i" 
   description="Saate teada, kuidas kasutada Azure käsurea liides Lake andmesalve konto loomine, looge Lake andmeanalüüsi töö U-SQL-i abil ja esitage töö. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Õpetus: alustamine Azure'i andmed Lake Andmekaevandustööriistade abil Azure'i käsurea liides (CLI)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Saate teada, kuidas kasutada Azure CLI Azure'i andmeanalüüsi Lake kontode loomine, määratlemine Lake andmeanalüüsi töö [U-SQL-i](data-lake-analytics-u-sql-get-started.md)ja edastab tööd andmete Lake Analyticsi kontod. Andmeanalüüsi Lake kohta leiate lisateavet teemast [Azure andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md).

Selles õppetükis saate välja tööd, mis loeb vahekaardi eraldatud väärtuste (TSV) fail ja teisendab komaeraldusega väärtuste (CSV) faili. Läbida samas õpetuse muude toetatud tööriistade abil, klõpsake selle jaotise ülaosas vahekaarte.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- **Azure'i CLI**. Vt [installida ja konfigureerida Azure CLI](../xplat-cli-install.md).
    - Alla ja installige see demo lõpuleviimiseks **väljalaske-eelse** [Azure'i CLI tööriistad](https://github.com/MicrosoftBigData/AzureDataLake/releases) .
- **Autentimist**kasutades järgmine käsk:

        azure login
    Autentimisel töökoha või kooli kontoga kohta leiate lisateavet teemast [Azure tellimuse: Azure'i CLI ühenduse loomine](../xplat-cli-connect.md).
- **Azure'i ressursihaldur režiimi aktiveerimine**, kasutades järgmine käsk:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Andmeanalüüsi Lake konto loomine

Saate kasutada mis tahes töö peab teil olema Lake andmeanalüüsi konto. Andmeanalüüsi Lake konto loomiseks peate määrama järgmist:

- **Azure'i ressursirühm**: A Lake andmeanalüüsi konto tuleb luua Azure'i ressursirühm sees. [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) võimaldab teil töö ressurssidega oma rakenduse rühmana. Saate juurutada, värskendamine või kustutada kõik ressursse rakenduse ühe koordineeritud kasutusel.  

    Loendada ressursi rühmad teie tellimus:
    
        azure group list 
    
    Uue ressursirühma loomiseks tehke järgmist.

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Andmete Lake Analytics konto nimi**
- **Asukoht**: üks Azure andmekeskuste, mis toetab Lake andmeanalüüsi.
- **Vaikimisi andmete Lake konto**: iga Lake andmeanalüüsi konto on vaikimisi andmete Lake konto.

    Olemasolevate andmete Lake konto loendisse:
    
        azure datalake store account list

    Andmete Lake uue konto loomiseks tehke järgmist.

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Andmete Lake konto nimi peab sisaldama ainult väiketähti ja numbreid.



**Andmeanalüüsi Lake konto loomine**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Andmete Lake Kasutusanalüüsi kuvamine konto](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Andmeanalüüsi Lake konto nimi peab sisaldama ainult väiketähti ja numbreid.


## <a name="upload-data-to-data-lake-store"></a>Laadi andmed andmesalve Lake

Selles õpetuses töötlete mõne otsingu logid.  Otsingu log talletatud andmed Lake poe või Azure'i bloobimälu. 

Azure'i portaal sisaldab kasutajaliidese mõned näidisfailide andmete kopeerimiseks vaikekonto andmete Lake, mis sisaldavad logifaili otsimine. Vaadake teemat [ettevalmistamine lähteandmete](data-lake-analytics-get-started-portal.md#prepare-source-data) üles andmeid andmesalve Lake vaikekonto.

Kasutage cli kasutage failide üleslaadimiseks järgmine käsk:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Azure'i bloobimälu pääsete juurde ka Lake andmeanalüüsi.  Azure'i bloobimälu üles andmeid, vt [Azure'i CLI Azure Storage abil](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Esitage Lake andmeanalüüsi tööde haldamine

Andmeanalüüsi Lake töö kirjutada U-SQL-i keeles. A-SQL-i kohta leiate lisateavet teemast [Alustamine U-SQL-i keele](data-lake-analytics-u-sql-get-started.md) ja [U-SQL keele viide](http://go.microsoft.com/fwlink/?LinkId=691348).

**Luua skripti Lake andmeanalüüsi töö**

- Järgmise U-SQL skripti tekstifaili luua, ja salvestage tekstifail oma töökoha:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    A-SQL-skripti abil **Extractors.Tsv()**andmefailis loeb ja loob siis CSV-faili abil **Outputters.Csv()**. 
    
    Ärge muutke kaks teede kui lähtefail kopeerimine teise asukohta.  Kui see ei eksisteeri, loob Lake andmeanalüüsi väljund kausta.
    
    Lihtsam on kasutada suhtelised teed failid vaikimisi andmete Lake kontod. Samuti saate absoluutne teed.  Näide 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Failide lingitud salvestusruumi kontod juurdepääsuks peate kasutama absoluutne teed.  Lingitud Azure Storage konto talletatud failide süntaks on järgmine:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure'i bloobimälu container avaliku plekid või avaliku ümbriste juurdepääsuõigused pole praegu toetatud.      

    
**Esitada töö**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Loendis töö, töö üksikasjade ja tühistada töö saab järgmised käsud:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Pärast töö on lõpule viidud, saate faili järgmised cmdlet-käskude abil ja faili alla laadida.
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Vt ka

- Muude tööriistade abil sama õpetuse vaatamiseks klõpsake menüü lülitid lehe ülaosas.
- Keerukama päringu, leiate artiklist [analüüsi veebisaidi logid Azure'i Lake andmeanalüüsi abil](data-lake-analytics-analyze-weblogs.md).
- Alustamiseks U-SQL-i rakenduste arendamise, lugege teemat [arendada U-SQL-i skriptide abil andmete Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- A-SQL-is leiate teemast [Azure andmete Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md).
- Vaadake haldamise toiminguid, [Hallata Azure Lake andmeanalüüsi Azure'i portaalis](data-lake-analytics-manage-use-portal.md).
- Andmeanalüüsi Lake ülevaate saamiseks vt [Azure'i andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md).

