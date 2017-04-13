<properties
    pageTitle="Koostada esimese andmete factory (REST) | Microsoft Azure'i"
    description="Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste abil andmete Factory REST API-ga."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Õpetus: Koostada oma esimese Azure'i andmed factory andmete Factory REST API abil
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-build-your-first-pipeline.md)
- [Azure'i portaal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShelli](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressursihaldur Mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-GA](data-factory-build-your-first-pipeline-using-rest-api.md)

Selles artiklis abil saate andmeid Factory REST API luua oma esimese Azure'i andmed factory.

## <a name="prerequisites"></a>Eeltingimused
- Lugege läbi [Õpetuse ülevaate](data-factory-build-your-first-pipeline.md) artikli ja täitke juhised **nõutav** .
- Teie arvutisse installida [Curl](https://curl.haxx.se/dlwiz/) . Saate kasutada tööriista CURL ülejäänud käskude luua andmete factory. 
- Järgige [selle artikli](../resource-group-create-service-principal-portal.md) juhiseid. 
    1. Nimega **ADFGetStartedApp** Azure Active Directory veebirakenduse loomine.
    2. Saate **Kliendi ID** ja **salajane võti**. 
    3. **Rentniku ID**hankimine. 
    4. Saate määrata rakenduse **ADFGetStartedApp** **Andmete Factory kaasautori** roll.  
- Installige [Azure'i PowerShelli](../powershell-install-configure.md).  
- Käivitage **PowerShelli** ja käivitage järgmine käsk. Azure'i PowerShelli lahti jätta selles õpetuses lõpuni. Kui sulgeda ja uuesti avada, peate käivitama käsu uuesti.
    1. Käivita **Logi sisse – AzureRmAccount** ja sisestage kasutajanimi ja parool, mida kasutate Azure portaali sisse logida.  
    2. Käivitage **Get-AzureRmSubscription** kõik selle konto tellimused kuvamiseks.
    3. Käivitage **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** valige tellimus, mida soovite töötada. Asendage **NameOfAzureSubscription** Azure tellimuse nime. 
3. Azure'i ressursirühm, nimega **ADFTutorialResourceGroup** , käivitage järgmine käsk on PowerShellis loomiseks tehke järgmist.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Osa selle õpetuse juhised Oletame, et kasutada nimega ADFTutorialResourceGroup ressursirühma. Kui kasutate muu ressursirühm, peate kasutama nime asemel ADFTutorialResourceGroup ressursirühma selles õpetuses.

## <a name="create-json-definitions"></a>JSON määratluste loomine
Saate luua JSON failide kaust, kus asub curl.exe jälgima. 

### <a name="datafactoryjson"></a>datafactory.JSON 
> [AZURE.IMPORTANT] Nimi peab olema globaalselt kordumatu ja nii, et soovite eesliite ja järelliite ADFCopyTutorialDF oleks kordumatu nimi. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Asendage **account_name** ja **accountkey** nimi ja Azure storage konto võti. Salvestusruumi Accessi tootenumbri hankimine leiate artiklist [vaate, kopeerimine ja uuesti luua salvestusruumi kiirklahvide](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Järgmises tabelis on ära toodud väljavõte kasutatakse atribuutide JSON kirjeldused:

| Atribuut | Kirjeldus |
| :------- | :---------- |
| Versioon | Määrab, et versioon on Hdinsightiga loodud olema 3,2. | 
| ClusterSize | Hdinsightiga kobar suurus. | 
| TimeToLive | Mis määrab jõudeaja jaoks Hdinsightiga kobar, see enne kustutamist. |
| linkedServiceName | Saate määrata salvestusruumi konto, mida kasutatakse Hdinsightiga loodud logide salvestamiseks |

Võtke arvesse järgmist. 

- Andmete Factory loob **Windowsi-põhiste** Hdinsightiga kobar teile eespool JSON. Te oleksite selle **Linuxi-põhiste** Hdinsightiga kobar loomine. Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Võite kasutada **oma Hdinsightiga kobar** asemel kuvatakse nõudmisel Hdinsightiga kobar. Üksikasjad leiate [Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- Hdinsightiga kobar loob **vaikimisi container** bloobimälu, määratud JSON (**linkedServiceName**). Klaster kustutamisel ei kustutata Hdinsightiga selles ümbrises. Selline käitumine on kujundus. Nõudmisel Hdinsightiga lingitud teenusega Hdinsightiga kobar luuakse iga kord, kui mõnda sektorit töötlemise, kui ei ole olemasoleva live kobar (**timeToLive**) ja kustutatakse kui töötlemine on lõpule jõudnud.

    Nagu töödeldakse veel sektoritele, näete oma Azure'i bloobimälu palju ümbriste. Kui te ei pea neid tõrkeotsingu tööd, võite kustutage need salvestusruumi kuidagi vähendada. Järgige neid ümbriste nimed mustri: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Näiteks [Microsoft salvestusruumi Exploreri](http://storageexplorer.com/) abil saate kustutada ümbriste Azure'i bloobimälus.

Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Funktsiooni JSON määratleb andmekomplekt nimega **AzureBlobInput**, mis tähistab sisendandmete tegevuse teel. Lisaks saate määrata, et sisendandmete asub bloobimälu ümbris nimega **adfgetstarted** ja kaust nimega **inputdata**.

Järgmises tabelis on ära toodud väljavõte kasutatakse atribuutide JSON kirjeldused:

| Atribuut | Kirjeldus |
| :------- | :---------- |
| tüüp | Atribuudi tüüp väärtuseks on seatud AzureBlob kuna andmete asub Azure'i bloobimälu. |  
| linkedServiceName | viitab StorageLinkedService, mis on varem loodud. |
| faili nimi | Atribuut pole kohustuslik. Kui argument seda atribuuti, kuvatakse folderPath kõik failid on peale. Sel juhul ainult input.log töödeldakse. |
| tüüp | Me kasutame tekstivorming on logifailide vormingus tekst. | 
| columnDelimiter | logifailide veerud on piiratud koma () |
| sagedus/intervall | määratud kuu ja intervalli sagedus on 1, mis tähendab, et Sisestuskeel sektorid on saadaval iga kuu. | 
| väliste | selle atribuudi väärtuseks on seatud väärtus true, kui sisendandmete on loodud andmete Factory teenus. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

Funktsiooni JSON määratleb nimega **AzureBlobOutput**, väljundi andmete tegevuse tulemas tähistava Andmekomplekt. Lisaks saate määrata tulemite talletatavad bloobimälu ümbris nimega **adfgetstarted** ja kaust nimega **partitioneddata**. Jaotises **saadavuse** määrab, et väljundi andmekomplekti valmistatakse kuude kaupa.

### <a name="pipelinejson"></a>Pipeline.JSON
> [AZURE.IMPORTANT] Asendage **storageaccountname** oma Azure storage konto nimi. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

JSON koodilõigu, loote kohaletoimetamisel, mis koosneb andmete töötlemise Hdinsightiga klaster taru kasutav üks tegevus.

Taru skriptifail **partitionweblogs.hql**, talletatakse Azure storage konto (määratud scriptLinkedService, mida nimetatakse **StorageLinkedService**) ja container **adfgetstarted** **skripti** kausta.

**Määratleb** jaotise määrab käitusaja sätteid, mis edastatakse taru skripti taru konfiguratsiooni väärtused (nt ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

**Algus** ja **lõpp** atribuutide tulemas määrab tulemas aktiivse perioodi.

Saate määrata tegevuse JSON taru skript käivitatakse Arvuta, on **linkedServiceName** – **HDInsightOnDemandLinkedService**määratud.

> [AZURE.NOTE] [Müügivõimaluste anatoomia](data-factory-create-pipelines.md#anatomy-of-a-pipeline) kohta vaadake teavet eelmises näites kasutatakse JSON atribuudid. 

## <a name="set-global-variables"></a>Määramine globaalsed muutujad

Azure'i PowerShellis käivitada pärast asendamine soovitud väärtused ise järgmised käsud:

> [AZURE.IMPORTANT] [Juhised saada kliendi ID, kliendi salajane – jututuba, lugege teemat grammatikavigu](#prerequisites) rentniku ID ja Tellimuse ID-ga.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Autentimiseks AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Andmete factory loomine

Selles etapis loote Azure'i andmed Factory **FirstDataFactoryREST**nimega. Andmete factory võib olla üks või mitu torujuhtmete. Müügivõimaluste võib olla üks või mitu tegevusi. Näiteks Kopeeri tegevuse andmete kopeerimine mõnest muust allikast sihtkoha andmete pood ja Hdinsightiga taru tegevuste käivitamiseks taru skripti andmete teisendamiseks. Käivitage järgmised käsud andmete factory loomiseks: 

1. Saate määrata muutuja nimega **cmd**käsk. 

    Kinnitage, andmete factory nimi määrake siin (ADFCopyTutorialDF) vastab määratud **datafactory.json**nimi. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Käivitage käsk **Invoke-käsu**abil.

        $results = Invoke-Command -scriptblock $cmd;
3. Tulemusi vaadata. Kui andmete factory on loodud, kuvatakse JSON andmete Factory **tulemused**; muul juhul kuvatakse tõrketeade.  

        Write-Host $results

Võtke arvesse järgmist.
 
- Azure'i andmed Factory nimi peab olema globaalselt kordumatu. Kui kuvatakse tõrketeade tulemuste: **andmete factory nimi "FirstDataFactoryREST" on pole saadaval**, tehke järgmist:  
    1. Muutke nime (nt yournameFirstDataFactoryREST) **datafactory.json** faili. Nimede reeglid andmete Factory artefakte [Andmete Factory - nime andmise reeglid](data-factory-naming-rules.md) teemat.
    2. Esimene käsk kui **$cmd** muutuja on määratud väärtuse, asendage FirstDataFactoryREST uus nimi ja käivitage käsk. 
    3. Käivitage järgmised kaks käsud kasutada REST API luua andmete factory ja toimingu tulemuste printimine. 
- Andmete Factory eksemplari loomiseks peate olema kaasautor/administraator Azure tellimuse
- Andmete factory nimi võib registreerida DNS-i nimi tulevikus ja seega muutuvad avalikult nähtav.
- Kui kuvatakse tõrketeade: "**see tellimus pole registreeritud kasutada nimeruumi Microsoft.DataFactory**", tehke ühte järgmistest ja proovige uuesti avaldada: 

    - Azure'i PowerShelli registreerida andmete Factory pakkuja järgmine käsk: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Saate käivitage järgmine käsk, et kinnitada, et andmete Factory, mis on registreeritud pakkuja: 
    
            Get-AzureRmResourceProvider
    - Azure'i tellimuse [Azure portaali](https://portal.azure.com) sisse login ja liikuge andmete Factory blade (või) andmete factory Azure portaali loomine. See toiming automaatselt registreerib pakkuja teie eest.

Müügivõimaluste loomist peate esmalt looma mõned andmed Factory üksused. Esmalt looge lingitud teenused link andmete poed/avaldis arvutab teie andmesalve, määratleda sisendi ja väljundi andmekomplektide andmete lingitud andmete esitamiseks. 

## <a name="create-linked-services"></a>Looge lingitud teenused 
Selles etapis tuleb teil link Azure Storage konto ja kuvatakse nõudmisel Windows Azure Hdinsightiga kobar oma andmete factory. Azure Storage konto hoiab selle valimi tulemas sisend- ja andmeid. Lingitud Hdinsightiga teenuse saab käivitada taru müügivõimaluste selles näites on märgitud. 

### <a name="create-azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus loomine
Selles etapis tuleb teil link Azure Storage konto oma andmete factory. Selles õpetuses abil saate talletada sisend andmed ja HQL skriptifail sama Azure Storage konto.

1. Saate määrata muutuja nimega **cmd**käsk. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Käivitage käsk **Invoke-käsu**abil.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Tulemusi vaadata. Kui lingitud teenus on loodud, kuvatakse JSON lingitud teenuse **tulemused**; muul juhul kuvatakse tõrketeade.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Windows Azure Hdinsightiga lingitud teenuse loomine
Selles etapis tuleb link kuvatakse nõudmisel Hdinsightiga kobar oma andmete factory. Hdinsightiga kobar käitusajal luuakse automaatselt ja kustutatud pärast seda teha töötlemiseks ja jõude jaoks määratud aja jooksul. Võite kasutada oma Hdinsightiga kobar asemel kuvatakse nõudmisel Hdinsightiga kobar. Üksikasjad leiate [Arvutada lingitud teenused](data-factory-compute-linked-services.md) .  

1. Saate määrata muutuja nimega **cmd**käsk.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Käivitage käsk **Invoke-käsu**abil.

        $results = Invoke-Command -scriptblock $cmd;
3. Tulemusi vaadata. Kui lingitud teenus on loodud, kuvatakse JSON lingitud teenuse **tulemused**; muul juhul kuvatakse tõrketeade.  

        Write-Host $results

## <a name="create-datasets"></a>Andmekomplektide loomine
Selles etapis loote andmekomplektide sisend ja väljund andmete töötlemiseks taru. Vaadake neid andmekomplektide **StorageLinkedService** selles õpetuses olete loonud. Lingitud teenuse osutab Azure Storage konto ja andmekomplektide määrata talletamist, mis hoiab sisendit container, kausta, faili nimi ja andmete väljund.   

### <a name="create-input-dataset"></a>Sisestuskeel andmekomplekti loomine
Selles etapis loote Sisestuskeel andmekomplekti talletatud Azure'i bloobimälu sisendandmete tähistada.

1. Saate määrata muutuja nimega **cmd**käsk. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Käivitage käsk **Invoke-käsu**abil.

        $results = Invoke-Command -scriptblock $cmd;
3. Tulemusi vaadata. Kui andmekomplekti on loodud, kuvatakse JSON andmekogumi **tulemused**; muul juhul kuvatakse tõrketeade.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Väljundi andmekomplekti loomine
Selles etapis loote väljundi andmekomplekti talletatud Azure'i bloobimälu väljundi andmete esitamiseks.

1. Saate määrata muutuja nimega **cmd**käsk.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Käivitage käsk **Invoke-käsu**abil.

        $results = Invoke-Command -scriptblock $cmd;
3. Tulemusi vaadata. Kui andmekomplekti on loodud, kuvatakse JSON andmekogumi **tulemused**; muul juhul kuvatakse tõrketeade.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Müügivõimaluste loomine
Selles juhises saate luua oma esimese müügivõimaluste **HDInsightHive** tegevuse. Sisestuskeel sektorit on saadaval iga kuu (sagedus: kuu, intervall: 1), toodetud väljundi sektorit iga kuu ja tegevuse ajasti atribuut on seatud ka kord kuus. Sätete väljundi andmekomplekti ja tegevuste ajasti peab vastama. Väljundi andmekomplekti on praegu, mis juhib ajakava, seega peate looma ka väljundi andmekomplekti isegi juhul, kui tegevuse pole mis tahes väljundi loomiseks. Kui tegevuse ei võta mis tahes sisendit, võite jätkata Sisestuskeel andmekomplekti loomine.  

Veenduge, et leiate Azure'i bloobimälu **adfgetstarted/inputdata** kausta fail **input.log** ja käivitage järgmine käsk tulemas juurutamiseks. Kuna varem on seadistatud **algus** ja **lõpp** aeg ja **isPaused** väärtuseks false, müügivõimaluste (tegevus teel) käivitatakse kohe pärast juurutamist. 

1. Saate määrata muutuja nimega **cmd**käsk.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Käivitage käsk **Invoke-käsu**abil.

        $results = Invoke-Command -scriptblock $cmd;
3. Tulemusi vaadata. Kui andmekomplekti on loodud, kuvatakse JSON andmekogumi **tulemused**; muul juhul kuvatakse tõrketeade.  

        Write-Host $results
5. Palju õnne, olete loonud oma esimese müügivõimaluste Azure PowerShelli kaudu!

## <a name="monitor-pipeline"></a>Müügivõimaluste jälgimine
Selles etapis tuleb kasutada andmete Factory REST API sektorid tulemas abil koostatud jälgimiseks.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Kuvatakse nõudmisel Hdinsightiga kobar loomine kulub tavaliselt millalgi (umbes 20 minutit). Seetõttu oodata müügivõimaluste teha töötlemiseks sektorit **30 minutit** .  

Käivitage käsk Invoke ja järgmise seni, kuni näete **valmis** riigis või **nurjunud** sektorit. Kui olete valmis olekus on sektorit, vaadake **partitioneddata** kausta **adfgetstarted** ümbrises bloobimälus väljundi andmete jaoks.  Kuvatakse nõudmisel Hdinsightiga kobar loomine tavaliselt võtab aega.

![väljundi andmed](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Sisestuskeel faili kustutamine kui sektorit on töötlemine õnnestus. Seetõttu sektorit uuesti või õpetuse uuesti teha soovite, laadige inputdata kausta adfgetstarted-ümbrisest Sisestuskeel faili (input.log).

Samuti saate Azure portaali jälgida sektorid ja seotud probleemide tõrkeotsinguks. Lisateavet leiate teemast [Azure portaalis torujuhtmete jälgimine](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) üksikasjad.  

## <a name="summary"></a>Kokkuvõte 
Selles õpetuses loodud on Azure andmete factory andmete töötlemise taru skripti käivitades Hdinsightiga Hadoopi klaster. Saate kasutada andmete Factory redaktori Azure portaali teha järgmist:  

1.  Loodud on Azure **andmete factory**.
2.  Loodud kaks **lingitud teenused**.
    1.  **Azure Storage** lingitud teenus hoiab andmeid factory sisend faile oma Azure'i bloobimälu link.
    2.  **Windows Azure Hdinsightiga** nõudmisel lingitud teenus on nõudmisel Hdinsightiga Hadoopi kobar linkida andmeid factory. Azure'i andmed loob mõne Hdinsightiga Hadoopi kobar lihtsalt õigel ajal töödelda sisendandmete ja väljundi andmete. 
3.  Loodud kaks **andmekomplektide**, mis kirjeldavad sisend- ja andmete Hdinsightiga taru tegevuste teel. 
4.  **Müügivõimaluste** loodud **Hdinsightiga taru** tegevuse. 

## <a name="next-steps"></a>Järgmised sammud
Selles artiklis loomist müügivõimaluste teisendus tegevus (Hdinsightiga tegevuste), mis töötab ka nõudmisel Windows Azure Hdinsightiga kobar taru skripti. Kopeeri tegevuse mõne Azure'i bloobimälu andmete kopeerimine Azure SQL-i kasutamise kohta leiate teemast [Õppeteema: andmete kopeerimine mõnda Azure'i bloobimälu Azure SQL-i](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Vt ka
| Teema: | Kirjeldus |
| :---- | :---- |
| [Andmete Factory REST API viide](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Täielik dokumentatsiooni näidata andmete Factory cmdletid |
| [Tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) | Selles artiklis on toodud andmete teisendus tegevused (nt Hdinsightiga taru teisendus kasutasite selles õpetuses) loendi Azure'i andmed Factory ei toeta. |
| [Plaanimis- ja täitmise](data-factory-scheduling-and-execution.md) | Selles artiklis selgitatakse plaanimis- ja täitmise aspektide Azure'i andmed Factory rakenduse mudel. |
| [Torujuhtmete](data-factory-create-pipelines.md) | See artikkel aitab teil mõista torujuhtmed ja Azure'i andmed Factory ja kuidas neid kasutada ehitada lõpuni Andmepõhiste töövoogude oma stsenaarium või ettevõtte tegevusi. |
| [Andmekomplektide](data-factory-create-datasets.md) | See artikkel aitab teil mõista andmekogumite Azure'i andmed Factory.
| [Jälgimine ja haldamine torujuhtmete Azure portaali labad abil](data-factory-monitor-manage-pipelines.md) | Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja oma torujuhtmete abil Azure portaali labad silumine. |
| [Jälgimine ja haldamine torustike jälgimise App abil](data-factory-monitor-manage-app.md) | Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja silumine torujuhtmete jälgimine ja haldamine rakenduse abil. 

