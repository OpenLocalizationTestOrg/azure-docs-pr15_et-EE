<properties
 pageTitle="Arendaja juhend - asjade jaoturi lõpp-punktid | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - teave asjade jaoturi lõpp-punktid"
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

# <a name="reference---iot-hub-endpoints"></a>Viide - asjade jaoturi lõpp-punktid

## <a name="list-of-iot-hub-endpoints"></a>Loendi asjade jaoturi lõpp-punktid

Azure'i asjade jaoturi on mitme rentniku teenus, mis pakub oma funktsioone, mis osalejate. Järgmisel joonisel on esitatud erinevad lõpp-punktid, et asjade jaoturi seab.

![Asjade jaoturi lõpp-punktid][img-endpoints]

Lõpp-punktid kirjeldus on:

* **Ressursi pakkuja**. Asjade jaoturi ressursi pakkuja seab on [Azure ressursihaldur] [ lnk-arm] kasutajaliides, mis võimaldab Azure tellimuse omanikud loomine ja kustutamine asjade jaoturi asjade jaoturi atribuutide värskendamine. Asjade jaoturi atribuudid haldavad [jaoturi taseme turbepoliitikate][lnk-accesscontrol], mitte seadme taseme juurdepääsu reguleerimine ja funktsionaalne Valikud cloud-seade ja seadme pilve sõnumside. Asjade jaoturi ressursi pakkuja võimaldab teil eksportida [seadme identiteedid][lnk-importexport].
* **Seadme identiteedi haldamine**. Iga asjade jaoturi seab kogumi haldamiseks seadme identiteedid HTTP ülejäänud lõpp-punktid (loomine, tuua, värskendamine ja kustutamine). [Seadme identiteedid] [ lnk-device-identities] kasutatakse seadme autentimise ja juurdepääsu juhtimine.
* **Seadme Double haldamine**. Iga asjade jaoturi seab kogumi teenuse suunatud HTTP ülejäänud lõpp-punkti päringu ja värskendada [seadme kaksikud] [ lnk-twins] (Värskenda sildid ja atribuudid).
* **Töö haldus**. Iga asjade jaoturi seab kogumi teenuse suunatud HTTP ülejäänud lõpp-punkti päringu ja [hallata][lnk-jobs].
* **Seadme lõpp-punktid**. Iga seadme ette valmistatud registris seadme identiteedi jaoks asjade jaoturi seab kogum, mida seadme abil saate sõnumite saatmiseks ja vastuvõtmiseks lõpp-punktid:
    - *Seadme pilve sõnumeid saata*. Kasutage seda lõpp-punkti [seadme pilve sõnumite]saatmiseks[lnk-d2c].
    - *Sõnumite vastuvõtu cloud-seade*. Seade kasutab seda lõpp-punkti suunatud [cloud-seadme sõnumite][lnk-c2d].
    - *Kontaktiga faile üles laadida*. Seadme kasutab seda lõpp-punkti saada Azure'i salvestusruumi SAS URI asjade keskuse kaudu [faili]üles laadimiseks[lnk-upload].
    - *Laadi alla ja värskenduse Double atribuudid*. Seadme kasutab seda lõpp-punktid, et oma [seadme Double][lnk-twins]kasutaja atribuudid.
    - *Vastuvõtu otsese meetodite taotlused*. Seade kasutab seda lõpp-punktid kuulamiseks [otsese meetodite][lnk-methods]'s Kutsed.

    Nende lõpp-punktid on avatud, kasutades [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 ja [AMQP 1.0] [ lnk-amqp] protokollid. Pange tähele, et AMQP on saadaval ka üle [WebSockets] [ lnk-websockets] pordi 443.
    
    Kaksikute ja meetodite endpoins on saadaval ainult abil [MQTT v3.1.1][lnk-mqtt].

* **Teenuse lõpp-punktid**. Iga asjade jaoturi seab kogumi oma rakenduse tagasi end abil saate suhelda seadmetes lõpp-punktid. Nende lõpp-punktid on praegu ainult exposed abil [AMQP] [ lnk-amqp] protokolli, välja arvatud meetod kutsumise näitaja, mis on esitatud HTTP 1.1 kaudu.
    - *Sõnumite vastuvõtu seadme pilve*. Selle lõpp-punkti ühildub [Azure'i sündmuse jaoturi][lnk-event-hubs]. Tagaandmebaas teenust kasutada [seadme pilve sõnumite] lugemiseks[ lnk-d2c] saadetud seadmetes.
    - *Pilveteenuse-seadme sõnumeid saata ja vastu võtta kohaletoimetamise litsents*. Nende lõpp-punktid lubamine rakenduse tagasi end usaldusväärne [cloud-seadme sõnumite]saatmiseks[lnk-c2d], ja vastava kohaletoimetamise või aegumise litsents.
    - *Vastuvõtu faili teatised*. See sõnumside lõpp-punkti võimaldab teil saada teatisi, kui faili üleslaadimine edukalt seadmetes. 
    - *Otsest meetod kutsumise*. Selle lõpp-punkti võimaldab tagaandmebaas teenuse [otsene meetod] [ lnk-methods] seadmes.

Soovitud [asjade jaoturi API -d ja SDK-d] [ lnk-sdks] artikkel kirjeldab erinevaid võimalusi, juurde pääseda nende lõpp-punktid.

Lõpetuseks, on oluline arvestage, et kõik asjade jaoturi lõpp-punktid [TLS] [ lnk-tls] krüptimata/turvamata kanalite kunagi varem esitatud protokoll ja pole lõpp-punkti.

## <a name="field-gateways"></a>Väli lüüsid

Asjade lahendusele, *välja lüüsi* asub seadmetes oma asjade jaoturi lõpp-punktid. See asub tavaliselt seadmetes. Seadmetes otse välja lüüsi abil suhelda seadmete ei toeta protokolli. Välja lüüsi ühendab protokoll, mis toetab asjade jaoturi abil asjade jaoturi lõpp. Välja lüüsi võib olla väga eriotstarbeline riistvara või madala võimsusega arvutis, kus töötab tarkvara, millega lõpuni stsenaarium, mis on mõeldud lüüsi.

Saate kasutada [Azure asjade lüüsi SDK] [ lnk-gateway-sdk] välja lüüsi rakendada. See SDK pakub teatud funktsioone nagu võimalus multiplex asjade jaoturi mitmes seadmes peale sama ühendust teatis.

## <a name="next-steps"></a>Järgmised sammud

Muud viited Teemad tutvustava asjade jaoturi arendaja on järgmised:

- [Päringu keel kaksikud, võimalused ja tööde haldamine][lnk-devguide-query]
- [Kui ka ahendamine][lnk-devguide-quotas]
- [Asjade jaoturi MQTT tugi][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md