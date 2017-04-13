<properties
pageTitle="Outlook.com-i | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Outlook.com-i connector võimaldab teil hallata oma e-posti, kalendrite ja kontaktide. Sooritada erinevaid toiminguid, näiteks saada e-posti, koosolekute plaanimine, lisada kontakte jne."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Outlook.com-i konnektor kasutamise alustamine

Outlook.com-i connector võimaldab teil hallata oma e-posti, kalendrite ja kontaktide. Sooritada erinevaid toiminguid, näiteks saada e-posti, koosolekute plaanimine, lisada kontakte jne.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse kohe leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

Outlook.com-i konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 Outlook.com-i konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="outlookcom-actions"></a>Outlook.com-i toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|E-kirju toob kaustast|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|Saadetakse meilisõnum|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Kustutab e-posti ID|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|E-posti, kui teil on veel loetud märgib|
|[OutboundSMTPServeri parameetrid](connectors-create-api-outlook.md#ReplyTo)|Meilisõnumile vastamine|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Toob e-posti manuse ID|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Mitu võimalust e-postiga saata ja oodake, et adressaat saaks reageerida tagasi üks suvanditest.|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Saatke meilisõnum kinnitamise ja oodake, kuni adressaat vastust|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Kalendrite toob|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Kalendrist üksuste toob|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Loob uue sündmuse|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Kindla üksuse toob kalendrist|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Kustutab kalendriüksuse|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Kalendriüksuse osaliselt värskendused|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Kontaktikaustade toob|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Toob kontaktide kausta Kontaktid|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Loob uue kontakti|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Kindla kontakti kontaktikausta, toob|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Kontakti kustutamine|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Osaliselt värskendab kontakti|
### <a name="outlookcom-triggers"></a>Outlook.com-i päästikute
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Sündmuse kiiresti alustamise kohta|Käivitab voog on eelseisvad kalendrisündmuse käivitamisel|
|Uue e-posti|Käivitab uue meilisõnumi saabumisel vool|
|Klõpsake uued üksused|Kui luuakse uus kalendriüksus|
|Värskendatud üksusi|Kui kalendriüksuse on muudetud|


## <a name="create-a-connection-to-outlookcom"></a>Outlook.com-i ühenduse loomine
Outlook.com-iga loogika rakenduste loomiseks peate esmalt luua **ühenduse** siis üksikasjad pakuvad järgmised atribuudid: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|Outlook.com-i mandaati|
Pärast ühenduse loomist saate käivitada toiminguid ja kuulake päästikute, selles artiklis kirjeldatud.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.  

## <a name="reference-for-outlookcom"></a>Outlook.com-i viide
Kehtib versioon: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Sündmuse kiiresti alustamise kohta: käivitab voog on eelseisvad kalendrisündmuse käivitamisel 

```GET: /Events/OnUpcomingEvents``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|päringu|ükski|Kalendri ainuidentifikaator|
|lookAheadTimeInMinutes|täisarv|Ei|päringu|15|Aeg (minutites) edasi otsida eelseisvaid sündmusi|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|202|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="getemails"></a>GetEmails
Saada e-kirju: e-kirju toob kaustast 

```GET: /Mail``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|folderPath|string|Ei|päringu|Sisendkausta|Meilisõnumite toomiseks kausta tee (vaikimisi: "Sisendkausta")|
|algusse|täisarv|Ei|päringu|10|Arv e-kirju toomiseks (vaikimisi: 10)|
|fetchOnlyUnread|kahendmuutuja|Ei|päringu|True|Saate tuua ainult Lugemata meilisõnumid?|
|includeAttachments|kahendmuutuja|Ei|päringu|FALSE|Kui on seatud tõene, manuste tuuakse ka koos e-posti|
|searchQuery|string|Ei|päringu|ükski|Otsingupäringu meilisõnumite filtreerimine|
|Jäta|täisarv|Ei|päringu|0|Arv e-kirju vahele (vaikimisi: 0)|
|skipToken|string|Ei|päringu|ükski|Luba juurde toomise uus leht|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="sendemail"></a>SendEmail
Meilisõnumi saatmine: saadetakse meilisõnum 

```POST: /Mail``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|emailMessage| |Jah|sisu|ükski|E-posti|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="deleteemail"></a>DeleteEmail
Meilisõnumi kustutamine: kustutab e-posti ID 

```DELETE: /Mail/{messageId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|messageId|string|Jah|tee|ükski|Meilisõnumi kustutamiseks ID|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="markasread"></a>MarkAsRead
Loetuks märkimine: e-posti, kui teil on veel loetud märgib 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|messageId|string|Jah|tee|ükski|E-posti, et märkida, samuti ID lugemine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="replyto"></a>OutboundSMTPServeri parameetrid
E-posti vastus: e-posti vastused 

```POST: /Mail/ReplyTo/{messageId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|messageId|string|Jah|tee|ükski|E-posti vastamine ID-d|
|kommentaari|string|Jah|päringu|ükski|Kommentaari vastus|
|replyAll|kahendmuutuja|Ei|päringu|FALSE|Kõigile adressaatidele vastamine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="getattachment"></a>GetAttachment
Manuse hankimine: toob e-posti manuse ID 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|messageId|string|Jah|tee|ükski|E-posti ID|
|attachmentId|string|Jah|tee|ükski|Manuse allalaadimiseks ID-d|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="onnewemail"></a>OnNewEmail
Uue e-posti: vool käivitab uue meilisõnumi saabumisel 

```GET: /Mail/OnNewEmail``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|folderPath|string|Ei|päringu|Sisendkausta|E-posti kausta toomiseks (vaikimisi: sisendkausta)|
|Kui soovite|string|Ei|päringu|ükski|Adressaatide meiliaadressid|
|kaudu|string|Ei|päringu|ükski|Aadressilt|
|tähtsus|string|Ei|päringu|Tavaline|E-posti (kõrge, madal, tavaline) prioriteedi (vaikimisi: tavaline)|
|fetchOnlyWithAttachment|kahendmuutuja|Ei|päringu|FALSE|Saate tuua ainult meilisõnumite manusega|
|includeAttachments|kahendmuutuja|Ei|päringu|FALSE|Manustega|
|subjectFilter|string|Ei|päringu|ükski|Teema otsida stringi|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Toiming õnnestus|
|202|Aktsepteeritud|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Saada e-posti võimalusi: mitu võimalust e-postiga saata ja oodake, et adressaat saaks reageerida tagasi üks suvanditest. 

```POST: /mailwithoptions/$subscriptions``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Jah|sisu|ükski|Tellimuse taotluse suvandid e-posti jaoks|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|201|Tellimuse, mis on loodud|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Kinnitamise meilisõnumi saatmine: kinnitamise meilisõnumit saata ja oodake, kuni adressaat vastust 

```POST: /approvalmail/$subscriptions``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Jah|sisu|ükski|Tellimuse taotluse kinnitamine e-posti jaoks|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|201|Tellimuse, mis on loodud|
|400|BadRequest|
|401|Volitused|
|403|Keelatud|
|500|Sisemine serveritõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="calendargettables"></a>CalendarGetTables
Kalendrite hankimine: toob kalendrid 

```GET: /datasets/calendars/tables``` 

Pole selle kõne parameetrid
#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendargetitems"></a>CalendarGetItems
Saada sündmuste: toob üksuste lisamine kalendrisse 

```GET: /datasets/calendars/tables/{table}/items``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri toomiseks ainuidentifikaator|
|$filter|string|Ei|päringu|ükski|ODATA filter päringu kirjete arvu piiramine|
|$orderby|string|Ei|päringu|ükski|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|täisarv|Ei|päringu|ükski|Vahele kirjete arv (vaikimisi = 0)|
|$top|täisarv|Ei|päringu|ükski|Suurim tuua kirjete arv (vaikimisi = 256)|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendarpostitem"></a>CalendarPostItem
Sündmuse loomine: loob uue sündmuse 

```POST: /datasets/calendars/tables/{table}/items``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri ainuidentifikaator|
|üksuse| |Jah|sisu|ükski|Kalendriüksuse loomine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendargetitem"></a>CalendarGetItem
Saada sündmus: toob mõnda kindlat üksust kalender 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri ainuidentifikaator|
|ID|string|Jah|tee|ükski|Kalendriüksuse toomiseks ainuidentifikaator|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Sündmuse kustutamine: kustutab kalendriüksuse 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri ainuidentifikaator|
|ID|string|Jah|tee|ükski|Kalendriüksuse kustutamiseks ainuidentifikaator|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Värskenduse sündmuse: osaliselt värskendab kalendriüksuse 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri ainuidentifikaator|
|ID|string|Jah|tee|ükski|Kalendriüksuse värskendamiseks ainuidentifikaator|
|üksuse| |Jah|sisu|ükski|Kalendriüksuse värskendamine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Klõpsake uute üksuste: käivitada siis, kui luuakse uus kalendriüksus 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri ainuidentifikaator|
|$filter|string|Ei|päringu|ükski|ODATA filter päringu kirjete arvu piiramine|
|$orderby|string|Ei|päringu|ükski|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|täisarv|Ei|päringu|ükski|Vahele kirjete arv (vaikimisi = 0)|
|$top|täisarv|Ei|päringu|ükski|Suurim tuua kirjete arv (vaikimisi = 256)|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Klõpsake värskendatud üksuste: kui kalendriüksuse on muudetud 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kalendri ainuidentifikaator|
|$filter|string|Ei|päringu|ükski|ODATA filter päringu kirjete arvu piiramine|
|$orderby|string|Ei|päringu|ükski|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|täisarv|Ei|päringu|ükski|Vahele kirjete arv (vaikimisi = 0)|
|$top|täisarv|Ei|päringu|ükski|Suurim tuua kirjete arv (vaikimisi = 256)|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="contactgettables"></a>ContactGetTables
Saada kontaktikaustu: toob kontaktide kaustad 

```GET: /datasets/contacts/tables``` 

Pole selle kõne parameetrid
#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="contactgetitems"></a>ContactGetItems
Kontaktide: toob kontaktide kausta Kontaktid 

```GET: /datasets/contacts/tables/{table}/items``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kausta Kontaktid tuua ainuidentifikaator|
|$filter|string|Ei|päringu|ükski|ODATA filter päringu kirjete arvu piiramine|
|$orderby|string|Ei|päringu|ükski|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|täisarv|Ei|päringu|ükski|Vahele kirjete arv (vaikimisi = 0)|
|$top|täisarv|Ei|päringu|ükski|Suurim tuua kirjete arv (vaikimisi = 256)|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="contactpostitem"></a>ContactPostItem
Kontakti loomine: loob uue kontakti 

```POST: /datasets/contacts/tables/{table}/items``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kontaktikausta ainuidentifikaator|
|üksuse| |Jah|sisu|ükski|Kontakti loomine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="contactgetitem"></a>ContactGetItem
Saada kontakt: toob konkreetse kausta Kontaktid 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kontaktikausta ainuidentifikaator|
|ID|string|Jah|tee|ükski|Kontakti toomiseks ainuidentifikaator|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Kontakti kustutamine: kustutab kontakti 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kontaktikausta ainuidentifikaator|
|ID|string|Jah|tee|ükski|Kontakti kustutamiseks ainuidentifikaator|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="contactpatchitem"></a>ContactPatchItem
Kontakti värskendamine: osaliselt värskendab kontakti 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Tabel|string|Jah|tee|ükski|Kontaktikausta ainuidentifikaator|
|ID|string|Jah|tee|ükski|Kontakti värskendamiseks ainuidentifikaator|
|üksuse| |Jah|sisu|ükski|Kontakti üksuse värskendamine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [stringi, objekti]]


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="object"></a>Objekti


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Manused|massiiv|Ei |
|Kaudu|string|Ei |
|Koopia|string|Ei |
|Salakoopia|string|Ei |
|Teema|string|Jah |
|Sisu|string|Jah |
|Tähtsus|string|Ei |
|IsHtml|kahendmuutuja|Ei |
|Kui soovite|string|Jah |



### <a name="sendattachment"></a>SendAttachment


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|@odata.type|string|Ei |
|Nimi|string|Jah |
|ContentBytes|string|Jah |



### <a name="receivemessage"></a>ReceiveMessage


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Ei |
|IsRead|kahendmuutuja|Ei |
|Omab|kahendmuutuja|Ei |
|DateTimeReceived|string|Ei |
|Manused|massiiv|Ei |
|Kaudu|string|Ei |
|Koopia|string|Ei |
|Salakoopia|string|Ei |
|Teema|string|Jah |
|Sisu|string|Jah |
|Tähtsus|string|Ei |
|IsHtml|kahendmuutuja|Ei |
|Kui soovite|string|Jah |



### <a name="receiveattachment"></a>ReceiveAttachment


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Jah |
|ContentType|string|Jah |
|@odata.type|string|Ei |
|Nimi|string|Jah |
|ContentBytes|string|Jah |



### <a name="digestmessage"></a>DigestMessage


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Teema|string|Jah |
|Sisu|string|Ei |
|Tähtsus|string|Ei |
|Digest|massiiv|Jah |
|Manused|massiiv|Ei |
|Kui soovite|string|Jah |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



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



### <a name="tablemetadata"></a>TableMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Ei |
|pealkiri|string|Ei |
|x-ms-õigus|string|Ei |
|x-ms-funktsioonid|määratletud ei|Ei |
|skeemi|määratletud ei|Ei |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|sortRestrictions|määratletud ei|Ei |
|filterRestrictions|määratletud ei|Ei |
|filterFunctions|massiiv|Ei |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|sorditav|kahendmuutuja|Ei |
|unsortableProperties|massiiv|Ei |
|ascendingOnlyProperties|massiiv|Ei |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|filtreeritavad|kahendmuutuja|Ei |
|nonFilterableProperties|massiiv|Ei |
|requiredProperties|massiiv|Ei |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|NotificationUrl|string|Ei |
|Sõnumi|määratletud ei|Ei |



### <a name="messagewithoptions"></a>MessageWithOptions


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Teema|string|Jah |
|Suvandid|string|Jah |
|Sisu|string|Ei |
|Tähtsus|string|Ei |
|Manused|massiiv|Ei |
|Kui soovite|string|Jah |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Ei |
|ressurss|string|Ei |
|notificationType|string|Ei |
|notificationUrl|string|Ei |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|NotificationUrl|string|Ei |
|Sõnumi|määratletud ei|Ei |



### <a name="approvalmessage"></a>ApprovalMessage


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Teema|string|Jah |
|Suvandid|string|Jah |
|Sisu|string|Ei |
|Tähtsus|string|Ei |
|Manused|massiiv|Ei |
|Kui soovite|string|Jah |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|SelectedOption|string|Ei |



### <a name="tableslist"></a>TablesList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="table"></a>Tabel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Ei |
|DisplayName|string|Ei |



### <a name="item"></a>Üksuse


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ItemInternalId|string|Ei |



### <a name="calendaritemslist"></a>CalendarItemsList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="calendaritem"></a>CalendarItem


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ItemInternalId|string|Ei |



### <a name="contactitemslist"></a>ContactItemsList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="contactitem"></a>ContactItem


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ItemInternalId|string|Ei |



### <a name="datasetslist"></a>DataSetsList


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="dataset"></a>Andmekomplekti


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Ei |
|DisplayName|string|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)