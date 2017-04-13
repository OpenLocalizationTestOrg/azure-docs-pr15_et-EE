<properties 
    pageTitle="Käivitage U-SQL skripti Azure andmeanalüüsi Lake Azure'i andmed Factory" 
    description="Saate teada, kuidas käivitades skriptide U-SQL Azure'i andmeanalüüsi Lake Arvuta teenuse andmete töötlemiseks." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Käivitage U-SQL skripti Azure andmeanalüüsi Lake Azure'i andmed Factory
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)
 
Müügivõimaluste rakenduses on Azure andmete factory töötleb andmeid lingitud salvestusruumi teenuste lingitud Arvuta teenuste abil. See sisaldab tegevusi, kus iga tegevuse täidab konkreetse töötlemine jada. Selles artiklis kirjeldatakse **Andmete Lake Analytics U-SQL-i tegevuse** käivituva skripti **U-SQL** **Azure'i andmeanalüüsi Lake** lingitud Arvuta teenuse. 

> [AZURE.NOTE] 
> Looge konto Azure andmeanalüüsi Lake enne Müügivõimaluste loomine andmete Lake Analytics U-SQL-i tegevus. Azure'i Lake andmeanalüüsi kohta leiate teemast [Azure andmeanalüüsi Lake alustamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Vaadake üle soovitud [koostada esimese müügivõimaluste õppetükk](data-factory-build-your-first-pipeline.md) üksikasjalikud juhised andmete factory, lingitud teenused, andmekomplektide ja müügivõimaluste loomiseks. Andmete Factory üksuste loomiseks kasutada andmete Factory toimetaja või Visual Studio või Azure PowerShelli JSON Koodilõigud.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure'i andmeanalüüsi Lake lingitud teenus
Saate luua teenuse Azure andmeanalüüsi Lake Arvuta linkimiseks mõne Azure'i andmed factory teenuse **Azure andmeanalüüsi Lake** lingitud. Andmete Lake Analytics U-SQL-i tegevuse tulemas viitab lingitud teenus. 

Järgmises näites sisaldab teenuse Azure Lake andmeanalüüsi lingitud JSON määratlus. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Järgmises tabelis on ära toodud JSON määratluse kasutatakse atribuutide kirjeldused. 

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
Tüüp | Atribuudi tüüp väärtuseks olema seatud: **AzureDataLakeAnalytics**. | Jah
account_name | Azure'i andmeanalüüsi Lake konto nimi. | Jah
dataLakeAnalyticsUri | Azure'i andmed Lake Analytics URI. |  Ei 
luba | Luba kood tuuakse automaatselt pärast nupu **Autoriseerin** andmete Factory Editoris ja lõpuleviimine OAuthi Logi sisse.  | Jah 
subscriptionId | Azure'i tellimuse id | Ei (kui pole määratud, tellimusele, andmete factory kasutatakse). 
resourceGroupName | Azure'i ressursirühma nimi |  Ei (kui pole määratud, ressursirühm andmete factory, kasutatakse).
SeansiId | seansi id OAuthi autoriseerimine seanss. Iga seansi id on kordumatu ja võib kasutada ainult üks kord. Seansi Id on automaatselt genereeritud andmed Factory redaktor. | Jah

Teie loodud **Autoriseerin** nupu abil autoriseerimine koodi aegub pärast uuesti. Erinevat tüüpi kasutajakontode aegumise korda leiate järgmisest tabelist. Võidakse kuvada järgmine tõrketeade sõnum autentimine **Luba on aegunud**: mandaat toiming tõrge: invalid_grant - AADSTS70002: identimisteabe valideerimisel ilmnes tõrge. AADSTS70008: Esitatud juurdepääsu andmine on aegunud või tühistatud. Jälita ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 korrelatsiooni ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 ajatempli: 2015-12-15 21:09:31Z

 
| Kasutaja tüüp | Pärast lõppemist |
| :-------- | :----------- | 
| Azure Active Directory ei hallata kasutajakontosid (@hotmail.com, @live.com, jne.) | 12 tundi |
| Kasutajakonto hallatavate, Azure Active Directory (AAD) | 14 päeva pärast viimast sektorit käivitada. <br/><br/>90 päeva, kui mõnda sektorit põhjal OAuthi lingitud teenus töötab vähemalt ühe korra 14 päeva jooksul. |

Vältige/lahendamiseks selle vea, kasutades **Autoriseerin** reauthorize nuppu **Luba on aegunud** ja ümberkorraldamine lingitud teenus. Saate luua ka programmiliselt koodiga kohta järgmisest jaotisest **SeansiId** ja **Luba** atribuutide väärtused. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Genereerida programmiliselt SeansiId ja luba väärtused 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Teemadest [AzureDataLakeStoreLinkedService klassi](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)ja [AuthorizationSessionGetResponse klassi](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) koodis kasutada andmete Factory klassid üksikasjad. Viite lisamine: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog klassi. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Andmete Lake Analytics U-SQL-i tegevus 

Järgmised JSON koodilõigu määratleb müügivõimaluste koos andmete Lake Analytics U-SQL-i tegevus. Tegevuse määratluse on varem loodud Azure'i Lake andmeanalüüsi lingitud teenuse viide.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Järgmises tabelis kirjeldatakse nimede ja kirjelduste atribuute, mis on omased see tegevus. 

Atribuut | Kirjeldus | Nõutav
:-------- | :----------- | :--------
tüüp | Atribuudi tüüp väärtuseks peab olema seatud **DataLakeAnalyticsU-SQL-i**. | Jah
scriptPath | A-SQL-skript sisaldava kausta tee. Faili nimi on tõstutundlikud. | Ei, (kui kasutate skript)
scriptLinkedService | Lingitud teenus, mis talletamist, mis sisaldab andmeid factory skripti lingid | Ei, (kui kasutate skript)
skripti | Määrake Tekstisisese skripti asemel scriptPath ja scriptLinkedService määratlemine. Näide: "skript": "Loomine andmebaasi test". | Ei (kui kasutate scriptPath ja scriptLinkedService)
degreeOfParallelism | Samaaegselt kasutatavate töö käivitamiseks sõlmed maksimaalne arv. | Ei
Priority (prioriteet) | Määratleb töö, mille välja kõik järjestatud peaks olema valitud esimese käivitamiseks. Väiksem number, seda suurem prioriteet. | Ei 
parameetrid | A-SQL-skript parameetrid | Ei 

Vaadake [SearchLogProcessing.txt skripti määratlus](#script-definition) skripti määratlus. 

## <a name="sample-input-and-output-datasets"></a>Kuulake sisendi ja andmekomplektide väljund

### <a name="input-dataset"></a>Sisestuskeel andmekomplekti
Selles näites asuvad sisendandmete Azure'i andmed Lake poes (SearchLog.tsv faili datalake/sisend kausta). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Väljundi andmekomplekti
Selles näites on talletatud U-SQL-i skripti väljundi andmete Azure'i andmed Lake poes (datalake/väljund kaust). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Valimi andmesalve Lake lingitud teenus
Siit leiate Azure'i andmesalve Lake lingitud teenust kasutavad sisend andmekomplektide valimi määratlus. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Teemast [Azure Lake poest andmete teisaldamine](data-factory-azure-datalake-connector.md) artiklis JSON atribuutide kirjeldus. 

## <a name="sample-u-sql-script"></a>Skripti näide A-SQL-is 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Väärtused **@in** ja **@out** U-SQL-skript parameetrite edasi dünaamiliselt ADF kasutades jaotist "parameetrid". Müügivõimaluste määratluse jaotisest "parameetrid".

Saate määrata muid atribuute, nt degreeOfParallelism ja prioriteet ka definitsiooni müügivõimaluste vajavate Azure'i andmeanalüüsi Lake teenuse käivitamine.

## <a name="dynamic-parameters"></a>Dünaamilised parameetrid
Valimi müügivõimaluste määratlus, sisse ja välja parameetrid on määratud väärtustega raske koodiga. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

On võimalik dünaamiline parameetrite asemel kasutada. Näiteks: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

Sel juhul failid on ikka peale /datalake/input kaustast ja väljundi faile on loodud /datalake/output kausta. Faili nimed on dünaamiline algusaja sektori alusel.  