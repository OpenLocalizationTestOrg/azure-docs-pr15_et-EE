<properties
 pageTitle="Arendaja juhend - kui ka pidurdamise | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - kirjeldus, mida rakendada asjade jaoturi ja ahendamise oodatud käitumine"
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

# <a name="reference---quotas-and-throttling"></a>Viide - kui ka ahendamine

## <a name="quotas-and-throttling"></a>Kui ka ahendamine

Iga Azure'i tellimus võib olla kuni 10 asjade jaoturi.

Iga asjade jaoturi on ette valmistatud teatud SKU üksuste arvu (Lisateavet leiate [Azure'i asjade jaoturi hinnad][lnk-pricing]). SKU ja üksuste arvu määratlemine saadetavate sõnumite maksimaalne igapäevane piirmäära.

Kasutatava SKU siinse ahendamise piirangud, mis asjade jaoturi jõustab kõigi toimingute kohta.

## <a name="operation-throttles"></a>Toiming pidurdab

Toiming pidurdab on määr piirangud, rakendatakse minute vahemikud, mis on mõeldud kuritarvitamise vältimiseks. Asjade jaoturi proovib esitus võimaluse tõrgete vältimiseks, kuid see algab esitus erandid on ahendamise rikkumise liiga kauaks.

Järgmises loendis jõustatud pidurdab. Üksikute jaoturiga viidata väärtused.

| Ahendamise | Väärtuse keskuse kohta. |
| -------- | ------------- |
| Identiteedi registri toimingud (loomine, tuua, loendi, värskendada, kustutada) | ühiku/5000/min (S3) jaoks <br/> ühiku/100/min (S1 ja S2) jaoks. |
| Seadme ühendused | ühiku/6000/sec (S3) jaoks 120/sec/ühik (S2), ühiku/12/sec (S1) jaoks. <br/>Minimaalselt 100 sekundis. <br/> Näiteks kahe S1 üksused on 2\*12 = 24/sec, kuid teil on vähemalt 100 sekundis kogu teie üksused. Üheksa S1 üksused, on teil 108 sekundis (9\*12) oma üksuste üle. |
| Seadme pilve saadab | ühiku/6000/sec (S3) jaoks 120/sec/ühik (S2), ühiku/12/sec (S1) jaoks. <br/>Minimaalselt 100 sekundis. <br/> Näiteks kahe S1 üksused on 2\*12 = 24/sec, kuid teil on vähemalt 100 sekundis kogu teie üksused. Üheksa S1 üksused, on teil 108 sekundis (9\*12) oma üksuste üle. |
| Pilveteenuse-seade saadab | ühiku/5000/min (jaoks S3) 100 min/ühiku (S1 ja S2) jaoks. |
| Pilveteenuse-seade saab | ühiku/50000/min (S3) jaoks 1000 min/ühiku (S1 ja S2) jaoks. |
| Faili üleslaadimine toimingud | 5000 faili üles laadida ja ühiku/teatised/min (jaoks S3), 100 faili üleslaadimine teatised min/ühiku (S1 ja S2). <br/> 10000 SAS URI-d võib olla korraga Azure Storage konto jaoks.<br/> 10 SAS URI-d seade võib olla välja korraga. | 

On oluline selgitada, et *seadme ühendused* ahendamise reguleerib määr, kus võimalik kindlaks uue seadme ühendused soovitud asjade jaoturi ja pole korraga ühendatud seadmete maksimaalne arv. Funktsiooni ahendamise sõltub ühikute, mis on ette valmistatud jaoturi jaoks.

Näiteks kui ostate S1 ühe üksuse, saate ahendamise 100 ühenduste sekundis. See tähendab, et 100 000 seadmete ühendamiseks kulub vähemalt 1000 sekundit (16 minutit). Siiski võib olla nii palju korraga ühendatud seadmete, kui teil on oma seadme identiteedi registris registreeritud.

Asjade jaoturi põhjalik arutelu pidurdamise käitumine, leiate ajaveebipostitusest [asjade jaoturi pidurdamise ja][lnk-throttle-blog].

>[AZURE.NOTE] Mis tahes ajal, on võimalik suurendada suureneva ettevalmistatud ühikute soovitud asjade keskuses kvootide ega ahendamise piirangud.

>[AZURE.IMPORTANT] Identiteedi registri toimingud on mõeldud käitusaja mobiilsideseadmete halduse ja ettevalmistamise stsenaariumid. Lugemise või värskendamine suure hulga seadme identiteedid on toetatud [importimine ja eksportimine töö][lnk-importexport].

## <a name="next-steps"></a>Järgmised sammud

Muud viited Teemad tutvustava asjade jaoturi arendaja on järgmised:

- [Asjade jaoturi lõpp-punktid][lnk-devguide-endpoints]
- [Päringu keel kaksikud, võimalused ja tööde haldamine][lnk-devguide-query]
- [Asjade jaoturi MQTT tugi][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md