<properties
 pageTitle="Arendaja juhend - seadme identiteedi registri | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - kirjeldus seadme identiteedi register ja kuidas seda kasutada oma seadmete haldamine"
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

# <a name="manage-device-identities-in-iot-hub"></a>Seadme identiteedid asjade keskuses haldamine

## <a name="overview"></a>Ülevaade

Iga asjade jaoturi on seadme identiteedi registri, mis talletab teavet seadmetes, mis on lubatud ühenduse jaoturi. Seadme ühendust jaoturi, peate olema selle seadme jaoturi seadme identiteedi registris kirje. Seadme peab ka autentimiseks jaoturi seadme identiteedi registris talletatud mandaatide põhjal.

Seadme identiteedi register on kõrge, seadme identiteedi ressursid ülejäänud-ühenduse võimalusega kogum. Registri kirje lisamisel loob asjade jaoturi teenust nagu soovitud järjekorras, mis sisaldab parda cloud-seadme sõnumite kogumi seadme kohta ressursse.

### <a name="when-to-use"></a>Millal kasutada

Seadme identiteedi registrit kasutada, kui teil on vaja ettevalmistamise seadmetes, mis ühenduse asjade jaoturi ja kui teil on vaja seadme kohta juurdepääsu oma jaoturi seadme suunatud lõpp-punktid.

> [AZURE.NOTE] Seadme identiteedi registri ei sisalda rakenduse kohased metaandmed.

## <a name="device-identity-registry-operations"></a>Seadme identiteedi registri toimingud

Asjade jaoturi seadme identiteedi registri seab järgmisi toiminguid:

* Seadme identiteedi loomine
* Seadme identiteedi värskendamine
* Saate tuua seadme identiteedi ID
* Seadme identiteedi kustutamine
* Loetle kuni 1000 identiteedid
* Ekspordi kõik identiteedid Azure'i bloobimälu
* Azure'i bloobimälu identiteedid importimine

Kõik need toimingud loodetav kokkulangevus, saate kasutada määratud [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Ainus võimalus tuua kõik identiteedid on jaoturi identiteedi registris on kasutada [ekspordi] [ lnk-export] funktsioonid.

Asjade jaoturi seadme identiteedi register:

- Ei sisalda rakenduse metaandmed.
- Pääseb sõnastiku, nt võti **deviceId** abil.
- Ei toeta väljendusvõimalustega päringud.

Lahenduse asjade on tavaliselt eraldi lahenduse kohased hoida, mis sisaldab rakenduse kohased metaandmete. Näiteks salvestada nutikas building lahendus lahendus kohased pood, kus on juurutatud temperatuuriandurist jututuba.

> [AZURE.IMPORTANT] Kasutage ainult seadme identiteedi registri mobiilsideseadmete halduse ja ettevalmistamise toimingud. Kõrge läbilaskevõime toimingute käitusajal peaks sõltuvad registris seadme identiteedi toimingute tegemiseks. Näiteks seadme ühenduse oleku kontrolli enne saatmist käsk pole toetatud mustri. Kontrollige [pidurdamise määr] [ lnk-quotas] seadme identiteedi registri ja [seadme südamelöögi ajal] [ lnk-guidance-heartbeat] mustri.

## <a name="disable-devices"></a>Seadmete keelamine

Värskendamise **oleku** atribuut identiteedi registris, saate keelata seadmed. Tavaliselt kasutatakse selle atribuudi kahte järgmist stsenaariumi:

- Ajal korraldamise ebausaldusväärsete. Lisateavet leiate teemast [Seadme ettevalmistamise][lnk-guidance-provisioning].
- Kui mingil põhjusel teie arvates seade on kahjustatud või on muutunud volitamata.

## <a name="import-and-export-device-identities"></a>Seade importimise ja eksportimise identiteedid

Saate eksportida seadme identiteedid mitmekaupa asjade jaoturi identiteedi registrist [asjade jaoturi ressursi pakkuja lõpp-punkti]asünkroonsete toimingute abil[lnk-endpoints]. Ekspordi on pikaajaline töö, kasutavate klientide esitatud bloobimälu container lugeda registrist identiteedi seadme identiteedi andmed salvestada.

Saate importida seadme identiteedid mitmekaupa asjade jaoturi identiteedi registri abil asünkroonsete toimingute [asjade jaoturi ressursi pakkuja lõpp-punkti][lnk-endpoints]. Import on pikaajaline tööd, kasutage andmete kliendi esitatud bloobimälu ümbrises kirjutamiseks seadme identiteedi andmete seadme identiteedi registrisse.

- Importimise ja eksportimise API-de kohta üksikasjaliku teabe saamiseks vt [asjade jaoturi ressursi pakkuja REST API-de][lnk-resource-provider-apis].
- Lisateavet selle kohta, kuidas importida ja eksportida tööde haldamine, vt [hulgi juhtimine asjade jaoturi seadme identiteedid][lnk-bulk-identity].

## <a name="device-provisioning"></a>Seadme ettevalmistamine

Seadme andmed, mida antud asjade lahenduse talletab sõltub selle lahenduse erinõuetega. Kuid vähemalt lahenduse tuleb salvestada seadme identiteedid ja autentimise võtmed. Azure'i asjade jaoturi sisaldab identiteedi registri, mida saab salvestada iga seadme ID-d, võtmed autentimist ja olekukoodi nagu väärtused. Lahenduse saate kasutada muid Azure teenused, nt Azure'i tabelimälu, Azure'i bloobimälu või Azure'i DocumentDB mis tahes täiendavate seadme andmete talletamiseks.

*Seadme ettevalmistamise* on algse seadme andmete lisamine oma lahenduse poed. Uue ühenduse loomiseks oma keskuse seadme lubamiseks peate lisama uue seadme ID ja võtmed asjade jaoturi identiteedi register. Osana ebausaldusväärsete, peate lähtestamine seadme andmete muu lahendus poed.

## <a name="device-heartbeat"></a>Seadme südamelöögi ajal

Asjade jaoturi identiteedi register sisaldab **connectionState**välja. Saab kasutada ainult **connectionState** välja arendamise käigus ja silumine, asjade lahenduste peaks pole päringu väli käitusajal (näiteks, et kontrollida, kui seade on ühendatud, et otsustada, kas saata cloud-seadme sõnumi või SMS).

Kui teie asjade lahendus on vaja teada, kas seade on ühendatud (käitusajal, või kui atribuudi **connectionState** pakub täpsemalt), teie lahendus tuleks rakendada *mustreid südamelöögi ajal*.

Mustri südamelöögi ajal seade saadab seadme pilve sõnumite vähemalt ühe korra iga määratud aja jooksul, (nt vähemalt kord tunnis). See tähendab, et isegi juhul, kui seade ei saa andmeid saata, saadab endiselt seadme pilve tühja meilisõnumi (tavaliselt koos atribuut, mis määratletakse on südamelöögi ajal). Teenuse pool lahendus säilitab kaardi ja selle viimase südamelöögi ajal iga seadmega saadud ja eeldab, et on probleem seadme, kui seda ei saa südamelöögi ajal sõnumi oodatud aja jooksul.

Keerukamaid rakendamist võivad hõlmata teavet [toimingute jälgimine] [ lnk-devguide-opmon] proovib ühendust luua või suhelda, kuid ei ole seadmete tuvastamiseks. Kui rakendate südamelöögi ajal mustri, veenduge, et kontrollida [asjade jaoturi kvootide ja pidurdab][lnk-quotas].

> [AZURE.NOTE] Kui asjade lahenduse peab seadme ühenduse oleku üksnes, et määrata, kas pilve-seadme sõnumite saatmiseks ja sõnumid on ei leviedastada suurte andmekomplektide seadmed, lihtsam mustri, millega tuleks arvestada on kasutada lühike kehtivuse aeg. See saavutab sama tulemuse nagu säilitades seadme ühenduse oleku registri abil südamelöögi ajal mustri, olles oluliselt tõhusamaks. Samuti on võimalik, sõnumi kinnitused edastatav asjade jaoturi ja millised seadmed saavad sõnumeid, mis pole võrgus või on nurjunud, paludes.

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile devcie identiteedi registri kohta lisateavet.

## <a name="device-identity-properties"></a>Seadme identiteedi atribuudid

Seadme identiteedid on esindatud JSON dokumendid, millel on järgmised atribuudid.

| Atribuut | Suvandid | Kirjeldus |
| -------- | ------- | ----------- |
| deviceId | nõutav värskendustega kirjutuskaitstud | ASCII 7-bitise tähti ja numbreid tõstutundlikud string (kuni 128 tähemärki) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | nõutav kirjutuskaitstud | Jaoturi genereeritud, tõstutundlikud string kuni 128 tähemärki. Seda kasutatakse sama **deviceId**seadmete eristada, kui kustutatud ja uuesti luua. |
| etag | nõutav kirjutuskaitstud | String, mis tähistab nõrk etag seadme identiteedi, ühe [RFC7232][lnk-rfc7232].|
| Auth | valikuline. | On kombineeritud objekti, mis sisaldab autentimise teavet ja turve materjalid. |
| Auth.symkey | valikuline. | Kombineeritud objekti, mis sisaldab esmane ja teisene klahvi, salvestatud base64-vormingus. |
| olek | nõutav | Accessi näitaja. Võib olla **lubatud** või **keelatud**. Kui **lubatud**, seade on lubatud ühendada. Kui **keelatud**, see seade ei pääse juurde mis tahes seadme suunatud lõpp-punkti. |
| statusReason | valikuline. | 128 märgi pikkuse string, mis talletab selle seadme identiteedi oleku põhjus. Kõik UTF-8 märgid on lubatud. |
| statusUpdateTime | kirjutuskaitstud | Ajalise näidik, mis näitab kuupäeva ja kellaaja viimase värskenduse olek. |
| connectionState | kirjutuskaitstud | Ühenduse oleku, mis näitab, välja: kas **ühendatud** või **Ühendus katkestatud**. Väli tähistab asjade jaoturi vaate seadme olek. **Tähtis**: See väli tuleks kasutada ainult eesmärgil arengu/silumine. Ühenduse oleku värskendatakse ainult MQTT või AMQP kasutavates seadmetes. Lisaks see põhineb protokoll taseme ping (MQTT ping või AMQP ping) ja see võib olla kuni viivitus kestab 5 minutit. Nendel põhjustel võib olla vale-positiivsed nagu ühendatud seadmete teatatud, kuid mis tegelikult üksteisest lahutatud. |
| connectionStateUpdatedTime | kirjutuskaitstud | Ajalise näidik, mis näitab ühenduse oleku kuupäev ja kellaaeg viimati värskendatud. |
| lastActivityTime  | kirjutuskaitstud | Ajaline tähis, kuupäev ja kellaaeg viimati kuvatud seade ühendatud, saadud või saadetud sõnumile. |

> [AZURE.NOTE] Ühenduse oleku võib tähistada ainult asjade jaoturi vaate ühenduse olekut. Värskenduste selle seisundi seniks võrgu tingimustest ja konfiguratsioone.

## <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud mõni kasutamine, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil saate.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud, kuidas asjade jaoturi seadme identiteedi registrit kasutada, võib olla huvitatud arendaja juhend järgmisi teemasid:

- [Asjade jaoturi juurdepääsu reguleerimine][lnk-devguide-security]
- [Riigi ja konfiguratsioone sünkroonimine seadme kaksikud abil][lnk-devguide-device-twins]
- [Otsest meetodit seadmes][lnk-devguide-directmethods]
- [Mitmes seadmes tööde plaanimine][lnk-devguide-jobs]

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õpetus:

- [Azure'i asjade jaoturi kasutamise alustamine][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md