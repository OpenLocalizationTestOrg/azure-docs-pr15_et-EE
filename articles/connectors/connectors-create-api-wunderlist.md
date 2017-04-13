<properties
pageTitle="Wunderlist | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Wunderlist pakuvad todo loendi ja ülesannete halduri hõlbustamiseks nende rühmas.  Kas soovite dokumenti jagada kallimale ostunimekiri, projektiga töötamisele, või plaanite mõnda puhkuse Wunderlist on lihtne jäädvustada, ühiskasutus ja täitke oma to¬dos. Wunderlist sünkroonib kohe telefoni, tahvelarvuti ja arvutis, et pääseksite kõigi tööülesannete igalt poolt."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Alustamine Wunderlist konnektor

Wunderlist pakuvad todo loendi ja ülesannete halduri hõlbustamiseks nende rühmas.  Kas soovite dokumenti jagada kallimale ostunimekiri, projektiga töötamisele, või plaanite mõnda puhkuse Wunderlist on lihtne jäädvustada, ühiskasutus ja täitke oma to¬dos. Wunderlist sünkroonib kohe telefoni, tahvelarvuti ja arvutis, et pääseksite kõigi tööülesannete igalt poolt.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse kohe leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

Wunderlist konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 Wunderlist konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="wunderlist-actions"></a>Wunderlist toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Saate tuua loendid, mis on teie kontoga seotud.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Loendi loomine.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Tuua teatud loendi Tööülesanded.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Tööülesande loomine|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Kindla tööülesande või konkreetsesse nimekirja saate tuua alamülesanded.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Kindla tööülesande sees alamtoimingu loomine|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Märkmete kindla loendi või kindla tööülesande tuua.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Kindla tööülesande märkme lisamiseks|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Saate tuua kindla loendi või kindla tööülesande tööülesande kommentaare.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Kindla tööülesande kommentaari lisamine|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Saate tuua kindla loendi või kindla tööülesande jaoks saate meeldetuletused.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Meeldetuletuse seadmine.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Saate tuua kindla loendi või kindla tööülesande failid.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Laadib konkreetsesse nimekirja|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Kustutab loend|
|[Atribuudiväärtusega](connectors-create-api-wunderlist.md#updatelist)|Kindla loendi värskendamine|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Kindla tööülesande toob|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Kindla tööülesande värskendused|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Kindla tööülesande kustutamine|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Tagastab kindla alamtoimingu|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Teatud alamtoimingu värskendused|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Kustutab teatud alamtoimingu|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Saate tuua märkuse|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|Märkuse värskendamine|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Märkuse kustutamine|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Kindla tööülesande kommentaari tuua|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Värskendage kindla meeldetuletus|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Teatud meeldetuletus kustutamine|
### <a name="wunderlist-triggers"></a>Wunderlist päästikute
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Kui on tööülesande tähtaja|Käivitab uue kulgemist, kui loendis tööülesande tuleb tasuda|
|Uue ülesande loomise|Käivitab uue kulgemist, kui loendis on loodud uue ülesande|
|Meeldetuletuse ilmnemisel|Käivitab uue meilivoo meeldetuletuse ilmnemisel|


## <a name="create-a-connection-to-wunderlist"></a>Wunderlist ühenduse loomine
Loogika rakenduste Wunderlist loomiseks peate esmalt **ühenduse** loomine siis üksikasjad pakuvad järgmised atribuudid: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|Wunderlist mandaat|
Pärast ühenduse loomist saate käivitada toiminguid ja kuulake päästikute, selles artiklis kirjeldatud. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="reference-for-wunderlist"></a>Wunderlist tutvustus
Kehtib versioon: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Kui on tööülesande tähtaja: käivitab uue kulgemist, kui loendis tööülesande tuleb tasuda 

```GET: /trigger/tasksdue``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|


## <a name="triggertasknew"></a>TriggerTaskNew
Uue ülesande loomise: käivitab uue kulgemist, kui loendis on loodud uue ülesande 

```GET: /trigger/tasksnew``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|


## <a name="triggerreminder"></a>TriggerReminder
Meeldetuletuse ilmnemisel: käivitab uue meilivoo meeldetuletuse ilmnemisel 

```GET: /trigger/reminders``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|task_id|täisarv|Ei|päringu|ükski|Ülesande ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|


## <a name="retrievelists"></a>RetrieveLists
Saada loendid: loendid, mis on teie kontoga seostatud tuua. 

```GET: /lists``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="createlist"></a>CreateList
Loendi loomine: loendi loomine. 

```POST: /lists``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse| |Jah|sisu|ükski|Uue loendi loomiseks|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|Vaikimisi|Toiming nurjus.|


## <a name="listtasks"></a>ListTasks
Saada tööülesanded: tuua teatud loendi Tööülesanded. 

```GET: /tasks``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|lõpetatud|kahendmuutuja|Ei|päringu|ükski|Lõpetatud|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="createtask"></a>CreateTask
Tööülesande loomine: tööülesande loomine 

```POST: /tasks``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse| |Jah|sisu|ükski|Uue tööülesande loomiseks|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|201|Loodud|


## <a name="listsubtasks"></a>ListSubTasks
Saada alamülesanded: tuua alamülesanded konkreetsesse nimekirja või kindla tööülesande. 

```GET: /subtasks``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|task_id|täisarv|Ei|päringu|ükski|Ülesande ID|
|lõpetatud|kahendmuutuja|Ei|päringu|ükski|Lõpetatud|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="createsubtask"></a>CreateSubTask
Looge alamtoimingu: alamtoimingu jooksul kindla tööülesande loomine 

```POST: /subtasks``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse| |Jah|sisu|ükski|Kui soovite luua uue alamtoimingu|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|201|Loodud|


## <a name="listnotes"></a>ListNotes
Märkmete hankimine: saate tuua märkmete kindla loendi või kindla tööülesande. 

```GET: /notes``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|task_id|täisarv|Ei|päringu|ükski|Ülesande ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="createnote"></a>CreateNote
Märkme loomine: kindla tööülesande märkme lisamiseks 

```POST: /notes``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse| |Jah|sisu|ükski|Uue märkme loomiseks|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|201|Loodud|


## <a name="listcomments"></a>ListComments
Saada tööülesande kommentaarid: tuua konkreetsesse nimekirja või kindla tööülesande tööülesande kommentaare. 

```GET: /task_comments``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|task_id|täisarv|Ei|päringu|ükski|Ülesande ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="createcomment"></a>CreateComment
Kommentaari lisamine tööülesandele: kindla tööülesande kommentaari lisamine 

```POST: /task_comments``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse| |Jah|sisu|ükski|Uue tööülesande kommentaari luua|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|201|Loodud|


## <a name="retrievereminders"></a>RetrieveReminders
Saada meeldetuletused: tuua konkreetsesse nimekirja või kindla tööülesande jaoks saate meeldetuletused. 

```GET: /reminders``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|task_id|täisarv|Ei|päringu|ükski|Ülesande ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="createreminder"></a>CreateReminder
Meeldetuletuse seadmine: meeldetuletuse seadmine. 

```POST: /reminders``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse| |Jah|sisu|ükski|Luuakse uus meeldetuletus|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|Vaikimisi|Toiming nurjus.|


## <a name="retrievefiles"></a>RetrieveFiles
Saada failid: konkreetsesse nimekirja või kindla tööülesande failide toomiseks. 

```GET: /files``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|task_id|täisarv|Ei|päringu|ükski|Ülesande ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|Vigane päring|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="getlist"></a>GetList
Loendi saamiseks: toob konkreetsesse nimekirja 

```GET: /lists/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Loendi ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="deletelist"></a>DeleteList
Loendi kustutamine: kustutab loend 

```DELETE: /lists/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Loendi ID|
|Redaktsioonide|täisarv|Jah|päringu|ükski|Redaktsioonide|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|204|Sisu|


## <a name="updatelist"></a>Atribuudiväärtusega
Loendi värskendamine: konkreetsesse nimekirja värskendamine 

```PATCH: /lists/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Loendi ID|
|postituse| |Jah|sisu|ükski|Loendi üksikasjad|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="gettask"></a>GetTask
Saada tööülesande: toob kindla tööülesande 

```GET: /tasks/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|ID|täisarv|Jah|tee|ükski|Ülesande ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="updatetask"></a>UpdateTask
Tööülesande värskendamine: kindla tööülesande värskendused 

```PATCH: /tasks/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|ID|täisarv|Jah|tee|ükski|Ülesande ID|
|postituse| |Jah|sisu|ükski|Tööülesande üksikasjad|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="deletetask"></a>DeleteTask
Tööülesande kustutamine: kindla tööülesande kustutamine 

```DELETE: /tasks/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|täisarv|Jah|päringu|ükski|Loendi ID|
|ID|täisarv|Jah|tee|ükski|Ülesande ID|
|Redaktsioonide|täisarv|Jah|päringu|ükski|Redaktsioonide|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|204|Sisu|


## <a name="getsubtask"></a>GetSubTask
Saada alamtoimingu: toob teatud alamtoimingu 

```GET: /subtasks/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Alamülesanne ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="updatesubtask"></a>UpdateSubTask
Värskendage alamtoimingu: teatud alamtoimingu värskendused 

```PATCH: /subtasks/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Alamülesanne ID|
|postituse| |Jah|sisu|ükski|Alamülesanne üksikasjad|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="deletesubtask"></a>DeleteSubTask
Kustutage alamtoimingu: kustutab teatud alamtoimingu 

```DELETE: /subtasks/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Alamülesanne ID|
|Redaktsioonide|täisarv|Jah|päringu|ükski|Redaktsioonide|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|204|Sisu|


## <a name="getnote"></a>GetNote
Saada Märkus: saate tuua märkuse 

```GET: /notes/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Märkus ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="updatenote"></a>UpdateNote
Värskendage Märkus: märkuse värskendamine 

```PATCH: /notes/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Märkus ID|
|postituse| |Jah|sisu|ükski|Märkus üksikasjad|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="deletenote"></a>DeleteNote
Märkuse kustutamiseks: märkuse kustutamine 

```DELETE: /notes/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Märkus ID|
|Redaktsioonide|täisarv|Jah|päringu|ükski|Redaktsioonide|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|204|Sisu|


## <a name="getcomment"></a>GetComment
Saada tööülesande kommentaari: tuua kindla tööülesande kommentaari 

```GET: /task_comments/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|string|Jah|tee|ükski|Kommentaari ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="updatereminder"></a>UpdateReminder
Värskendage meeldetuletuse: teatud meeldetuletuse värskendamine 

```PATCH: /reminders/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Meeldetuletuse ID|
|postituse| |Jah|sisu|ükski|Meeldetuletuse üksikasjad|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|


## <a name="deletereminder"></a>DeleteReminder
Meeldetuletuse kustutamine: teatud meeldetuletuse kustutamine 

```DELETE: /reminders/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|ID|täisarv|Jah|tee|ükski|Meeldetuletuse ID-d.|
|Redaktsioonide|täisarv|Jah|päringu|ükski|Redaktsioonide|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|204|Sisu|


## <a name="object-definitions"></a>Objekti määratlused 

### <a name="list"></a>Loend


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|created_at|string|Ei |
|pealkiri|string|Ei |
|list_type|string|Ei |
|tüüp|string|Ei |
|Redaktsioonide|täisarv|Ei |



### <a name="createdlist"></a>CreatedList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|created_at|string|Ei |
|pealkiri|string|Ei |
|Redaktsioonide|täisarv|Ei |
|tüüp|string|Ei |



### <a name="task"></a>Ülesanne


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|assignee_id|täisarv|Ei |
|assigner_id|täisarv|Ei |
|created_at|string|Ei |
|created_by_id|täisarv|Ei |
|due_date|string|Ei |
|list_id|täisarv|Ei |
|Redaktsioonide|täisarv|Ei |
|tärniga|kahendmuutuja|Ei |
|pealkiri|string|Ei |



### <a name="subtask"></a>Alamtoimingu


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|task_id|täisarv|Ei |
|created_at|string|Ei |
|created_by_id|täisarv|Ei |
|Redaktsioonide|string|Ei |
|pealkiri|string|Ei |



### <a name="note"></a>Märkus


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|task_id|täisarv|Ei |
|sisu|string|Ei |
|created_at|string|Ei |
|updated_at|string|Ei |
|Redaktsioonide|täisarv|Ei |



### <a name="comment"></a>Kommentaari


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|task_id|täisarv|Ei |
|Redaktsioonide|täisarv|Ei |
|teksti|string|Ei |
|tüüp|string|Ei |
|created_at|string|Ei |



### <a name="reminder"></a>Meeldetuletuse


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|kuupäev|string|Ei |
|task_id|täisarv|Ei |
|Redaktsioonide|täisarv|Ei |
|tüüp|string|Ei |
|created_at|string|Ei |
|updated_at|string|Ei |



### <a name="createdreminder"></a>CreatedReminder


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|kuupäev|string|Ei |
|task_id|täisarv|Ei |
|Redaktsioonide|täisarv|Ei |
|created_at|string|Ei |
|updated_at|string|Ei |



### <a name="file"></a>Faili


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|URL-i|string|Ei |
|task_id|täisarv|Ei |
|list_id|täisarv|Ei |
|user_id|täisarv|Ei |
|file_name|string|Ei |
|content_type|string|Ei |
|file_size|täisarv|Ei |
|local_created_at|string|Ei |
|created_at|string|Ei |
|updated_at|string|Ei |
|tüüp|string|Ei |
|Redaktsioonide|täisarv|Ei |



### <a name="newtask"></a>NewTask


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|list_id|täisarv|Jah |
|pealkiri|string|Jah |
|assignee_id|täisarv|Ei |
|lõpetatud|kahendmuutuja|Ei |
|recurrence_type|string|Ei |
|recurrence_count|täisarv|Ei |
|due_date|string|Ei |
|tärniga|kahendmuutuja|Ei |



### <a name="newlist"></a>NewList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|pealkiri|string|Jah |



### <a name="newsubtask"></a>NewSubtask


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|list_id|täisarv|Jah |
|task_id|täisarv|Jah |
|pealkiri|string|Jah |
|lõpetatud|kahendmuutuja|Ei |



### <a name="newnote"></a>NewNote


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|list_id|täisarv|Jah |
|task_id|täisarv|Jah |
|sisu|string|Jah |



### <a name="newcomment"></a>NewComment


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|list_id|täisarv|Jah |
|task_id|täisarv|Jah |
|teksti|string|Jah |



### <a name="newreminder"></a>NewReminder


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|list_id|täisarv|Jah |
|task_id|täisarv|Jah |
|kuupäev|string|Jah |



### <a name="updatetask"></a>UpdateTask


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Redaktsioonide|täisarv|Ei |
|pealkiri|string|Ei |
|assignee_id|täisarv|Ei |
|lõpetatud|kahendmuutuja|Ei |
|recurrence_type|string|Ei |
|recurrence_count|täisarv|Ei |
|due_date|string|Ei |
|tärniga|kahendmuutuja|Ei |



### <a name="updatelist"></a>Atribuudiväärtusega


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Redaktsioonide|täisarv|Ei |
|pealkiri|string|Ei |



### <a name="updatesubtask"></a>UpdateSubtask


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Redaktsioonide|täisarv|Ei |
|pealkiri|string|Ei |
|lõpetatud|kahendmuutuja|Ei |



### <a name="updatenote"></a>UpdateNote


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Redaktsioonide|täisarv|Ei |
|sisu|string|Ei |



### <a name="updatereminder"></a>UpdateReminder


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|kuupäev|string|Ei |
|Redaktsioonide|täisarv|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)