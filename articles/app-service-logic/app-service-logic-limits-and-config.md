<properties
    pageTitle="Loogika rakenduse piirangud ja konfiguratsiooni | Microsoft Azure'i"
    description="Ülevaade teenuste piirangud ja väärtused loogika rakenduste jaoks saadaval."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Loogika rakenduse piirangud ja konfigureerimine

Allpool leiate Azure'i loogika rakenduste teavet praeguse piirangud ja konfiguratsiooni üksikasjad.

## <a name="limits"></a>Piirangud

### <a name="http-request-limits"></a>HTTP taotluse piirangud

Need piirangud ühe HTTP taotlus ja/või Connectori kõne

#### <a name="timeout"></a>Ajalõpp

|Nimi|Limiit|Märkmete|
|----|----|----|
|Päringu ajalõpp|1 minuti|Mõne [asünkroonse mustri](app-service-logic-create-api-app.md) või [tsükkel kuni](app-service-logic-loops-and-scopes.md) saate kompenseeri vastavalt vajadusele|

#### <a name="message-size"></a>Sõnumi maht

|Nimi|Limiit|Märkmete|
|----|----|----|
|Sõnumi maht|50 MB|Mõned ühendused ja API-d ei pruugi 50MB.  Taotluse päästik toetab kuni 25MB|
|Avaldise hindamisel limiit|131,072 märke|`@concat()`, `@base64()`, `string` ei saa olla pikem kui see|

#### <a name="retry-policy"></a>Proovige poliitika

|Nimi|Limiit|Märkmete|
|----|----|----|
|Uuestiproovimise katsest|4|Saate konfigureerida, [proovige poliitika parameeter](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Proovige max viivitus|1 tund|Saate konfigureerida, [proovige poliitika parameeter](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Proovige min viivitus|20 min|Saate konfigureerida, [proovige poliitika parameeter](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Käivita kestus ja säilitus.

Need piirangud ühe loogika rakenduse käivitamine.

|Nimi|Limiit|Märkmete|
|----|----|----|
|Käivitage kestus|90 päeva||
|Salvestusruumi säilitus.|90 päeva|See on Käivita aeg|
|Min Korduvus intervall|15 sec||
|Max Korduvus intervall|500 päeva||


### <a name="looping-and-debatching-limits"></a>Hoidke ja debatching piirangud

Need piirangud ühe loogika rakenduse käivitamine.

|Nimi|Limiit|Märkmete|
|----|----|----|
|ForEach üksused|5000|[Päringu toimingu](../connectors/connectors-native-query.md) abil saate filtreerida suuremat massiivi vastavalt vajadusele|
|Kuni iteratsiooni|10 000||
|SplitOn üksused|5000||
|ForEach paralleelsus|20|Saate seada järjestikku foreach, lisades `"operationOptions": "Sequential"` abil soovitud `foreach` toiming|


### <a name="throughput-limits"></a>Läbilaskevõime piirangud

Need piirangud ühe loogika rakenduse eksemplari. 

|Nimi|Limiit|Märkmete|
|----|----|----|
|Päästikute sekundis|100|Saate levitada töövoogude mitme rakendustes vastavalt vajadusele|

### <a name="definition-limits"></a>Määratlus piirangud

Need on ühe loogika rakenduse määratluse piirangud.

|Nimi|Limiit|Märkmete|
|----|----|----|
|ForEach toimingud|1|Saate lisada pesastatud töövoogude laiendada seda vastavalt vajadusele|
|Toiminguid töövoo kohta.|60|Saate lisada pesastatud töövoogude laiendada seda vastavalt vajadusele|
|Toimingu pesastamise sügavus lubatud|5|Saate lisada pesastatud töövoogude laiendada seda vastavalt vajadusele|
|Puhul piirkonna tellimuse kohta|1000||
|Päästikute töövoo kohta.|10||
|Max märkide avaldis|8192||
|Max `trackedProperties` tähtedega suurus|16000|
|`action`/`trigger`nime piirang|80||
|`description`pikkus limiit|256||
|`parameters`limiit|50||
|`outputs`limiit|10||

## <a name="configuration"></a>Konfigureerimine

### <a name="ip-address"></a>IP-aadress

Kõnede kaudu [konnektor](../connectors/apis-list.md) on pärit allpool määratud IP-aadress.

Loogika rakendusest otse (st kaudu [HTTP](../connectors/connectors-native-http.md) või [HTTP + ärplema](../connectors/connectors-native-http-swagger.md)) kõnede võib sisaldada mis tahes [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Loogika rakenduse piirkond|Väljaminevate IP|
|-----|----|
|Austraalia Ida|40.126.251.213|
|Austraalia kodutee|40.127.80.34|
|Brasiilia Lõuna|191.232.38.129|
|Keskse India|104.211.98.164|
|Kesk-USA|40.122.49.51|
|Ida-Aasia|23.99.116.181|
|Ida-USA|191.237.41.52|
|Ida-USA 2|104.208.233.100|
|Jaapan Ida|40.115.186.96|
|Jaapan Lääne|40.74.130.77|
|Põhja Kesk-USA|65.52.218.230|
|Põhja-Euroopa|104.45.93.9|
|Lõuna-, Kesk-USA|104.214.70.191|
|Kagu-Aasia|13.76.231.68|
|Lõuna India|104.211.227.225|
|Lääne Euroopa|40.115.50.13|
|Lääne India|104.211.161.203|
|Lääne USA.|104.40.51.248|


## <a name="next-steps"></a>Järgmised sammud  

- Loogika Appsi kasutamise alustamine, järgige õpetuse [loogika rakenduse loomine](app-service-logic-create-a-logic-app.md) .  
- [Vaate levinud näited ja stsenaariumid](app-service-logic-examples-and-scenarios.md)
- [Te saate loogika rakenduste abil äriprotsesside automatiseerimine](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Saate teada, kuidas teie integreerida loogika rakendused](http://channel9.msdn.com/Events/Build/2016/P462)