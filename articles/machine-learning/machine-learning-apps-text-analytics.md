<properties
    pageTitle="Seadme õ API: Teksti Analytics | Microsoft Azure'i"
    description="Microsofti arvuti õ teksti Analytics API-d saab kasutada struktureerimata tekst meeleolu analüüs, võtme fraasi eraldamine, keeletuvastuse ja teema tuvastamine analüüsimiseks."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Seadme õ API: Teksti analüüsiteabe meeleolu, võti fraasi eraldamine, keeletuvastuse ja teema tuvastamise

>[AZURE.NOTE] Sellest juhendist leiavad API versiooni 1. Versiooni 2, [**vaadake selles dokumendis**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). 2 versioon on nüüd selle API eelistatud versiooni.

## <a name="overview"></a>Ülevaade

Teksti Analytics API on tekst analytics [web services](https://datamarket.azure.com/dataset/amla/text-analytics) ehitatud Azure seadme õ. API saab kasutada struktureerimata tekst meeleolu analüüsi, võtme fraasi eraldamine, keeletuvastuse ja teema tuvastamise analüüsimiseks. Koolitus andmed on vaja kasutada seda API: tuua ainult oma teksti andmeid. See API kasutab täpsemalt loomulikus keeles töötlemise meetodid kõige paremini klassi prognoose.

Saate vaadata meie [demo sait](https://text-analytics-demo.azurewebsites.net/), kust leiate [näidised](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) kuidas rakendada tekstile analytics C# ja Python teksti analytics tegelikkuses.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Meeleolu analüüs

API tagastab numbriline tulemus 0 ja 1 vahel. 1 lähedal hinded näitavad positiivne meeleolu, ajal hinded lähedale 0 näitab negatiivne meeleolu. Meeleolu Keskmine on loodud, kasutades liigitamine meetodite abil. Sisestuskeel funktsioonid on klassifitseerijale kaasata n-g, funktsioonid, mis on loodud osa, kõne sildid ja Wordi embeddings. Praegu inglise on ainus toetatud keel.
 
## <a name="key-phrase-extraction"></a>Võtme fraasi eraldamine.

API tagastab loendi stringide tähistab teksti sisestamine olulisi põhilistest punktidest. Üksikkasutaja tehnika Microsoft Office'i keerukaid loomulikus keeles töötlemine tööriistakomplekt. Praegu inglise on ainus toetatud keel.

## <a name="language-detection"></a>Keele tuvastamine

API tagastab tuvastati keele- ja arvuliste Keskmine 0 ja 1 vahel. 1 lähedal hinded näitavad 100% kindel, et tuvastatud keel on tõene. Kuni 120 keeled on toetatud.

## <a name="topic-detection"></a>Teema: automaattuvastus

See on äsja avaldatud API, mis tagastab ülaosas tuvastatud teemade loendit esitatud teksti kirjed. Teema on tähistatud võtme fraas, mis võib olla üks või rohkem seotud sõnu. Selle API nõuab minimaalselt 100 teksti kirjeid nii, et esitada, kuid on mõeldud tuvastamiseks teemade üle sadu tuhandetele kirjed. Pange tähele, et see API kulude 1 tehingu esitatud teksti kirje kohta. API on mõeldud töötavad hästi lühikesed ja inimeste kirjutatud teksti, nt kommentaare ja kasutajale tagasiside.

---

## <a name="api-definition"></a>API määratlus

### <a name="headers"></a>Päised

Tagada õige päiste kaasamine teie taotlus, mis peaks olema järgmised:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Teie konto teie konto võti leiate [Azure'i Data Marketi](https://datamarket.azure.com/account/keys). Pange tähele, et praegu ainult JSON aktsepteeritakse sisend- ja kellaajavormingud. XML-i ei toetata.

---

## <a name="single-response-apis"></a>Ühe vastuse API-d

### <a name="getsentiment"></a>GetSentiment

**URL-I** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Näide taotlus**

Kõne ajal all me taotlemise meeleolu analüüsi fraasi "Tere, maailm":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

See toob vastuse järgmiselt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL-I**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Näide taotlus**

Kõne allpool, on esitanud, võti laused leitud tekst "Oli hind jääda, teatav ja sõbralik personal":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

See toob vastuse järgmiselt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL-I**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Näide taotlus**

HANGI kõne ajal allpool me paluda meeleolu fraase *Tere, maailm* teksti jaoks

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

See toob vastuse järgmiselt:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Valikulised parameetrid**

`NumberOfLanguagesToDetect`on valikuline parameeter. Vaikeväärtus on 1.

---

## <a name="batch-apis"></a>Paketi API-d

Teksti Analytics teenus võimaldab teil teha meeleolu ja klahvi-lause kasulik teha kogumitena. Pange tähele, et iga kirjete arvu skoorile ühele. Näiteks, kui soovite meeleolu 1000 kirjete ühe telefonil 1000 tehingud maha.

Pange tähele, et süsteemi sisestatud ID-d on tagastatud süsteemi ID-d. Veebiteenuse ei kontrolli, et need ID-d on kordumatu. Helistaja kinnitamine unikaalsuse vastutab. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL-I** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Näide taotlus**

Kõne postituse all, saame küsimise tundeid fraaside "Tere, maailm", "Hello Foo World" ja "Hello minu World" taotluse sisu:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Taotlege keha.

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

Allpool vastuseks saate teksti ID-ga seostatud hinded loendit:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL-I**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Näide taotlus**

Selles näites me taotlenud tundeid jaoks järgmised teksti fraase loendit: 

* "See oli hind jääda, teatav ja sõbralik personal"
* "See oli mõne suurepäraseid Koosta konverentsi, väga huvitav räägib"
* "Liiklus hirmsaid, ma kulunud 3 tunni järel läheb lennujaamast"

Selle taotluse POSTITUSSE kõnet lõpp-punkti:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Taotlege keha.

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

Allpool vastuseks saate teksti ID-ga seostatud fraase loendit:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Inglise",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Prantsuse",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Teema: automaattuvastus API-d

See on äsja avaldatud API, mis tagastab ülaosas tuvastatud teemade loendit esitatud teksti kirjed. Teema on tähistatud võtme fraas, mis võib olla üks või rohkem seotud sõnu. Pange tähele, et see API kulude 1 tehingu esitatud teksti kirje kohta.

Selle API nõuab minimaalselt 100 teksti kirjeid nii, et esitada, kuid on mõeldud tuvastamiseks teemade üle sadu tuhandetele kirjed.


### <a name="topics--submit-job"></a>Teemade – Edasta töö

**URL-I**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Näide taotlus**


Allpool postituse kõne me taotlemise teemade hulk 100 artiklite, kus on näidatud esimese ja viimase Sisestuskeel artiklite ja kahe StopPhrases kaasatakse.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Taotlege keha.

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

Vastus allpool, saate selle JobId esitatud töö:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Üksiku sõna või mitme sõna laused, mida ei saa tagastatuna teemade loend. Saab kasutada väga üldine teemade välja filtreerida. Näiteks andmekomplekt teha arvustuste kohta, "teha" ja "asukoht" võib olla mõistlikum Peata fraasid.  

### <a name="topics--poll-for-job-results"></a>Teemade – küsitluse tulemuste töö

**URL-I**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Näide taotlus**

Liigu JobId, tagastatakse "Edasta töö" juhises toomiseks tulemused. Soovitame selle lõpp-punkti kõne iga minut kuni olek = "Lõpuleviimine" vastuse. Kulub umbes 10 min töö valmis või rohkem tööde kirjete arvu tuhandete.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Kui see on töötlemist, vastus on järgmine:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


API tagastab väljundi JSON-vormingus järgmises vormingus:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Iga osa vastuse atribuudid on järgmised:

**TopicInfo atribuudid**

| Klahv | Kirjeldus |
|:-----|:----|
| TopicId | Iga teema ainuidentifikaator. |
| Keskmine | Teema määratud kirjete arvu. |
| KeyPhrase | Koondväljavõtteid sõna või fraasi teema. 1 või mitme sõna võib olla. |

**TopicAssignment atribuudid**

| Klahv | Kirjeldus |
|:-----|:----|
| ID | Identifikaator kirje. Võrdub kaasatud sisend ID-d. |
| TopicId | Teema ID, mis on määratud kirjet. |
| Kauguse | Turvaline, et kirje kuulub teema. Kaugus lähemale null näitab kõrgema confidence. |
