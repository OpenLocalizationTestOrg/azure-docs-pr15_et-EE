<properties
 pageTitle="Arendaja juhendi Teemad asjade jaoturi | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend, mis sisaldab asjade jaoturi lõpp-punktid, Turve, seadme identiteedi registri, mobiilsideseadmete halduse ja sõnumside"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure'i asjade jaoturi arendaja juhend

Azure'i asjade jaoturi on täielikult hallatav teenus, mis aitab lubada vahelise miljoneid asjade seadmete turvalist kahesuunalise suhtluse ja rakenduse uuesti lõpetada.

Azure'i asjade jaoturi pakub:

* Turvaline side seadme kohta turvalisus identimisteavet kasutades ja juurdepääsu juhtimine.
* Usaldusväärne seadme pilve ja pilveteenuse-seadme hyper skaala sõnumside.
* Lihtne seade seadme teekide jaoks kõige populaarsemate keelte ja platvormide jaoks loodud Ühenduvus.

See asjade jaoturi arendaja juhend hõlmab järgmisi artikleid:

- [Sõnumite saatmiseks ja vastuvõtmiseks asjade jaoturi abil] [ devguide-messaging] sõnumside funktsioonid, mis asjade jaoturi seab (seadme pilve ja pilveteenuse-seade). See artikkel sisaldab ka sõnumivormingute, nt teemade kohta teabe ja toetatud suhtlus protokollid ja nad kasutavad pordinumbrid.
- [Laadige failid seadmest,] [ devguide-upload] kirjeldatakse, kuidas saate failid mõne seadme kaudu üles laadida. Artikkel sisaldab ka teavet teatiste teemade kohta., uplaod protsessi saab saata.
- [Seadme identiteedid asjade keskuses hallata] [ devguide-identities] kirjeldatakse, mida teavet iga asjade jaoturi seadme identiteedi registri talletab ja kuidas pääsete juurde ja saate seda muuta.
- [Asjade jaoturi juurdepääsu] [ devguide-security] kirjeldatakse mudelit saab anda juurdepääsu asjade jaoturi funktsioonile, mõlemad seadmete jaoks ja cloud komponendid. Artikkel sisaldab teavet kasutades sõned ja X.509 serdid ja üksikasjad, saate anda õigused.
- [Riigi ja konfiguratsioone sünkroonimine seadme kaksikud abil] [ devguide-device-twins] kirjeldatakse *seadme Double* mõiste ja see seab selle seadme sünkroonimine nt funktsioone. See artikkel sisaldab teavet seadme Double talletatud andmed.
- [Otsest meetodit seadmes] [ devguide-directmethods] kirjeldatakse elutsükli otsene meetod, teavet autonoomsest meetodite tagaandmebaas rakenduste seadmes ja hallata oma seadmes otsene meetod.
- [Mitmes seadmes tööde] [ devguide-jobs] kirjeldatakse, kuidas saate ajastada tööde haldamine mitmes seadmes. Artiklist kirjeldatakse, kuidas tööd teha toiminguid nagu otsese meetodi, mis on devcie abil devcie Double värskendamine täitmine edastada. Samuti kirjeldatakse päringu töö olek.
- [Viide - asjade jaoturi lõpp-punktid] [ devguide-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus. Selles artiklis kirjeldatakse ka, kuidas saate kasutada välja lüüsi lubamiseks mõne seadme ühenduse loomiseks oma asjade jaoturi lõpp-punktid.
- [Viide - päringukeele kaksikud, võimalused ja töö] [ devguide-query] kirjeldatakse selle päringu keel, mis võimaldab teabe toomiseks teie keskuse kohta oma seadmes kaksikud ja -tööde haldamine.
- [Viide - kui ka pidurdamise] [ devguide-quotas] nähtud asjade jaoturi teenuse ja ahendamise käitumise kuvamiseks, kui ületate kvoodi eeldatavaid kokkuvõte.
- [Viide - seade ja SDK-d] [ devguide-sdks] loendite SDK-d, saate töötada nii seadmest ja teenuse rakendusi, mis teie asjade jaoturi suhelda. See artikkel sisaldab linke online API dokumentatsiooni.
- [Viide - asjade jaoturi MQTT tugi] [ devguide-mqtt] saate üksikasjalikku teavet selle kohta, kuidas asjade jaoturi toetab protokolli MQTT. Artiklist kirjeldatakse sisseehitatud soovitud SDK-d MQTT protokolli tugi ja MQTT protokoll otse kasutamise kohta leiate lisateavet.
- [Sõnastik] [ devguide-glossary] levinud asjade jaoturi seotud terminite loendit.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

