## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Sõnumeid, mille EventProcessorHost Java

EventProcessorHost on Java klassi, mis lihtsustab sündmuse jaoturi haldamise püsivate postkastidega vastuvõtmise sündmused ja samal ajal saab neid sündmuse jaoturi. EventProcessorHost abil saate tükeldada sündmuste üle mitme vastuvõtjad, isegi siis, kui majutatud erinevaid sõlmi. Selles näites on kujutatud ühe vastuvõtja EventProcessorHost kasutamise kohta.

###<a name="create-a-storage-account"></a>Salvestusruumi konto loomine

EventProcessorHost kasutamiseks peab teil olema [Azure Storage konto][]:

1. [Azure'i klassikaline portaali][]sisse logida ja klõpsake nuppu **Uus** ekraani allosas.

2. Klõpsake **Data Services**, siis **salvestusruumi**, seejärel **Kiiresti luua**, ja tippige oma salvestusruumi konto nimi. Valige teie soovitud piirkond ja seejärel nuppu **Loo salvestusruumi konto**.

    ![][11]

3. Klõpsake äsja loodud salvestusruumi kontot ja seejärel klõpsake **Kiirklahvide haldamine**:

    ![][12]

    Kopeerige esmane kiirklahv hiljem kasutamine selles õpetuses.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Kasutades EventProcessor Host Java projekti loomine

Java kliendi teek sündmus jaoturi jaoks on saadaval kasutada Maven projektides [Maven keskses hoidlas][Maven Package], ja saate viidatud kasutamine järgmine sõltuvus deklaratsioon Maven projekti faili:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Erinevat tüüpi Koosta keskkonnas, saate otseselt hankida uusim välja JAR failid [Maven keskses hoidlas] [ Maven Package] või [github](https://github.com/Azure/azure-event-hubs/releases)punktist väljaanne jaotuse.  

1. Järgmises näites jaoks kõigepealt looma uue projekti Maven konsooli/shell rakenduse lemmik Java arenduskeskkond. Klassi nimetatakse ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Järgmine kood abil saate luua uue klassi nimetatakse ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Luua lõplik klassi nimetatakse ```EventProcessorSample```, kasutades järgmist.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Asendage väärtused sündmus keskuse ja salvestusruumi konto loomisel kasutatud järgmised väljad.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Selle õpetuse kasutab ühekordsest EventProcessorHost. Läbilaskevõime suurendamiseks on soovitatav käivitada mitu eksemplari EventProcessorHost. Sel juhul paljudel juhtudel automaatselt koordineerimiseks üksteisega saldo laadimiseks sündmused received. Kui soovite mitme vastuvõtjad iga protsessi *Kõik* sündmused, peate kasutama **ConsumerGroup** mõistet. Kui sündmuste saavate erinevates arvutites, see võib olla kasulik neid kasutatakse nimede EventProcessorHost eksemplarid, mis põhineb masinad (või rollid) määramiseks.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure'i salvestusruumi konto]: ../articles/storage/storage-create-storage-account.md
[Azure'i klassikaline portaal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

