<properties
    pageTitle="Teatis jaoturi – Enterprise Push arhitektuur"
    description="Juhised ettevõttekeskkonnas Azure'i teatis jaoturi abil"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Ettevõtte tõuketeatised arhitektuuri juhised

Suurettevõtete täna liiguvad järk-järgult loomisel mobiilirakenduste Kas lõppkasutajad (väline) või (sise) töötajate jaoks. Olemasoleva taustväärtus süsteemid mainframes või mõned LoB rakendused, mis tuleb integreerida mobiilirakenduse arhitektuur on olemas. Sellest juhendist räägivad kuidas kõige paremini, tehke selle integreerimine soovitamine võimalik lahendus levinud stsenaariumi.

Tõuketeatised teatise saatmiseks kasutajad saavad mobiilirakenduse kaudu taustväärtus spikritest intressi sündmuse ilmnemisel on sagedased nõue. Nt Panga klient, kellel on Panga panga rakendus oma iPhone'is soovib kohaletoimetamisest deebet on muudetud teatud oma konto või kus esinejal eelarve kinnitamise rakendus oma Windows Phone rahandus töötaja soovib teada, kui ta saab taotluse kinnitamine sisevõrgu stsenaariumis kohal.

Pangakonto või kinnitamise töötlemine tõenäoliselt teha mõned taustväärtus süsteemi, mida peab kasutaja PTT. Võib olla mitu selliste kirjutamata süsteemide, mis tuleb kõik koostada sama liiki loogika tõuketeatised kasutusele võtta, kui sündmuse käivitab teatis. Siin keerukus seisneb integreerimise mitu kirjutamata süsteemid koos ühe tõuketeatised kus lõppkasutajatele on tellinud erinevate teatised ja seal võib olla isegi mitme mobiilirakendused, nt puhul sisevõrk mobiilirakendused kui ühe mobiilirakenduse võiksite mitme kirjutamata Systemsi teatisi. Taustväärtus süsteemid ei tea, või teadma tõuketeatised semantika/tehnoloogia nii levinud lahenduse siin traditsiooniliselt on osa, mis küsitlused kirjutamata süsteemide sündmused huvi ja vastutab tõuketeatised saatmine kliendile tutvustada.
Siin on rääkida veel paremaks lahenduse Azure'i teenus siini - teema/tellimus mudelit, mis vähendab keerukus tehes lahendus scalable abil.

Siin on lahendus üldine arhitektuur (generalized koos mitme mobiilirakendused, kuid kehtib, kui seal on ainult üks mobiilirakenduse)

## <a name="architecture"></a>Arhitektuur

![][1]

Võtme selle arhitektuuri diagrammi on Azure teenuse siini, mis pakub tellimuste teemade programmeerimisega mudeli (rohkem selle [Teenuse siini Pub/Sub]kavandamise). Vastuvõtja, mis on sellisel juhul mobiilsideseadmete kirjutamata (tavaliselt [Azure mobiilsideteenuse], mis alustab mobiilirakenduste PTT) ei saa otse taustväärtus süsteemide sõnumid, kuid selle asemel meil vahe võtmiseks kiht, mis esitatud [Siini Azure'i teenus] , mis võimaldab mobiilsideseadmete kirjutamata ühe või mitme taustväärtus süsteemi sõnumeid vastu. Teenuse siini teema tuleb iga taustväärtus süsteemide nt konto HR, Finance, mis on põhimõtteliselt "Teemad" mida alustab tõuketeatised teatis saadetavate sõnumite jaoks luua. Taustväärtus süsteemide saadab sõnumite järgmisi teemasid. Mobile Taustväärtus saate tellida ühe või mitme selliseid teemasid, luues teenuse siini tellimus. Annab õiguse mobiilsideseadmete taustväärtus vastavate taustväärtus süsteemist saada. Mobiilse taustväärtus endiselt Kuulake oma tellimuste sõnumeid ja kohe, kui saabub sõnum, see muutub uuesti ja saadab selle teate jaoturi teatis. Teatis jaoturi siis lõpuks pakub sõnumi mobiilirakenduse kaudu. Nii kokku põhikomponentide, meil on:

1. Kirjutamata (LoB/Pärandrakenduse süsteemide)
    - Loob teenuse siini teema
    - Saadab sõnumi
2. Mobiilse taustväärtus
    - Loob teenuse tellimus
    - Saab sõnumi (Taustväärtus süsteem)
    - Saadab teate kliendid (Azure'i teatis jaoturi) kaudu
3. Mobiilirakenduses
    - Saab ja Kuva teadet

###<a name="benefits"></a>Eelised:

1. Lahutamine vastuvõtja (rakenduse/mobiilsideteenuse teatis jaoturi kaudu) ja saatja (taustväärtus süsteemid) võimaldab täiendavad taustväärtus süsteemid on integreeritud minimaalsete muutmine.
2. See muudab ka mitme mobiilirakendused, on võimalik saada ühe või mitme taustväärtus süsteemide sündmused stsenaarium.  

## <a name="sample"></a>Näide:

###<a name="prerequisites"></a>Eeltingimused
Teil tuleb täita järgmised õppetükid kurssi viia põhimõtet kui ka levinud loomine ja konfigureerimine juhiseid:

1. [Teenuse siini Pub/Sub programmeerimisega] – seda selgitatakse töötab koos teenuse siini Teemad/tellimuste üksikasju nimeruumi sisaldama Teemad/tellimuste loomise kohta Kuidas saata ja neid sõnumeid saada.
2. [Teatis jaoturi - Windows universaalne õppeteema] – see selgitab, kuidas Windowsi poe rakenduse häälestamine ja kasutamine teatis jaoturi registreerida ja seejärel saada teatisi.

###<a name="sample-code"></a>Proovi kood

Täielik proovi kood on saadaval veebisaidil [Teatise jaoturi näidised]. See on jaotatud järgmisteks alljaotisteks kolmest osast.

1. **EnterprisePushBackendSystem**

    lisamine. Selle projekti kasutab *WindowsAzure.ServiceBus* Nugeti paketti ja [teenuse siini Pub/Sub programmeerimisega]põhjal.

    b. See on lihtne C# konsooli rakendus simuleerida LoB süsteemi, mis sõnumi toimetada mobiilirakenduse kaudu.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`saab luua teenuse siini teemat kus saadame sõnumeid.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`kasutatakse sõnumite saatmine teenuse siini teema. Siin lihtsalt saadame kogumi juhusliku sõnumite perioodiliselt eesmärgil valimi teema. Tavaliselt on taustväärtus süsteem, mis saadab sõnumeid, kui leiab aset sündmus.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    lisamine. Selle projekti kasutab *WindowsAzure.ServiceBus* ja *Microsoft.Web.WebJobs.Publish* Nugeti pakettide ja [teenuse siini Pub/Sub programmeerimisega]põhjal.

    b. See on teise C# konsooli rakendus mis me jooksma kui ka [Azure WebJob] , kuna see on pidevaks kuulata sõnumeid LoB/kirjutamata Systemsi. See on osa teie mobiilse taustväärtus.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`saab luua teenuse siini tellimuse teemat kus kirjutamata süsteemi sõnumeid saata. Sõltuvalt äri stsenaariumi, loob see osa ühe või mitme tellimuste vastavate teemade (nt mõned võib olla sõnumeid HR süsteemist, mõned rahandus süsteem ja jne)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification kasutatakse sõnumi lugemiseks teema, mida kasutate oma tellimust ja kui Loe õnnestus siis mobiilirakenduse abil Azure'i teatis jaoturi saadetakse käsitöö teatis (jaotises näidis stsenaarium Windows kohalikke töölauateatise).

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Avaldamiseks on **WebJob**nimega, lahenduse Visual Studios paremklõps ja valige **WebJob avaldamine**

    ![][2]

    f. Valige avaldamissaidi profiili ja Azure uue veebisaidi loomine, kui see pole juba olemas, mis on selles WebJob majutada ja kui olete veebisaidi seejärel **Avalda**.

    ![][3]

    g. Konfigureerige töö olema "Käivitada pidevalt" nii, et kui [Azure klassikaline portaali] sisse logima peaksite nägema umbes järgmine:

    ![][4]


3. **EnterprisePushMobileApp**

    lisamine. See on Windowsi poe rakendus, mis töölauateatisega teatisi WebJob, mis töötab teie mobiilse taustväärtus osana ja kuvage see. See põhineb [Teatis jaoturi - Windows universaalne õpetuse].  

    b. Kontrollige, kas teie taotlus on lubatud töölauateatisega teatisi.

    c. Veenduge, et alustada kood kannab nime juures rakenduse teatis järgmine jaoturiga (pärast *HubName* ja *DefaultListenSharedAccessSignature*asendamine:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Töötab valimi.

1. Veenduge, et teie WebJob töötab edukalt ja kavandatud "Käivitada pidevalt".
2. **EnterprisePushMobileApp** , hakkavad Windowsi poe rakenduse käivitamine
3. Käivita **EnterprisePushBackendSystem** konsooli rakendus, mis kuvatakse simuleerida LoB kirjutamata ja hakkavad sõnumite saatmiseks ja näete töölauateatisega teatised kuvataks umbes selline:

    ![][5]

4. Sõnumite algselt saadeti teenuse siini teemad, mis on kontrollib teenuse siini tellimused oma Web töö. Kui sõnumi vastu võttis, teavitus on loodud ja saadetud mobiilirakenduse kaudu. Saate vaadata WebJob logid töötlemise kinnitamiseks, kui lähete logid link [Azure'i klassikaline] portaalis oma Web töö kaudu.

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Teatis jaoturi näidised]: https://github.com/Azure/azure-notificationhubs-samples
[Azure'i mobiilsideteenus]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure'i teenus siini]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Teenuse siini Pub/Sub kavandamine]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure'i WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Teatis jaoturi - Windows universaalne õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
