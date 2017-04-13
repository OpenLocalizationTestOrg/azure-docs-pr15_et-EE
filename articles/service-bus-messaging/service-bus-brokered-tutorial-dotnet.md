<properties 
    pageTitle="Teenuse siini vahendajaks sõnumside .net-i õpetus | Microsoft Azure'i"
    description="Vahendatud sõnumside .net-i õpetus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Teenuse siini vahendajaks sõnumside .net-i õpetus

Azure'i teenus siini pakub kahte täielik sõnumside lahendusi – ühe keskse "relay" klienditeenindusele töötab pilves, mis toetab mitmesuguseid eri transport protokollid ja Web services sätestatud standardeid; muu SOAP oli-*, ja ülejäänud. Klient ei pea asutusesisese teenuse otsene ühendus ega ei ole vaja teada, kus asub teenuse ja kohapealse teenus ei pruugi kõik sissetulevad pordid tulemüüris avatud.

Teine sõnumside lahendus võimaldab "vahendatud" kiirsõnumside võimalusi. Need saab vaadelda nii asünkroonne või sidumata sõnumside funktsioone, mis tugi avaldada-Telli, ajaline lahutamine ja laadi teenuse siini sõnumside taristu kasutamise stsenaariumid tasakaalustamiseks. Sidumata side on mitmeid eeliseid; näiteks klientide ja serverite saate ühendada vastavalt vajadusele ja toiminguid asünkroonne viisil.

Selles õpetuses mõeldud on teile ülevaade ja järjekorrad kogemusega, üks põhilisi komponente teenuse siini vahendajaks sõnumside. Kui te töötate selle teemaloendis selles õpetuses, on teil rakendus, mis kuvab sõnumite loendit, loob järjekorda ja saadab sõnumid selle järjekorda. Lõpuks rakenduse saab ja kuvab kuhjuda meilisõnumeid, siis migreerimispaketiga ressursse ja väljub. Vastavate õpetuse, mis kirjeldab, kuidas luua rakendus kasutab teenuse siini Relay, leiate teemast [teenuse siini edastada, sõnumside õpetuse](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Sissejuhatus ja eeltingimused

Järjekorrad pakkumine esimese, esimese välja FIFO kohaletoimetamise ühe või mitme konkureerivate tarbijatele. FIFO tähendab, et sõnumite oodatakse tavaliselt vastu ja töödelda vastuvõtjad ajaline järjestuses, milles need olid enqueued, ja iga sõnumi vastu ja töödelda vaid ühe sõnumi tarbija. Kasutamise järjekorrad põhieelis on see *ajaline lahutamine* komponendid saavutamiseks: Teisisõnu ja -tarbijate ei pea olema sõnumite saatmine ja vastuvõtmine korraga, kuna sõnumid on talletatud püsivalt järjekorda. Seotud kasu on *laadimine ühtlustamise*, mis võimaldab tootjate ja tarbijate erinevate määrade sõnumite saatmiseks ja vastuvõtmiseks.

Järgnevalt mõned enne õpetusega alustamist peate järgima haldus ja eelnevalt nõutud juhised. Esimene on teenuse nimeruumi loomiseks ja saada ühiskasutusega kiirklahv allkirja (SAS). Nimeruumi pakub iga rakenduse kaudu teenuse siini esitatud rakenduse soovitud äärist. SAS klahvi luuakse automaatselt süsteemi teenuse nimeruumi loomisel. Teenuse nimeruum ja SAS klahvi kombinatsiooni pakub kellega teenuse siini autendib juurdepääsu rakenduse mandaati.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Teenuse nimeruumi loomine ja saada SAS võti

Esimese asjana teenuse nimeruumi loomiseks ja saada [Ühiskasutusse Accessi allkirja](service-bus-sas-overview.md) (SAS) võti. Nimeruumi pakub iga rakenduse kaudu teenuse siini esitatud rakenduse soovitud äärist. SAS klahvi luuakse automaatselt süsteemi teenuse nimeruumi loomisel. Teenuse nimeruum ja SAS klahvi kombinatsiooni pakub mandaadi teenuse siini rakenduse juurdepääsu autentimiseks.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Järgmiseks on Visual Studio projekti loomine ja kirjutage kaks helper funktsioonid, mis komaga eraldatud sõnumite loendi laadimine tugevalt tipitud [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [loendis](https://msdn.microsoft.com/library/6sh2ey19.aspx) objekti.

### <a name="create-a-visual-studio-project"></a>Visual Studio projekti loomine

1. Avage Visual Studio administraatorina programmi menüüs Start paremklõpsata ja klõpsata käsku **Käivita administraatorina**.

1. Luua uue konsooli rakendus projekti. Klõpsake menüüd **fail** ja valige **Uus**ja seejärel klõpsake nuppu **projekti**. Dialoogiboksis **Uus projekt** klõpsake **Visual C#** (kui **Visual C#** ei kuvata, otsige jaotises **Muud keeled**), klõpsake malli **Konsooli rakendus** ja pange sellele **QueueSample**. Kasutage vaikeväärtust **asukoht**. Klõpsake nuppu **OK** , et luua projekt.

1. Nugeti package manager abil saate lisada teenuse siini teekide projekti.
    1. Solution Exploreris Paremklõpsake **QueueSample** projekti ja seejärel klõpsake **Haldamine NuGet-paketid**.
    2. Dialoogiboksis **Haldamine Nugeti pakettide** klõpsake menüüd **Sirvi** ja **Azure teenuse siini**otsida, siis klõpsake nuppu **Installi**.
<br />
1. Topeltklõpsake Solution Exploreris Program.cs faili avamiseks Visual Studio redaktoris. Nimeruumi nimi selle vaikenime muutmine `QueueSample` et `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Muutke soovitud `using` laused, nagu on näidatud järgmine kood.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Looge teksti fail nimega Data.csv ja Kopeeri tekst komaga eraldatud.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Salvestage ja sulgege fail Data.csv ja asukohta, kuhu salvestasite meeles pidada.

1. Paremklõpsake Solution Exploreris projekti (antud näites **QueueSample**), klõpsake nuppu **Lisa**ja seejärel nuppu **Olemasolevad üksus**.

1. Liikuge sirvides Data.csv fail, mille lõite juhises 6. Klõpsake menüüd fail ja seejärel klõpsake nuppu **Lisa**. Tagada, et * *kõik failid (*.*) ** on valitud failitüüpide loendit.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Meetod, mis sõelub sõnumite loendi loomine

1. Rakenduses on `Program` klassi enne selle `Main()` meetod deklareerida kahte muutujat: ühte tüüpi **Saidihaldur**, Data.csv sõnumite loendi. Teise peab olema tüübiga loendis objekti, et [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)tugevalt tipitud. Viimane on vahendatud sõnumiloendi õpetuse järgmise juhiseid kasutavad.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Väljaspool `Main()`, määratlege soovitud `ParseCSV()` meetod, mis sõelub Data.csv sõnumite loendit ja laadib sõnumid [Saidihaldur](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) tabelisse, nagu on näidatud siin. Meetod tagastab **Saidihaldur** objekti.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. Klõpsake soovitud `Main()` meetod lisada lause, mis nõuab soovitud `ParseCSVFile()` meetod:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Meetod, mis laadib meilisõnumite loendi loomine

1. Väljaspool `Main()`, määratlemine on `GenerateMessages()` meetod, mis võtab **Saidihaldur** objekti tagastatud `ParseCSVFile()` ja tabel laaditakse vahendatud sõnumite tugevalt tipitud loendit. Meetod tagastab **loendi** objekti, nagu järgmises näites. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. Klõpsake `Main()`, vahetult pärast kõne `ParseCSVFile()`, lisada lause, mis nõuab selle `GenerateMessages()` meetodi tagastatav väärtus: `ParseCSVFile()` argumendina:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Saada kasutajatunnust

1. Esmalt Looge kolm globaalne stringi muutujate korraldada need väärtused. Nende muutujate deklareerimine vahetult pärast eelmise muutuv deklaratsioon; näiteks:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Järgmisena looge funktsioon, mida aktsepteerib ja salvestab teenuse nimeruumi ja SAS võti. Lisage see meetod väljaspool `Main()`. Näiteks: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. Klõpsake `Main()`, vahetult pärast kõne `GenerateMessages()`, lisada lause, mis nõuab selle `CollectUserInput()` meetod:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Lahendus koostamine

Klõpsake menüü **koostada** Visual Studio saate klõpsake **Lahenduse luua** või vajutage **Klahvikombinatsiooni Ctrl + Shift + B** õigsust töö seni.

## <a name="create-management-credentials"></a>Looge identimisteabe haldamine

Selles etapis tuleb määratleda allkirja (SAS) mandaat, mis rakenduse lubatakse ühiskasutusega juurdepääsu loomiseks kasutate haldamise toiminguid.

1. Selgust, paigutab selle õpetuse järjekorda toimingute eraldi meetod. Luua mõne asünkroonse `Queue()` meetod on `Program` klassi pärast selle `Main()` meetod. Näiteks:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Järgmiseks on SAS mandaati kasutades [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objekti loomiseks. Loomise meetod võtab SAS võtme nimi ja saadud väärtus on `CollectUserInput()` meetod. Lisage järgmine kood on `Queue()` meetod:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Saate luua uue nimeruum halduse objekti, nimeruumi nimi ja haldamise mandaadi saadud eelmises etapis argumentidena sisaldavad URI. Lisage järgmine kood vahetult pärast eelmises toimingus lisatud kood. Veenduge, et asendada `<yourNamespace>` oma teenuse nimeruumi nimi:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Näide

Selles etapis oma kood peaks välja nägema järgmine:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Järjekorda sõnumeid saata

Selles etapis tuleb luua järjekorda, siis selle järjekorda vahendatud sõnumite loendi sõnumite saatmine.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Järjekorra loomiseks ja järjekorda sõnumeid saata

1. Esmalt looge järjekorda. Näiteks, pange selle `myQueue`, ja see deklareerida otse pärast haldamise toiminguid saate lisada soovitud `Queue()` meetod viimases etapis:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. Klõpsake soovitud `Queue()` meetod, sõnumside factory objekti luua vastloodud teenuse siini URI argumendina. Lisage järgmine kood otse pärast viimases etapis lisatud haldamise toiminguid. Veenduge, et asendada `<yourNamespace>` oma teenuse nimeruumi nimi:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Järgmisena looge abil [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) klassi järjekorda objekti. Lisage järgmine kood vahetult pärast viimases etapis lisatud kood.

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Järgmisena lisage vöötkood tsüklid kaudu vahendatud sõnumiloendi loodud varem saatmine iga järjekorda. Lisage järgmine kood vahetult pärast selle `CreateQueueClient()` eelmises etapis lause:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Saadud sõnumite järjekord

Selles etapis tuleb teil saada sõnumiloendi eelmises etapis loodud järjekorda.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Vastuvõtja loomine ja järjekorda sõnumeid saada

Klõpsake soovitud `Queue()` meetod itereerima läbi kuhjuda ja sõnumeid, mis on meetodil [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) , printimise iga sõnumi konsooli. Lisage järgmine kood vahetult pärast eelmises toimingus lisatud kood.

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Pange tähele, et selle `Thread.Sleep` kasutatakse ainult simuleerida sõnumi töötlemine ja ilmselt ei pea seda real kiirsõnumside rakenduses.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Järjekorda meetod lõpetada ja ressursside puhastamiseks

Vahetult pärast eelmise koodi lisada sõnumi factory ja järjekorda ressursside puhastamiseks järgmine kood:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Järjekorda meetod

Viimase sammuna tuleb lisada lause, mis nõuab selle `Queue()` meetod `Main()`. Lisage järgmised esiletõstetud rida koodi Main() lõpus.
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Näide

Järgmine kood sisaldab täieliku **QueueSample** taotluse.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Koostamine ja QueueSample rakenduse käivitamine

Nüüd, kui olete täitnud eelmist toimingut, saate luua ja käivitada **QueueSample** rakendus.

### <a name="build-the-queuesample-application"></a>Rakenduse QueueSample

Visual Studio, klõpsake menüü **koostamine** klõpsake **Lahenduse luua**või vajutage **Klahvikombinatsiooni Ctrl + Shift + B**. Kui ilmneb tõrkeid, veenduge, et teie kood on õige eelmises etapis lõpus esitatud täieliku näite põhjal.

## <a name="next-steps"></a>Järgmised sammud

Selle õpetuse ilmnes, kuidas luua teenuse siini kliendi rakenduste ja teenuste kohta teenuse siini vahendajaks sõnumside võimaluste kasutamine. Sarnaseid õpetuse, mis kasutab teenuse siini [Relay](service-bus-messaging-overview.md#Relayed-messaging), leiate teemast [teenuse siini edastada, sõnumside õpetuse](../service-bus-relay/service-bus-relay-tutorial.md).

Lisateavet [Teenuse siini](https://azure.microsoft.com/services/service-bus/), leiate järgmistest teemadest.

- [Teenuse siini sõnumside ülevaade](service-bus-messaging-overview.md)
- [Teenuse siini põhialused](service-bus-fundamentals-hybrid-solutions.md)
- [Teenuse siini ülesehitus](service-bus-architecture.md)

