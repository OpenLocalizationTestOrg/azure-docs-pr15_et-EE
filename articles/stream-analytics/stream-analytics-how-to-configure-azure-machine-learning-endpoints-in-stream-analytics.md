<properties 
    pageTitle="Kuidas konfigureerida Azure seadme õ lõpp-punktid voo Analytics | Microsoft Azure'i" 
    description="Seadme keele kasutaja määratletud funktsioonid voo Analytics"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Masina voo Analytics õ integreerimine

Voo Analytics toetab kasutaja määratletud funktsioonid, mis kõne välja Azure seadme õ lõpp-punktid. REST API tugi on selle funktsiooni kasutamiseks on üksikasjalikult [voo Analytics REST API teek](https://msdn.microsoft.com/library/azure/dn835031.aspx). Selles artiklis antakse vaja seda võimalust voo Analytics edukaks rakendamiseks kohta. Juhend on samuti sisestatud ja on saadaval [siin](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Ülevaade: Azure'i kohapeal õ terminoloogia

Microsoft Azure'i masina õ pakub koostööpõhise, hiirega tööriista abil saate koostada, testige ja juurutada ennustav lahendusi oma andmete põhjal. See tööriist on nn *Azure seadme õ Studio*. Studio saab interaktiivselt kasutada seadme õppematerjalid ja hõlpsalt luua, testige ja saades oma kujundus. Järgmiste ressursside ja nende määratlused on allpool.

- **Tööruumi**: *Tööruum* on ümbris, mis on kõik muud seadme õppematerjalid koos ümbrises haldus ja juhtimine.
- **Katse**: *katsete* on loodud andmeteadlaste kasutada andmekomplektide ja koolitada kohapeal õ mudeli.
- **Lõpp-punkt**: *lõpp-punktid* on kasutada sisendina funktsioonid teha, rakendada määratud seadme õ mudeli ja tagastada poolitusjoonega väljundi Azure seadme Õppekeskuse objekti.
- **Webservice hinded**: *hinded webservice* on lõpp-punktid kogum, nagu eespool kirjeldatud.

Iga lõpp-punkti on API-de paketi täitmise ja sünkroonse täitmiseks. Voo Analytics kasutab sünkroonse täitmise. Kindla teenuse nimeks [Taotluse/vastuse teenuse](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) AzureML Studios.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Seadme Õppekeskuse ressurssidele voo Analytics tööde haldamine

Voo Analyticsi töö töötluse, taotluse/vastuse endpoint, mis [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)ja ärplema määratlus on kõik vajalikud eduka täitmiseks. Voo Analytics on sisuosast ärplema lõpp-punkti URL-i, otsib kasutajaliides ja tagastab vaikimisi UDF-i määratlus kasutajale täiendavate lõpp.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Voo Analytics ja seadme Õppekeskuse UDF-i kaudu REST API konfigureerimine

REST API-de abil võib konfigureerida oma töö helistada Azure seadme keele funktsioonid. Juhised on järgmised:

1. Voo Analytics töö loomine
2. Sisend määratlemine
3. Väljund määratlemine
4. Kasutaja määratletud funktsioon (UDF) loomine
5. Voo Analytics teisendus, mis nõuab UDF kirjutamine
6. Töö alustamine

## <a name="creating-a-udf-with-basic-properties"></a>Tavaline atribuudid on UDF-i loomine

Näiteks järgmine proovi kood loob skalaarse UDF-i lõpp Azure'i kohapeal õ seob *newudf* nimega. Pange tähele, et *lõpp-punkti* (teenuse URI) leiate abi API lehel valitud teenusest ja *apiKey* leiate teenuste avalehele.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Näide taotluse keha:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Kõne RetrieveDefaultDefinition lõpp-punkti vaikimisi UDF

Kui luukere UDF on loodud on vaja UDF täieliku piiritleda. RetreiveDefaultDefinition lõpp-punkti aitab teil saada vaikimisi määratluse skalaarse funktsiooni, mis on seotud Azure seadme õ lõpp. Allpool last peab teil saada vaikimisi UDF-i määratluse skalaarse funktsiooni, mis on seotud Azure'i kohapeal õ lõpp. See ei määrata tegeliku lõpp-punkti on juba esitatud panna taotluse ajal. Voo Analytics helistab lõpp-punkti, kui see on esitatud selgesõnaliselt esitatud. Muul juhul kasutab üks algselt viidatud. Siin UDF leiab ühe tekstistringi parameetri (lause) ja tagastab ühe väljund tüüpi string, mis näitab, et lause "meeleolu" silti.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Näide taotluse keha:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Valimi väljund selle soovite otsida midagi all.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Paikade UDF vastus 

UDF peab nüüd paigatud koos eelmist vastust, nagu allpool näidatud.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Koosolekukutse kehasse (RetrieveDefaultDefinition väljund):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Rakendada helistamiseks UDF voo Analytics teisendus

Nüüd päringu iga Sisestuskeel sündmuse UDF (siin nimega scoreTweet) ja kirjutada selle sündmuse vastuse väljund.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
