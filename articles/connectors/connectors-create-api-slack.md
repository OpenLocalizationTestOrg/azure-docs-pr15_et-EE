<properties
pageTitle=" Kasutage loogika rakenduste vaikne konnektor | Microsoft Azure'i"
description="Rakendustes Microsoft Azure'i rakenduse teenuse loogika vaikne konnektor kasutamise alustamine"
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

# <a name="get-started-with-the-slack-connector"></a>Vaikne konnektor kasutamise alustamine

Vaikne on meeskonnatöö kommunikatsiooni vahend, mis ühendab kõik teie meeskond kirjavahetuse ühes paigutada, Kiire otsitavate ja saadaval, kuhu minna.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon.

Vaikne konnektor, saate teha järgmist.

* Kasutage seda loogika rakenduste koostamine

Saate lisada toimingu loogika rakendustes, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Vaatame rääkida päästikute ja toimingud

Vaikne konnektor saab kasutada toimingu; ei ole päästikute. Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 Vaikne konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="slack-actions"></a>Vaikne toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|PostMessage'i|Sõnumi määratud kanalisse postitamine.|
## <a name="create-a-connection-to-slack"></a>Vaikne ühenduse loomine
Vaikne konnektor kasutamiseks saate esmalt **ühenduse** loomine, siis neid atribuute üksikasjade pakkumine: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|Vaikne mandaat|

Logige sisse vaikne ja vaikne **ühenduse** loogika rakenduse konfigureerimise lõpuleviimiseks tehke järgmist

1. Valige **Korduvus**
2. Valige **sagedus** ja sisestage soovitud **intervall**
3. Valige **Lisa toiming**  
![Vaikne konfigureerimine][1]  
4. Sisestage otsinguväljale vaikne ja oodake otsingu tagastamiseks kõiki kirjeid koos vaikne nimi
5. **Vaikne - Post sõnumi** valimine
6. Valige **logige sisse toorparafiinil**.  
![Vaikne konfigureerimine][2]
7. Vaikne mandaadi sisse logida lubada esitamiseks    
![Vaikne konfigureerimine][3]  
8. Teid suunatakse ümber ettevõtte Logi lehel. **Lubada** Vaikne loogika rakenduse suhelda:      
![Vaikne konfigureerimine][5] 
9. Pärast luba lõpulejõudmist suunatakse teid sinna konfigureerimisega jaotise **toorparafiinil - saada kõik sõnumid** loogika rakendusse. Lisage muud päästikute ja toimingud, mida teil on vaja.  
![Vaikne konfigureerimine][6]
10. Salvestage oma töö, valides **salvestamine** ülaltoodud menüüriba.


>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="slack-rest-api-reference"></a>Vaikne REST API viide
#### <a name="this-documentation-is-for-version-10"></a>Need dokumendid on versioon: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Sõnumi määratud kanalisse postitamine.
**```POST: /chat.postMessage```** 



| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|kanali|string|Jah|päringu|ükski|Kanali, privaatse rühmaga või Kiirsõnumside kanali sõnumi saatmiseks. Võib olla mõne name(ex: #general) või encoded ID-ga.|
|teksti|string|Jah|päringu|ükski|Sõnumi saatmiseks teksti. Vormindamise suvandeid, vt https://api.slack.com/docs/formatting.|
|kasutajanimi|string|Ei|päringu|ükski|Robot nimi|
|as_user|kahendmuutuja|Ei|päringu|ükski|Liigu true asemel autenditud kasutajana sõnumi postitada, kui robot|
|sõeluda|string|Ei|päringu|ükski|Saate muuta, kuidas sõnumeid ei käsitleta. Lisateavet leiate teemast https://api.slack.com/docs/formatting.|
|link_names|täisarv|Ei|päringu|ükski|Otsing ja seos kanali nimed ja kasutajanimed.|
|unfurl_links|kahendmuutuja|Ei|päringu|ükski|Andke õige luba unfurling peamiselt tekstipõhine sisu.|
|unfurl_media|kahendmuutuja|Ei|päringu|ükski|Andke unfurling meedia sisu keelamiseks false.|
|icon_url|string|Ei|päringu|ükski|URL-i pildi kasutamiseks ikoonina teade|
|icon_emoji|string|Ei|päringu|ükski|Emotikonide teade ikoonina kasutamiseks|


### <a name="here-are-the-possible-responses"></a>Siin on võimalik vastuseid.

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|408|Päringu ajalõpp|
|429|Liiga palju taotlusi|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|503|Vaikne teenus pole saadaval|
|504|Lüüsi ajalõpp|
|Vaikimisi|Toiming nurjus.|
------



## <a name="object-definitions"></a>Objekti määratlust: 

 **Teade**: Yammeri sõnum

Nõutavate atribuutide teade:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|ID|täisarv|
|content_excerpt|string|
|sender_id|täisarv|
|replied_to_id|täisarv|
|created_at|string|
|network_id|täisarv|
|message_type|string|
|sender_type|string|
|URL-i|string|
|web_url|string|
|group_id|täisarv|
|sisu|määratletud ei|
|thread_id|täisarv|
|direct_message|kahendmuutuja|
|client_type|string|
|client_url|string|
|keel|string|
|notified_user_ids|massiiv|
|Privaatsus|string|
|liked_by|määratletud ei|
|system_message|kahendmuutuja|



 **PostOperationRequest**: tähistab postituse taotluse Yammer Connectori postitamiseks yammer

Nõutavate atribuutide PostOperationRequest:

sisu

**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|sisu|string|
|group_id|täisarv|
|replied_to_id|täisarv|
|direct_to_id|täisarv|
|leviedastus|kahendmuutuja|
|Teema1|string|
|teema2|string|
|topic3|string|
|topic4|string|
|topic5|string|
|topic6|string|
|topic7|string|
|topic8|string|
|topic9|string|
|topic10|string|
|topic11|string|
|topic12|string|
|topic13|string|
|topic14|string|
|topic15|string|
|topic16|string|
|topic17|string|
|topic18|string|
|topic19|string|
|topic20|string|



 **MessageList**: sõnumite loend

Nõutavate atribuutide MessageList:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|sõnumite|massiiv|



 **MessageBody**: sõnumi keha

Nõutavate atribuutide MessageBody:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|sõeluda|string|
|tavaline|string|
|rikkaliku|string|



 **LikedBy**: meeldis

Nõutavate atribuutide LikedBy:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Count|täisarv|
|nimed|massiiv|



 **YammmerEntity**: meeldis

Nõutavate atribuutide YammmerEntity:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|tüüp|string|
|ID|täisarv|
|full_name|string|


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Objekti määratlust: 

 **WebResultModel**: Bingi veebiotsingu tulemid

Nõutavate atribuutide WebResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Pealkiri|string|
|Kirjeldus|string|
|DisplayUrl|string|
|ID|string|
|FullUrl|string|



 **VideoResultModel**: Bingi video otsingutulemite

Nõutavate atribuutide VideoResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Pealkiri|string|
|DisplayUrl|string|
|ID|string|
|MediaUrl|string|
|Käitusaja|täisarv|
|Pisipilt|määratletud ei|



 **ThumbnailModel**: MMS elemendi atribuute pisipilt

Nõutavate atribuutide ThumbnailModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|MediaUrl|string|
|ContentType|string|
|Laius|täisarv|
|Kõrgus|täisarv|
|FileSize|täisarv|



 **ImageResultModel**: Bingi pildiotsingu tulemustest

Nõutavate atribuutide ImageResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Pealkiri|string|
|DisplayUrl|string|
|ID|string|
|MediaUrl|string|
|SourceUrl|string|
|Pisipilt|määratletud ei|



 **NewsResultModel**: Bingi Uudised otsingutulemite

Nõutavate atribuutide NewsResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Pealkiri|string|
|Kirjeldus|string|
|DisplayUrl|string|
|ID|string|
|Allikas|string|
|Kuupäev|string|



 **SpellResultModel**: Bingi õigekirja soovitused tulemused

Nõutavate atribuutide SpellResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|ID|string|
|Väärtus|string|



 **RelatedSearchResultModel**: Bingi seotud Otsingu tulemused

Nõutavate atribuutide RelatedSearchResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Pealkiri|string|
|ID|string|
|BingUrl|string|



 **CompositeSearchResultModel**: Bingi kombineeritud Otsingu tulemused

Nõutavate atribuutide CompositeSearchResultModel:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|WebResultsTotal|täisarv|
|ImageResultsTotal|täisarv|
|VideoResultsTotal|täisarv|
|NewsResultsTotal|täisarv|
|SpellSuggestionsTotal|täisarv|
|WebResults|massiiv|
|ImageResults|massiiv|
|VideoResults|massiiv|
|NewsResults|massiiv|
|SpellSuggestionResults|massiiv|
|RelatedSearchResults|massiiv|


## <a name="object-definitions"></a>Objekti määratlust: 

 **PostOperationResponse**: tähistab vastuse postituse toimimise vaikne konnektor vaikne sisestamiseks

Nõutavate atribuutide PostOperationResponse:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|Ok|kahendmuutuja|
|kanali|string|
|TS|string|
|sõnumi|määratletud ei|
|tõrge|string|



 **MessageItem**: kanali sõnumi.

Nõutavate atribuutide MessageItem:


Atribuutide pole nõutav. 


**Kõik atribuudid**: 


| Nimi | Andmetüüp |
|---|---|
|teksti|string|
|ID|string|
|kasutaja|string|
|loodud|täisarv|
|is_user kustutatud|kahendmuutuja|


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
