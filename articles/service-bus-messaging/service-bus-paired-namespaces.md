<properties 
    pageTitle="Teenuse siini paaris nimeruumid | Microsoft Azure'i"
    description="Andmepunktipaaride nimeruum rakendamise üksikasjad ja maksumus"
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

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Paaris nimeruum rakendamise üksikasjad ja maksumus mõju

[PairNamespaceAsync][] meetodit, kasutades [SendAvailabilityPairedNamespaceOptions][] eksemplari, sooritab nähtav teie nimel. Kuna on maksumus peaksite arvesse võtma, kui funktsiooni abil, on kasulik mõista nende ülesannete nii, et see juhtub ootab käitumist. API tegeleb järgmised automaatne käitumine teie nimel:

-   Mahajäämus järjekorda loomine.
-   [MessageSender][] objekti, mis sobib järjekorrad või teemade loomine.
-   Kui üksused on tekstsõnumside muutub pole saadaval, saadab ping sõnumite üksusele katse tuvastamiseks vabanemise selle üksuse uuesti.
-   Soovi korral loob kogumi "sõnumi pumbad" mis sõnumite teisaldamine järjekordades mahajäämus esmane kohale.
-   Koordinaadid sulgeda/Murrang esmaseid ja teiseseid [MessagingFactory][] eksemplaris.

Kõrge, funktsioon toimib järgmiselt: kui esmane üksus on terve muutusteta käitumine ilmneda. Kui soovite [FailoverInterval][] kestuse möödub ja esmase olemi näeb ei õnnestunud saadab pärast-mööduv [MessagingException][] või [TimeoutException][]järgmised käitumine ilmneb:

1.  Saada esmase olemi toimingud on keelatud ja süsteemi ping esmase olemi kuni ping saate toimetatud.

2.  Juhusliku mahajäämus järjekord oleks märgitud.

3.  [BrokeredMessage][] objektide suunatakse valitud mahajäämus järjekorras.

1.  Kui saada toiming valitud mahajäämus järjekorda nurjub, selle järjekorda on tõmmatakse pöörde ja uue järjekorra oleks märgitud. Kõigi saatjate [MessagingFactory][] eksemplaris lugege tõrke.

Järgmised arvud kujutatakse jada. Esmalt saatja saadab sõnumid.

![Andmepunktipaaride nimeruumid][0]

Korral saata esmane järjekorda, algab saatja sõnumeid saata CEIP valitud mahajäämus järjekorras. Korraga, see algab ping tööülesande.

![Andmepunktipaaride nimeruumid][1]

Selles etapis sõnumid on endiselt teisene järjekorras ja on esmane järjekorda ei toimetatud. Kui esmane järjekord on terve uuesti, vähemalt üks protsess peaks töötama soovitud syphon. Funktsiooni syphon toimetab sõnumite erinevate mahajäämus järjekorrad proper sihtkoha üksuste (järjekorrad ja Teemad).

![Andmepunktipaaride nimeruumid][2]

Ülejäänud selles teemas käsitletakse üksikasjad, kuidas neid andmeühikuga tööd.

## <a name="creation-of-backlog-queues"></a>Mahajäämus järjekorda loomine

[SendAvailabilityPairedNamespaceOptions][] objekti [PairNamespaceAsync][] meetodil on möödas näitab, mitu mahajäämus järjekorda, mida soovite kasutada. Iga mahajäämus järjekord siis loodud järgmiste atribuutidega konkreetselt määramine (kõik muud väärtused on seatud [QueueDescription][] vaikesätted):

| Tee                                   | [esmane nimeruum] / x-servicebus-transfer / [indekseerida], kus [Register] on väärtus [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 15 minutit                                                                                             |
| EnableDeadLetteringOnMessageExpiration | True                                                                                                 |
| EnableBatchedOperations                | True                                                                                                 |

Näiteks esimese mahajäämus järjekord loodud nimeruum **contoso** nimega `contoso/x-servicebus-transfer/0`.

Järjekorrad loomisel kood kontrollib esmalt kui selline järjekord on olemas. Kui järjekorda pole olemas, luuakse järjekorda. Kood ei puhastada "eest" mahajäämus järjekorrad. Eelkõige sellest, kui taotluse esmane nimeruum **contoso** taotleb viis mahajäämus järjekorrad, kuid mahajäämus järjekord teega `contoso/x-servicebus-transfer/7` on olemas, selle eest mahajäämus järjekord on endiselt olemas, kuid ei kasutata. Süsteem võimaldab konkreetselt eest mahajäämus järjekorrad olemas, mis ei tohi kasutada. Nimeruumi omanikuna vastutate järjekorrata mahajäämus kasutamata/soovimatu puhastamiseks. Seda otsust on teenuse siini ei saa teada, mis otstarbel on olemas kõik järjekorrad oma nimeruumi. Lisaks kui järjekorda eesnime olemas, kuid ei vasta eeldatud [QueueDescription][], siis teie põhjused, miks on oma vaikekäitumise muutmiseks. Garantiid tehtud muudatuste kohale mahajäämus oma kood. Veenduge, et põhjalikult muudatuste kontrollimiseks.

## <a name="custom-messagesender"></a>Kohandatud MessageSender

Saatmisel, kõigi sõnumite läbida sisemise [MessageSender][] objekti, mis käitub tavaliselt, kui kõik töötab, ja suunab mahajäämus kohale, kui asju "murda." Saades-mööduv tõrge, käivitub. Pärast [kuuline ajavahemik][] koosneb [FailoverInterval][] atribuudi väärtust ajal, mis ei õnnestunud sõnumid saadetakse, on tõrkesiire tegeleb. Selles etapis olla iga üksuse järgmisi asju.

- Ping tööülesande aktiveeritakse iga [PingPrimaryInterval][] kontrollida, kui üksus on saadaval. Kui see toiming õnnestub, käivitab kõik kliendi koodi, mis kasutab üksuse kohe esmane nimeruumi uute sõnumite saatmine.

- Tulevaste taotluste saatmiseks, et sama olemi muude saatjatelt tulemuseks on [BrokeredMessage][] saadetakse muuta istuda mahajäämus järjekorras. Muudatuse osa atribuute eemaldab [BrokeredMessage][] objekti ja salvestab need mujal. Järgmised atribuudid on tühi ja lisada uue pseudonüüm ja teenuse siini ja SDK sõnumite töötlemiseks ühtlaselt all.

| Vana atribuudi nimi       | Uue atribuudi nimi |
|-------------------------|-------------------|
| SeansiId               | x-ms-SeansiId    |
| TimeToLive              | x-ms-timetolive   |
| ScheduledEnqueueTimeUtc | x-ms-tee         |

Algse sihtkoha tee on ka salvestatud sõnumi atribuudi nimega x-ms-tee. See võimaldab sõnumite paljudele ettevõtetele eksisteerivad ühe mahajäämus järjekorras. Atribuutide tõlgitud tagasi soovitud syphon.

Kohandatud [MessageSender][] objekti saab tekib probleeme, kui sõnumeid lähenemine 256 KB piirmäära ja Tõrkesiirde tegeleb. Kohandatud [MessageSender][] objekti talletab sõnumid kõigile järjekorrad ja teemade koos järjekordades mahajäämus. Selle objekti segab palju primaries koos sees mahajäämus järjekordade meilisõnumeid. Laadi tasakaalustamiseks hulgas paljud kliendid, kes ei tea üksteise käsitlema SDK CEIP huvitavat ühe mahajäämus järjekord iga [QueueClient][] või [TopicClient][] loodud kood.

## <a name="pings"></a>Ping

Ping sõnum on tühi [BrokeredMessage][] selle [ContentType][] atribuudiga rakenduse/vnd.ms-servicebus-ping ja [TimeToLive][] väärtuseks 1 teise. See ping on üks teisiti omadus teenuse siini: server kunagi pakub ping kui mis tahes helistaja taotleb on [BrokeredMessage][]. Seetõttu tuleb kunagi saate teada, kuidas saada ja ignoreerida kõiki sõnumeid. Iga üksuse (kordumatu järjekorda või teema) kohta [MessagingFactory][] näiteks kliendi kohta kuvatakse pingis, kui need on saadaval olla. Vaikimisi on see juhtub kord minutis. Ping sõnumid loetakse tavalise teenuse siini sõnumeid ja võib põhjustada kulude läbilaskevõime ja sõnumites. Kui kliendid avastada, et süsteemi on saadaval, sõnumite peatada.

## <a name="the-syphon"></a>Funktsiooni syphon

Vähemalt üks käivitatava programmi rakenduse peaks selle syphon aktiivselt töötama. Funktsiooni syphon teostab pikk küsitlus vastu, mis kestab 15 minutit. Kui kõik üksused on saadaval ja teil on 10 mahajäämus järjekorrad, mis hostib soovitud syphon rakendus nõuab vastuvõtmise toiming 40 korda tunnis, 960 korda päevas ja 28800 korda 30 päeva. Kui soovitud syphon aktiivselt liigub sõnumite mahajäämus esmane järjekorda, iga sõnumi kogemusi järgmised kulud (standard sõnumi suurus ja läbilaskevõime kõnetasud kõik järk-järgult):

1.  Mahajäämus saata.

2.  Saadud mahajäämus.

3.  Esmane saata.

4.  Saadud esmast.

## <a name="closefault-behavior"></a>Sule/viga käitumine

Mis majutab syphon, kui põhi- või [MessagingFactory][] vead või suletud ilma selle partneri rakenduses on tehtud/suletud ja selle syphon tuvastab olekus, syphon toimingud. Kui muud [MessagingFactory][] ei ole suletud 5 sekunditega, kuvatakse selle syphon vea endiselt avatud [MessagingFactory][].

## <a name="next-steps"></a>Järgmised sammud

Vaadake teemat [asünkroonne sõnumside mustrite ja kõrge-saadavus][] üksikasjalik arutelu teenuse siini asünkroonne sõnumside. 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Kuuline ajavahemik]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asünkroonne sõnumside mustrite ja kõrge-saadavus]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
