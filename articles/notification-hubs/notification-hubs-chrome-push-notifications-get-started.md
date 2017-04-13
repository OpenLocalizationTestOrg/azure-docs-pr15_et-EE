<properties
    pageTitle="Tõuketeatiste saatmine Chrome'i rakenduste Azure teatis jaoturi abil | Microsoft Azure'i"
    description="Saate teada, kuidas saatmine Tõuketeatiste Chrome'i rakendus Azure'i teatis jaoturi abil."
    services="notification-hubs"
    keywords="mobiililükketeatised Tõuketeatiste, lükake teatis, chrome tõuketeatised"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Tõuketeatiste saatmine Chrome'i rakenduste Azure teatis jaoturi abil

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Selles teemas näidatakse, kuidas saatmiseks tõuketeatisi rakendusse Chrome'i, mis kuvatakse Google Chrome'i brauseris kontekstis Azure'i teatis jaoturi abil. Selles õpetuses loome Chrome'i rakendus, mis saab Tõuketeatiste [Google Cloud sõnumside (GCM)](https://developers.google.com/cloud-messaging/)abil. 

>[AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Õpetuse juhendab teid Tõuketeatiste lubamiseks lihtsa järgmiselt:

* [Google Cloud sõnumside lubamine](#register)
* [Teie teate jaoturi konfigureerimine](#configure-hub)
* [Teate jaoturi Chrome'i rakenduse ühendamine](#connect-app)
* [Tõuketeatised teatise saata oma Chrome'i rakendus](#send)
* [Täiendavate funktsioonide ja võimaluste](#next-steps)

>[AZURE.NOTE] Chrome'i rakenduse tõuketeatised pole üldise-brauseri teatised – need on seotud brauseri laiendatavuse mudel (vt lisateavet [Chrome'i rakenduste ülevaade] ). Lisaks töölaua brauseri Chrome'i rakendusi käivitada mobile (Androidi ja iOS-i) Apache Cordova kaudu. Vaadake lisateavet [Chrome'i Mobile'i rakendused] .

Konfigureerida GCM ja Azure teatis jaoturi on identne konfigureerimise for Android, kuna aegunud [Google Chrome sõnumside pilveteenuste] ja sama GCM toetab nüüd nii Androidi seadmete ja Chrome'i eksemplarid.

##<a id="register"></a>Google Cloud sõnumside lubamine

1. Liikuge [Google Cloud konsooli] veebisaidile, logige sisse oma Google'i konto identimisteave ja seejärel klõpsake nuppu **Loo projekt** . Sisestage asjakohane **Projekti nime**ja seejärel klõpsake nuppu **Loo** .

    ![Google Cloud konsooli – projekti loomine][1]

2. Kirjutage **Projektinumber** **projektide** lehel äsja loodud projekti. Kasutate selle **GCM saatja ID** Chrome'i rakenduse GCM registreerida.

    ![Google Cloud konsool - projekti Number][2]

3. Klõpsake vasakpoolsel paanil **API-de & auth**, klõpsake nuppu ja seejärel kerige allapoole ja klõpsake tumblernupu lubamiseks **Google Cloud sõnumside Androidi jaoks**. Teil pole lubamiseks **Google Chrome sõnumside pilve**.

    ![Google Cloud konsool - Server võti][3]

4. Klõpsake vasakpoolsel paanil nuppu **mandaate** > **Luua uue tootenumbri** > **Serveri võti** > **loomine**.

    ![Google Cloud konsool - identimisteave][4]

5. Kirjutage serveri **API võti**. Tuleb konfigureerida see oma teatise keskuses järgmiseks Tõuketeatiste saatmiseks GCM lubamiseks.

    ![Google Cloud konsool - API võti][5]

##<a id="configure-hub"></a>Teie teate jaoturi konfigureerimine

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. tera **sätted** , valige **Teatise teenuste** ja seejärel **Google (GCM)**. Sisestage API võti ja salvestada.

&emsp;&emsp;![Azure'i teatis jaoturi - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Teate jaoturi Chrome'i rakenduse ühendamine

Teie teate jaoturi on nüüd konfigureeritud GCM töötamiseks ja teil on ühendusstringi oma rakenduse nii saatmiseks ja vastuvõtmiseks Tõuketeatiste registreerimiseks. LK

###<a name="create-a-new-chrome-app"></a>Uue Chrome'i rakenduse loomine

Allpool valimi [Chrome'i rakenduse GCM valimi] põhjal ja kasutab on soovitatav viis Chrome'i rakenduse loomine. Me esile seotud Azure'i teatis jaoturi juhiseid. 

>[AZURE.NOTE] Soovitame teil alla laadida allikas selle Chrome'i rakenduse kaudu [Chrome'i rakenduse teatise jaoturi valimi].

Chrome'i rakendus on loodud JavaScripti kaudu ja abil saate oma eelistatud Wordi redaktorid loomiseks seda. Allpool on see Chrome'i rakendus on näha.

![Google Chrome rakendus][15]

1. Looge kaust ja pange sellele `ChromePushApp`. Muidugi on nimi – kui te nimi on midagi muud, veenduge, et teil asendada tee sisse nõutud koodi lõigud.

2. Laadige alla [crypto-js teegi] kausta teise juhises loodud. See teek kaust sisaldab kahte alamkaustad: `components` ja `rollups`.

3. Luua mõne `manifest.json` faili. Kõik Chrome'i rakendused on tagatud manifestifaili, mis sisaldab rakenduse metaandmete ja kõige olulisem, mis on antud rakendus, kui kasutaja installib kõik õigused.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Teade kuvatakse `permissions` element, mis määrab, et see Chrome'i rakendus on võimalik Tõuketeatiste saadud GCM. Peate määrama ka Azure teatis jaoturi URI kui Chrome rakendus teeb ülejäänud kõne registreerida.
    Meie valimi rakendus kasutab ka ikooni faili, `gcm_128.png`, leiate allika, mis on algse GCM proovi uuesti kasutada. Saate asendada selle mis tahes pilt, mis sobib [ikooni kriteeriumid](https://developer.chrome.com/apps/manifest/icons).

4. Looge fail nimega `background.js` järgmine kood:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    See on fail, mille Chrome'i rakenduse aken HTML-i (**register.html**) ja ka määratleb sündmuseohjuri **messageReceived** käsitlema sissetulevaid tõuketeatised teatis.

5. Looge fail nimega `register.html` – see määratleb Chrome'i rakendus UI. 

   >[AZURE.NOTE] See näide kasutab **CryptoJS v3.1.2**. Kui laadisite teegi muu versiooni, veenduge, et teil asendada õigesti versiooni soovitud `src` tee.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Looge fail nimega `register.js` koodiga allpool. Selle faili saate määrata skripti taga `register.html`. Chrome'i rakendused ei võimalda Tekstisisese täitmise, nii, et peate looma oma UI eraldi varundamise skript.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Ülaltoodud skripti sisaldab järgmisi olulisi.
    - **Window.OnLoad** määratleb nupp – klõpsake nuppe sündmusi UI. Üks registreerib GCM ja teine kasutab tagastatakse pärast registreerimise GCM registreerima Azure'i teatis jaoturi registreerimise ID-d.
    - **updateLog** on funktsioon, mis võimaldab teil lihtne logimine võimaluste käsitlemiseks.
    - **registerWithGCM** on esimene nupp klõpsuga sündmuseohjuri, mis teeb selle `chrome.gcm.register` GCM praeguse Chrome'i rakenduse eksemplari registreerimiseks helistada.
    - **registerCallback** on tagasihelistamise funktsioon, mis käivitatakse GCM registreerimise kõne annab.
    - **registerWithNH** on teine nupp klõpsuga sündmuseohjuri, mis registreerib teatis jaoturi. Ta saab `hubName` ja `connectionString` (mida kasutaja määratud) ja käsitöö kõne teatis jaoturi registreerimise REST API-ga.
    - **splitConnectionString** ja **generateSaSToken** on abilised, mis tähistab JavaScripti rakendamiseks SaS Turbeloa loomise protsessi, mida tuleb kasutada kõnede REST API-ga. Lisateavet leiate teemast [Levinud põhimõtet](http://msdn.microsoft.com/library/dn495627.aspx).
    - **sendNHRegistrationRequest** on funktsioon, mis teeb HTTP ülejäänud, kõne Azure'i teatis jaoturi.
    - **registrationPayload** määratleb registreerimise XML-i last. Lisateavet leiate teemast [Loomine registreerimise NH REST API]. Me värskenduse registreerimise ID me saadud GCM.
    - **Kliendi** on **XMLHttpRequest** , mida me kasutame HTTP POST taotluse eksemplari. Pange tähele, et uuendame soovitud `Authorization` päise `sasToken`. Selle kõne edukaks registreeruma Azure'i teatis jaoturi Chrome'i rakenduse eksemplar.


Üldine kausta ülesehitust selle projekti tuleks näeb see:     ![Google Chrome'i rakendus – kausta struktuuri][21]

###<a name="set-up-and-test-your-chrome-app"></a>Chrome'i rakenduse seadistamine

1. Avage oma Chrome'i brauseris. Avage **Chrome'i laiendid** ja lubage **arendaja režiimis**.

    ![Google Chrome: luba arendaja režiimis][16]

2. Klõpsake nuppu **Laadi lahti laiend** ja liikuge kausta, kus loodud faile. Saate kasutada ka soovi korral kuvatakse **Chrome'i rakendused ja laiendid arendaja tööriist**. See tööriist on Chrome'i rakendus ise (installitud Chrome'i veebipoest) ja antakse täpsemalt silumine võimalused oma Chrome'i rakenduste arendamiseni.

    ![Google Chrome: Laadi lahti laiend][17]

3. Kui Chrome rakendus on loodud ilma vigu, siis kuvatakse teie Chrome'i rakendus kuvataks.

    ![Google Chrome: Chrome'i rakenduse kuvamine][18]

4. Sisestage **Projekti Number** , mida teil varem **Google Cloud konsooli** saatja ID ja klõpsake **GCM registreerida**. Kuvatakse teade **registreerimine GCM õnnestus.**

    ![Google Chrome: Chrome'i rakenduse kohandamine][19]

5. Sisestage oma **Teate jaoturi nimi** ja **DefaultListenSharedAccessSignature** saadud portaalist varem ja nuppu **Azure'i teate jaoturi registreerida**. Kuvatakse teade **teate jaoturi registreerimine eduka!** ja registreerimise vastuse, mis sisaldab Azure'i teatis jaoturi registreerimise üksikasju ID.

    ![Google Chrome: määrake teatise jaoturi üksikasjad][20]  

##<a name="send"></a>Teatise saata oma Chrome'i rakendus

Katsetamiseks, saadame Chrome'i tõuketeatised on .net-i abil konsooli rakendus. 

>[AZURE.NOTE] Tõuketeatiste teatis jaoturi abil saate saata mis tahes taustväärtus meie avaliku <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">ülejäänud kasutajaliidese</a>kaudu. Vaadake meie [dokumentatsiooni portaalis](https://azure.microsoft.com/documentation/services/notification-hubs/) veel mitu platvormi näiteid.

1. Visual Studio, klõpsake menüü **fail** nuppu **Uus** ja seejärel **projekti**. Jaotises **Visual C#**, klõpsake **Windowsi** ja **Konsooli rakendus**ja seejärel klõpsake nuppu **OK**.  See loob uue konsooli rakendus projekti.

2. Klõpsake menüü **Tööriistad** nuppu **Teek paketi haldur** ja seejärel **Package Manager konsooli**. Kuvatakse paketi Manager konsooli.

3. Konsooli aknas, käivitage järgmine käsk:

        Install-Package Microsoft.Azure.NotificationHubs

    Sellega lisatakse Azure'i teenus siini SDK <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus Nugeti pakett</a>viide.

4. Avatud `Program.cs` ja lisage järgmine `using` lause:

        using Microsoft.Azure.NotificationHubs;

5. Klõpsake soovitud `Program` klassi, lisada järgmisel viisil:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Veenduge, et asendada selle `<hub name>` Kohatäite nimi, mis kuvatakse teie teate jaoturi tera [portaali](https://portal.azure.com) teate jaoturi abil. Ühendusstringi nimetatakse ka ühenduse stringi kohatäite asendamiseks `DefaultFullSharedAccessSignature` teatis jaoturi konfigureerimine jaotises saadava.

    >[AZURE.NOTE] Veenduge, et ühendusstringi abil **täielik** juurdepääs **kuulata** juurdepääsu. **Kuulake** Accessi Ühendusstring ei anna õigused Tõuketeatiste saata.

5. Lisage järgmised kõned on `Main` meetod:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Veenduge, et töötab Chrome ja käivitage konsooli rakendus.

7. Teie töölaual, peaksite nägema järgmine teade ilmub.

    ![Google Chrome: teatis][13]

8. Võite vaadata ka kõik teie teatised Chrome'i teatised akna tegumiribal abil (Windowsis) kui Chrome on installitud.

    ![Google Chrome: teatiste loend][14]

>[AZURE.NOTE] Peate ei on Chrome'i rakendus töötab või avamine brauseris (kuigi ise Chrome'i brauseris peate kasutama). Lisaks saate kõik teie teatised kokkuvõtlik aknas Chrome'i teatised.

## <a name="next-steps"> </a>Järgmised sammud

Lisateavet teate jaoturi [teatis jaoturi]ülevaade.

Teatud kasutajate suunata viidata õpetuse [Azure'i teatis jaoturi Teavitage kasutajaid] . 

Kui soovite segmendi kasutajate poolt huvirühmade, võite järgida [Azure'i teatis jaoturi katkestamine uudiste] õpetuse.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome'i rakenduse teatise jaoturi näidis]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud konsooli]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Teatis jaoturi ülevaade]: notification-hubs-push-notification-overview.md
[Chrome'i rakenduste ülevaade]: https://developer.chrome.com/apps/about_apps
[Chrome'i rakenduse GCM näidis]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome Mobile'i rakendused]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Registreerimise NH REST API loomine]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[Crypto-js teek]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Chrome sõnumside]: https://developer.chrome.com/apps/cloudMessagingV1
[Teavitage kasutajaid, Azure teatis jaoturi]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure'i teatis jaoturi uudiseid]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
