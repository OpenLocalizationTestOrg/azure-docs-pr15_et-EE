<properties
    pageTitle="Kasutage koosolekukutsete ja kutsele vastamise toimingud | Microsoft Azure'i"
    description="Koosolekukutsete ja kutsele vastamise päästik ja toimingu Azure loogika rakenduse ülevaade"
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

# <a name="get-started-with-the-request-and-response-components"></a>Koosolekukutsete ja kutsele vastamise komponentide kasutamise alustamine

Loogika rakenduse koosolekukutsete ja kutsele vastamise komponendid, saate vastata reaalajas sündmused.

Näiteks saate teha järgmist.

- Vastata HTTP-päring andmetega kohapealse andmebaasist loogika rakenduse kaudu.
- Loogika rakenduse kaudu välise webhook sündmuse käivitamine.
- Helistage loogika rakenduse toiminguga koosolekukutsete ja kutsele vastamise kaudu teise loogika rakendusest.

Koosolekukutsete ja kutsele vastamise toimingud loogika rakenduse kasutamise alustamine, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>HTTP-päring päästik kasutamine

Käivitamiseks on sündmus, mida saab kasutada töövoo, mis on määratletud loogika rakenduse käivitamiseks. [Lisateavet päästikute kohta](connectors-overview.md).

Siin on näide jada häälestamise Designeris loogika rakenduse HTTP-päring.

1. Lisage loogika rakenduse päästik, **taotluse – kui an HTTP-päring** . Soovi korral saate JSON skeemi sisestada taotluse keha (nt [JSONSchema.net](http://jsonschema.net)tööriista abil). See võimaldab designer genereerimiseks loodud sõned atribuutide HTTP-päring.
2. Saate salvestada loogika rakenduse mõne muu toimingu lisamine
3. Pärast salvestamist rakenduse loogika, saate avada HTTP taotluse URL-i taotluse isikukaardi kaudu.
4. Mõne HTTP POSTITA (saate kasutada tööriista nagu [postiljon](https://www.getpostman.com/)) URL-i käivitab loogika rakendus.

>[AZURE.NOTE] Kui määratlete vastuse toimingut, on `202 ACCEPTED` vastus saadetakse kohe helistaja. Vastuse toimingu abil saate kohandada vastuse.

![Vastuse päästik](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Toiminguga HTTP-vastus

HTTP vastuse toiming on lubatud ainult siis, kui kasutate seda töövoo, mis käivitab HTTP-päring. Kui määratlete vastuse toimingut, on `202 ACCEPTED` vastus saadetakse kohe helistaja.  Saate lisada mis tahes etapis selle töövoo vastuse toiming. Loogika rakenduse ainult hoiab sissetuleva taotluse avatud vastuse minutiks.  Pärast hetke, kui töövoog saatjaks oli vastust (ja vastuse toimingu määratlus on olemas), on `504 GATEWAY TIMEOUT` tagastatakse funktsiooni helistaja.

Niimoodi HTTP vastuse toimingu lisamiseks.

1. Valige nupp **Uus toiming** .
2. Valige **Lisa toiming**.
3. Tippige otsinguväljale toimingu **vastuse** loendis vastuse toiming.

    ![Valige toiming vastus](./media/connectors-native-reqres/using-action-1.png)

4. Lisage vajalike HTTP vastuse sõnumi parameetrid.

    ![Toimingu vastus](./media/connectors-native-reqres/using-action-2.png)

5. Klõpsake vasakus ülanurgas tööriistariba salvestamiseks ja rakenduse loogika on nii salvestamine ja avaldamine (aktiveerida).

## <a name="request-trigger"></a>Päästik taotlemine

Siin on päästik, mis selle konnektori toetab üksikasjad. On ühe päringu käivitamiseks.

|Päästik|Kirjeldus|
|---|---|
|Taotlemine|Juhul, kui HTTP-päring on saanud|

## <a name="response-action"></a>Vastuse toiming

Siin on esitatud üksikasjad toimingute jaoks, mille selle konnektori toetab. On ühe vastuse toiming, mida saab kasutada ainult kui taotluse päästik kaasnevad.

|Toiming|Kirjeldus|
|---|---|
|Vastus|Annab vastuseks seotud HTTP-päring|

### <a name="trigger-and-action-details"></a>Päästiku ja toimingu üksikasjad

Järgmises tabelis on kirjeldatud päästik ja toimingu väljad ja vastavad väljund üksikasjad.

#### <a name="request-trigger"></a>Päästik taotlemine
Järgmises sisendi välja jaoks käivitab sissetulevate HTTP-päring kaudu.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|JSON-skeemifailid|skeemi|HTTP taotluse keha JSON skeemi|
<br>

**Väljundi üksikasjad**

Järgnevalt on kutse väljundi üksikasjad.

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Päised|objekti|Taotluse päised|
|Sisu|objekti|Objekti taotlemine|

#### <a name="response-action"></a>Vastuse toiming

HTTP vastuse toimingu Sisestuskeel väljad on järgmised. A * tähendab, et see on nõutav väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|Oleku kood *|statusCode|HTTP olekukoodi|
|Päised|päised|JSON objekti, mis tahes vastus päiste kaasamiseks|
|Sisu|sisu|Vastuse sisu|

## <a name="next-steps"></a>Järgmised sammud

Nüüd, proovige platvormi ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Muude saadaolevate konnektorid loogika rakendustes saate uurida vaadates meie [API-de loendis](apis-list.md).
