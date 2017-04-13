<properties
    pageTitle="Saatke asjade jaoturi cloud-seade | Microsoft Azure'i"
    description="Järgige selles õppetükis saate teada, kuidas saatmine cloud-seadme kasutamine Azure asjade jaoturi C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Õpetus: Kuidas asjade jaoturi ja .net cloud-seadme saatmine

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis aitab lubada vahelise miljoneid asjade seadmete turvalist kahesuunalise suhtluse ja rakenduse uuesti lõpetada. Õpetusega [Alustamine asjade jaoturi] näitab, kuidas luua soovitud asjade jaoturi, seadme identiteedi see säte ja kood jäljendatud seade, mis saadab seadme pilve sõnumite.

Selle õpetuse põhineb [asjade jaoturi alustamine]. See näitab teile, kuidas soovite:

- Oma rakenduse pilve tagasi lõppu, cloud-seadme sõnumeid saata ühe seadme asjade keskuse kaudu.
- Pilveteenuse-seadme sõnumeid seadmes.
- Oma rakenduse pilve lõpetamise uuesti, taotleda seadme asjade keskuse kaudu saadetud sõnumite kohaletoimetamise kinnituse (*tagasiside*).

Cloud-seadme sõnumite kohta lisateavet leiate [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

Õppeteema lõpus, võite käivitada kaks Windows konsooli rakendusi:

* **SimulatedDevice**, muudetud versiooni rakenduse loodud [asjade jaoturi alustamine], mis loob teie asjade jaoturi ja saab pilve-seadme sõnumeid.
* **SendCloudToDevice**, mis saadab sõnumi cloud-seadme jäljendatud seadme asjade jaoturi abil ja seejärel saab selle kohaletoimetamise kinnituse.

> [AZURE.NOTE] Asjade jaoturi SDK toetab mitme seadme platvormide ja keelte jaoks (sh C, Java ja Javascript) kaudu Azure'i asjade seadme SDK-d. Ühendage seade selles õpetuses kood ning üldiselt Azure'i asjade jaoturi üksikasjalikud juhised leiate teemast [Azure asjade Arenduskeskus].

Selle õpetuse tegemiseks peate järgmist.

+ Microsoft Visual Studio 2015

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

## <a name="receive-messages-on-the-simulated-device"></a>Meilisõnumite jäljendatud seadmes

Selles jaotises saate muuta jäljendatud seadme rakenduse loodud [Alustamine asjade jaoturi] cloud-seadme sõnumeid asjade keskuse kaudu.

1. Visual Studio, Projectis **SimulatedDevice** lisada järgmise meetodi **programmi** ainekursust.

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    Funktsiooni `ReceiveAsync` meetod tagastab asünkroonselt vastuvõetud sõnumi ajal, et selle kätte seade. Tagastab *null* pärast kindlaksmäärataval ajalõpp (sel juhul kasutatakse vaikimisi ühe minuti). Sellisel juhul kood tuleks jätkata oodata uute sõnumite. See on selle põhjuseks on `if (receivedMessage == null) continue` joon.

    Kõne suunamiseks `CompleteAsync()` asjade jaoturi teavitab, et sõnum on edukalt töödeldud. Sõnumi turvaline eemaldamine seadme järjekorda. Kui midagi juhtunud, et takistada seadme rakendus sõnumi töötlemise lõpuleviimine, pakub asjade jaoturi see uuesti. Seejärel on oluline, et sõnum töötlemise loogika seadme rakenduses *idempotent*, nii, et sama sõnumit vastu mitu korda annab sama tulemuse. Rakenduse saate ka ajutiselt loobuda sõnumi, mille tulemuseks on asjade jaoturi säilitades sõnumi tulevaste ettenähtud järjekorras. Või taotluse tagasilükkamiseks sõnumile kuhjuda sõnumi eemaldatakse jäädavalt. Pilveteenuse-seadme sõnumi elutsükli kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Kui HTTP asemel MQTT või AMQP transport, kasutades funktsiooni `ReceiveAsync` meetod tagastab kohe. Toetatud muster HTTP cloud-seadme sõnumid on aeg-ajalt ühendatud seadmete kontrollimine sõnumite harva (vähem kui iga 25 minuti järel). Veel HTTP emissiooni saab tulemuste asjade keskuses pidurdamise taotlused. MQTT, AMQP ja HTTP ja asjade jaoturi pidurdamise erinevuste kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

2. Enne järgmise meetodi **põhi** meetodi lisamine soovitud `Console.ReadLine()` joon:

        ReceiveC2dAsync();

> [AZURE.NOTE] Selles õpetuses ei rakenda lihtsuse huvides, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

## <a name="send-a-cloud-to-device-message"></a>Pilveteenuse-seadme sõnumi saatmine

Selles jaotises kirjutate Windows konsooli rakendus, mis saadab cloud-seadme sõnumite jäljendatud seadmes rakendus.

1. Praeguse lahenduse Visual Studios luua uue projekti Visual C# töölaua rakenduse **Konsooli rakendus** projekti malli abil. Projekti **SendCloudToDevice**nimi.

    ![Uue projekti Visual Studio][20]

2. Solution Exploreris lahendus paremklõpsake ja seejärel klõpsake nuppu **Halda Nugeti pakettide lahenduse...**. 

    Avatakse aken **Haldamine NuGet-paketid** .

3. Otsige `Microsoft Azure Devices`, klõpsake nuppu **Installi**ja nõustuge kasutustingimustega. 

    See allalaadimist, installib ja lisab [Azure'i asjade Interneti - teenuse SDK Nugeti pakett]viide.

4. Lisage järgmine `using` **Program.cs** faili ülaosas lause:

        using Microsoft.Azure.Devices;

5. Saate lisada **programmi** klassi järgmised väljad. Asendada asjade jaoturi ühendusstringi [Alustamine asjade jaoturi]kohatäite väärtus:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Saate lisada **programmi** klassi järgmisel viisil:

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    See meetod saadab cloud-seadme uue sõnumi ID-ga, seade `myFirstDevice`. Muutke see parameeter vastavalt sellele, juhuks, kui seda muutnud, mida kasutatakse [asjade jaoturi alustamine].

7. Lõpuks lisage järgmised read **Esilehele** meetod.

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Visual Studio sees paremklõpsake oma lahenduse, ja valige **käivitamisel määrata projektide …**. Valige **käivitus mitmed projektid**, valige **ProcessDeviceToCloudMessages**, **SimulatedDevice**ja **SendCloudToDevice**toimingu **käivitamine** .

9.  Vajutage **klahvi F5**. Kõigi kolme rakenduste peab algama. Valige **SendCloudToDevice** windows ja vajutage sisestusklahvi **Enter**. Peaksite nägema sõnumit ei saanud jäljendatud rakenduse abil.

    ![Rakenduse vastuvõtmise sõnum][21]

## <a name="receive-delivery-feedback"></a>Saada tagasisidet kohaletoimetamise
On võimalik taotluse kohaletoimetamise (või aegumise) kinnitused asjade keskuse kaudu cloud-seadme iga sõnumi kohta. See võimaldab cloud tagasi lõpuks uuesti või hüvitamise loogika hõlpsalt teatada. Pilveteenuse-seadme tagasiside kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

Selles jaotises muudate **SendCloudToDevice** rakenduse koosolekukutse tagasiside ja saadud asjade jaoturi.

1. Visual Studios, **SendCloudToDevice** projekti, saate lisada järgmise meetodi **programmi** klassi.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Pöörake tähelepanu sellele, vastuvõtu mustri sama kasutasite seadme rakenduse pilve-seadme sõnumeid vastu.

2. Lisage järgmise meetodi **põhi** meetodi, kohe pärast selle `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` joon:

        ReceiveFeedbackAsync();

3. Cloud-seadme sõnumi kohaletoimetamisega tagasiside taotlemiseks tuleb määrata atribuut **SendCloudToDeviceMessageAsync** meetod. Lisage järgmine rida, kohe pärast selle `var commandMessage = new Message(...);` joon:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Käivitage rakendused, vajutage **klahvi F5**. Peaksite nägema kõigi kolme rakenduste käivitamine. Valige **SendCloudToDevice** windows ja vajutage sisestusklahvi **Enter**. Peaksite nägema sõnumit ei saanud jäljendatud rakenduse abil ja mõne sekundi, sai **SendCloudToDevice** rakenduse tagasiside sõnum.

    ![Rakenduse vastuvõtmise sõnum][22]

> [AZURE.NOTE] Selles õpetuses ei rakenda lihtsuse huvides, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis saate õppinud, kuidas pilve-seadme sõnumite saatmiseks ja vastuvõtmiseks. 

Täieliku lõpuni lahendusi, mis kasutavad asjade jaoturi näiteid, leiate teemast [Azure asjade komplekti].

Asjade jaoturi lahendusi arendamise kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure'i asjade Interneti - teenuse SDK Nugeti pakett]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Asjade jaoturi arendaja juhend]: iot-hub-devguide.md
[Asjade jaoturi kasutamise alustamine]: iot-hub-csharp-csharp-getstarted.md
[Azure'i asjade Arenduskeskus]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure'i asjade komplekti]: https://azure.microsoft.com/documentation/suites/iot-suite/