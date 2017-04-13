<properties 
    pageTitle="Teenuse siini järjekorrad, teemasid ja tellimused | Microsoft Azure'i"
    description="Ülevaade teenuse siini sõnumside üksused."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Teenuse siini järjekorrad, teemasid ja tellimused

Microsoft Azure'i teenus siini toetab pilvepõhist, sõnumi rakendusse vahevara tehnoloogiale, sh, usaldusväärseid Teatejärjekorra ja püsival avaldamise/tellimise sõnumside. Neid vahendatud kiirsõnumside võimalusi saate vaadelda kui sidumata funktsioonid, mis tugi avaldada-Telli ning ajalised lahutamine ja laadi sõnumside struktuuri siini teenuse kasutamise stsenaariumid tasakaalustamiseks. Sidumata side on mitmeid eeliseid; näiteks klientide ja serverite saate ühendada vastavalt vajadusele ja toiminguid asünkroonne viisil.

Kiirsõnumside üksused teenuse siini vahendatud sõnumside võimaluste tuum moodustavaid on järjekorrad, teemade/tellimused ja reeglid/toimingud.

## <a name="queues"></a>Järjekorrad

Järjekorrad pakkumine esimese, esimese välja FIFO kohaletoimetamise ühe või mitme konkureerivate tarbijatele. Sõnumid on tavaliselt oodata vastu ja töödelda vastuvõtjad järjestuses, kus need olid varem lisatud kuhjuda ja iga sõnumi on saanud ja vaid ühe sõnumi tarbija töödelda. Kasutades järjekorrad põhieelis on "ajalised lahutamine" rakenduse komponentide. Teisisõnu, (saatjad) ja -tarbijate (vastuvõtjad) ei pea olema sõnumite saatmine ja vastuvõtmine samal ajal, et sõnumid talletatakse püsivalt järjekorda. Lisaks tootja ei pea ootama vastuse tarbija jätkamiseks töötlemine ja sõnumite saatmine.

Seotud kasu on "Laadi ühtlustamise," mis lubab tootjate ja tarbijate erinevate määrade sõnumite saatmiseks ja vastuvõtmiseks. Paljudes rakendustes, süsteemi koormus muutub aja jooksul; siiski töötlemise ajal iga üksuse töö jaoks nõutavad on tavaliselt konstantne. Vahendamisel omandatud sõnumi tootjad ja järjekorda tarbijatele tähendab, et nõudvate rakenduse ainult peab olema ette valmistatud saama toime Keskmine laadi asemel tippväärtus laadi. Sügavus kuhjuda kasvab ja lepingute sissetulevate koormuse muutudes. See otse säästab raha summa taristu vajalikud rakenduse Laadi suhtes. Kui koormus suureneb, saab lugeda järjekorda lisada mitme töötaja protsessi. Iga sõnumi töödeldakse ainult ühe töötaja protsesside. Lisaks selle tasakaalustamiseks pull-põhiste laadi võimaldab optimaalse kasutamise töötaja arvutites isegi kui töötaja arvutites erinevad seoses töötlemise võimsus, nad tõmbab sõnumite oma ülemmäära. See muster on sageli nimetatakse "kiiruisutamises tarbija" mustri.

Järjekorda töötluse ootele vahe sõnumi tootjate ja tarbijate vahel kasutamine tagab komponendid on omased lahti ühendada. Kuna tootjad ja tarbijad ei tea üksteisest, saab tarbija uuendada ilma mingit mõju tootja.

Loomine järjekorda on mitme järgmist toimingut. Teenuse siini sõnumside üksuste (järjekorrad ja teemade) jaoks haldamise toiminguid teha [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) ainekursust, mis on ehitatud baasaadressi teenuse siini nimeruumi ja kasutajale mandaadi sisestamise teel. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) pakub võimalust luua, loetlemine ja sõnumside üksuste kustutamine. Pärast loomist SAS nime ja klahvi teenuse nimeruumi halduse objekti [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objekti saate [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) meetodi abil luua järjekorda. Näiteks:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Seejärel saate luua järjekorda objekti ja sõnumside factory teenuse siini URI argumendina. Näiteks:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Seejärel saate sõnumeid saata järjekorda. Näiteks, kui teil on nimega vahendatud sõnumite loendi `MessageList`, kood kuvatakse umbes järgmine:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Seejärel saate sõnumeid järjekorda, järgmiselt:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Režiimis [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) vastuvõtmise toiming on ühe-shot; See tähendab, kui teenuse siini saab taotluse, see märgib sõnumi nagu tarbitud ja tagastab selle rakenduse. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) režiim on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini märgib sõnumid tarbitud, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, vastamata see sõnum, mida on kasutatud enne krahhi.

[PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) režiimis, vastuvõtmise toiming muutub kahe etapi, mis võimaldab tugi rakendustele, mistõttu puuduvad sõnumeid. Kui teenuse siini saab taotluse, leiab see olla tarbimine järgmise sõnumi, lukustab muude tarbijate takistamine seda vastu, ja tagastab rakenduse. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab teise etapi vastuvõtu protsessi [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) helistades vastuvõetud sõnumi. Kui teenus siini näeb [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) helistada, see märgib sõnumid tarbitud.

Kui rakendus ei saa sõnumit töödelda mingil põhjusel, seda saate helistada [hülgama](https://msdn.microsoft.com/library/azure/hh181837.aspx) meetodit vastuvõetud sõnumi (mitte [täielik](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). See võimaldab teenuse siini sõnumi avamiseks ja saanud uuesti sama tarbija või mõne muu konkureerivate tarbija jaoks kättesaadavaks teha. Teiseks on seostatud lukustus ajalõpp ja kui taotlus ei töödelda sõnumi lukustus enne aegumist ajalõpp (näiteks siis, kui rakendus jookseb), siis teenuse avab sõnumi ja teeb selle kättesaadavaks uuesti vastu võtta (sisuliselt [loobuda](https://msdn.microsoft.com/library/azure/hh181837.aspx) toimingute tegemise vaikimisi).

Pange tähele, et juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) kutse, sõnum on taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli *Vähemalt ühe korra *töötlemine; mis on iga sõnumi töötlemise vähemalt ühe korra. Teatud juhtudel võib sama sõnumit siiski taastarnimiseni. Kui seda stsenaariumi ei saa luba dubleeritud töötlemise, siis täiendavad loogika on vaja tuvastamiseks **MessageId** atribuut, mis jääb samaks üle saatmiskatsete põhineb duplikaadid, mida saab teha rakenduses. Seda nimetatakse *Täpselt pärast* töötlemine.

Lisateabe saamiseks ja töötamise näide sellest, kuidas luua ja saata sõnumeid ja järjekorrad kaudu leiate teemast [teenuse siini vahendajaks sõnumside .net-i õpetus](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Teemade ja tellimused

Vastupidiselt järjekorrad, kus iga sõnumi töödeldakse ühe tarbija *teemade* ja *tellimuste* sisestage vormile üks-mitmele teatise *avaldamise/tellimise* muster. Kasulik mastaapimise väga suure hulga adressaadid, iga avaldatud sõnum on kättesaadav iga tellimuse registreeritud teema. Sõnumite teemale saatmine ja saamine ühe või mitme seotud tellimuste sõltuvalt filtri reeglid, mis saate määrata tellimuse kohta alusel. Tellimused saate kasutada filtreid sõnumid, mida nad saavad piirata. Sõnumid saadetakse teemale samamoodi, nagu need saadetakse järjekorda, kuid mitte saadetud meilisõnumid teema otse. Selle asemel saadud tellimused. Teema: tellimuse sarnaneb virtuaalse järjekorda, mida saab teema saadetud sõnumite koopiad. Saadetud meilisõnumid tellimuse ühtemoodi nii, nagu need on saadud järjekorda.

Võrdluse, sõnumi saatmine funktsionaalsust järjekorda kaardid otse teema ja oma sõnumi vastuvõtmine funktsionaalsuse kaardid tellimus. Muu hulgas, see tähendab, et tellimuste toetavad seoses järjekorrad selles jaotises eespool kirjeldatud sama mustrid: konkureerivate tarbija, ajaline lahutamine, laadi ühtlustamise ja koormusetasakaalustuseks.

Loote teema on sarnane loomise järjekorda, nagu on näidatud eelmises jaotises. Looge teenuse URI ja luua nimeruumi kliendi [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) klassi abil. Seejärel saate luua teema [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) meetodi abil. Näiteks:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Järgmisena lisage tellimuste vastavalt soovile.

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Seejärel saate luua teema kliendi. Näiteks:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Kasutades sõnumi saatja, saate saata ja vastu võtta sõnumeid ja teema, nagu on näidatud eelmises jaotises. Näiteks:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Sarnane järjekorrad saadetud meilisõnumid tellimuse kasutamise asemel [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) objekti [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objekt. Saate luua kliendi tellimuse, läbides teema nimi, tellimuse ja (vajadusel) vastuvõtu režiimi nime parameetrid. Näiteks koos **varude** tellimus:

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Reeglid ja toimingud

Mitme stsenaariumi töödelda sõnumid, mis on teatud omadused erinevatel viisidel. Selle saate konfigureerida tellimused, et leida sõnumid, mis on soovitud atribuudid ja seejärel nende atribuutide teatud muudatusi teha. Ajal teenuse siini tellimuste näha kõiki sõnumeid, mis saadetakse teema, saate kopeerida ainult need sõnumid alamhulk virtuaalse tellimuse järjekorda. Seda saab teha tellimuse filtrite abil. Muudatustest, nimetatakse *filtri toimingud*. Tellimuse loomisel võite pakkuda filtriavaldis, mis toimib atribuutide sõnumis nii Süsteemiatribuudid (nt **silt**) ja kohandatud rakenduse atribuutide (nt **väli StoreName**.) SQL-i filtri avaldisel on valikuline sel juhul; ilma SQL-i filtriavaldise tehakse midagi filter tellimuse jaoks määratletud kõik sõnumid selle tellimuse jaoks.

Kasutades eelmises näites filter sõnumitele, mis on pärit ainult **Store1**, luua armatuurlaua tellimuse järgmiselt:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Selle tellimuse filtriga kohas, ainult sõnumid, mis on selle `StoreName` atribuudi `Store1` kopeeritakse virtuaalse järjekorda jaoks soovitud `Dashboard` tellimus.

Võimalikud filter väärtuste kohta lisateabe saamiseks dokumentatsioonist [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) ja [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) tunnid. Vaadake selle [vahendajaks sõnumside: täpsemaid filtreid](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) ja [Teema filtrid](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) näidised.

## <a name="next-steps"></a>Järgmised sammud

Vaadake lisateavet teenuse siini vahendajaks sõnumside üksuste kasutamise näited ja kogenud järgmist.

- [Teenuse siini sõnumside ülevaade](service-bus-messaging-overview.md)
- [Teenuse siini vahendajaks sõnumside .net-i õpetus](service-bus-brokered-tutorial-dotnet.md)
- [Teenuse siini vahendajaks sõnumside ülejäänud õppeteema](service-bus-brokered-tutorial-rest.md)
- [Teema: filtrite näidis](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Vahendatud sõnumside: Täpsemad filtrid näidis](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

