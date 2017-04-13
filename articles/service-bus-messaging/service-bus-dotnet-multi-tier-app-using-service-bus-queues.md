<properties
    pageTitle=".Net-i mitmekihilise rakenduse | Microsoft Azure'i"
    description=".Net-i õpetus, mis aitab teil välja mitmekihilise rakenduse Azure, mis kasutab teenuse siini järjekorrad suhelda astme vahel."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.Net-i mitmekihilise rakenduse abil Azure'i teenus siini järjekorrad

## <a name="introduction"></a>Sissejuhatus

Microsoft Azure arendamine on lihtne kasutada Visual Studio ja tasuta Azure SDK .net-i jaoks. Selles õpetuses juhendab teid loomise rakendus kasutab mitu Azure ressursse, mis töötab teie kohaliku keskkonnas juhiseid. Juhistes eeldatakse, et te ei ole eelnevalt kogemusi, kasutades Azure.

Saate teada järgmist:

-   Kuidas lubada Azure arengu ühe allalaadimine ja installimine oma arvutisse.
-   Kuidas kasutada Visual Studio Azure arendada.
-   Kuidas luua mitmekihilise rakenduse Azure kasutamine veebi ja töötaja rollid.
-   Kuidas suhelda astme teenuse siini järjekorrad kasutamise vahel.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

Selles õppetükis saate koostada ja Azure pilveteenuses mitmekihilise rakenduse käivitada. Esiosa on ASP.net-i MVC web rolli ja tagasi selleks tuleb töötaja rolli, mis kasutab teenuse siini järjekorda. Saate sama mitmekihilise rakenduse esiosa web projekti asemel pilveteenus Azure veebisaidile juurutatud. Juhised selle kohta, mida teha teisiti Azure veebisaidi esi lõpetada, jaotisest [järgmised toimingud](#nextsteps) . Samuti võite proovida [.net-i kohta – ruumide/cloud hübriid rakenduse](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) õpetuse.

On kujutatud järgmisel kuvatõmmisel on täidetud.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Stsenaarium ülevaade: vaheline rolli suhtlus

Tellimuse töötlemiseks edastada tuleb ees UI komponent web roll, kus töötab suhelda keskmisel taseme loogika töötab töötaja roll. Selles näites kasutatakse teenuse siini vahendajaks sõnumside jaoks kasutatakse suhtlemine.

Abil vahendatud sõnumside vahel veebi- ja keskmise astme decouples kahest osast. Erinevalt otsese sõnumside (ehk teisisõnu öeldes TCP või HTTP), web taseme pole ühendust selle otse; selle asemel see sunnib ühikud töö sõnumeid, teenuse siini, mis säilitab need usaldusväärselt kuni keskele taseme on valmis tarbimine ja töödelda sisse.

Teenuse siini pakub kaks, kes toetaks vahendatud sõnumside: järjekorrad ja teemade. Järjekorrad, kus on iga järjekorda saadetud sõnumi tarbitud ühe vastuvõtja. Teemade tugi, kus iga avaldatud sõnum on kättesaadav registreeritud teema tellimuse avalda/tellimise mustri. Iga tellimuse säilitab loogiliselt sõnumite oma järjekorda. Tellimuste konfigureerida filtri reeglid, mis piirata määramine sõnumite edasi tellimuse järjekorda, et need, mis vastavad filtri abil. Järgmises näites kasutatakse teenuse siini järjekorrad.

![][1]

See teatis süsteem on mitmeid eeliseid otsese sõnumside.

-   **Ajalise lahutamine.** Asünkroonne sõnumside mustriga tootjad ja tarbijad ei pea olema võrgus samal ajal. Teenuse siini usaldusväärselt talletab sõnumid kuni nõudvate tootja on valmis neid vastu võtta. See võimaldab jaotatud rakendus katkestatakse, kas vabatahtlikult, näiteks hooldus või tõttu komponent krahh, ilma mõjutavad kogu süsteemi komponendid. Lisaks nõudvate rakendus võib vaja ainult teatud ajal päeva veebis.

-   **Laadi ühtlustamise.** Paljudes rakendustes, süsteemi koormus sõltub aja jooksul, samas kui töötlemise ajal iga üksuse töö jaoks nõutavad on tavaliselt konstantne. Vahendamisel omandatud sõnumi tootjad ja järjekorda tarbijatele tähendab, et nõudvate rakenduste (töötaja) ainult peab olema ette valmistatud mahutamiseks Keskmine laadi, mitte tippväärtus laadi. Sügavus kuhjuda kasvab ja lepingute sissetulevate koormuse muutudes. See otse säästab raha poolest infrastruktuuri vajalikud rakenduse Laadi.

-   **Laadi tasakaalustamiseks.** Nagu koormus suureneb, saab lugeda järjekorda lisada mitme töötaja protsessi. Iga sõnumi töödeldakse ainult ühe töötaja protsesside. Lisaks selle tasakaalustamiseks pull-põhiste laadi võimaldab optimaalse kasutamise töötaja masinad isegi siis, kui töötaja masinad erinevad töötlemine power, nagu nad tõmbab sõnumite oma ülemmäära. See muster sageli nimetatakse *kiiruisutamises tarbija* mustri.

    ![][2]

Järgmistes lõikudes käsitletakse koodi, et rakendatakse see arhitektuur.

## <a name="set-up-the-development-environment"></a>Arengu keskkonna häälestamine

Enne alustamist Azure rakenduste arendamise, tööriistad ja häälestada oma arenduskeskkond.

1.  Installige Azure'i SDK .net-i [tööriistade ja SDK][]Get.

2.  Klõpsake Office'i versiooni te kasutate Visual Studio **SDK installimine** . Selle õpetuse juhised kasutada Visual Studio 2015.

4.  Kui teil palutakse käivitada või salvestada installiprogrammi, klõpsake nuppu **Käivita**.

5.  **Web platvormi Installeri**, klõpsake nuppu **Installi** ja installimist jätkata.

6.  Kui installimine on lõpule jõudnud, on teil kõik vajalik arendada rakenduse käivitamine. SDK sisaldab tööriistu, mille abil saate hõlpsalt töötada Azure rakendusi Visual Studio. Kui teil pole installitud Visual Studio, installib SDK ka tasuta Visual Studio Express.

## <a name="create-a-namespace"></a>Nimeruumi loomine

Järgmiseks on teenuse nimeruumi loomine ja ühiskasutusse Accessi allkirja (SAS) võti hankida. Nimeruumi pakub iga rakenduse kaudu teenuse siini esitatud rakenduse soovitud äärist. Nimeruumi loomisel luuakse SAS klahvi süsteem. Nimeruumi ja SAS klahvi kombinatsiooni pakub teenuse siini rakenduse juurdepääsu autentimiseks mandaadi.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Web rolli loomine

Selles jaotises saate luua rakenduse esiosa. Esmalt looge saate lehti, mis kuvab rakenduse.
Pärast seda, koodi, mis esitab teenuse siini järjekorda üksused ja kuvab olek teavet järjekorda lisada.

### <a name="create-the-project"></a>Projekti loomine

1.  Microsoft Visual Studio abil administraatoriõigused, käivitage. Visual Studio alustamiseks administraatoriõigustega Paremklõpsake **Visual Studio** programmi ikooni ja seejärel klõpsake käsku **Käivita administraatorina**. Azure Arvuta emulaator, mida selles artiklis käsitletakse nõuab Visual Studio käivitada administraatoriõigustega.

    Visual Studio, klõpsake menüüs **fail** nuppu **Uus**ja valige **Project**.

2.  Klõpsake **Cloud** **Installitud Mallid**jaotises **Visual C#**, ja seejärel klõpsake käsku **Azure'i pilveteenuses**. Projekti **MultiTierApp**nimi. Klõpsake nuppu **OK**.

    ![][9]

3.  **.NET Framework 4.5** rollid, topeltklõpsake **ASP.net-i Web roll**.

    ![][10]

4.  Libistage kursoriga üle **WebRole1** jaotises **Azure'i pilveteenuses lahenduse**, klõpsake ikooni pliiatsi ja Nimeta web roll **FrontendWebRole**. Klõpsake nuppu **OK**. (Veenduge, et sisestate "Frontend" on väiketähed "e," ei "FrontEnd".)

    ![][11]

5.  Klõpsake dialoogiboksis **Uus ASP.net-i projekti** **malli valimine** loendis **MVC**.

    ![][12]

6. Veel dialoogiboksis **Uus ASP.net-i projekt** nuppu **Muuda autentimist** . Dialoogiboksis **Muuda autentimist** **Ei autentimise**nuppu ja seejärel klõpsake nuppu **OK**. Selles õpetuses juurutamist rakendus, mis ei pea kasutaja sisselogimine.

    ![][16]

7. Tagasi **Uue ASP.net-i projekti** dialoogiboksis nuppu **OK** , et luua projekt.

6.  **Solution Exploreris** **FrontendWebRole** Projectis, paremklõpsake **Viited**ja seejärel nuppu **Halda NuGet-paketid**.

7.  Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus`. Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega.

    ![][13]

    Pange tähele, et nõutud klientrakenduse assemblereid on nüüd viidatud mõned uute failide koodi lisamist.

9.  **Solution Exploreris**, paremklõpsake **mudelite** ja klõpsake nuppu **Lisa**, seejärel klõpsake **ainekursuse**. Tippige **nimi** väljale nimi **OnlineOrder.cs**. Seejärel klõpsake nuppu **Lisa**.

### <a name="write-the-code-for-your-web-role"></a>Teie web rolli koodi kirjutamine

Selles jaotises saate luua rakenduse kuvab erinevate lehtede.

1.  Visual Studio failis OnlineOrder.cs asendage olemasolevad nimeruumi määratlus järgmine kood:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Topeltklõpsake **Solution Exploreris** **Controllers\HomeController.cs**. Lisage **abil** järgmistest kaasata ka teenuse siini äsja loodud mudeli nimeruumid faili ülaosas.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Ka HomeController.cs faili Visual Studios, asendage olemasolevad nimeruumi määratlus järgmine kood. Järgmine kood sisaldab käsitlemise üksuste esitamise järjekorda.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Klõpsake menüü **koostada** Testige oma töö täpsuse siiani **Lahenduse luua** .

5.  Nüüd looge vaate jaoks soovitud `Submit()` meetod, mis on varem loodud. Paremklõpsake selle `Submit()` meetod (ülekoormuse, `Submit()` , mis võtab pole parameetreid), ja seejärel valige **Lisa vaade**.

    ![][14]

6.  Kuvatakse dialoogiboks vaate loomiseks. Valige loendis **malli** **loomine**. Klõpsake loendis **mudeli klassi** **OnlineOrder** klassi.

    ![][15]

7.  Klõpsake nuppu **Lisa**.

8.  Nüüd, muuta oma rakenduse kuvatav nimi. **Solution Exploreris**, topeltklõpsake soovitud **Views\Shared\\_Layout.cshtml** faili avamiseks Visual Studio redaktoris.

9.  Asendage kõigi esinemiskordade **minu ASP.net-i rakenduse** **LITWARE'S tooted**.

10. Eemaldage **Home**, **kohta**ja **kontakti** lingid. Esiletõstetud koodi kustutamiseks tehke järgmist.

    ![][28]

11. Lõpuks muuta lehe kohta järjekorda lisada. Topeltklõpsake **Solution Exploreris** **Views\Home\Submit.cshtml** faili avamiseks Visual Studio redaktoris. Lisage pärast `<h2>Submit</h2>`. Nüüd on `ViewBag.MessageCount` on tühi. See on hiljem asustada.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Teil on rakendatud nüüd oma UI. Võite vajutada **klahvi F5** rakenduse käivitada ja veenduge, et see näeb välja nagu oletasime.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Teenuse siini järjekorda üksuste esitamiseks koodi kirjutamine

Nüüd lisada koodi esitamise üksuste järjekorda. Esmalt loote teenuse siini järjekorda ühenduseteavet sisaldava klassi. Lähtestage oma ühenduse Global.aspx.cs. Lõpuks värskendada esitamise koodi varem loodud tekstivormingus HomeController.cs tegelikult esitada üksused teenuse siini järjekorda.

1.  Paremklõpsake **Solution Exploreris** **FrontendWebRole** (Paremklõpsake projekt, mitte roll). Klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **klassi**.

2.  Klassi **QueueConnector.cs**nimi. Klõpsake nuppu **Lisa** klassi loomiseks.

3.  Nüüd lisage kood, mis kapseloi ühenduse teavet ja lähtestab ühendus teenuse siini järjekorda. Asendage QueueConnector.cs kogu sisu järgmine kood ja sisestage väärtused `your Service Bus namespace` (nimeruumi nimi) ja `yourKey`, mis on varem saadud Azure portaali **primaarvõti** .

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Nüüd, veenduge, et teie **lähtestada** meetod saab kutsuda. Topeltklõpsake **Solution Exploreris** **Global.asax\Global.asax.cs**.

5.  Lisage järgmine rida koodi **Application_Start** meetod lõpus.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Lõpetuseks, värskendage web kood varem loodud, esitada üksuste järjekorda. Topeltklõpsake **Solution Exploreris** **Controllers\HomeController.cs**.

7.  Värskenda selle `Submit()` meetod (ülekoormuse, mis pole parameetrid) järgmiselt, et saada sõnum count järjekorda.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Värskenda selle `Submit(OnlineOrder order)` meetod (ülekoormuse, mis võtab üks parameeter) järgmiselt esitada teavet järjekorda.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Nüüd saate rakendus uuesti. Iga kord, kui saadate tellimuse sõnumi arv suureneb.

    ![][18]

## <a name="create-the-worker-role"></a>Töötaja rolli loomine

Nüüd loote töötaja rolli, mis töötleb edastuste järjestuses. Selles näites kasutatakse **Töötaja rolli teenuse siini järjekorda** Visual Studio projekti malli. Juba saadud portaalist nõutud mandaatidega.

1. Veenduge, et Visual Studio on ühendatud Azure'i kontosse.

2.  Visual Studio, paremklõpsake **Solution** Exploreris **rollid** kausta **MultiTierApp** projekti all.

3.  Klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Uus töötaja rolli projekt**. Kuvatakse dialoogiboks **Lisamine uude rolli projekti** .

    ![][26]

4.  Klõpsake dialoogiboksis **Lisamine uude rolli projekti** **Töötaja rolli teenuse siini järjekorda**.

    ![][23]

5.  Väljale **nimi** nimi projekti **OrderProcessingRole**. Seejärel klõpsake nuppu **Lisa**.

6.  Kopeerige ühendusstring, mida te saada samm 9 "Luua teenuse siini nimeruum" jaotise lõikelauale.

7.  Paremklõpsake **Solution Exploreris** **OrderProcessingRole** 5 juhises loodud (Veenduge, et Paremklõpsake **OrderProcessingRole** jaotises **rollid**ja pole klassi). Seejärel klõpsake käsku **Atribuudid**.

8.  Dialoogiboksi **Atribuudid** vahekaardil **sätted** nuppu välja **väärtus** **Microsoft.ServiceBus.ConnectionString**ja seejärel kleepige kopeeritud juhist 6 lõpp-punkti väärtus.

    ![][25]

9.  Saate luua mõne **OnlineOrder** klassi muuta, et tellimused töötlete neid järjekorda. Saate kasutada juba loodud ainekursuse. Paremklõpsake **Solution Exploreris** **OrderProcessingRole** klassi (paremklõpsake ikooni ainekursuse, mitte roll). Klõpsake nuppu **Lisa**ja seejärel nuppu **Olemasolevad üksus**.

10. Liikuge sirvides alamkausta **FrontendWebRole\Models**jaoks, ja seejärel topeltklõpsake **OnlineOrder.cs** selle projekti lisamiseks.

11. **WorkerRole.cs**, muutke väärtus **QueueName** muutuja kaudu `"ProcessingQueue"` et `"OrdersQueue"` nagu on näidatud järgmine kood.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Lisage järgmine kasutamine lause WorkerRole.cs faili ülaosas.

    ```
    using FrontendWebRole.Models;
    ```

13. Rakenduses on `Run()` funktsioon sees on `OnMessage()` helistada, sisu asendada selle `try` klausel järgmine kood.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Rakendus on lõpule viidud. Saate rakenduse täisversioon testida paremklõpsake Solution Exploreris MultiTierApp projekt, valides **käivitus Projecti seatud**, ja seejärel vajutage klahvi F5. Pange tähele, et sõnum count inkrementida, kuna töötaja roll töötleb üksuste kuhjuda ja märgib nende lõpetatuks. Azure'i arvutada emulaator UI vaadates saate vaadata jälgimise väljund oma töötaja roll. Saate seda teha, paremklõpsates tegumiriba olekualal ikooni emulaator ja valides **Kuva arvutada emulaator UI**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Järgmised sammud  

Lisateavet teenuse siini, leiate järgmistest teemadest:  

* [Azure'i teenus siini][sbmsdn]  
* [Teenuse siini teenus][sbwacom]  
* [Kuidas kasutada teenuse siini järjekorrad][sbwacomqhowto]  

Mitmekihilise stsenaariumid kohta leiate lisateavet järgmistest teemadest.  

* [.Net-i mitmekihilise rakenduse salvestusruumi tabelid, järjekorrad ja plekid kasutamine][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Tööriistad ja SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  