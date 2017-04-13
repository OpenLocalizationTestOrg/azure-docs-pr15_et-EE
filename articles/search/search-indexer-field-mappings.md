<properties
pageTitle="Väli vastendused Azure'i otsingu indexers rakenduses"
description="Azure'i otsingu indekseerija välja vastendused erinevusi väljanimed ja andmete esinduste konto konfigureerimine"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Väli vastendused Azure'i otsingu indexers rakenduses

Azure'i otsingu indexers kasutamisel leiate ise aeg-ajalt olukordades, kus teie sisendandmete üsna ei vasta teie target index skeemiga. Sel juhul saate **välja vastendused** andmete soovitud kuju muuta. 

Teatud olukordades, kus väli vastendused on kasulik:
 
- Andmeallikas on välja `_id`, kuid Azure'i otsing ei luba väljanimesid, alustades allkriips. Väljade vastendamiseks võimaldab välja "ümbernimetamine". 
- Soovite registri samade andmetega andmete allikas, nt mitme välja asustamiseks, kuna soovite rakendada eri analüsaatorid nende väljade. Väli vastendused teile "kahvli" välja andmete allikas.
- Teil on vaja Base64 kodeerida või dekodeerida oma andmed. Välja vastendused toetab mitut **vastenduse funktsioone**, sealhulgas funktsioonide Base64 kodeerimise ja dekodeerimise.   


> [AZURE.IMPORTANT] Praegu on välja vastendused funktsioonide eelvaates. See on saadaval ainult versioon **2015 02 28 eelvaates**abil REST API-ga. Palun pidage meeles, et eelvaate API-d on mõeldud katse- ja ja Tootmiskeskkondades ei tohi kasutada.

## <a name="setting-up-field-mappings"></a>Kuidas häälestada välja vastendused

Kui loote uue indekseerija, [Luua indekseerijat](search-api-indexers-2015-02-28-preview.md#create-indexer) API abil saate lisada välja vastendused. Väli vastendused indekseerimise indekseerija [Update indekseerijat](search-api-indexers-2015-02-28-preview.md#update-indexer) API abil saate hallata. 

Väljade vastendamiseks koosneb 3 osast: 

1. A `sourceFieldName`, mis tähistab andmeallika välja. Atribuut on nõutav. 

2. Valikulise `targetFieldName`, mis tähistab otsinguregistri välja. Kui välja jäetud, kasutatakse andmeallika nimi. 

3. Valikulise `mappingFunction`, kus saate muuta oma andmed, kasutades ühte mitmest eelmääratletud funktsioonid. [Allpool](#mappingFunctions)on funktsioonide täieliku loendi.

Väljade vastendused lisatakse selle `fieldMappings` massiiv indekseerija määratluse. 

Näiteks siin on, kuidas saab majutada väljanimesid erinevused: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indekseerija võib olla mitu välja vastendused. Oletagem näiteks, siin on, kuidas te saate "kahvli" välja:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure'i otsing kasutab väiketähed võrdlus lahendamiseks välja vastendused välja ja funktsioon nimed. See on mugav (teil pole õige kõik kest), kuid see tähendab, et teie andmeallika või index ei tohi olla väljad, mis erinevad vaid juhul.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Välja vastenduse funktsioonid

Praegu on toetatud järgmised funktsioonid: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Sooritab *URL-i ohutu* Base64 kodeeringus Sisestuskeel string. Eeldab, et on kodeeritud UTF-8. 

#### <a name="sample-use-case"></a>Valimi kasutamine juhul 

Ainult URL-i ohutu märgid võivad ilmuda on Azure Otsing dokumendi võti (kuna kliendid peate saama kasutades otsingu API, nt dokumendi aadress). Kui teie andmed sisaldavad URL-i ebaturvaliste märgid ja soovite seda võtme välja otsinguregistri asustamiseks kasutada, kasutage seda funktsiooni.   

#### <a name="example"></a>Näide 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Sooritab Base64 dekodeerimise Sisestuskeel string. Sisend eeldatakse, et *URL-i ohutu* Base64-kodeeringuga stringi. 

#### <a name="sample-use-case"></a>Valimi kasutamine juhul 

Bloobimälu kohandatud metaandmete väärtused peavad olema ASCII-kodeeringuga. Saate Base64 kodeeringus tähistada suvalise Unicode'i stringid bloobimälu kohandatud metaandmete. Siiski teha otsingu mõtestatud abil saate selle funktsiooni omakorda kodeeritud andmed uuesti "tavalisi" stringide ilma vaevata luua otsinguregistri.  

#### <a name="example"></a>Näide 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Jagab stringi väli, kasutades määratud eraldaja ja huvitavat luba tulemuseks tükeldamine määratud asukohas.

Näiteks kui on `Jane Doe`, `delimiter` on `" "`(tühik) ja `position` on 0, on tulem `Jane`; Kui soovitud `position` 1, on tulem on `Doe`. Kui asukoha viitab luba, mida pole olemas, tagastatakse tõrge.

#### <a name="sample-use-case"></a>Valimi kasutamine juhul 

Andmeallika sisaldab mõnda `PersonName` välja, ja soovite indekseerida, et eraldi `FirstName` ja `LastName` väljad. Selle funktsiooni abil saate jagada, Tühikumärgi kasutamine eraldajana sisend.

#### <a name="parameters"></a>Parameetrid

- `delimiter`: string Sisestuskeel stringi tükeldamine eraldaja kasutada.
- `position`: täisarv 0-põhine asukoha valimiseks pärast Sisestuskeel stringi jagatakse luba.    

#### <a name="example"></a>Näide

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Muudab vormindatud JSON massiivi stringide stringi massiiv, mida saab kasutada asustamiseks string on `Collection(Edm.String)` registris olev välja. 

Näiteks kui Sisestuskeel string on `["red", "white", "blue"]`, siis sihtvälja tüüp `Collection(Edm.String)` on täidetud kolme väärtustega `red`, `white` ja `blue`. Sisestatud andmed, mida ei saa sõeluda nimega JSON stringi massiivi, tagastatakse tõrge. 

#### <a name="sample-use-case"></a>Valimi kasutamine juhul

SQL Azure'i andmebaas ei sisalda sisseehitatud andmetüüp, et loomulikult kaarte `Collection(Edm.String)` Azure väljad. Stringi saidikogumi väljad asustamiseks JSON stringi massiivi allika andmete vormindamiseks ja kasutage seda funktsiooni. 

#### <a name="example"></a>Näide 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Aidake meil Azure'i otsingu paremaks muuta

Kui teil on soovitada funktsioone või täiustused ideid, palun jõuda meile meie [UserVoice saidi](https://feedback.azure.com/forums/263029-azure-search/).