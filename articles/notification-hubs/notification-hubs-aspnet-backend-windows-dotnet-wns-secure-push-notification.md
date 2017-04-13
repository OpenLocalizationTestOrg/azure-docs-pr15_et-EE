<properties
    pageTitle="Azure'i teatis jaoturi turvaline tõuketeatised"
    description="Saate teada, kuidas saada turvaline Tõuketeatiste Azure. Koodinäiteid kirjutatud C# .net-i API abil."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure'i teatis jaoturi turvaline tõuketeatised

> [AZURE.SELECTOR]
- [Windowsi universaalne](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS-i](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Ülevaade

Tõuketeatised teatis tugi Microsoft Azure võimaldab teil lihtne kasutada, paljudele, mastaabitud kontorist tõuketeatised taristu, mis lihtsustab rakendamise Tõuketeatiste tarbija ja ettevõtte puhul mobiilside kaudu juurdepääsu.

Tõttu nõuetele või turvalisus piiranguid, mõnikord rakenduse võib soovite kaasata midagi teate, et ei saa edastada standard tõuketeatised teatis taristu kaudu. Selles õppetükis kirjeldatakse, kuidas saavutada sama kogemus saatmisega tundliku teabe turvaline, autenditud kliendi seade ja rakenduse taustväärtus vahelise ühenduse kaudu.

Kõrge, voo on järgmine:

1. Rakenduse tagaandmebaas:
    - Salvestab turvaline last Tagaandmebaasi.
    - Saadab teate ID (secure teavet saadetakse) seade.
2. Rakendus töötab seadme taustal, kui nad saavad teate:
    - Seadme kontaktide paluda turvaline last tagaandmebaas.
    - Rakenduse saate kuvada soovitud last kujul seadmesse teatis.

See on oluline märkida, et eelmise voogu (ja selle õpetuse) me oletame, et seade talletab mõne autentimise luba kohalike salvestusruumi, pärast kasutaja logib sisse. See tagab täielikult tõrgeteta, kui seadme saate tuua teate turvaline last selle märgiks abil. Kui rakenduse salvestada seadme autentimine märkide või kui nende tähiste saate aegunud, seadme rakendus, teate saamisel peaks olema kuvatud üldise teatise viipa kasutaja käivitage rakendus. Rakenduse siis autendib kasutaja ja kuvab teate last.

Selle õpetuse Secure tõuketeatised näitab, kuidas turvaliselt tõuketeatised teatise saatmine. [Teavitage kasutajaid](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) õpetuse, et teil tuleb täita juhised selle õpetuse esmalt õpetus tugineb.

> [AZURE.NOTE] Selle õpetuse eeldab, et olete loonud ja oma teate jaoturi konfigureeritud, nagu on kirjeldatud [Alustamine teatis jaoturi (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Samuti, võtke arvesse, et Windows Phone 8.1 jaoks on vaja Windows (mitte Windows Phone) identimisteabe ja tausttoimingud ei tööta Windows Phone 8.0 või Silverlighti 8.1. Windowsi poe rakendused, saate tööülesande tausta kaudu ainult siis, kui rakendus töötab lukustuskuva lubatud (klõpsake märkeruut selle Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Windows Phone projekti muutmine

1. Projectis **NotifyUserWindowsPhone** lisada järgmine kood App.xaml.cs tõuketeatised tausta tööülesande registreerimiseks. Lisage järgmine rida koodi lõpus olevat `OnLaunched()` meetod:

        RegisterBackgroundTask();

2. Veel App.xaml.cs, lisage järgmine kood kohe pärast selle `OnLaunched()` meetod:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Lisage järgmine `using` laused App.xaml.cs faili ülaosas:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Visual Studio menüü **fail** käsku **Salvesta kõik**.

## <a name="create-the-push-background-component"></a>Tõuketeatised tausta komponent loomine

Järgmiseks on luua tõuketeatised tausta komponent.

1. Solution Exploreris paremklõpsake ülataseme sõlm lahenduse (sel juhul**lahenduse SecurePush** ), seejärel klõpsake nuppu **Lisa**ja klõpsake valikut **Uus projekt**.

2. Laiendage **Poe rakendused**, siis **Windows Phone rakendused**, siis **Windows Runtime komponent (Windows Phone)**nuppu. Projekti **PushBackgroundComponent**nimi ja seejärel klõpsake nuppu **OK** , et luua projekt.

    ![][12]

3. Solution Exploreris Paremklõpsake **PushBackgroundComponent (Windows Phone 8.1)** projekti, siis klõpsake nuppu **Lisa**, siis klõpsake **Class**. Selle uue ainekursuse **PushBackgroundTask.cs**nimi. Klõpsake nuppu **Lisa** ainekursuse loomiseks.

4. Asendage **PushBackgroundComponent** nimeruumi määratlus kogu sisu järgmine kood, asendades kohatäite `{back-end endpoint}` koos oma tagaandmebaas juurutamisel saadud tagaandmebaas lõpp-punkti:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. Solution Exploreris paremklõpsake projekti **PushBackgroundComponent (Windows Phone 8.1)** ja seejärel klõpsake **Haldamine NuGet-paketid**.

6. Klõpsake vasakus servas **Online**.

7. Tippige **otsinguväljale** **Http kliendi**.

8. Otsingutulemite loendis, klõpsake **Microsoft HTTP kliendi teekide**ja klõpsake nuppu **Installi**. Installimise lõpuleviimiseks.

9. Tagasi Nugeti **Otsing** , tippige väljale **Json.net**. Installige **Json.NET** pakett ja seejärel sulgege aken Nugeti Package Manager.

10. Lisage järgmine `using` laused **PushBackgroundTask.cs** faili ülaosas:

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Solution Exploreris Projectis **NotifyUserWindowsPhone (Windows Phone 8.1)** Paremklõpsake **Viited**ja seejärel klõpsake nuppu **Lisada viide**. Viide Tabelihalduri dialoog, märkige **PushBackgroundComponent**kõrval olev ruut ja seejärel klõpsake nuppu **OK**.

12. Topeltklõpsake Solution Exploreris **Package.appxmanifest** **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt. Jaotises **teatiste**määramine **Töölauateatisega toega** **Jah**.

    ![][3]

13. Veel **Package.appxmanifest**, klõpsake lehe ülaservas **deklaratsiooni** menüüd. **Saadaval deklaratsiooni** rippmenüü, klõpsake nuppu **Tausttoimingud**ja seejärel klõpsake nuppu **Lisa**.

14. **Package.appxmanifest**, klõpsake jaotises **Atribuudid**ruut **Push teatis**.

15. Tippige **PushBackgroundComponent.PushBackgroundTask** **Sisendpunkti** väli **Package.appxmanifest**, klõpsake jaotises **Rakenduse sätted**.

    ![][13]

16. Klõpsake menüü **fail** nuppu **Salvesta kõik**.

## <a name="run-the-application"></a>Käivitage rakendus

Rakenduse käivitamiseks tehke järgmist.

1. Visual Studio, käivitage rakendus **AppBackend** Web API-ga. ASP.net-i veebileht, mis kuvatakse.

2. Visual Studio, käivitage rakendus Windows Phone **NotifyUserWindowsPhone (Windows Phone 8.1)** . Windows Phone emulaator töötab ja laadib rakendus automaatselt.

3. **NotifyUserWindowsPhone** rakenduse UI, sisestage kasutajanimi ja parool. Need võivad olla mis tahes stringi, kuid need peavad olema sama väärtuse.

4. **NotifyUserWindowsPhone** rakenduse UI, klõpsake **sisse logida ja registreerimist**. Klõpsake nuppu **saada tõuketeatised**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
