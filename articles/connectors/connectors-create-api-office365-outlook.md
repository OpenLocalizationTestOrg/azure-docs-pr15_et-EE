<properties
    pageTitle="Office 365 Outlook Connectori rakenduste loogika lisamine | Microsoft Azure'i"
    description="Saate luua Office 365 Connectori lubamiseks suhtluse teenusekomplektiga Office 365 loogika rakendused. Näide: loomine, redigeerimine ja värskendamine kontaktid ja kalendriüksused."
    services=""    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="anneta"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="10/18/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-office-365-outlook-connector"></a>Office 365 Outlook Connectori kasutamise alustamine 

Office 365 Outlook connector võimaldab suhtlemine Teenusekomplektis Office 365 Outlook. Selle konnektori abil saate luua, redigeerida, ja kontaktid ja kalendriüksused, värskendamine ja saavad ka, saata ja vastata e-posti.

Office 365 Outlooki, saate:

- Koostage oma töövoo kasutamine Office 365 e-posti ja kalendri funktsioone. 
- Päästikute kasutamine oma töövoo käivitamine, kui seal on uus meilisõnum, kui kalendriüksuse on värskendatud ja palju muud.
- Toimingute abil saate meilisõnumit saata, luua uus kalendri sündmus ja palju muud. Näiteks kui Salesforce'i (päästik) on uue objekti, saatke meilisõnum oma Office 365 Outlook (toiming). 

Selles teemas kirjeldatakse, kuidas kasutada Office 365 Outlook Connectori loogika rakenduses ja loetletakse ka päästikute ja toimingute.

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakendused üldiselt kättesaadav (GA).

Loogika rakenduste kohta lisateabe saamiseks lugege teemat [mis on loogika rakendused](../app-service-logic/app-service-logic-what-are-logic-apps.md) ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Office 365 ühendamine

Enne oma loogika rakenduse juurdepääsu mis tahes teenuse, esmalt luua *ühenduse* teenuse. Ühenduse pakub Ühenduvus loogika rakendus ja muu teenus. Näiteks ühenduse Office 365 Outlook, peate esmalt mõne Office 365 *ühenduse*. Ühenduse loomiseks sisestage mandaat tavaliselt abil juurdepääsu teenusele ühenduse loomiseks. Nii Office 365 Outlook, sisestage mandaat teie Office 365 kontoga ühenduse loomine.


## <a name="create-the-connection"></a>Ühenduse loomine

>[AZURE.INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]

## <a name="use-a-trigger"></a>Kasutage käivitamiseks

Käivitamiseks on sündmus, mida saab kasutada määratletud loogika rakenduse töövoo käivitamiseks. Päästikute "küsitlus" teenuse intervall ja sagedus, mida soovite. [Lisateavet päästikute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Loogika rakendus, tippige "office 365" päästikute soovitud loendi saamiseks.  

    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)

2. Valige **Office 365 Outlook - eelseisva sündmuse kiiresti käivitamisel**. Kui ühendus on juba olemas, valige seejärel rippmenüü loendist Kalendri.

    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)

    Kui teil palutakse sisse logida, siis sisestage rakendusse üksikasjad ühenduse loomine. [Loo ühendus](connectors-create-api-office365-outlook.md#create-the-connection) selles teemas on loetletud toimingud. 

    > [AZURE.NOTE] Selles näites loogika rakendus töötab kalendrisündmuse uuendamisel. See käivitab tulemuste vaatamiseks lisada mõne muu toimingu, mis saadab teile sõnumi teksti. Näiteks lisada Twilio *sõnumi saatmine* toimingu selle teksti saate kalendrisündmus käivitamisel 15 minutit. 

3. Klõpsake nuppu **Redigeeri** ja **sagedus** ja **intervall** väärtused. Näiteks kui soovite küsitlus päästik iga 15 minuti järel, seejärel **sagedus** väärtuseks **minutid**ja seada **intervall** **15**. 

    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)

4. **Salvestage** muudatused (ülemises vasakus nurgas tööriistariba). Loogika rakenduse salvestatakse ja võib automaatselt lubatud.


## <a name="use-an-action"></a>Kasutage toimingut

Toimingu on poolt määratletud loogika rakenduse töövoo toimingu. [Lisateavet leiate teemast toimingute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Valige plussmärk. Kuvatakse mitu valikut: **Lisa toimingu**, **Lisa tingimus**või ühe **rohkem** suvandeid.

    ![](./media/connectors-create-api-office365-outlook/add-action.png)

2. Valige **Lisa toiming**.

3. Tippige väljale tekst "office 365" loendi kõik saadaolevad toimingud.

    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 

4. Selles näites valige **Office 365 Outlook - kontakti loomine**. Kui ühendus on juba olemas, siis valige **Kaust ID**, **eesnimi**ja muid atribuute.  

    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)

    Kui teilt küsitakse ühendusteabe, seejärel sisestage üksikasjad ühenduse loomine. [Loo ühendus](connectors-create-api-office365-outlook.md#create-the-connection) selles teemas kirjeldatakse neid atribuute. 

    > [AZURE.NOTE] Selles näites me Office 365 Outlooki uue kontakti loomiseks. Mõne muu päästik väljund abil saate luua kontakti. Näiteks saate lisada päästik Salesforce'i *objekti loomisel* . Lisage Office 365 Outlook *Loo kontakt* kasutab Salesforce'i välju uue uue kontakti loomiseks Teenusekomplektis Office 365 toiming. 

5. **Salvestage** muudatused (ülemises vasakus nurgas tööriistariba). Loogika rakenduse salvestatakse ja võib automaatselt lubatud.


## <a name="technical-details"></a>Tehnilised andmed

Siin on päästikute, toiminguid ja vastuseid, mis toetab sellega üksikasjad.

## <a name="office-365-triggers"></a>Office 365 päästikute

|Päästik | Kirjeldus|
|--- | ---|
|[Kui on eelseisvad sündmuse algab varsti](connectors-create-api-office365-outlook.md#when-an-upcoming-event-is-starting-soon)|See toiming käivitab voog on eelseisvad kalendrisündmuse käivitamisel.|
|[Uue meilisõnumi saabumisel](connectors-create-api-office365-outlook.md#when-a-new-email-arrives)|See toiming käivitab vool uue meilisõnumi saabumisel|
|[Uue sündmuse loomise](connectors-create-api-office365-outlook.md#when-a-new-event-is-created)|See toiming käivitab vool, kui luuakse uus sündmus kalendris.|
|[Kui sündmus on muudetud](connectors-create-api-office365-outlook.md#when-an-event-is-modified)|See toiming käivitab vool, kui sündmus on muudetud kalendri.|


## <a name="office-365-actions"></a>Office 365 toimingud

|Toiming|Kirjeldus|
|--- | ---|
|[Saada e-kirju](connectors-create-api-office365-outlook.md#get-emails)|Selle toimingu saab e-kirju kausta.|
|[Meilisõnumi saatmine](connectors-create-api-office365-outlook.md#send-an-email)|Selle toimingu meilisõnumi saatmine|
|[Meilisõnumi kustutamine](connectors-create-api-office365-outlook.md#delete-email)|See toiming kustutab e-posti ID.|
|[Loetuks märkimine](connectors-create-api-office365-outlook.md#mark-as-read)|Selle toimingu märgib e-posti, kui teil on veel loetud.|
|[E-posti vastamine](connectors-create-api-office365-outlook.md#reply-to-email)|Selle toimingu vastuste e-posti.|
|[Manuse hankimine](connectors-create-api-office365-outlook.md#get-attachment)|Selle toimingu saab manuse e-posti ID.|
|[Saada e-posti ja selle suvandid](connectors-create-api-office365-outlook.md#send-email-with-options)|See toiming saadab meilisõnumiga mitu võimalust ja ootab, et adressaat saaks reageerida tagasi üks suvanditest.|
|[Kinnitamise meilisõnumi saatmine](connectors-create-api-office365-outlook.md#send-approval-email)|See toiming saadab kinnitamise e-posti ja ootab vastust adressaadile.|
|[Kalendrite hankimine](connectors-create-api-office365-outlook.md#get-calendars)|See toiming on loetletud saadaval kalendrid.|
|[Saada sündmused](connectors-create-api-office365-outlook.md#get-events)|Selle toimingu saab sündmuste kalender.|
|[Sündmuse loomine](connectors-create-api-office365-outlook.md#create-event)|See toiming loob uue sündmuse kalendris.|
|[Sündmuse hankimine](connectors-create-api-office365-outlook.md#get-event)|Selle toimingu saab teatud sündmuste kalender.|
|[Sündmuse kustutamine](connectors-create-api-office365-outlook.md#delete-event)|See toiming kustutab kalendris sündmuse.|
|[Sündmuse värskendamine](connectors-create-api-office365-outlook.md#update-event)|See toiming värskendab kalendris sündmuse.|
|[Kontaktikaustade hankimine](connectors-create-api-office365-outlook.md#get-contact-folders)|See toiming on loetletud kontaktikaustade saadaval.|
|[Kontaktide](connectors-create-api-office365-outlook.md#get-contacts)|Selle toimingu saab kontaktide kausta Kontaktid.|
|[Kontakti loomine](connectors-create-api-office365-outlook.md#create-contact)|See toiming loob uue kontakti kaustas Kontaktid.|
|[Saada kontakt](connectors-create-api-office365-outlook.md#get-contact)|Selle toimingu saab konkreetse kausta Kontaktid.|
|[Kontakti kustutamine](connectors-create-api-office365-outlook.md#delete-contact)|See toiming kustutab kontakt kausta Kontaktid.|
|[Kontakti värskendamine](connectors-create-api-office365-outlook.md#update-contact)|See toiming värskendab kontakt kausta Kontaktid.|

### <a name="trigger-and-action-details"></a>Päästiku ja toimingu üksikasjad

Selles jaotises Vt iga päästiku ja toimingu, sh nõutav või valikuline Sisestuskeel atribuudid ja mis tahes vastavate väljundi seostatud konnektor teatud üksikasjad.

#### <a name="when-an-upcoming-event-is-starting-soon"></a>Kui on eelseisvad sündmuse algab varsti
See toiming käivitab voog on eelseisvad kalendrisündmuse käivitamisel. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri ainuidentifikaator|
|lookAheadTimeInMinutes|Ilme edasi aeg|Aeg (minutites) edasi otsida eelseisvaid sündmusi|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarItemsList: Kalendri üksuste loendit

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|väärtus|massiiv|Kalendriüksuste loend|


#### <a name="get-emails"></a>Saada e-kirju
Selle toimingu saab e-kirju kausta. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|folderPath|Kausta tee|Meilisõnumite toomiseks kausta tee (vaikimisi: "Sisendkausta")|
|algusse|Algusse|Arv e-kirju toomiseks (vaikimisi: 10)|
|fetchOnlyUnread|Ainult lugemata sõnumite toomiseks|Saate tuua ainult Lugemata meilisõnumid?|
|includeAttachments|Manustega|Kui on seatud tõene, manuste tuuakse ka koos e-posti|
|searchQuery|Otsingupäringu|Otsingupäringu meilisõnumite filtreerimine|
|Jäta|Jäta|Arv e-kirju vahele (vaikimisi: 0)|
|skipToken|Jäta luba|Luba juurde toomise uus leht|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
ReceiveMessage: Saadetakse meilisõnum

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|Kaudu|string|Kaudu|
|Kui soovite|string|Kui soovite|
|Teema|string|Teema|
|Sisu|string|Sisu|
|Tähtsus|string|Tähtsus|
|Omab|kahendmuutuja|Manustega|
|ID|string|Sõnumi Id|
|IsRead|kahendmuutuja|On kirjutuskaitstud|
|DateTimeReceived|string|Saadud kuupäev-kellaaeg|
|Manused|massiiv|Manused|
|Koopia|string|Määrake meiliaadressid semikooloniga eraldatud fraasi, näitekssomeone@contoso.com|
|Salakoopia|string|Määrake meiliaadressid semikooloniga eraldatud fraasi, näitekssomeone@contoso.com|
|IsHtml|kahendmuutuja|On HTML-vormingus|


#### <a name="send-an-email"></a>Meilisõnumi saatmine
Selle toimingu meilisõnumi saatmine 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|emailMessage *|E-posti|E-posti|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.

#### <a name="delete-email"></a>Meilisõnumi kustutamine
See toiming kustutab e-posti ID. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|messageId *|Sõnumi Id|Meilisõnumi kustutamiseks ID|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.

#### <a name="mark-as-read"></a>Loetuks märkimine
Selle toimingu märgib e-posti, kui teil on veel loetud. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|messageId *|Sõnumi Id|E-posti, et märkida, samuti ID lugemine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.


#### <a name="reply-to-email"></a>E-posti vastamine
Selle toimingu vastuste e-posti. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|messageId *|Sõnumi Id|E-posti vastamine ID-d|
|kommentaari *|Kommentaari|Kommentaari vastus|
|replyAll|Kõigile vastamine|Kõigile adressaatidele vastamine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.


#### <a name="get-attachment"></a>Manuse hankimine
Selle toimingu saab manuse e-posti ID. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|messageId *|Sõnumi Id|E-posti ID|
|attachmentId *|Manuse Id|Manuse allalaadimiseks ID-d|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.


#### <a name="when-a-new-email-arrives"></a>Uue meilisõnumi saabumisel
See toiming käivitab vool uue meilisõnumi saabumisel.

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|folderPath|Kausta tee|E-posti kausta toomiseks (vaikimisi: sisendkausta)|
|Kui soovite|Kui soovite|Adressaatide meiliaadressid|
|kaudu|Kaudu|Aadressilt|
|tähtsus|Tähtsus|E-posti (kõrge, madal, tavaline) prioriteedi (vaikimisi: tavaline)|
|fetchOnlyWithAttachment|Manustega|Saate tuua ainult meilisõnumite manusega|
|includeAttachments|Manustega|Manustega|
|subjectFilter|Teema Filter|Teema otsida stringi|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
TriggerBatchResponse [ReceiveMessage]

| Atribuudi nimi | Andmetüüp |
|---|---|
|väärtus|massiiv|


#### <a name="send-email-with-options"></a>Saada e-posti ja selle suvandid
See toiming saadab meilisõnumiga mitu võimalust ja ootab, et adressaat saaks reageerida tagasi üks suvanditest. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|optionsEmailSubscription *|Tellimuse taotluse suvandid e-posti jaoks|Tellimuse taotluse suvandid e-posti jaoks|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
SubscriptionResponse: Mudeli kinnitamise e-posti tellimuse jaoks

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Tellimuse ID|
|ressurss|string|Tellimuse taotluse ressurss|
|notificationType|string|Teate tüüp|
|notificationUrl|string|Teatise URL-i|


#### <a name="send-approval-email"></a>Kinnitamise meilisõnumi saatmine
See toiming saadab kinnitamise e-posti ja ootab vastust adressaadile. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|approvalEmailSubscription *|Tellimuse taotluse kinnitamine e-posti jaoks|Tellimuse taotluse kinnitamine e-posti jaoks|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
SubscriptionResponse: Mudeli kinnitamise e-posti tellimuse jaoks

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Tellimuse ID|
|ressurss|string|Tellimuse taotluse ressurss|
|notificationType|string|Teate tüüp|
|notificationUrl|string|Teatise URL-i|


#### <a name="get-calendars"></a>Kalendrite hankimine
See toiming on loetletud saadaval kalendrid. 

On selle kõne parameetreid pole.

##### <a name="output-details"></a>Väljundi üksikasjad
TablesList

| Atribuudi nimi | Andmetüüp |
|---|---|
|väärtus|massiiv|


#### <a name="get-events"></a>Saada sündmused
Selle toimingu saab sündmuste kalender. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|$filter|Päringu filtreerimine|ODATA filter päringu tagastatud kirjete piiramine|
|$orderby|Järjestusalus|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|Jäta loendamine|Vahele kirjete arv (vaikimisi = 0)|
|$top|Suurim lubatud Get loendamine|Suurim tuua kirjete arv (vaikimisi = 256)|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarEventList: Kalendri üksuste loendit

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|väärtus|massiiv|Kalendriüksuste loend|


#### <a name="create-event"></a>Sündmuse loomine
See toiming loob uue sündmuse kalendris. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|üksuse *|Üksuse|Sündmuse loomine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarEvent: Konnektor teatud kalendri sündmus mudeli klass.

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Sündmuse kordumatut tunnust.|
|Osalejad|massiiv|Sündmuse osalejate loend.|
|Sisu|määratletud ei|Sündmusega seostatud sõnumi kehasse.|
|BodyPreview|string|Sündmusega seostatud sõnumi eelvaate.|
|Kategooriad|massiiv|Sündmusega seostatud kategooriad.|
|ChangeKey|string|Tuvastab sündmuse objekti versiooni. Iga kord, kui sündmus on muudetud, ChangeKey muudab ka.|
|DateTimeCreated|string|Kuupäeva ja kellaaja, mis on loodud sündmus.|
|DateTimeLastModified|string|Kuupäev ja kellaaeg viimati muudeti sündmus.|
|Lõpeta|string|Sündmuse lõppkellaaeg.|
|EndTimeZone|string|Saate määrata koosoleku ajavöönd lõpuaeg. Väärtus peab olema määratletud Windows (näide: "Vaikse ookeani aja").|
|Manust|kahendmuutuja|Määrake väärtuseks true, kui sündmus on manused.|
|Tähtsus|string|Sündmuse prioriteedi: madal, tavaline või kõrge.|
|IsAllDay|kahendmuutuja|Määrake väärtuseks true, kui sündmus kestab kogu päeva.|
|IsCancelled|kahendmuutuja|Määrake väärtuseks true, kui sündmus on tühistatud.|
|IsOrganizer|kahendmuutuja|Määrake väärtuseks true, kui sõnumi saatja on ka korraldaja.|
|Asukoht|määratletud ei|Sündmuse asukoht.|
|Korraldaja|määratletud ei|Korraldaja sündmus.|
|Korduvus|määratletud ei|Sündmuse korduvuse.|
|Meeldetuletuse|täisarv|Aja minutit enne sündmuse algust meelde.|
|ResponseRequested|kahendmuutuja|Määrake väärtuseks true, kui soovite saatja vastust, kui sündmus on aktsepteerinud või tagasi lükatud.|
|ResponseStatus|määratletud ei|Näitab vastuse saadetud sõnumile sündmuse tüüp.|
|SeriesMasterId|string|Ainuidentifikaator sarja juhtslaidi sündmuse tüüp.|
|ShowAs|string|Kuvatakse vaba või hõivatud.|
|Alustamine|string|Sündmuse algusaeg.|
|StartTimeZone|string|Saate määrata koosoleku zone alguskellaaeg aeg. Väärtus peab olema määratletud Windows (näide: "Vaikse ookeani piirkonna Standard Time").|
|Teema|string|Sündmuse teema.|
|Tüüp|string|Sündmuse tüüp: ühe eksemplari, esinemiskord, erand või sarja juhtslaidi.|
|Veebis|string|Sündmusega seostatud sõnumi eelvaate.|


#### <a name="get-event"></a>Sündmuse hankimine
Selle toimingu saab teatud sündmuste kalender. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|ID *|Üksuse id|Valige sündmuse|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarEvent: Konnektor teatud kalendri sündmus mudeli klass.

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Sündmuse kordumatut tunnust.|
|Osalejad|massiiv|Sündmuse osalejate loend.|
|Sisu|määratletud ei|Sündmusega seostatud sõnumi kehasse.|
|BodyPreview|string|Sündmusega seostatud sõnumi eelvaate.|
|Kategooriad|massiiv|Sündmusega seostatud kategooriad.|
|ChangeKey|string|Tuvastab sündmuse objekti versiooni. Iga kord, kui sündmus on muudetud, ChangeKey muudab ka.|
|DateTimeCreated|string|Kuupäeva ja kellaaja, mis on loodud sündmus.|
|DateTimeLastModified|string|Kuupäev ja kellaaeg viimati muudeti sündmus.|
|Lõpeta|string|Sündmuse lõppkellaaeg.|
|EndTimeZone|string|Saate määrata koosoleku ajavöönd lõpuaeg. Väärtus peab olema määratletud Windows (näide: "Vaikse ookeani aja").|
|Manust|kahendmuutuja|Määrake väärtuseks true, kui sündmus on manused.|
|Tähtsus|string|Sündmuse prioriteedi: madal, tavaline või kõrge.|
|IsAllDay|kahendmuutuja|Määrake väärtuseks true, kui sündmus kestab kogu päeva.|
|IsCancelled|kahendmuutuja|Määrake väärtuseks true, kui sündmus on tühistatud.|
|IsOrganizer|kahendmuutuja|Määrake väärtuseks true, kui sõnumi saatja on ka korraldaja.|
|Asukoht|määratletud ei|Sündmuse asukoht.|
|Korraldaja|määratletud ei|Korraldaja sündmus.|
|Korduvus|määratletud ei|Sündmuse korduvuse.|
|Meeldetuletuse|täisarv|Aja minutit enne sündmuse algust meelde.|
|ResponseRequested|kahendmuutuja|Määrake väärtuseks true, kui soovite saatja vastust, kui sündmus on aktsepteerinud või tagasi lükatud.|
|ResponseStatus|määratletud ei|Näitab vastuse saadetud sõnumile sündmuse tüüp.|
|SeriesMasterId|string|Ainuidentifikaator sarja juhtslaidi sündmuse tüüp.|
|ShowAs|string|Kuvatakse vaba või hõivatud.|
|Alustamine|string|Sündmuse algusaeg.|
|StartTimeZone|string|Saate määrata koosoleku zone alguskellaaeg aeg. Väärtus peab olema määratletud Windows (näide: "Vaikse ookeani piirkonna Standard Time").|
|Teema|string|Sündmuse teema.|
|Tüüp|string|Sündmuse tüüp: ühe eksemplari, esinemiskord, erand või sarja juhtslaidi.|
|Veebis|string|Sündmusega seostatud sõnumi eelvaate.|


#### <a name="delete-event"></a>Sündmuse kustutamine
See toiming kustutab kalendris sündmuse. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|ID *|ID|Valige sündmuse|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.


#### <a name="update-event"></a>Sündmuse värskendamine
See toiming värskendab kalendris sündmuse. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|ID *|ID|Valige sündmuse|
|üksuse *|Üksuse|Sündmuse värskendamine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarEvent: Konnektor teatud kalendri sündmus mudeli klass.

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Sündmuse kordumatut tunnust.|
|Osalejad|massiiv|Sündmuse osalejate loend.|
|Sisu|määratletud ei|Sündmusega seostatud sõnumi kehasse.|
|BodyPreview|string|Sündmusega seostatud sõnumi eelvaate.|
|Kategooriad|massiiv|Sündmusega seostatud kategooriad.|
|ChangeKey|string|Tuvastab sündmuse objekti versiooni. Iga kord, kui sündmus on muudetud, ChangeKey muudab ka.|
|DateTimeCreated|string|Kuupäeva ja kellaaja, mis on loodud sündmus.|
|DateTimeLastModified|string|Kuupäev ja kellaaeg viimati muudeti sündmus.|
|Lõpeta|string|Sündmuse lõppkellaaeg.|
|EndTimeZone|string|Saate määrata koosoleku ajavöönd lõpuaeg. Väärtus peab olema määratletud Windows (näide: "Vaikse ookeani aja").|
|Manust|kahendmuutuja|Määrake väärtuseks true, kui sündmus on manused.|
|Tähtsus|string|Sündmuse prioriteedi: madal, tavaline või kõrge.|
|IsAllDay|kahendmuutuja|Määrake väärtuseks true, kui sündmus kestab kogu päeva.|
|IsCancelled|kahendmuutuja|Määrake väärtuseks true, kui sündmus on tühistatud.|
|IsOrganizer|kahendmuutuja|Määrake väärtuseks true, kui sõnumi saatja on ka korraldaja.|
|Asukoht|määratletud ei|Sündmuse asukoht.|
|Korraldaja|määratletud ei|Korraldaja sündmus.|
|Korduvus|määratletud ei|Sündmuse korduvuse.|
|Meeldetuletuse|täisarv|Aja minutit enne sündmuse algust meelde.|
|ResponseRequested|kahendmuutuja|Määrake väärtuseks true, kui soovite saatja vastust, kui sündmus on aktsepteerinud või tagasi lükatud.|
|ResponseStatus|määratletud ei|Näitab vastuse saadetud sõnumile sündmuse tüüp.|
|SeriesMasterId|string|Ainuidentifikaator sarja juhtslaidi sündmuse tüüp.|
|ShowAs|string|Kuvatakse vaba või hõivatud.|
|Alustamine|string|Sündmuse algusaeg.|
|StartTimeZone|string|Saate määrata koosoleku zone alguskellaaeg aeg. Väärtus peab olema määratletud Windows (näide: "Vaikse ookeani piirkonna Standard Time").|
|Teema|string|Sündmuse teema.|
|Tüüp|string|Sündmuse tüüp: ühe eksemplari, esinemiskord, erand või sarja juhtslaidi.|
|Veebis|string|Sündmusega seostatud sõnumi eelvaate.|


#### <a name="when-a-new-event-is-created"></a>Uue sündmuse loomise
See toiming käivitab vool, kui luuakse uus sündmus kalendris. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|$filter|Päringu filtreerimine|ODATA filter päringu tagastatud kirjete piiramine|
|$orderby|Järjestusalus|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|Jäta loendamine|Vahele kirjete arv (vaikimisi = 0)|
|$top|Suurim lubatud Get loendamine|Suurim tuua kirjete arv (vaikimisi = 256)|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarItemsList: Kalendri üksuste loendit

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|väärtus|massiiv|Kalendriüksuste loend|


#### <a name="when-an-event-is-modified"></a>Kui sündmus on muudetud
See toiming käivitab vool, kui sündmus on muudetud kalendri. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kalendri id|Kalendri valimine|
|$filter|Päringu filtreerimine|ODATA filter päringu tagastatud kirjete piiramine|
|$orderby|Järjestusalus|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|Jäta loendamine|Vahele kirjete arv (vaikimisi = 0)|
|$top|Suurim lubatud Get loendamine|Suurim tuua kirjete arv (vaikimisi = 256)|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
CalendarItemsList: Kalendri üksuste loendit


| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|väärtus|massiiv|Kalendriüksuste loend|


#### <a name="get-contact-folders"></a>Kontaktikaustade hankimine
See toiming on loetletud kontaktikaustade saadaval. 

On selle kõne parameetreid pole.

##### <a name="output-details"></a>Väljundi üksikasjad
TablesList

| Atribuudi nimi | Andmetüüp |
|---|---|
|väärtus|massiiv|


#### <a name="get-contacts"></a>Kontaktide
Selle toimingu saab kontaktide kausta Kontaktid. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kausta id|Kausta Kontaktid tuua ainuidentifikaator|
|$filter|Päringu filtreerimine|ODATA filter päringu tagastatud kirjete piiramine|
|$orderby|Järjestusalus|Päring ODATA orderBy määratlemiseks järjestuse kirjed|
|$skip|Jäta loendamine|Vahele kirjete arv (vaikimisi = 0)|
|$top|Suurim lubatud Get loendamine|Suurim tuua kirjete arv (vaikimisi = 256)|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
ContactList: Kontaktide loend

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|väärtus|massiiv|Kontaktide loend|


#### <a name="create-contact"></a>Kontakti loomine
See toiming loob uue kontakti kaustas Kontaktid. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kausta id|Valige kaust Kontaktid|
|üksuse *|Üksuse|Kontakti loomine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Kontakti: Kontakt

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Kontakti kordumatut tunnust.|
|ParentFolderId|string|Kontakti emakausta ID.|
|Sünnipäeva|string|Kontakti sünni.|
|FileAs|string|Kontakti nimi on esitatud all.|
|DisplayName|string|Kontakti kuvatav nimi.|
|GivenName|string|Kontakti eesnimi.|
|Initsiaalid|string|Kontakti initsiaalid.|
|MiddleName|string|Kontakti teine nimi.|
|Hüüdnimi|string|Kontakti hüüdnimi.|
|Perekonnanimi|string|Kontakti perekonnanimi.|
|Pealkiri|string|Kontakti tiitel.|
|Genereerimine|string|Kontakti loomine.|
|EmailAddresses|massiiv|Kontakti e-posti aadressid.|
|ImAddresses|massiiv|Kontakti kiirsõnumside kiirsõnumside (IM) aadressid.|
|Ametinimetus|string|Kontakti ametinimetus.|
|Ettevõtte nimi|string|Kontakti ettevõtte nimi.|
|Osakonna|string|Kontakti osakonna poole.|
|OfficeLocation|string|Kontakti kontori asukoht.|
|Kutse|string|Kontakti kutse.|
|BusinessHomePage|string|Kontakti äri avalehe.|
|AssistantName|string|Kontakti sisselogimisabimehe nimi.|
|Haldur|string|Funktsiooni Ärikontaktide halduri nimi.|
|HomePhones|massiiv|Kontakti Kodutelefon arvud.|
|BusinessPhones|massiiv|Kontakti business telefoninumbrid|
|MobilePhone1|string|Kontakti mobiiltelefoni number.|
|Koduaadress|määratletud ei|Kontakti kodune aadress.|
|Tööaadress|määratletud ei|Kontakti Tööaadress.|
|OtherAddress|määratletud ei|Aadresside kontakti.|
|YomiCompanyName|string|Kontakti foneetiline Jaapani ettevõtte nimi.|
|YomiGivenName|string|Foneetiline Jaapani antud nimi (eesnimi) kontakti.|
|YomiSurname|string|Jaapani Foneetiline perekonnanimi (perekonnanimi) kontakti|
|Kategooriad|massiiv|Kontaktiga seostatud kategooriad.|
|ChangeKey|string|Tuvastab sündmuse objekti versiooni|
|DateTimeCreated|string|Kontakti loomise aeg.|
|DateTimeLastModified|string|Kontakti muutmise aeg.|


#### <a name="get-contact"></a>Saada kontakt
Selle toimingu saab konkreetse kausta Kontaktid. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kausta id|Valige kaust Kontaktid|
|ID *|Üksuse id|Kontakti toomiseks ainuidentifikaator|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Kontakti: Kontakt

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Kontakti kordumatut tunnust.|
|ParentFolderId|string|Kontakti emakausta ID.|
|Sünnipäeva|string|Kontakti sünni.|
|FileAs|string|Kontakti nimi on esitatud all.|
|DisplayName|string|Kontakti kuvatav nimi.|
|GivenName|string|Kontakti eesnimi.|
|Initsiaalid|string|Kontakti initsiaalid.|
|MiddleName|string|Kontakti teine nimi.|
|Hüüdnimi|string|Kontakti hüüdnimi.|
|Perekonnanimi|string|Kontakti perekonnanimi.|
|Pealkiri|string|Kontakti tiitel.|
|Genereerimine|string|Kontakti loomine.|
|EmailAddresses|massiiv|Kontakti e-posti aadressid.|
|ImAddresses|massiiv|Kontakti kiirsõnumside kiirsõnumside (IM) aadressid.|
|Ametinimetus|string|Kontakti ametinimetus.|
|Ettevõtte nimi|string|Kontakti ettevõtte nimi.|
|Osakonna|string|Kontakti osakonna poole.|
|OfficeLocation|string|Kontakti kontori asukoht.|
|Kutse|string|Kontakti kutse.|
|BusinessHomePage|string|Kontakti äri avalehe.|
|AssistantName|string|Kontakti sisselogimisabimehe nimi.|
|Haldur|string|Funktsiooni Ärikontaktide halduri nimi.|
|HomePhones|massiiv|Kontakti Kodutelefon arvud.|
|BusinessPhones|massiiv|Kontakti business telefoninumbrid|
|MobilePhone1|string|Kontakti mobiiltelefoni number.|
|Koduaadress|määratletud ei|Kontakti kodune aadress.|
|Tööaadress|määratletud ei|Kontakti Tööaadress.|
|OtherAddress|määratletud ei|Aadresside kontakti.|
|YomiCompanyName|string|Kontakti foneetiline Jaapani ettevõtte nimi.|
|YomiGivenName|string|Foneetiline Jaapani antud nimi (eesnimi) kontakti.|
|YomiSurname|string|Jaapani Foneetiline perekonnanimi (perekonnanimi) kontakti|
|Kategooriad|massiiv|Kontaktiga seostatud kategooriad.|
|ChangeKey|string|Tuvastab sündmuse objekti versiooni|
|DateTimeCreated|string|Kontakti loomise aeg.|
|DateTimeLastModified|string|Kontakti muutmise aeg.|


#### <a name="delete-contact"></a>Kontakti kustutamine
See toiming kustutab kontakt kausta Kontaktid. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kausta id|Valige kaust Kontaktid|
|ID *|ID|Kontakti kustutamiseks ainuidentifikaator|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.


#### <a name="update-contact"></a>Kontakti värskendamine
See toiming värskendab kontakt kausta Kontaktid. 

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Kausta id|Valige kaust Kontaktid|
|ID *|ID|Kontakti värskendamiseks ainuidentifikaator|
|üksuse *|Üksuse|Kontakti üksuse värskendamine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Kontakti: Kontakt

| Atribuudi nimi | Andmetüüp | Kirjeldus |
|---|---|---|
|ID|string|Kontakti kordumatut tunnust.|
|ParentFolderId|string|Kontakti emakausta ID.|
|Sünnipäeva|string|Kontakti sünni.|
|FileAs|string|Kontakti nimi on esitatud all.|
|DisplayName|string|Kontakti kuvatav nimi.|
|GivenName|string|Kontakti eesnimi.|
|Initsiaalid|string|Kontakti initsiaalid.|
|MiddleName|string|Kontakti teine nimi.|
|Hüüdnimi|string|Kontakti hüüdnimi.|
|Perekonnanimi|string|Kontakti perekonnanimi.|
|Pealkiri|string|Kontakti tiitel.|
|Genereerimine|string|Kontakti loomine.|
|EmailAddresses|massiiv|Kontakti meiliaadressid.|
|ImAddresses|massiiv|Kontakti kiirsõnumside kiirsõnumside (IM) aadressid.|
|Ametinimetus|string|Kontakti ametinimetus.|
|Ettevõtte nimi|string|Kontakti ettevõtte nimi.|
|Osakonna|string|Kontakti osakonna poole.|
|OfficeLocation|string|Kontakti kontori asukoht.|
|Kutse|string|Kontakti kutse.|
|BusinessHomePage|string|Kontakti äri avalehe.|
|AssistantName|string|Kontakti sisselogimisabimehe nimi.|
|Haldur|string|Funktsiooni Ärikontaktide halduri nimi.|
|HomePhones|massiiv|Kontakti Kodutelefon arvud.|
|BusinessPhones|massiiv|Kontakti business telefoninumbrid|
|MobilePhone1|string|Kontakti mobiiltelefoni number.|
|Koduaadress|määratletud ei|Kontakti kodune aadress.|
|Tööaadress|määratletud ei|Kontakti Tööaadress.|
|OtherAddress|määratletud ei|Aadresside kontakti.|
|YomiCompanyName|string|Kontakti foneetiline Jaapani ettevõtte nimi.|
|YomiGivenName|string|Foneetiline Jaapani antud nimi (eesnimi) kontakti.|
|YomiSurname|string|Jaapani Foneetiline perekonnanimi (perekonnanimi) kontakti|
|Kategooriad|massiiv|Kontaktiga seostatud kategooriad.|
|ChangeKey|string|Tuvastab sündmuse objekti versiooni|
|DateTimeCreated|string|Kontakti loomise aeg.|
|DateTimeLastModified|string|Kontakti muutmise aeg.|



## <a name="http-responses"></a>HTTP vastused

Toimingute ja käivitab ülaltoodud saate tagastavad üks või mitu HTTP olek järgmisi koode. 

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="next-steps"></a>Järgmised sammud

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Tutvuge on saadaval konnektorid loogika rakendustes meie [API-de loendis](apis-list.md).