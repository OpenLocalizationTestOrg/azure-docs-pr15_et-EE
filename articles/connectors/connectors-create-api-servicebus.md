<properties
pageTitle="Saate teada, kuidas kasutada Azure teenuse siini konnektor loogika rakenduste | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Ühenduse loomine teenuse Azure siini, et sõnumite saatmiseks ja vastuvõtmiseks. Saate teha toiminguid, näiteks saada järjekorda, saata teema, saadud järjekorra ja saadud tellimus."
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
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Azure'i teenus siini konnektor kasutamise alustamine

Ühenduse loomine teenuse Azure siini, et sõnumite saatmiseks ja vastuvõtmiseks. Saate teha toiminguid, näiteks saada järjekorda, saata teema, saadud kuhjuda ja saadud tellimus.

[Mis tahes konnektor](./apis-list.md)kasutamiseks peate esmalt loogika rakenduse loomine. Saate alustada, [luues loogika rakenduse kohe](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Ühenduse loomine teenuse siini

Enne oma loogika rakenduse juurdepääsu mis tahes teenuse, peate esmalt teenusega ühenduse loomiseks. [Ühenduse](./connectors-overview.md) pakub Ühenduvus loogika rakendus ja muu teenus.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Teenuse siini päästik kasutamine

Käivitamiseks on sündmus, mida saab kasutada määratletud loogika rakenduse töövoo käivitamiseks. [Lisateavet päästikute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Teenuse siini toimingut kasutada

Toimingu on poolt määratletud loogika rakenduse töövoo toimingu. [Lisateavet leiate teemast toimingute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Tehnilised andmed

Siin on esitatud üksikasjad päästikute, toiminguid ja vastuseid, mis toetab sellega kohta.

### <a name="service-bus-triggers"></a>Teenuse siini päästikute

Teenuse siini on järgmine päästikute.  

|Päästik | Kirjeldus|
|--- | ---|
|[Kui sõnum on vastu võetud järjekorras](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|See toiming käivitab vool, kui teate, mis on saanud järjekorda.|
|[Kui sõnum on vastu võetud teema tellimus](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|See toiming käivitab vool sõnumi saabumisel teema tellimus.|


### <a name="service-bus-actions"></a>Teenuse siini toimingud

Teenuse siini järgmisi toiminguid:


|Toiming|Kirjeldus|
|--- | ---|
|[Sõnumi saatmine](connectors-create-api-servicebus.md#send-message)|See toiming saadab sõnumi järjekorda või teema.|
### <a name="action-and-trigger-details"></a>Toimingu ja päästik üksikasjad

Siin on toimingud üksikasjad ja käivitab selle konnektor koos vastustega.



#### <a name="send-message"></a>Sõnumi saatmine

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|ContentData *|Sisu|Sõnumi sisu.|
|ContentType|Sisutüüp|Sisutüübi sõnumi sisu.|
|Atribuudid|Atribuudid|Võti ja väärtuse paarideks iga vahendajaks atribuut.|
|entityName *|Järjekorda/teema nimi|Järjekorda või teema nimi.|

Need täpsemad parameetrid on saadaval.

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|MessageId|Sõnumi Id|Kasutaja määratletud väärtus, mis teenuse siini abil saate tuvastada eksemplaris sõnumeid, kui lubatud.|
|Kui soovite|Kui soovite|Aadress – saata.|
|OutboundSMTPServeri parameetrid|Vastamine|Aadress kuhjuda vastamine.|
|ReplyToSessionId|Seansi Id vastamine|Identifikaator seansi vastamine.|
|Sildi|Sildi|Rakendusele vastav silt.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Kuupäev ja kellaaeg, UTC, kui sõnumi lisatakse järjekorda.|
|SeansiId|Seansi Id|Seansi ID.|
|CorrelationId|Korrelatsiooni Id|Korrelatsiooni ID.|
|TimeToLive|Eluiga|Kestus, puugid, et sõnum on lubatud. Kui sõnum on saadetud teenuse siini algab kestus.|



Mõne * näitab, et atribuut on nõutav.


#### <a name="when-a-message-is-received-in-a-queue"></a>Kui sõnum on vastu võetud järjekorras

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|queueName *|Järjekorra nimi|Järjekorra nimi.|


Mõne * näitab, et atribuut on nõutav.


##### <a name="output-details"></a>Väljundi üksikasjad

ServiceBusMessage: Objektil sisu ja teenuse siini sõnumi atribuudid.


| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ContentData|string|Sõnumi sisu.|
|ContentType|string|Sisutüübi sõnumi sisu.|
|Atribuudid|objekti|Võti ja väärtuse paarideks iga vahendajaks atribuut.|
|MessageId|string|Kasutaja määratletud väärtus, mis teenuse siini abil saate tuvastada eksemplaris sõnumeid, kui lubatud.|
|Kui soovite|string|Saata aadress.|
|OutboundSMTPServeri parameetrid|string|Aadress kuhjuda vastamine.|
|ReplyToSessionId|string|Identifikaator seansi vastamine.|
|Sildi|string|Rakendusele vastav silt.|
|ScheduledEnqueueTimeUtc|string|Kuupäev ja kellaaeg, UTC, kui sõnumi lisatakse järjekorda.|
|SeansiId|string|Seansi ID.|
|CorrelationId|string|Korrelatsiooni ID.|
|TimeToLive|string|Kestus, puugid, et sõnum on lubatud. Kui sõnum on saadetud teenuse siini algab kestus.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Kui sõnum on vastu võetud teema tellimus

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|topicName *|Teema nimi|Nime teema.|
|subscriptionName *|Teema: tellimuse nimi|Teema: tellimuse nime.|


Mõne * näitab, et atribuut on nõutav.


##### <a name="output-details"></a>Väljundi üksikasjad

ServiceBusMessage: Objektil sisu ja teenuse siini sõnumi atribuudid.


| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ContentData|string|Sõnumi sisu.|
|ContentType|string|Sisutüübi sõnumi sisu.|
|Atribuudid|objekti|Võti ja väärtuse paarideks iga vahendajaks atribuut.|
|MessageId|string|Kasutaja määratletud väärtus, mis teenuse siini abil saate tuvastada eksemplaris sõnumeid, kui lubatud.|
|Kui soovite|string|Saata aadress.|
|OutboundSMTPServeri parameetrid|string|Aadress kuhjuda vastamine.|
|ReplyToSessionId|string|Identifikaator seansi vastamine.|
|Sildi|string|Rakendusele vastav silt.|
|ScheduledEnqueueTimeUtc|string|Kuupäev ja kellaaeg, UTC, kui sõnumi lisatakse järjekorda.|
|SeansiId|string|Seansi ID.|
|CorrelationId|string|Korrelatsiooni ID.|
|TimeToLive|string|Kestus, puugid, et sõnum on lubatud. Kui sõnum on saadetud teenuse siini algab kestus.|



### <a name="http-responses"></a>HTTP vastused

Eelmise toimingute ja käivitab võib tagastada ühte või mitut järgmisi koode HTTP olek:

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge.|
|Vaikimisi|Toiming nurjus.|

## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).
