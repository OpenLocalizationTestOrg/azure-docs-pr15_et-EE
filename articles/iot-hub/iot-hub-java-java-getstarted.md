<properties
    pageTitle="Azure'i asjade jaoturi Java alustamine | Microsoft Azure'i"
    description="Azure'i asjade jaoturi Java saada alustamine õpetuse. Azure'i asjade jaoturi ja Java asjade Interneti lahenduse kasutusele Microsoft Azure'i asjade SDK-d kasutada."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Azure'i asjade jaoturi Java kasutamise alustamine

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Õppeteema lõpus on teil kolm Java konsooli rakendusi.

* **looge seadme identiteedi**, mis loob seadme identiteedi ja nendega seotud võti jäljendatud videoseadme ühendamiseks.
* **lugemis-d2c-sõnumid**, mis kuvab telemeetria jäljendatud seadmest.
* **modelleerida seade**, mis ühendab oma asjade jaoturi varem loodud seadme ID ja saadab sõnumi telemeetria iga teise abil AMQP Protocol (protokoll).

> [AZURE.NOTE] Artikli [Asjade jaoturi SDK-d] [ lnk-hub-sdks] teave on erinevate SDK-d, mille abil saate luua nii rakendusi käivitada seadmed ja teie lahendus tagasi end.

Selle õpetuse tegemiseks vajate järgmist:

+ Java SE 8. <br/> [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Java selles õpetuses Linux või Windows.

+ Maven 3.  <br/> [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Maven selles õpetuses Linux või Windows.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Viimase sammuna **primaarvõtme** väärtus üles ja klõpsake **sõnumside**. Enne **sõnumid** , kirjutage **sündmuse jaoturi ühilduv nimi** ja **sündmuse jaoturi ühilduv lõpp-punkti**. Peate need kolm väärtust **lugemis-d2c-sõnumite** rakenduse loomisel.

![Azure'i portaali asjade jaoturi sõnumside blade][6]

Nüüd olete loonud oma asjade jaoturi ja teil on asjade jaoturi hostname asjade jaoturi ühendusstringi asjade jaoturi primaarvõtme, sündmuse jaoturi ühilduv nimi ja sündmuse jaoturi ühilduv lõpp-punkti, peate täitma selles õpetuses.

## <a name="create-a-device-identity"></a>Seadme identiteedi loomine

Selles jaotises saate luua Java konsooli rakendus, mis loob uue seadme identiteedi identiteedi registri oma asjade keskuses. Seade ei saa ühendust asjade jaoturi juhul, kui kirje on registris seadme identiteedi. **Seadme identiteedi registri** jaotisest [Asjade jaoturi arendaja juhend] [ lnk-devguide-identity] lisateabe saamiseks. Kui käivitate selle konsooli rakendus, see tekitab kordumatu seadme ID ja seadme abil saate ise kindlaks teha, kui seadme pilve sõnumid saadetakse asjade jaoturi võti.

1. Looge uus tühi kaust nimega java asjade toomine alustamine. Asjade java-alustamise kausta luua uue Maven projekti nimega **loomine seadme identiteedi** abil oma käsuviibas järgmine käsk. Pange tähele, et see on üks, pikk käsk:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Liikuge oma käsureale uue kausta loomine seadme identiteedi.

3. Avage pom.xml faili loomine – seadme identiteedi kausta tekstiredaktoris abil ja lisada järgmised sõltuvus **sõltuvused** sõlm. See võimaldab teil kasutada oma rakenduse iothub-teenus-sdk.

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Salvestage ja sulgege fail pom.xml.

5. Avage tekstiredaktoris abil create-device-identity\src\main\java\com\mycompany\app\App.java faili.

6. Faili **importimine** järgmistest lisamiseks tehke järgmist.

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. **Rakenduse** ainekursust, asendades **{yourhubconnectionstring}** väärtus oma märkida varem järgmiste klassi taseme muutujate lisamiseks tehke järgmist.

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Muuta allkirja **peamise** meetodi abil erandite järgmiselt:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Lisage järgmine kood **peamiseks** kehana. Järgmine kood tekitab seade nimega *javadevice* asjade jaoturi identiteedi registris, kui pole veel olemas. Seejärel kuvatakse seadme ID-d ja võti, mis läheb teil hiljem vaja:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Salvestage ja sulgege fail App.java.

11. **Loo-seadme identiteedi** rakenduse abil Maven koostamiseks, käivitada loomine – seadme identiteedi kausta-käsuviibale järgmine käsk:

    ```
    mvn clean package -DskipTests
    ```

12. **Loo-seadme identiteedi** rakenduse abil Maven käivitamiseks käivitada loomine – seadme identiteedi kausta-käsuviibale järgmine käsk:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Kirjutage **seadme id** ja **seadme võtit**. Peate hiljem kui loote rakendus, mis loob ühenduse asjade jaoturi seade.

> [AZURE.NOTE] Asjade jaoturi identiteedi registri salvestab ainult seadme identiteedid lubamiseks jaoturi turvaline juurdepääs. See talletab seadme ID-d ja klahvid, mida kasutada turvalisus mandaadid ja lubatud või keelatud lippu, mille abil saate üksikuid seadmes juurdepääsu keelamiseks. Kui rakenduse peab muu seadme metaandmete talletada, kasutada selle poest rakenduse kohased. [Asjade jaoturi arendaja juhend] viidata[ lnk-devguide-identity] lisateabe saamiseks.

## <a name="receive-device-to-cloud-messages"></a>Seadme pilve sõnumeid

Selles jaotises saate luua Java konsooli rakendus, mis loeb seadme pilve sõnumite asjade keskuse kaudu. Soovitud asjade jaoturi seab mõne [Sündmuse jaoturi][lnk-event-hubs-overview]-ühilduv lõpp-punkti selleks, et saaksite seadme pilve sõnumeid lugeda. Selles õpetuses hoida asju on lihtne, loob lihtsa lugeja, mis ei sobi kõrge läbilaskevõime juurutamine. [Protsessi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse näitab, kuidas seadme pilve sõnumite tasandil. [Alustamine sündmuse jaoturi] [ lnk-eventhubs-tutorial] õppetükk annab täiendava teabe kohta, kuidas protsessi sõnumeid sündmuse jaoturi kaudu ja on mõeldud asjade jaoturi sündmuse jaoturi ühilduv lõpp-punktid.

> [AZURE.NOTE] Sündmuse jaoturi ühilduv lõpp-punkti jaoks seadme pilve sõnumite lugemine. alati kasutab AMQP Protocol (protokoll).

1. *Loo seadme identiteedi* tööetapis loodud asjade java-alustamise kausta luua uue Maven projekti nimega **lugemis-d2c-sõnumite** abil oma käsuviibas järgmine käsk. Pange tähele, et see on üks, pikk käsk:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Liikuge oma käsuviibas uue lugemis-d2c-sõnumid kausta.

3. Kasutades tekstiredaktoris, avage pom.xml fail lugemis-d2c-sõnumid kausta ja lisada järgmised sõltuvus **sõltuvused** sõlm. See võimaldab teil eventhubs-kliendi paketi abil rakenduse lugeda sündmuse jaoturi ühilduv lõpp-punkti.

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Salvestage ja sulgege fail pom.xml.

5. Avage tekstiredaktoris abil read-d2c-messages\src\main\java\com\mycompany\app\App.java faili.

6. Faili **importimine** järgmistest lisamiseks tehke järgmist.

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Lisada järgmised klassi taseme muutujad klassimärkmiku **rakendus** . Asendage **{youriothubkey}**, **{youreventhubcompatibleendpoint}**ja **{youreventhubcompatiblename}** eelnevalt märkis väärtused:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Saate lisada **rakenduse** klassi **receiveMessages** järgmisel viisil. See meetod loob **EventHubClient** eksemplari sündmuste jaoturi ühilduv lõpp-punkti loomiseks ja seejärel loob asünkroonselt **PartitionReceiver** eksemplari lugeda mõne sündmuse jaoturi partition. See pidevalt tsüklid ja prinditakse sõnumi andmed kuni rakenduse lõpeb.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] See meetod kasutab filtri loob vastuvõtja nii, et vastuvõtja ainult loeb pärast vastuvõtja käivitub asjade jaoturi saadetud sõnumid. See on kasulik testimiskeskkonnas sõnumite praegusi näeksite. Tootmiskeskkonnas oma kood tuleks veenduge, et see töötleb kõik sõnumid – saate teada, [Kuidas asjade jaoturi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetus kohta lisateabe saamiseks.

9. Muuta allkirja **peamised** meetodi abil, kaasa arvatud järgmiselt:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Lisage järgmine kood **rakenduse** tunni **peamine** meetod. Järgmine kood loob kaks **EventHubClient** ja **PartitionReceiver** eksemplarid ja võimaldab teil rakenduse sulgete, kui olete lõpetanud sõnumite töötlemist.

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Järgmine kood eeldab, et olete loonud oma asjade jaoturi F1 (tasuta) astme. Tasuta asjade jaoturi on kaks sektsiooni nimega "0" ja "1".

11. Salvestage ja sulgege fail App.java.

12. Maven kasutav **lugemis-d2c-sõnumite** koostamine, et käivitada lugemis-d2c-sõnumid kausta-käsuviibale järgmine käsk:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises saate luua Java konsooli rakendus, mis jäljendab seade, mis saadab seadme pilve sõnumite asjade jaoturiga.

1. *Loo seadme identiteedi* tööetapis loodud asjade java-alustamise kausta luua uue Maven projekti nimega **modelleerida seadme** abil oma käsuviibas järgmine käsk. Pange tähele, et see on üks, pikk käsk:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Liikuge oma käsuviibas modelleerida seadme uue kausta.

3. Kasutades tekstiredaktoris, avage pom.xml fail modelleerida seadme kausta ja lisada järgmised sõltuvused **sõltuvused** sõlm. See võimaldab teil suhelda oma asjade jaoturi ja serialiseerida JSON Java objektide iothub-java-kliendi paketi oma rakenduse kasutamine.

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Salvestage ja sulgege fail pom.xml.

5. Avage tekstiredaktoris abil simulated-device\src\main\java\com\mycompany\app\App.java faili.

6. Faili **importimine** järgmistest lisamiseks tehke järgmist.

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. **Rakenduse** ainekursust, asendades oma asjade jaoturi nime ja abil loodud *seadme identiteedi loomine* jaotises seadme võtmeväärtuse **{yourdevicekey}** **{youriothubname}** järgmiste klassi taseme muutujate lisamiseks tehke järgmist.

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    See valimi rakendus kasutab muutuja **Protocol (protokoll)** , kui see instantiates **DeviceClient** objekti. HTTP- või AMQP protokolli abil saate suhelda asjade jaoturi.

8. Lisage järgmine pesastatud **TelemetryDataPoint** klassi sees **rakenduse** klassi seadme saadab oma asjade jaoturi telemeetria andmete määramiseks.

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Lisage järgmised pesastatud **EventCallback** klassi **rakenduse** klassi kinnituse olek tagastav asjade jaoturi töötleb jäljendatud seadmest sõnumi kuvamiseks sisse. See meetod teavitab ka rakenduse peamine teema, kui sõnum on töödeldud:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Lisage järgmised pesastatud **MessageSender** klassi sees klassimärkmiku **rakendus** . **Käivitage** selle klassi meetodit genereeritud telemeetria Näidisandmete saata oma asjade jaoturi ja ootab kinnituse enne järgmise sõnumi saatmist.

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    See meetod saadab seadme pilve uue sõnumi ühe sekundi pärast asjade jaoturi tunnustab eelmise sõnumi. Sõnum sisaldab JSON seeriasertide objekti soovitud deviceId ja juhuarv Tuul kiiruseandurit simuleerida.

11. Järgmine kood, mis loob teemat seadme pilve sõnumeid saata oma asjade jaoturi **peamiseks** asendamiseks tehke järgmist.

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Salvestage ja sulgege fail App.java.

13. Koostamiseks **modelleerida seadet** kasutav Maven, käivitada modelleerida seadme kausta-käsuviibale järgmine käsk:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Selles õpetuses ei rakenda hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale-lugemis-d2c kausta, käivitage järgmine käsk vigu oma asjade keskuses jälgimise alustamiseks:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java asjade jaoturi teenuse klientrakenduse seadme pilve sõnumite jälgimine][7]

2. Käsuviibale-modelleerida seadme kausta, käivitage järgmine käsk alustada oma asjade jaoturi telemeetria andmete saatmine:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java asjade jaoturi seadme klientrakendusega seadme pilve sõnumite saatmiseks][8]

3. [Azure'i portaalis] paani **kasutus** [ lnk-portal] kuvatakse jaoturi saadetud sõnumite arv:

    ![Azure'i portaali kasutamine paani arv asjade jaoturi saadetud sõnumid][43]

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses konfigureeritud uue asjade jaoturi Azure portaali ja seejärel loodud seadme identiteedi jaoturi identiteedi registri. Selle seadme identiteedi saate kasutada jäljendatud seadmes rakendus jaoturi seadme pilve sõnumeid saata. Saate ka loodud rakendus, mis kuvab jaoturi vastuvõetud sõnumid. 

Alustamine asjade jaoturi jätkamiseks ja uurida asjade stsenaariumide näha:

- [Teie seadme ühendamine][lnk-connect-device]
- [Mobiilsideseadmete halduse töötamise alustamine][lnk-device-management]
- [Asjade lüüsi SDK töötamise alustamine][lnk-gateway-SDK]

Saate teada, kuidas laiendada oma asjade lahenduse ja seadme pilve sõnumite alal, lugege teemat [protsessi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/