<properties
   pageTitle="Optimeerida oma Azure kood Visual Studio | Microsoft Azure'i"
   description="Siit saate teada, kohta, kuidas Azure'i koodi optimeerimine tööriistad Visual Studio aidata oma koodi rohkem robustne ja paremini toimiv."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Azure'i koodi optimeerimine

Kui olete programmeerimise rakendused, mis kasutavad Microsoft Azure'i, on mõned kodeerimise tavad peate järgima rakenduse skaleeritavus, käitumine ja jõudluse cloud keskkonnas probleemide vältimiseks. Microsoft osutab Azure'i koodi analüüsi tööriist, mis tuvastab ja tuvastab mitut neid tavaliselt ilmnenud probleemid ja aitab teil need lahendada. Tööriista Visual Studio Nugeti kaudu saate alla laadida.

## <a name="azure-code-analysis-rules"></a>Azure'i analüüs reeglid

Azure'i koodi analüüsi tööriist kasutab järgmisi reegleid oma Azure koodi automaatselt lipuga märkimiseks, kui see leiab jõudlust mõjutavad teadaolevad probleemid. Tuvastatud probleeme kuvada kujul on hoiatused või koostaja tõrkeid. Koodiparandusi või soovituste hoiatuse või tõrke lahendamiseks antakse sageli lihtversioon pirnid ikooni kaudu.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vältige kasutamist vaikerežiim (protsessi) seansi olek

### <a name="id"></a>ID

AP0000

### <a name="description"></a>Kirjeldus

Kui kasutate (protsessi) seansi oleku vaikerežiim pilve rakendused, kaotate seansi olek.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Vaikimisi on määratud fail seansi olek režiimi-protsess. Lisaks, kui pole määratud konfiguratsioonifailis kirje seansi olek režiimi vaikimisi protsess. Protsessi režiimi salvestab seansi olek mälu veebiserverisse. Kui näiteks taaskäivitamist või uue eksemplari kasutatakse laadi tasakaalustamiseks või Tõrkesiirde tugi, mällu veebiserverisse seansi olek ei salvestata. Olukord takistab rakendus on scalable pilve.

ASP.net-i seansi olek toetab mitut eri talletamise võimalused seansi olek andmete jaoks: InProc, StateServer, SQLServer kohandatud ja väljas. Soovitatav on kasutada kohandatud režiimi host andmete mõne välise seansi olek poes, nt [Azure seansi olek pakkuja Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Lahendus

Üks soovitatav lahendus on salvestama hallatavate vahemälu teenuse seansi olek. Saate teada, kuidas [Azure'i seansi olek pakkuja Redis](http://go.microsoft.com/fwlink/?LinkId=401521) abil saate talletada oma seansi olek. Seansi olekut saate salvestada ka muudes kohtades tagamaks, et teie taotlus on scalable pilve. Lisateavet asemel lahenduste lugege [Seansi olek režiimi](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Käivita meetod ei tohiks asünkroonse

### <a name="id"></a>ID

AP1000

### <a name="description"></a>Kirjeldus

Luua asünkroonne viise (nt [ootavad](https://msdn.microsoft.com/library/hh156528.aspx)) väljaspool [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetodit ja seejärel kõne asünkroonse meetodite [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklareerimise [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetod, nagu asünkroonse põhjustab töötaja roll esitatavaks uuesti sisestada.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Helistamiseks asünkroonse meetodite [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetodiga põhjustab teenus cloud runtime prügikastis töötaja roll. Töötaja roll käivitamisel kõik programmi täitmise toimub [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetodiga. Väljumist [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetodit põhjustab töötaja rolli taaskäivitama. Kui töötaja rolli käitusaja tabab asünkroonse meetod, saadab kõik toimingud pärast asünkroonse meetodit ja tagastab. See põhjustab töötaja roll [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetod ja uuesti käivitada. Järgmise iteratsiooni täitmise, töötaja roll tabab asünkroonse meetodit uuesti ja taaskäivitamist, mis põhjustavad taaskasutada uuesti kasutada ka töötajal roll.

### <a name="solution"></a>Lahendus

Viige kõik asünkroonse toimingud väljaspool [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetod. Seejärel helistage refactored asünkroonse meetodi [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetodiga, nt oota RunAsync (). Azure'i koodi analüüsi tööriist aitab teil selle probleemi lahendamiseks.

Järgmised koodilõigu näitab koodi määrata probleemi lahendamiseks:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Teenuse siini ühiskasutusse Accessi allkirja autentimist kasutades

### <a name="id"></a>ID

AP2000

### <a name="description"></a>Kirjeldus

Kasuta autentimiseks ühiskasutusse Accessi allkirja (SAS). Teenuse siini autentimine on on aegunud Access Control teenus (ACS).

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Turvalisuse Azure Active Directory on asendades ACS autentimise SAS autentimist. Lepingule ülemineku kohta teavet teemast [Azure Active Directory on ACS tulevikus](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) .

### <a name="solution"></a>Lahendus

Kasutage rakenduste SAS autentimist. Järgmises näites kujutatakse kasutada mõne olemasoleva SAS luba teenuse siini nimeruumi või üksuse juurde pääseda.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Lisateavet järgmistest teemadest.

- Saate ülevaate leiate [Ühiskasutusse Accessi allkirja autentimine teenuse siini](https://msdn.microsoft.com/library/dn170477.aspx)

- [Kuidas kasutada teenuse siini ühiskasutusse Accessi allkirja autentimine](https://msdn.microsoft.com/library/dn205161.aspx)

- Valimi projekti, leiate teemast [teenuse siini tellimuste abil ühiskasutusse Accessi allkirja (SAS) autentimine](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Kaaluge OnMessage meetod vältimiseks "saada tsükkel"

### <a name="id"></a>ID

AP2002

### <a name="description"></a>Kirjeldus

Et vältida läinud "saada tsükkel", **OnMessage** meetodi on parem lahendus kui **vastuvõtu** meetodi sõnumeid. Siiski peab **vastuvõtu** meetodit kasutada siis, kui määrate vaike serveri ootama aega, veenduge, et server ajal on rohkem kui üks minut.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Kui helistate **OnMessage**, käivitab kliendi sisemise sõnumi pump, mis pidevalt küsitlused järjekorda või tellimuse. See sõnum pump sisaldab silmuses probleemid kõne sõnumeid. Kui kõne ajalõpp, probleemid selle uue kõne. Ajalõpp intervalli määrab [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx), mida kasutatakse [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) atribuudi väärtust.

**OnMessage** võrreldes **vastuvõtu** kasutamise eeliseid on, et kasutajad ei pea käsitsi küsitlus sõnumite jaoks, toime erandid, töötlemine samaaegselt mitu sõnumit ja täitke sõnumid.

Kui helistate **vastuvõtu** vaikeväärtus kasutamata, kindlasti *ServerWaitTime* väärtus on rohkem kui üks minut. Säte *ServerWaitTime* rohkem kui üks minut takistab server enne sõnumi täielikult ajastus.

### <a name="solution"></a>Lahendus

Lugege järgmised koodi näited jaoks soovitatavad tavadega. Lisateavet leiate teemast [QueueClient.OnMessage meetodit (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)ja [QueueClient.Receive (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Teemast Azure sõnumside infrastruktuuri jõudluse parandamiseks kujundus mustri [Asünkroonne sõnumside Primer](https://msdn.microsoft.com/library/dn589781.aspx).

Järgmises näites **OnMessage** abil saate sõnumeid.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Järgmises näites kasutades **vastuvõtu** Vaikeaja serveri ootamine.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Järgmises näites kasutamise **vastuvõtu** aega-vaikimisi serveri ootamine.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Kaaluge asünkroonne teenus siini meetodid

### <a name="id"></a>ID

AP2003

### <a name="description"></a>Kirjeldus

Asünkroonne teenus siini meetodite abil saate parandada jõudlust vahendatud sõnumside.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Asünkroonne meetoditega võimaldab rakenduse programmi kokkulangevus, kuna käivitamisel iga kõne ei blokeerida peamine teema. Kui kasutate teenuse siini sõnumside meetodid, toimingute tegemise (saatmine, vastuvõtmine, kustutamine, jne) võtab aega. Seekord sisaldab teenuse siini teenuse latentsus taotluse ja vastuse Lisaks toimingu töötlemine. Arv, korraga suurendamiseks toimingute käivitada samaaegselt. Lisateabe saamiseks vaadake [Head tavad jõudluse täiustused abil teenuse siini vahendajaks sõnumside](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Lahendus

Siis soovitatav asünkroonne meetodi kasutamise kohta lisateabe saamiseks vt [QueueClient klassi (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Teemast Azure sõnumside infrastruktuuri jõudluse parandamiseks kujundus mustri [Asünkroonne sõnumside Primer](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Kaaluge eraldamine teenuse siini järjekorrad ja teemad

### <a name="id"></a>ID

AP2004

### <a name="description"></a>Kirjeldus

Sektsiooni teenuse siini järjekorrad ja teemade parema jõudluse teenuse siini sõnumside.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Jõudluse läbilaskevõime ja teenuse kättesaadavus eraldamine teenuse siini järjekorrad ja teemade tõus, sest üldine jõudlus sektsioonitud järjekorda või teema on enam piiratud ühe sõnumi ta või kiirsõnumside pood. Lisaks ajutiste katkestuste sõnumside poe ei tee sektsioonitud järjekorda või teema pole saadaval. Lisateavet leiate teemast [Eraldamine sõnumside üksuste](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Lahendus

Järgmised koodilõigu kujutatakse partition sõnumside üksused.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Lisateabe saamiseks vt [liigendatud teenuse siini järjekorrad ja teemasid | Microsoft Azure'i ajaveebi](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) ja tutvuge [Microsoft Azure'i teenus siini liigendatud järjekorda](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) valimi.

## <a name="do-not-set-sharedaccessstarttime"></a>Määrake SharedAccessStartTime

### <a name="id"></a>ID

AP3001

### <a name="description"></a>Kirjeldus

Tuleks vältida, SharedAccessStartTimeset abil praeguse kellaaja kohe alustada ühiskasutusse antud juurdepääs poliitika. Peate ainult atribuut, kui soovite alustada hiljem ühiskasutusse antud juurdepääs poliitika.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Kella sünkroonimise põhjustab veidi aega erinevuse andmekeskuste vahel. Näiteks soovite loogiliselt arvate storage SAS poliitika algusaja seadmine praeguse kellaaja, kasutades DateTime.Now või sarnanevad põhjustab SAS poliitika jõustuvad kohe. Siiski veidi aega erinevused andmekeskuste, võivad põhjustada probleeme selle Kuna andmekeskuse mõnikord võib olla veidi hilisem kui algusaeg, teised aga see. Seetõttu saab SAS poliitika aegumise kiiresti (või isegi kohe) kui eluiga poliitika on seatud liiga lühike.

Veel ühiskasutusse antud juurdepääs allkirja Azure Storage kasutamise kohta vaadake teemast [Tutvustus tabeli SAS (ühiskasutusse Accessi allkiri), järjekorda SAS ja bloobimälu SAS – Microsoft Azure'i salvestusruumi meeskonna ajaveeb - saidi Avaleht - MSDN-i ajaveebide värskendus](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lahendus

Lause, mis määrab ühiskasutusega juurdepääsu poliitika algusaja eemaldada. Azure'i koodi analüüsi tööriist hõlmab Paranda probleemile. Turvalisuse haldamise kohta leiate lisateavet teemast kujundus mustri [Valet klahvi mustri](https://msdn.microsoft.com/library/dn568102.aspx).

Järgmised koodilõigu näitab koodi määrata probleemile.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Ühiskasutusse antud juurdepääs poliitika kehtivuse aeg peab olema rohkem kui 5 minuti

### <a name="id"></a>ID

AP3002

### <a name="description"></a>Kirjeldus

Võib olla nii palju, kui viie minuti erinevus kella vahel andmekeskuste erinevates kohtades tõttu tingimuse tuntud "kella skew." Et vältida muude poliitika luba, mis lõpeb varem, seada olla rohkem kui 5 minuti.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Erinevates kohtades kogu maailmas andmekeskuste sünkroonima kell signaal. Kuna see võtab aega kell signaal sõitma eri kohti, võib olla aeg dispersioon vahel andmekeskuste veebisaidil erinevates kohtades kuigi arvatavasti sünkroonitakse kõik. Aja erinevus võib mõjutada ühiskasutusse antud juurdepääs poliitika käivitamise aja ja aegumise intervalli. Seetõttu juurdepääs ühiskasutusse antud poliitika jõustub kohe tagamiseks ei Määrake algusaeg. Lisaks veenduge, et aegumise aeg on rohkem kui 5 minutit, et vältida ennetähtaegse ajalõpp.

Ühiskasutusse antud juurdepääs allkirja Azure salvestusruumi kasutamise kohta leiate lisateavet teemast [Tutvustus tabeli SAS (ühiskasutusse Accessi allkiri), järjekorda SAS ja bloobimälu SAS – Microsoft Azure'i salvestusruumi meeskonna ajaveeb - saidi Avaleht - MSDN-i ajaveebide värskendus](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lahendus

Turvalisuse haldamise kohta leiate lisateavet teemast kujundus mustri [Valet klahvi mustri](https://msdn.microsoft.com/library/dn568102.aspx).

Järgmises näites pole märgitud ühiskasutusse antud juurdepääs poliitika algusaeg.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Järgmises näites täpsustada ühiskasutusse antud juurdepääs poliitika algusaeg poliitika aegumise kestus suurem kui 5 minuti.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Lisateabe saamiseks vt [loomine ja kasutamine ühiskasutusse Accessi allkirja](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Kasutage CloudConfigurationManager

### <a name="id"></a>ID

AP4000

### <a name="description"></a>Kirjeldus

Projektide, nagu Azure'i veebisaidi ja Azure mobiilside [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) klassi kasutamise ei Tutvustage käitusaja probleemid. Hea tava, aga on hea mõte kasutada Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) ühtne viis konfiguratsioone kõigi Azure pilveteenuse rakenduste haldamine.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

CloudConfigurationManager loeb asjakohane rakenduse keskkonna konfiguratsioon faili.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Lahendus

Refaktoorime kasutada [CloudConfigurationManager klassi](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)oma koodi. Azure'i kood analüüsitööriist pakub kood lahendus selle probleemi.

Järgmised koodilõigu näitab koodi määrata probleemile. Asendamine

`var settings = ConfigurationManager.AppSettings["mySettings"];`

abil

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Siin on näide sellest, kuidas säilitada konfiguratsiooni säte App.config või Web.config failis. Lisada konfiguratsiooni faili appSettings jaotis Sätted. Allpool on näiteks eelmise koodi fail.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Kõva kodeeritud ühendusstringi kasutamise vältimine

### <a name="id"></a>ID

AP4001

### <a name="description"></a>Kirjeldus

Kui kasutate raske koodiga ühendusstringi ja soovite need hiljem värskendada, siis on teil muuta oma lähtekoodi ja Kompileeri rakendus. Juhul, kui salvestate oma ühendusstringi konfiguratsioonifailis, muutmiseks neid hiljem, värskendades lihtsalt konfiguratsiooni faili.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Suur kodeerimise ühendusstringi halb tava on, kuna see tutvustab probleeme, kui ühendusstringi on vaja kiiresti muuta. Lisaks, kui projekt tuleb sisse möllida andmeallika juhtelemendi, raske koodiga ühendusstringi tutvustamine turvahaavatavusi Kuna stringid saab vaadata lähtekoodi.

### <a name="solution"></a>Lahendus

Ühendusstringi talletatakse failid või Azure keskkonnas.

- Autonoomse rakenduste, kasutage app.config stringi ühendusesätete talletamiseks.

- Majutatud IIS-i veebirakenduste kasutada web.config ühendusstringi talletamiseks.

- ASP.net-i vNext rakenduste, kasutage configuration.json ühendusstringi talletamiseks.

Näiteks web.config või app.config konfiguratsioone failide kasutamise kohta leiate teavet teemast [ASP.net-i Web konfigureerimise juhised](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Kuidas Azure keskkonna muutujate töö kohta leiate teavet teemast [Azure veebisaitide: Kuidas rakenduse stringide ja ühenduse stringide töö](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Ühendusstringi talletamine andmeallika juhtelemendi kohta leiate teavet teemast [Ärge tundlikku teavet nagu ühendusstringi failid, mis on talletatud lähtekoodi hoidla](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Kasutage diagnostika konfiguratsioonifail

### <a name="id"></a>ID

AP5000

### <a name="description"></a>Kirjeldus

Asemel diagnostika sätete konfigureerimise abil Microsoft.WindowsAzure.Diagnostics, programmeerimisega API näiteks koodi, peaksite konfigureerima diagnostika sätete diagnostics.wadcfg faili. (Või kui kasutate Azure SDK 2,5 diagnostics.wadcfgx). Seda tehes saate muuta diagnostika sätete ilma Kompileeri kood.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

Enne Azure'i SDK 2,5 (mis kasutab Azure diagnostika 1.3), Azure'i diagnostika WAD võib konfigureeritud, kasutades mitmeid erinevaid viise: lisamine konfiguratsiooni bloobimälu hoidlas, olulistel kood, deklaratiivseid konfiguratsiooni või vaikimisi konfiguratsiooni abil. Eelistatud võimalus konfigureerimine diagnostika siiski kasutada XML-konfiguratsioonifail (diagnostics.wadcfg või diagnositcs.wadcfgx SDK 2,5 ja uuemad versioonid) rakenduse Projectis. Selle stsenaariumi diagnostics.wadcfg faili täielikult määratleb konfiguratsiooni ja saab värskendada ja ümberpaigutatud saab. Diagnostics.wadcfg konfiguratsioonifail kasutamine segamist programmeerimismeetodite säte konfiguratsiooni [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)või [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)klassi abil saate tekitada segadust. Lisateabe saamiseks vaadake [lähtestamine või muutmine Azure'i diagnostika konfigureerimine](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Alates WAD 1.3 (kaasas Azure'i SDK 2,5), ei ole enam võimalik koodi abil saate konfigureerida diagnostika. Selle tulemusena saate ainult sisestada konfiguratsiooni rakendamine või värskendamisel diagnostika laiendamine.

### <a name="solution"></a>Lahendus

Konfiguratsioonifail diagnostika (diagnositcs.wadcfg või diagnositcs.wadcfgx SDK 2,5 ja uuemad versioonid) diagnostikasätete liikumiseks kasutage diagnostika konfiguratsiooni designer. Soovitatav on ka [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) installida ja kasutada uusimat diagnostika funktsiooni.

1. Kiirmenüü rolli, mida soovite konfigureerida, valige Atribuudid ja seejärel menüüd konfigureerimine.

1. Jaotises **diagnostika** veenduge, et oleks märgitud ruut **Luba diagnostika** .

1. Klõpsake nuppu **Konfigureeri** .

  ![Juurdepääs suvand Luba diagnostika](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Lisateavet leiate [Azure'i pilveteenustega ja Virtuaalmasinates diagnostika konfigureerimine](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vältige deklareerimise staatilise DbContext objektid

### <a name="id"></a>ID

AP6000

### <a name="description"></a>Kirjeldus

Vältige mälu salvestamiseks deklareerimise staatilise DBContext objektid.

Palun jagage oma ideid ja [Azure analüüs tagasiside](http://go.microsoft.com/fwlink/?LinkId=403771)tagasisidet.

### <a name="reason"></a>Põhjus

DBContext objektide all iga kõne päringu tulemusi. Staatilise DBContext objektide kõrvaldatakse pole seni, kuni rakenduse domeen on maha. Seetõttu staatilise DBContext objekti saab kasutada suurel hulgal mälu.

### <a name="solution"></a>Lahendus

Deklareerida DBContext kohaliku muutuja või-staatiline eksemplari välja, kasutamiseks tööülesande ja seejärel laske kõrvaldada pärast kasutamist.

MVC kontrolleril klassi järgmises näites on kujutatud DBContext objekti kasutamise kohta.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Järgmised sammud

Optimzing ja Azure rakendused tõrkeotsingu kohta leiate lisateavet teemast [tõrkeotsing web appi teenuses Azure rakenduse abil Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
