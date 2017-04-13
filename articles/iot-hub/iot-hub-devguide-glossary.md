<properties
 pageTitle="Arendaja juhend - sõnastik | Microsoft Azure'i"
 description="Asjade jaoturi seotud levinud terminite sõnastik"
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

# <a name="glossary-of-iot-hub-terms"></a>Asjade jaoturi terminite sõnastik

Selles artiklis on loetletud mõned levinud tingimused, mis on seotud asjade jaoturi.

## <a name="advanced-message-queueing-protocol-amqp"></a>Täiustatud sõnumi Queueing protokolli (AMQP)

[AMQP](https://www.amqp.org/) on üks sõnumside protokoll asjade jaoturi toetab seadmed suhtlemiseks. Sõnumside protokoll asjade jaoturi toetab kohta leiate lisateavet teemast [asjade jaoturi abil sõnumite saatmiseks ja vastuvõtmiseks](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Pilveteenuse seadme (C2D)

Tavaliselt kasutatakse viidata ühendatud seadet asjade keskuse kaudu saadetud sõnumid. Sageli, on need sõnumid käsud, paluge seadme mingi toimingu. Lisateavet leiate teemast [asjade jaoturi abil sõnumite saatmiseks ja vastuvõtmiseks](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Tingimus

Viitab seadme olek teave, näiteks ühenduvuse meetodit praegu kasutusel, nagu seadme rakendus. Seadmete saate teatada ka nende võimaluste kohta. Tingimus ja võimalus Double seadme abil saate teha päringuid.

## <a name="data-point-message"></a>Andmepunkti sõnum

Andmepunkti – sõnumi on cloud-seadme sõnum, mis sisaldab telemeetria andmeid, nt tuulekiirus või temperatuuri.

## <a name="desired-properties"></a>Soovitud atribuudid

Seadme kaksikud kontekstis soovitud atribuudid kasutatakse *atribuutide teatatud* koos sünkroonimine seadme konfiguratsiooni või tingimus. Soovitud atribuudid saate seada ainult rakendus uuesti end ja on kinni seadme rakendus. 

## <a name="device-to-cloud-d2c"></a>Seadme pilveteenuse (D2C)

Tavaliselt kasutatakse viidata asjade jaoturiga ühendatud seadet saadetud sõnumid. Need sõnumid võivad olla [andmepunkt](#data-point-message) või [interaktiivsed](#interactive-message) sõnumeid. Lisateavet leiate teemast [asjade jaoturi abil sõnumite saatmiseks ja vastuvõtmiseks](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Seadme identiteedi registri

[Seadme identiteedi registri](iot-hub-devguide-identity-registry.md) on sisseehitatud osa asjade jaoturi, mis talletab teavet üksikute seadmete ühenduse jaoturi lubatud.

## <a name="device"></a>Seadme

Asjade kontekstis seade on tavaliselt väikesemahuliste, autonoomne arvuti seade võib andmete kogumise või piirata muudes seadmetes. Näiteks seade võib olla keskkonna jälgimise seadmes või domeenikontrolleri, jootmise ja ventilatsiooni süsteemide kasvuhoones.

## <a name="device-twin"></a>Seadme Double

[Seadme Double](iot-hub-devguide-device-twins.md) on asjade keskuses füüsilise seadme tingimus ja konfiguratsiooni sätted. Seadme Double abil saate hallata seadme konfiguratsiooni.

## <a name="direct-method"></a>Otsene meetod

[Otsene meetod](iot-hub-devguide-direct-methods.md) on viis teile meetodi käivitamiseks käivitada seadmes dokumentidega API oma asjade keskuse kohta.

## <a name="event-hub-compatible-endpoint"></a>Sündmuse jaoturi ühilduv lõpp-punkti

Seadme pilve oma asjade jaoturi saadetud sõnumite lugemiseks saate ühendust luua oma jaoturi lõpp ja mis tahes sündmuse jaoturi ühilduv meetodi abil saate sõnumeid lugeda. Sündmuse jaoturi ühilduv meetodid hõlmavad kasutades sündmuse jaoturi SDK-d ja Azure voo analüütika.

## <a name="field-gateway"></a>Välja lüüsi

Välja lüüsi võimaldab ühenduvuse seadmete jaoks ei saa ühendust luua otse asjade jaoturi ja tavaliselt juurutatud kohalikult teie seadmetega. Lisateavet leiate teemast [mis on Azure asjade jaoturi?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Töö

Teie lahendus tagasi end abil saate töö plaanimine ja jälgida tegevuste registreeritud oma asjade jaoturi seadmete kogum. Tegevuste hulka seadme Double soovitud atribuutide värskendamine, seadme Double siltide värskendamine ja kasutada otsest võimalust.

## <a name="protocol-gateway"></a>Lüüsi Protocol (protokoll)

Lüüsi Protocol (protokoll) on tavaliselt paigutatud pilveteenuses ja pakub protokolli tõlketeenuseid ühenduse asjade jaoturi seadmete jaoks. Lisateavet leiate teemast [mis on Azure asjade jaoturi?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interaktiivne sõnum

Interaktiivne meilisõnumi on cloud-seadme sõnum, mis käivitab toimingu tagasi lõpuks rakenduse kohe. Näiteks võite seadme saata helisignaal tõrge, mis tuleks automaatselt sisse logitud rakendusse CRM süsteemi kohta.

## <a name="iot-hub"></a>Asjade jaoturi

Asjade jaoturi on täielikult hallatud Azure'i teenus, mis võimaldab turvalist kahesuunaline miljonid asjade seadmed ja lahenduse tagasi end vahelise suhtluse. Lisateavet leiate teemast [mis on Azure asjade jaoturi?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>Asjade komplekti

Azure'i asjade komplekti paketid koos mitme Azure'i teenuste eelkonfigureeritud lahendusi. Neid eelkonfigureeritud lahendusi, mis võimaldavad teil lõpuni rakendusi tavastsenaariumid asjade kiiresti hakata. Lisateavet leiate teemast [mis on Azure asjade komplekti?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Töö

[Töö](iot-hub-devguide-jobs.md) asjade keskuses võimaldab toiminguid nagu püsivara uuendada oma jaoturiga ühendatud mitmes seadmes.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) on üks sõnumside protokoll asjade jaoturi toetab seadmed suhtlemiseks. Sõnumside protokoll asjade jaoturi toetab kohta leiate lisateavet teemast [asjade jaoturi abil sõnumite saatmiseks ja vastuvõtmiseks](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Teatatud atribuudid

Seadme kaksikud kontekstis teatatud atribuudid kasutatakse koos *soovitud atribuudid* sünkroonimine seadme konfiguratsiooni või tingimus. Teatatud atribuudid saate seada ainult seadme rakendus ja saate lugeda ja, käitades rakenduses tagasi lõpuks.

## <a name="tags"></a>Sildid

Devcie kaksikud kontekstis, sildid on seadme metaandmete talletatud ja toodavate rakenduse tagasi kujul JSON dokumendi lõppu. Siltide ei ole nähtav, rakenduste seadmes.

## <a name="system-properties"></a>Süsteemiatribuudid

Seadme kaksikud kontekstis Süsteemiatribuudid on kirjutuskaitstud ja seadme kasutamise näiteks viimase tegevuse aja ja ühenduse olek.