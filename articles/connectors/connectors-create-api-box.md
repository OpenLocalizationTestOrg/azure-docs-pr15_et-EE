<properties
    pageTitle="Loogika rakenduste lisamine väljale konnektor | Microsoft Azure'i"
    description="Väljale konnektor REST API parameetritega ülevaade"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>Alustamine väljale konnektor
Ühenduse loomine väljale ja looge faile, Kustuta failid ja muud. 

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon.

Klõpsake dialoogiboksis saate teha järgmist.

- Koostage oma voog kuvatakse väljal andmete põhjal. 
- Päästikute kasutamine, kui fail on loodud või värskendatud.
- Kasutage toiminguid, mis kopeerida faili, kustutada faili ja palju muud. Neid toiminguid vastuse saamine, ja seejärel tehke väljund muude toimingute jaoks saadaval. Näiteks kui fail on muutunud ruut, saate selle faili ja e-posti see teenusekomplekti Office 365.

Saate lisada toimingu loogika rakendustes, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud
Lahter sisaldab järgmisi päästik ja toimingud.

| Päästikute | Toimingud|
| --- | --- |
|<ul><li>Kui fail on loodud</li><li>Kui faili on muudetud</li></ul> | <ul><li>Faili loomine</li><li>Kui fail on loodud</li><li>Faili kopeerimine</li><li>Faili kustutamine</li><li>Ekstrakti archive kausta</li><li>Faili sisu abil id hankimine</li><li>Faili sisu teed kasutades</li><li>Faili metaandmete abil id hankimine</li><li>Faili metaandmete teed kasutades hankimine</li><li>Update-faili</li><li>Kui faili on muudetud</li></ul>

Kõik konnektorid toetavad andmete JSON ja XML-vormingus.

## <a name="create-a-connection-to-box"></a>Väljale ühenduse loomine
Selle konnektori lisamisel loogika rakenduste annate loogika rakenduste ühenduse loomiseks oma ruut.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Kui olete loonud ühenduse, sisestage soovitud atribuudid. **REST API viide** selles teemas kirjeldatakse neid atribuute.

>[AZURE.TIP] Saate kasutada samal väljal sellega loogika muudes rakendustes.

## <a name="swagger-rest-api-reference"></a>Ärplema REST API viide
Kehtib versioon: 1.0.

### <a name="create-file"></a>Faili loomine
Lisatud faili ruut.  
```POST: /datasets/default/files```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|folderPath|string|Jah|päringu|Ükski |Kausta tee väljale Faili üleslaadimine|
|Nimi|string|Jah|päringu|Ükski |Faili loomine väljale nimi|
|sisu|string(Binary) |Jah|sisu|Ükski |Faili üleslaadimine ruut sisu|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="when-a-file-is-created"></a>Kui fail on loodud
Käivitab vool uue faili loomisel väljale kausta.  
```GET: /datasets/default/triggers/onnewfile```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|kausta ID|string|Jah|päringu|Ükski |Väljal kausta ainuidentifikaator|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="copy-file"></a>Faili kopeerimine
Kopeerib faili ruut.  
```POST: /datasets/default/copyFile```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Allikas|string|Jah|päringu|Ükski |URL-i lähtefail|
|sihtkoht|string|Jah|päringu| Ükski|Sihtkoha faili tee väljale, sh sihtrakenduse nimi|
|ülekirjutamine|kahendmuutuja|Ei|päringu| Ükski|Sihtfail kirjutatakse üle, kui on seatud "true"|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="delete-file"></a>Faili kustutamine
Kustutab faili ruut.  
```DELETE: /datasets/default/files/{id}```


| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|Ükski |Faili kustutamine boksist ainuidentifikaator|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="extract-archive-to-folder"></a>Ekstrakti archive kausta
Ekstraktib arhiivifaili väljale kausta (näide: zip).  
```POST: /datasets/default/extractFolderV2```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Allikas|string|Jah|päringu| |Arhiivi faili tee|
|sihtkoht|string|Jah|päringu| |Tee väljale arhiivi sisu ekstraktimiseks|
|ülekirjutamine|kahendmuutuja|Ei|päringu| |Sihtkoha failid kirjutatakse üle, kui on seatud "true"|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="get-file-content-using-id"></a>Faili sisu abil id hankimine
Toob faili sisu väljal id abil.  
```GET: /datasets/default/files/{id}/content```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|Ükski |Faili väljale ainuidentifikaator|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="get-file-content-using-path"></a>Faili sisu teed kasutades
Toob teed kasutades väljal faili sisu.  
```GET: /datasets/default/GetFileContentByPath```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|tee|string|Jah|päringu|Ükski |Kordumatu väljal faili tee|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="get-file-metadata-using-id"></a>Faili metaandmete abil id hankimine
Faili metaandmete toob väljal faili id abil.  
```GET: /datasets/default/files/{id}```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee| Ükski|Faili väljale ainuidentifikaator|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="get-file-metadata-using-path"></a>Faili metaandmete teed kasutades hankimine
Toob faili metaandmete abil tee väljale.  
```GET: /datasets/default/GetFileByPath```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|tee|string|Jah|päringu|Ükski |Kordumatu väljal faili tee|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="update-file"></a>Update-faili
Uuendab väljal faili.  
```PUT: /datasets/default/files/{id}```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee| Ükski|Faili värskendamiseks väljale ainuidentifikaator|
|sisu|string(Binary) |Jah|sisu|Ükski |Väljale värskendama faili sisu|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


### <a name="when-a-file-is-modified"></a>Kui faili on muudetud
Käivitab vool, kui faili on muudetud väljale kausta.  
```GET: /datasets/default/triggers/onupdatedfile```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|kausta ID|string|Jah|päringu|Ükski |Väljal kausta ainuidentifikaator|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Atribuudi nimi | Andmetüüp | Nõutav|
|---|---|---|
|tabelina|määratletud ei|Ei|
|bloobimälu|määratletud ei|Ei|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|Allikas|string|Ei|
|displayName|string|Ei|
|urlEncoding|string|Ei|
|tableDisplayName|string|Ei|
|tablePluralName|string|Ei|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|Allikas|string|Ei|
|displayName|string|Ei|
|urlEncoding|string|Ei|

#### <a name="blobmetadata"></a>BlobMetadata

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|Nimi|string|Ei|
|DisplayName|string|Ei|
|Tee|string|Ei|
|LastModified|string|Ei|
|Suurus|täisarv|Ei|
|Meediumitüübil|string|Ei|
|IsFolder|kahendmuutuja|Ei|
|ETag|string|Ei|
|FileLocator|string|Ei|

## <a name="next-steps"></a>Järgmised sammud

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).
