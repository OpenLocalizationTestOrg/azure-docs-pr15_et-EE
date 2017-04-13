<properties
   pageTitle="Loogika rakenduste sisu tippige töötlemine | Microsoft Azure'i"
   description="Mõista, kuidas loogika rakenduste tegeleb kujundus ja käitusaja sisutüübid"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Loogika rakenduste sisu tippige töötlemine

On palju erinevaid sisu, mida saate flow loogika rakenduse – sh JSON, XML, tasapinnalise failide ja binaarandmeid kaudu.  Küll kõik sisutüübid on toetatud, kuid mõned on algupäraselt mõista mootori loogika rakendused ja teised võivad nõuda casting või dokumenditeisenduste vastavalt vajadusele.  Järgmises artiklis kirjeldatakse kuidas mootori käsitleb eri sisutüüpide ning kuidas nad saavad õigesti töödelda vastavalt vajadusele.

## <a name="content-type-header"></a>Sisutüüp päis

Lihtsa alustamiseks Heitkem pilk kahe `Content-Types` mis ei nõua teisendamise või casting kasutada rakenduses loogika - `application/json` ja `text/plain`.

### <a name="applicationjson"></a>Rakenduse/json

Töövoo engine sõltub selle `Content-Type` HTTP päise kõnede korral käsitlemise.  Mis tahes taotluse sisutüübiga `application/json` salvestatakse ja käsitleda JSON objekt.  Lisaks saate sõeluda vaikimisi JSON sisu, mis tahes casting vajamata.  Nii taotluse, mis on sisutüüp päis `application/json ` järgmiselt:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

saanud töövoos koos sellist avaldist sõeluda `@body('myAction')['foo'][0]` saada väärtus (sel juhul `bar`).  Täiendavad valamist on vaja.  Kui töötate andmetega, mis on JSON, kuid ei ole määratud päise, mida saate käsitsi siirata JSON abil soovitud `@json()` funktsiooni (nt: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Teksti/tavaliseks

Sarnaselt `application/json`, HTTP sõnumeid, kus on `Content-Type` päises `text/plain` salvestatakse töötlemata kujul.  Lisaks, kui kaasatud järgnevate toimingute ilma mis tahes casting taotluse lähevad koos mõne `Content-Type`: `text/plain` päis.  Näiteks kui töötamine lamefailiga võidakse kuvada järgmised HTTP sisu:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Kui järgmise toimingus saatsite selle mõne muu taotluse kehana (`@body('flatfile')`), taotlus on `text/plain` sisutüüp päis.  Kui töötate andmetega, mis on Lihttekst, kuid ei ole määratud päise, mida saate käsitsi siirata teksti abil soovitud `@string()` funktsiooni (nt: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Rakendus-xml ja rakenduse/oktettide-voo ja muunduri funktsioonid

Loogika rakenduse Engine säilitab alati soovitud `Content-Type` mis võeti vastu HTTP taotluse või vastuse.  Mida see tähendab, kui sisu on saadud `Content-Type` , `application/octet-stream`, sh mis järgneva toimingu abil valamist tulemuseks väljamineva taotluse koos `Content-Type`: `application/octet-stream`.  Sel viisil mootori saate guaruntee andmeid ei kaotsi hiirekursori kogu töövoog.  Siiski toimingu olek (sisendi ja väljundi) on talletatud JSON objekti nagu kogu töövoo juhtimine.  See tähendab, et mõned andmetüübid säilitamiseks, mootor teisendab kahendarvu base64-kodeeringuga stringi sobivat metaandmed, mis säilitab nii sisu `$content` ja `$content-type` – mis automaatselt ümber.  Saate teisendada ka käsitsi vahel sisutüübid ehitatud muunduri funktsioonide abil:

* `@json()`-tekitab andmed`application/json`
* `@xml()`-tekitab andmed`application/xml`
* `@binary()`-tekitab andmed`application/octet-stream`
* `@string()`-tekitab andmed`text/plain`
* `@base64()`– teisendab sisu base64 string
* `@base64toString()`– teisendab base64-kodeeringuga stringi`text/plain`
* `@base64toBinary()`– teisendab base64-kodeeringuga stringi`application/octet-stream`
* `@encodeDataUri()`-kodeeritakse dataUri bait massiivi string
* `@decodeDataUri()`-dekodeerib on dataUri üheks baitide massiivis

Oletagem näiteks, et olete saanud HTTP-päring koos `Content-Type`: `application/xml` on:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Ma võiks siirata ja hiljem kasutamine umbes `@xml(triggerBody())`, või funktsiooni, näiteks `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Muud sisutüübid

Muid sisutüüpe on toetatud ja loogika rakendus töötab, kuid võib olla vaja käsitsi toomine sõnumikehas, dekodeerimise soovitud `$content`.  Näiteks, kui mul on käivitumist välja lisamine `application/x-www-url-formencoded` taotluse moodi järgmist:

```
CustomerName=Frank&Address=123+Avenue
```

Kuna see ei lihtteksti- või JSON, salvestatakse see toiming nimega:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Kus `$content` on last, säilitada kõik andmed stringina base64 kodeeringuga.  Kuna pole praegu kohalikke funktsioon vormi andmete jaoks, võib siiski kasutada selle töövoo andmete funktsiooni, näiteks andmeid käsitsi kasutades `@string(body('formdataAction'))`.  Kui mul on ka minu Väljamineva meili taotlus on `application/x-www-url-formencoded` sisutüüp päis, võib lihtsalt lisada selle toimingu keha nagu mis tahes casting ilma `@body('formdataAction')`.  Kuid see toimib ainult kui sisu on ainus parameeter on `body` Sisestuskeel.  Kui proovite teha `@body('formdataAction')` sees, on `application/json` saate käitustõrge nagu encoded keha saadab koosolekukutse.
