<properties
 pageTitle="Arendaja juhend - sõnumside | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - seadme pilve ja pilveteenuse-seadme sõnumside"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="send-and-receive-messages-with-iot-hub"></a>Sõnumite saatmiseks ja vastuvõtmiseks asjade jaoturi abil

## <a name="overview"></a>Ülevaade

Asjade jaoturi pakub järgmist sõnumside primitiivid seadme suhelda.

- [Seadme pilve] [ lnk-d2c] seadmest, et rakendus uuesti end.
- [Pilveteenuse-seadme] [ lnk-c2d] rakendusest tagasi lõppu (*teenuse* või *pilveteenuses*).

Asjade jaoturi sõnumite funktsioonile Core atribuudid on töökindluse ja kestvus sõnumeid. Neid atribuute lubada paindlikkuse vahelduva ühenduvuse seadme küljel ja diagrammi juhul töötlemine cloud küljel laadimiseks abil. Asjade jaoturi rakendab *vähemalt ühe korra* kohaletoimetamise garantiid seadme pilve nii pilve-seadme sõnumside.

Asjade jaoturi toetab mitut [seadme suunatud Protokollid] [ lnk-protocols] (nt MQTT, AMQP ja HTTP). Kogu Protokollid tõrgeteta koostalitlusvõime toetamiseks asjade jaoturi määratleb [levinud sõnumivormingu] [ lnk-message-format] kõik seadme suunatud Protokollid toetavad.

Asjade jaoturi seab [sündmuse jaoturi ühilduv lõpp-punkti] [ lnk-compatible-endpoint] lubamiseks tagaandmebaas rakenduste jaoturi seadme pilve sõnumeid lugeda.

### <a name="when-to-use"></a>Millal kasutada

Sõnumside on core võimalus asjade keskuse kohta. Kasutage seda iga kord, kui peate oma seadme sõnumeid saata oma tagasi lõppu või saata sõnumeid tagasi end oma seadme.

Asjade jaoturi ja sündmuse jaoturi teenuste võrdlus, vaadake teemat [asjade jaoturi võrdlus ja sündmuse jaoturi][lnk-compare].

## <a name="device-to-cloud-messages"></a>Seadme pilve sõnumid

Saadetud seadme pilve sõnumeid läbi seadme suunatud endpoint (**/devices/ {deviceId} / sõnumite/sündmused**). Tagaandmebaas teenust saab seadme pilve sõnumeid teenuse suunatud lõpp (**/messages/events**), mis ühildub [Sündmuse jaoturi]kaudu[lnk-event-hubs]. Seega saate kasutada standard [sündmuse jaoturi integratsioon ja SDK-d] [ lnk-compatible-endpoint] seadme pilve sõnumeid.

Asjade jaoturi rakendab seadme pilve sõnumside viisil, mis sarnaneb [Sündmuse jaoturi][lnk-event-hubs]. Asjade jaoturi seadme pilve sõnumid on rohkem kui [Teenus siini] sündmuse jaoturi *sündmuste* [ lnk-servicebus] *sõnumeid*.

Selle rakendamist on järgmised tagajärjed:

* Sarnaselt sündmuse jaoturi sündmusi, et seade to cloud sõnumid on püsival ja säilitata soovitud asjade keskuses kuni seitse päeva (vt [seadme pilve konfiguratsiooni suvandid][lnk-d2c-configuration]).
* Seadme pilve sõnumid on liigendatud üle sektsioonid fikseeritud komplekt, mis on seatud loomise ajal (vt [seadme pilve konfiguratsiooni suvandid][lnk-d2c-configuration]).
* Analoogselt sündmuse jaoturi, et kliendid seadme pilve sõnumite lugemise töödelda sektsioonid ja checkpointing. Vaata [Sündmuse jaoturi - tarbimine sündmuste][lnk-event-hubs-consuming-events].
* Sündmuse jaoturi sündmusi, seadme pilve sõnumite võib olla maksimaalselt 256 KB ja pakettidena optimeerida saadab rühmitamiseks. Töölehed saab kõige 256 KB ja kõige 500 sõnumeid.

Siiski on oluline eristada asjade jaoturi seadme pilve sõnumside ja sündmuse jaoturi.

* Nagu on selgitatud teemas [asjade jaoturi juurdepääsu reguleerimine] [ lnk-devguide-security] jaotises asjade jaoturi abil saavad seadme kohta autentimise ja juurdepääsu juhtimine.
* Asjade jaoturi abil saavad samaaegselt ühendatud seadmete miljoneid (vt [kvootide ja ahendamise][lnk-quotas]), samas kui sündmus jaoturi on piiratud 5000 AMQP ühendused nimeruum kohta.
* Asjade jaoturi ei võimalda suvalise eraldamine on **PartitionKey**abil. Seadme pilve sõnumid on liigendatud vastavalt nende pärit **deviceId**.
* Asjade jaoturi skaleerimist on veidi teistsugune, kui sündmus jaoturi skaleerimist. Lisateabe saamiseks lugege teemat [Skaleerimist asjade jaoturi][lnk-guidance-scale].

> [AZURE.NOTE] Te ei saa asendada asjade jaotis sündmuse jaoturi kõigi stsenaariumide. Näiteks teatud sündmuste töötlemine arvutuste, võib olla vaja jaotamine sündmuste eri atribuuti või välja enne selle andmevoogu analüüsimine. Selle stsenaariumi korral võite kasutada mõnda sündmust jaoturi seos kahe osa voo töötlemine kohaletoimetamisel. Lisateabe saamiseks lugege teemat [Azure sündmuse jaoturi ülevaade] *sektsioonid* [lnk-eventhub-partitions].

Seadme pilve sõnumside, kasutamise kohta vt [asjade jaoturi API -d ja SDK-d][lnk-sdks].

> [AZURE.NOTE] HTTP kasutamisel seadme pilve sõnumite saatmiseks atribuutide nimesid ja väärtuste võib sisaldada ainult ASCII tähti ja numbreid, pluss ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Mitte-telemeetria liikluse

Sageli, lisaks telemeetria andmepunkti, seadmed ka saata sõnumeid ja taotlusi, mis nõuavad täitmise ja töötlemine: loogika kihid. Näiteks kriitilised teatisi, mida peate käivitama teatud toimingu tagasi end või seadme vastused käsud saadetakse tagasi lõppu.

Parim viis töötlemine sellist teadet kohta leiate lisateavet teemast soovitud [õpetus: kohta asjade jaoturi seadme pilve sõnumite] [ lnk-d2c-tutorial] õpetuse.

### <a name="device-to-cloud-configuration-options"></a>Seadme pilve konfiguratsiooni suvandid

Soovitud asjade jaoturi seab selleks, et saaksite kontrollida seadme pilve sõnumside järgmised atribuudid.

* **Sektsiooni loendus**. Selle atribuudi loomisel määratleda arvu sektsioonid seadme pilve sündmuse kohta.
* **Säilituspoliitika aeg**. Atribuut määrab seadme pilve sõnumite säilitamise aeg. Vaikimisi on üks päev, kuid võib olla kuni seitse päeva.

Lisaks analoogselt sündmuse jaoturi, et asjade jaoturi võimaldab hallata seadme pilve rühmi vastu lõpp-punkti.

Saate muuta kõiki neid atribuute, kas programmiliselt [asjade jaoturi ressursi pakkuja REST API-de][lnk-resource-provider-apis], või kasutades [Azure portaali][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Tüssamisvastane atribuudid

Seadme tüssamine asjade jaoturi seadme pilve sõnumites vältimiseks sõnumeid templite kõik järgmised atribuudid:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Kahe esimese sisaldavad **deviceId** ja **generationId** pärit seadet, nagu iga [seadme identiteedi atribuudid][lnk-device-properties].

Atribuut **ConnectionAuthMethod** sisaldab JSON seeriasertide objekti, mille järgmised atribuudid:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Pilveteenuse-seadme sõnumid

Saadate cloud-seadme sõnumeid teenuse suunatud endpoint (**/messages/devicebound**) kaudu. Seadme saab neid seadme endpoint (**/devices/ {deviceId} / sõnumite/devicebound**) kaudu.

Iga sõnumi cloud-seade on suunatud ühest seadmest, seades atribuuti **soovite** , et **/devices/ {deviceId} / sõnumite/devicebound**.

>[AZURE.IMPORTANT] Iga seadme järjekorra hoiab kõige 50 cloud-seadme sõnumeid. Proovite sama seadme annab tulemuseks vea rohkem sõnumeid saata.

> [AZURE.NOTE] Pilveteenuse-seadme meilisõnumite saatmisel atribuutide nimesid ja väärtuste võib sisaldada ainult ASCII tähti ja numbreid, pluss ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Sõnumi elutsükkel

Vähemalt ühe korra kohaletoimetamise tagamiseks asjade jaoturi püsib cloud-seadme sõnumite järjekordades seadme kohta. Seadmete peab selgesõnaliselt kinnitama *lõpetamise* asjade jaoturi eemaldamiseks järjekorda. See tagab paindlikkust Ühenduvus ja seadme ebaõnnestumist.

Järgmisel joonisel on esitatud elutsükli olekus graafik cloud-seadme sõnum.

![Pilveteenuse-seadme sõnumi elutsükkel][img-lifecycle]

Kui teenus saadab sõnumi, leitakse *Enqueued*. Kui seadme soovib *saada* sõnumi, asjade jaoturi *lukud* (määrab selle oleku **nähtamatu**) sõnumi lubamisel muud teemad samasse seadmesse alustamiseks saada muid sõnumeid. Kui seadme teemat sõnumi töötlemine lõpule jõudnud, teavitab asjade jaoturi *lõpuleviimine* sõnum.

Seadme teha ka järgmist.

- *Hülga* sõnumit, mis põhjustab asjade jaoturi seadmiseks **Deadlettered** olekus. Märkus: seadmed ühendavad MQTT ei saa kõnest C2D sõnumeid.
- Sõnum, mis põhjustab asjade jaoturi panna kirja tagasi järjekorras olek seatud **Enqueued** *loobuda* .

Lõime võib nurjuda töödelda sõnumi asjade jaoturi teatamata. Sel juhul sõnumid automaatselt üleminek **nähtamatu** oleku tagasi **Enqueued** olekus pärast *nähtavuse (või lock) ajalõpp*. See ajalõpu vaikeväärtus on üks minut.

Sõnumi saate sujuv **Enqueued** ja **nähtamatud** Ühendriikide kõige korda asjade jaoturi atribuudi **kohaletoimetamise maksimaalne arv** määratud arvu jaoks. Pärast üleminekud arvu määrab asjade jaoturi **Deadlettered**sõnumi olek. Samuti asjade jaoturi määrab sõnumi olek **Deadlettered** pärast selle aegumise aeg (vt [aega live][lnk-ttl]).

Cloud-seadme sõnumitele juhendi leiate [õpetus: pilveteenuste-seadme sõnumite asjade jaoturi abil saatmise][lnk-c2d-tutorial]. Erinevate API-d ja SDK-d teemasid jätke cloud-seadme funktsionaalsust, leiate teemast [asjade jaoturi API -d ja SDK-d][lnk-sdks].

> [AZURE.NOTE] Tavaliselt cloud-seadme sõnumite täitke iga kord, kui sõnumi vähenemist ei mõjuta rakenduse loogika. Näiteks sõnumi sisu on edukalt kestnud kohalikku või toiming on edukalt täidetud. Sõnum võib ka läbiviimiseks siirdamiseks teabe, kelle kaotus ei mõjutaks taotluse funktsioon. Mõnikord pikaajalisi ülesandeid, saavad cloud-seadme sõnumi teostada pärast püsib kohalikku tööülesande kirjeldus. Seejärel saate teavitage rakenduse tagasi lõpuks ühe või mitme seadme pilve sõnumitega eri etappidega toimingu edenemine.

### <a name="message-expiration-time-to-live"></a>Sõnumi aegumise (live aeg)

Iga sõnumi cloud-seade on ka aegumise aeg. Seekord on seatud teenuse (atribuudis **ExpiryTimeUtc** ) või asjade jaoturi vaikimisi *aega live* määratud atribuudi asjade jaoturi abil. Vt [Cloud-seadme konfiguratsiooni suvandid][lnk-c2d-configuration].

> [AZURE.NOTE] Levinud võimalus ära sõnumi aegumine ja vältida sõnumite saatmine ühendamata seadmed, on lühike kellaaeg Live'i väärtuste määramiseks. Seda moodust saab sama tulemuse nagu seadme ühenduse oleku, säilitades olles tõhusamaks. Kui te taotleda sõnumi kinnitused, asjade jaoturi annab märku Millised seadmed saavad sõnumeid ja millised seadmed pole võrgus või ei ole.

### <a name="message-feedback"></a>Sõnumi tagasiside

Pilveteenuse-seadme sõnumi saatmisel saate teenuse taotleda ühe sõnumi tagasisidet lõplik olek vajaliku sõnumi kohaletoimetamisega.

- Kui määrate atribuudi **Ack** **positiivne**, luuakse asjade jaoturi tagasiside teade kui ja ainult siis, kui cloud-seadme sõnumi jõudnud oleku **lõpule viidud** .
- Kui määrate atribuudi **Ack** **negatiivne**, loob asjade jaoturi sõnumile tagasiside ainult juhul, kui, cloud-seadme sõnum jõuab **Deadlettered** .
- Kui määrate atribuudi **Ack** **täielik**, luuakse asjade jaoturi mõlemal juhul tagasiside teade.

> [AZURE.NOTE] ** **Ack** täis**, kui te ei saa tagasiside sõnumi, tähendab see, et tagasiside sõnum on aegunud. Teenust ei saa teada, mis juhtus algse sõnumi. Tegelikult teenuse tagama, et see saab tagasisidet enne selle lõppemist. Suurim lubatud aegumise aeg kaks päeva, mis võimaldab palju aega, et saada teenuse töötab uuesti tõrke ilmnemisel.

Nagu on selgitatud teemas [lõpp-punktid][lnk-endpoints], asjade jaoturi pakub sõnumitena tagasiside teenuse suunatud endpoint (**/messages/servicebound/feedback**) kaudu. Tagasiside saamise semantika on samad, mis on pilv-seadme sõnumite ja on sama [sõnumi elutsükli][lnk-lifecycle]. Võimaluse korral on sõnumi tagasiside batched ühes sõnumis, järgmises vormingus:

| Atribuut | Kirjeldus |
| -------- | ----------- |
| EnqueuedTime | Ajatempli, mis näitab, kui sõnum on loodud. |
| Kasutajanimi | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

Sisu on JSON seeriasertide array kirjeid, igal versioonil järgmised atribuudid:

| Atribuut | Kirjeldus |
| -------- | ----------- |
| EnqueuedTimeUtc | Ajatempli, mis näitab, kui sõnumi tulemustest juhtus. Näiteks seadme lõpetatud või sõnumi aegunud. |
| OriginalMessageId | Cloud-seadme sõnumi, millele see teave tagasiside puudutab **MessageId** . |
| StatusCode | Nõutav täisarv. Tagasiside sõnumid, mis on loodud asjade jaoturi kasutada. <br/> 0 = edu <br/> 1 = sõnum on aegunud <br/> 2 = maksimaalne arv ületab <br/> 3 = sõnumi tagasi lükatud |
| Kirjeldus | Stringi **StatusCode**väärtused. |
| DeviceId | **DeviceId** , millele see tükk tagasiside puudutab cloud-seadme sõnumi sihtrakenduse seadme. |
| DeviceGenerationId | **DeviceGenerationId** , millele see tükk tagasiside puudutab cloud-seadme sõnumi sihtrakenduse seadme. |


>[AZURE.IMPORTANT] Teenuse peate määrama **MessageId** cloud-seadme sõnumi saaksid selle tagasiside vastavus algse sõnumi.

Järgmises näites on kujutatud tagasiside sõnumi kehasse.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Pilveteenuse-seadme konfiguratsiooni suvandid

Iga asjade jaoturi seab järgmist konfiguratsiooni suvandid cloud-seadme sõnumside.

| Atribuut | Kirjeldus | Vahemik ja vaikeväärtus |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Vaikimisi TTL cloud-seadme sõnumite jaoks. | ISO_8601 intervall kuni 2-d (miinimum 1 minut). Vaikimisi: 1 tund. |
| maxDeliveryCount | Maksimaalne arv järjekorrad cloud-seade-seadme kohta. | 1 kuni 100. Vaikimisi: 10. |
| feedback.ttlAsIso8601 | Säilituspoliitika teenuse seotud tagasisidet sõnumite jaoks. | ISO_8601 intervall kuni 2-d (miinimum 1 minut). Vaikimisi: 1 tund. |
| feedback.maxDeliveryCount | Tagasiside järjekorra maksimaalne arv. | 1 kuni 100. Vaikimisi: 100. |

Lisateavet leiate teemast [loomine asjade jaoturi][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Seadme pilve sõnumite lugemine

Asjade jaoturi seab lõpp oma tagaandmebaas teenuste seadme pilve oma jaoturi saadud sõnumeid lugeda. Lõpp-punkti on sündmuse jaoturi ühilduv, mille abil saate kasutada mis tahes soovitud menetlustele sündmuse jaoturi teenuse toetab sõnumite lugemiseks.

Kui kasutate [Azure teenuse siini SDK .net-i] [ lnk-servicebus-sdk] või [Sündmuse jaoturi – sündmuse protsessor Host][lnk-eventprocessorhost], saate kasutada mis tahes asjade jaoturi ühendusstringi vajalikke õigusi. Seejärel kasutada **sõnumite/sündmuste** sündmuse jaoturi nime.

Kui kasutate SDK-d (või toote integratsioon), mis on teadlikud asjade jaoturi, sündmuse jaoturi ühilduv lõpp-punkt- ja sündmuse jaoturi ühilduv nimi peab toomiseks asjade jaoturi sätted [Azure portaali][lnk-management-portal]:

1. Klõpsake asjade jaoturi tera, **sõnumside**.
2. Jaotises **seadme pilve sätted** leiate järgmised väärtused: **sündmuse jaoturi ühilduv lõpp-punkti**, **sündmuse jaoturi ühilduv nimi**ja **sektsioonid**.

    ![Seadme pilve sätted][img-eventhubcompatible]

> [AZURE.NOTE] Kui SDK nõuab **Hostname** või **Namespace** väärtuse, kui eemaldada skeemi **sündmuse jaoturi ühilduv lõpp-punkti**. Näiteks kui teie sündmuse jaoturi ühilduv lõpp-punkti **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **hostname (hostinimi)** oleks **iothub ns-myiothub 1234.servicebus.windows.net**ja **Namespace** oleks **iothub ns-myiothub 1234**.

Seejärel saate **ServiceConnect** õigustega määratud sündmuse jaoturi ühenduse ühiskasutuses Accessi turbepoliitika.

Kui teil on vaja koostada eelmise teabe abil soovitud sündmus jaoturi ühendusstring, kasutage järgmist mustrit:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Järgmises loendis SDK-d ja integratsioon, mida saate kasutada sündmuse jaoturi ühilduv lõpp-punktid, mis seab asjade jaoturi abil:

* [Java sündmuse jaoturi klient](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm tila](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Github saate vaadata [tila allikas](https://github.com/apache/storm/tree/master/external/storm-eventhubs) .
* [Apache Spark integreerimine](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile rohkem teavet vahetada sõnumeid asjade jaoturi abil.

## <a name="message-format"></a>Sõnumi vorming

Asjade jaoturi sõnumite hulka kuuluvad:

* *Süsteemi atribuutide*kogum. Asjade jaoturi tõlgendab või määrab atribuudid. See on lülitame.
* *Teenuserakenduse atribuutide*kogum. Sõnastiku stringi atribuudid, mida saate määratleda rakendus ja Accessi vajamata deserialiseerimiseks sõnumi kehasse. Asjade jaoturi muudab kunagi neid atribuute.
* Läbipaistmatu binaarsed asutuse.

Kuidas sõnum on erinevaid protokolle kodeeritud kohta leiate lisateavet teemast [asjade jaoturi API -d ja SDK-d][lnk-sdks].

Järgmises tabelis on loetletud Süsteemiatribuudid asjade jaoturi sõnumite kogum.

| Atribuut | Kirjeldus |
| -------- | ----------- |
| MessageId | Sõnumi, kasutatakse taotluse-vasta mustrite kasutaja kõigest ID. Vorming: Tõstutundlikud string (kuni 128 tähemärki) tähtnumbrilised märgid ASCII 7-bitise + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Järjekorranumber | Arv (kordumatud seadme-järjekord kohta) määratud asjade jaoturi iga cloud-seadme sõnumile. |
| Kui soovite | [Pilveteenuse-seadme] määratud sihtkohta[ lnk-c2d] sõnumeid. |
| ExpiryTimeUtc | Kuupäev ja kellaaeg sõnumite aegumist. |
| EnqueuedTime | Kuupäev ja kellaaeg, asjade jaoturi võttis sõnumi. |
| CorrelationId | Stringi atribuudi taotluse taotlus-vasta mustrid MessageId tavaliselt sisaldava sõnumi vastuse. |
| Kasutajanimi | ID abil saate määrata sõnumite päritolu. Kui sõnumid on loodud asjade jaoturi, on seatud `{iot hub name}`. |
| ACK | Tagasiside sõnumi generaator. Atribuut kasutatakse cloud-seadme sõnumite taotleda asjade keskuse loomiseks tagasiside sõnumite tulemusena sõnumi tarbimine seadme abil. Võimalikud väärtused:, **mitte keegi** (vaikeväärtus): pole tagasiside sõnum on loodud, **positiivne**: tagasiside teate, kui sõnum on lõpule viidud, **negatiivne**: tagasiside teate, kui sõnum on aegunud (või maksimaalne arv on jõudnud) ilma seadet või **täielik**lõpule: negatiivsete. Lisateavet leiate teemast [sõnumi tagasiside][lnk-feedback]. |
| ConnectionDeviceId | ID määratud asjade jaoturi seadme pilve sõnumitele. See sisaldab **deviceId** seadet, mis saadetakse sõnum. |
| ConnectionDeviceGenerationId | ID määratud asjade jaoturi seadme pilve sõnumitele. See sisaldab **generationId** (nagu iga [seadme identiteedi atribuudid][lnk-device-properties]) seadet, mis saadetakse sõnum. |
| ConnectionAuthMethod | Autentimise meetodit määratud asjade jaoturi seadme pilve sõnumitele. Atribuut sisaldab teavet autentimise meetodit, mida kasutatakse sõnumi saatnud seade. Lisateabe saamiseks lugege teemat [seadme Cloud tüssamisvastast][lnk-antispoofing].|

## <a name="communication-protocols"></a>Protokollide

Asjade jaoturi abil saavad kasutada [MQTT]seadmete[lnk-mqtt], MQTT üle WebSockets [AMQP][lnk-amqp], AMQP WebSockets ja HTTP protokolle seadme-side side. Järgmises tabelis on ära toodud üksikasjalik soovitusi oma valiku Protocol (protokoll):

| Protocol (protokoll) | Kui valida selle Protocol (protokoll) |
| -------- | ------------------------------------ |
| MQTT <br> MQTT üle WebSocket     | Kasutage kõigis seadmetes, mis ei nõua ühendada mitu seadet (iga oma seadme kohta mandaadi) sama TLS-ühenduse kaudu. |
| AMQP <br> AMQP üle WebSocket    | Kasutada välja ja pilveteenuse lüüside ära ühenduse multipleksimise kõigis seadmetes. |
| HTTP KAUDU    | Kasutage seadmed, mis ei toeta muude protokolle. |

Kui valite oma seadme-side side protokoll, võtke arvesse järgmist.

* **Pilveteenuse-seadme mustri**. HTTP ei saa rakendada serveri tõuketeatised tõhusalt. Sellisel kujul HTTP kasutamisel küsitlus seadmete asjade jaoturi cloud-seadme sõnumite jaoks. See lähenemine on ebatõhus ja seadme asjade jaoturi. Jaotises Praegune HTTP juhistest peaks iga seadme küsitlus sõnumite iga 25 minuti järel või rohkem. Teisalt, MQTT ja AMQP toetavad serveri tõuketeatised kui cloud-seadme sõnumeid. Need võimaldavad sõnumite kohe sunnib asjade keskuse kaudu seade. Kui kohaletoimetamise latentsus ei sobi, MQTT või AMQP on parim Protokollid kasutada. Harva ühendatud seadmete HTTP toimib samuti.
* **Väli lüüsid**. MQTT ja HTTP kasutamisel ei saa ühendust luua mitu seadet (iga oma seadme kohta mandaadi) kasutamine samas TLS-ühenduse kaudu. Seega jaoks [välja lüüsi stsenaariumid][lnk-azure-gateway-guidance], nende Protokollid on optimaalsetesse, kuna need vajavad ühte TLS seost välja lüüsi ja asjade jaoturi iga seadme ühendatud välja lüüsi.
* **Madal ressursside seadmed**. MQTT ja HTTP teekide on väiksem andmed kui AMQP teekide väiksemates vähem ruumi. Kui seade on piiratud ressursid (näiteks alla 1 MB RAM-i), neid protokolle võib olla ainult protokolli rakendamist.
* **Võrgu läbikäiku**. Standardse AMQP protokolli kasutab porti 5671, ajal MQTT kuulab porti 8883, mis võivad põhjustada probleeme võrgu-HTTP Protokollid on suletud. MQTT üle WebSockets, AMQP WebSockets ja HTTP üle on saadaval, kasutatakse selle stsenaariumi.
* **Last suurus**. MQTT ja AMQP on kahendarvu protokollid, mille tulemuseks kompaktsema lõhkelaengu http-d.

> [AZURE.NOTE] HTTP kasutamisel iga seadme peaks küsitlus cloud-seadme sõnumite iga 25 minuti järel või veel. Arendamise käigus on aktsepteeritav, kui iga 25 minutit küsitlus.

## <a name="port-numbers"></a>Pordinumbrid

Seadmed saavad suhelda asjade jaoturi abil erinevate protokollide Azure. Tavaliselt liigub lahendus erinõuetega valik Protocol (protokoll). Järgmises tabelis on loetletud Väljaminev pordid, mis tuleb avada mõne seadme saama kasutada kindlat protokolli:

| Protocol (protokoll) | Port(s)) |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT üle WebSockets | 443    |
| AMQP     | 5671    |
| AMQP WebSockets üle | 443    |
| HTTP KAUDU     | 443     |
| LWM2M (haldus) | 5684 |

Kui olete loonud mõne asjade jaoturi Azure piirkonnas, hoiab jaoturi jaoturi eluiga sama IP-aadress. Siiski säilitada teenuse kvaliteeti, kui Microsoft viib asjade jaoturi erinevate skaala ühik seejärel määratakse uus IP-aadress.

## <a name="notes-on-mqtt-support"></a>Märkmeid MQTT tugi

Asjade jaoturi rakendab MQTT v3.1.1 protokoll järgmistele piirangutele ja teatud käitumine:

  * **QoS 2 ei toetata**. Kui seadme kliendi avaldab sõnumi **QoS**2, asjade jaoturi suleb võrguühendust. Kui seadme kliendi tellib teema **QoS**2, asjade jaoturi annab suurima QoS tase 1 **SUBACK** paketi.
  * **Säilita sõnumeid ei jää püsima**. Kui seadme kliendi avaldab sõnumi SÄILITA lipu seadmine 1, asjade jaoturi lisab on **x-valida-säilitamise** rakenduse atribuut sõnum. Sel juhul asjade jaoturi ei kao, Säilita sõnumi, kuid selle asemel edastab selle rakenduse tagaandmebaas.

Lisateabe saamiseks lugege teemat [Asjade jaoturi MQTT toetavad][lnk-devguide-mqtt].

Lõplik arvesse võtta, peaksite [Azure'i asjade protokoll lüüsi] [ lnk-azure-protocol-gateway] võimaldab teil suure jõudlusega kohandatud protokolli lüüsi, mis liidesed asjade jaoturi juurutamine. Lüüsi Azure'i asjade Protocol (protokoll) võimaldab teil kohandada seadme protokoll varasema MQTT juurutuste mahutamiseks või muud kohandatud protokollid. Seda moodust nõua, et käivitada ja kasutada kohandatud protokolli lüüsi.

## <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud teil on kasutada, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Nüüd saate teada, kuidas saata ja vastu võtta keskuse asjade sõnumeid, võib olla huvitatud arendaja juhend järgmisi teemasid:

- [Laadige failid seadmest][lnk-devguide-upload]
- [Seadme identiteedid asjade keskuses haldamine][lnk-devguide-identities]
- [Asjade jaoturi juurdepääsu reguleerimine][lnk-devguide-security]
- [Riigi ja konfiguratsioone sünkroonimine seadme kaksikud abil][lnk-devguide-device-twins]
- [Otsest meetodit seadmes][lnk-devguide-directmethods]
- [Mitmes seadmes tööde plaanimine][lnk-devguide-jobs]

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õppetükid

- [Azure'i asjade jaoturi kasutamise alustamine][lnk-getstarted-tutorial]
- [Kuidas saata cloud-seadme sõnumite asjade jaoturi abil][lnk-c2d-tutorial]
- [Protsessi asjade jaoturi seadme cloud sõnumite][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
