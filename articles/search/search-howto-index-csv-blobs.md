<properties
pageTitle="Indekseerimise CSV plekid otsingu Azure'i bloobimälu indekseerija | Microsoft Azure'i"
description="Saate teada, kuidas indekseerida CSV plekid Azure otsingu abil"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Otsingu Azure'i bloobimälu indekseerija CSV plekid indekseerimine 

Vaikimisi [Otsingu Azure'i bloobimälu indekseerija](search-howto-indexing-azure-blob-storage.md) sõelub eraldatud teksti plekid nimega ühe kogumi teksti. Siiski plekid CSV andmeid sisaldav, sageli soovite käsitleda iga rea bloobimälu eraldi dokumendina. Näiteks antud eraldatud järgmine tekst: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

soovite ehk sõeluda 2 dokumendid, iga sisaldav "id", "datePublished" ja "sildid" väljad.

Sellest artiklist saate teada, kuidas sõeluda CSV plekid indekseerija otsingu Azure'i bloobimälu. 

> [AZURE.IMPORTANT] See funktsioon on praegu eelvaade. See on saadaval ainult versioon **2015 02 28 eelvaates**abil REST API-ga. Palun pidage meeles, et eelvaade on mõeldud katse- ja API-d ja Tootmiskeskkondades ei tohi kasutada. 

## <a name="setting-up-csv-indexing"></a>Kuidas häälestada CSV indekseerimine

Indekseerida CSV plekid, loomine või koos mõne indekseerija definitsiooni värskendamiseks on `delimitedText` sõelumine režiimi:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Luua indekseerijat API kohta lisateabe saamiseks lugege teemat [Indekseerija loomine](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`näitab, et iga bloobimälu (tühjaks) esimene rida sisaldab päiseid.
Kui plekid ei sisalda mõnda algse päise rea, peaks päiste indekseerija konfiguratsioonis määratud: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Praegu on toetatud ainult UTF-8 kodeeringus. Ka ainult koma `','` märk on toetatud eraldajana. Kui vajate abi muude kodeeringute või eraldajaid, palun andke meile teada sarnaneb [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Eraldatud tekst sõelumine režiimi kasutamisel Azure'i otsing eeldab, et kõik plekid andmeallika saab CSV. Kui teil on vaja toetada sama andmeallika CSV- ja -CSV plekid segu, palun andke meile teada sarnaneb [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Näited

Lihtsate see kõik koos siin on lõpule viidud last näited. 

Andmeallikas: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indekseerija:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Aidake meil Azure'i otsingu paremaks muuta

Kui teil on soovitada funktsioone või täiustused ideid, palun jõuda meile meie [UserVoice saidi](https://feedback.azure.com/forums/263029-azure-search/).