<properties 
    pageTitle="Automaatne edasisaatmine teenuse siini sõnumside üksuste | Microsoft Azure'i"
    description="Kuidas kettäärisega järjekorda või tellimuse teise järjekorda või teema."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Aheldamise teenuse siini üksuste automaatne edasisaatmine

Funktsiooni *automaatne edasisaatmine* võimaldab keti järjekorda või mõne muu järjekorda või teema, mis on osa sama nimeruumi tellimus. Kui automaatne edasisaatmine on lubatud, teenuse siini automaatselt eemaldab sõnumid, mis paigutatakse esimese järjekorda või tellimus (allikas) ja paneb need teise järjekorda või teema (sihtkoht). Pange tähele, et võimalik sõnumi saatmiseks otse sihtkoha üksus. Samuti ei ole võimalik keti on subqueue, nt deadletter järjekorda, teise järjekorda või teema.

## <a name="using-auto-forwarding"></a>Automaatne edasisaatmine abil

Saate lubada automaatne edasisaatmine, seades allika, nagu järgmises näites objektide [QueueDescription][] või [SubscriptionDescription][] [QueueDescription.ForwardTo][] või [SubscriptionDescription.ForwardTo][] atribuudid.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Andmeallika üksus on loodud ajal peab olemas sihtkoha üksus. Kui sihtkoha üksus pole, tagastab teenuse siini erandi, kui teil palutakse luua andmeallika üksus.

Saate välja üksikute teema mastaapimiseks automaatne edasisaatmine. Teenuse siini piirab 2000 [antud teema tellimuste arv](service-bus-quotas.md) . Teise taseme teemade loomisega mahub täiendavaid tellimusi. Pange tähele, et isegi juhul, kui te ei ole seotud piirangud teenuse siini tellimuste arv, lisada teise taseme teemade parandada oma teema üldine jõudlus.

![Automaatne edasisaatmine stsenaarium][0]

Samuti saate lahutada sõnumi saatjate vastuvõtjad automaatne edasisaatmine. Oletame näiteks, ERP-süsteemis, mis koosneb kolmest moodulist: tellimuse töötlemine, varude haldamine ja klientide juhtimine. Kõik need moodulid loob sõnumid, mis on enqueued üheks vastava teema. Alice ja Bob on müügiesindajad, kes on huvitatud kõik sõnumid, mis on seotud oma klientidele. Nende meilisõnumite vastuvõtmiseks Alice ja Bob luua isiklikud kuhjuda ja tellimuse ERP teemad, mis nende järjekorda kõigi sõnumite automaatseks edastamiseks.

![Automaatne edasisaatmine stsenaarium][1]

Kui Alice läheb puhkuse, oma isikliku järjekorra, mitte ERP teema, täitub. Selle stsenaariumi korral Kuna müügiesindaja ei saanud kõik sõnumid, ERP teemade pole kunagi jõuda kvoodi.

## <a name="auto-forwarding-considerations"></a>Automaatne edasisaatmine kaalutlused

Kui sihtkoha üksus on kogunenud palju sõnumeid ja ületab kvoodi või sihtkoha üksus on keelatud, lisab andmeallika üksuse sõnumite oma [surnud täht järjekorda](service-bus-dead-letter-queues.md) kuni sihtkoht on ruumi (või üksus on uuesti lubatud). Need sõnumid jätkatakse live järjekorras surnud täht nii, et peate selgesõnaliselt vastu võtta ja töödelda kaudu surnud täht järjekorda.

Kui Aheldamise üksikute teemade saada kombineeritud teemaga palju tellimusi, on soovitatav, et teil on mõõdukad arvu tellimuste esimese taseme teema ja palju tellimusi teise taseme teemade kohta. Näiteks võimaldab esimese taseme teema 20 tellimustega, iga teise taseme teemale 200 tellimustega ühendatud suurema läbilaskevõimega, kui esimese taseme teema 200 tellimustega iga ühendatud 20 tellimustega teise taseme teema.

Teenuse siini arved ühe toiminguga iga edasisaadetud sõnumi kohta. Näiteks sõnumi saatmist 20 tellimused, üks neist, mis on konfigureeritud automaatselt edasi saadetud sõnumite teise järjekorda või teema, mille teema on tasulised kui 21 toimingute kui kõik esimese taseme tellimused kuvatakse sõnumi koopia.

Tellimus, mis on ühendatud teise järjekorda või teema loomiseks tellimuse looja peab olema õiguste **haldus** lähte- ja sihtkoha olemi kohta. Andmeallika teema sõnumite saatmine ainult nõuab allika teema **saatmise** õigused.

## <a name="next-steps"></a>Järgmised sammud

Üksikasjalikku teavet automaatne edasisaatmine, lugege teemadest viide:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Teenuse siini. jõudlustäiustused kohta leiate lisateavet teemast [Partitioned sõnumside üksused][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Sektsioonitud sõnumside üksused]: service-bus-partitioning.md