<properties
pageTitle="SendGrid | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. SendGrid ühenduse pakkuja saate meilisõnumeid saata ja adressaatide loendite haldus."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Alustamine SendGrid konnektor

SendGrid ühenduse pakkuja saate meilisõnumeid saata ja adressaatide loendite haldus.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse nüüd leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

SendGrid konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 SendGrid konnektor on saadaval järgmised toimingud. Ei ole päästikute.

### <a name="sendgrid-actions"></a>SendGrid toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[SendEmail](connectors-create-api-sendgrid.md#sendemail)|Saadab meilisõnumi SendGrid API (kuni 10 000 adressaadid)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Adressaatide loendi üksiku adressaadi lisamine|


## <a name="create-a-connection-to-sendgrid"></a>SendGrid ühenduse loomine
Loogika rakenduste SendGrid loomiseks peate esmalt **ühenduse** loomine siis üksikasjad pakuvad järgmised atribuudid: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|ApiKey|Jah|Sisestage oma SendGrid Api võti|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

Pärast ühenduse loomist saate käivitada toiminguid ja kuulake päästikute, selles artiklis kirjeldatud.

## <a name="reference-for-sendgrid"></a>SendGrid tutvustus
Kehtib versioon: 1.0

## <a name="sendemail"></a>SendEmail
Meilisõnumi saatmine: saadetakse meilisõnum, SendGrid API (kuni 10 000 adressaadid) abil 

```POST: /api/mail.send.json``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|taotlus| |Jah|sisu|ükski|Meilisõnumi saatmine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|429|Liiga palju taotlus|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Adressaadi lisamiseks loendisse: adressaadiloendi üksiku adressaadi lisamine 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|listId|string|Jah|tee|ükski|Adressaatide loendi ainuidentifikaator|
|recipientId|string|Jah|tee|ükski|Adressaadi ainuidentifikaator|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused 

### <a name="emailrequest"></a>EmailRequest


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|kaudu|string|Jah |
|fromname|string|Ei |
|Kui soovite|string|Jah |
|toname|string|Ei |
|Teema|string|Jah |
|sisu|string|Jah |
|ishtml|kahendmuutuja|Ei |
|koopia|string|Ei |
|ccname|string|Ei |
|Salakoopia|string|Ei |
|bccname|string|Ei |
|OutboundSMTPServeri parameetrid|string|Ei |
|kuupäev|string|Ei |
|päised|string|Ei |
|failide|massiiv|Ei |
|failinimed|massiiv|Ei |



### <a name="emailresponse"></a>EmailResponse


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|sõnumi|string|Ei |



### <a name="recipientlists"></a>RecipientLists


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|loendid|massiiv|Ei |



### <a name="recipientlist"></a>RecipientList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|täisarv|Ei |
|Nimi|string|Ei |
|recipient_count|täisarv|Ei |



### <a name="recipients"></a>Adressaadid


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|adressaadid|massiiv|Ei |



### <a name="recipient"></a>Adressaadi


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|e-posti|string|Ei |
|last_name|string|Ei |
|first_name|string|Ei |
|ID|string|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)