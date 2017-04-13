<properties
    pageTitle="Azure'i asjade jaotis C# alustamine | Microsoft Azure'i"
    description="Azure'i asjade jaoturi C# saada alustamine õpetuse. Azure'i asjade jaoturi ja C# asjade Interneti lahenduse kasutusele Microsoft Azure'i asjade SDK-d kasutada."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Azure'i asjade jaoturi .NET kasutamise alustamine

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Õppeteema lõpus on teil kolm Windows konsooli rakendusi.

* **CreateDeviceIdentity**, mis loob seadme identiteedi ja nendega seotud võti jäljendatud videoseadme ühendamiseks.
* **ReadDeviceToCloudMessages**, mis kuvab telemeetria jäljendatud seadmest.
* **SimulatedDevice**, mis ühendab oma asjade jaoturi varem loodud seadme identiteediga ja saadab sõnumi telemeetria iga teine AMQP protokolli abil.

> [AZURE.NOTE] On erinevate SDK-d, mille abil saate luua nii rakendusi käivitada seadmed ja teie lahendus tagasi end kohta leiate teavet teemast [Asjade jaoturi SDK-d][lnk-hub-sdks].

Selle õpetuse tegemiseks vajate järgmist:

+ Microsoft Visual Studio 2015.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nüüd olete loonud oma asjade jaoturi ja teil on selle õpetuse ülejäänud soovitud hostname ja ühenduse string.

## <a name="create-a-device-identity"></a>Seadme identiteedi loomine

Selles jaotises saate luua Windows konsooli rakendus, mis loob teie asjade keskuses registris identiteedi seadme identiteedi. Seade ei saa ühendust asjade jaoturi juhul, kui kirje on registris seadme identiteedi. Leiate lisateavet jaotisest "Seadme identiteedi registri" [Asjade jaoturi arendaja juhend][lnk-devguide-identity]. Selle konsooli rakenduse käivitamisel see tekitab kordumatu seadme ID ja seadme abil saate ise kindlaks teha, kui seadme pilve sõnumid saadetakse asjade jaoturi võti.

1. Visual Studio, lisada Visual C# Windowsi klassikaline töölaua projekti praegune lahendus **Konsooli rakendus** projekti malli abil. Veenduge, et .NET Framework versioon on 4.5.1 või uuem versioon. Projekti **CreateDeviceIdentity**nimi.

    ![Visual C# Windowsi klassikaline töölaua uue projekti][10]

2. Solution Exploreris Paremklõpsake **CreateDeviceIdentity** projekti ja seejärel klõpsake nuppu **Halda Nuget-paketid**.

3. **Nugeti Package Manager** aknas valige **Sirvi**, otsige **microsoft.azure.devices**, **installida** **Microsoft.Azure.Devices** paketi installimiseks valige ja nõustuge kasutustingimustega. Selle toimingu allalaadimist, installib ja lisab [Microsoft Azure'i asjade teenuse SDK] viide[ lnk-nuget-service-sdk] Nugeti pakett ja sõltuvustega.

    ![Nugeti Package Manager aken][11]

4. Lisage järgmine `using` laused **Program.cs** faili ülaosas:

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Saate lisada **programmi** klassi järgmised väljad. Asendage kohatäite väärtuse jaoks asjade jaoturi eelmises jaotises loodud ühendusstring.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Saate lisada **programmi** klassi järgmisel viisil:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    See meetod loob seadme identiteedi ID **myFirstDevice**. (Kui selle seadme ID juba olemas registrisse, koodi lihtsalt toob olemasoleva seadme teabe.) Seejärel kuvatakse rakenduse selle primaarvõti. Kasutate selle klahvi jäljendatud seadme ühenduse loomiseks oma asjade jaoturi.

7. Lõpuks lisage järgmised read **Esilehele** meetod.

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Seda rakendust käivitama ja seadme võtit üles.

    ![Rakenduse genereeritud seadme võtit][12]

> [AZURE.NOTE] Asjade jaoturi identiteedi registri salvestab ainult seadme identiteedid lubamiseks jaoturi turvaline juurdepääs. See talletab seadme ID-d ja klahvid, mida kasutada turvalisus mandaadid ja lubatud või keelatud lippu, mille abil saate üksikuid seadmes juurdepääsu keelamiseks. Kui rakenduse peab muu seadme metaandmete talletada, kasutada selle poest rakenduse kohased. Lisateabe saamiseks lugege teemat [Asjade jaoturi arendaja juhend][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Seadme pilve sõnumeid

Selles jaotises saate luua Windows konsooli rakendus, mis loeb seadme pilve sõnumite asjade keskuse kaudu. Soovitud asjade jaoturi seab on [Azure sündmuse jaoturi][lnk-event-hubs-overview]-ühilduv lõpp-punkti selleks, et saaksite seadme pilve sõnumeid lugeda. Selles õpetuses hoida asju on lihtne, loob lihtsa lugeja, mis ei sobi kõrge läbilaskevõime juurutamine. Seadme pilve sõnumite tasandil kohta leiate teavet teemast [protsessi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse. Lisateavet selle kohta, kuidas protsessi sõnumeid sündmuse jaoturi kaudu, lugege teemat [Alustamine sündmuse jaoturi] [ lnk-eventhubs-tutorial] õpetuse. (Selle õpetuse on kohaldatav asjade jaoturi sündmuse jaoturi ühilduv lõpp-punktid).

> [AZURE.NOTE] Sündmuse jaoturi ühilduv lõpp-punkti jaoks seadme pilve sõnumite lugemine. alati kasutab AMQP Protocol (protokoll).

1. Visual Studio, lisada Visual C# Windowsi klassikaline töölaua projekti praeguse lahenduse **Konsooli rakendus** projekti malli abil. Veenduge, et .NET Framework versioon on 4.5.1 või uuem versioon. Projekti **ReadDeviceToCloudMessages**nimi.

    ![Visual C# Windowsi klassikaline töölaua uue projekti][10]

2. Solution Exploreris Paremklõpsake **ReadDeviceToCloudMessages** projekti ja seejärel klõpsake nuppu **Halda Nuget-paketid**.

3. Aknas **Nugeti Package Manager** **WindowsAzure.ServiceBus**otsida, valige **installimiseks**ja nõustuge kasutustingimustega. Selle toimingu allalaadimist, installib ja lisab viite siini [Azure'i teenus][lnk-servicebus-nuget], koos kõigi sõltuvustega. See pakett võimaldab rakendus ühenduse loomiseks oma asjade jaoturi sündmuse jaoturi ühilduv lõpp.

4. Lisage järgmine `using` laused **Program.cs** faili ülaosas:

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Saate lisada **programmi** klassi järgmised väljad. Asendage kohatäite väärtuse jaoks loodud jaotises "Loo soovitud asjade jaoturi" asjade jaoturi ühendusstring.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Saate lisada **programmi** klassi järgmisel viisil:

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Seda meetodit kasutab **EventHubReceiver** näiteks vastuvõtmiseks pilvest kõik asjade jaoturi seadme – kui soovite-sõnumite vastuvõtmine sektsioonid. Pöörake tähelepanu sellele, kuidas te kaotate mõne `DateTime.Now` parameetri **EventHubReceiver** objekti, loomisel nii, et see saab ainult saadetud pärast käivitamist. Selle filter on kasulik testimiskeskkonnas sõnumite praegusi näeksite. Tootmiskeskkonnas oma kood tuleks veenduge, et töötleb kõigi sõnumite. Lisateabe saamiseks lugege teemat [kohta asjade jaoturi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse.

7. Lõpuks lisage järgmised read **Esilehele** meetod.

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises saate luua Windows konsooli rakendus, mis jäljendab seade, mis saadab seadme pilve sõnumite asjade jaoturiga.

1. Visual Studio, lisada Visual C# Windowsi klassikaline töölaua projekti praeguse lahenduse **Konsooli rakendus** projekti malli abil. Veenduge, et .NET Framework versioon on 4.5.1 või uuem versioon. Projekti **SimulatedDevice**nimi.

    ![Visual C# Windowsi klassikaline töölaua uue projekti][10]

2. Solution Exploreris Paremklõpsake **SimulatedDevice** projekti ja seejärel klõpsake nuppu **Halda Nuget-paketid**.

3. **Nugeti Package Manager** aknas valige **Sirvi**, otsige **Microsoft.Azure.Devices.Client**, **installida** **Microsoft.Azure.Devices.Client** paketi installimiseks valige ja nõustuge kasutustingimustega. Selle toimingu allalaadimist, installib ja lisab [Azure'i asjade - seadme SDK Nugeti pakett] viide[ lnk-device-nuget] ja sõltuvustega.

4. Lisage järgmine `using` **Program.cs** faili ülaosas lause:

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Saate lisada **programmi** klassi järgmised väljad. Kohatäite väärtused abil saate tuua jaotises "Loo soovitud asjade jaoturi" asjade jaoturi hostname asendada ja seadme võtit tuua jaotises "Loo seadme identiteedi".

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Saate lisada **programmi** klassi järgmisel viisil:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    See meetod saadab seadme pilve uue sõnumi iga teine. Sõnum sisaldab JSON seeriasertide objekti seadme ID-d ja juhuarv Tuul kiiruseandurit simuleerida.

7. Lõpuks lisage järgmised read **Esilehele** meetod.

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Vaikimisi loob **loomine** meetodit **DeviceClient** eksemplari, mis kasutab AMQP protokolli suhelda asjade jaoturi abil. Kasuta HTTP-protokolli, kasutage alistamine **loomine** meetod, mis võimaldab teil määrata protokolli. Kui kasutate HTTP-protokolli, tuleks lisada ka **Microsoft.AspNet.WebApi.Client** Nugeti pakett projekti **System.Net.Http.Formatting** nimeruumi kaasata.

Selle õpetuse juhatab teid läbi soovitud asjade jaoturi seadme kliendi loomise juhiseid. Saate kasutada ka [Ühendatud teenuse Azure asjade jaoturi] [ lnk-connected-service] Visual Studio laiend vajalikku koodi lisamiseks oma seadme klientrakendusega.


> [AZURE.NOTE] Selles õpetuses ei rakenda hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1.  Visual Studio Solution Exploreris paremklõpsake oma lahenduse, ja klõpsake **seadmine käivitus projektid**. Valige **mitme käivitus projektide**ja märkige soovitud **ReadDeviceToCloudMessages** ja **SimulatedDevice** projektide jaoks toimingu **käivitamine** .

    ![Käivitusatribuutide projekti][41]

2.  Vajutage klahvi **F5** nii töötab rakendusi käivitada. Konsooli väljundi **SimulatedDevice** rakenduse kaudu kuvatakse jäljendatud seadme saadab oma asjade jaoturi sõnumid. Konsooli väljundi **ReadDeviceToCloudMessages** rakenduse kaudu kuvatakse sõnumid, mis teie asjade jaoturi saab.

    ![Konsooli väljundi rakenduste kaudu][42]

3. [Azure'i portaalis] paani **kasutus** [ lnk-portal] kuvatakse jaoturi saadetud sõnumite arv:

    ![Azure'i portaali kasutamine paan][43]


## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses konfigureeritud soovitud asjade jaoturi Azure portaali ja seejärel loodud seadme identiteedi jaoturi identiteedi registri. Selle seadme identiteedi saate kasutada jäljendatud seadmes rakendus jaoturi seadme pilve sõnumeid saata. Saate ka loodud rakendus, mis kuvab jaoturi vastuvõetud sõnumid. 

Alustamine asjade jaoturi jätkamiseks ja teiste stsenaariumide asjade avastamiseks näha:

- [Teie seadme ühendamine][lnk-connect-device]
- [Mobiilsideseadmete halduse töötamise alustamine][lnk-device-management]
- [Asjade lüüsi SDK töötamise alustamine][lnk-gateway-SDK]

Saate teada, kuidas laiendada oma asjade lahenduse ja seadme pilve sõnumite alal, lugege teemat [protsessi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
