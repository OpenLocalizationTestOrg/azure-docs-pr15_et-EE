<properties 
    pageTitle="Kasutage masina õppimise | Microsoft Azure'i" 
    description="Kirjeldab, kuidas luua loomine sõnastikupõhise torujuhtmete abil Azure'i andmed Factory ja Azure arvuti koolitus" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Looge sõnastikupõhise torujuhtmete Azure seadme õppimise abil   
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Sissejuhatus

[Azure'i masina õ](https://azure.microsoft.com/documentation/services/machine-learning/) võimaldab teil koostada, testige ja juurutada ennustav lahendusi. Üksikasjalik seisukohast on teha kolm toimingut: 

1. **Loo koolitus katse**. Mida teha seda toimingut Azure'i ML Studio abil. ML studio on koostööpõhise visuaalse arenduskeskkond koolitada ja testimine koolitus andmete kasutamise näidis ennustav kasutatav.
2. **Sõnastikupõhise katse teisendada**. Kui mudelisse on saanud olemasolevate andmetega ja olete valmis seda kasutada uute andmete Keskmine, ettevalmistamine ja sujuvamaks muutmine oma katse jaoks hinded.
3. **Seda veebiteenuse juurutamine**. Saate avaldada oma hinded katse teenuse Azure web. Saate selle web teenuse lõpp-punkti kaudu mudelisse andmeid saata ja vastu võtta tulemi prognoose arvestusliku mudel.  

Azure'i andmed Factory võimaldab teil kerge vaevaga luua nii torustikud kasutada avaldatud [Azure seadme õ] [ azure-machine-learning] veebiteenuse ennustav jaoks. Leiate [Azure'i andmed Factory tutvustus](data-factory-introduction.md) ja [Koostage oma esimese müügivõimaluste](data-factory-build-your-first-pipeline.md) artiklitest kiiresti alustada Azure'i andmed Factory teenus. 

**Paketi täitmise tegevuste** kasutamine on Azure andmete Factory müügivõimaluste, saate autonoomsest Azure'i ML veebiteenuse, teha prognoose paketi andmeid. Vt [kasutada ka Azure ML veebiteenuse abil paketi täitmise tegevuse](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) jaotisest.

Aja jooksul prognoosmudelite Azure'i ml hinded katsete vaja koolitada ümber, kasutades uue Sisestuskeel kogumid. Azure'i ML mudelit, mis andmete Factory müügivõimaluste kaudu saate ümber, tehes järgmist: 

1. Avaldada veebiteenuse koolitus katse (mitte sõnastikupõhise katse). Saate selle juhise Azure'i ML Studios nagu tegite esitamist sõnastikupõhise katse veebiteenuse eelmises näites.
2. Azure'i ML paketi täitmise tegevuse abil autonoomsest veebiteenuse koolitus katse puhul. Põhimõtteliselt, saate Azure'i ML paketi täitmise tegevuse autonoomsest koolituse veebiteenuse nii hinded veebiteenus. 
  
Kui olete teinud ümberõpet, mida soovite värskendada hinded veebiteenuse (sõnastikupõhine katse esitatud veebiteenuse) äsja koolitatud mudeliga. Siin on toodud juhiseid. 

1. Vaikelokaadisättest lõpp-punkti lisada hinded veebiteenus. Vaikimisi lõpp-punkti veebiteenuse ei saa värskendada, seega peate looma vaikelokaadisättest lõpp-punkti Azure'i portaalis. Nii kontseptuaalset teavet ning üksikasjalik juhised artiklist [Loomine lõpp-punktid](../machine-learning/machine-learning-create-endpoint.md) .
2. Värskendage olemasoleva Azure'i ML lingitud teenuste jaoks kasutada-vaikimisi lõpp-punkti hinded. Uue lõpp-punkti veebiteenuse, mida on värskendatud kasutamiseks kasutamise alustamine.
3. **Azure'i ML värskendada ressursside tegevuse** abil saate uuendada veebiteenuse äsja koolitatud mudel.  

Vt jaotisest [värskendamine Azure'i ML mudelite Update ressurss tegevuse abil](#updating-azure-ml-models-using-the-update-resource-activity) . 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Paketi täitmise tegevuse abil veebiteenuse kasutada

Saate kasutada Azure'i andmed Factory korraldab andmete liikumine ja töötlemine ja tehke abil Azure seadme õ paketi täitmine. Siin on ülataseme juhiseid.

1. Looge lingitud Azure'i masina õ service. Teil on vaja järgmist:
    1. **Taotleda URI** paketi täitmist API-ga. Leiate taotlemine URI web services lehel linki **Paketi täitmist** .
    1. Avaldatud Azure'i masina õ veebiteenuse **API võti** . Leiate API võti, klõpsates nuppu veebiteenus, mis on avaldatud. 
 2. **AzureMLBatchExecution** toiminguga.

    ![Seadme õ armatuurlaud](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Paketi URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Stsenaarium: Katsed Web teenuse sisendeid/väljundeid, mis viitavad andmete Azure'i bloobimälu
Selle stsenaariumi korral Azure'i masina õ veebiteenuse teeb andmete abil faili Azure'i bloobimälu prognoose ja talletab selle bloobimälu ennustamine tulemused. Järgmised JSON määratleb andmete Factory müügivõimaluste AzureMLBatchExecution tegevusega. Tegevuse on andmekomplekti **DecisionTreeInputBlob** sisendina ja **DecisionTreeResultBlob** võrdsed. **DecisionTreeInputBlob** edastatakse sisendina veebiteenusele atribuudi **webServiceInput** JSON abil. **DecisionTreeResultBlob** edastatakse mõne väljundina veebiteenuse **webServiceOutputs** JSON atribuudi.  

> [AZURE.IMPORTANT] 
> Kui veebiteenuse kulub mitme sisendina, kasutada atribuuti **webServiceInputs** **webServiceInput**kasutamise asemel. Jaotisest [veebiteenus nõuab mitut sisendit](#web-service-requires-multiple-inputs) atribuudi webServiceInputs kasutamise näide.
>  
> Andmekomplektide, millele viidatakse **webServiceInput**/**webServiceInputs** ja **webServiceOutputs** atribuudid ( **typeProperties**) peab ka tegevuse **sisendina** ja **väljundid**.
> 
> Oma Azure'i ML katse, on web teenuse sisendi ja väljundi pordid ja globaalse parameetrite vaikenimed ("input1", "input2"), mida saate kohandada. Nimed webServiceInputs, webServiceOutputs ja globalParameters sätete kasutamiseks peab täpselt vastama katsete nimed. Valimi taotluse last saate vaadata oma Azure'i ML lõpp-punkti kinnitamiseks oodatud vastenduse lehel paketi täitmise spikker. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Ainult sisendi ja väljundi AzureMLBatchExecution tegevuse saab edasi veebiteenuse parameetrid. Näiteks ülaltoodud JSON koodilõigu, on DecisionTreeInputBlob sisend AzureMLBatchExecution tegevusele, mis on andmeid, et veebiteenuse kaudu webServiceInput parameeter.   

### <a name="example"></a>Näide

Selles näites kasutatakse Azure Storage sisend- ja andmete mahutamiseks. 

Soovitame läbida [koostamiseks oma andmeid Factory koos esimese müügivõimaluste] [ adf-build-1st-pipeline] kuueosalisest enne läheb läbi käesoleva näite. Andmete Factory redaktori abil saate luua andmete Factory esemeid (lingitud teenused, andmekomplektide, müügivõimaluste) selles näites.   
 

1. Luua **lingitud teenuse** **Azure Storage**. Kui sisend- ja failid on erinevate salvestusruumi kontod, peate kahe lingitud teenused. Siin on näide JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Saate luua soovitud **Sisestuskeel** Azure'i andmed Factory **andmekomplekti**. Erinevalt mõne muu andmete Factory andmekomplektide nende andmekomplektide peab sisaldama **folderPath** -ja **faili nimi** . Saate eraldamine põhjustada iga paketi täitmise (iga andmete sektorit) töötlemine või aedvili kordumatu sisendi ja väljundi faili. Võimalik, et peate kaasata mõned varustava tegevuse sisend muutuda CSV-faili vorming ja paigutamine salvestusruumi konto jaoks iga sektorit. Sel juhul ei hõlmaks **välise** ja **externalData** sätted, mis on näidatud järgmises näites ja oma DecisionTreeInputBlob oleks väljundi andmekomplekti muu tegevuse.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Sisestuskeel CSV-faili peab olema veerus päiserida. Kui kasutate **Kopeeri tegevuse** loomine/Teisalda csv soovitud bloobimälu sisse, määrake valamu atribuudi **blobWriterAddHeader** väärtuseks **true**. Näiteks:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Kui ka CSV-failis pole päiserida, võidakse kuvada järgmine tõrketeade: **tegevuse tõrge: lugemise string. Ootamatu luba: StartObject. Tee ", rida 1, positsioon 1**.
3. Saate luua **väljundi** Azure'i andmed Factory **andmekomplekti**. Selles näites kasutatakse eraldamine kordumatud väljund tee sektorit iga täitmiseks loomiseks. Ilma osadeks, kirjutaks tegevuse fail.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Looge **lingitud teenuse** tüüp: **AzureMLLinkedService**, pakkudes API võti ja mudeli paketi täitmise URL-i.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Lõpuks autor müügivõimaluste sisaldav **AzureMLBatchExecution** tegevus. Müügivõimaluste sooritab käitusajal, toimige järgmiselt.
    1. Saab oma Sisestuskeel andmekomplektide Sisestuskeel faili asukoht.
    2. Käivitab Azure seadme õ paketi täitmist API
    3. Kopeerib paketi täitmise väljundi bloobimälu, mis on antud teie andmekogumis väljundi. 

    > [AZURE.NOTE] AzureMLBatchExecution tegevuse võib olla null või rohkem sisendi ja ühe või mitme väljundi.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Algus** ja **lõpp** kuupäevade ja kellaaegade peab olema [ISO](http://en.wikipedia.org/wiki/ISO_8601)-vormingus. Näide: 2014-10-14T16:32:41Z. **Lõpuaeg** pole kohustuslik. Kui määrate atribuudi **end** väärtus arvutatakse nimega "**start + 48 tundi.**" Tulemas lõputult käivitamiseks määrata **9999-09-09** **end** atribuudi väärtust. Vt lisateavet JSON atribuudid [JSON skriptimise viide](https://msdn.microsoft.com/library/dn835050.aspx) .

    > [AZURE.NOTE] Määrata selle AzureMLBatchExecution sisend tegevuse pole kohustuslik. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Stsenaarium: Katsed lugeja/kirjutaja moodulid viidata andmete erinevad hoidlate

Teine levinum stsenaarium Azure'i ML katsete loomisel on kasutada lugeja ja kirjutaja moodulid. Lugeja mooduli kasutatakse andmete laadimiseks katse ja kirjutaja mooduli on teie katsete andmed salvestada. Lisateavet lugeja ja kirjutaja moodulid kohta leiate teemadest [lugeja](https://msdn.microsoft.com/library/azure/dn905997.aspx) ja [kirjutaja](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN-i teeki.     

Lugeja ja kirjutaja moodulid kasutamisel on mõistlik kasutada iga atribuudi need lugeja/kirjutaja moodulid veebiteenuse parameeter. Need web parameetrid võimaldavad teil konfigureerida ajal käitusaja väärtused. Näiteks saate luua katse abil lugemise moodul, mis kasutab Azure'i SQL-andmebaasi: XXX.database.windows.net. Pärast kasutusele võetud veebiteenuse, soovite lubada tarbijad veebiteenuse määramiseks muu Azure SQL serveri nimega YYY.database.windows.net. Saate selle väärtuseks on konfigureeritud lubama veebiteenuse parameeter.

> [AZURE.NOTE] Web teenuse sisestus- ja väljundi erinevad Web teenuse parameetrid. Esimese stsenaariumi puhul on näha, kuidas sisestus- ja väljund saate määrata Azure'i ML veebiteenus. Selle stsenaariumi korral te kaotate parameetrid veebiteenuse teineteisele lugeja/kirjutaja moodulid atribuudid. 

Vaatame stsenaariumi Web parameetritega. Teil on juurutatud Azure seadme õ veebiteenuse, mis kasutab lugeja mooduli andmeid lugeda ühest ei toeta Azure seadme õ andmeallikad (nt: Azure'i SQL-andmebaasi). Pärast paketi täitmist toimub, kirjutada tulemused kirjutaja mooduli (Azure'i SQL-andmebaasi) abil.  Pole web teenuse sisendi ja väljundi on määratletud katsete. Sel juhul soovitatav konfigureerida oluline web teenuse parameetrid lugeja ja kirjutaja moodulid. Selle konfiguratsiooni võimaldab lugeja/kirjutaja moodulid tuleb konfigureerida AzureMLBatchExecution tegevuse kasutamisel. Saate määrata Web teenuse parameetrite **globalParameters** jaotisele tegevuse JSON järgmiselt. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

Saate [Andmeid Factory funktsioonide](https://msdn.microsoft.com/library/dn835056.aspx) läbimise väärtused Web teenuse parameetrite, nagu on näidatud järgmises näites:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Web teenuse parameetrid on tõstutundlikud, tagada määratud tegevuse nimed JSON vastavad esitatud veebiteenuse need. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Lugeja mooduli abil saate andmeid lugeda mitu faili Azure'i bloobimälu
Suur andmete välistrasside nagu siga ja taru võivad põhjustada ühte või rohkem väljundi faili laiendiga pole. Näiteks kui määrate taru Välistabel, välise taru tabeli andmed saate salvestatakse Azure'i bloobimälu koos järgmist nime 000000_0. Lugeja mooduli kasutamine katse lugeda mitu faili ja kasutada neid prognoose. 

Azure'i masina õ katse lugeja mooduli kasutamisel saate määrata Azure'i bloobimälu sisendina. Azure'i bloobimälu failid võivad olla väljund faili (näide: 000000_0) töötavate Hdinsightiga siga ja taru skripti valmistanud. Lugeja moodul võimaldab teil lugeda faili (laiendiga pole) konfigureerimise abil soovitud **tee container, directory/bloobimälu**. **Container tee** osutab container ja **directory/bloobimälu** punktide kausta, mis sisaldab faile, nagu on näidatud järgmisel pildil. Tärni, \*) **saate määrata, et kõik failid container/kausta (, andmete/aggregateddata aastas = 2014/kuu-6 /\*)** on lugeda katse osana.

![Azure'i bloobimälu atribuudid](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Näide 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Torujuhtme AzureMLBatchExecution toimingute Web teenuse parameetrid

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
JSON eeltoodud näites:

- Juurutatud Azure seadme õ veebiteenuse kasutab lugeja ja kirjutaja mooduli lugemis-ja kirjutamisõigusega Azure'i SQL-andmebaasi, et andmeid. See veebiteenus seab neli järgmisi: andmebaasi serverinime, andmebaasi nime, serveri konto kasutajanimi ja serveri kasutajakonto parooli.  
- **Algus** ja **lõpp** kuupäevade ja kellaaegade peab olema [ISO](http://en.wikipedia.org/wiki/ISO_8601)-vormingus. Näide: 2014-10-14T16:32:41Z. **Lõpuaeg** pole kohustuslik. Kui määrate atribuudi **end** väärtus arvutatakse nimega "**start + 48 tundi.**" Tulemas lõputult käivitamiseks määrata **9999-09-09** **end** atribuudi väärtust. Vt lisateavet JSON atribuudid [JSON skriptimise viide](https://msdn.microsoft.com/library/dn835050.aspx) .

### <a name="other-scenarios"></a>Teiste stsenaariumide

#### <a name="web-service-requires-multiple-inputs"></a>Veebiteenuse jaoks on vaja mitut sisendit
Kui veebiteenuse kulub mitme sisendina, kasutada atribuuti **webServiceInputs** **webServiceInput**kasutamise asemel. Andmekomplektide, millele viidatakse **webServiceInputs** peab ka tegevuse **sisendina**.
 
Oma Azure'i ML katse, on web teenuse sisendi ja väljundi pordid ja globaalse parameetrite vaikenimed ("input1", "input2"), mida saate kohandada. Nimed webServiceInputs, webServiceOutputs ja globalParameters sätete kasutamiseks peab täpselt vastama katsete nimed. Valimi taotluse last saate vaadata oma Azure'i ML lõpp-punkti kinnitamiseks oodatud vastenduse lehel paketi täitmise spikker.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Veebiteenus pole vaja

Azure'i ML paketi täitmise veebiteenuste saab käivitada töövooge, näiteks R või Python skripte, mis võivad nõuda sisendeid. Või katse võib olla konfigureeritud abil lugemise moodul, mis ei jäta mis tahes GlobalParameters. Sel juhul tuleks AzureMLBatchExecution tegevuse konfigureeritud järgmiselt:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Veebiteenuse ei nõua sisend/väljund
Azure'i ML paketi täitmise veebiteenuse ei pruugi olla mis tahes veebiteenuse väljundi konfigureeritud. Selle näite puhul ei ole veebiteenuse sisendi või väljundi ega on mis tahes GlobalParameters konfigureeritud. On endiselt konfigureeritud ise tegevuse väljund, kuid see on antud, nagu on webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web teenuse kasutab lugejad ja poolt ja tegevuste töötab ainult siis, kui muud tegevused on õnnestunud

Azure'i ML web teenuse lugeja ja kirjutaja moodulid võib olla konfigureeritud käivitamiseks koos või ilma mis tahes GlobalParameters. Aga soovite manustada teenuse kõned teel, mida kasutab andmekomplekti sõltuvused Kasuta teenust ainult siis, kui mõni varustava töötlemine on lõpule viidud. Samuti võite mõne muu toimingu käivitada kui paketi täitmist on lõpule jõudnud, kasutades seda moodust. Sel juhul saate kiire sõltuvused tegevuse sisendina ja väljundeid, ilma neid veebiteenuse sisendeid või väljundeid nime panemise abil.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

**Takeaways** on:

-   Kui teie katse lõpp-punkti kasutab mõnda webServiceInput: see on esitatud bloobimälu andmekomplekti ja sisaldub tegevuse sisendina ja atribuudi webServiceInput. Muul juhul webServiceInput atribuut on välja jäetud. 
-   Kui teie katse lõpp-punkti kasutab webServiceOutput(s): nad on esindatud bloobimälu andmekomplektide ja kaasatakse tegevuse väljundeid ja atribuudi webServiceOutputs. Tegevuse väljundid ja webServiceOutputs on vastendatud iga katse väljund nime järgi. Muul juhul webServiceOutputs atribuut on välja jäetud.
-   Kui teie katse lõpp-punkti seab globalParameter(s), nad on esitatud tegevuse globalParameters atribuudi võtme, ja väärtuse paarideks. Muul juhul globalParameters atribuut on välja jäetud. Klahvid on tõstutundlikud. Väärtused, võib kasutada [Azure'i andmed Factory funktsioone](data-factory-scheduling-and-execution.md#data-factory-functions-reference) . 
- Täiendavad andmekomplektide sisalduvate tegevuse sisendi ja väljundi atribuudid on viidatud tegevuste typeProperties ilma. Nende andmekomplektide abil sektorit sõltuvused täitmine juhtimist, kuid muidu ignoreerib AzureMLBatchExecution tegevuse. 


## <a name="updating-models-using-update-resource-activity"></a>Värskenduse ressursi tegevuse mudelite värskendamine
Aja jooksul prognoosmudelite Azure'i ml hinded katsete vaja koolitada ümber, kasutades uue Sisestuskeel kogumid. Pärast seda, kui olete teinud ümberõpet, mida soovite värskendada hinded veebiteenuse retrained ML mudeliga. On tüüpilised toimingute ümberõppe ja värskendamist Azure'i ML mudelite veebiteenuste kaudu. 

1. [Azure'i ML Studio](https://studio.azureml.net)katse luua. 
2. Kui olete rahul mudelit, Azure'i ML Studio abil saate avaldada veebiteenuste mõlema funktsiooni **koolitus katsetamiseks** ja hinded /**sõnastikupõhise katse**.

Järgmises tabelis kirjeldatakse selles näites kasutatakse veebiteenuste.  Üksikasjad leiate [ümber masina õ mudelid programmiliselt](../machine-learning/machine-learning-retrain-models-programmatically.md) .

| Veebiteenuse tüüp | kirjeldus 
| :------------------ | :---------- 
| **Veebiteenuse koolitus** | Koolitus andmeid saab ja annab tulemiks koolitatud mudelite. Ümberõpet väljund on .ilearner failis on Azure'i bloobimälu.  **Vaikimisi lõpp-punkti** on teie jaoks automaatselt loodud koolitus katse nimega veebiteenuse avaldamisel. Saate luua veel lõpp-punktid, kuid näide kasutab ainult vaikimisi lõpp-punkti |
| **Veebiteenuse hinded** | Saab sildistamata andmete näited ja teeb prognoose. Ennustamine väljund võib olla erinevad vormid, näiteks csv-failina või ridade Azure SQL-andmebaasis, katse konfiguratsioonist. Vaikimisi lõpp-punkti on teie jaoks automaatselt loodud sõnastikupõhise katse nimega veebiteenuse avaldamisel. Teise **-vaike- ja värskendatavate lõpp-punkti** loomiseks [Azure portaali](https://manage.windowsazure.com). Saate luua veel lõpp-punktid, kuid selles näites kasutatakse ainult üks-vaikimisi värskendatavate lõpp-punkti. Üksikasjalikud juhised leiate teemast [Loomine lõpp-punktid](../machine-learning/machine-learning-create-endpoint.md) .       
 
Järgmisel pildil on kujutatud seos koolitus ja hinded Azure'i ml lõpp-punktid. 

![Veebiteenused](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


Kutsute **koolitus veebiteenuse** **Azure'i ML paketi täitmise tegevuse**abil. Koolitus veebiteenuse kasutada on sama, mis Azure'i ML veebiteenuse (hinded veebiteenuse) hinded andmete jaoks kasutada. Eelnevate jaotiste katta autonoomsest Azure'i ML veebiteenuse, on üksikasjalikult Azure'i andmed Factory müügivõimaluste kaudu tehke järgmist. 
  
Äsja koolitatud mudeliga veebiteenuse värskendamiseks **Azure'i ML värskendada ressursside tegevuse** abil saate autonoomsest **veebiteenuse hinded** . Nagu ülaltoodud tabelis, peate loomine ja kasutamine-vaikimisi värskendatavate lõpp-punkti. Lisaks värskendage oma andmed factory vaikelokaadisättest lõpp-punkti kasutada nii, et need alati kasutada retrained mudeli olemasoleva lingitud teenustes. 

Järgmistel pakub rohkem üksikasju. See on näiteks ümberõpe ja Azure ML mudelid on Azure andmete Factory müügivõimaluste värskendamine. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Stsenaarium: ümberõpe ja Azure ML mudelit, mis värskendamine
Selles jaotises antakse valimi müügivõimaluste kasutava **Azure'i ML paketi täitmise tegevuse** mudeli koolitada. Tulemas kasutab ka **Azure ML värskendada ressursside tegevuse** hinded veebiteenuse mudeli värskendada. Jaotis näeb JSON Koodilõigud kõik lingitud teenused, andmekomplektide ja müügivõimaluste näites. 

Siin on valimi müügivõimaluste diagrammi vaate. Nagu näete, Azure'i ML paketi täitmise tegevuse suunab koolituse sisendit ja annab koolituse väljundi (iLearner fail). Azure'i ML Update ressurss tegevuse võtab selle koolituse väljundi ja värskendab mudeli hinded web teenuse lõpp-punkti. Ei värskenda ressursi tegevuse mis tahes väljundi loomiseks. Funktsiooni placeholderBlob on lihtsalt näiva väljundi andmekomplekti, mis käivitamiseks tulemas on nõutav Azure andmete Factory teenus. 

![Müügivõimaluste skeem](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure'i bloobimälu lingitud teenuse:
Azure Storage hoiab järgmised andmed:

- koolitus andmed. Azure'i ML koolitus veebiteenuse sisendandmete.  
- iLearner faili. Azure'i ML koolitus veebiteenuse väljund. See fail on ka Update ressurss tegevuse sisendi.  
   
Siin on lingitud teenuse valimi JSON määratlus. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Koolitus Sisestuskeel andmekomplekti:
Järgmine andmekomplekti tähistab Azure'i ML koolitus veebiteenuse Sisestuskeel koolitus andmed. Azure'i ML paketi täitmise tegevuse võtab tuvastatavate sisendina. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Koolitus väljundi andmekomplekti:
Järgmine andmekomplekti tähistab iLearner väljundfail Azure'i ML koolitus veebiteenusest. Azure'i ML paketi täitmise tegevuse toodab selle andmekomplekti. Tuvastatavate on Azure ML Update ressurss tegevuse sisendi.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Lingitud teenuse lõpp-punkti Azure'i ML koolitus 
Järgmised JSON koodilõigu määratleb Azure seadme õ lingitud teenus, mis viitab vaikimisi lõpp-punkti veebiteenuse koolitus. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

**Azure'i ML Studio**, tehke **mlEndpoint** ja **apiKey**väärtuste saamiseks järgmist.

1. Klõpsake vasakul menüüs **VEEBITEENUSED** .
2. Klõpsake loendis veebiteenuste **koolitus veebiteenus** . 
3. Klõpsake nuppu Kopeeri **API võti** tekstivälja kohal. Kleebi lõikelaual võti andmete Factory JSON-redaktor.
4. **Azure'i ML studio**linki **Paketi täitmist** .
5. Jaotise **taotleda** **Taotleda URI** kopeerida ja kleepida andmed Factory JSON-redaktor.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Lingitud teenus Azure'i ML värskendatavate hinded lõpp-punkti:
Järgmised JSON koodilõigu määratleb Azure seadme õ lingitud teenus, mis hinded veebiteenuse-vaikimisi värskendatavate lõpp-punktid.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Enne loomine ja juurutamine on Azure ML lingitud teenuse, järgige mõne teise (vaikelokaadisättest ja värskendatavate) lõpp-punkti loomiseks hinded veebiteenuse [Loomine lõpp-punktid](../machine-learning/machine-learning-create-endpoint.md) artiklis toodud juhiseid.

Pärast vaike värskendatavate lõpp-punkti loomine, tehke järgmist.

- Klõpsake URI väärtuse saamiseks atribuudi **mlEndpoint** JSON **Paketi täitmist** .
- **Värskenduse RESSURSI** linki saada URI väärtus atribuudi **updateResourceEndpoint** JSON. API võti on lõpp-punkti lehel ise (alumises paremas nurgas). 

![värskendatavate lõpp-punkti](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Kohatäite väljundi andmekomplekti:
Azure'i ML Update ressurss tegevuse ei loo mis tahes väljundi. Azure'i andmed Factory nõuab siiski soovitud väljundi andmekomplekti müügivõimaluste järjestikuste juhtida. Seetõttu kasutame näiva/kohatäite andmekomplekti selles näites.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Müügivõimaluste
Tulemas on kaks tegevust: **AzureMLBatchExecution** ja **AzureMLUpdateResource**. Azure'i ML paketi täitmise tegevuse võtab koolituse andmed sisendina ja annab tulemiks on väljundina iLearner failina. Tegevuse käivitab veebiteenuse koolitus (koolitus katse esitatud veebiteenuse) andmetega Sisestuskeel koolitus ja võtab vastu veebiteenuse ilearner faili. Funktsiooni placeholderBlob on lihtsalt näiva väljundi andmekomplekti, mis käivitamiseks tulemas on nõutav Azure andmete Factory teenus. 

![Müügivõimaluste skeem](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Lugeja ja kirjutaja moodulid

Levinud stsenaariumi Web parameetritega on kasutamise ja Azure SQL-i lugejate poolt. Lugeja mooduli kasutatakse andmete laadimiseks andmete halduse teenuste väljaspool Azure seadme õ Studio katse. Kirjutaja moodul on andmete salvestamiseks oma katsete andmete haldamise teenustesse Azure seadme õ Studio väljaspool.  

Azure'i bloobimälu/Azure SQL-i lugeja/kirjutaja kohta leiate üksikasjalikumat teavet leiate [lugeja](https://msdn.microsoft.com/library/azure/dn905997.aspx) ja [kirjutaja](https://msdn.microsoft.com/library/azure/dn905984.aspx) teemadest MSDN-i teeki. Näide eelmises jaotises kasutatud Azure'i bloobimälu lugeja ja Azure'i bloobimälu kirjutaja. Selles jaotises kirjeldatakse Azure SQL-i lugeja ja kirjutaja Azure SQL-i abil.


## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Q:** Mul on mitu faili, et minu suur andmete torujuhtmetes luuakse. Kasutada AzureMLBatchExecution tegevuse kõik failid töötada?

**A:** Jah. Jaotisest **abil lugemise mooduli mitu faili Azure'i bloobimälu andmeid lugeda** lisateavet. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure'i ML paketi hinded tegevus
Kui te kasutate **AzureMLBatchScoring** tegevuse integreerimine Azure seadme õ, soovitame kasutada uusimat **AzureMLBatchExecution** tegevuse. 

AzureMLBatchExecution tegevuse tuuakse August 2015 väljaanne Azure'i SDK ja Azure PowerShelli.

Kui soovite AzureMLBatchScoring tegevuse jätkama, jätkake lugedes selles jaotises.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure'i ML paketi hinded tegevuse abil Azure Storage sisend 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Web teenuse parameetrid
Web teenuse parameetrite väärtused määramiseks **typeProperties** jaotise lisada **AzureMLBatchScoringActivty** jaotis tulemas JSON, nagu on näidatud järgmises näites: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


Saate [Andmeid Factory funktsioonide](https://msdn.microsoft.com/library/dn835056.aspx) läbimise väärtused Web teenuse parameetrite, nagu on näidatud järgmises näites:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Web teenuse parameetrid on tõstutundlikud, tagada määratud tegevuse nimed JSON vastavad esitatud veebiteenuse need. 

## <a name="see-also"></a>Vt ka

- [Azure'i ajaveebipostitus: Azure'i andmed Factory ja Azure seadme õ töötamise alustamine](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
