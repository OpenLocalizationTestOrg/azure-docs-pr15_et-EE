<properties
    pageTitle="Mitu platvormi teatised saata kasutajatele teatis jaoturi (ASP.NET)"
    description="Saate teada, kuidas ühe taotluse platvormi diagnostika teatis, et kõik platvormid eesmärgid saata teate jaoturi mallide kasutamise kohta."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Mitu platvormi teatised saata teate jaoturi kasutajad


Eelmise õpetuses [Teata kasutajad teatis jaoturi]soovite õpitut tõuketeatised kõigis seadmetes, mis on registreeritud mõne kindla autenditud kasutaja abil tehke järgmist. Selles õpetuses tuli mitu päringuid saata teate iga toetatud klientarvutite platvormi. Teatis jaoturi toetab malle, mis võimaldavad teil määrata, kuidas teatud seade soovib saada teatisi. See lihtsustab platvormidel teatiste saatmine. See teema näitab, kuidas saata ühe taotluse platvormi diagnostika teatise, et eesmärgid kõigi platvormide jaoks loodud mallide ära. Täpsemat teavet mallide kohta leiate teemast [Azure teatis jaoturi ülevaade][Templates].

> [AZURE.NOTE] Teatis jaoturi võimaldab seadme mitme malli registreerima sama silti. Sel juhul suunamise silti sissetuleva meilisõnumi tulemuseks seadmesse, üks iga malli jaoks on esitatud mitme teatised. See võimaldab teil sama sõnumi kuvamiseks mitme visuaalsete teatiste, näiteks nii nagu märgi ja töölauateatise Windowsi poe rakenduses.

Täitke järgmised juhised mallide kasutamine platvormidel teatiste saatmine:

1. Visual Studio Solution Exploreris laiendage **kontrollerid** kausta ja seejärel RegisterController.cs faili avada.

2. Leidke kood tekstiploki **postituse** meetod, mis loob uue registreerimise asendamine soovitud `switch` sisu järgmine kood:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Järgmine kood kõned platvormi kohased meetodi abil luua malli registreerimise kohalikke registreerimise asemel. Olemasoleva registreerimise ei saa muuta, kuna malli registreerimise tuletada kohalikke registreerimise.

3. **Teatiste** kontrolleril, Asendage **sendNotification** meetod järgmine kood:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Järgmine kood saadab isikule teate kõik platvormid samal ajal ja ilma kohalikke last. Teatise koostab ja pakub õige last väärtusega esitatud _sildi_ registreeritud Mallid määratletud iga seadmega.

4. Uuesti oma projekti avaldada, WebApi tagaandmebaas.

5. Käivitage rakendus kliendi uuesti ja veenduge, et registreerimine õnnestus.

6. (Valikuline) Kliendi rakenduse juurutamine teise seadmesse ja seejärel käivitage rakendus.

    Pange tähele, et igas seadmes kuvatakse teatis.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete täitnud selles õpetuses, lugege lisateavet teatis jaoturi ja mallid järgmisi teemasid:

+ **[Uudiseid saata teate jaoturi abil]** <br/>Näitab teise stsenaarium mallide kasutamine

+  **[Azure'i teatis jaoturi ülevaade][Templates]**<br/>Teema: ülevaade üksikasjalikku teavet mallide kohta.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Uudiseid saata teate jaoturi abil]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Teavitage kasutajaid teatis jaoturi abil]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
