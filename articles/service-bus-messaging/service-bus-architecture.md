<properties 
    pageTitle="Teenuse siini ülesehitus | Microsoft Azure'i"
    description="Kirjeldatakse sõnumi ja töötlemine arhitektuur Azure'i teenus siini relay."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Teenuse siini ülesehitus

Selles artiklis kirjeldatakse sõnumi ja töötlemine arhitektuur Azure'i teenus siini relay.

## <a name="service-bus-scale-units"></a>Teenuse siini skaala üksused

Teenuse siini on korraldatud *ulatuse üksused*. Skaala üksus on ühiku juurutamise ja sisaldab kõiki nõutavad käivitada teenus. Iga piirkonna kasutab ühe või mitme teenuse siini skaala üksused.

Teenuse siini nimeruum on vastendatud skaala ühik. Üksus Mastaapimissuvandite tegeleb igat tüüpi teenuse siini üksuste: releed ja vahendatud sõnumside üksuste (järjekorrad, teemade, tellimused). Teenuse siini skaala ühik koosneb järgmistest osadest.

- **Lüüsi sõlmed kogum.** Lüüsi sõlmed autentida sissetulevad taotlused ja käsitlemiseks relay taotlused. Iga lüüsi sõlm on avaliku IP-aadress.

- **Sõnumside ta sõlmed kogum.** Kiirsõnumside ta sõlmed taotlused sõnumside üksuste kohta.

- **Ühe lüüsi pood.** Lüüsi poe hoiab andmeid kõik üksused, mis on määratletud see üksus Mastaapimissuvandite. Lüüsi poe rakendatakse peal SQL Azure'i andmebaas.

- **Mitme kiirsõnumside poed.** Kiirsõnumside poed hoidke sõnumite järjekorrad, teemade ja tellimused, mis on määratletud see üksus Mastaapimissuvandite. See sisaldab ka kõik tellimuse andmed. Kui [sektsioonitud sõnumside üksused](service-bus-partitioning.md) on lubatud, järjekorda või teema on vastendatud ühe sõnumside pood. Tellimused on talletatud sama sõnumside poe oma ema teema. Teenuse siini [Premium sõnumside](service-bus-premium-messaging.md), v.a rakendatakse sõnumside poed peal SQL Azure'i andmebaasid.

## <a name="containers"></a>Ümbriste

Iga sõnumside üksus on määratud teatud ümbrises. Ümbris on loogiline ehitada, mis kasutab täpselt ühe sõnumside poe abil poe jaoks see ümbris olulisi andmeid. Iga on määratud sõnumside ta sõlm. Tavaliselt on rohkem kui ta sõlmed sõnumside ümbriste. Seetõttu iga sõnumside ta sõlm laadib mitme ümbriste. Jaotuse ümbriste sõnumside ta sõlme on korraldatud nii, et kõik sõnumside ta sõlmed võrdselt laaditakse. Kui laadi muster muudatused (nt üks ümbriste saab väga hõivatud) või kui kiirsõnumside ta sõlm muutub ajutiselt saadaval, on ümbriste edasi sõnumside ta sõlme vahel.

## <a name="processing-of-incoming-messaging-requests"></a>Sissetulevate sõnumite taotluste

Kui klient saadab taotluse teenuse siini, Azure laadi koormusetasakaalustusteenuse marsruutimine see kõiki lüüsi sõlmed. Lüüsi sõlm lubab taotluse. Kui kutse on tekstsõnumside üksus (järjekorda, teema, tellimuse), lüüsi sõlm otsib lüüsi poes üksus ja määratleb, milliseid sõnumside poes üksus asub. Seejärel otsib mis sõnumside ta sõlm on praegu teenindamine see ümbris ja saadab kutse sõnumside ta sõlme. Kiirsõnumside ta sõlm töötleb taotluse ja värskendamist üksuse olek container poest. Kiirsõnumside ta sõlm saadab vastuse sõlme lüüsi, mis saadab sobiva lahenduse kliendile väljastanud algne koosolekukutse.

![Sissetulevate sõnumite taotluste](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Sissetuleva meili edastamine taotluste

Kui klient saadab taotluse teenuse siini, Azure laadi koormusetasakaalustusteenuse marsruutimine see kõiki lüüsi sõlmed. Kui kutse on kuulamine taotluse, lüüsi sõlm loob uue relay. Kui kutse on teatud edastamine ühenduse taotluse, lüüsi sõlm edastab ühendust loomast lüüsi sõlme omanik selle relay. Lüüsi sõlm, mis kuulub soovitud relay saadab rendezvous taotluse kuulamise klient, paludes kuulajale lüüsi sõlm, mis on saanud ühendust loomast ajutine kanali loomiseks.

Kui relay ühendus on loodud, saate klientide Exchange'i sõnumeid lüüsi sõlm, mida kasutatakse kogunemiskoht kaudu.

![Sissetuleva meili edastamine taotluste](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete lugenud ülevaate teenuse siini ülesehitus, alustamiseks külastage järgmisi linke:

- [Teenuse siini sõnumside ülevaade](service-bus-messaging-overview.md)
- [Teenuse siini põhialused](service-bus-fundamentals-hybrid-solutions.md)
- [Ootel sõnumside lahenduse teenuse siini järjekorrad abil](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
