<properties
    pageTitle="Asjade jaoturi seadme pilve sõnumite (Java) | Microsoft Azure'i"
    description="Järgige Java õppeteema teavet kasulik mustrite asjade jaoturi seadme pilve sõnumite töötlemiseks."
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
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Õpetus: Kuidas asjade jaoturi seadme pilve sõnumite Java abil

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis võimaldab usaldusväärsest ja turvaline kahesuunalise side miljoneid asjade seadmed ja rakenduse uuesti end. Muud õpetused ([Alustamine asjade jaoturi] ja [saatke cloud-seadme asjade jaoturi][lnk-c2d]) näitab teile, kuidas kasutada seadme pilve ja pilveteenuse-seadme sõnumside põhifunktsioone asjade jaoturi.

Selle õpetuse tugineb õpetusega [Alustamine asjade jaoturi] näidatud koodi, kus on näha kaks scalable mustrite, mille abil saate seadmes pilve sõnumite:

- [Azure'i bloobimälu]seadme pilve sõnumite usaldusväärne talletamist. Stsenaarium on *külma tee* analytics, kus saate talletada telemeetria andmete plekid analytics protsessidest sisendina kasutada. Need protsessid saate juhtida selliste tööriistadega nagu [Azure andmete Factory] või virnlintdiagrammil [Hdinsightiga (Hadoopi)] .

- Usaldusväärne töötlemise *interaktiivse* seadme pilve sõnumeid. Seadme pilve sõnumid on interaktiivsed, kui ta on kohe käivitab rakenduse tagasi lõpuks toimingute kogum. Näiteks seade võib saada alarmi teade, mis käivitab CRM-i süsteemis on Piletite lisamine. Seevastu *andmepunkti* sõnumite lihtsalt siseneda analytics mootori. Temperatuur telemeetria seadmest, mis on talletamise hilisemaks analüüsiks on näiteks andmepunkti – sõnumi.

Kuna asjade jaoturi seab mõne [Sündmuse jaoturi][lnk-event-hubs]-ühilduv lõpp-punkti saada seadme pilve sõnumite see õpetus kasutab [EventProcessorHost] eksemplar. Käesoleval juhul:

* Azure'i bloobimälu usaldusväärselt talletab *andmepunkti* sõnumite.
* Edastab *interaktiivse* seadme pilve sõnumeid Azure'i [teenus siini järjekorda] kohe töötlemiseks.

Teenuse siini aitab tagada usaldusväärne töötlemise interaktiivsed sõnumeid, nagu see pakub ühe sõnumi postkastid ja kellaaja aken vastavalt de kordamise.

> [AZURE.NOTE] Näiteks **EventProcessorHost** on ainult üks viis interaktiivsed sõnumite töötlemiseks. Muude suvandite hulka kuuluvad [Azure teenuse struktuuri] [ lnk-service-fabric] ja [Azure voo Analytics][lnk-stream-analytics].

Õppeteema lõpus käivitate kolme Java konsooli rakendused:

* **modelleerida seade**, muudetud versiooni lõite teemas õpetus [asjade jaoturi alustamine] rakenduse saadab andmepunkti seadme pilve sõnumite iga teine ja interaktiivse seadme pilve sõnumite iga 10 sekundi järel. See rakendus kasutab AMQP protokoll asjade jaoturi suhelda.
* **protsess-d2c-sõnumite** kasutab [EventProcessorHost] klassi sõnumite toomiseks sündmuse jaoturi ühilduv lõpp-punkti. See siis tingimata talletab andmepunkti sõnumite Azure'i bloobimälu ja edastab interaktiivsed sõnumite teenuse siini järjekorda.
* **protsessi interaktiivsed sõnumite** järjekorrad tühistage interaktiivsed sõnumid teenuse siini järjekorda.

> [AZURE.NOTE] Asjade jaoturi SDK toetab mitme seadme platvormide ja tekst, sh C, Java ja JavaScripti. Kuidas asendada jäljendatud seadme selles õpetuses füüsilise seadmega ja seadmed asjade jaoturiga ühenduse loomise juhised leiate teemast [Azure asjade Arenduskeskus].

Selle õpetuse kehtib otse tarbimine sündmuse jaoturi ühilduv sõnumeid, nt [Hdinsightiga (Hadoopi)] projektid muid võimalusi. Lisateavet leiate [Azure'i asjade jaoturi arendaja juhendist - seadme Cloud].

Selle õpetuse tegemiseks vajate järgmist:

+ Õpetusega [Alustamine asjade jaoturi] täielik töötamine versiooni.

+ Java SE 8. <br/> [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Java selles õpetuses Linux või Windows.

+ Maven 3.  <br/> [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Maven selles õpetuses Linux või Windows.

+ Aktiivne Azure'i konto. <br/>Kui teil pole kontot, saate luua ka [tasuta konto](https://azure.microsoft.com/free/) vaid paar minutit.

Mõned põhilised teadmised [Azure Storage] ja [Azure'i teenus]peaks teil olema.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktiivne sõnumite saatmine jäljendatud seadme kaudu

Selles jaotises saate muuta jäljendatud seadme rakenduse loodud [Alustamine asjade jaoturi] õpetuse asjade jaoturi interaktiivse seadme pilve sõnumeid saata.

1. Avage fail simulated-device\src\main\java\com\mycompany\app\App.java tekstiredaktoris abil. See fail sisaldab koodi **modelleerida seadme** rakenduse loodud õpetusega [Alustamine asjade jaoturi] .

2. **Rakenduse** klassi järgmist pesastatud klassi lisamiseks tehke järgmist.

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    See tund on sarnane **MessageSender** klassi **modelleerida seadme** Projectis. Ainult erinevused on nüüd atribuudi **MessageId** süsteemi, ja kohandatud atribuuti nimega **messageType**.
    Koodi määrab atribuudi **MessageId** üldiselt ainuidentifikaator (UUID). Teenuse siini abil saate selle identifikaator tühistage dubleerimine saadud sõnumeid. Valimi kasutab atribuudi **messageType** eristada interaktiivsed andmepunkti sõnumite. Rakenduse läbib see teave sõnumi atribuudid, mitte sõnumi kehasse nii, et sündmuse protsessor ei pea andmeatribuutide sõnum teha sõnumi marsruutimist.

    > [AZURE.NOTE] See on oluline luua **MessageId** kasutatav tühistage dubleerimiseks interaktiivsed sõnumite seadme kood. Vahelduva võrgu suhtlus või muude tõrkeid põhjustada mitme Reklaamitarad sama sõnumi selle seadme kaudu. Saate kasutada ka semantilise sõnumi ID, nt räsi andmeväljad vastav sõnum asemel valitud kasutajatunnust.

3. **Peamine** meetod nii interaktiivne sõnumite saatmiseks ja andmepunkti sõnumite nagu on näidatud järgmises koodilõigu muutmiseks tehke järgmist.

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Salvestage ja sulgege fail simulated-device\src\main\java\com\mycompany\app\App.java.

    > [AZURE.NOTE] Selles õpetuses ei rakenda huvides lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitika, nt eksponentsiaalse backoff, soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

5. Koostamiseks **modelleerida seadet** kasutav Maven, käivitada modelleerida seadme kausta-käsuviibale järgmine käsk:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Protsess seadme pilve sõnumid

Selles jaotises saate luua Java konsooli rakendus, mis töötleb seadme pilve sõnumite asjade keskuse kaudu. Asjade jaoturi seab [sündmuse jaoturi]-ühilduv lõpp-punkti lubamiseks rakenduse seadme pilve sõnumeid lugeda. Selle õpetuse kasutab [EventProcessorHost] klassi teadetest konsooli rakendus. Sündmuse jaoturi kaudu protsessi sõnumite kohta leiate lisateavet teemast õpetusega [Alustamine sündmuse jaoturi] .

Kui rakendada andmepunkti sõnumite usaldusväärseid salvestusruumi või interaktiivse meilisõnumite edasisaatmine on event töötlus tugineb sõnumi tarbija postkastid ette edenemisega peamiseks. Lisaks saavutamiseks kõrge läbilaskevõime, kui peate lugema sündmuse jaoturi esitate suurte pakkidena postkastid. Seda moodust loob dubleeritud töötlemise suure hulga sõnumeid, kui ilmneb tõrge, ja saate pöörduda tagasi eelmisele kontrollpunkt võimalus. Selles õpetuses näete, kuidas sünkroonida Azure Storage kirjutab ja teenuse siini de kattumise windows **EventProcessorHost** postkastid.

Azure Storage sõnumite usaldusväärselt kirjutamiseks valimi kasutab funktsiooni üksikute Blokeeri Kinnita [Blokeeri]plekid[Azure Block Blobs]. Sündmuse protsessor liidetakse sõnumite mälu enne postkasti esitada. Näiteks pärast akumuleeritud puhvri sõnumite jõuab 4 MB mahu ülempiir blokeerimine või pärast teenuse siini de kattumise ajaakna kaitseaega. Seejärel enne kontrollpunkt, kood kinnitab uus plokk soovitud bloobimälu.

Sündmuse protsessor kasutab sündmuse jaoturi sõnumi Advance Blokeeri ID-d. See võimaldab sündmuse protsessori de kattumise kontrolli enne selle paneb uus plokk mäluruumi, jälgides võimalike krahhi vahel toime plokk ja selle kontrollpunkt.

> [AZURE.NOTE] Selle õpetuse kasutab ühe Azure Storage konto kirjutamiseks kõik sõnumid asjade keskuse kaudu tuua. Otsustada, kui peate kasutama teie lahendus mitme Azure Storage konto, lugege teemat [Azure Storage skaleeritavus juhised].

Rakendus kasutab funktsiooni teenuse siini de kattumise vältimiseks duplikaatide töötleb interaktiivsed sõnumeid. Jäljendatud seadme lisab ajatempli koos kordumatu **MessageId**iga interaktiivne sõnum. Id teenus siini tagamaks, et määratud de kattumise kellaaja aknas edastatakse pole kaks sõnumid, mille sama **MessageId** vastuvõtjad. See de kattumise koos järjekordade teenuse siini esitatud ühe sõnumi lõpetamise semantika lihtne rakendada interaktiivsed sõnumite usaldusväärseid töötlemise.

Veenduge, et pole sõnumi on väljaspool de kattumise akna uuesti, sünkroonib koodi teenuse siini järjekorda de kattumise akna **EventProcessorHost** kontrollpunkt süsteem. See sünkroonimine on tehtud sunnib postkasti vähemalt ühe korra iga kord, kui de kattumise akna kaitseaega (selles õpetuses aken on üks tund).

> [AZURE.NOTE] Selle õpetuse kasutab ühe sektsioonitud teenuse siini järjekorra protsessi interaktiivsed sõnumite tuua asjade keskuse kaudu. Teie lahendus skaleeritavus rahuldamiseks teenuse siini järjekorrad kasutamise kohta lisateabe saamiseks dokumentatsioonist [Azure'i teenus siini] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Azure Storage konto ja teenus siini järjekorda ettevalmistamine

Klassi [EventProcessorHost] kasutamiseks peab teil olema Azure Storage konto lubamiseks **EventProcessorHost** kontrollpunkt teabe salvestamiseks. Saate kasutada Azure Storage konto või järgige [Azure Storage] uue konto loomiseks. Kirjutage Azure Storage konto ühendusstring.

> [AZURE.NOTE] Kui kopeerite ja kleebite Azure Storage konto ühendusstring, veenduge, et ei ole tühikuid kaasata.

Samuti vajate teenuse siini järjekorra interaktiivsed sõnumite usaldusväärseid töötlemise lubamine. Saate luua järjekorda programmiliselt, üks tund de kattumise aknas, nagu on selgitatud teemas [kasutamise teenuse siini järjekorrad][teenuse siini järjekorda]. Teise võimalusena saate kasutada [Azure klassikaline portaali][lnk-classic-portal], järgmiselt:

1. Klõpsake vasakus ülanurgas nuppu **Uus** . Klõpsake **Rakenduse teenused** > **Teenuse siini** > **järjekorda** > **Kohandatud loomine**. Sisestage nimi **d2ctutorial**, valige piirkond, ja kasutada mõne olemasoleva nimeruum või looge uus. Nimeruumi nimi üles, peate selle hiljem selles õpetuses. Järgmisel lehel Valige **Luba duplikaatide tuvastamine**ja seatud **dubleerimine tuvastamise ajalugu ajaakna** üks tund. Klõpsake alumises paremas nurgas salvestada oma järjekorda konfiguratsiooni märke.

    ![Azure'i portaalis järjekorra loomiseks][30]

2. Teenuse siini järjekorda loendis, klõpsake **d2ctutorial**ja klõpsake nuppu **Konfigureeri**. Õigustega **kuulata** loomine kahe ühiskasutusega juurdepääsu poliitikad, ühe nimega **saatmise** õigustega **saata** ja ühe nimega **kuulata** . Märkige üles nii poliitikate **primaarvõtme** väärtused, peate need hiljem selles õpetuses. Kui olete lõpetanud, klõpsake allservas nuppu **Salvesta** .

    ![Azure'i portaalis järjekorra konfigureerimine][31]

### <a name="create-the-event-processor"></a>Protsessor sündmuse loomine

Selles jaotises saate luua Java rakendus protsessi sõnumeid sündmuse jaoturi ühilduv lõpp.

Esimene ülesanne on lisada Maven projekti nimega **protsessi-d2c sõnumeid** , mille saab seadme pilve sõnumeid asjade jaoturi sündmuse jaoturi ühilduv lõpp-punkti ja marsruudib sõnumeid tagaandmebaas muude teenustega.

1. Õpetusega [Alustamine asjade jaoturi] loodud asjade java-alustamise kausta nimega **protsess-d2c-sõnumite** abil oma käsuviibas järgmine käsk Maven projekti loomine. Pange tähele, et see on üks, pikk käsk:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Liikuge oma käsuviibas uue protsessi-d2c-sõnumid kausta.

3. Kasutades tekstiredaktoris, avage pom.xml fail protsess-d2c-sõnumid kausta ja lisada järgmised sõltuvused **sõltuvused** sõlm. Need sõltuvused võimaldavad teil suhelda oma asjade jaoturi ja teenuse siini järjekorda rakenduse azure-eventhubs, azure-eventhubs-eph ja azure-servicebus pakettide kasutamine:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Salvestage ja sulgege fail pom.xml.

Järgmise ülesande on hõlmav tund **ErrorNotificationHandler** lisamine projekti.

1. Process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java faili loomiseks kasutada tekstiredaktorit. Järgmine kood lisamiseks faili tõrketeated **EventProcesssorHost** eksemplarist kuvamiseks tehke järgmist.

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Salvestage ja sulgege fail ErrorNotificationHandler.java.

Nüüd saate lisada klassi, mis rakendab **IEventProcessor** kasutajaliidese. Klassi **EventProcessorHost** kõned see tund töödelda seadme pilve asjade jaoturi saadud sõnumeid. Selle klassi koodi rakendab loogika salvestada sõnumite usaldusväärselt bloobimälu container ja interaktiivsed sõnumid teenuse siini kuhjuda edastada.

**OnEvents** meetod seab **latestEventData** muutuja, mis jälgib uusim sõnum lugeda, selle sündmuse protsessor offset ja jada arvu. Pidage meeles, et iga protsessor vastutab ühe sektsiooni. **OnEvents** meetodit seejärel saab paketi sõnumite asjade keskuse kaudu ja töötleb neid järgmiselt: see saadab interaktiivsed sõnumite teenuse siini kuhjuda ja lisab andmed hetkel sõnumeid **toAppend** mälu puhvri. Kui mälu puhvri jõuab 4 MB Blokeeri piiratud või de kattumise kellaaeg Windowsi kaitseaega (üks tund pärast viimast kontrollpunkt selles õpetuses), käivitab meetodit postkasti.

**AppendAndCheckPoint** meetod loob esmalt **blockId** lisandada selle bloobimälu blokeerida. Azure'i salvestusruumi nõuab kõigi blokeerida ID olema ühepikkused, et meetodit Vatipadjad nihke, mille ees on null. Siis, kui selle ID-ga plokk on juba selle bloobimälu, meetodit kirjutab see praeguse puhvri sisuga.

> [AZURE.NOTE] Koodi lihtsustamiseks selles õpetuses kasutab ühe bloobimälu kohta partition sõnumite talletamiseks. Reaal lahenduse rakendada, luues Lisafailid pärast teatud aja jooksul või kindla suurusega jõuab jooksvalt faili. Pidage meeles, et mõne Blokeeri Azure'i bloobimälu võib sisaldada kõige 195 GB andmeid.

Järgmise ülesande on rakendada **IEventProcessor** kasutajaliidese:

1. Process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java faili loomiseks kasutada tekstiredaktorit.

2. EventProcessor.java faili lisada järgmised impordi ja klassi määratlus. Klassi **EventProcessor** rakendab **IEventProcessor** kasutajaliides, mis määratleb sündmuse jaoturi kliendi toimimist:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Rakendada **IEventProcessor** kasutajaliidese **EventProcessor** klassi järgmistest meetoditest lisamiseks tehke järgmist.

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Klassi **EventProcessor** järgmiste klassi taseme muutujate lisamiseks tehke järgmist.

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Klassi **EventProcessor** järgmised allkirjaga sisestusmeetod **AppendAndCheckPoint** lisamiseks tehke järgmist.

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Lisage järgmine kood **AppendAndCheckPoint** meetodi abil tuua sektsiooni praeguse sõnumi offset ja jada arv.

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. Meetodi **AppendAndCheckPoint** **BlockEntry** eksemplari jaoks järgmise ajaploki salvestada selle bloobimälu loomiseks kasutada offset praeguse väärtuse:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. **AppendAndCheckPoint** meetod üles laadida uusima kogum sõnumite blokeerimine bloobimälu ja tuua praeguse loetelu.

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. **AppendAndCheckPoint** meetod, luua uue bloobimälu algse blokeerimine või olemasoleva bloobimälu blokeering lisada:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Lõpuks **AppendAndCheckPoint** meetodit, luua postkasti sektsiooni ja koostada järgmise ajaploki sõnumite salvestamiseks.

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. **OnEvents** meetod, lisage järgmine kood asjade jaoturi lõpp-punkti ja sõnumite edasisaatmine interaktiivsed sõnumeid vastu teenuse siini järjekorda. Klõpsake kõne **AppendAndCheckPoint** meetodit blokeering on täis- või aeg, mille aegub:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Lõpuks **onEvents** meetodit, lisage klausel "veel kui", kui aeg, mille lõpeb pole ühtegi teadet, mis on pärit asjade jaoturi **AppendAndCheckPoint** helistada.

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Muudatuste salvestamiseks EventProcessor.java faili.

Lõplik tööülesande **protsess-d2c-sõnumite** projekt on **peamine** meetod, mis instantiates **EventProcessorHost** näiteks koodi lisamine.

1. Avage fail process-d2c-messages\src\main\java\com\mycompany\app\App.java tekstiredaktoris abil.

2. Faili **importimine** järgmistest lisamiseks tehke järgmist.

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Lisada järgmised klassi taseme muutuja klassimärkmiku **rakendus** . Tegite üles varem [ettevalmistamise Azure Storage konto ja teenus siini järjekorda](#provision-an-azure-storage-account-and-a-service-bus-queue) jaotises Azure Storage konto ühendusstring **{yourstorageaccountconnectionstring}** asendamiseks tehke järgmist.

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Lisage järgmised klassi taseme muutujad **rakenduse** klassi ja oma teenuse siini nimeruum ja **{yourservicebussendkey}** **{yourservicebusnamespace}** asendamine oma järjekorda **saata** klahvi. Varem tehtud [ettevalmistamise Azure Storage konto ja teenus siini järjekorda](#provision-an-azure-storage-account-and-a-service-bus-queue) jaotises nimeruum ja **Kuulake** väärtused üles:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Lisada järgmised klassi taseme muutujad klassimärkmiku **rakendus** . **{Youreventhubcompatibleendpoint}** asendada sündmuse jaoturi ühilduv lõpp-punkti väärtus. Lõpp-punkti väärtus, mis näeb välja umbes **tema... nimeruum** nii, et eemaldamisel on **sb: / /** ees- ja järelliide **.servicebus.windows.net/** . **{Youreventhubcompatiblename}** asendada sündmuse jaoturi ühilduv nimi. Asendage **{youriothubkey}** **iothubowner** võti. Tegite need väärtused märkme [loomine soovitud asjade jaoturi] [ lnk-create-an-iot-hub] õpetusega *Alustamine Azure'i asjade jaoturi Java* jaotis:

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Allkirja **peamiseks** muuta järgmiselt:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. **Peamised** meetodi abil bloobimälu ümbris, mis talletab sõnumeid saada järgmine kood lisamiseks tehke järgmist.

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. **Peamine** meetod viite siini teenuse teenuse saada järgmine kood lisamiseks tehke järgmist.

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. **Peamine** meetod, konfigureerimine ja **EventProcessorHost** eksemplari loomine. Suvandi **setInvokeProcessorAfterReceiveTimeout** tagab **EventProcessorHost** eksemplari viitab **onEvents** meetodit **IEventProcessor** liides, isegi siis, kui ei töödelda sõnumeid. **OnEvents** meetod siis alati käivitab **AppendAndCheckPoint** meetodit aeg, mille aegumisel.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. **Peamised** meetodi registreerida **EventProcessorHost** eksemplari **IEventProcessor** rakendamist:

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Loogika lisamine lõpuks **peamine** meetod sulgeda **EventProcessorHost** eksemplari:

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Salvestage ja sulgege fail process-d2c-messages\src\main\java\com\mycompany\app\App.java.

13. **Protsess-d2c-sõnumite** rakenduse abil Maven koostamiseks, käivitada protsess-d2c-sõnumid kausta-käsuviibale järgmine käsk:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Interaktiivne sõnumeid

Selles jaotises saate kirjutada Java konsooli rakendus, mida saab interaktiivse sõnumeid teenuse siini järjekorda.

Esimene ülesanne on lisada Maven projekti nimega **protsessi interaktiivsed sõnumeid** , mis saab **EventProcessor** eksemplari teenuse siini järjekorda saadetud sõnumid.

1. Õpetusega [Alustamine asjade jaoturi] loodud asjade java-alustamise kausta nimega **protsessi interaktiivsed sõnumite** abil oma käsuviibas järgmine käsk Maven projekti loomine. Pange tähele, et see on üks, pikk käsk:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Liikuge oma käsuviibas uue protsessi interaktiivsed sõnumid kausta.

3. Abil tekstiredaktoris, avage pom.xml fail protsessi interaktiivsed sõnumid kausta ja lisada järgmised sõltuvus **sõltuvused** sõlm. See sõltuvus võimaldab teil kasutage azure-servicebus pakkimine oma rakenduse suhelda oma teenuse siini järjekorda.

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Salvestage ja sulgege fail pom.xml.

Järgmise ülesande on kood sõnumite toomiseks teenuse siini järjekorda lisada.

1. Avage fail process-interactive-messages\src\main\java\com\mycompany\app\App.java tekstiredaktoris abil.

2. Lisage järgmine `import` laused faili:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Lisage järgmised klassi taseme muutujad **rakenduse** klassi ja oma teenuse siini nimeruum ja **{yourservicebuslistenkey}** **{yourservicebusnamespace}** asendamine oma järjekorda **kuulata** võti. Varem tehtud [ettevalmistamise Azure Storage konto ja teenus siini järjekorda](#provision-an-azure-storage-account-and-a-service-bus-queue) jaotises nimeruum ja **Kuulake** väärtused üles:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. **Rakenduse** klassi järjekorda sõnumeid vastu järgmised pesastatud klassi lisamiseks tehke järgmist.

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Allkirja **peamiseks** muuta järgmiselt:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. **Peamine** meetod, lisage järgmine kood uute sõnumite kuulamine alustamiseks:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Salvestage ja sulgege fail process-interactive-messages\src\main\java\com\mycompany\app\App.java.

8. **Protsessi interaktiivsed sõnumite** rakenduse abil Maven koostamiseks, käivitada protsessi interaktiivsed sõnumid kausta-käsuviibale järgmine käsk:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis kolm rakenduste käitamiseks.

1.  **Protsessi interaktiivsed sõnumite** rakenduse käivitamiseks Käsuviip või kesta liikuge protsessi interaktiivsed sõnumid kausta ja käivitage järgmine käsk:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Protsessi interaktiivsed sõnumite käivitamine][processinteractive]

2.  **Protsess-d2c-sõnumite** rakenduse käivitamiseks Käsuviip või kesta liikuge protsess-d2c-sõnumid kausta ja käivitage järgmine käsk:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Käivita protsess d2c sõnumid][processd2c]

3.  **Modelleerida seadme** rakenduse käivitamiseks Käsuviip või kesta liikuge kausta, modelleerida seade ja käivitage järgmine käsk:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Käivitage modelleerida seade][simulateddevice]

> [AZURE.NOTE] Oma bloobimälu värskenduste nägemiseks peate väiksem väärtus, nt **1024** **MAX_BLOCK_SIZE** konstandi **StoreEventProcessor** tunni vähendamiseks. See muudatus on kasulik, kuna see võtab aega jõuda Blokeeri mahu ülempiir jäljendatud seadme saadetud andmetega. Blokeeri väiksem, kus pole vaja nii kaua oodata kuvamiseks bloobimälu luua ja värskendada. Siiski Blokeeri suuremaks kasutamine muudab rakenduse rohkem muutuv.

## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis saate õppinud, kuidas usaldusväärselt abil [EventProcessorHost] klassi andmepunkti ja interaktiivse seadme pilve sõnumite töötlemine.

[Kuidas saata cloud-seadme erineva asjade jaoturi] [ lnk-c2d] näidatakse, kuidas oma tagasi lõppu seadmetes sõnumeid saata.

Täieliku lõpuni lahendusi, mis kasutavad asjade jaoturi näited leiate [Azure'i asjade komplekti][lnk-suite].

Asjade jaoturi lahendusi arendamise kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Azure'i bloobimälu]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure'i andmed Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hdinsightiga (Hadoopi)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Teenuse siini järjekord]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure'i asjade jaoturi arendaja juhend - seadme Cloud]: iot-hub-devguide-messaging.md

[Azure'i salvestusruum]: https://azure.microsoft.com/documentation/services/storage/
[Azure'i teenus siini]: https://azure.microsoft.com/documentation/services/service-bus/

[Asjade jaoturi arendaja juhend]: iot-hub-devguide.md
[Asjade jaoturi kasutamise alustamine]: iot-hub-java-java-getstarted.md
[Azure'i asjade Arenduskeskus]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Azure'i salvestusruumi kohta]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Sündmuse jaoturi kasutamise alustamine]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure'i salvestusruumi skaleeritavus juhised]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub