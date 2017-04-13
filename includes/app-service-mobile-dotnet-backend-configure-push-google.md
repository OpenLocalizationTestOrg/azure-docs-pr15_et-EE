Kasutage toimingut, mis vastab teie taustväärtus projekti tüüp&mdash;kas [.NET taustväärtus](#dotnet) või [Node.js taustväärtus](#nodejs).

### <a name="dotnet"></a>.Net-i kirjutamata projekti

1. Visual Studios, paremklõpsake server project ja klõpsake nuppu **Halda Nugeti pakettide**, otsida `Microsoft.Azure.NotificationHubs`, klõpsake nuppu **Installi**. See installib teatis jaoturi kliendi teek.

2. Kontrollerid kausta, avage TodoItemController.cs ja lisage järgmine `using` laused:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Asendada selle `PostTodoItem` meetod järgmine kood:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Video uuesti avaldamiseks server project.

### <a name="nodejs"></a>Node.js kirjutamata projekti

1. Kui te pole seda veel teinud, [laadige Kiirjuhend projekti](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) või muu [online toimetaja Azure portaali](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)kasutamine.
 
1. Asendage kood todoitem.js faili järgmist:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    See saadab GCM teatise, mis sisaldab soovitud item.text, kui lisatakse uus todo üksus. 

2. Kohalikus arvutis faili redigeerimisel uuesti avaldada server project. 
