<properties
    pageTitle="Saatke asjade jaoturi cloud-seade | Microsoft Azure'i"
    description="Järgige selles õppetükis saate teada, kuidas Azure'i asjade jaoturi abil Java cloud-seadme sõnumite saatmiseks."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Õpetus: Kuidas asjade jaoturi ja Java cloud-seadme saatmine

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis aitab lubada vahelise miljoneid asjade seadmete turvalist kahesuunalise suhtluse ja rakenduse uuesti lõpetada. Õpetusega [Alustamine asjade jaoturi] näitab, kuidas luua soovitud asjade jaoturi, seadme identiteedi see säte ja kood jäljendatud seade, mis saadab seadme pilve sõnumite.

Selle õpetuse põhineb [asjade jaoturi alustamine]. See näitab teile, kuidas soovite:

- Oma rakenduse pilve tagasi lõppu, cloud-seadme sõnumeid saata ühe seadme asjade keskuse kaudu.
- Pilveteenuse-seadme sõnumeid seadmes.
- Oma rakenduse pilve lõpetamise uuesti, taotleda seadme asjade keskuse kaudu saadetud sõnumite kohaletoimetamise kinnituse (*tagasiside*).

Cloud-seadme sõnumite kohta lisateavet leiate [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

Õppeteema lõpus käivitate kaks Java konsooli rakendusi:

* **modelleerida seadme**muudetud versiooni rakenduse loodud [asjade jaoturi alustamine], mis loob teie asjade jaoturi ja saab pilve-seadme sõnumeid.
* **saada-c2d-sõnumid**, mis saadab sõnumi cloud-seadme jäljendatud seadme asjade jaoturi abil ja seejärel saab selle kohaletoimetamise kinnituse.

> [AZURE.NOTE] Asjade jaoturi SDK toetab mitme seadme platvormide ja keelte jaoks (sh C, Java ja Javascript) kaudu Azure'i asjade seadme SDK-d. Ühendage seade selles õpetuses kood ning üldiselt Azure'i asjade jaoturi üksikasjalikud juhised leiate teemast [Azure asjade Arenduskeskus].

Selle õpetuse tegemiseks vajate järgmist:

+ Java SE 8. <br/> [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Java selles õpetuses Linux või Windows.

+ Maven 3.  <br/> [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Maven selles õpetuses Linux või Windows.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

## <a name="receive-messages-on-the-simulated-device"></a>Meilisõnumite jäljendatud seadmes

Selles jaotises saate muuta jäljendatud seadme rakenduse loodud [Alustamine asjade jaoturi] cloud-seadme sõnumeid asjade keskuse kaudu.

1. Avage tekstiredaktoris abil simulated-device\src\main\java\com\mycompany\app\App.java faili.

2. Järgmised **MessageCallback** klassi kuuluva pesastatud klassimärkmiku **rakendus** klassi sees. Kui seade saab sõnumi asjade keskuse kaudu **käivitada** meetodit järgida. Selles näites seade alati teavitab jaoturi hääldusega sõnum:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Muuta **peamine** meetod **MessageCallback** eksemplari loomise ja kõne **setMessageCallback** meetodit enne selle avamist kliendi järgmiselt:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Kui te kasutate HTTP asemel MQTT või AMQP transport, kontrollib **DeviceClient** eksemplari meilisõnumeid asjade jaoturi harva (vähem kui iga 25 minuti järel). MQTT, AMQP ja HTTP ja asjade jaoturi pidurdamise erinevuste kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Pilveteenuse-seadme sõnumi saatmine

Selles jaotises saate luua Java konsooli rakendus, mis saadab cloud-seadme sõnumite jäljendatud seadmes rakendus. Peate seadme Id seadme [Alustamine asjade jaoturi] õpetuses lisasite. Samuti vajate ühendusstringi oma asjade jaoturi, mille leiate [Azure'i portaalis].

1. Looge Maven projekti nimega **saatmise-c2d-sõnumite** abil oma käsuviibas järgmine käsk. Pange tähele, et see on üks, pikk käsk:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Liikuge oma käsuviibas c2d-sõnumite saatmine-uue kausta.

3. Kasutades tekstiredaktoris, avage pom.xml fail saada-c2d-sõnumid kausta ja lisada järgmised sõltuvus **sõltuvused** sõlm. Sõltuvus lisamise võimaldab teil teenusega asjade jaoturi suhtlemiseks kasutada oma rakenduse **iothub java-teenus kliendi** paketi.

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Salvestage ja sulgege fail pom.xml.

5. Avage tekstiredaktoris abil send-c2d-messages\src\main\java\com\mycompany\app\App.java faili.

6. Faili **importimine** järgmistest lisamiseks tehke järgmist.

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. **Rakenduse** ainekursust, asendades **{yourhubconnectionstring}** ja **{yourdeviceid}** väärtused oma märkida varem järgmiste klassi taseme muutujate lisamiseks tehke järgmist.

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. Järgmine kood, mis loob ühenduse oma asjade jaoturi, saadab sõnumi seadme ja seejärel ootab kinnitus, et seade vastu ja töödeldud sõnumi **peamiseks** asendamiseks tehke järgmist.

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Selles õpetuses ei rakenda lihtsuse huvides, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale-modelleerida seadme kausta, käivitage järgmine käsk alustada telemeetria saata oma asjade jaoturi ja kuulata cloud-seadme sõnumeid saata oma keskuse kaudu:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Käivitage rakendus jäljendatud seade][img-simulated-device]

2. Käsuviibale-saada-c2d-sõnumid kausta, käivitage järgmine käsk cloud-seadme sõnumi saatmine ja oodake, kuni tagasiside kinnitus:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Käivitage käsk cloud-seadme sõnumi saatmine][img-send-command]

## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis saate õppinud, kuidas pilve-seadme sõnumite saatmiseks ja vastuvõtmiseks. 

Täieliku lõpuni lahendusi, mis kasutavad asjade jaoturi näiteid, leiate teemast [Azure asjade komplekti].

Asjade jaoturi lahendusi arendamise kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Asjade jaoturi kasutamise alustamine]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Asjade jaoturi arendaja juhend]: iot-hub-devguide.md
[Azure'i asjade Arenduskeskus]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure'i portaal]: https://portal.azure.com
[Azure'i asjade komplekti]: https://azure.microsoft.com/documentation/suites/iot-suite/