<properties
    pageTitle="Tõuketeatiste lisamine rakenduse Universal platvormil (UWP) | Azure'i mobiilirakendused"
    description="Saate teada, kuidas Azure'i rakenduse teenuse mobiilirakenduste ja Azure teatis jaoturi saatmine Tõuketeatiste Universal platvormil (UWP) rakenduse abil."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Tõuketeatiste teie Windowsi rakenduse lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Ülevaade

Selles õpetuses lisate Tõuketeatiste [Windowsi quick start](app-service-mobile-windows-store-dotnet-get-started.md) projekt, et tõuketeatised teatis saadetakse seade iga kord, kui kirje lisamisel.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate tõuketeatised teatis laiend paketi. Lisateavet leiate [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Teate jaoturi konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Teie minirakendus Tõuketeatiste registreerimine

Esitage oma Windowsi poe rakendus ja seejärel konfigureerida server projekti integreerida koos Windowsi teatise Services (WNS) tõuketeatised saatmiseks peate.

1. Visual Studio lahenduste Explorer, paremklõpsake UWP rakenduse project, klõpsake **poe** > **... Poe rakendus seostada**. 

    ![Sidusettevõtte Windowsi poe rakendus](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. Viisardis **järgmise**, logige sisse oma Microsofti kontoga sisse, sisestage oma rakenduse nimi reservi **uue rakenduse nime**, klõpsake käsku **reservi**.

3. Pärast rakenduse registreerimine on loodud, valige uue rakenduse nime, klõpsake nuppu **Järgmine**ja klõpsake **seostada**. Sellega lisatakse nõutavad andmed Windowsi poe rakenduse manifesti.  

7. Avage [Windows Arenduskeskus](https://dev.windows.com/en-us/overview)logige sisse oma Microsofti kontoga, klõpsake nuppu **minu rakendused**rakendus registreerimine ja seejärel laiendamine **teenuste** > **tõuketeatised**. 

8. Klõpsake lehe **tõuketeatised** jaotises **Microsoft Azure Mobile teenused** **Live'i sait** .

9. Registreerimise leht, märkige jaotises **rakenduse saladusi** ja **Paketi SID**, mida kasutate edasi oma mobiilirakenduse kirjutamata konfigureerimiseks väärtuse. 

    ![Sidusettevõtte Windowsi poe rakendus](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Kliendi salajane ja paketi SID on oluline turvalisuse mandaat. Need väärtused teistega või levitada neid oma rakendusega. **Rakenduste ID-d** kasutatakse koos salajane Microsofti Account autentimise konfigureerimine.

##<a name="configure-the-backend-to-send-push-notifications"></a>Taustväärtus saata Tõuketeatiste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Värskenduse saatmiseks tõuketeatisi server

Kasutage toimingut, mis vastab teie taustväärtus projekti tüüp&mdash;kas [.NET taustväärtus](#dotnet) või [Node.js taustväärtus](#nodejs).

### <a name="dotnet"></a>.NET taustväärtus projekti

1. Visual Studio, paremklõpsake server project ja **Hallata Nugeti pakettide**, otsida Microsoft.Azure.NotificationHubs, klõpsake käsku **installida**. See installib teatis jaoturi kliendi teek.

2. Laiendage **kontrollerid**, avage TodoItemController.cs ja lisage järgmine laused abil:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. Pärast kõne **InsertAsync**lisada **PostTodoItem** meetod järgmine kood:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Järgmine kood ütleb teate jaoturi tõuketeatised teatise saatmine pärast uue üksuse lisamine.

4. Video uuesti avaldamiseks server project.

### <a name="nodejs"></a>Node.js kirjutamata projekti

1. Kui te pole seda veel teinud, [laadige Kiirjuhend projekti](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) või muu [online toimetaja Azure portaali](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)kasutamine.

2. Asendage kood todoitem.js faili järgmist:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    See saadab WNS töölauateatise, mis sisaldab soovitud item.text kui lisatakse uus todo üksus.

2. Kohalikus arvutis faili redigeerimisel uuesti avaldada server project.

##<a id="update-app"></a>Tõuketeatiste lisamiseks rakenduse

Järgmiseks peate rakenduse Registreeruge Tõuketeatiste käivitamisel. Kui teil on juba lubatud autentimine, veenduge, et kasutaja märgid sisse enne Tõuketeatiste registreeruda.

1. Avage fail **App.xaml.cs** projekti ja lisage järgmine `using` laused:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Sama faili lisada järgmised **InitNotificationsAsync** meetodi määratluse **rakenduse** klassi:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Järgmine kood toob ChannelURI rakenduse WNS ja seejärel registreerib selle ChannelURI oma rakenduse mobiilirakenduse.

3. **OnLaunched** sündmuseohjuri **App.xaml.cs**ülaosas **asünkroonse** muutja lisamine meetod määratluse ja lisada järgmise kõne uus **InitNotificationsAsync** meetod, nagu järgmises näites:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    See tagab, et lühiajaline ChannelURI registreeritakse iga kord, kui rakendus on käivitatud.

4. Projekti UWP rakenduse taastada. Teie rakendus on nüüd valmis töölauateatisega teatisi.

##<a id="test"></a>Rakenduse testi tõuketeatised

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Järgmised sammud

Lugege lisateavet Tõuketeatiste:

* [Kuidas kasutada hallatavate kliendi Azure'i mobiilirakenduste kohta](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Mallide abil paindlikkust platvormidel sunnib ja lokaliseeritud sunnib saata. Saate teada, kuidas registreerida mallid.

* [Tõuketeatised teatis probleemide](../notification-hubs/notification-hubs-push-notification-fixer.md)  
On erinevad põhjused, miks teatised võib saada lähevad või ei jõua seadmetes. Selles teemas näidatakse, kuidas analüüsida ja aru saada põhjuseks tõuketeatised teatis ebaõnnestumist. 

On soovitatav jätkuva üks järgmistest õpetused edasi.

+ [Rakenduse lisamiseks autentimine](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Saate teada, kuidas kasutajad oma rakenduse pakkujaga identiteedi kinnitamiseks.

+ [Rakenduse ühenduseta sünkroonimise lubamine](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil ka Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

