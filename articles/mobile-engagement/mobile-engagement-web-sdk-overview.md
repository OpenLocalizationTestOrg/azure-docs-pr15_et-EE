<properties
    pageTitle="Azure'i mobiilsideseadmete kaasamine Web SDK ülevaade | Microsoft Azure'i"
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
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure'i mobiilsideseadmete kaasamine Web SDK

Kuidas integreerida Azure Mobile kaasamine web Appis üksikasjade muutmiseks alustage siit. Kui soovite seda proovida enne oma web app töötamise alustamine, vaadake meie [15-minutiline õpetuse](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Toimingute integreerimine
1. Saate teada, [Kuidas integreerida Mobile kaasamine oma web Appis](mobile-engagement-web-integrate-engagement.md).

2. Sildi lepingu rakendamiseks saate teada, [Kuidas kasutada täiustatud Mobile kaasamine sildistamine API teie web Appis](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Väljalaskemärkmed

### <a name="202-10182016"></a>2.0.2 (2016/10/18)

-   Fikseeritud krahhi privaatne sirvimine (Safari).
-   Fikseeritud krahh brauserites küpsiste keelatud.

Kõik versioonid, vt [täielik väljalaskemärkmed](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Toimingute täiendamine

### <a name="upgrade-from-121-to-200"></a>1.2.1 2.0.0 uuendamine

Järgmistes jaotistes kirjeldatakse, kuidas migreerida Mobile kaasamine Web SDK integreerimine Capptain teenus, Capptain SASi rakendusele Azure Mobile allikaid. Kui on varem 1.2.1 migreerimine versioonist, pöörduge Capptain veebisaidi esmalt 1.2.1 migreerida, ja seejärel rakendage järgmistest toimingutest.

Mobile kaasamine Web SDK see versioon ei toeta Samsung Nutiteleri, Opera TV, webOS või funktsiooni REACHi.

>[AZURE.IMPORTANT] Capptain ja Azure Mobile kaasamine pole sama teenuse ja järgmistest toimingutest esile tõsta ainult kuidas migreerida kliendi rakendus. Migreerimine Mobile kaasamine Web SDK rakendus on siirata andmete Capptain serveri Mobile kaasamine serveris.

#### <a name="javascript-files"></a>JavaScripti failide

Asendage fail capptain-sdk.js azure-engagement.js faili ja seejärel värskendage oma skripti impordi vastavalt sellele.

#### <a name="remove-capptain-reach"></a>Capptain REACHi eemaldamine

Mobile kaasamine Web SDK see versioon ei toeta funktsiooni REACHi. Kui Capptain REACHi on integreeritud rakenduse, peate selle eemaldada.

Saavutamiseks CSS-i importimine lehelt eemaldada ja kustutada seotud CSS-faili (capptain-reach.css, vaikimisi).

Kustutage REACHi järgmistest allikatest: Sule pilt (capptain-close.png, vaikimisi) ja kaubamärgi ikooni (capptain--teavitusikooni, vaikimisi).

Eemaldage saavutamiseks UI-rakenduse teatiste saamiseks. Vaikepaigutus näeb välja umbes järgmine:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Teksti ja veebi teadaanded ja küsitluste saavutamiseks UI eemaldada. Vaikepaigutus näeb välja umbes järgmine:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Eemaldamine on `reach` objekti oma konfiguratsiooni, kui see on olemas. See näeb välja umbes järgmine:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Eemaldage kõik muud REACHi kohandamine, nagu näiteks kategooriate.

#### <a name="remove-deprecated-apis"></a>API eemaldamine

Mõned Capptain API on aegunud Mobile kaasamine Web SDK.

Eemaldage kõik järgmised API kõned: `agent.connect`, `agent.disconnect`, `agent.pause`, ja `agent.sendMessageToDevice`.

Mõni järgmised kontekstiatribuuti eemaldamine konfiguratsioonist Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, ja `onPushMessageReceived`.

#### <a name="configuration"></a>Konfigureerimine

Mobile kaasamine kasutab ühendusstringi SDK identifikaatorite, näiteks rakenduse identifikaator konfigureerimiseks.

Asendage rakenduse ID oma ühendusstring. Pange tähele, et globaalne objekti SDK konfiguratsiooni vahetatakse `capptain` et `azureEngagement`.

Enne migreerimise:

    window.capptain = {
      appId: ...,
      [...]
    };

Pärast migreerimist:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Ühendusstringi rakenduse jaoks kuvatakse Azure'i portaalis.

#### <a name="javascript-apis"></a>JavaScripti API-d

Globaalne JavaScripti objekti `window.capptain` on ümber nimetatud `window.azureEngagement`, kuid saate kasutada funktsiooni `window.engagement` pseudonüüm API kõnede jaoks. Ei saa kasutada seda alias määratleda SDK konfigureerimine.

Näiteks `capptain.deviceId` muutub `engagement.deviceId`, `capptain.agent.startActivity` muutub `engagement.agent.startActivity`, jne.

Kui juba on integreeritud rakenduse Azure Mobile kaasamine Web SDK varasemas versioonis, lugege [täiendamine toimingute](mobile-engagement-web-upgrade-procedure.md)kohta.
