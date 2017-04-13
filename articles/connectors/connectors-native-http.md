<properties
    pageTitle="HTTP toimingu lisada loogika rakendustes | Microsoft Azure'i"
    description="HTTP toiming atribuutide ülevaade"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>HTTP toimingu alustamine

Toiminguga HTTP saate laiendamine töövoogude oma ettevõtte jaoks ja edastama mis tahes lõpp-punkti http kaudu.

Sa saad:

- Saate luua loogika aktiveerimine (päästik), kui haldate veebisait läheb alla rakendus töövood.
- Mis tahes lõpp-punkti üksteisele http kaudu oma töövoogude laieneda muude teenustega.

HTTP toimingu loogika rakenduse kasutamise alustamine, lugege teemat [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>HTTP päästik kasutamine

Käivitamiseks on sündmus, mida saab kasutada töövoo, mis on määratletud loogika rakenduse käivitamiseks. [Lisateavet päästikute kohta](connectors-overview.md).

Siin on näide jada häälestamise HTTP päästik loogika rakenduse Designer.

1. Lisage loogika rakenduse HTTP päästik.
2. Sisestage parameetrid HTTP lõpp-punkti, mida soovite küsitlus.
3. Muutke Korduvus intervalli kohta, kui sageli see peaks küsitlus.
4. Loogika rakendus käivitub kohe sisuga, mis tagastatakse iga kontrollimise ajal.

![HTTP päästik](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>HTTP päästik tööpõhimõtted

HTTP päästik teeb kõne lõpp HTTP korduva intervall. Vaikimisi HTTP vastuse koodi alla 300 tulemused loogika rakenduse käivitamine. Saate lisada koodi vaates, mis hindab pärast HTTP kõne kindlaks teha, kui loogika rakendus peaks tule tingimus. Siin on näide HTTP päästik, mis käivitub iga kord, kui tagastatud olek kood on suurem kui või võrdne `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

HTTP päästik parameetrid üksikasjadega on saadaval [MSDN-i](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Toiminguga HTTP

Toimingu on toiming, mis teostavad töövoo, mis on määratletud loogika rakendus. [Lisateavet leiate teemast toimingute kohta](connectors-overview.md).

1. Valige nupp **Uus toiming** .
2. Valige **Lisa toiming**.
3. Tippige otsinguväljale toimingu **http** HTTP toimingu lisamiseks.

    ![Valige toiming http kaudu](./media/connectors-native-http/using-action-1.png)

4. Lisage soovitud parameetrid HTTP kõne jaoks vajalike.

    ![Toimingu http kaudu](./media/connectors-native-http/using-action-2.png)

5. Klõpsake vasakus ülanurgas tööriistariba salvestada. Rakenduse loogika on nii salvestamine ja avaldamine (aktiveerida).

## <a name="http-trigger"></a>HTTP päästik

Siin on päästik, mis selle konnektori toetab üksikasjad. HTTP konnektor on üks käivitada.

|Päästik|Kirjeldus|
|---|---|
|HTTP KAUDU|HTTP-kõne muudab ja tagastab vastuse sisu.|

## <a name="http-action"></a>HTTP toiming

Siin on esitatud üksikasjad toimingute jaoks, mille selle konnektori toetab. HTTP konnektor on üks võimalik toiming.

|Toiming|Kirjeldus|
|---|---|
|HTTP KAUDU|HTTP-kõne muudab ja tagastab vastuse sisu.|

## <a name="http-details"></a>HTTP üksikasjad

Järgmises tabelis on kirjeldatud kohustuslike ning valikuliste väljad toiming ja vastavate väljundi üksikasjad, mis on seotud toimingu abil.


#### <a name="http-request"></a>HTTP-päring
Järgnevalt on Sisestuskeel väljad soovitud toimingu, mis teeb väljaminev HTTP-päring.
A * tähendab, et see on nõutav väli.

|Kuvatav nimi|Atribuudi nimi|Kirjeldus|
|---|---|---|
|Meetod *|meetod|HTTP-Verbi kasutamine|
|URI *|URI|HTTP-päringu URI|
|Päised|päised|JSON objekti HTTP päiste kaasamiseks|
|Sisu|sisu|HTTP taotluse keha|
|Autentimine|autentimine|Üksikasjad jaotises [autentimine](#authentication)|
<br>

#### <a name="output-details"></a>Väljundi üksikasjad

Järgmisena kirjeldame väljundi üksikasjad HTTP vastus.

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Päised|objekti|Vastus päised|
|Sisu|objekti|Vastuse objekt|
|Olekukoodi|int|HTTP olekukoodi|

## <a name="authentication"></a>Autentimine

Azure'i rakendust Service loogika rakenduste funktsioon võimaldab kasutada erinevat tüüpi autentimist vastu HTTP lõpp-punktid. Saate seda autentimist **HTTP**, **[HTTP + ärplema](./connectors-native-http-swagger.md)**ja **[HTTP Webhook](./connectors-native-webhook.md)** konnektorid. Järgmist tüüpi autentimine on konfigureeritav.

* [Lihttekstautentimine](#basic-authentication)
* [Kliendi serdi autentimine](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuthi autentimine](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Lihttekstautentimine

Elementaarautentimine on vaja järgmisi autentimine objekti.
A * tähendab, et see on nõutav väli.

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Tippige *|tüüp|Tüüpi autentimine (peab olema `Basic` elementaarautentimine jaoks)|
|Kasutajanimi *|kasutajanimi|Kasutajanimi autentida|
|Parooli *|parooli|Parooli kinnitamiseks|

>[AZURE.TIP] Kui soovite kasutada selline parool, mida ei saa taastada määratluse, kasutage mõnda `securestring` parameeter ja `@parameters()` [töövoo määratlus funktsioon](http://aka.ms/logicappdocs).

Nii oleks väljale autentimine luua objekti umbes järgmine:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Kliendi serdi autentimine

Järgmine autentimise objekt on vaja kliendi serdi autentimist. A * tähendab, et see on nõutav väli.

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Tippige *|tüüp|Tüübi autentimine (peab olema `ClientCertificate` SSL-sertide klient)|
|PFX *|pfx|Isikliku teabe Exchange (PFX) faili sisu Base64-kodeeringuga|
|Parooli *|parooli|Parooli PFX-fail|

>[AZURE.TIP] Saate kasutada mõne `securestring` parameeter ja `@parameters()` [töövoo määratlus funktsioon](http://aka.ms/logicappdocs) parameetrit, mida ei saa pärast salvestamist loogika rakenduse määratluse loetavad kasutada.

Näiteks:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure'i AD OAuthi autentimine

Azure'i AD OAuthi autentimine on vaja järgmist autentimise objekti. A * tähendab, et see on nõutav väli.

|Atribuudi nimi|Andmetüüp|Kirjeldus|
|---|---|---|
|Tippige *|tüüp|Tüübi autentimine (peab olema `ActiveDirectoryOAuth` Azure AD OAuthi jaoks)|
|Rentniku *|rentniku|Rentniku identifikaator Azure AD rentniku jaoks|
|Sihtrühma *|sihtrühma|Määramine`https://management.core.windows.net/`|
|Kliendi ID *|clientId|Kliendi identifikaatori Azure AD Rakenduse|
|Salajane *|salajane|Salajane luba taotleb kliendi|

>[AZURE.TIP] Saate kasutada mõne `securestring` parameeter ja `@parameters()` [töövoo määratlus funktsioon](http://aka.ms/logicappdocs) parameeter, mida ei saa pärast salvestamist määratluse loetavad kasutama.

Näiteks:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, proovige platvormi ja [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Muude saadaolevate konnektorid loogika rakendustes saate uurida vaadates meie [API-de loendis](apis-list.md).
