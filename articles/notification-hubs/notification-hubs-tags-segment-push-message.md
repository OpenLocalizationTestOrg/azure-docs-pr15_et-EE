<properties
    pageTitle="Marsruutimine ja sildi avaldised"
    description="See teema selgitab marsruutimine ja sildi avaldiste Azure teatis jaoturi."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Marsruutimine ja sildi avaldised

##<a name="overview"></a>Ülevaade

Sildi avaldiste võimaldavad teil target teatud komplekti seadmete või täpsemalt registreerimine, saates tõuketeatised teatis teatis jaoturi kaudu.


## <a name="targeting-specific-registrations"></a>Suunamise teatud registreerimine

Sihtrakenduse ainus võimalus teatud teatise registreerimise on sildid seostada, siis suunata need sildid. [Registreerimise juhtimine](notification-hubs-push-notification-registration-management.md)kasutajaprofiiliteabe saamiseks Tõuketeatiste rakendus on seadme pide teatis jaoturi registreerimiseks. Registreerimise loodud teate jaoturi rakenduse kirjutamata saate saata Tõuketeatiste seda.
Rakenduse kirjutamata saate valida sihtkohta teatud teatis koos registreerimise järgmisel viisil:

1. **Leviedastus**: registreerumine teate jaoturi vastu teatis.
2. **Sildi**: registreerumine, mis sisaldavad määratud silt vastu teatis.
3. **Sildi avaldise**: registreerumine, mille kogumi sildid vastavad määratud avaldise vastu teatis.

## <a name="tags"></a>Sildid

Sildi võib olla mis tahes string, kuni 120 märki, mis sisaldab tähtedest ja numbritest koosnev ja mitte-tähtnumbrilisi järgmisi märke: '_' ‘@’, "#" ".",":", "-". Järgmises näites on kujutatud rakendus, kust saate töölauateatis teatised teatud muusika rühmade kohta. Selle stsenaariumi korral on lihtne viis marsruutimiseks teatised sildi registreerimise siltidega, mis tähistavad erinevate lint, nagu järgmisel pildil.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Selle pildi sõnumi sildistatud **Beatles** jõuab ainult tahvelarvuti sildiga **Beatles**registreeritud.

Registreerimise siltide loomise kohta leiate lisateavet teemast [Registreerimise juhtimine](notification-hubs-push-notification-registration-management.md).

Saate saata teatised siltide abil soovitud saada teatisi selle `Microsoft.Azure.NotificationHubs.NotificationHubClient` klassi [Microsoft Azure'i teatis jaoturi](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Samuti saate Node.js või Push teatised REST API-d.  Siin on näide SDK abil.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Siltide ei pea olema eelnevalt ettevalmistatud ja võib viidata mitmele rakendusega seotud mõisteid. Näiteks selle näite rakenduse kasutajad saavad kommenteerida lint ja soovite tema sõprade, olenemata riba, kus need kommenteerimise saadud röstsaiaga, mitte ainult oma lemmik lint kommentaarid, vaid ka kõik kommentaarid. Järgmisel pildil on kujutatud selle stsenaariumi:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Järgmisel pildil Alice on huvi Beatles värskendusi ja Bob on huvi selle Wailers värskendusi. Bob on ka huvitatud Charlie kommentaarid ja Charlie on huvitatud selle Wailers. Kui Charlie kommenteerida Beatles saadetakse teatis, nii Alice ja Bob saada.

Ajal saate kodeerida mitme probleemid silte (nt "band_Beatles" või "follows_Charlie"), sildid on lihtne stringide ja pole atribuutide väärtustega. Registreerimise sobib ainult teatud sildi või kohta.

Kuidas kasutada täielikku üksikasjaliku juhendi siltide saatmiseks huvirühmade, leiate [Katkestamine uudiste](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Target (sihtkoht) kasutajatele siltide kasutamine

Teine võimalus siltide kasutamine on kõigis seadmetes, teatud kasutaja tuvastamiseks. Registreerimise saate sildistatud silt, mis sisaldab kasutaja id, nagu järgmisel pildil:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Järgmisel pildil sõnumi sildistatud uid:Alice jõuab kõik registreerimise sildistatud uid:Alice; Seega kõik Alice'i seadmed.


##<a name="tag-expressions"></a>Sildi avaldised

On juhtumeid, kus on teatise registreerimise komplekt, mis on ühe sildi, vaid loogikaavaldis sildid suunata.

Kaaluge spordialad rakendus, mis saadab meeldetuletuse kõik Boston Red Sox ja Cardinals vahel mängu kohta. Kui kliendi rakendus registreerib siltide kohta huvi meeskondadel ja asukohta, siis teate tuleks suunata kõigile Bostonis, kes on punane Sox või Cardinals huvi. See tingimus saab väljendada järgmised loogikaavaldis abil:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Sildi avaldiste võib sisaldada kõigi toetatud brauserid, AND (& &), või (|) ja mitte (!). Need võivad sisaldada ka sulud. Sildi avaldised on piiratud 20 sildid, kui need sisaldavad ainult ORs; muul juhul nad on piiratud 6 sildid.

Siin on näide saatmise teatised sildi avaldistega SDK abil.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
