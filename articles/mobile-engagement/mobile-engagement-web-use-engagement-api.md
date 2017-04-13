<properties
    pageTitle="Azure'i mobiilsideseadmete kaasamine Web SDK API-de | Microsoft Azure'i"
    description="Uusimad värskendused ja toiminguid Web SDK Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Veebirakenduse Azure Mobile kaasamine API kasutamine

Selles dokumendis on lisaks dokumendi, mis ütleb teile, kuidas [integreerida veebirakenduse Mobile allikaid](mobile-engagement-web-integrate-engagement.md). Pakub põhjalikumat Azure Mobile kaasamine API abil saate oma rakenduse statistika aruande üksikasjad.

Mobile kaasamine API on esitatud selle `engagement.agent` objekti. Azure'i Mobile kaasamine Web SDK alias on vaikimisi `engagement`. Saate määratleda selle pseudonüümi SDK konfiguratsiooni.

## <a name="mobile-engagement-concepts"></a>Mobile kaasamine mõisted

Järgmised osad piiritleda levinud web platvormi [Mobile kaasamine põhimõtet](mobile-engagement-concepts.md) .

### <a name="session-and-activity"></a>`Session`ja`Activity`

Kui kasutaja jääb jõude kaks tegevuste vahel üle mõne sekundi, kasutaja tegevuste jada on kaks erinevat seansid tükeldatud. Seansi ajalõpp teie nimetatakse neid mõne sekundi.

Kui veebirakendust ei ise deklareerida kasutaja tegevuste lõppu (helistades on `engagement.agent.endActivity` funktsioon), Mobile kaasamine serveri aegub automaatselt kasutaja seansi kolme minuti jooksul pärast selle rakenduse leht on suletud. Seda nimetatakse seansi ajalõpu.

### `Crash`

Vaikimisi ei looda automatiseeritud aruanded tabamatu JavaScripti erandid. Siiski saate teatada jookseb käsitsi, kasutades funktsiooni `sendCrash` (vt jaotis aruandluse jookseb).

## <a name="reporting-activities"></a>Tegevuste teatamine

Kui kasutaja käivitab uue ja kasutajale lõppemisel praeguse tegevuse aruandlus kasutaja tegevuste sisaldab.

### <a name="user-starts-a-new-activity"></a>Kasutaja käivitab uue

    engagement.agent.startActivity("MyUserActivity");

Helistage `startActivity()` muudab iga kasutaja tegevust. Selle funktsiooni esimene käivitab uue kasutaja seansiga.

### <a name="user-ends-the-current-activity"></a>Kasutaja lõpeb praeguse tegevuse

    engagement.agent.endActivity();

Helistage `endActivity()` vähemalt ühe korra siis, kui kasutaja lõpetab viimase tegevuse. See teatab Mobile kaasamine Web SDK, et kasutaja on praegu jõude ja, et sulgeda pärast aegumist seansi ajalõpp peab kasutaja seansi. Kui helistate `startActivity()` enne seansi ajalõpp teie seansi lihtsalt uuesti alustada.

Kuna seal on usaldusväärne kutsumise navigaator akna sulgemisel, on sageli keeruline või ei ole võimalik jaole juba kasutaja tegevuste veebikeskkonnas tekstivälja lõppu. Sellepärast Mobile kaasamine serveri aegub automaatselt kasutaja seansi kolme minuti jooksul pärast selle rakenduse leht on suletud.

## <a name="reporting-events"></a>Aruannete sündmused

Sündmuste kohta hõlmab seansi sündmused ja autonoomse sündmused.

### <a name="session-events"></a>Seansi sündmused

Tavaliselt kasutatakse seansi sündmuste kasutaja toimingud kasutaja seansi jooksul teatada.

**Näide ilma andmeteta eest:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Näide eest andmetega:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Autonoomse sündmused

Erinevalt seansi sündmusi, autonoomne saate sündmused väljaspool seanss.

Selleks kasutage ``engagement.agent.sendEvent`` asemel ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Aruannete tõrked

Tõrgete kohta hõlmab seansi tõrgete ja autonoomse tõrkeid.

### <a name="session-errors"></a>Seansi tõrked

Tavaliselt kasutatakse seansi tõrked teatada tõrkeid, mis mõjutavad kasutaja kasutaja seansi jooksul.

**Näide ilma andmeteta eest:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Näide eest andmetega:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Autonoomse tõrked

Erinevalt seansi tõrgete, autonoomne tõrked võivad ilmneda väljaspool seanss.

Selleks kasutage `engagement.agent.sendError` asemel `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Aruandlusteenuste tööde haldamine

Aruandlusteenuste töö kaanel ning töö sündmused ja jookseb teatamine.

**Näide:**

Kui soovite jälgida taotluse AJAXI, tuleks kasutada järgmist:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Aruannete tõrked töö käigus

Tõrgete võib olla seotud esitatava töö asemel praeguse kasutaja seansiga.

**Näide:**

Kui soovite tõrketeate, kui taotluse AJAXI nurjub.

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Sündmuste aruandlusteenuste töö käigus

Sündmuste võib olla seotud esitatava töö asemel kasutaja seansi tänu selle `engagement.agent.sendJobEvent` funktsioon.

See funktsioon töötab täpselt nagu `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Jookseb teatamine

Kasutage funktsiooni `sendCrash` funktsioon aruandele jookseb käsitsi.

Funktsiooni `crashid` argument on string, mis tuvastab tüüpi krahh.
Funktsiooni `crash` argumendi väärtus on tavaliselt virnas jälitus krahhi stringina.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Täiendav parameetrid

Saate manustada suvalise andmete sündmus, tõrge, tegevuse või töö.

Andmeid saab JSON objekti (kuid mitte massiivi või lihtsad tüüp).

**Näide:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Piirangud

Alade regulaaravaldised klahvid, väärtus tüübid ja suurus on piirangud, mis rakenduvad eest parameetrid.

#### <a name="keys"></a>Kiirklahvid

Iga objekti sisestage peab vastama tavalise järgmine avaldis:

    ^[a-zA-Z][a-zA-Z_0-9]*

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="values"></a>Väärtused

Väärtused on string, arv ja kahendmuutujaga tüübid.

#### <a name="size"></a>Suurus

Lisad piirduvad 1024 märki ühe kõne (pärast Mobile kaasamine Web SDK kodeeritakse see JSON sisse).

## <a name="reporting-application-information"></a>Teenuserakenduse teabe teatamine

Saate käsitsi teatada jälgimise teabe (või muu rakendusele vastav teave), kasutades funktsiooni `sendAppInfo()` funktsioon.

Pange tähele, et see teave saadetakse sammhaaval. Ainult teatud klahvi Viimane väärtus säilitatakse teatud seadme jaoks.

Sündmuse lisad, nagu saate kasutada mis tahes JSON objekti abstraktse teenuserakenduse teabe. Pange tähele, massiivid või alamtäpi objektide käsitletakse tasapinnalise stringide (kasutades JSON sariväljaanne).

**Näide:**

Siin on kasutaja sugu ja sünniaeg saatmise proovi kood:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Piirangud

Regulaaravaldised võtmed ja suurus alade on piirangud, mida rakendada teenuserakenduse teabe.

#### <a name="keys"></a>Kiirklahvid

Iga objekti sisestage peab vastama tavalise järgmine avaldis:

    ^[a-zA-Z][a-zA-Z_0-9]*

See tähendab, et klahvid peab algama vähemalt ühe tähe, millele järgneb tähtede, numbrite või allakriipsutatud märkideks (\_).

#### <a name="size"></a>Suurus

Rakenduse teave on kuni 1024 märki ühe kõne (pärast Mobile kaasamine Web SDK kodeeritakse see JSON sisse).

Eelmises näites, JSON, mis on saadetud server on 44 tähemärki.

    {"birthdate":"1983-12-07","gender":"female"}
