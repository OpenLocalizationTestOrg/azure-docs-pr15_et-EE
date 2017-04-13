<properties
    pageTitle="Loogika rakenduste lisamine Facebooki konnektor | Microsoft Azure'i"
    description="Facebooki konnektor REST API parameetritega ülevaade"
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

# <a name="get-started-with-the-facebook-connector"></a>Alustamine Facebooki konnektor
Ühenduse loomine Facebookiga ja ajaskaala postitada, saavad kanali lehele ja palju muud. 

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon.


Facebooki, saate teha järgmist.

- Koostage oma voog kuvatakse Facebooki andmete põhjal. 
- Kasutage käivitamiseks sai uue postituse.
- Kasuta toiminguid, mida oma ajajoonele postitamiseks saada kanali lehele ja palju muud. Neid toiminguid vastuse saamine, ja seejärel tehke väljund muude toimingute jaoks saadaval. Näiteks kui teie ajaskaalal on uue postituse, saate selle posti ja lükake see oma Twitteri kanal. 



Saate lisada toimingu loogika rakendustes, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud
Facebooki konnektor sisaldab järgmisi päästik ja toimingud. 

| Päästikute | Toimingud|
| --- | --- |
| <ul><li>Kui seal on uue postituse all ajaskaalal</li></ul> |<ul><li>Minu ajaskaalalt kanali hankimine</li><li>Postitage oma ajaskaalale</li><li>Kui seal on uue postituse all ajaskaalal</li><li>Kanali lehelt</li><li>Saada kasutaja ajaskaala</li><li>Lehe postitamine</li></ul>

Kõik konnektorid toetavad andmete JSON ja XML-vormingus.

## <a name="create-a-connection-to-facebook"></a>Facebooki ühenduse loomine
Selle konnektori lisamisel loogika rakenduste annate loogika rakendused oma Facebooki ühenduse.

1. Oma Facebooki kontosse sisselogimine
2. Valige **Autoriseerin**ja luba loogika rakenduste ühenduse loomiseks ja kasutada oma Facebooki. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Saate seda sama Facebooki ühenduse loogika muudes rakendustes.

## <a name="swagger-rest-api-reference"></a>Ärplema REST API viide
Kehtib versioon: 1.0.

### <a name="get-feed-from-my-timeline"></a>Minu ajaskaalalt kanali hankimine
Sisselogitud kasutaja ajaskaala saab kanaleid.  
```GET: /me/feed```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|väljad|string|Ei|päringu|ükski |Määrake väljad, mida soovite tagastatud. Näide (id, nimi, pilt).|
|limiit|täisarv|Ei|päringu| ükski|Postituste tuuakse arvu ülempiir|
|abil|string|Ei|päringu| ükski|Postituste loendit piirata ainult need asukoht, millele on manustatud.|
|filtreerimine|string|Ei|päringu| ükski|Saate tuua ainult postitusi, mis vastavad teatud voo filter.|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


### <a name="post-to-my-timeline"></a>Postitage oma ajaskaalale
Sisselogitud kasutaja ajajoonele postitamiseks olek.  
```POST: /me/feed```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|postituse|string |Jah|sisu|ükski |Uue sõnumi sisestada|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Kui seal on uue postituse all ajaskaalal
Käivitab uue meilivoo uue postituse ajaskaala sisselogitud kasutaja korral.  
```GET: /trigger/me/feed```

On parameetreid pole. 

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


### <a name="get-page-feed"></a>Kanali lehelt
Kanali määratud lehe postituste toomine.  
```GET: /{pageId}/feed```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|pageId|string|Jah|tee| ükski|Leht, millest on postituste tuuakse ID-d.|
|limiit|täisarv|Ei|päringu| ükski|Postituste tuuakse arvu ülempiir|
|include_hidden|kahendmuutuja|Ei|päringu|ükski |Kas soovite kaasata ka peidetud lehe postitustele|
|väljad|string|Ei|päringu|ükski |Määrake väljad, mida soovite tagastatud. Näide (id, nimi, pilt).|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


### <a name="get-user-timeline"></a>Saada kasutaja ajaskaala
Kasutaja ajaskaala postituste toomine.  
```GET: /{userId}/feed```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Kasutajanimi|string|Jah|tee|ükski |Mille ajaskaala on tuuakse kasutaja ID-d.|
|limiit|täisarv|Ei|päringu|ükski |Postituste tuuakse arvu ülempiir|
|abil|string|Ei|päringu|ükski |Postituste loendit piirata ainult need asukoht, millele on manustatud.|
|filtreerimine|string|Ei|päringu| ükski|Saate tuua ainult postitusi, mis vastavad teatud voo filter.|
|väljad|string|Ei|päringu| ükski|Määrake väljad, mida soovite tagastatud. Näide (id, nimi, pilt).|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


### <a name="post-to-page"></a>Lehe postitamine
Sõnumi postitamine Facebooki lehele sisseloginud kasutajana.  
```POST: /{pageId}/feed```

| Nimi|Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|pageId|string|Jah|tee|ükski |Lehe postitamiseks ID-d.|
|postituse|mitme |Jah|sisu|ükski |Uue sõnumi sisestada.|

#### <a name="response"></a>Vastus
|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused

#### <a name="getfeedresponse"></a>GetFeedResponse

|Atribuudi nimi | Andmetüüp | Nõutav|
|---|---|---|
|andmete|massiiv|Ei|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|andmete|massiiv|Ei|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Kasutaja profiili ühe kanali
Profiili võib olla kasutaja lehe rakenduse või rühma. 

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|admin_creator|massiiv|Ei|
|Pealdis|string|Ei|
|created_time|string|Ei|
|kirjeldus|string|Ei|
|feed_targeting|määratletud ei|Ei|
|kaudu|määratletud ei|Ei|
|ikoon|string|Ei|
|is_hidden|kahendmuutuja|Ei|
|is_published|kahendmuutuja|Ei|
|link|string|Ei|
|sõnumi|string|Ei|
|Nimi|string|Ei|
|object_id|string|Ei|
|pilt|string|Ei|
|koht|määratletud ei|Ei|
|Privaatsus|määratletud ei|Ei|
|atribuudid|massiiv|Ei|
|Allikas|string|Ei|
|status_type|string|Ei|
|lugu|string|Ei|
|suunamise|määratletud ei|Ei|
|Kui soovite|massiiv|Ei|
|tüüp|string|Ei|
|updated_time|string|Ei|
|with_tags|määratletud ei|Ei|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Kasutaja profiili ühe kanali
Profiili võib olla kasutaja lehe rakenduse või rühma.

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|created_time|string|Ei|
|kaudu|määratletud ei|Ei|
|sõnumi|string|Ei|
|tüüp|string|Ei|

#### <a name="adminitem"></a>AdminItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|link|string|Ei|

#### <a name="propertyitem"></a>PropertyItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|Nimi|string|Ei|
|teksti|string|Ei|
|href|string|Ei|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|sõnumi|string|Jah|
|link|string|Ei|
|pilt|string|Ei|
|Nimi|string|Ei|
|Pealdis|string|Ei|
|kirjeldus|string|Ei|
|koht|string|Ei|
|Sildid|string|Ei|
|Privaatsus|määratletud ei|Ei|
|object_attachment|string|Ei|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|sõnumi|string|Jah|
|link|string|Ei|
|pilt|string|Ei|
|Nimi|string|Ei|
|Pealdis|string|Ei|
|kirjeldus|string|Ei|
|toimingud|massiiv|Ei|
|koht|string|Ei|
|Sildid|string|Ei|
|object_attachment|string|Ei|
|suunamise|määratletud ei|Ei|
|feed_targeting|määratletud ei|Ei|
|avaldatud|kahendmuutuja|Ei|
|scheduled_publish_time|string|Ei|
|backdated_time|string|Ei|
|backdated_time_granularity|string|Ei|
|child_attachments|massiiv|Ei|
|multi_share_end_card|kahendmuutuja|Ei|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|

#### <a name="profilecollection"></a>ProfileCollection

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|andmete|massiiv|Ei|

#### <a name="useritem"></a>UserItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|first_name|string|Ei|
|last_name|string|Ei|
|Nimi|string|Ei|
|sugu|string|Ei|
|kohta|string|Ei|

#### <a name="actionitem"></a>ActionItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|Nimi|string|Ei|
|link|string|Ei|

#### <a name="targetitem"></a>TargetItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|Kõik riigid|massiiv|Ei|
|asukohtades|massiiv|Ei|
|piirkondade|massiiv|Ei|
|linna|massiiv|Ei|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Objekti, mis määrab uudiste kanali suunamise seda postitust
Kõik need rühmad tõenäoliselt selle postituse nägemiseks, teised on väiksem võimalus. Kehtib ainult lehed.

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|Kõik riigid|massiiv|Ei|
|piirkondade|massiiv|Ei|
|linna|massiiv|Ei|
|age_min|täisarv|Ei|
|age_max|täisarv|Ei|
|sood|massiiv|Ei|
|relationship_statuses|massiiv|Ei|
|interested_in|massiiv|Ei|
|college_years|massiiv|Ei|
|huvide|massiiv|Ei|
|relevant_until|täisarv|Ei|
|education_statuses|massiiv|Ei|
|asukohtades|massiiv|Ei|

#### <a name="placeitem"></a>PlaceItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|Nimi|string|Ei|
|overall_rating|arv|Ei|
|asukoht|määratletud ei|Ei|

#### <a name="locationitem"></a>LocationItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|linna|string|Ei|
|riik|string|Ei|
|laiuskraad|arv|Ei|
|located_in|string|Ei|
|laiuskraadide|arv|Ei|
|Nimi|string|Ei|
|piirkond|string|Ei|
|olek|string|Ei|
|tänav|string|Ei|
|ZIP|string|Ei|

#### <a name="privacyitem"></a>PrivacyItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|kirjeldus|string|Ei|
|väärtus|string|Jah|
|luba|string|Ei|
|keelamiseks|string|Ei|
|sõbrad|string|Ei|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|link|string|Ei|
|pilt|string|Ei|
|image_hash|string|Ei|
|Nimi|string|Ei|
|kirjeldus|string|Ei|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|URL-i|string|Jah|
|Pealdis|string|Ei|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Jah|
|post_id|string|Jah|

#### <a name="postvideorequest"></a>PostVideoRequest

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|videoData|string|Jah|
|kirjeldus|string|Jah|
|pealkiri|string|Jah|
|uploadedVideoName|string|Ei|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|andmete|määratletud ei|Jah|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|URL-i|string|Jah|
|is_silhouette|kahendmuutuja|Jah|
|kõrgus|string|Ei|
|laius|string|Ei|

#### <a name="geteventresponse"></a>GetEventResponse

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|andmete|massiiv|Jah|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Atribuudi nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Jah|
|Nimi|string|Jah|
|start_time|string|Ei|
|end_time|string|Ei|
|ajavöönd|string|Ei|
|asukoht|string|Ei|
|kirjeldus|string|Ei|
|ticket_uri|string|Ei|
|rsvp_status|string|Jah|


## <a name="next-steps"></a>Järgmised sammud

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).
