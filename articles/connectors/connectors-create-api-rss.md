<properties
pageTitle="RSS-I | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. RSS-i connector võimaldab kasutajatel avaldada ja kanali üksused alla laadida. See lubab ka kasutajad käivitada toiminguid uue üksuse avaldamisel kanalile."
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

# <a name="get-started-with-the-rss-connector"></a>RSS-i konnektor kasutamise alustamine
RSS on web populaarse syndication vorming, mida sageli värskendatud sisu – näiteks ajaveebi kirjed ja uudiste avaldada.  Mitme sisutüübi tootjad pakuvad RSS-kanali tellimine selle kasutajatel.  RSS-i Connectori abil saate tuua kanali teave ja päästik puhul uute üksuste avaldamisel RSS-kanali.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse kohe leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

RSS-i konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 RSS-i konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="rss-actions"></a>RSS-i toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Saada kõik RSS-kanali üksused.|
### <a name="rss-triggers"></a>RSS-i päästikute
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Kui uue kanali üksuse avaldatud|Käivitab töövoo uue kanali avaldamisel|


## <a name="create-a-connection-to-rss"></a>RSS-i ühenduse loomine

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="reference-for-rss"></a>RSS-i viide
Kehtib versioon: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Kui uue kanali üksuse avaldatud: käivitab töövoo uue kanali avaldamisel 

```GET: /OnNewFeed``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|feedUrl|string|Jah|päringu|ükski|Kanali URL-i|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="listfeeditems"></a>ListFeedItems
Loetle kõik RSS-kanali üksuste.: saada kõik RSS-kanali üksused. 

```GET: /ListFeedItems``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|feedUrl|string|Jah|päringu|ükski|Kanali URL-i|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="feeditem"></a>FeedItem


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Jah |
|pealkiri|string|Jah |
|sisu|string|Jah |
|lingid|massiiv|Ei |
|updatedOn|string|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)