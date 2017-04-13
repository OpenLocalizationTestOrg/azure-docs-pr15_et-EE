<properties
    pageTitle="Asjade jaoturi seadme pilve sõnumite (.Net) | Microsoft Azure'i"
    description="Järgige selle õpetuse teavet kasulik mustrite asjade jaoturi seadme pilve sõnumite töötlemiseks."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Õpetus: Kuidas asjade jaoturi seadme pilve sõnumite .net-i abil

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis võimaldab usaldusväärsest ja turvaline kahesuunalise side miljoneid asjade seadmed ja rakenduse uuesti end. Muud õpetused ([Alustamine asjade jaoturi] ja [saatke cloud-seadme asjade jaoturi][lnk-c2d]) näitab teile, kuidas kasutada seadme pilve ja pilveteenuse-seadme sõnumside põhifunktsioone asjade jaoturi.

Selle õpetuse tugineb õpetusega [Alustamine asjade jaoturi] näidatud koodi, kus on näha kaks scalable mustrite, mille abil saate seadmes pilve sõnumite:

- [Azure'i bloobimälu]seadme pilve sõnumite usaldusväärne talletamist. Stsenaarium on *külma tee* analytics, kus saate talletada telemeetria andmete plekid analytics protsessidest sisendina kasutada. Need protsessid saate juhtida selliste tööriistadega nagu [Azure andmete Factory] või virnlintdiagrammil [Hdinsightiga (Hadoopi)] .

- Usaldusväärne töötlemise *interaktiivse* seadme pilve sõnumeid. Seadme pilve sõnumid on interaktiivsed, kui ta on kohe käivitab rakenduse tagasi lõpuks toimingute kogum. Näiteks seade võib saada alarmi teade, mis käivitab CRM-i süsteemis on Piletite lisamine. Seevastu *andmepunkt* sõnumite lihtsalt siseneda analytics mootori. Näiteks temperatuur telemeetria seadmest, mis on talletamise hilisemaks analüüsiks on andmete punkti sõnum.

Kuna asjade jaoturi seab mõne [Sündmuse jaoturi][lnk-event-hubs]-ühilduv lõpp-punkti saada seadme pilve sõnumite see õpetus kasutab [EventProcessorHost] eksemplar. Käesoleval juhul:

* Azure'i bloobimälu usaldusväärselt talletab *andmepunkt* sõnumeid.
* Edastab *interaktiivse* seadme pilve sõnumeid Azure'i [teenus siini järjekorda] kohe töötlemiseks.

Teenuse siini aitab tagada usaldusväärne töötlemise interaktiivsed sõnumeid, nagu see pakub ühe sõnumi postkastid ja kellaaja aken vastavalt de kordamise.

> [AZURE.NOTE] Näiteks **EventProcessorHost** on ainult üks viis interaktiivsed sõnumite töötlemiseks. Muude suvandite hulka kuuluvad [Azure teenuse struktuuri] [ lnk-service-fabric] ja [Azure voo Analytics][lnk-stream-analytics].

Õppeteema lõpus käivitate kolme Windowsi konsooli rakendused:

* **SimulatedDevice**, muudetud versiooni rakenduse loodud [Alustamine asjade jaoturi] õpetuses saadab andmed hetkel seadme pilve sõnumeid iga teine ja interaktiivse seadme pilve sõnumite iga 10 sekundi järel. See rakendus kasutab AMQP protokoll asjade jaoturi suhelda.
* **ProcessDeviceToCloudMessages** kasutab [EventProcessorHost] klassi sõnumite toomiseks sündmuse jaoturi ühilduv lõpp-punkti. See siis tingimata salvestab andmed hetkel sõnumeid Azure'i bloobimälu ja edastab interaktiivsed sõnumite teenuse siini järjekorda.
* Tühistage järjekorrad **ProcessD2CInteractiveMessages** interaktiivsed sõnumid teenuse siini järjekorda.

> [AZURE.NOTE] Asjade jaoturi SDK toetab mitme seadme platvormide ja tekst, sh C, Java ja JavaScripti. Kuidas asendada jäljendatud seadme selles õpetuses füüsilise seadmega ja seadmed asjade jaoturiga ühenduse loomise kohta leiate teemast [Azure asjade Arenduskeskus].

Selle õpetuse kehtib otse tarbimine sündmuse jaoturi ühilduv sõnumeid, nt [Hdinsightiga (Hadoopi)] projektid muid võimalusi. Lisateavet leiate [Azure'i asjade jaoturi arendaja juhendist - seadme Cloud].

Selle õpetuse tegemiseks vajate järgmist:

+ Microsoft Visual Studio 2015.

+ Aktiivne Azure'i konto. <br/>Kui teil pole kontot, saate luua ka [tasuta konto](https://azure.microsoft.com/free/) vaid paar minutit.

Mõned põhilised teadmised [Azure Storage] ja [Azure'i teenus]peaks teil olema.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktiivne sõnumite saatmine jäljendatud seadme kaudu

Selles jaotises saate muuta jäljendatud seadme rakenduse loodud [Alustamine asjade jaoturi] õpetuse asjade jaoturi interaktiivse seadme pilve sõnumeid saata.

1. Visual Studio, Projectis **SimulatedDevice** lisada järgmise meetodi **programmi** ainekursust.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    See meetod on sarnane **SimulatedDevice** projekti **SendDeviceToCloudMessagesAsync** meetod. Ainult erinevused on nüüd atribuudi **MessageId** süsteemi, ja kasutajale atribuudi nimega **messageType**.
    Koodi määrab atribuudi **MessageId** Globaalne ainuidentifikaator (GUID). Teenuse siini abil saate selle identifikaator tühistage dubleerimine saadud sõnumeid. Valimi kasutab atribuudi **messageType** interaktiivne andmete punkti sõnumitest eristada. Rakenduse läbib see teave sõnumi atribuudid, mitte sõnumi kehasse nii, et sündmuse protsessor ei pea andmeatribuutide sõnum teha sõnumi marsruutimist.

    > [AZURE.NOTE] See on oluline luua **MessageId** kasutatav tühistage dubleerimiseks interaktiivsed sõnumite seadme kood. Vahelduva võrgu suhtlus või muude tõrkeid põhjustada mitme Reklaamitarad sama sõnumi selle seadme kaudu. Saate kasutada ka semantilise sõnumi ID, nt räsi andmeväljad vastav sõnum asemel GUID.

2. Enne järgmise meetodi **põhi** meetodi lisamine soovitud `Console.ReadLine()` joon:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Selles õpetuses ei rakenda huvides lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitika, nt eksponentsiaalse backoff, soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

## <a name="process-device-to-cloud-messages"></a>Protsess seadme pilve sõnumid

Selles jaotises saate luua Windows konsooli rakendus, mis töötleb seadme pilve sõnumite asjade keskuse kaudu. Asjade jaoturi seab [sündmuse jaoturi]-ühilduv lõpp-punkti lubamiseks rakenduse seadme pilve sõnumeid lugeda. Selle õpetuse kasutab [EventProcessorHost] klassi teadetest konsooli rakendus. Sündmuse jaoturi kaudu protsessi sõnumite kohta leiate lisateavet teemast õpetusega [Alustamine sündmuse jaoturi] .

Kui rakendate andmepunkti sõnumite usaldusväärseid salvestusruumi või interaktiivse sõnumite edasisaatmine on sündmuse töötlemise juures on keerukas tugineb sõnumi tarbija postkastid ette edenemisega. Lisaks saavutamiseks kõrge läbilaskevõime, kui peate lugema sündmuse jaoturi esitate suurte pakkidena postkastid. Seda moodust loob dubleeritud töötlemise suure hulga sõnumeid kui ilmneb tõrge, ja saate pöörduda tagasi eelmisele kontrollpunkt võimalus. Selles õpetuses näete, kuidas sünkroonida Azure Storage kirjutab ja teenuse siini de kattumise windows **EventProcessorHost** postkastid.

Azure Storage sõnumite usaldusväärselt kirjutamiseks valimi kasutab funktsiooni üksikute Blokeeri Kinnita [Blokeeri]plekid[Azure Block Blobs]. Sündmuse protsessor liidetakse sõnumite mälu enne postkasti esitada. Näiteks pärast akumuleeritud puhvri sõnumite jõuab 4 MB mahu ülempiir blokeerimine või pärast teenuse siini de kattumise ajaakna kaitseaega. Seejärel enne kontrollpunkt, kood kinnitab uus plokk soovitud bloobimälu.

Sündmuse protsessor kasutab sündmuse jaoturi sõnumi Advance Blokeeri ID-d. See võimaldab sündmuse protsessori de kattumise kontrolli enne selle paneb uus plokk mäluruumi, jälgides võimalike krahhi vahel toime plokk ja selle kontrollpunkt.

> [AZURE.NOTE] Selle õpetuse kasutab ühe Azure Storage konto kirjutamiseks kõik sõnumid asjade keskuse kaudu tuua. Otsustada, kui peate kasutama teie lahendus mitme Azure Storage konto, lugege teemat [Azure Storage skaleeritavus juhised].

Rakendus kasutab funktsiooni teenuse siini de kattumise vältimiseks duplikaatide töötleb interaktiivsed sõnumeid. Jäljendatud seadme lisab ajatempli koos kordumatu **MessageId**iga interaktiivne sõnum. Need ID lubada teenus siini tagamaks, et määratud de kattumise kellaaja aknas edastatakse pole kaks sõnumid, mille sama **MessageId** vastuvõtjad. See de kattumise koos järjekordade teenuse siini esitatud ühe sõnumi lõpetamise semantika lihtne rakendada interaktiivsed sõnumite usaldusväärseid töötlemise.

Veenduge, et pole sõnumi on väljaspool de kattumise akna uuesti, sünkroonib koodi teenuse siini järjekorda de kattumise akna **EventProcessorHost** kontrollpunkt süsteem. See sünkroonimine on tehtud sunnib postkasti vähemalt ühe korra iga kord, kui de kattumise akna kaitseaega (selles õpetuses aken on üks tund).

> [AZURE.NOTE] Selle õpetuse kasutab ühe sektsioonitud teenuse siini järjekorra protsessi interaktiivsed sõnumite tuua asjade keskuse kaudu. Teie lahendus skaleeritavus rahuldamiseks teenuse siini järjekorrad kasutamise kohta lisateabe saamiseks dokumentatsioonist [Azure'i teenus siini] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Azure Storage konto ja teenus siini järjekorda ettevalmistamine
Klassi [EventProcessorHost] kasutamiseks peab teil olema Azure Storage konto lubamiseks **EventProcessorHost** kontrollpunkt teabe salvestamiseks. Saate kasutada Azure Storage konto või järgige [Azure Storage] uue konto loomiseks. Kirjutage Azure Storage konto ühendusstring.

> [AZURE.NOTE] Kui kopeerite ja kleebite Azure Storage konto ühendusstring, veenduge, et ei ole tühikuid kaasata.

Samuti vajate teenuse siini järjekorra interaktiivsed sõnumite usaldusväärseid töötlemise lubamine. Saate luua järjekorda programmiliselt, üks tund de kattumise aknas, nagu on selgitatud teemas [kasutamise teenuse siini järjekorrad][teenuse siini järjekorda]. Teise võimalusena saate kasutada [Azure klassikaline portaali][lnk-classic-portal], järgmiselt:

1. Klõpsake vasakus ülanurgas nuppu **Uus** . Klõpsake **Rakenduse teenused** > **Teenuse siini** > **järjekorda** > **Kohandatud loomine**. Sisestage nimi **d2ctutorial**, valige piirkond, ja kasutada mõne olemasoleva nimeruum või looge uus. Järgmisel lehel Valige **Luba duplikaatide tuvastamine**ja seatud **dubleerimine tuvastamise ajalugu ajaakna** üks tund. Klõpsake alumises paremas nurgas salvestada oma järjekorda konfiguratsiooni märke.

    ![Azure'i portaalis järjekorra loomiseks][30]

2. Teenuse siini järjekorda loendis, klõpsake **d2ctutorial**ja klõpsake nuppu **Konfigureeri**. Õigustega **kuulata** loomine kahe ühiskasutusega juurdepääsu poliitikad, ühe nimega **saatmise** õigustega **saata** ja ühe nimega **kuulata** . Kui olete lõpetanud, klõpsake allservas nuppu **Salvesta** .

    ![Azure'i portaalis järjekorra konfigureerimine][31]

3. Klõpsake **armatuurlaua** üleval, ja seejärel **ühenduseteavet** allservas. Kirjutage kaks ühendusstringi.

    ![Järjekorda armatuurlaua Azure'i portaalis][32]

### <a name="create-the-event-processor"></a>Protsessor sündmuse loomine

1. Praeguse lahenduse Visual Studios **Konsooli rakendus** projekti malli abil Visual C# Windows projekti loomiseks valige **fail** > **lisamine** > **Uue projekti**. Veenduge, et .NET Framework versioon on 4.5.1 või uuem versioon. Projekti **ProcessDeviceToCloudMessages**nimi ja klõpsake nuppu **OK**.

    ![Uue projekti Visual Studio][10]

2. Solution Exploreris Paremklõpsake **ProcessDeviceToCloudMessages** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**. Kuvatakse dialoogiboks **Nugeti Package Manager** .

3. Otsige **WindowsAzure.ServiceBus**, klõpsake nuppu **Installi**ja nõustuge kasutustingimustega. See toiming allalaadimist installib ja lisab [Azure'i teenus siini Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus), kõik sõltuvustega viide.

4. Otsige **Microsoft.Azure.ServiceBus.EventProcessorHost**, klõpsake nuppu **Installi**ja nõustuge kasutustingimustega. See toiming allalaadimist installib ja lisab [Azure'i siini sündmuse teeninduskeskuse - EventProcessorHost Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), koos kõigi sõltuvustega viide.

5. Paremklõpsake **ProcessDeviceToCloudMessages** projekt, klõpsake nuppu **Lisa**ja klõpsake **klassi**. Selle uue ainekursuse **StoreEventProcessor**nimi ja seejärel klõpsake nuppu **OK** , et luua ainekursust.

6. Lisage StoreEventProcessor.cs faili ülaosas järgmistest:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Asendada selle ainekursuse sisu järgmine kood:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Klassi **EventProcessorHost** kõned see tund töödelda seadme pilve asjade jaoturi saadud sõnumeid. Selle klassi koodi rakendab loogika salvestada sõnumite usaldusväärselt bloobimälu container ja interaktiivsed sõnumid teenuse siini kuhjuda edastada.

    **OpenAsync** meetodit lähtestab **currentBlockInitOffset** muutuja, mis jälgib praeguse offset lugeda, selle sündmuse protsessor esimese sõnumi. Pidage meeles, et iga protsessor vastutab ühe sektsiooni.

    **ProcessEventsAsync** meetodit saab paketi sõnumite asjade keskuse kaudu ja töötleb neid järgmiselt: see saadab interaktiivsed sõnumite teenuse siini kuhjuda ja lisab andmed hetkel sõnumeid mälu puhvri nimetatakse **toAppend**. Kui mälu puhvri jõuab 4 MB piirang või de kattumise kellaaeg Windowsi kaitseaega (üks tund pärast postkasti selles õpetuses), käivitab rakenduse postkasti.

    **AppendAndCheckpoint** meetod loob esmalt blockId, lisada blokeerida. Azure'i salvestusruumi nõuab kõigi blokeerida olema ühepikkused, nii, et meetodit Vatipadjad offset nullid - ID-d `currentBlockInitOffset.ToString("0000000000000000000000000")`. Seejärel, kui selle ID-ga plokk on juba selle bloobimälu, meetodit kirjutab see puhvri praeguse sisu.

    > [AZURE.NOTE] Koodi lihtsustamiseks selles õpetuses kasutab ühe bloobimälu kohta partition sõnumite talletamiseks. Reaal lahenduse rakendada, luues Lisafailid pärast teatud aja jooksul või kindla suurusega jõuab jooksvalt faili. Pidage meeles, et mõne Blokeeri Azure'i bloobimälu võib sisaldada kõige 195 GB andmeid.

8. **Programmi** tunni, lisage lause **abil** ülaosas.

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. **Programmi** tunni **põhi** meetodit muuta järgmiselt. Õpetusega [Alustamine asjade jaoturi] **iothubowner** ühendusstringi **{asjade jaoturi ühendusstringi}** asendada. Asendage salvestusruumi ühendusstringi märkisite selle jaotise alguses ühendusstring. Järjekorra **d2ctutorial** märkisite selle jaotise alguses nimega **saatmise** õigused teenuse siini ühendusstringi asendamiseks tehke järgmist.

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Huvides lihtne, kasutatakse selle õpetuse [EventProcessorHost] klassi ühekordsest. Lisateabe saamiseks lugege teemat [Sündmuse jaoturi programmeerimisega juhend].

## <a name="receive-interactive-messages"></a>Interaktiivne sõnumeid
Selles jaotises saate kirjutada Windows konsooli rakendus, mida saab interaktiivse sõnumeid teenuse siini järjekorda. Arhitekt lahenduse teenuse siini kasutamise kohta leiate lisateavet teemast [teenuse siini mitmekihilise rakenduste koostamine][].

1. Klõpsake praeguse lahenduse Visual Studio Visual C# Windows projekti loomine **Konsooli rakendus** projekti malli abil. Projekti **ProcessD2CInteractiveMessages**nimi.

2. Solution Exploreris Paremklõpsake **ProcessD2CInteractiveMessages** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**. See toiming kuvab **Nugeti Package Manager** .

3. Otsige **WindowsAzure.ServiceBus**, klõpsake nuppu **Installi**ja nõustuge kasutustingimustega. See toiming allalaadimist installid ja lisab viite [Siini Azure'i teenus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), koos kõigi sõltuvustega.

4. Lisage **Program.cs** faili ülaosas **abil** järgmistest.

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Lõpuks lisada järgmised read **Main** meetod. Ühendusstringi nimega **d2ctutorial**järjekorra õigustega **kuulata** asendada:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1.  Visual Studio Solution Exploreris teie lahendus paremklõpsake ja valige **Seadmine käivitus projektid**. **Mitme käivitus projekti**valige **käivitamine** nimega toimingu **ProcessDeviceToCloudMessages**, **SimulatedDevice**ja **ProcessD2CInteractiveMessages** projektide jaoks.

2.  Vajutage klahvi **F5** käivitada kolme konsooli rakendusi. **ProcessD2CInteractiveMessages** rakendus peaks protsessi iga interaktiivse sõnumi **SimulatedDevice** rakendusest.

  ![Kolm konsooli rakendusi][50]

> [AZURE.NOTE] Oma bloobimälu värskenduste nägemiseks peate väiksem väärtus, nt **1024** **MAX_BLOCK_SIZE** konstandi **StoreEventProcessor** tunni vähendamiseks. See muudatus on kasulik, kuna see võtab aega jõuda Blokeeri mahu ülempiir jäljendatud seadme saadetud andmetega. Blokeeri väiksem, kus pole vaja nii kaua oodata kuvamiseks bloobimälu luua ja värskendada. Siiski Blokeeri suuremaks kasutamine muudab rakenduse rohkem muutuv.

## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis saate õppinud, kuidas usaldusväärselt abil [EventProcessorHost] klassi andmepunkti ja interaktiivse seadme pilve sõnumite töötlemine.

[Kuidas saata cloud-seadme erineva asjade jaoturi] [ lnk-c2d] näidatakse, kuidas oma tagasi lõppu seadmetes sõnumeid saata.

Täieliku lõpuni lahendusi, mis kasutavad asjade jaoturi näited leiate [Azure'i asjade komplekti][lnk-suite].

Asjade jaoturi lahendusi arendamise kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure'i bloobimälu]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure'i andmed Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hdinsightiga (Hadoopi)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Teenuse siini järjekord]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure'i asjade jaoturi arendaja juhend - seadme Cloud]: iot-hub-devguide-messaging.md

[Azure'i salvestusruum]: https://azure.microsoft.com/documentation/services/storage/
[Azure'i teenus siini]: https://azure.microsoft.com/documentation/services/service-bus/

[Asjade jaoturi arendaja juhend]: iot-hub-devguide.md
[Asjade jaoturi kasutamise alustamine]: iot-hub-csharp-csharp-getstarted.md
[Azure'i asjade Arenduskeskus]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Azure'i salvestusruumi kohta]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Sündmuse jaoturi kasutamise alustamine]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure'i salvestusruumi skaleeritavus juhised]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Sündmuse jaoturi programmeerimisega juhend]: ../event-hubs/event-hubs-programming-guide.md
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Teenuse siini mitmekihilise rakenduste koostamine]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
