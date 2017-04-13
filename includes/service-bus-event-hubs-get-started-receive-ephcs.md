## <a name="receive-messages-with-eventprocessorhost"></a>Sõnumeid, mille EventProcessorHost

[EventProcessorHost][] on .net-i klassi, mis lihtsustab sündmuse jaoturi haldamise püsivate postkastidega vastuvõtmise sündmused ja samal ajal saab neid sündmuse jaoturi. [EventProcessorHost][]kasutamisel saate tükeldada sündmuste üle mitme vastuvõtjad, isegi siis, kui majutatud erinevaid sõlmi. Selles näites on kujutatud ühe vastuvõtja [EventProcessorHost][] kasutamise kohta. Valimi [Scaled välja sündmuse töötlemiseks][] näitab, kuidas kasutada [EventProcessorHost][] mitme vastuvõtjad.

[EventProcessorHost][]kasutamiseks peab teil olema [Azure Storage konto][]:

1. [Azure'i portaali][]sisse logida, ja klõpsake nuppu **Uus** ülaosas ekraani vasakus.

2. **Andmete + salvestusruumi**, klõpsake käsku **salvestusruumi konto**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Tippige labale **Loo salvestusruumi konto** salvestusruumi konto nimi. Valige soovitud Azure'i tellimus, ressursirühm ja asukohta, kus soovite luua ressursi. Klõpsake nuppu **Loo**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Klõpsake loendis salvestusruumi kontod vastloodud salvestusruumi kontot.

5. Klõpsake labale salvestusruumi konto **kiirklahvide**. Kopeerige **võti1** hiljem kasutamine selles õpetuses väärtus.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Visual Studio, looge uus Visual C# töölaua rakenduse projekt **Konsooli rakendus** projekti malli abil. Projekti **vastuvõtja**nimi.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. Solution Exploreris paremklõpsake lahendus ja seejärel klõpsake nuppu **Halda Nugeti pakettide lahenduse**.

6. Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Veenduge, et projekti nime (**vastuvõtja**) on määratud väljale **versioonid** . Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio allalaadimist, installib ja lisab [Azure'i siini sündmuse teeninduskeskuse - EventProcessorHost Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), koos kõigi sõltuvustega viide.

7. Paremklõpsake **vastuvõtja** projekt, klõpsake nuppu **Lisa**ja klõpsake **klassi**. Selle uue ainekursuse **SimpleEventProcessor**nimi ja seejärel klõpsake nuppu **Lisa** ainekursuse loomiseks.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. Lisage SimpleEventProcessor.cs faili ülaosas järgmistest:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Asendada selle ainekursuse sisu järgmine kood:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    See tund on kutsutud **EventProcessorHost** protsessi sündmuste saanud sündmuse keskuse kaudu. Pange tähele, et selle `SimpleEventProcessor` klassi kasutab Stopper perioodiliselt helistamiseks kontrollpunkt meetodit **EventProcessorHost** kontekstis. See tagab, et vastuvõtja taaskäivitamist, seda ise rohkem kui 5 minuti töötlemise töö.

9. **Programmi** tunni, lisage järgmine `using` faili ülaosas lause:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Seejärel asendada selle `Main` meetod on `Program` ainekursuse järgmine kood, valemis sündmuse jaoturi nime ja varem salvestatud nimeruum taseme ühendusstringi ja klahvi, mille kopeerisite eelmiste jaotiste salvestusruumi konto. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Selle õpetuse kasutab ühekordsest [EventProcessorHost][]. Läbilaskevõime suurendamiseks on soovitatav käivitada mitu eksemplari [EventProcessorHost][], nagu on näidatud valimis [Scaled välja sündmuse töötlemiseks][] . Sel juhul paljudel juhtudel automaatselt koordineerimine omavahel saldo laadimiseks sündmused received. Kui soovite mitme vastuvõtjad iga protsessi *Kõik* sündmused, peate kasutama **ConsumerGroup** mõistet. Kui sündmuste saavate erinevates arvutites, see võib olla kasulik neid kasutatakse nimede [EventProcessorHost][] eksemplarid, mis põhineb masinad (või rollid) määramiseks. Järgmiste teemade kohta leiate lisateavet teemast [Sündmuse jaoturi ülevaade][] ja [Sündmuse jaoturi programmeerimisega juhendi][] teemad.

<!-- Links -->
[Sündmuse jaoturi ülevaade]: ../articles/event-hubs/event-hubs-overview.md
[Sündmuse jaoturi programmeerimisega juhend]: ../articles/event-hubs/event-hubs-programming-guide.md
[Mastaabitud välja event töötlus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure'i salvestusruumi konto]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure'i portaal]: https://portal.azure.com