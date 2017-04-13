<properties 
    pageTitle="Saate luua rakendusi, mis kasutavad teenuse siini teemade ja tellimused | Microsoft Azure'i"
    description="Sissejuhatus avaldamine – tellida teenuse siini teemade ja tellimuste pakutavaid võimalusi."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Saate luua rakendusi, mis kasutavad teenuse siini teemade ja tellimused

Azure'i teenus siini toetab pilvepõhist, sõnumi suunatud vahevara tehnoloogiale, sealhulgas, usaldusväärseid Teatejärjekorra ja püsival avaldamise/tellimise sõnumside. Selles artiklis koostab [loomine](service-bus-create-queues.md) rakendustes, mis kasutavad teenuse siini järjekorrad teabe ja pakub pakutud teenuse siini teemade avalda/tellimise võimaluste tutvustus.

## <a name="evolving-retail-scenario"></a>Päevakohaste jaemüügi stsenaarium

See artikkel ei lahene jaemüügi stsenaarium, mis on kasutusel [loomine rakendustes, mis kasutavad teenuse siini järjekorrad](service-bus-create-queues.md). Tagasi kutsuda müügi andmeid: üksikute punkti, müük (POS) terminalide peab marsruuditakse laoseisu haldamise süsteem, mis kasutab andmeid kindlaks teha, kui stock täiendada. Iga kassaterminal aruandeid oma müügiandmeid sõnumeid saata **DataCollectionQueue** järjekorda, kus need jäävad alles, kuni nad kätte süsteemi, nagu järgmisel joonisel:

![Teenuse siini 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Selle stsenaariumi muutuda, on lisatud uus nõue süsteemiga: poe omanik soovib jälgida, kuidas pood esinemas reaalajas.

Selle nõude käsitlema süsteemi peab "koputage" müügi andmevoos välja. Soovime siiski iga saadetud POS terminalides süsteemi nagu enne saata sõnum, kuid soovime me abil saate esitada armatuurlaua vaate poe omanik iga sõnumi koopia.

Mis tahes olukorras, nagu see, mida vajate iga sõnumi kaupa mitme isikutele, saate teenuse siini *teemade*. Teemadest avalda/tellimise mustri, kus iga avaldatud sõnum on kättesaadav registreeritud teema üks või mitu tellimust. Seevastu järjekorrad koos iga sõnum ühe tarbija.

Sõnumid saadetakse teema samamoodi nagu need saadetakse järjekorda. Siiski ei saadetud meilisõnumid teema otse; saadud tellimused. Kui arvate, tellimuse teemale nimega virtuaalse järjekorda, mida saab selle teema saadetud sõnumite koopiad. Saadetud meilisõnumid tellimuse samamoodi nagu saadud järjekorda.

Naasmine jaemüügi stsenaarium kuhjuda asendatakse teema ja tellimuse lisanud, mille saate laoseisu haldamise süsteemi komponent. Kuvatakse süsteemi järgmiselt:

![Teenuse siini 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Siin konfiguratsiooni teostab ühtemoodi eelmise järjekorda vastavalt kujunduse. Teema saadetud marsruuditakse **varude** tellimus, millest **Süsteemi** tarbib neid.

Halduse armatuurlaua toetamiseks loome teise tellimuse teemal, nagu järgmisel joonisel:

![Teenuse siini 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Selle konfiguratsiooni puhul iga sõnumi POS automaatidest tehakse kättesaadavaks **armatuurlaual** ja **varude** tellimused.

## <a name="show-me-the-code"></a>Kuva kood

[Loo rakendusi, mis kasutavad teenuse siini järjekorrad](service-bus-create-queues.md) artiklis kirjeldatakse Azure'i konto jaoks registreeruda ja luua teenuse nimeruumi. Teenuse siini nimeruum kasutamiseks peab taotluse viide teenuse siini komplekti, spetsiaalselt Microsoft.ServiceBus.dll. Lihtsaim viis viidata teenuse siini sõltuvused on installimiseks teenuse siini [Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Leiate komplekti osana Azure'i SDK. Allalaadimine on saadaval veebisaidil [Azure'i SDK allalaadimislehele](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Teema ja tellimuste loomine

Halduse teenuse siini sõnumside üksuste (järjekorrad ja avaldamise/tellimise teemade) jaoks tehete [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) klassi kaudu. Sobiv mandaate on vaja luua mõne konkreetse nimeruumi [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) eksemplar. Teenuse siini kasutab mudeli [Ühiskasutusse Accessi allkirja (SAS)](service-bus-sas-overview.md) vastavalt turvalisus. Klassi [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) tähistab Turbeloa pakkujat sisseehitatud factory meetoditega esitus mõned tuntud Turbeloa pakkujad. Kasutame [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) meetodi ootelepanekuks SAS mandaat. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) eksemplar on seejärel ehitatud baasaadressi teenuse siini nimeruum ja Turbeloa pakkuja.

Klassi [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) pakub võimalust luua, loetlemine ja sõnumside üksuste kustutamine. Mis on siin näidatud koodi näitab, kuidas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) eksemplar on loodud ja **DataCollectionTopic** teema loomiseks kasutatud.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Pange tähele, et ülekoormuse [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) meetod, mis võimaldavad teil määrata teema atribuudid. Näiteks saate määrata aja live (TTL) vaikeväärtuse teema saadetud sõnumid. Järgmisena lisage **varude** ja **armatuurlaua** tellimused.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Sõnumite saatmiseks teema

Käitusaja toimingute teenuse siini üksustele; näiteks sõnumite saatmine ja vastuvõtmine, rakenduse peate esmalt looma [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekti. Sarnaselt [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) ainekursust, luuakse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) eksemplari baasaadressi teenuse nimeruumi ja Turbeloa pakkuja kaudu.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Sõnumite saatmiseks ja saadud teenuse siini teemad, on [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) klassi eksemplarid. See tund koosneb kogumi standardatribuudid (nt [silt](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), hoidke rakenduse atribuutide ja keha suvalise rakenduse andmete kasutatava sõnastiku. Rakenduse saate määrata mis tahes sarjadesse jaotatav objekt (järgmises näites edastab **SalesData** objekti, on kassaterminal müügiandmete tähistava), mille keha, mis kasutab [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) serialiseerida objekti. Teise võimalusena saate esitada [voo](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) objekti.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Lihtsaim viis sõnumeid saata teema on kasutada [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) objekti loomiseks otse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) eksemplari.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Sõnumeid, mis tellimusest

Sarnane järjekordade abil saate kasutada [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekti, otse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)abil loodavad tellimuse sõnumeid saada. Saate valida ühe kaks erinevat režiimi (**ReceiveAndDelete** ja **PeekLock**) vastu võtta, nagu [loomine](service-bus-create-queues.md)rakendustes, mis kasutavad teenuse siini järjekorrad.

Pange tähele, et [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) tellimuste loomisel *entityPath* parameetri vormi `topicPath/subscriptions/subscriptionName`. Seetõttu [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) **varude** **DataCollectionTopic** teema tellimuse loomiseks *entityPath* peab olema seatud `DataCollectionTopic/subscriptions/Inventory`. Kood kuvatakse järgmiselt:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Tellimuse filtrid

Seni sel teema saadetud sõnumid on teha kättesaadavaks kõigi registreeritud tellimused. Võtme fraasi siin on "kättesaadav." Ajal teenuse siini tellimuste näha kõiki sõnumeid, mis saadetakse teema, saate kopeerida ainult need sõnumid alamhulk virtuaalse tellimuse järjekorda. See toimub tellimuse *filtrite*abil. Kui loote tellimuse, võite pakkuda filtriavaldise SQL92 laadi predikaat, mis töötab nii Süsteemiatribuudid (nt [silt](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) kui ka rakenduse atribuutide, nagu eelmises näites **väli StoreName** sõnumi atribuutide kujul.

Päevakohaste illustreerimiseks seda stsenaariumi, teine pood on meie jaemüügi stsenaarium lisada. Kõik POS terminalid: nii poed müügiandmetega veel marsruutida tsentraliseeritud süsteemi, kuid poe ülemuse armatuurlaua tööriista abil on ainult huvi selle poe jõudlus. Tellimuse filtreerimine selleks saate kasutada. Pange tähele, et kui POS terminalid avaldada sõnumeid, nad atribuudi **väli StoreName** rakenduse sõnumi. Antud kaks salvestab, näiteks **Redmond** ja **Seattle**, POS terminalid on Redmond talletada oma müügiandmeid sõnumid, mille **väli StoreName** võrdne **Redmond**, Seattle poe POS terminalid kasutatav **väli StoreName** on võrdne **Seattle**tempel. Redmond poe poe ülemus soovib ainult POS automaatidesse andmete kuvamiseks. Süsteemi kuvatakse järgmiselt:

![Teenuse-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Häälestada selle marsruudi, saate luua **armatuurlaua** tellimuse järgmiselt:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Tellimuse filtriga kopeeritakse ainult sõnumid, mille **väli StoreName** atribuudi väärtuseks **Redmond** **armatuurlaua** tellimuse virtuaalse järjekorda. Seal on palju tellimuse filtreerimine, kuid. Rakenduste võib olla mitme filtri reeglid lisaks võimalus muuta sõnumi atribuute, kui see on tellimuse virtuaalse järjekorda läbib tellimuse kohta.

## <a name="summary"></a>Kokkuvõte

Kõik põhjused, miks kasutada queuing kirjeldatud [luua rakendusi, mis kasutavad teenuse siini järjekorrad](service-bus-create-queues.md) kehtivad ka teemasid, spetsiaalselt:

- Ajalise lahutamine – sõnumi tootjad ja tarbijad ei pea olema samal ajal võrgus.

- Laadi ühtlustamise – ilusaks koormus peaks olema ette valmistatud Keskmine koormus asemel tippväärtus laadi nõudvate rakenduste teema.

- Laadi tasakaalustamiseks – sarnase järjekorda, võib olla mitu konkureerivate tarbijad kuulamine ühe tellimusel koos iga sõnumi antakse välja ainult üks tarbijate, seetõttu tasakaalustamiseks laadi.

- Lahti ühendada – sõnumside võrgu võivad muutuda, kui mõjutamata olemasoleva lõpp-punktid; näiteks tellimuste lisamise või muutmisega filtreerida, et uute kasutajate lubamiseks teema.

## <a name="next-steps"></a>Järgmised sammud

POS jaemüügi stsenaariumi järjekorrad kasutamise kohta lisateabe saamiseks vt [luua rakendusi, mis kasutavad teenuse siini järjekorrad](service-bus-create-queues.md) .