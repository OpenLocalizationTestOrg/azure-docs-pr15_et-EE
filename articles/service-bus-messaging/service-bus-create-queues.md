<properties 
    pageTitle="Kirjutage rakendusi, mis kasutavad teenuse siini järjekorrad | Microsoft Azure'i"
    description="Kuidas kirjutada lihtne järjekorda-põhine rakendus, mis kasutab teenuse siini."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Saate luua rakendusi, mis kasutavad teenuse siini järjekorrad

Selles teemas kirjeldatakse teenuse siini järjekorrad ja näidatakse, kuidas kirjutada lihtne järjekorda-põhine rakendus, mis kasutab teenuse siini.

Kaaluge stsenaarium maailma jaemüügi müügiandmeid Point-of-Sale (POS) terminalidest peab laoseisu haldamise süsteem, kui stock täiendada määratlemiseks andmeid kasutava viidud. See lahendus kasutab teenuse siini sõnumside jaoks suhtlemine terminalid ja süsteemi, nagu on näidatud järgmisel joonisel:

![Teenuse siini järjekorrad pilt 1](./media/service-bus-create-queues/IC657161.gif)

Iga kassaterminal aruannete selle müügiandmeid **DataCollectionQueue**sõnumeid saata. Enne, kui need on toodavate süsteemi, jäävad need sõnumid selles järjekorras. See muster sageli nimetatakse *asünkroonne sõnumside*, kuna selle kassaterminal pole süsteemi jätkamist vastuse ootamine.

## <a name="why-queuing"></a>Miks queuing?

Enne vaatame koodi, mida on vaja selle rakenduse häälestamine, kaaluge selle stsenaariumi asemel POS terminalid järjekorda kasutamise eelised rääkida otse (sünkroonselt) laoseisu haldamise süsteem.

### <a name="temporal-decoupling"></a>Ajalise lahutamine

Asünkroonne sõnumside mustriga tootjad ja tarbijad ei pea olema samal ajal võrgus. Kiirsõnumside taristu usaldusväärselt talletab sõnumid kuni nõudvate tootja on valmis neid vastu võtta. See tähendab, et komponentide jaotatud rakendus saab lahti, kas vabatahtlikult; näiteks hooldus või komponent krahhi tõttu, ilma mõjutamata kogu süsteem. Lisaks võidakse ainult nõudvate rakenduse peavad olema võrgus teatud ajal päeva. Näiteks sel jaemüügi süsteemi võib olla ainult tulevad online äri päeva pärast.

### <a name="load-leveling"></a>Ühtlustamise laadimine

Paljudes rakendustes süsteemi koormus muutub aja jooksul töötlemise ajal iga üksuse töö jaoks nõutavad on tavaliselt pidev. Vahendamisel omandatud sõnumi tootjad ja järjekorda tarbijatele tähendab, et nõudvate rakenduste (töötaja) ainult peab olema ette valmistatud teenuse tippväärtus laadi asemel mõne Keskmine laadi. Sügavus kuhjuda kasvab ja lepingu sissetulevate koormuse muutudes. See otse säästab raha summa taristu vajalikud rakenduse Laadi suhtes.

![Teenuse siini järjekorrad pilt 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Laadi tasakaalustamiseks

Kui koormus suureneb, saab lisada rohkem töötaja protsesside töötajate järjekord lugeda. Iga sõnumi töödeldakse ainult ühe töötaja protsesside. Lisaks selle tasakaalustamiseks pull-põhiste laadi võimaldab töötaja arvutite optimaalse kasutamise isegi kui töötaja arvutites erinevad seoses töötlemise võimsus, nagu nad tõmbab sõnumite oma ülemmäära. See muster sageli nimetatakse konkureerivate tarbija mustri.

![Teenuse siini järjekorrad pilt 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Lahti ühendada

Sõnumi järjekorda paneku Kesk sõnumi tootjate ja tarbijate vahel kasutamine tagab komponendid on sisemine lahti ühendada. Kuna tootjad ja tarbijad ei tea üksteisest, saab tarbija uuendada ilma mingit mõju tootja. Lisaks sõnumside topoloogia võivad muutuda mõjutamata olemasoleva lõpp-punktid. Selgitame seda rohkem kui räägitakse avaldamise/tellimise.

## <a name="show-me-the-code"></a>Kuva kood

Järgmises jaotises kuvatakse teenuse siini koostamiseks selle rakenduse kasutamise kohta.

### <a name="sign-up-for-an-azure-account"></a>Azure'i konto kasutajaks registreerumine

Peate selleks, et alustada tööd teenuse siini Azure'i konto. Kui te ei ole veel üks, te saate registreeruda tasuta konto [siin](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Nimeruumi loomine

Kui teil on tellimus, saate [luua uue nimeruum](service-bus-create-namespace-portal.md). Iga nimeruum toimib ulatuse container määratud teenuse siini üksuste jaoks. Andke oma uue nimeruum kordumatu nimi üle kõik teenuse siini kontod. 

### <a name="install-the-nuget-package"></a>Installige Nugeti pakett.

Teenuse siini nimeruumi kasutamiseks peab taotluse viide teenuse siini komplekti, spetsiaalselt Microsoft.ServiceBus.dll. Saate otsida selle komplekti osana Microsoft Azure'i SDK ja allalaadimine on saadaval veebisaidil [Azure'i SDK allalaadimislehele](https://azure.microsoft.com/downloads/). [Teenuse siini Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus) aga on kõige lihtsam viis teenuse siini API ja kõik teenuse siini sõltuvused rakenduse konfigureerimiseks.

### <a name="create-the-queue"></a>Kuhjuda loomine

Halduse teenuse siini sõnumside üksuste (järjekorrad ja avaldamise/tellimise teemade) jaoks tehete [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) klassi kaudu. Teenuse siini kasutab [Ühiskasutusse Accessi allkirja (SAS)](service-bus-sas-overview.md) vastavalt turvalisus mudel. Klassi [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) tähistab Turbeloa pakkujat sisseehitatud factory meetoditega esitus mõned tuntud Turbeloa pakkujad. Kasutame [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) meetodi ootelepanekuks SAS mandaat. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) eksemplar on seejärel ehitatud baasaadressi teenuse siini nimeruum ja Turbeloa pakkuja.

Klassi [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) pakub võimalust luua, loetlemine ja sõnumside üksuste kustutamine. Kood, mis on näidatud siin näitab, kuidas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) eksemplar on loodud ja saab luua **DataCollectionQueue** järjekorda.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Pange tähele, et ülekoormuse [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) meetod, mis võimaldavad korraldada järjekorra atribuudid. Näiteks saate määrata vaikimisi aja live (TTL) väärtus järjekorda saadetud sõnumite jaoks.

### <a name="send-messages-to-the-queue"></a>Järjekorda sõnumeid saata

Käitusaja toimingute teenuse siini üksustele; näiteks sõnumite saatmine ja vastuvõtmine, rakenduse peate esmalt looma [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekti. Sarnaselt [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) ainekursust, luuakse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) eksemplari baasaadressi teenuse nimeruumi ja Turbeloa pakkuja kaudu.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Sõnumite saatmiseks ja saadud teenuse siini järjekorrad on [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) klassi eksemplarid. See tund koosneb kogumi standardatribuudid (nt [silt](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), hoidke rakenduse atribuutide ja keha suvalise rakenduse andmete kasutatava sõnastiku. Rakenduse saate määrata mis tahes sarjadesse jaotatav objekt (järgmises näites edastab **SalesData** objekti, on kassaterminal müügiandmete tähistava), mille keha, mis kasutab [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) serialiseerida objekti. Teise võimalusena saate esitada [voo](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) objekti.

Lihtsaim viis antud järjekorda, meie puhul **DataCollectionQueue**sõnumite saatmiseks on kasutada [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) objekti loomiseks otse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) eksemplari.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Sõnumite vastuvõtu järjekord

Järjekorda sõnumeid vastu, saate kasutada [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekti, otse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)abil loodavad. Sõnumi vastuvõtjad saate töötada kaks erinevat režiimi: **ReceiveAndDelete** ja **PeekLock**. [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) on seatud vastuvõtja sõnumi loomisel parameetrina [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) kõne.


**ReceiveAndDelete** režiimi kasutades selle vastuvõtmine on üks-shot toiming; See tähendab, kui teenuse siini saab taotluse, see märgib sõnumi nagu tarbitud ja tagastab selle rakenduse. **ReceiveAndDelete** režiim on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba käsitleta sõnumit, kui tekib tõrge. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini märgitud sõnumid tarbitud, kui rakenduse taaskäivitamist ja tarbimine sõnumite uuesti algust, see on vastamata sõnumit, mis oli tarbitud enne krahhi.

**PeekLock** režiimis, kuvatakse vastuvõtu muutub kahe etapi toiming, mis võimaldab toetada rakendusi, mistõttu puuduvad sõnumid. Kui teenuse siini saab taotluse, leiab see järgmine sõnum olla tarbitud, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab teise etapi vastuvõtu protsessi [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) helistades vastuvõetud sõnumi. Kui teenus siini näeb [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) helistada, see märgib sõnumid tarbitud.

Võimalikud on kaks muud tulemused. Esmalt, kui rakendus ei saa sõnumit töödelda mingil põhjusel, selle saate helistada [loobuda](https://msdn.microsoft.com/library/azure/hh181837.aspx) vastuvõetud sõnumi ( [valmis](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) asemel. See põhjustab teenuse siini sõnumi avamiseks ja saanud uuesti, kas sama tarbija või mõne muu lõpuleviimine tarbija jaoks kättesaadavaks teha. Teiseks on seostatud lukustus ajalõpp ja kui rakendus ei saa sõnumit töödelda enne lukustus aegub ajalõpp (näiteks siis, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi avamine ja kättesaadavaks uuesti vastu võtta (sisuliselt [loobuda](https://msdn.microsoft.com/library/azure/hh181837.aspx) toimingute tegemise vaikimisi).

Pange tähele, et kui rakendus jookseb pärast sõnumi töötleb, kuid enne [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) taotlus on välja antud, sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. See on sageli nimetatakse *Vähemalt ühe korra* töötlemine. See tähendab, et iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis täiendavad loogika on vajalik rakendus duplikaatide tuvastamiseks. Selle saavutamiseks [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) atribuudi sõnumi. Selle atribuudi väärtus jääb samaks saatmiskatsete üle. Seda nimetatakse *Täpselt pärast* töötlemist.

Kood, mis on näidatud siin saab ja töötleb sõnumi **PeekLock** režiim, mis on vaikimisi, kui väärtus [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) selgesõnaliselt puudub.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Kasutada järjekord

Eespool toodud näidetes loodud objektide [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) ja [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) otse [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) saata ja vastu võtta sõnumid vastavalt järjekorda, kaudu. Alternatiivne lähenemine on kasutada [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) klassi, mis toetab nii saata ja vastu võtta lisaks veel funktsioone, näiteks seansid toimingud.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid järjekorda, vt [loomine rakenduste kasutavad teenuse siini teemade ja tellimuste](service-bus-create-topics-subscriptions.md) jätkata selle arutelu avalda/tellida teenuse siini teemade ja tellimuste võimaluste kasutamine.