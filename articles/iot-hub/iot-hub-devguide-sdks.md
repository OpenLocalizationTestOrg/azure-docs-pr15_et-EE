<properties
 pageTitle="Arendaja juhend - asjade jaoturi SDK-d | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - ja lingid on erinevad Azure'i asjade jaoturi seade ja SDK-d kohta."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>Asjade jaoturi SDK-d

## <a name="iot-hub-device-sdks"></a>Asjade jaoturi seadme SDK-d

Microsoft Azure'i asjade seadme SDK-d sisaldavad koodi, mis hõlbustab building seadmed ja rakendused, mis ühenduse ja haldab Azure'i asjade jaoturi teenused.

Järgmised asjade seadme SDK-d on saadaval GitHub alla laadida.

- [Azure'i asjade seadme SDK C] [ lnk-c-device-sdk] kirjutatud teisaldamist ja platvormi ühilduvus ANSI C (C99).
- [Azure'i asjade seadme SDK .net-i jaoks][lnk-dotnet-device-sdk]
- [Azure'i asjade seadme SDK Java][lnk-java-device-sdk]
- [Azure'i asjade seadme SDK Node.js][lnk-node-device-sdk]
- [Microsoft Azure'i asjade seadme SDK Python 2,7][lnk-python-device-sdk]

> [AZURE.NOTE] Seletusfailid GitHub hoidlate kahendfaile ja sõltuvused arengu arvutisse installimiseks keele- ja platvormi kohased paketi haldurid kasutamise kohta leiate teemast.

## <a name="os-platforms-and-hardware-compatibility"></a>OS platvormide ja riistvara ühilduvus

Teatud riistvara seadmete SDK ühilduvuse kohta leiate lisateavet teemast [Azure sertifitseeritud asjade seadme kataloogi][lnk-certified].

## <a name="iot-hub-service-sdks"></a>Asjade jaoturi teenuse SDK-d

Microsoft Azure'i asjade teenuse SDK-d sisaldavad koodi, mis hõlbustab building rakendusi, mis nendega otse asjade keskuse kaudu hallata seadmed ja turvalisus.

Järgmised asjade teenuse SDK-d on saadaval GitHub alla laadida.

- [Azure'i asjade teenuse SDK .net-i jaoks][lnk-dotnet-service-sdk]
- [Azure'i asjade teenuse SDK Node.js][lnk-node-service-sdk]
- [Azure'i asjade teenuse SDK Java][lnk-java-service-sdk]

> [AZURE.NOTE] Seletusfailid GitHub hoidlate kahendfaile ja sõltuvused arengu arvutisse installimiseks keele- ja platvormi kohased paketi haldurid kasutamise kohta leiate teemast.

## <a name="azure-iot-gateway-sdk"></a>Azure'i asjade lüüsi SDK

See Azure asjade lüüsi SDK sisaldab taristu ja moodulid asjade lüüsi lahenduste loomiseks. Saate luua kohandatud mis tahes lõpuni stsenaarium lüüside SDK laiendada.

Saate alla laadida [Azure'i asjade lüüsi SDK] [ lnk-gateway-sdk] GitHub kaudu.

## <a name="online-api-reference-documentation"></a>Online API dokumentides

Järgmises loendis online viide dokumentatsiooni Azure'i asjade seadme, teenuse ja lüüsi teegid linke:

- [Internet asjade .net-i.][lnk-dotnet-ref]
- [Asjade jaoturi ülejäänud][lnk-rest-ref]
- [Azure'i asjade seadme SDK C][lnk-c-ref]
- [Azure'i asjade seadme SDK Java][lnk-java-ref]
- [Azure'i asjade teenuse SDK Java][lnk-java-service-ref]
- [Azure'i asjade seadme SDK Node.js][lnk-node-ref]
- [Azure'i asjade teenuse SDK Node.js][lnk-node-service-ref]
- [Azure'i asjade lüüsi SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Järgmised sammud

Muud viited Teemad tutvustava asjade jaoturi arendaja on järgmised:

- [Asjade jaoturi lõpp-punktid][lnk-devguide-endpoints]
- [Päringu keel kaksikud, võimalused ja tööde haldamine][lnk-devguide-query]
- [Kui ka ahendamine][lnk-devguide-quotas]
- [Asjade jaoturi MQTT tugi][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md