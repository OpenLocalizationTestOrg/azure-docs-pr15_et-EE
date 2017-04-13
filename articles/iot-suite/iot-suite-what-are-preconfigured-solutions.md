<properties
 pageTitle="Azure'i asjade eelkonfigureeritud lahendused | Microsoft Azure'i"
 description="Azure'i asjade kirjeldus eelkonfigureeritud lahenduste ja nende arhitektuur lingid lisaressurssidele."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Mis on eelkonfigureeritud Azure'i asjade komplekti lahendusi?

Azure'i asjade komplekti eelkonfigureeritud lahendused on levinud asjade lahenduse mustrite Azure tellimuse abil saate juurutada rakendusi. Saate kasutada eelkonfigureeritud lahendusi.

- Alustuseks oma asjade lahendusi.
- Lisateavet levinud mustrid asjade lahenduse kujundus ja areng.

Iga eelkonfigureeritud lahendus on lõpule jõudnud, lõpuni rakendamist, luua telemeetria jäljendatud seadmete kasutava.

Lisaks juurutamine ja Azure lahendused töötab, saate alla laadida täieliku lähtekoodi ja seejärel kohandamine ja laiendamine lahendus teie teatud asjade nõuetele.

> [AZURE.NOTE] Juurutage üks eelkonfigureeritud lahendusi, külastage [Microsoft Azure'i asjade komplekti][lnk-azureiotsuite]. Artikli [Alustamine asjade eelkonfigureeritud lahenduste] [ lnk-getstarted-preconfigured] abil saate rohkem teavet selle kohta, kuidas juurutada ja käivitada üks lahendusi.

Järgmine tabel näitab, kuidas nende lahenduste vastendamine asjade funktsioonid:

| Lahendus | Andmete manustamisest | Seadme identiteedi | Juhtimis- ja | Reeglid ja toimingud | Ennustav |
|------------------------|-----|-----|-----|-----|-----|
| [Remote jälgimine][lnk-getstarted-preconfigured] | Jah | Jah | Jah | Jah | -   |
| [Sõnastikupõhise hooldustööd][lnk-predictive-maintenance] | Jah | Jah | Jah | Jah | Jah |

- *Andmete manustamisest*: sissepääsu andmete skaala pilveteenusesse.
- *Seadme identiteedi*: kordumatu identiteedid iga ühendatud seadme haldamine.
- *Juhtimis- ja*: pilvest põhjustada mingi toimingu seade seadme sõnumeid saata.
- *Reeglid ja toimingud*: lahenduse tagasi lõpuks kasutab reeglid toimivad teatud seadme pilveteenuses olevaid andmeid.
- *Ennustav*: lahenduse tagasi lõpuks kehtib analüüsib prognoosida, kui teatud toiminguid peaks toimuma seadme pilveteenuses olevaid andmeid. Näiteks analüüsimine õhusõiduki mootori telemeetria kui mootori hooldus on tähtaja määramiseks.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Järelevalve Remote'i eelkonfigureeritud lahenduse ülevaade

Valisime remote jälgimise eelkonfigureeritud lahendus selle artikli arutada, sest see näitab mitme levinud kujunduselemente muid lahendusi, ühiskasutusse anda.

Järgmine diagramm näitab klahv elementide remote jälgimise lahendus. Järgmistes jaotistes pakuvad lisateavet järgmisi elemente.

![Järelevalve Remote'i eelkonfigureeritud lahenduse arhitektuur][img-remote-monitoring-arch]

## <a name="devices"></a>Seadmetes

Kui juurutate järelevalve Remote'i eelkonfigureeritud lahendus, neli jäljendatud seadmed on eelnevalt ettevalmistatud lahenduse, mis on jahutusseadise simuleerida. Need jäljendatud seadmed on sisseehitatud temperatuuri- ja mudel, mis eraldab telemeetria. Need jäljendatud seadmed on kaasatud illustreerimiseks lõpuni voo kaudu lahendus ja ette käsud on mugav allikas telemeetria ja eesmärki, kui olete tagaandmebaas arendaja abil lahendus lähtepunktina kohandatud rakendamiseks.

Kui seadme esmalt ühendub asjade jaoturi järelevalve Remote'i eelkonfigureeritud lahendus, loetletakse seadme teabe sõnumi asjade jaoturi käsud, mis seadme saavad vastata. Järelevalve Remote'i eelkonfigureeritud lahendus on käsud: 

- *Ping seade*: seade ei vasta selle käsu abil kinnitus. See on kasulik kontrollimiseks, et seade on endiselt aktiivne ja kuulata.
- *Käivitage telemeetria*: juhendab seadme telemeetria saatmine.
- *Lõpeta telemeetria*: juhendab seadme telemeetria saatmise lõpetada.
- *Muuda punkt temperatuur*: määrab seade saadab jäljendatud temperatuur telemeetria väärtused. See on kasulik tagaandmebaas loogika kontrollida.
- *Diagnostika telemeetria*: määrab kui seade peaks telemeetria välise temperatuuri saatmine.
- *Muuda seadme olek*.: seab seadme olek metaandmete atribuudi seadme aruanded. See on kasulik tagaandmebaas loogika kontrollida.

Lahenduse, mis eraldavad sama telemeetria ja sama käske saate lisada rohkem jäljendatud seadmed. 

## <a name="iot-hub"></a>Asjade jaoturi

See eelkonfigureeritud lahendus asjade jaoturi eksemplari vastab *Cloud lüüsi* tüüpilised [asjade lahenduse arhitektuuri][lnk-what-is-azure-iot].

Soovitud asjade jaoturi saab telemeetria seadmete juures ühe lõpp-punkti. Soovitud asjade jaoturi väidab seadme teatud lõpp-punktid kui iga seadmed saavad tuua käsud, mis on saadetud.

Asjade jaoturi teeb saadud telemeetria teenuse-side telemeetria, lugege lõpp-punkti kaudu.

## <a name="azure-stream-analytics"></a>Azure'i voo Analytics

Eelkonfigureeritud lahendus kasutab kolme [Azure'i voo Analytics] [ lnk-asa] (Ash) töö, et filtreerida telemeetria voo seadmete kaudu:


- *DeviceInfo töö* - väljundid andmete sündmuse jaoturi, mis marsruudib seadme registreerimine teatud, saadetud sõnumid kui seadme esmalt loob või käsu **Muuda seadme olek** , lahenduse seadme registri (DocumentDB andmebaas). 
- *Telemeetria töö* - saadab kõik töötlemata telemeetria Azure'i bloobimälu jaoks külmladustamine ja arvutab telemeetria liitmisi, mis lahenduse armatuurlaua kuvamine.
- *Reeglite töö* - filtreerib telemeetria voo ületada mis tahes reegli lävede ja väljundi andmed sündmuse jaoturiga väärtusi. Kui reegel käivitub, lahenduse portaali armatuurlaua vaade kuvatakse see sündmus uue rea alarm nurjumiskirjet ja käivitab toimingu, mis põhinevad reeglid ja toimingud vaadetes lahenduse portaalis määratletud sätted.

See eelkonfigureeritud lahendus ASA töö moodustavad osa **asjade lahenduse tagasi end** tüüpilised [asjade lahenduse arhitektuuri][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Sündmuse protsessor

See eelkonfigureeritud lahendus sündmuse protsessor moodustab osa **asjade lahenduse tagasi end** tüüpilised [asjade lahenduse arhitektuuri][lnk-what-is-azure-iot].

**DeviceInfo** ja **reeglid** ASA töö saata oma sündmuse jaoturi kohaletoimetamiseks tagasi end muude teenustega. Lahendus kasutab mõnda [EventPocessorHost] [ lnk-event-processor] eksemplari, töötavad on [WebJob][lnk-web-job], et need sündmuse jaoturi kaudu sõnumeid lugeda. **EventProcessorHost** kasutab **DeviceInfo** andmete andmebaasis DocumentDB seadme andmete värskendamiseks, ja kasutab **reeglid** andmete autonoomsest loogika rakendus ja teatisi kuvada lahenduse portaalis värskendus.

## <a name="device-identity-registry-and-documentdb"></a>Seadme identiteedi registri ja DocumentDB

Iga asjade jaoturi sisaldab [seadme identiteedi registri] [ lnk-identity-registry] , mis talletab Seadme klahvid. Asjade jaoturi kasutab seda teavet autentida seadmed – seade peab olema registreeritud ja on kehtiv võti enne, kui see saab ühenduda jaoturi.

See lahendus talletab seadmed, nt nende, nad toetavad käskude ja muude metaandmete Lisateave. Lahendus kasutab DocumentDB andmebaasi lahenduse konkreetse seadme andmete talletamiseks ja lahenduse portaali laadib andmed selle DocumentDB andmebaasist kuvamine ja redigeerimine.

Lahendus säilitab teabe seadme identiteedi registris sünkroonitud DocumentDB andmebaasi sisu. **EventProcessorHost** kasutab tööst **DeviceInfo** voo analytics andmete sünkroonimine haldamiseks.

## <a name="solution-portal"></a>Lahendus portaal

![Lahendus armatuurlaud][img-dashboard]

Lahendus portaal on veebipõhine kasutajaliides, eelkonfigureeritud lahenduse osana pilveteenusesse juurutatud. See võimaldab teil.

- Armatuurlaua telemeetria ja helisignaal ajaloo kuvamine.
- Ettevalmistamise uute seadmed.
- Hallata ja jälgida seadmed.
- Kindlate seadmete käsud saata.
- Halda reegleid ja toiminguid.

See eelkonfigureeritud lahendus lahendus portaali vormid osa **asjade lahenduse tagasi end** ja **töötlemine ja business connectivity** tüüpilised [asjade lahenduse arhitektuuri][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Järgmised sammud

Asjade lahenduse arhitektuurides kohta leiate lisateavet teemast [Microsoft Azure'i asjade teenuste: viide arhitektuur][lnk-refarch].

Nüüd teate, mis on eelkonfigureeritud lahendus on, saate alustada, *jälgida* eelkonfigureeritud lahenduse juurutamine: [Alustamine eelkonfigureeritud lahenduste][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md