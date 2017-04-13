<properties
pageTitle="Kasutage Office 365 videoportaali konnektor loogika rakenduste | Microsoft Azure'i"
description="Office 365 videoportaali konnektor oma rakendustes Microsoft Azure'i rakenduse teenuse loogika kasutamise alustamine"
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

# <a name="get-started-with-the-office365-video-connector"></a>Office 365 Video konnektor kasutamise alustamine
Ühenduse Office 365 videoportaali saada teavet teenusekomplekti Office 365 video, videoid ja muud loendit. Office 365 videoportaali konnektor saab kasutada:

- Loogika rakendused 

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. Skeemi varasemad versioonid ei toeta seda konnektor.

Office 365 videoportaali, saate teha järgmist.

- Koostage oma voog saate Office 365 videoportaali andmete põhjal. 
- Kasutage toiminguid, mida video portaali oleku kontrollimine, loendi kõik video kanali ja palju muud. Neid toiminguid vastuse saamine, ja seejärel tehke väljund muude toimingute jaoks saadaval. Näiteks saate konnektor Bingi otsingu abil saate otsida Office 365 videod ja selle video kohta teabe saamiseks Office 365 video konnektor abil. Kui video vastab teie vajadustele, saate seda videot Facebookis postitada. 

Toimingu lisamiseks loogika rakendused, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

Office 365 videoportaali konnektor on saadaval järgmised toimingud. Ei ole päästikute.

| Päästikute | Toimingud|
| --- | --- |
| Ükski | <ul><li>Kontrollib video portaali olek</li><li>Saada kõik võtta</li><li>Video taasesitus URL-i Azure Media Servicesi manifesti hankimine</li><li>Saada esitaja luba saada juurdepääsu video dekrüptimine</li><li>Saab teavet kindla office365 video</li><li>Loendite kanali office365 videote esitamine</li></ul>

Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

## <a name="create-a-connection-to-office365-video-connector"></a>Office 365 Video konnektor ühenduse loomine
Selle konnektori lisamisel loogika rakenduste peate oma Office 365 videoportaali kontosse sisselogimine ja luba loogika rakendused oma kontoga ühenduse loomiseks.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Kui olete loonud ühenduse, sisestage Office 365 video atribuudid, nt rentniku nime või kanali ID-ga. **REST API viide** selles teemas kirjeldatakse neid atribuute.

>[AZURE.TIP] Saate kasutada sama Office 365 videoportaali sellega loogika muudes rakendustes.

## <a name="swagger-rest-api-reference"></a>Ärplema REST API viide
Kehtib versioon: 1.0.

### <a name="checks-video-portal-status"></a>Kontrollib video portaali olek 
Kontrollib video portaali olekut ning vaadake, kas video teenused on lubatud.  
```GET: /{tenant}/IsEnabled``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|rentniku|string|Jah|tee|ükski|Rentniku kataloogi kasutaja nimi on osa|


#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|404|Ei leitud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|



### <a name="get-all-viewable-channels"></a>Saada kõik võtta 
Saab kõik kanalid kasutajal on juurdepääs vaatamine.  
```GET: /{tenant}/Channels``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|rentniku|string|Jah|tee|ükski|Rentniku kataloogi kasutaja nimi on osa|


#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|404|Ei leitud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Loendite kanali office365 videote esitamine 
Loendite office365 videote esitamine kanali.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|rentniku|string|Jah|tee|ükski|Rentniku kataloogi kasutaja nimi on osa|
|channelId|string|Jah|tee|ükski|Kanali ID-d, kust tuleb videod tooma|


#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|404|Ei leitud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|




### <a name="gets-information-about-a-particular-office365-video"></a>Saab teavet kindla office365 video 
Saab teavet kindla office365 video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|rentniku|string|Jah|tee|ükski|Rentniku kataloogi kasutaja nimi on osa|
|channelId|string|Jah|tee|ükski|Kanali id|
|videoId|string|Jah|tee|ükski|Video id|


#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|404|Ei leitud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Video taasesitus URL-i Azure Media Servicesi manifesti hankimine 
Saate video taasesitus URL-i Azure Media Servicesi manifesti.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|rentniku|string|Jah|tee|ükski|Rentniku kataloogi kasutaja nimi on osa|
|channelId|string|Jah|tee|ükski|Kanali id|
|videoId|string|Jah|tee|ükski|Video id|
|streamingFormatType|string|Jah|päringu|ükski|Streaming vormingu tüüp. 1 - sujuva voogesituse või MPEG-MÕTTEKRIIPSU. 0 - HLS Streaming|


#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|404|Ei leitud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Saada esitaja luba saada juurdepääsu video dekrüptimine 
Saada esitaja luba saada juurdepääsu dekrüptida video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|rentniku|string|Jah|tee|ükski|Rentniku kataloogi kasutaja nimi on osa|
|channelId|string|Jah|tee|ükski|Kanali id|
|videoId|string|Jah|tee|ükski|Video id|


#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|404|Ei leitud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused

#### <a name="channel-channel-class"></a>Kanali: Kanali klassi

| Nimi | Andmetüüp | Nõutav|
|---|---|---|
|ID|string|Ei|
|Pealkiri|string|Ei|
|Kirjeldus|string|Ei|


#### <a name="video"></a>Video 

| Nimi | Andmetüüp |Nõutav|
|---|---|---|
|ID|string|Ei|
|Pealkiri|string|Ei|
|Kirjeldus|string|Ei|
|CreationDate|string|Ei|
|Omanik|string|Ei|
|ThumbnailUrl|string|Ei|
|VideoUrl|string|Ei|
|VideoDuration|täisarv|Ei|
|VideoProcessingStatus|täisarv|Ei|
|ViewCount|täisarv|Ei|


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).
