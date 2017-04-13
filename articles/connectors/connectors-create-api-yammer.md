<properties
pageTitle="Yammer Connectori rakenduste loogika lisamine | Microsoft Azure'i"
description="REST API parameetritega Yammer Connectori ülevaade"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-yammer-connector"></a>Yammer Connectori kasutamise alustamine

Ühenduse Yammeri Accessi vestlustele oma ettevõtte võrku.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon.

Yammeris saate teha järgmist.

- Koostage oma andmete saate Yammeri voog. 
- Kasutage käivitab jaoks, kui seal on uus sõnum rühmale või kanalisse oma järgmist.
- Sõnumi postitamine, saavad kõik sõnumid ja muud toimingute abil. Neid toiminguid vastuse saamine, ja seejärel tehke väljund muude toimingute jaoks saadaval. Uue sõnumi kuvamisel saate saata meilisõnumi teenusekomplekti Office 365.

Saate lisada toimingu loogika rakendustes, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud
Yammeri sisaldab järgmisi päästikute ja toimingud. 

Päästik | Toimingud
--- | ---
<ul><li>Kui rühm on uues sõnumis</li><li>Kui seal on uus sõnum minu järgmist kanali</li></ul>| <ul><li>Kõigi sõnumite hankimine</li><li>Rühmale sõnumite saab</li><li>Minu järgmine sõnumid kanali saab</li><li>Postituse sõnum</li><li>Kui rühm on uues sõnumis</li><li>Kui seal on uus sõnum minu järgmist kanali</li></ul>

Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

## <a name="create-a-connection-to-yammer"></a>Yammeri ühenduse loomine
Yammer Connectori kasutamiseks saate esmalt **ühenduse** loomine, siis neid atribuute üksikasjade pakkumine: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|Yammeri mandaat|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="yammer-rest-api-reference"></a>Yammeri REST API viide
Need dokumendid on versioon: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Saada kõik avaliku sõnumid sisselogitud kasutaja Yammeri võrk
Vastab "Kõik" Vestlused Yammeri liideses.  
```GET: /messages.json```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|older_then|täisarv|Ei|päringu|ükski|Annab vastuseks sõnumid, mis on vanemad kui arvuline stringina määratud sõnumi ID-d. See on abiks paginating sõnumeid. Näiteks kui vaatate praegu 20 sõnumeid ja vanimast on number 2912, saate lisada "? older_than = 2912″, et saada 20 sõnumite enne neid näete Teie taotlus.|
|newer_then|täisarv|Ei|päringu|ükski|Tagastab sõnumite uuem kui arvuline stringina määratud sõnumi ID-d. See võib kasutada kui küsitlused uute sõnumite jaoks. Kui vaatate sõnumeid ja viimase sõnumi tagastada 3516, saate teha taotluse parameetriga "? newer_than = 3516″ tagamaks, et te ei saa sõnumite koopiate juba lehel.|
|limiit|täisarv|Ei|päringu|ükski|Tagastab ainult määratud teadete arv.|
|lehe|täisarv|Ei|päringu|ükski|Saada lehe määratud. Kui tagastatud andmed on suurem kui limiit, saate selle välja järgmiste lehtede juurdepääsuks|


### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|408|Päringu ajalõpp|
|429|Liiga palju taotlusi|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|503|Yammeri teenus pole saadaval|
|504|Lüüsi ajalõpp|
|Vaikimisi|Toiming nurjus.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Rühmale sõnumi postitamine või kõik ettevõtte kanal
Kui rühma ID, sõnumi postitatud määratud rühmale veel seda postitatud kõik ettevõtte kanali.    
```POST: /messages.json``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|sisestusmeetodi| |Jah|sisu|ükski|Postituse sõnumi taotlus|


### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|201|Loodud|



## <a name="object-definitions"></a>Objekti määratlused

#### <a name="message-yammer-message"></a>Sõnum: Võimaldab saata Yammeri

| Nimi | Andmetüüp | Nõutav |
|---|---| --- | 
|ID|täisarv|Ei|
|content_excerpt|string|Ei|
|sender_id|täisarv|Ei|
|replied_to_id|täisarv|Ei|
|created_at|string|Ei|
|network_id|täisarv|Ei|
|message_type|string|Ei|
|sender_type|string|Ei|
|URL-i|string|Ei|
|web_url|string|Ei|
|group_id|täisarv|Ei|
|sisu|määratletud ei|Ei|
|thread_id|täisarv|Ei|
|direct_message|kahendmuutuja|Ei|
|client_type|string|Ei|
|client_url|string|Ei|
|keel|string|Ei|
|notified_user_ids|massiiv|Ei|
|Privaatsus|string|Ei|
|liked_by|määratletud ei|Ei|
|system_message|kahendmuutuja|Ei|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Tähistab postituse taotluse Yammer Connectori postitamiseks yammer

| Nimi | Andmetüüp | Nõutav |
|---|---| --- | 
|sisu|string|Jah|
|group_id|täisarv|Ei|
|replied_to_id|täisarv|Ei|
|direct_to_id|täisarv|Ei|
|leviedastus|kahendmuutuja|Ei|
|Teema1|string|Ei|
|teema2|string|Ei|
|topic3|string|Ei|
|topic4|string|Ei|
|topic5|string|Ei|
|topic6|string|Ei|
|topic7|string|Ei|
|topic8|string|Ei|
|topic9|string|Ei|
|topic10|string|Ei|
|topic11|string|Ei|
|topic12|string|Ei|
|topic13|string|Ei|
|topic14|string|Ei|
|topic15|string|Ei|
|topic16|string|Ei|
|topic17|string|Ei|
|topic18|string|Ei|
|topic19|string|Ei|
|topic20|string|Ei|

#### <a name="messagelist-list-of-messages"></a>MessageList: Meilisõnumite loendi

| Nimi | Andmetüüp | Nõutav |
|---|---| --- | 
|sõnumite|massiiv|Ei|


#### <a name="messagebody-message-body"></a>MessageBody: Sõnumi kehasse

| Nimi | Andmetüüp | Nõutav |
|---|---| --- | 
|sõeluda|string|Ei|
|tavaline|string|Ei|
|rikkaliku|string|Ei|

#### <a name="likedby-liked-by"></a>LikedBy: Meeldis

| Nimi | Andmetüüp | Nõutav |
|---|---| --- | 
|Count|täisarv|Ei|
|nimed|massiiv|Ei|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Meeldis

| Nimi | Andmetüüp | Nõutav |
|---|---| --- | 
|tüüp|string|Ei|
|ID|täisarv|Ei|
|full_name|string|Ei|


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
