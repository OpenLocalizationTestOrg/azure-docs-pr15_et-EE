<properties
    pageTitle="Azure'i Mobile kaasamine Web SDK versioonitäienduse toimingute | Microsoft Azure'i"
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


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure'i Mobile kaasamine Web SDK versioonitäienduse toimingute

Kui juba on integreeritud oma veebirakenduse Azure Mobile kaasamine Web SDK varasemas versioonis, peate SDK uuendamisel, võtke arvesse järgmist.

Kui te vahele mitme versiooni Mobile kaasamine Web SDK, peate võib mitu toiminguid teostada versiooniuuenduse installimise käigus. Näiteks kui migreerite 1.4.0 1.6.0, tehke esmalt täiendada 1.4.0 1.5.0 toiminguid. Seejärel järgige täiendada 1.5.0 1.6.0 toiminguid.

Sellest, millist versiooni täiendamist, asendage mis tahes azure-engagement.js faili varasema versiooni faili uusim versioon.

## <a name="upgrade-from-121-to-200"></a>1.2.1 2.0.0 uuendamine

Selles jaotises kirjeldatakse, kuidas migreerida Mobile kaasamine Web SDK integreerimine Capptain teenus, Capptain SASi rakendusele Azure Mobile allikaid. Kui migreerite varasemast versioonist, pöörduge Capptain veebisaidi 1.2.1 esmalt migreerida, ja seejärel rakendage järgmistest toimingutest.

Mobile kaasamine Web SDK see versioon ei toeta Samsung Nutiteleri, Opera TV, webOS või funktsiooni REACHi.

>[AZURE.IMPORTANT] Sama teenus ei ole Capptain ja Azure Mobile allikaid. Järgmise toimingu tõstetakse esile ainult kuidas migreerida kliendi rakendus. Migreerimine Mobile kaasamine Web SDK rakendus on siirata andmete Capptain serveri Mobile kaasamine serveris.

### <a name="javascript-files"></a>JavaScripti failide

Asendage fail capptain-sdk.js azure-engagement.js faili ja seejärel värskendage oma skripti impordi vastavalt sellele.

### <a name="remove-capptain-reach"></a>Capptain REACHi eemaldamine

Mobile kaasamine Web SDK see versioon ei toeta funktsiooni REACHi. Kui Capptain REACHi integreeritud rakenduse, peate selle eemaldada.

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

Teksti ja veebi teadaanded ja küsitluste saavutamiseks UI eemaldada. Vaikepaigutuse näeb välja umbes järgmine:

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

### <a name="remove-deprecated-apis"></a>API eemaldamine

Mõned Capptain API on aegunud Mobile kaasamine Web SDK.

Eemaldage kõik järgmised API kõned: `agent.connect`, `agent.disconnect`, `agent.pause`, ja `agent.sendMessageToDevice`.

Capptain konfiguratsioonile järgmist kontekstiatribuuti esinemisvõimaluse eemaldamine: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, ja `onPushMessageReceived`.

### <a name="configuration"></a>Konfigureerimine

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

Ühendusstringi rakenduse jaoks kuvatakse Azure'i portaal.

### <a name="javascript-apis"></a>JavaScripti API-d

Globaalne JavaScripti objekti `window.capptain` on ümber nimetatud `window.azureEngagement` , kuid saate kasutada funktsiooni `window.engagement` pseudonüüm API kõnede jaoks. Ei saa kasutada pseudonüüm määratleda SDK konfigureerimine.

Näiteks `capptain.deviceId` muutub `engagement.deviceId`, `capptain.agent.startActivity` muutub `engagement.agent.startActivity`, jne.
