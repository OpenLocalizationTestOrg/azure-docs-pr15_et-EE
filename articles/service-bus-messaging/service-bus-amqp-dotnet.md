<properties 
    pageTitle="Teenuse siini .net-i ja AMQP 1.0 | Microsoft Azure'i"
    description="AMQP teenuse siini kaudu .net-i abil"
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

# <a name="using-service-bus-from-net-with-amqp-10"></a>AMQP 1.0 teenuse siini kaudu .net-i abil

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Teenuse siini SDK allalaadimine

AMQP 1.0 tugi on saadaval teenuse siini SDK 2.1 või uuem versioon. Saate tagada, peate teenuse siini bittide laadides [Nugeti][]uusim versioon.

## <a name="configuring-net-applications-to-use-amqp-10"></a>.Net-i rakendusi kasutada AMQP 1.0 konfigureerimine

Vaikimisi teenuse siini .net-i kliendi teek suhtleb teenuse siini teenuse sihtotstarbeline SOAP-põhiste protokolli abil. AMQP 1.0 vaikimisi asemel kasutama protokolli nõuab konkreetsete konfiguratsiooni teenuse siini ühendusstring, järgmises jaotises kirjeldatud. Peale selle muudatuse rakenduse koodi ei muutu AMQP 1.0 kasutamisel.

Praeguses versioonis on mõne API funktsioonid, mis pole toetatud AMQP kasutamisel. Neid Toetuseta funktsioonid on loetletud allpool jaotises [Mittetoetatavaid funktsioone, piirangud, ja käitumist erinevused](#unsupported-features-restrictions-and-behavioral-differences). Teatud Täpsemad konfiguratsioonisätted on ka erinevat tähendust AMQP kasutamisel.

### <a name="configuration-using-appconfig"></a>Konfiguratsiooni App.config abil

See on hea tava rakenduste App.config konfiguratsioonifail abil saate talletada sätted. Teenuse siini rakendused, saate App.config sätete teenus siini **ConnectionString** väärtuse talletamiseks. Mõni näide App.config faili on järgmine:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Väärtus on `Microsoft.ServiceBus.ConnectionString` säte on konfigureerida teenuse siini ühendamiseks kasutatava teenuse siini ühendusstring. Vorming on järgmine:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Kui `[namespace]` ja `SharedAccessKey` saadakse [Azure portaali][]. Lisateabe saamiseks lugege teemat [Alustamine teenuse siini järjekorrad][].

AMQP kasutamisel lisab ühendusstringi koos `;TransportType=Amqp`. Selles esituses teavitab kliendi teek teenuse siini abil AMQP 1.0 selle ühenduse loomiseks.

## <a name="message-serialization"></a>Sõnumi sariväljaanne

Vaikimisi protokolli kasutamisel vaikekäitumist sariväljaanne .net-i kliendi teek on [DataContractSerializer][] tüübi abil serialiseerida [BrokeredMessage][] eksemplari kliendi teek ja teenuse siini teenuse vahel. AMQP transport režiimi kasutades kasutab kliendi teek sariväljaanne [vahendatud sõnumi][BrokeredMessage] sõnumisse AMQP AMQP süsteemi tüüp. See sariväljaanne võimaldab sõnumi vastu ja vastuvõtmise rakendus, mis töötab potentsiaalselt eri platvorm, näiteks Java rakendus kasutab JMS API juurdepääsu teenuse siini tõlgendada.

Kui te ehitada [BrokeredMessage][] eksemplari, saate sisestada .NET objekti parameetrina ehitaja olla sõnumi kehasse. Mis saab vastendada AMQP lihtsad tüüpi objektide puhul on keha päiseteave AMQP andmetüübid. Kui objekti ei saa otse kaardistada AMQP lihtsad tüüp; määratletud kohandatud tüüp, siis objekt on seeriasertide abil [DataContractSerializer][]ja sarjadesse jaotatud baiti saadetakse meilisõnumi AMQP andmed.

Interoperability-.NET klientidega hõlbustamiseks kasutada ainult .NET tüübid, mis võib olla päiseteave otse AMQP tüübid sõnumi kehasse. Järgmises tabelis üksikasjad nende tüüpide ja vastavate vastenduse süsteemile AMQP tüüp.

| .Net-i sisu objekti tüüp          | Vastendatud AMQP tüüp                          | AMQP keha jaotis tüüp                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| bool                           | kahendmuutuja                                   | AMQP väärtus                                                                                                                                                |
| bait                           | ubyte                                     | AMQP väärtus                                                                                                                                                |
| ushort                         | ushort                                    | AMQP väärtus                                                                                                                                                |
| uint                           | uint                                      | AMQP väärtus                                                                                                                                                |
| ulong                          | ulong                                     | AMQP väärtus                                                                                                                                                |
| sbyte                          | bait                                      | AMQP väärtus                                                                                                                                                |
| lühike                          | lühike                                     | AMQP väärtus                                                                                                                                                |
| int                            | int                                       | AMQP väärtus                                                                                                                                                |
| pikk                           | pikk                                      | AMQP väärtus                                                                                                                                                |
| hõljumine                          | hõljumine                                     | AMQP väärtus                                                                                                                                                |
| kahekordne                         | kahekordne                                    | AMQP väärtus                                                                                                                                                |
| Decimal                        | decimal128                                | AMQP väärtus                                                                                                                                                |
| char                           | char                                      | AMQP väärtus                                                                                                                                                |
| Kuupäev ja kellaaeg                       | ajatempli                                 | AMQP väärtus                                                                                                                                                |
| GUID                           | UUID                                      | AMQP väärtus                                                                                                                                                |
| byte]                         | binaarsed                                    | AMQP väärtus                                                                                                                                                |
| string                         | string                                    | AMQP väärtus                                                                                                                                                |
| System.Collections.IList       | loend                                      | AMQP väärtus: kogumis olevaid üksusi saab ainult need, mis on määratletud selles tabelis.                                                             |
| System.Array                   | massiiv                                     | AMQP väärtus: kogumis olevaid üksusi saab ainult need, mis on määratletud selles tabelis.                                                             |
| System.Collections.IDictionary | kaardi                                       | AMQP väärtus: kogumis olevaid üksusi saab ainult need, mis on määratletud selles tabelis. Märkus: ainult stringi klahve ei toetata.                        |
| URI                            | Kirjeldatud string (vt järgmine tabel) | AMQP väärtus                                                                                                                                                |
| DateTimeOffset                 | Pikk kirjeldatud (vt järgmine tabel)   | AMQP väärtus                                                                                                                                                |
| Kuuline ajavahemik                       | Pikk kirjeldatud (vt järgmist)         | AMQP väärtus                                                                                                                                                |
| Voo                         | binaarsed                                    | AMQP andmed (võib olla mitu). Andmete jaotised sisaldavad töötlemata baitide voo objekti lugeda.                                                           |
| Mõnele muule objektile                   | binaarsed                                    | AMQP andmed (võib olla mitu). Sisaldab objekti, mis kasutab funktsiooni DataContractSerializer või serializer, mis on esitatud rakenduse sarjadesse jaotatud kahendarvuks. |

| .Net-i tüüp      | Vastendatud AMQP kirjeldatud tüüp                                                                                              | Märkmete                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Kuuline ajavahemik       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Toetuseta funktsioonid ja piirangud käitumist erinevused

Teenuse siini .NET API järgmised funktsioonid on toetatud praegu AMQP kasutamisel:

-   Tehinguid

-   Saata edastamine sihtkoht

On ka teenuse siini .NET API toimimist small erinevusi AMQP, võrrelduna vaikimisi protokolli kasutamisel.

-   [OperationTimeout][] ignoreeritakse.

-   `MessageReceiver.Receive(TimeSpan.Zero)`kui rakendatakse `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Sõnumite lõpuleviimine, Lukusta sõned saab teha ainult poolt algselt saadud sõnumid sõnumi.

## <a name="controlling-amqp-protocol-settings"></a>AMQP protokolli sätted juhtimine

.Net-i API-de nähtavaks tegemine mitme sätteid nii, et juhtida toimimist AMQP Protocol (protokoll):

-   **MessageReceiver.PrefetchCount**: määrab algse krediit rakendatud link. Vaikimisi on 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: AMQP paneeli maksimummaht pakutud läbirääkimistel ühendus juhtelementide avatud aeg. Vaikimisi on 65 536 baiti.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: kui on batchable, see väärtus määratleb maksimaalne viivitus saatmise korraldused. Pärivad saatjad/vastuvõtjad vaikimisi. Üksikute saatja ja vastuvõtja saate alistada vaikimisi, mis on 20 millisekundit.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: määrab, kas AMQP ühendused on loodud SSL-ühenduse kaudu. Vaikimisi on **täidetud**.

## <a name="next-steps"></a>Järgmised sammud

Kas olete valmis saada lisateavet? Külastage järgmisi linke:

- [Teenuse siini AMQP ülevaade]
- [Teenuse siini liigendatud järjekorrad ja teemade AMQP 1.0 tugi]
- [Klõpsake teenuse siini Windows Server AMQP]

  [Teenuse siini järjekorrad kasutamise alustamine]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[Nugeti]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure'i portaal]: https://portal.azure.com
[Teenuse siini AMQP ülevaade]: service-bus-amqp-overview.md
[Teenuse siini liigendatud järjekorrad ja teemade AMQP 1.0 tugi]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[Klõpsake teenuse siini Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
