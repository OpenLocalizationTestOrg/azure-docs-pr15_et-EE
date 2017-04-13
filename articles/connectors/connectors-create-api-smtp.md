<properties
pageTitle="SMTP | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Ühenduse SMTP e-posti saata."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Alustamine SMTP konnektor

Ühenduse SMTP e-posti saata.

[Mis tahes konnektor](./apis-list.md)kasutamiseks peate esmalt loogika rakenduse loomine. Saate alustada, [luues loogika rakenduse kohe](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>SMTP ühenduse loomine

Enne oma loogika rakenduse juurdepääsu mis tahes teenuse, peate esmalt teenusega *ühenduse* loomiseks. [Ühenduse](./connectors-overview.md) pakub Ühenduvus loogika rakendus ja muu teenus. Näiteks SMTP ühendust luua, et peate esmalt on SMTP- *ühendus*. Ühenduse loomiseks oleks vaja mandaat, tavaliselt abil juurdepääsu teenusele ühenduse loomiseks. SMTP näites, peate identimisteavet ühenduse nimi, SMTP-serveri aadress ja kasutajale sisselogimisteave SMTP ühenduse loomiseks. [Lugege lisateavet ühenduste]()  

### <a name="create-a-connection-to-smtp"></a>SMTP ühenduse loomine

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Kasutage lisamispäästiku SMTP

Käivitamiseks on sündmus, mida saab kasutada määratletud loogika rakenduse töövoo käivitamiseks. [Lisateavet päästikute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Selles näites, kuna SMTP ei ole oma, käivitamiseks kasutame **Salesforce'i - objekti loomisel** päästik. See käivitab aktiveerib Salesforce'i uue objekti loomisel. Selles näites me saate selle seadistada nii, et iga kord, kui uue müügivihje on loodud Salesforce, SMTP konnektor on loodud uue müügivihje teatis koos toimub *saatmine e-posti* toimingu.

1. Sisestage otsinguväljale *salesforce'i* loogika rakenduste veebikujundaja, seejärel valige päästik **Salesforce'i - objekti loomisel** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Juhtelemendi **objekti loomisel** kuvatakse.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Valige **Objekti tüüp** valige *juhtimine* objektide loendist. Selles etapis tuleb teil on, mis näitab, päästik, mis teavitab iga kord, kui luuakse uus müügivihje Salesforce'i loogika rakenduse loomisel.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Käivitab on loodud.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Toimingu SMTP kasutamine

Toimingu on poolt määratletud loogika rakenduse töövoo toimingu. [Lisateavet leiate teemast toimingute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Nüüd, kui päästiku on lisatud, järgige SMTP toimingut, mis juhtub uue müügivihje on loodud Salesforce'i lisamiseks tehke järgmist.

1. Valige **+ uus samm** toiming, mida soovite teha, kui luuakse uus müügivihje lisada.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Valige **Lisa toiming**. See avatakse välja Otsing, kus saate otsida mis tahes toimingu te soovite teha.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Sisestage *smtp* otsimiseks SMTP seotud toiminguid.  

4. Valige toiming, mis tuleks sooritada siis, kui luuakse uus müügivihje **SMTP - meili saata** . Toimingu juhtelemendi blokeerimise avatakse. Peate kujundaja blokeerimise smtp ühendust luua, kui te pole seda teinud varem.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Sisestage soovitud meiliteabe **SMTP - saata meilisõnumi** blokeerimine.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Selleks, et aktiveerida oma töövoo töö salvestamine.  

## <a name="technical-details"></a>Tehnilised andmed

Siin on päästikute, toiminguid ja vastuseid, mis toetab sellega üksikasjad.

## <a name="smtp-triggers"></a>SMTP päästikute

SMTP on pole päästikute. 

## <a name="smtp-actions"></a>SMTP toimingud

SMTP on järgmine toiming.


|Toiming|Kirjeldus|
|--- | ---|
|[Meilisõnumi saatmine](connectors-create-api-smtp.md#send-email)|See toiming saadab e-posti ühele või mitmele adressaadile.|

### <a name="action-details"></a>Toimingu üksikasjad

Siin on esitatud üksikasjad selle konnektori koos oma vastustega toimingute jaoks.


### <a name="send-email"></a>Meilisõnumi saatmine
See toiming saadab e-posti ühele või mitmele adressaadile. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|Kui soovite|Kui soovite|Määrake meiliaadressid semikooloniga eraldatud fraasi, näiteksrecipient1@domain.com;recipient2@domain.com|
|KOOPIA|koopia|Määrake meiliaadressid semikooloniga eraldatud fraasi, näiteksrecipient1@domain.com;recipient2@domain.com|
|Teema|Teema|E-posti teema|
|Sisu|Sisu|E-posti sisu|
|Kaudu|Kaudu|Näiteks saatja meiliaadress.sender@domain.com|
|IsHtml|On HTML-vormingus|Saatke meilisõnum HTML-vormingus (tõene/väär)|
|Salakoopia|Salakoopia|Määrake meiliaadressid semikooloniga eraldatud fraasi, näiteksrecipient1@domain.com;recipient2@domain.com|
|Tähtsus|Tähtsus|Oluline on e-posti (kõrge, tavaline või madal)|
|ContentData|Manuste sisu andmed|Andmete sisu (base64 kodeeringuga saab voogu ja -on stringi)|
|ContentType|Manuste sisutüüp|Sisutüüp|
|ContentTransferEncoding|Manuste sisu edastamine kodeerimine|Sisu edastamine kodeering (base64 või mitte keegi)|
|Faili nimi|Manused faili nimi|Faili nimi|
|ContentId|Manuste sisu ID|Sisu id|

Mõne * näitab, et atribuut on nõutav


## <a name="http-responses"></a>HTTP vastused

Toimingute ja käivitab ülaltoodud saate tagastavad ühte või mitut HTTP olek järgmisi koode. 

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
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)