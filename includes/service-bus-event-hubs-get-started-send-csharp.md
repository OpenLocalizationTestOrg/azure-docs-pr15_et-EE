## <a name="send-messages-to-event-hubs"></a>Sündmuse jaoturi sõnumeid saata

Selles jaotises kirjutate Windows konsooli rakendus, mis saadab sündmuste oma sündmuse jaoturi.

1. Visual Studio, looge uus Visual C# töölaua rakenduse projekt **Konsooli rakendus** projekti malli abil. Projekti **saatja**nimi.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. Solution Exploreris paremklõpsake lahendus ja seejärel klõpsake nuppu **Halda Nugeti pakettide lahenduse**. 

3. Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus`. Veenduge, et projekti nime (**saatja**) on määratud väljale **versioonid** . Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio allalaadimist, installib ja lisab [Azure'i teenus siini teegi Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus)viide.

4. Lisage järgmine `using` laused **Program.cs** faili ülaosas:

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Järgmised väljad lisada **programmi** ainekursust, nimi ja eelmises tööetapis loodud sündmuse jaoturi varem salvestatud nimeruum taseme ühendusstringi kohatäite väärtuste asendamine.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Saate lisada **programmi** klassi järgmisel viisil:

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    See meetod saadab pidevalt sündmuste oma sündmuse keskuses 200-ms viivitusega.

7. Lõpuks lisage järgmised read **Esilehele** meetod.

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
