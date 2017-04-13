<properties
 pageTitle="Azure'i asjade Interneti lahendused | Microsoft Azure'i"
 description="Ülevaade asjade Azure, sh valimi lahenduse arhitektuuri ja kuidas see on seotud Azure'i asjade jaoturi, seadme SDK-d ja eelkonfigureeritud lahendused"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Järgmised sammud

Azure'i asjade jaoturi on Azure'i teenus, mis võimaldab turvaline ja usaldusväärne kahesuunaline suhtlus vahel rakenduse tagasi lõpp ja miljoneid seadmed. See võimaldab rakenduse tagasi end, et:

- Telemeetria tasandil saadud seadmetes.
- Marsruutimiseks andmed teie seadmest voo sündmuse protsessor.
- Saate faili üleslaadimiseks seadmed.
- Kindlate seadmete pilve-seadme käsud saata.

Asjade jaoturi abil saate rakendada oma lahenduse tagasi end. Lisaks sisaldab asjade jaoturi seadme identiteedi registri ettevalmistamise seadmed, volituste turvalisus ja nende õiguste jaoturi ühenduse loomiseks kasutada. Asjade keskuse kohta leiate lisateavet teemast [mis on asjade jaoturi?] [lnk-iot-hub].

Siit saate teada, kuidas Azure'i asjade jaoturi võimaldab standardid vastavalt asjade mobiilsideseadmete halduse eemalt hallata, konfigureerida ja värskendada oma seadmete jaoks, leiate [Azure'i ülevaade asjade jaoturi mobiilsideseadmete halduse][lnk-device-management].

Klientrakendustes rakendamiseks mitmesuguseid seadme riistvara platvormid ja opsüsteemides, saate kasutada seadme asjade SDK-d. Asjade seadme SDK-d kaasata teegid, mis hõlbustavad saatmise telemeetria soovitud asjade jaoturi ja vastuvõtt cloud-seadme käsud. Kui kasutate funktsiooni SDK-d, saate valida mitme võrguprotokollide asjade jaoturi suhelda. Lisateavet leiate teemast [teavet seadme SDK-d][lnk-device-sdks].

Alustamiseks mõned koodi kirjutamine ja töötavad mõned näidised, lugege teemat [Alustamine asjade jaoturi] [ lnk-getstarted] õpetuse.

Teil võib olla huvitatud [Azure'i asjade komplekti][lnk-iot-suite], mis on eelkonfigureeritud lahenduste kogum. Asjade komplekti võimaldab teil töö Kiire alustamine ja mastaapimiseks asjade projektide asjade tavastsenaariumid – nt jälgida, varade haldamine ja sõnastikupõhise hooldustööd.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md