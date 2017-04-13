<properties 
    pageTitle="Teenuse siini tehingud | Microsoft Azure'i" 
    description="Azure'i teenus siini atomic tehingud ja saada kaudu ülevaade" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Teenuse siini tehingu töötlemise ülevaade

Selles artiklis käsitletakse tehingu võimaluste Azure'i teenus siini. Suur arutelu illustreerib [Teenuse siini valimi Atomic tehingud](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Selles artiklis on piiratud ülevaate tehingu töötlemise ja teenuse siini, *saata* funktsiooni Atomic tehingute valimi on laiem ja keerukamaid ulatus.

## <a name="transactions-in-service-bus"></a>Teenuse siini tehingute

[Tehingu](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) pakuvad koos kahe või enama toiminguid on *täitmise ulatust*. Laadi, peab sellise tehingu veenduge, et kõik toimingud, mis on antud rühma toimingute kas õnnestub või ei ühiselt. Sellega tehinguid tegutseda üks üksus, mis on sageli nimetatakse *atomicity*. 

Teenuse siini on selgituseks sõnumi ta ja tagab selgituseks viitamistervikluse kõik sisemise toimingud selle salvestab sõnumi vastu. Kõigi sõnumite sees teenuse siini, nt sõnumite teisaldamine [surnud täht järjekorda](service-bus-dead-letter-queues.md) või sõnumite [automaatne edasisaatmine](service-bus-auto-forwarding.md) üksuste vahel on selgituseks. Kui siini teenus sõnumi vastu võtab, see on juba salvestatud ja siltidega järjekorranumber. Seejärel mis tahes sõnumi edastamine ühenduses teenuse siini on koordineeritud toimingute kogu üksused ja viib valimata kaotsimineku (allikas õnnestub ja target nurjub) või kordamise (allikas ei õnnestu ja target õnnestub) sõnumi.

Teenuse siini toetab rühmitamise toimingute suhtes sõnumside üksus (järjekorda, teema, tellimuse) hõlmatud tehingu. Näiteks saate saata mitmele sõnumite jooksul toimingu ulatuses üks järjekorda ja sõnumid kuvatakse pühendunud järjekorra log üksnes juhul, kui tehingu on edukalt lõpule jõudnud.

## <a name="operations-within-a-transaction-scope"></a>Tehingu ulatuse piires 

Toimingud, mida tuleks teha tehingu ulatus on järgmised:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: saata, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: lõpule jõudnud, CompleteAsync loobuma, AbandonAsync, Deadletter, DeadletterAsync, edasi lükata, DeferAsync, RenewLock, RenewLockAsync 

Vastuvõtmine toimingud ei sisalda, sest eeldatakse, et rakendus saab sõnumite [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) režiimi, kasutades sees teatud tsükkel vastu või mõne [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) tagasihelistamise ja alles siis avaneb töötlemiseks sõnumi toimingu ulatuses.

Likvideerimise (täielik, loobuma surnud-täht edasi lükata) sõnumi ilmneb siis raames, ja sõltuvad tehingu üldine tulemuste kohta.

## <a name="transfers-and-send-via"></a>Ülekannetes ja "saata"

Andmete järjekorda protsessor ja seejärel mõni muu järjekord selgituseks üleandmine lubamiseks toetab teenus siini *edastamine*. Edastamine kasutusel saatja esmalt saadab sõnumi "edastamine järjekord" ja edastamine kuhjuda kohe teisaldab sõnumid sama robustne edastamine rakendamist, mis sõltub automaatselt edasi saadetud võimalus abil soovitud sihtkohta järjekorda. Sõnum on kunagi edastamine järjekorda log nii, et see muutub nähtavaks edastamine järjekorda tarbijate jaoks.

Selle selgituseks võimalus power ilmneb edastamine järjekord ise on saatja Sisestuskeel sõnumite allikas. Teisisõnu, teenuse siini saate edastada sõnumi sihtkoha järjekord "kaudu" edastamine järjekorda, täites täielik (või edasi lükata, või surnud-täht) sisestusteade, kõigi juurde ühest atomic toiming toimingut. 

### <a name="see-it-in-code"></a>Vaadake seda kood

Häälestada edastamine, saate luua sõnumi saatja, eesmärke sihtkoht kuhjuda kaudu edastamine järjekorda. Teil on ka vastuvõtja, mis tõmbab sõnumeid selle samas järjekorras. Näiteks:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Lihtne tehingu seejärel kasutab järgmisi elemente, nagu järgmises näites:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet teenuse siini järjekorrad järgmisi artikleid:

- [Aheldamise teenuse siini üksuste automaatne edasisaatmine](service-bus-auto-forwarding.md)
- [Automaatselt edasi saadetud näidis](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomic tehingud teenuse siini näidis](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure'i järjekorrad ja teenuse siini järjekorrad võrreldes](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Kuidas kasutada teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)