<properties 
    pageTitle="Teenuse siini teemade kasutamine Python | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure teenuse siini teemade ja tellimuste Python kaudu." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Kuidas kasutada teenuse siini teemade ja tellimused

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini teemade ja tellimused. Näidiste Python kirjutada ja kasutada [Python Azure'i paketi][]. Stsenaariumid, mis hõlmab kaasake **teemade ja tellimuste loomise**, **tellimuse filtrite loomise**, **teemale sõnumite saatmine**, **sõnumeid tellimusest**ja **teemade ja tellimuste kustutamine**. Teemade ja tellimuste kohta leiate lisateavet jaotisest [Järgmised toimingud](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Märkus:** Kui teil on vaja installida Python või [Python Azure'i paketi][], leiate [Python installi juhend](../python-how-to-install.md).

## <a name="create-a-topic"></a>Teema loomine

Objekti **ServiceBusService** võimaldab töötada teemade. Lisage mis tahes Python faili, kus soovite programmiliselt juurde teenuse siini ülaosas järgmine tekst:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Järgmine kood tekitab **ServiceBusService** objekti. Asendage `mynamespace`, `sharedaccesskeyname`, ja `sharedaccesskey` koos oma tegelik nimeruum ühiskasutusse Accessi allkirja (SAS) võti nimi ja sisestage väärtus.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

[Azure'i portaalis][]saate hankida väärtused SAS võtme nimi ja väärtus.

```
bus_service.create_topic('mytopic')
```

**loomine\_teema** toetab ka lisasuvandid, mis võimaldavad teil Alista vaikesätted teema näiteks sõnumi aega live või teema maksimaalne maht. Järgmises näites määrab mahu ülempiir teema 5 GB ja kellaaeg live (TTL) väärtust 1 minuti jooksul:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Tellimuste loomine

Teemade tellimused on loonud ka **ServiceBusService** objekti. Tellimused on nimega ja võib-olla valikuline filter, mis piirab kogumi sõnumid toimetada see tellimus virtuaalse järjekorda.

> [AZURE.NOTE] Tellimused on püsiv ja jätkub kuni need või teema, millele nad on märgitud, kustutatakse.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luua tellimuse vaikefilter (MatchAll)

**MatchAll** filter on vaikefilter, mida kasutatakse kui filtrit pole määratud, kui luuakse uus tellimus. **MatchAll** filtri kasutamisel paigutatakse kõik sõnumid, mis on avaldatud teema see tellimus virtuaalse järjekorda. Järgmises näites nimega 'AllMessages' tellimuse loob ja kasutab **MatchAll** vaikefilter.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Tellimuste filtritega loomine

Samuti saate määratleda filtrid, mis võimaldavad teil määrata, milliseid teema saadetud sõnumid ilmub kindla teemaga seotud tellimuse sees.

Kõige paindlik tellimuste filter on mõne **SqlFilter**, mis rakendab SQL92 alamhulga. SQL-i filtrid toimivad atribuutide sõnumid, mis on avaldatud teema. SQL-i filtriga kasutatavate avaldiste kohta leiate lisateavet teemast [SqlFilter.SqlExpression][] süntaks.

Saate lisada filtreid tellimust, kasutades funktsiooni **loomine\_reegli** **ServiceBusService** objekti meetodit. See meetod võimaldab uute filtrite lisamine olemasolevale tellimusele.

> [AZURE.NOTE] Kuna vaikefilter rakendatakse kõigi uute tellimused automaatselt, peate eemaldama vaikefilter või **MatchAll** alistab võite määrata filtreid. Saate eemaldada vaikimisi reegel, kasutades funktsiooni **kustutamine\_reegli** **ServiceBusService** objekti meetodit.

Järgmises näites luuakse nimega tellimuse `HighMessages` koos **SqlFilter** , mis valib ainult sõnumid, mis on suurem kui 3 kohandatud **messagenumber** atribuut:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Samuti järgmises näites luuakse nimega tellimuse `LowMessages` koos **SqlFilter** , mis valib ainult sõnumid, mis on **messagenumber** atribuudi väiksem või võrdne 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nüüd, kui sõnum saadetakse `mytopic` see saadetakse alati vastuvõtjad tellinud **AllMessages** teema tellimus ja valikuliselt toimetatud vastuvõtjad tellinud **HighMessages** ja **LowMessages** teema tellimused (olenevalt sõnumi sisu).

## <a name="send-messages-to-a-topic"></a>Sõnumite saatmiseks teema

Sõnumi saatmiseks teenuse siini teema, peate kasutama rakenduse selle **saata\_teema\_sõnumi** **ServiceBusService** objekti meetodit.

Järgmises näites näitab, kuidas viis testida sõnumite saatmiseks `mytopic`. Pange tähele, et iga sõnumi **messagenumber** atribuudi väärtus muutub kursis iteratsiooni (seda määrab, millised tellimused jõua adressaadini):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Teenuse siini teemade toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Teema sõnumite arv pole piiratud, kuid kokku suurus sõnumid, mille teema on ülempiir. Selles teemas maht on määratletud loomise ajal, 5 GB ülempiir. Kvootide kohta leiate lisateavet teemast [teenuse siini piirmäärasid][].

## <a name="receive-messages-from-a-subscription"></a>Sõnumeid, mis tellimusest

Saadetud meilisõnumid tellimuse kasutades selle **vastu\_tellimuse\_sõnumi** meetod **ServiceBusService** objekti:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Sõnumid kustutatakse tellimusest, nagu need on loetav, kui parameetri **peek\_Lukusta** väärtuseks **False**. Saate lugeda (peek) ja lukustamine sõnumi kustutamine kuhjuda parameetri ilma **pisieelvaade\_Lukusta** väärtuseks **True**.

Toimimist lugemise ja kustutage sõnum osana vastuvõtmise toiming on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

Kui soovitud **pisieelvaade\_Lukusta** parameeter on seatud väärtusele **tõene**, on vastuvõtu muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi helistades **sõnumi** objekti **kustutamine** meetod. **Kustuta** meetodit märgib sõnumi nagu tarbitud ja eemaldab tellimus.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis selle saate helistada **avada** meetod objektil **sõnum** . See põhjustab teenuse siini sees tellimuse sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

On ka seotud lukustatud jooksul tellimuse sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse avab sõnumi automaatselt ja teeb selle kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **kustutamine** meetodit nimetatakse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Töötlemine on vähemalt üks kord**, st iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada, kasutades **MessageId** atribuut, mis samaks saatmiskatsete üle.

## <a name="delete-topics-and-subscriptions"></a>Teemade ja tellimuste kustutamine

Teemade ja tellimused on püsiv ja tuleb selgesõnaliselt [Azure portaali][] kaudu või programmiliselt kustutada. Järgmises näites on kujutatud, kuidas kustutada teema nimega `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Teema kustutamine kustutatakse ka kõik tellimused, mis on registreeritud teema. Samuti saate tellimuste sõltumatult kustutatud. Järgmine kood näitab, kuidas kustutada tellimuse nimega `HighMessages` kaudu soovitud `mytopic` teema:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete teenuse siini teemade põhitõdesid õppinud, järgige neid linke lisateabe.

-   Vt [järjekorrad, teemade, ja tellimused][].
-   Viide [SqlFilter.SqlExpression][]jaoks.

[Azure'i portaal]: https://portal.azure.com
[Python Azure'i pakett]: https://pypi.python.org/pypi/azure  
[Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Teenuse siini kvoote]: service-bus-quotas.md 
