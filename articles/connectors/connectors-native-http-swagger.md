
<properties
    pageTitle="Lisada HTTP + ärplema toimingu loogika rakendustes | Microsoft Azure'i"
    description="Ülevaade HTTP + ärplema toimingu ja toimingud"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http--swagger-action"></a>Alustamine HTTP + ärplema toiming

HTTP + ärplema toimingut, saate luua esimese klassi konnektor, mis tahes ülejäänud lõpp-punkti kaudu [ärplema dokumendi](https://swagger.io). Samuti saate laiendada loogika rakenduse helistada kõik ülejäänud lõpp-punkti esimese klassi loogika rakenduse Designer kogemus.

Alustada HTTP + ärplema toimingu loogika rakenduses, lugege teemat [uue loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Kasutage HTTP + ärplema käivitamiseks või toimingu

HTTP + ärplema päästiku ja toimingu funktsioon sama mis [HTTP toiming](connectors-native-http.md) , kuid sobivad paremini kujundus, kuvades kuju API ja väljundeid designer kaudu [ärplema metaandmed](https://swagger.io). Lisaks saate kasutada HTTP + ärplema käivitamiseks. Kui soovite rakendada küsitlused päästik, tuleks see järgib küsitlused muster, mida on kirjeldatud [luua kohandatud API kasutamine loogika rakendustega](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers).

[Lisateavet loogika rakenduse päästikute ja toimingute kohta.](connectors-overview.md)

Siin on näide sellest, kuidas kasutada HTTP + ärplema töötamise loogika rakenduse töövoo toimingu.

1. Valige nupp **Uus toiming** .
2. Valige **Lisa toiming**.
3. Tippige otsinguväljale toimingu **ärplema** loendi HTTP + ärplema toiming.

    ![Valige HTTP + ärplema toiming](./media/connectors-native-http-swagger/using-action-1.png)

4. Tippige ärplema dokumendi URL-i.
    - Loogika rakenduse kujundaja töötamiseks URL peab olema HTTPS-i lõpp-punkti ja CORS lubanud.
    - Kui ärplema dokument ei vasta selle nõude, saate dokumendi talletamine [Azure'i salvestusruumi CORS lubatud](#hosting-swagger-from-storage) .
5. Klõpsake nuppu **edasi** lugemine ja ärplema dokumendist renderdada.
6. Lisage soovitud parameetrid HTTP kõne jaoks vajalike.

    ![Täielik HTTP toiming](./media/connectors-native-http-swagger/using-action-2.png)

1. Klõpsake nuppu **Salvesta** ülemises vasakus nurgas tööriistariba ja rakenduse loogika on nii salvestamine ja avaldamine (aktiveerida).

### <a name="host-swagger-from-azure-storage"></a>Host ärplema: Azure'i salvestusruum

Võite viidata ärplema dokumendi, mis on majutatud või mis ei vasta turbe- ja rist-origin nõuded designer. Selle probleemi lahendamiseks saate Azure Storage ärplema dokumendi talletamine ja lubada CORS dokumendi viidata.  

Siin on loomine ja konfigureerimine ärplema dokumentide talletamine teenuses Azure Storage juhiseid.

1. [Loo konto Azure storage Azure'i bloobimälu](../storage/storage-create-storage-account.md). (Selle tegemiseks määratud õigused **avaliku**Accessi.)
2. Funktsiooni bloobimälu CORS lubamine. [See PowerShelli skripti](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) abil saate konfigureerida, et seade automaatselt.
3. Ärplema faili üles laadida soovitud bloobimälu sisse. Saate seda teha [Azure portaali](https://portal.azure.com) või tööriista nagu [Azure'i salvestusruumi Exploreri](http://storageexplorer.com/)kaudu.
1. HTTPS-link Azure'i bloobimälu dokumendile viidata. (Lingi järgib vormingut `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Tehnilised andmed

Järgnevalt on toodud andmed käivitab ja toiminguid, mis seda HTTP + ärplema konnektor toetab.

## <a name="http--swagger-triggers"></a>HTTP + ärplema päästikute

Käivitamiseks on sündmus, mida saab kasutada töövoo, mis on määratletud loogika rakenduse käivitamiseks. [Lugege lisateavet päästikute.](connectors-overview.md) HTTP + ärplema konnektor on üks käivitada.

|Päästik|Kirjeldus|
|---|---|
|HTTP + ärplema|HTTP kaudu helistamine ja tagastada vastuse sisu|

## <a name="http--swagger-actions"></a>HTTP + ärplema toimingud

Toimingu on toiming, mis teostavad töövoo, mis on määratletud loogika rakendus. [Lugege lisateavet toimingud.](connectors-overview.md) HTTP + ärplema konnektor on üks võimalik toiming.

|Toiming|Kirjeldus|
|---|---|
|HTTP + ärplema|HTTP kaudu helistamine ja tagastada vastuse sisu|

### <a name="action-details"></a>Toimingu üksikasjad

HTTP + ärplema konnektor on üks võimalik toiming. Järgmine on teavet iga toimingud, nende kohustuslike ning valikuliste väljad ja vastava väljundi üksikasjad, mis on seotud nende kasutamist.

#### <a name="http--swagger"></a>HTTP + ärplema

Veenduge abi ärplema metaandmete väljaminev HTTP-päring.
Tärn (*) tähendab, et kohustuslik väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|Meetod *|meetod|HTTP-Verbi kasutada.|
|URI *|URI|HTTP-päringu URI.|
|Päised|päised|JSON objekti HTTP päiste kaasata.|
|Sisu|sisu|HTTP taotluse keha.|
|Autentimine|autentimine|Taotlus kasutada autentimist. [Lisateavet leiate teemast HTTP](./connectors-native-http.md#authentication).|

**Väljundi üksikasjad**

HTTP-vastus

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Päised|objekti|Vastus päised|
|Sisu|objekti|Vastuse objekt|
|Olekukoodi|int|HTTP olekukoodi|

### <a name="http-responses"></a>HTTP vastused

Kõned erinevate toimingute tegemisel võidakse kuvada teatud vastuseid. Järgmine on tabel, mis kirjeldab vastava vastuste ja kirjeldused.

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge.|

---

## <a name="next-steps"></a>Järgmised sammud

Proovige platvormi ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md) kohe. Muude saadaolevate konnektorid loogika rakendustes saate uurida vaadates meie [API-de loendis](apis-list.md).
