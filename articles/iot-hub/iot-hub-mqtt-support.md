<properties
 pageTitle="Asjade jaoturi MQTT tugi | Microsoft Azure'i"
 description="Kirjeldus MQTT asjade jaoturi taseme tugi"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>Asjade jaoturi MQTT tugi

Asjade jaoturi lubab suhelda asjade jaoturi seadme lõpp-punktid abil [MQTT v3.1.1] seadmete[ lnk-mqtt-org] protokolli pordi 8883 või MQTT v3.1.1 pordi 443 WebSocket-protokolli. Asjade jaoturi nõuab kõigi seadme side oleks tagatud SSL/TLS-i abil (seega asjade jaoturi ei toeta ebaturvalise ühendused Port 1883).

## <a name="connecting-to-iot-hub"></a>Ühenduse asjade jaoturi

Seadme abil saate MQTT protokoll ühenduse asjade jaoturiga kas kasutades [Microsoft Azure'i asjade SDK-d] raamatukogud[ lnk-device-sdks] või, kasutades funktsiooni MQTT protokoll otse.

## <a name="using-the-device-client-sdks"></a>Seadme kliendi SDK-d

[Kliendi seadme SDK-d] [ lnk-device-sdks] toetavate MQTT Protocol (protokoll) on saadaval Java, Node.js, C, C# ja Python. Kliendi seadme SDK-d kasutada standard asjade jaoturi ühendusstringi asjade jaoturiga ühendust luua. Protokoll MQTT kasutamiseks olema seatud **MQTT**parameetri kliendi Protocol (protokoll). Vaikimisi seadme kliendi SDK-d ühenduse asjade jaoturiga koos **CleanSession** lipp **0** ja kasutada **QoS 1** sõnumi Exchange'i asjade jaoturi abil.

Kui seade on ühendatud asjade jaoturiga, seadme kliendi SDK-d pakuvad meetodid, mis võimaldavad seadme sõnumeid saata ja vastu võtta sõnumeid soovitud asjade keskuse kaudu.

Järgmine tabel sisaldab linke koodinäiteid iga toetatud keele jaoks ja parameetri abil luua ühendus asjade jaoturi MQTT protokolli abil saate määrata.

| Keel                   | Parameetri Protocol (protokoll)        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure'i asjade Interneti-seadme mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#:][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migreerimine versioonist AMQP seadme rakendus MQTT
Kui kasutate [Kliendi seadme SDK-d][lnk-device-sdks], rakenduselt abil AMQP, et MQTT nõuab muutmise protokolli parameeter client käivitamine, nagu eespool.

Seda tehes veenduge, et kontrollida järgmiste üksuste.

* AMQP annab vigade palju tingimusi, samal ajal MQTT katkestab ühenduse. Seetõttu võib oma erandi töötlemise loogika vaja mõned muudatused.
* MQTT ei toeta *kõnest* toimingute kui [C2D]sõnumeid[lnk-messaging]. Kui teie tagasi lõpuks peab vastuse saadud seadme rakendus, kaaluge [otsese meetodite][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Otse MQTT protokolli abil

Kui seade ei saa kasutada seadme kliendi SDK-d, see ikkagi ühenduse MQTT protokolli abil avaliku seadme lõpp-punktid. **Ühenduse loomine** paketi seade peaks kasutama järgmised väärtused:

- Kasutage välja **ClientId** **deviceId**. 
- Väli **Username** kasutada `{iothubhostname}/{device_id}`, kus on {iothubhostname} asjade jaoturi täielik CName.

    Näiteks kui teie asjade jaoturi nimi on **contoso.azure-devices.net** ja seadme nimi on **MyDevice01**, täielik **kasutajanimi** väli peaks sisaldama `contoso.azure-devices.net/MyDevice01`.

- **Parooli** välja puhul kasutada SAS luba. Luba SAS vorming on sama mis HTTP ja AMQP protokollid:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    SAS sõned loomise kohta leiate lisateavet jaotisest seade, [kasutades asjade jaoturi turvalisus sõned][lnk-sas-tokens].
    
    Kui katsetamine, võite kasutada ka [Seadme Exploreri] [ lnk-device-explorer] tööriist kiiresti luua SAS luba, mida saate kopeerida ja kleepida oma kood:
    
    1. Avage seadmes Exploreris vahekaardi **haldus** .
    2. Klõpsake **SAS Turbeloa** (paremas ülanurgas).
    3. Klõpsake **SASTokenForm**, valige rippmenüüst seadme **DeviceID** . Määrake oma **TTL**.
    4. Klõpsake nuppu **Genereeri** luua oma luba.
    
    SAS luba, mis on loodud, on see struktuur:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    See luba kasutada **MQTT ühendamiseks parooliväli** on osa:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT ühenduse loomine ja paketid katkestada, asjade jaoturi probleemid sündmuse **Toimingute jälgimine** kanali täiendavat teavet, mis aitavad teil ühenduvuse probleemide tõrkeotsing.

### <a name="sending-messages-to-iot-hub"></a>Asjade jaoturi sõnumite saatmine

Pärast eduka ühenduse loomist seadme saata sõnumeid asjade jaoturi abil `devices/{device_id}/messages/events/` või `devices/{device_id}/messages/events/{property_bag}` **Teema nimi**. Funktsiooni `{property_bag}` element võimaldab saata sõnumeid täiendavad atribuudid url-kodeeringuga vormingus seade. Näiteks:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] See `{property_bag}` elemendi kasutab sama kodeering nagu päringu stringid HTTP-protokolli.

Saate kasutada ka seadme klientrakendusega `devices/{device_id}/messages/events/{property_bag}` nimega selle **teema nimi on** määratleda *kuvatakse sõnumite* edastamise telemeetria sõnumina.

Asjade jaoturi QoS 2 sõnumeid ei toeta. Kui seadme kliendi avaldab sõnumi **QoS**2, asjade jaoturi suleb võrguühendust.
Asjade jaoturi püsivad Säilita sõnumeid. Kui seade saadab sõnumi **SÄILITA** lipu seadmine 1, asjade jaoturi lisab on **x-valida-säilitamise** rakenduse atribuudi sõnumi. Sel juhul püsib Säilita sõnum, mitte asjade jaoturi annab see kirjutamata rakendusele.

### <a name="receiving-messages"></a>Sõnumite vastuvõtmine

Asjade jaoturi sõnumeid vastu, peaksite seadme tellida, kasutades `devices/{device_id}/messages/devicebound/#` **Teema Filter**. Mitmetasemeline metamärkide **#** teema filter kasutatakse ainult lubada seadme saada täiendavad atribuudid teema nimi. Asjade jaoturi ei võimalda kasutamine on **#** või **?** Metamärkide sub teemade filtreerimiseks. Kuna asjade jaoturi ei ole üldine otstarve pub-sub sõnumside ta, ainult toetab dokumenteeritud teema nimed ja teema filtrid.

Pange tähele, et seade ei saa kõiki sõnumeid asjade keskuses enne, kui see on edukalt tellinud oma seadme kindla lõpp-punkti, tähistab soovitud `devices/{device_id}/messages/devicebound/#` teema filter. Pärast eduka tellimus on loodud, hakkavad seadme ainult cloud-seadme sõnumeid, mis on saadetud selle pärast tellimuse. Kui seade loob **CleanSession** lipp väärtuseks **0**, kuvatakse sunnita tellimus üle erinevate seansid. Sel juhul saate järgmine kord, kui see ühendab **CleanSession 0** seadme tasumata sõnumid, mis on saadetud see oli katkestatud. Kui seade kasutab **CleanSession** lipu seadmine **1** , aga seda ei saa mis tahes sõnumeid asjade keskuse kaudu seni, kuni see toetab oma seadme lõpp-punkti.

Asjade jaoturi pakub sõnumid, mille **Teema nimi** `devices/{device_id}/messages/devicebound/`, või `devices/{device_id}/messages/devicebound/{property_bag}` kui määratud on mõni sõnum atribuudid. `{property_bag}`sisaldab url-kodeeringuga võtme/väärtuse paari sõnumi atribuudid. Ainult rakenduse atribuutide ja kasutajale kõigest Süsteemiatribuudid (nt **messageId** või **correlationId**) on kaasatud atribuudisalvest. Süsteemi atribuutide nimesid on eesliide **$**, rakenduse atribuutide abil eesliidet algse atribuudi nimi.

Kui seadme kliendi tellib teema **QoS**2, asjade jaoturi annab suurima QoS tase 1 **SUBACK** paketi. Pärast seda, toob asjade jaoturi sõnumite seadme abil QoS 1.

### <a name="additional-considerations"></a>Täiendavad kaalutlused

Kui lõplik tähelepanu, kui teil on vaja kohandada MQTT protokolli käitumine küljel pilveteenuses, peaksite [Azure'i asjade protokoll lüüsi] [ lnk-azure-protocol-gateway] võimaldab teil suure jõudlusega kohandatud protokolli lüüsi, mis liidesed asjade jaoturi juurutamine. Lüüsi Azure'i asjade Protocol (protokoll) võimaldab teil kohandada seadme protokoll varasema MQTT juurutuste mahutamiseks või muud kohandatud protokollid. Seda moodust nõua, et käivitada ja kasutada kohandatud protokolli lüüsi.

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks lugege teemat [märkmeid MQTT toetavad] [ lnk-mqtt-devguide] Azure asjade jaoturi arendaja juhendist.

Protokoll MQTT kohta leiate lisateavet teemast [MQTT dokumentatsiooni][lnk-mqtt-docs].

Juurutamise asjade jaoturi kavandamise kohta leiate lisateavet järgmistest teemadest.

- [Azure'i sertifitseeritud asjade seadme kataloogi jaoks][lnk-devices]
- [Täiendavad tugi protokolle][lnk-protocols]
- [Sündmuse jaoturi ja võrdlus][lnk-compare]
- [Skaala HA, ja DR][lnk-scaling]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
