<properties
pageTitle="Projectonline'i | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Project Online on projekti portfellihalduse (PPM) ja igapäevatööd Microsoft Online'i paindlik lahendus. Kohale toimetatud – Office 365, Project Online võimaldab asutustel võimsaid projekti võimalustega kavandamine, eriti ja projektide ja projekti portfellivalikuotsuse investeeringute haldamine kiiresti hakata – peaaegu kõikjalt peaaegu igast seadmest."
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

# <a name="get-started-with-the-projectonline-connector"></a>Projectonline'i konnektor kasutamise alustamine

Project Online on projekti portfellihalduse (PPM) ja igapäevatööd Microsoft Online'i paindlik lahendus. Kohale toimetatud – Office 365, Project Online võimaldab asutustel võimsaid projekti võimalustega kavandamine, eriti ja projektide ja projekti portfellivalikuotsuse investeeringute haldamine kiiresti hakata – peaaegu kõikjalt peaaegu igast seadmest.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon. 

Saate alustada, luues loogika rakenduse nüüd leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

Projectonline'i konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus. 

 Projectonline'i konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="projectonline-actions"></a>Projectonline'i toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Project Online'i saidil projektide loendid|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Loob uue projekti project Online'i saidile|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Uue tööülesande saate projekti|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Loob mõne ettevõtteressursid project Online'i saidile|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Avaldatud projekti tööülesannete loendid|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Kontrollib, projekti saidil|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Olemasoleva projekti saidi sissemöllimine ja avaldamine|
### <a name="projectonline-triggers"></a>Projectonline'i päästikute
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Uue projekti loomise|Käivitab vool iga kord, kui luuakse uus projekt|
|Millal on loodud uue ressursi|Käivitab uue kulgemist, kui luuakse uus ressurss|
|Uue ülesande loomise|Käivitab vool, kui luuakse uus ülesanne|


## <a name="create-a-connection-to-projectonline"></a>Projectonline'i ühenduse loomine
Projectonline'i loogika rakenduste loomiseks peate esmalt luua **ühenduse** siis üksikasjad pakuvad järgmised atribuudid: 

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|Projectonline'i mandaat|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="reference-for-projectonline"></a>Projectonline'i tutvustus
Kehtib versioon: 1.0

## <a name="onnewproject"></a>OnNewProject
Uue projekti loomise: käivitab vool iga kord, kui luuakse uus projekt 

```GET: /trigger/_api/ProjectData/Projects``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Millal on loodud uue ressursi: käivitab uue kulgemist, kui luuakse uus ressurss 

```GET: /trigger/_api/ProjectData/Resources``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Uue ülesande loomise: käivitab vool, kui luuakse uus ülesanne 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Projektide loend: loetletakse projektid project Online'i saidil 

```GET: /_api/ProjectServer/Projects``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Loob uue projekti: loob uue projekti project Online'i saidile 

```POST: /_api/ProjectServer/Projects``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|
|proj| |Jah|sisu|ükski|Uue projekti loomine|

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


## <a name="createtask"></a>CreateTask
Uue tööülesande: loob uue ülesande, mida Projectis 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|string|Jah|tee|ükski|Ülesande lisamine projekti Ainuidentifikaator|
|ülesanne| |Jah|sisu|ükski|Uue ülesande lisamiseks projekti|

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


## <a name="createresource"></a>CreateResource
Saate luua uue ressursi: loob mõne ettevõtteressursid project Online'i saidile 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|
|ressurss| |Jah|sisu|ükski|Uue ettevõtte ressurss lisada projekti|

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


## <a name="listtasks"></a>ListTasks
Tööülesannete loendid: avaldatud projekti tööülesannete loendid 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|string|Jah|tee|ükski|Projekti tööülesannete toomiseks Ainuidentifikaator|

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


## <a name="checkoutproject"></a>CheckoutProject
Väljamöllimise projekti: möllib välja projekti saidil 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|string|Jah|tee|ükski|Ülesande lisamine projekti Ainuidentifikaator|

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


## <a name="publishproject"></a>PublishProject
Sisse- ja projekti avaldada: sissemöllimine ja avaldada olemasoleva projekti saidil 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|siteUrl|string|Jah|päringu|ükski|Projektisait saidi URL-i root (näide: https://sampletenant.sharepoint.com/teams/sampleteam)|
|project_id|string|Jah|tee|ükski|Projekti sissemöllimise Ainuidentifikaator|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="triggerproject"></a>TriggerProject


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ProjectStartDate|string|Ei |
|ProjectFinishDate|string|Ei |
|ProjectCreatedDate|string|Ei |
|ProjectId|string|Ei |
|ProjectModifiedDate|string|Ei |
|ProjectType|täisarv|Ei |
|ProjectName|string|Ei |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="triggerresource"></a>TriggerResource


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ResourceIdkasutamisel|string|Ei |
|ResourceBaseCalendar|string|Ei |
|ResourceBookingType|täisarv|Ei |
|ResourceCanLevel|kahendmuutuja|Ei |
|ResourceCostPerUse|arv|Ei |
|ResourceCreatedDate|string|Ei |
|ResourceEarliestAvailableFrom|string|Ei |
|ResourceEmail|string|Ei |
|ResourceInitials|string|Ei |
|ResourceIsActive|kahendmuutuja|Ei |
|ResourceIsGeneric|kahendmuutuja|Ei |
|ResourceLatestAvailableTo|string|Ei |
|ResourceModifiedDate|string|Ei |
|ResourceName|string|Ei |
|ResourceStatsuName|string|Ei |
|ResourceType|täisarv|Ei |
|TypeDescription|string|Ei |
|TypeName|string|Ei |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="triggertask"></a>TriggerTask


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ProjectId|string|Ei |
|TaskId|string|Ei |
|ProjectName|string|Ei |
|TaskName|string|Ei |
|TaskCreatedDate|string|Ei |
|TaskModifieddate|string|Ei |
|TaskStartDate|string|Ei |
|TaskFinishDate|string|Ei |
|TaskPriority|täisarv|Ei |
|TaskIsActive|kahendmuutuja|Ei |



### <a name="newproject"></a>NewProject


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Jah |
|Kirjeldus|string|Ei |
|Alustamine|string|Ei |



### <a name="newreource"></a>NewReource


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Jah |
|IsBudget|kahendmuutuja|Ei |
|IsGeneric|kahendmuutuja|Ei |
|IsInactive|kahendmuutuja|Ei |



### <a name="project"></a>Projekti


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ApprovedStart|string|Ei |
|ApprovedEnd|string|Ei |
|CheckedOutDate|string|Ei |
|CheckOutDescription|string|Ei |
|CheckOutId|string|Ei |
|CreatedDate|string|Ei |
|ID|string|Ei |
|IsCheckedOut|kahendmuutuja|Ei |
|LastPublishedDate|string|Ei |
|LastSavedDate|string|Ei |
|OptimizerDecision|täisarv|Ei |
|PlannerDecision|täisarv|Ei |
|ProjectType|täisarv|Ei |
|Nimi|string|Ei |
|WinprojVersion|string|Ei |



### <a name="projectswrapper"></a>ProjectsWrapper


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="newtask"></a>NewTask


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|parameetrid|määratletud ei|Jah |



### <a name="taskparameters"></a>TaskParameters


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Jah |
|Märkmete|string|Ei |
|Alustamine|string|Ei |
|Kestus|string|Ei |



### <a name="enterpriseresource"></a>EnterpriseResource


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|CanLevel|kahendmuutuja|Ei |
|Kood|string|Ei |
|CostAccrual|täisarv|Ei |
|CostCenter|string|Ei |
|Loodud|string|Ei |
|DefaultBookingType|täisarv|Ei |
|E-posti|string|Ei |
|ExternalId|string|Ei |
|Rühma|string|Ei |
|HireDate|string|Ei |
|ID|string|Ei |
|Initsiaalid|string|Ei |
|IsActive|kahendmuutuja|Ei |
|IsBudget|kahendmuutuja|Ei |
|IsCheckedOut|kahendmuutuja|Ei |
|IsGeneric|kahendmuutuja|Ei |
|IsTeam|kahendmuutuja|Ei |
|MaterialLabel|string|Ei |
|Muudetud|string|Ei |
|Nimi|string|Ei |
|Foneetika|string|Ei |
|ResourceType|täisarv|Ei |
|TerminationDate|string|Ei |



### <a name="taskswrapper"></a>TasksWrapper


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|väärtus|massiiv|Ei |



### <a name="task"></a>Ülesanne


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Loodud|string|Ei |
|Muudetud|string|Ei |
|Alustamine|string|Ei |
|Valmis|string|Ei |
|Nimi|string|Ei |
|ID|string|Ei |
|Priority (prioriteet)|täisarv|Ei |
|Valmidusprotsent|täisarv|Ei |
|Märkmete|string|Ei |
|Kontakti|string|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)