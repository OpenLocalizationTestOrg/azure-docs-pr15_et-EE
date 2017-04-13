<properties
pageTitle="OneDrive for Business | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Ühenduse loomine teenuse OneDrive for Business failide haldamiseks. Saate teha erinevaid toiminguid üles, nt värskendada, leida ja Kustuta failid."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-onedrive-for-business-connector"></a>OneDrive for Business Connectori alustamine

Ühenduse loomine teenuse OneDrive for Business failide haldamiseks. Saate teha erinevaid toiminguid üles, nt värskendada, leida ja Kustuta failid.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse nüüd leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

OneDrive for Business Connectori saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 OneDrive for Business connector on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="onedrive-for-business-actions"></a>OneDrive for Business toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Laadib metaandmete faili OneDrive for Business abil id|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|OneDrive for Businessis asuva faili värskendused|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Kustutab faili OneDrive for Businessi kaudu|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Metaandmete faili OneDrive for Business abil tee uuesti|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|Toob teenuses OneDrive for Business abil tee faili sisu|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|Toob sisu faili OneDrive for Business abil id|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Faili üleslaadimine teenusesse OneDrive for Business|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Kopeerib faili teenusesse OneDrive for Business|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|OneDrive for Business kausta failide loendid|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Failide OneDrive for Businessi juurkausta loendid|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|Ekstraktib kausta teenusesse OneDrive for Business|
### <a name="onedrive-for-business-triggers"></a>OneDrive for Businessi käivitab
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Kui fail on loodud|Käivitab vool OneDrive for Business kausta uue faili loomisel|
|Kui faili on muudetud|Käivitab vool, kui faili on muudetud OneDrive for Business kausta|


## <a name="create-a-connection-to-onedrive-for-business"></a>OneDrive for Business ühenduse loomine
OneDrive for Businessi loogika rakenduste loomiseks peate esmalt **ühenduse** loomine siis üksikasjad pakuvad järgmised atribuudid: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|Sisestage OneDrive for Businessi identimisteabe|
Pärast ühenduse loomist saate käivitada toiminguid ja kuulake päästikute, selles artiklis kirjeldatud. 

>[AZURE.INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="reference-for-onedrive-for-business"></a>OneDrive for Businessi tutvustus
Kehtib versioon: 1.0

## <a name="getfilemetadata"></a>GetFileMetadata
Hankida faili metaandmete id abil: toob metaandmete faili OneDrive for Business abil id 

```GET: /datasets/default/files/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Määrake faili|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="updatefile"></a>UpdateFile
Update-fail: värskendab faili OneDrive for Business 

```PUT: /datasets/default/files/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Määrake faili värskendamiseks|
|sisu| |Jah|sisu|ükski|OneDrive for Business värskendada faili sisu|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="deletefile"></a>DeleteFile
Kustutage fail: kustutab faili OneDrive for Businessi kaudu 

```DELETE: /datasets/default/files/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Määrake faili kustutamine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="getfilemetadatabypath"></a>GetFileMetadataByPath
Hankida faili metaandmete teed kasutades: toob metaandmete faili OneDrive for Business abil tee 

```GET: /datasets/default/GetFileByPath``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|tee|string|Jah|päringu|ükski|Kordumatu tee fail OneDrive for Business|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="getfilecontentbypath"></a>GetFileContentByPath
Faili sisu teed kasutades: toob teenuses OneDrive for Business abil tee faili sisu 

```GET: /datasets/default/GetFileContentByPath``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|tee|string|Jah|päringu|ükski|Kordumatu tee fail OneDrive for Business|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="getfilecontent"></a>GetFileContent
Faili sisu id abil: toob sisu faili OneDrive for Business abil id 

```GET: /datasets/default/files/{id}/content``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Määrake faili|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="createfile"></a>CreateFile
Faili loomine: Faili üleslaadimine teenusesse OneDrive for Business 

```POST: /datasets/default/files``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|folderPath|string|Jah|päringu|ükski|Faili üleslaadimine teenusesse OneDrive for Business kausta tee|
|Nimi|string|Jah|päringu|ükski|OneDrive for Business loomine faili nimi|
|sisu| |Jah|sisu|ükski|Faili üleslaadimine teenusesse OneDrive for Business sisu|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="copyfile"></a>CopyFile
Kopeerige fail: kopeerib faili teenusesse OneDrive for Business 

```POST: /datasets/default/copyFile``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Allikas|string|Jah|päringu|ükski|URL-i lähtefail|
|sihtkoht|string|Jah|päringu|ükski|Sihtkoha faili tee teenuses OneDrive for Business, sh sihtrakenduse nimi|
|ülekirjutamine|kahendmuutuja|Ei|päringu|FALSE|Sihtfail kirjutatakse üle, kui on seatud "true"|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="onnewfile"></a>OnNewFile
Kui fail on loodud: käivitab vool OneDrive for Business kausta uue faili loomisel 

```GET: /datasets/default/triggers/onnewfile``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|kausta ID|string|Jah|päringu|ükski|Määrake kaust|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="onupdatedfile"></a>OnUpdatedFile
Kui faili on muudetud: käivitab vool, kui faili on muudetud OneDrive for Business kausta 

```GET: /datasets/default/triggers/onupdatedfile``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|kausta ID|string|Jah|päringu|ükski|Määrake kaust|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="listfolder"></a>ListFolder
Kausta failide loendi: OneDrive for Businessi kausta failide loendid 

```GET: /datasets/default/folders/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Määrake kaust|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="listrootfolder"></a>ListRootFolder
Loendi juurkausta: failid OneDrive for Businessi juurkausta loendid 

```GET: /datasets/default/folders``` 

Pole selle kõne parameetrid
#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="extractfolderv2"></a>ExtractFolderV2
Kausta ekstraktida: ekstraktib teenusesse OneDrive for Business kausta 

```POST: /datasets/default/extractFolderV2``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Allikas|string|Jah|päringu|ükski|Arhiivi faili tee|
|sihtkoht|string|Jah|päringu|ükski|OneDrive for Business arhiivi sisu ekstraktimiseks tee|
|ülekirjutamine|kahendmuutuja|Ei|päringu|FALSE|Sihtkoha failid kirjutatakse üle, kui on seatud "true"|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused 

### <a name="datasetsmetadata"></a>DataSetsMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|tabelina|määratletud ei|Ei |
|bloobimälu|määratletud ei|Ei |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Allikas|string|Ei |
|displayName|string|Ei |
|urlEncoding|string|Ei |
|tableDisplayName|string|Ei |
|tablePluralName|string|Ei |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Allikas|string|Ei |
|displayName|string|Ei |
|urlEncoding|string|Ei |



### <a name="blobmetadata"></a>BlobMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Ei |
|Nimi|string|Ei |
|DisplayName|string|Ei |
|Tee|string|Ei |
|LastModified|string|Ei |
|Suurus|täisarv|Ei |
|Meediumitüübil|string|Ei |
|IsFolder|kahendmuutuja|Ei |
|ETag|string|Ei |
|FileLocator|string|Ei |



### <a name="object"></a>Objekti


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)