<properties
pageTitle="Azure'i Tabelimälu Azure otsingu indekseerimine"
description="Saate teada, kuidas Azure'i otsingu Azure'i tabelites talletatavad andmed"
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Azure'i Tabelimälu Azure otsingu indekseerimine

Selles artiklis kirjeldatakse talletatud Azure'i Tabelimälu viiteandmete Azure'i otsingu abil. Uue Azure otsingu tabeli indekseerija teeb selle protsessi kiireks ja sujuvalt. 

> [AZURE.IMPORTANT] See funktsioon on praegu eelvaade. See on saadaval ainult versioon **2015 02 28 eelvaates** abil REST API-ga ja .NET SDK versioon 2.0 eelvaade. Palun pidage meeles, et eelvaate API-d on mõeldud katse- ja ja Tootmiskeskkondades ei tohi kasutada.

## <a name="setting-up-azure-table-indexing"></a>Kuidas häälestada Azure'i tabeli indekseerimine

Häälestada ja konfigureerida indekseerija Azure'i tabeli, saate Azure'i Search REST API luua ja hallata **indexers** ja **andmeallikate** , nagu on kirjeldatud [indekseerija toimingud](https://msdn.microsoft.com/library/azure/dn946891.aspx). Samuti saate .NET SDK [versioon 2.0 – eelvaade](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) . Tabeli indekseerimine lisatakse Azure portaali tulevikus abi.

Andmeallika saate määrata, milliseid andmeid indekseerida, mandaati juurdepääsemiseks andmed ja -poliitikad, mis võimaldavad Azure'i otsingu tõhus kindlaks muudatused andmed (uued, muudetud või kustutatud read).

Indekseerija loeb andmeallikast pärinevate andmete ja laaditakse target otsing indeks.

Tabeli indekseerimine seadistamine

1. Andmeallika loomine
    - Määrake soovitud `type` parameeter.`azuretable`
    - Andke oma salvestusruumi konto ühendusstring nimega on `credentials.connectionString` parameeter.
    - Määrake tabeli nime abil on `container.name` parameeter.
    - Soovi korral saate määrata mõne päringu abil soovitud `container.query` parameeter. Võimaluse korral kasutada filtrit PartitionKey parima jõudluse; mis tahes muud päringu tulemuseks täielik kontrollima, mis võib põhjustada halva toimimise suurte tabelite jaoks.
2. Saate luua skeem, mis vastab tabelis, mida soovite indekseerida veergudele otsing indeks. 
3. Andmeallika otsinguregistri ühenduse loomiseks selle indekseerija.

### <a name="create-data-source"></a>Andmeallika loomine

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Andmeallika loomine API kohta leiate lisateavet teemast [Andmeallika loomine](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Registri loomine 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Registri loomine API kohta leiate lisateavet teemast [Registri loomine](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Indekseerija loomine 

Lõpuks luua andmeallika ja target registri viitav indekseerija. Näiteks:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Luua indekseerijat API kohta lisateabe saamiseks lugege teemat [Indekseerija loomine](search-api-indexers-2015-02-28-preview.md#create-indexer).

See on kõik on seda - indekseerimine palju õnne!

## <a name="dealing-with-different-field-names"></a>Erinevate väljanimesid tegelemine

Sageli olla oma olemasolevat registrit väljanimed erinevaid tabeli atribuudi nimesid. Saate **välja vastendused** atribuudi nimed tabeli väljanimesid otsinguregistri vastendamiseks. Väli vastenduste kohta leiate lisateavet teemast [Azure otsingu indekseerija välja vastendused ületada andmeallikate ja otsing registrite erinevused](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Töötlemine dokumendi võtmed

Azure'i otsinguväljale dokumendi võti kordumatult dokumendi. Iga otsinguregistri peab olema täpselt ühte tüüpi välja `Edm.String`. Võtme väli on nõutav iga dokumendi, mis lisatakse registri (tegelikult on ainult vajalik välja).

Kuna tabeli read on liitindeksi klahvi, Azure'i otsingu genereeritud sünteetiliste välja `Key` literaalide partition võti ja rea väärtused on. Näiteks juhul, kui rea PartitionKey on `PK1` ja RowKey on `RK1`, siis `Key` välja väärtus on `PK1RK1`. 

> [AZURE.NOTE] Funktsiooni `Key` väärtus võib sisaldada dokumendi Keys, nt kriipsud sobimatuid märke. Saate tegeleda lubamatud märgid, kasutades funktsiooni `base64Encode` [välja vastenduse funktsioon](search-indexer-field-mappings.md#base64EncodeFunction). Kui te seda teete, ärge unustage kasutada ka URL-i ohutu Base64 kodeeringus kui läbides dokumendi klahvid API helistab näiteks otsing.

## <a name="incremental-indexing-and-deletion-detection"></a>Suureneva indekseerimine ja kustutamise tuvastamine
 
Tabeli indekseerija käivitamiseks ajakava häälestamisel see reindexes ainult uus või värskendatud read, on rea määratud `Timestamp` väärtus. Teil pole vaja määrata muutmine tuvastamise poliitika – suureneva indekseerimine on lubatud teie jaoks automaatselt. 

Näitab, et teatud dokumendid tuleb eemaldada registri abil saate pehmeid Kustuta strateegia – selle asemel, et kustutada rida, näitamaks, et see on kustutatud ja pehmed kustutamise tuvastamise poliitika andmeallikas häälestamine atribuudi lisamine. Näiteks alltoodud poliitika kaaluge rida on kustutatud, kui atribuut on `IsDeleted` väärtusega `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Aidake meil Azure'i otsingu paremaks muuta

Kui teil on soovitada funktsioone või täiustused ideid, palun jõuda meile meie [UserVoice saidi](https://feedback.azure.com/forums/263029-azure-search/).