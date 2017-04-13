<properties
    pageTitle="Teksti Analytics API versioonile 2 | Microsoft Azure'i"
    description="Azure'i masina Õppekeskuse teksti analüüsi - 2 versiooni täiendamine"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Teksti Analytics API versioonile 2 #

Sellest juhendist viib teid läbi protsessi oma koodi üleminekut [esimese versiooni API](../machine-learning/machine-learning-apps-text-analytics.md) abil teine versioon. 

Kui olete kasutanud API ja soovite lisateavet, saate **[Lisateavet API siin](//go.microsoft.com/fwlink/?LinkID=759711)** või **[järgige lühijuhendi](//go.microsoft.com/fwlink/?LinkID=760860)**. Tehniline teave, vaadake **[API määratlus](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Osa 1. Uue tootenumbri hankimine ###

Esmalt peate saada **Azure portaali**uus API võti:

1. Liikuge teksti Analytics teenuse [Cortana ärianalüüsi Galerii](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Siin leiate lingid dokumentatsiooni ja koodi näidised.

1. Klõpsake nuppu **Logi sisse**. Järgmine link viib teid Azure haldusportaali, kus saate registreerute teenuse.

1. Valige leping. Võite valida **tasuta taseme 5000 tehingud kuus**. On tasuta leping, mida ei tuleb tasuda teenuse kasutamise kohta. Peate Azure tellimuse sisse logida. 

1. Pärast registreerumist teksti Analytics, antakse teile mõne **API võti**. Kopeerige see võti nagu peate selle API teenuste kasutamisel.

### <a name="part-2-update-the-headers"></a>Osa 2. Päiste värskendamine ###

Värskendage esitatud päise väärtused, nagu allpool näidatud. Pange tähele, et konto võti kodeeritakse enam.

**Version 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Versioon 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Osa 3. Base URL-i värskendamine ###

**Version 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Versioon 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Osa 4a. Värskendage vormingud meeleolu, fraase ja keeled ###

#### <a name="endpoints"></a>Lõpp-punktid ####

SAADA lõpp-punktid on nüüd taunitud, nii, et kõik sisestatud esitada postituse päringuga. Värskendage need allpool lõpp-punktid.

| |Version 1 ühe lõpp-punkti|Lõpp-punkti Version 1 paketi|Versiooni 2 lõpp-punkti|
|---|---|---|---|
|Kõne tüüp|HANKIMINE|POSTITUSE|POSTITUSE|
|Meeleolu|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Fraase|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Keeled|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Sisestuskeel vormingud ####

Pange tähele, ainult vormingu postitust on nüüd vastu, nii vormindate panust, mis varem kasutanud vastavalt ühe dokumendi lõpp-punktid. Sisendina pole tõstutundlik.

**Version 1 (paketi)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Versioon 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Meeleolu väljund ####

**Version 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versioon 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Fraase väljund ####

**Version 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versioon 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Keelte väljund ####


**Version 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versioon 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Osa 4b. Vormid teemade värskendamine ###

#### <a name="endpoints"></a>Lõpp-punktid ####

| |Version 1 lõpp-punkti | Versiooni 2 lõpp-punkti|
|---|---|---|
|Vormiandmete edastamine teema avastamiseks (POSTITA)|```StartTopicDetection```|```topics```|
|Toomise teema tulemid (saada)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Sisestuskeel vormingud ####

**Version 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Versioon 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Esitamise tulemused ####

**Version 1 (POSTITA)**

Varem, kui on töö lõpetanud, kuvatakse oleks järgmine JSON väljund, kus soovite selle jobId lisatud URL-i toomiseks väljund.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Versioon 2 (POSTITA)**

Vastuse nüüd ka päise väärtus järgmiselt, kus `operation-location` kasutatakse lõpp-tulemuste Küsitlus:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Toimingu tulemused ####

**Version 1 (saada)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Versioon 2 (saada)**

Nagu varem, tagastatakse **perioodiliselt küsitlus väljund** (soovitatud perioodi on iga minut) kuni väljund. 

Kui teemade API on lõpule jõudnud, oleku lugemine `succeeded` tagastatakse. See hõlmab väljundi tulemused vormingus allpool näidatud:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Osa 5. Katsetage seda! ###

Nüüd peaks olema valmis! Testige oma väikese valimi tagamaks, et teil edukalt protsessi andmete kood.
