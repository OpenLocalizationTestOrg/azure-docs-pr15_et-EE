<properties
 pageTitle="Azure'i asjade keskuse kaudu Azure'i sündmuse jaoturi võrdlus | Microsoft Azure'i"
 description="Esiletõstmine Funktsioonierinevused ja kasutamise juhtudel Azure'i asjade jaoturi ja Azure sündmuse jaoturi teenuste võrdlus."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Azure'i asjade jaoturi ja Azure sündmuse jaoturi võrdlus

Peamine kasutus juhul asjade jaoturi jaoks on koguda telemeetria seadmetest. Seetõttu asjade jaoturi võrreldakse sageli [Azure'i sündmuse jaoturi][]. Asjade jaoturi, nagu on sündmuse jaoturi sündmuse töötlemiseks teenus, mis võimaldab sündmus ja telemeetria sissepääsu suuri tasandil pilveteenusesse madal latentsus ja usaldusväärsus.

Teenused on siiski paljud erinevused, mis on üksikasjalikult kirjeldatud järgmises tabelis.

| Ala | Asjade jaoturi | Sündmuse jaoturi |
| ---- | ------- | ---------- |
| Suhtlus mustrite | Võimaldab seadme pilve ja pilveteenuse-seadme sõnumside. | Ainult võimaldab sündmuse sissepääsu (tavaliselt peetakse seadme pilve stsenaariumid). |
| Seadme protokolli tugi | Toetab MQTT, AMQP, AMQP WebSockets ja HTTP üle. Lisaks asjade jaoturi töötab [Azure'i asjade protokoll lüüsi][lnk-azure-protocol-gateway], kohandatavate Protocol (protokoll) lüüsi rakendada kohandatud Protokollid toetamiseks. | Toetab AMQP AMQP WebSockets ja HTTP üle. |
| Turvalisus | Pakub seadme kohta identiteedi ja tühistatava juurdepääsu reguleerimine. Leiate [jaotisest asjade jaoturi arendaja juhend]. | Sündmuse jaoturi kogu [ühiskasutusega juurdepääsupoliitikaid]pakub[Event Hubs - security], piiratud tühistamise toetavad [avaldaja poliitika]kaudu[Event Hubs publisher policies]. Asjade lahendused on sageli vaja rakendada kohandatud lahenduse seadme kohta identimisteabe ja mõõdud Tüssamisvastane. |
| Toimingute jälgimine | Võimaldab asjade lahenduste tellida suurel hulgal erinevaid seadme identiteedi haldamine ja Ühenduvus sündmusi, näiteks üksikute seadme autentimise vigu, pidurdamise ja halb vorming erandid. Need võimaldavad kiiresti tuvastada ühendusprobleemide üksikute seadme tasemel. | Seab ainult liitväärtuse mõõdikute. |
| Skaala | On optimeeritud tugi miljoneid korraga ühendatud seadmete. | Toetab üheaegselt ühenduste--kuni 5000 AMQP ühendused [Azure'i teenus siini kvootide][]kohta rohkem piiratud arv. Teisalt, sündmuse jaoturi võimaldab määratud sektsioon iga saadetud sõnumi eest. |
| Seadme SDK-d | [Seadme SDK-d] pakub[ Azure IoT Hub SDKs] erinevaid platvormide ja keelte jaoks. | On toetatud .net-i ja C. Pakub AMQP ja HTTP saada liidesed. |
| Faili üleslaadimine | Võimaldab asjade solutions pilveteenusesse seadmest faile üles laadida. Sisaldab faili teatis endpoint töövoo integreerimise ja soovitud kategooria silumine tugi jälgimine. | Kasutab taotluste sisse mustri käsitsi failide seadmetest ning seadmete salvestusruumi võti kande esitama. |

Kokkuvõttes isegi juhul, kui kasutada ainult puhul on seadme pilve telemeetria sissepääsu, asjade keskus võimaldab teenus, mis on mõeldud asjade seadme Ühenduvus. Jätkab Laiendage väärtust ettepanekud stsenaariumi puhul asjade kohased funktsioone. Sündmuse jaoturi on mõeldud sündmuse sissepääsu suuri alal, mõlemad muu andmekeskuse ja sisene-andmekeskuse stsenaariumid kontekstis.

See ei ole aeg-ajalt kasutada nii asjade jaoturi ja sündmuse jaoturi sama lahendus--kus asjade jaoturi tegeleb seadme pilve side ja sündmuse jaoturi tegeleb hiljem – sündmuse sissepääsu reaalajas töötlemine mootorite sisse.

## <a name="next-steps"></a>Järgmised sammud

Juurutamise asjade jaoturi kavandamise kohta leiate lisateavet teemast [mastaapimine, HA ja DR][lnk-scaling].

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

[Azure'i sündmuse jaoturi]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Turvalisus jaotises asjade jaoturi arendaja juhend]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure'i teenus siini kvoote]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
