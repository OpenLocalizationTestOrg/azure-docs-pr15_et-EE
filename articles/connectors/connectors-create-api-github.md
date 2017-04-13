<properties
pageTitle="GitHub | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. GitHub on veebipõhine Git hoidla majutusteenuses. See pakub jaotatud paranduse juhtelement ja allika koodi haldus (SCM) funktsioone Git samuti lisada oma funktsioonid."
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

# <a name="get-started-with-the-github-connector"></a>Alustamine GitHub konnektor

GitHub on veebipõhine Git hoidla majutusteenuses. See pakub jaotatud paranduse juhtelement ja allika koodi haldus (SCM) funktsioone Git samuti lisada oma funktsioonid.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse nüüd leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

GitHub konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 GitHub konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="github-actions"></a>GitHub toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Loob probleeme|
### <a name="github-triggers"></a>Päästikute GitHub
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Kui probleemiks on avatud|Probleemi on avatud.|
|Kui probleemiks on suletud|Probleemi on suletud.|
|Kui probleemiks on määratud|Probleemi on määratud|


## <a name="create-a-connection-to-github"></a>GitHub ühenduse loomine
Luua loogika rakenduste GitHub, peate esmalt luua **ühendus** siis üksikasjad pakuvad järgmised atribuudid: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|GitHub mandaat|
Pärast ühenduse loomist saate käivitada toiminguid ja kuulake päästikute, selles artiklis kirjeldatud. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="reference-for-github"></a>GitHub tutvustus
Kehtib versioon: 1.0

## <a name="createissue"></a>CreateIssue
Looge probleemi: loob probleeme 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|repositoryOwner|string|Jah|tee|ükski|Andmebaasi omanik|
|repositoryName|string|Jah|tee|ükski|Andmebaasi nimi|
|issueBasicDetails| |Jah|sisu|ükski|Probleemi üksikasjad|

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


## <a name="issueopened"></a>IssueOpened
Kui probleemiks on avatud: probleemi on avatud. 

```GET: /trigger/issueOpened``` 

Pole selle kõne parameetrid
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


## <a name="issueclosed"></a>IssueClosed
Kui probleemiks on suletud: probleemi on suletud. 

```GET: /trigger/issueClosed``` 

Pole selle kõne parameetrid
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


## <a name="issueassigned"></a>IssueAssigned
Kui probleemiks on määratud: probleemi on määratud 

```GET: /trigger/issueAssigned``` 

Pole selle kõne parameetrid
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|pealkiri|string|Jah |
|sisu|string|Jah |
|uue|string|Jah |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|pealkiri|string|Jah |
|sisu|string|Jah |
|uue|string|Jah |
|arv|string|Ei |
|olek|string|Ei |
|created_at|string|Ei |
|repository_url|string|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)