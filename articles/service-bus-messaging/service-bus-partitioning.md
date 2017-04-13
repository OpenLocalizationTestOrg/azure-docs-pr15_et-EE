<properties 
    pageTitle="Liigendatud järjekorrad ja teemade | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse partition teenuse siini järjekorrad ja teemade mitme sõnumi maaklerid abil."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Sektsioonitud järjekorrad ja teemad

Azure'i teenus siini töötab mitme sõnumi maaklerid sõnumite töötlemiseks ja mitme kiirsõnumside poed sõnumite talletamiseks. Tavalise järjekorda või teema on käsitleja oli ühes sõnumis ta ja talletatud ühe sõnumside poe. Teenuse siini võimaldab järjekorrad või teemades olema liigendatud mitmesse sõnumi maaklerid ja sõnumside poed. See tähendab, et üldine jõudlus sektsioonitud järjekorda või teema on piiratud enam ühes sõnumis ta või kiirsõnumside pood. Lisaks ajutiste katkestuste sõnumside poe ei muuda sektsioonitud järjekorda või teema pole saadaval. Sektsioonitud järjekorrad ja teemade võib sisaldada kõik teenuse siini funktsioone, näiteks tehingud ja seansid tugi.

Teenuse siini sisemust kohta leiate lisateavet teemast [teenuse siini ülesehitus][] .

## <a name="how-it-works"></a>Kuidas see toimib

Iga sektsioonitud järjekorda või teema koosneb mitmest fragmendid. Iga fragment salvestatakse erinevate sõnumside poes ja käsitleja oli muu sõnumi ta. Sõnumi saatmisel sektsioonitud järjekorda või teema määrab teenuse siini sõnumi ühte osadest. Valiku teha teenuse siini või saatja saab määrata sektsiooni võtme abil juhusliku ID-ga.

Kui klient soovib teate sektsioonitud järjekorda või tellimuse sektsioonitud teema, teenuse siini päringute kõik fragmendid sõnumite jaoks, siis tagastab esimese sõnumi, mis on saadud sõnumid poe vastuvõtja. Teenuse siini vahemälu teise sõnumite ja tagastab nende saab täiendavaid taotlusi vastu võtta. Vastuvõtmise klient ei tea eraldamine; kliendi suunatud toimimist sektsioonitud järjekorda või teema (näiteks lugeda, täitke, edasi lükata, deadletter, prefetching) on identne tavalise üksuse toimimist.

On täiendavaid tasuta edastamisel sõnumi, sõnumi saatja, teema või sektsioonitud järjekorda.

## <a name="enable-partitioning"></a>Luba eraldamine

Azure'i teenus siini sektsioonitud järjekorrad ja teemade kasutamine kasutada Azure SDK 2.2 või uuem versioon või määrake `api-version=2013-10` oma http taotleb.

Saate luua teenuse siini järjekorrad ja teemade 1, 2, 3, 4 ja 5 GB suuruses (vaikimisi 1 GB). Eraldamine lubatud, loob teenuse siini 16 sektsioonid iga määratud GB. Kui loote järjekorda, mis on 5 GB suuruseid, 16 sektsioonid maksimaalne järjekorra suurus muutub (5 \* 16) = 80 GB. Saate vaadata oma sektsioonitud järjekorda või teema maksimummaht vaadates selle kirje [Azure portaali][].

On mitu võimalust luua sektsioonitud järjekorda või teema. Kui loote järjekorda või teema rakenduse, saate lubada eraldamine järjekorda või teema järgi seada vastavalt [QueueDescription.EnablePartitioning][] või [TopicDescription.EnablePartitioning][] atribuudi väärtuseks **true**. Atribuutidest peab olema seatud ajal järjekorda või teema on loodud. Ei saa neid atribuute olemasoleva järjekorda või teema muuta. Näiteks:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Teise võimalusena saate luua sektsioonitud järjekorda või teema Visual Studio või [Azure portaali][]. Kui loote uue järjekorra või teema portaalis, määrata suvand **Luba eraldamine** **üldsätted** tera järjekorda või teema **sätete** aknas väärtuseks **true**. Visual Studio, klõpsake dialoogiboksis **Uus järjekorda** või **Uus teema** märkeruut **Luba eraldamine** .

## <a name="use-of-partition-keys"></a>Sektsiooni klahvid kasutamine

Kui sõnum on enqueued sektsioonitud järjekorda või teema, kontrollib teenuse siini sektsiooni võtme olemasolu. Kui see leiab ühe, valib fragmendid, et võti põhjal. Kui see ei leia sektsiooni võtme, valib fragment sisemise algoritmi järgi.

### <a name="using-a-partition-key"></a>Sektsiooni klahvi abil

Mõnel juhul, nt seansid või tehingute jaoks on vaja sõnumid salvestatakse teatud fragment. Kõik need stsenaariumid nõuavad sektsiooni võti. Kõik sõnumid, mida kasutada sama sektsiooni võti on määratud sama fragment. Kui fragment pole ajutiselt saadaval, teenuse siini tagastab tõrke.

Sõltuvalt stsenaariumi puhul kasutatakse erineva meilisõnumi atribuudid sektsiooni võti:

**SeansiId**: kui sõnum on seatud [BrokeredMessage.SessionId][] atribuut, siis teenuse siini kasutab selle atribuudi sektsiooni võti. Sellisel viisil kõik sõnumid, mis kuuluvad sama seansi käsitletakse sama sõnumi poolt. See võimaldab teenuse siini sõnumi tellimine samuti seansi olekus järjepidevuse tagamiseks.

**PartitionKey**: kui sõnum on atribuudi [BrokeredMessage.PartitionKey][] , kuid mitte [BrokeredMessage.SessionId][] atribuudi väärtuseks, siis teenuse siini kasutab atribuudi [PartitionKey][] sektsiooni võti. Kui sõnum sisaldab nii [SeansiId][] ja [PartitionKey][] atribuutide määramine, peab olema sama nii atribuudid. Kui [PartitionKey][] atribuut on seatud muu väärtus kui atribuudi [SeansiId][] , tagastab funktsioon teenuse siini **InvalidOperationException** erandi. Atribuudi [PartitionKey][] tuleks kasutada, kui saatja saadab-seansi arvestada kandeteateid. Sektsiooni võti tagab, et kõik sõnumid, mis saadetakse tehingu käsitletakse sama sõnumside poolt.

**MessageId**: kui järjekorda või teema on seatud **tõene** [QueueDescription.RequiresDuplicateDetection][] atribuut ja [BrokeredMessage.SessionId][] või [BrokeredMessage.PartitionKey][] atribuudid on pole määratud, siis [BrokeredMessage.MessageId][] serveeritakse sektsiooni võti. (Pange tähele, et Microsoft .NET ja AMQP teekide määramine automaatselt sõnumi ID, kui saatmise rakendus). Sel juhul käsitletakse sama sõnumi poolt sama sõnumi kõik eksemplarid. See võimaldab teenuse siini avastada ja kõrvaldada eksemplaris sõnumeid. Kui [QueueDescription.RequiresDuplicateDetection][] atribuut on seatud **tõene**, teenuse siini ei pea atribuudi [MessageId][] sektsiooni võti.

### <a name="not-using-a-partition-key"></a>Ei kasuta sektsiooni võti

Sektsiooni võtme puudumisel teenuse siini jaotab round-jaan mood, et kõik fragmendid sektsioonitud järjekorda või teema sõnumid. Kui valitud fragment pole saadaval, määrab teenuse siini erinevate fragment sõnum. Sellisel viisil Saatmistoiming õnnestus vaatamata ajutist puudumist sõnumside poe.

Teenuse siini anda piisavalt aega nimekirja lisamine eri fragmendid, sõnumi klient, mis saadab sõnumi [MessagingFactorySettings.OperationTimeout][] väärtus peab olema suurem kui 15 minutit. Soovitatav on [OperationTimeout][] atribuut seada vaikeväärtuse 60 sekundi järel.

Pange tähele, et sektsiooni võtme "viiku" teatud fragment sõnumi. Kui sõnumside poe, mis on see fragment pole saadaval, teenuse siini tagastab tõrke. Sektsiooni võtme puudumisel teenuse siini saate valida erinevaid fragmendid ja toiming õnnestus. Seetõttu on soovitatav, et te ei sisesta sektsiooni võtme juhul, kui see on nõutav.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Täpsemad Teemad: kasutada tehingud sektsioonitud üksustega

Tehingu osana saadetud sõnumid, peate määrama sektsiooni võti. See võib olla üks järgmised atribuudid: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]või [BrokeredMessage.MessageId][]. Kõik sõnumid, mis saadetakse sama kande peate määrama sama sektsiooni võti. Kui proovite saata sõnumi ilma sektsiooni võtme kande sees, tagastab funktsioon teenuse siini **InvalidOperationException** erandi. Kui proovite saata sama tehingust mitu sõnumit, mis on eri sektsiooni võtmed, tagastab teenuse siini **InvalidOperationException** erandi. Näiteks:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Kui atribuute, mis toimivad sektsiooni võti on määratud, teenuse siini pins teatud fragment sõnumi. Selline käitumine ilmneb kas tehingu kasutatakse. Soovitatav on, et teie määratud sektsioon klahvi kui ei ole vaja.

## <a name="using-sessions-with-partitioned-entities"></a>Sektsioonitud üksustega seansside kasutamine

Selgituseks sõnumi saatmiseks seansi arvestada teema või järjekorra sõnum peab olema [BrokeredMessage.SessionId][] atribuudi väärtuseks. Kui määratud on samuti [BrokeredMessage.PartitionKey][] atribuut, peab olema identne [SeansiId][] atribuut. Kui need erinevad, tagastab funktsioon teenuse siini **InvalidOperationException** erandi.

Erinevalt tavalise (liigendatud) järjekorrad või teemad, ei ole võimalik ühe toimingu abil saate eri seansid mitme sõnumeid saata. Kui proovitakse kohale toimetada, tagastab funktsioon teenuse siini **InvalidOperationException **erandi. Näiteks:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Sõnumite edasisaatmine koos automaatse liigendatud üksused

Azure'i teenus siini toetab automaatse sõnumi edasisaatmise kaudu, et või sektsioonitud üksuste vahel. Sõnumite automaatse edastamise lubamiseks atribuudi [QueueDescription.ForwardTo][] allika järjekorda või tellimus. Kui sõnum on määratud sektsioon klahvi ([SeansiId][], [PartitionKey][]või [MessageId][]), kasutatakse selle sektsiooni võti sihtkoha üksus.

## <a name="considerations-and-guidelines"></a>Kaalutlused ja juhised

- **Suure funktsioonid**: kui ettevõte kasutab funktsioone nagu seansid, duplikaatide tuvastamine või konkreetsete juhtimise eraldamine klahvi ja seejärel kiirsõnumside toimingute marsruuditakse alati teatud fragmendid. Kui mõnda selle fragmendid esineb veebiaadresside või aluseks oleva poe on vigane, need toimingud nurjuvad ja kättesaadavus on piiratud. Üldiselt järjepidevuse on ikka palju suuremad kui-liigendatud üksused; ainult alamkomplekt liikluse esineb probleeme, mitte kogu liikluse.
- **Haldus**: kõik fragmendid, üksuse tuleb teha toiminguid nagu loomine, värskendamine ja kustutamine. Kui mõni fragment on vigane võib põhjustada tõrkeid nende toimingute jaoks. Tööks too teave näiteks sõnumi loendab tuleb liita kõik fragmendid kaudu. Kui mõni fragment on vigane, piiratud staatusega üksuse võrgusolekuteabe tähist.
- **Väike sõnumi stsenaariumid**: sellise stsenaariumid, eriti siis, kui kasutate HTTP-protokolli, võib olla mitu teha toiminguid selleks, et saada kõik sõnumid saadetakse. Vastuvõtu taotluste esiosa sooritamine on võta vastu kõik fragmendid ja salvestab kõigi vastuste. Sama ühendust järgmise vastuvõtu taotluse oleks kasu selle vahemällu talletamine ja vastu võtta latentsused saab alumine. Kui teil on mitu ühendust või kasutada HTTP, mis loob uue ühenduse iga taotlus. Ei ole kindel, et see maa sama sõlme. Kui kõik olemasolevad sõnumid on lukus ja muu esiosa vahemälus, vastuvõtmise toiming tagastab **null**. Sõnumite lõpuks aegub ning saate neid uuesti. HTTP ülalhoidmise on soovitatav.
- **Sirvi/Peek sõnumite**: PeekBatch ei tagasta alati määratud [MessageCount atribuudi](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)teadete arv. On kaks peamist põhjust. Üks põhjus on selles, et sõnumite kogumit liidetud maht ületab lubatud 256KB suurust. Teine põhjus on selles, et kui järjekorda või teema on seatud **tõene** [EnablePartitioning atribuut](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) , sektsiooni ei pruugi olla piisavalt sõnumite lõpuleviimiseks nõutav teadete arv. Üldiselt kui rakenduse soovib teatud arvu sõnumite vastuvõtmine, seda peaksite helistama [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) seni, kuni ta saab seda sõnumite arv või on veel saadetaks peek. Vt lisateavet koodinäiteid, [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) või [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Viimane lisatud funktsioonid

- Lisage või eemaldage reegel toetab nüüd sektsioonitud üksustega. Erinevate üksuste liigendatud, neid toiminguid ei toeta tehingud. 
- Saatmise ja vastuvõtu sõnumeid ja sektsioonitud üksusest on nüüd AMQP toetatud.
- Järgmiste toimingute toetab nüüd AMQP: [Paketi saata](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Paketi vastu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [vastuvõtu järjekorranumber,](https://msdn.microsoft.com/library/azure/hh330765.aspx) [Peek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Pikendada lukustamine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Ajakava sõnumi](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Tühistamine ajastatud sõnum](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Reegli lisamine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Eemaldamine reegli](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Seansi pikendada Lock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Sea seansi olek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get seansi olek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Peek seansi sõnumite](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)ja [Loetlemine seansid](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Sektsioonitud üksuste piirangud

Praegu paneb teenuse siini järgmistele piirangutele sektsioonitud järjekorrad ja teemade kohta:

-   Sektsioonitud järjekorrad ja teemade ei toeta saata sõnumeid, mis kuuluvad eri seanssi ühe toimingu.
-   Teenuse siini võimaldab praegu kuni 100 sektsioonitud järjekorrad või teemade kohta nimeruum. Iga sektsioonitud järjekorda või teema arvestatakse piirmäära 10 000 üksuste nimeruum (ei kehti Premium taseme) kohta.

## <a name="next-steps"></a>Järgmised sammud

Arutelu [AMQP 1.0 tugi teenuse siini liigendatud järjekorrad ja teemade][] kohta eraldamine sõnumside üksuste kuvamine 

  [Teenuse siini ülesehitus]: service-bus-architecture.md
  [Azure'i portaal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [SeansiId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [Teenuse siini liigendatud järjekorrad ja teemade AMQP 1.0 tugi]: service-bus-partitioned-queues-and-topics-amqp-overview.md
