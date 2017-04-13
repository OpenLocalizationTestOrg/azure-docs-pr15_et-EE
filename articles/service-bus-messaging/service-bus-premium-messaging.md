<properties
    pageTitle="Teenuse siini Premium ja Standard sõnumside hinnad astme ülevaade | Microsoft Azure'i"
    description="Teenuse siini Premium ja Standard sõnumside"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Teenuse siini Premium ja Standard sõnumside astme 

Teenuse siini sõnumside, mis sisaldab sõnumside üksustele nagu järjekorrad ja teemad, kombineerides enterprise sõnumside võimalusi RTF-vormingus avaldamine – tellimine semantika cloud tasandil. Teenuse siini sõnumside kasutatakse side nurgakivi palju keerukaid pilve lahendusi.

*Premium* taseme teenuse siini sõnumside aadressid levinud klientide taotlusi skaala, jõudlus ja kättesaadavus olulise rakenduste ümber. Kuigi funktsioon määrab on peaaegu identsed, nende kahe astme teenuse siini sõnumside on mõeldud Teeni kasutamine eri juhtudel.

Järgmises tabelis on esile tõstetud üksikasjalik erinevusi.

| Premium                               | Standard                       |
|---------------------------------------|--------------------------------|
| Kõrge jõudlus                       | Muutuv läbilaskevõime            |
| Prognoositavad jõudlus               | Muutuv latentsus               |
| Hinna                   | Maksa muutuv hinnad |
| Võimalus üles ja alla töökoormus skaala | N/A                            |
| Sõnumi suurus > 256KB                  | Sõnumi maht on 256KB          |

**Teenuse siini Premium sõnumside** pakub ressursside eraldamise CPU ja mälu kihis nii, et iga kliendi töökoormus töötab eraldi. Selle ressursi container nimetatakse, *sõnumid üksus*. Iga premium nimeruum eraldatakse sõnumside vähemalt ühe üksuse. Saate osta 1, 2 või 4 iga teenuse siini Premium nimeruum sõnumside mõõtühikud. Ühe töökoormus või üksus võib ühendada mitu sõnumside üksuste ja sõnumside ühikute saate muuta, kuigi arveldamine on 24-tunnise või iga päev määr kulud. Tulem on teie teenuse siini-põhise lahenduse hõlpsalt prognoosida ja korratavad jõudlust.

Mitte ainult on selle jõudluse hõlpsalt prognoosida ja saadaval, kuid see on ka kiirem. Teenuse siini Premium sõnumside koostab kasutusele [Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/)salvestusruumi mootor. Premium sõnumside, on palju kiirem kui standardne taseme tippväärtus jõudlust.

## <a name="premium-messaging-technical-differences"></a>Premium sõnumside tehnilise erinevused

Järgnevalt on toodud mõned erinevused Premium ja Standard sõnumside astme.

### <a name="partitioned-queues-and-topics"></a>Sektsioonitud järjekorrad ja teemad

Sõnumside Premiumi on toetatud sektsioonitud järjekorrad ja teemad, kuid need ei toimi samamoodi nagu Standard- ja Basic astme teenuse siini sõnumside. Premium sõnumside kasutada SQL-i andmete säilitamiseks ja enam võimalik ressursside konkurentsiga seotud ühiskasutusega platvorm. Selle tulemusena eraldamine ei ole vaja. Lisaks on muutunud partition arvu 16 sektsioonid 2 sektsioonid Premiumi Standard menüü sõnumid. Kahe sektsioonid tagab kättesaadavuse ja on sobivam arv Premium käitusaja keskkonnas. Eraldamine kohta leiate lisateavet teemast [Partitioned järjekorrad ja teemade](service-bus-partitioning.md).

### <a name="express-entities"></a>Kiire üksused

Kuna Premium sõnumside töötab täielikult eraldatud käitusaja keskkonnas, ei toetata kiire üksuste Premium nimeruumid. Kiire funktsioonide kohta leiate lisateavet teemast [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) atribuuti.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet teenuse siini sõnumside kohta leiate järgmistest teemadest.

- [Azure'i teenus siini Premium (ajaveebipostituse) sõnumside tutvustus](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Azure'i teenus siini Premium (Channel9) sõnumside tutvustus](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Teenuse siini sõnumside ülevaade](service-bus-messaging-overview.md)
- [Kuidas kasutada teenuse siini järjekorrad](service-bus-dotnet-get-started-with-queues.md)
