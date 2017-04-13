Selles jaotises koodi värskendate olemasoleva mobiilirakenduste kirjutamata projekti saata tõuketeatised teatis iga kord, kui lisatakse uus üksus. See on tootja teatis jaoturi [malli](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funktsioon, mis on, mis võimaldab platvormidel sunnib. Erinevate kliendid on registreeritud Tõuketeatiste mallide kasutamise kohta ja ühe ühes kohas tõuketeatised pääsevad kõik kliendi platvormid.

Valige toiming, mis vastab teie taustväärtus projekti tüüp&mdash;kas [.NET taustväärtus](#dotnet) või [Node.js taustväärtus](#nodejs).

### <a name="dotnet"></a>.Net-i kirjutamata projekti
1. Visual Studios, paremklõpsake server project ja klõpsake nuppu **Halda Nugeti pakettide**, otsida `Microsoft.Azure.NotificationHubs`, klõpsake nuppu **Installi**. See installib teatis jaoturi teegi jaoks oma kirjutamata teatiste saatmine.

3. Avage Projectis serveri **kontrollerid** > **TodoItemController.cs**, ja lisage järgmine abil laused:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. Pärast kõne **InsertAsync**lisada **PostTodoItem** meetod järgmine kood:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    See saadab malli teate, mis sisaldab üksust. Tekst, kui lisatakse uus üksus.

4. Video uuesti avaldamiseks server project. 

### <a name="nodejs"></a>Node.js kirjutamata projekti

1. Kui te pole seda veel teinud, [laadige Kiirjuhend kirjutamata projekti](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) või muu [online toimetaja Azure portaali](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)kasutamine.

2. Asendage olemasolevad koodi todoitem.js järgmist:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    See saadab malli teate, mis sisaldab soovitud item.text, kui lisatakse uus üksus.

2. Kohalikus arvutis faili redigeerimisel uuesti avaldada server project.
