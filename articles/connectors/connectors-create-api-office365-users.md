<properties
    pageTitle="Office 365 kasutajatele konnektor rakendustes loogika lisamine | Microsoft Azure'i"
    description="Office 365 kasutajatele konnektor REST API parameetritega ülevaade"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Office 365 kasutajatele konnektor kasutamise alustamine

Ühenduse Office 365 kasutajatele, et saada profiilid, otsingu kasutajaid ja muud. 

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon.

Office 365 kasutajatele, saate teha järgmist.

- Koostage oma voog kuvatakse Office 365 kasutajate andmete põhjal. 
- Kasuta toiminguid, mida saan otsesed alluvad, saada v haldur kasutajaprofiili ja palju muud. Neid toiminguid vastuse saamine, ja seejärel tehke väljund muude toimingute jaoks saadaval. Näiteks saada isiku otsesed alluvad, ja seejärel võtke see teave ja SQL Azure'i andmebaasi värskendamine. 

Toimingu lisamiseks loogika rakendused, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

Office 365 kasutajatele konnektor on saadaval järgmised toimingud. Ei ole päästikute.

| Päästikute | Toimingud|
| --- | --- |
|Ükski | <ul><li>Hankida haldur</li><li>Saada minu profiil</li><li>Otsesed alluvad hankimine</li><li>Kasutajaprofiili hankimine</li><li>Kasutajate otsimine</li></ul>|

Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 


## <a name="create-a-connection-to-office-365-users"></a>Office 365 kasutajatele ühenduse loomine

Selle konnektori lisamisel loogika rakenduste peate oma Office 365 kasutajatele kontosse sisselogimine ja luba loogika rakendused oma kontoga ühenduse loomiseks.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Pärast ühenduse loomist saate sisestage Office 365 kasutajate atribuute, nagu kasutaja ID-ga. **REST API viide** selles teemas kirjeldatakse neid atribuute.

>[AZURE.TIP] Saate kasutada sama Office 365 kasutajatele sellega loogika muudes rakendustes.


## <a name="office-365-users-rest-api-reference"></a>Office 365 kasutajatele REST API viide
Kehtib versioon: 1.0.

### <a name="get-my-profile"></a>Saada minu profiil 
Tagastab praeguse kasutaja profiili.  
```GET: /users/me``` 

On selle kõne parameetreid pole.

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


### <a name="get-user-profile"></a>Kasutajaprofiili hankimine 
Tagastab kindla kasutajaprofiili.  
```GET: /users/{userId}``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Kasutajanimi|string|Jah|tee|ükski|Kasutaja peamine nime või e-posti id|

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


### <a name="get-manager"></a>Hankida haldur 
Kasutajaprofiili toob määratud kasutaja Manager.  
```GET: /users/{userId}/manager``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Kasutajanimi|string|Jah|tee|ükski|Kasutaja peamine nime või e-posti id|

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



### <a name="get-direct-reports"></a>Otsesed alluvad hankimine 
Saada otsesed alluvad.  
```GET: /users/{userId}/directReports``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|Kasutajanimi|string|Jah|tee|ükski|Kasutaja peamine nime või e-posti id|

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



### <a name="search-for-users"></a>Kasutajate otsimine 
Toob otsingutulemid kasutajaprofiilid.  
```GET: /users``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|searchTerm|string|Ei|päringu|ükski|Otsida stringi (kehtib: nimi, eesnimi, perekonnanimi, e-posti, e-posti hüüdnimi ja kasutaja turvasubjektinimi kuvamine)|

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



## <a name="object-definitions"></a>Objekti määratlused

#### <a name="user-user-model-class"></a>Kasutaja: Kasutaja mudeli klassi

|Atribuudi nimi | Andmetüüp |Nõutav
|---|---|---|
|DisplayName|string|Ei|
|GivenName|string|Ei|
|Perekonnanimi|string|Ei|
|E-posti|string|Ei|
|MailNickname|string|Ei|
|TelephoneNumber|string|Ei|
|AccountEnabled|kahendmuutuja|Ei|
|ID|string|Jah
|UserPrincipalName|string|Ei|
|Osakonna|string|Ei|
|Ametinimetus|string|Ei|
|Mobiiltelefon|string|Ei|


## <a name="next-steps"></a>Järgmised sammud

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

Minge tagasi [API-de loendis](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
