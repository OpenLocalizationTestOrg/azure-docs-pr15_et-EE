<properties
pageTitle="Otsingu Azure'i bloobimälu indekseerija JSON plekid indekseerimine"
description="Otsingu Azure'i bloobimälu indekseerija JSON plekid indekseerimine"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Otsingu Azure'i bloobimälu indekseerija JSON plekid indekseerimine 

Selles artiklis kirjeldatakse, kuidas konfigureerida otsingu Azure'i bloobimälu indekseerija liigendatud sisu ekstraktimiseks plekid, mis sisaldavad JSON.

## <a name="scenarios"></a>Stsenaariumid

Vaikimisi [Otsingu Azure'i bloobimälu indekseerija](search-howto-indexing-azure-blob-storage.md) sõelub JSON plekid ühe kogumi teksti. Sageli soovite säilitada oma JSON dokumentide struktuuri. Näiteks antud JSON dokument 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Võite sõeluda seda dokumenti Azure'i otsing "tekst", "datePublished" ja "sildid" väljad.

Teise võimalusena kui oma plekid sisaldavad **massiivi JSON objektide**, võite saada eraldi Azure'i Otsing dokumendi massiivi iga element. Näiteks antud mõne bloobimälu koos selle JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

saate asustada Azure'i otsinguregistri 3 eraldi dokumendid, kus iga väljadega "id" ja "tekst". 

> [AZURE.IMPORTANT] See funktsioon on praegu eelvaade. See on saadaval ainult versioon **2015 02 28 eelvaates**abil REST API-ga. Palun pidage meeles, et eelvaade on mõeldud katse- ja API-d ja Tootmiskeskkondades ei tohi kasutada. 

## <a name="setting-up-json-indexing"></a>Kuidas häälestada JSON indekseerimine

JSON plekid indekseerida, määrata selle `parsingMode` konfiguratsiooni parameeter `json` (kui soovite indekseerida iga bloobimälu ühe dokumendina) või `jsonArray` (kui teie plekid sisaldavad JSON massiiv): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Vajadusel kasutage **välja vastendused** target otsinguregistri asustamiseks kasutatav andmeallika JSON dokumendi atribuute.  See on kirjeldatud allpool üksikasjad. 

> [AZURE.IMPORTANT] Kui kasutate `json` või `jsonArray` sõelumine režiimi, Azure'i otsingu eeldab, et kõik plekid andmeallika saab JSON. Kui teil on vaja toetada sama andmeallika JSON- ja JSON plekid segu, palun andke meile teada sarnaneb [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Väli vastenduste abil saate otsingu dokumentide koostamine 

Praegu Azure'i otsing ei saa indekseerida suvalise JSON dokumendid otse, kuna see toetab ainult lihtsad andmetüübid, stringi massiivi ja GeoJSON punkte. Siiski saate **välja vastendused** valige JSON dokumendi osa ja "tõstke" need Otsing dokumendi ülataseme väljadele. Välja vastendused põhitoimingute kohta leiate teemast [Azure otsingu indekseerija välja vastendused ületada andmeallikate ja otsing registrite erinevused](search-indexer-field-mappings.md).

Meie näite JSON dokumenti jälle: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Oletame, et teil on otsinguregistrisse, kus järgmised väljad: `text` tüüpi Edm.String, `date` tüüpi Edm.DateTimeOffset, ja `tags` tüüpi Collection(Edm.String). Vastendamiseks oma JSON soovitud kujundit, kasutage välja vastendused järgmist: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Andmeallika väljanimede vastendused on määratud märke [JSON kursori](http://tools.ietf.org/html/rfc6901) abil. Saate alustada kaldkriips viidata root JSON dokumendi, siis minge soovitud atribuuti (suvalise tasemel pesastustasemega), kasutades edasi kaldkriips eraldatud tee süvitsi. 

Üksikute massiivi elementide võib viidata ka 0-põhine registri abil. Valige esimene element "sildid" massiivi eeltoodud näites kaudu, kasutage välja vastenduse umbes järgmine:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Kui andmeallika välja nimi välja vastenduse Path viitab atribuut, mida pole JSON, et vastenduse vahele ilma tõrge. Seda, et toetame dokumendid, millel on erinevad skeemi (mis on levinud kasutamine). Kuna ei ole kinnitamine, peate Olge ettevaatlik välja vastenduse kirjelduses kirjavigade vältimiseks. 

Kui JSON dokumentide sisaldavad ainult lihtsa ülataseme atribuudid, te ei pea välja vastendused üldse. Näiteks kui teie JSON näeb välja selline, ülataseme atribuudid "tekst", "datePublished" ja "sildid" kas otse vastendamine otsinguregistri vastava välja: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Pesastatud JSON massiivi indekseerimine

Mida teha, kui soovite indekseerida massiivi JSON objektid, kuid see massiiv on pesastatud kuhugi dokumenti? Atribuut, mis sisaldab massiivi abil saate valida soovitud `documentRoot` konfiguratsiooni atribuut. Näiteks kui teie plekid välja nägema selline: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

selle konfiguratsiooni abil sisalduvate atribuudi "level2" massiivi indeks. 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Näited

Lihtsate see kõik koos siin on lõpule viidud lõhkelaengu näited. 

Andmeallikas: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indekseerija:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Aidake meil Azure'i otsingu paremaks muuta

Kui teil on soovitada funktsioone või täiustused ideid, palun jõuda meile meie [UserVoice saidi](https://feedback.azure.com/forums/263029-azure-search/).