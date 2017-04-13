<properties
 pageTitle="Arendaja juhend - seadme kaksikud aru | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - kasutada seadme kaksikud asjade jaoturi ja seadmetes riigi- ja konfiguratsiooni andmete sünkroonimiseks"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Aru saada seadme kaksikud - eelvaade

## <a name="overview"></a>Ülevaade

*Seadme kaksikud* on JSON dokumendid, mis talletada seadme olek teavet (metaandmete konfiguratsioone ja tingimuste). Asjade jaoturi püsib seadme Double iga seadme, millega loote ühenduse asjade jaoturi. Selles artiklis kirjeldatakse:

* seadme Double struktuuri: *Sildid*, *soovitud* ja *teatatud atribuudid*, ja
* toimingud, mida seadme rakenduste ja tagasi lõpetatakse saab teha seadme kaksikud.

> [AZURE.NOTE] Sel ajal, seadme kaksikud pääseb juurde ainult asjade jaoturi MQTT protokolli abil ühendatud seadmete. Palun vaadake [MQTT toetavad] [ lnk-devguide-mqtt] artiklist leiate juhised, kuidas muuta olemasolevaid seadme rakenduse kasutamiseks MQTT.

### <a name="when-to-use"></a>Millal kasutada

Kasutage seadme twins.

* Talletage seadme teatud metaandmete pilves, nt juurutamise asukoha lisamine kohapeal.
* Aruande praeguse oleku teabe nagu saadaolevate võimaluste ja tingimuste seadme rakendus, nt seadme kaudu mobiilside või Wi-Fi-ühenduse kaudu.
* Saate sünkroonida olek pikaajalisi töövoogude vahel seadme rakendus ja tagasi lõpetada, nt tagasi lõpuks soovite määrata uue püsivara versiooni installimine ja seadme rakenduse aruandlus värskenduse toimingu eri etapid.
* Päringu oma seadme metaandmete, konfigureerimine või riik.

Kasutage [seadme pilve sõnumite] [ lnk-d2c] jaoks ajatempliga sündmusi, näiteks seeriate andurite andmeid või alarmid järjestus. Kasutada [cloud-seadme võimalusi] [ lnk-methods] jaoks interaktiivset juhtelementi seadmeid, näiteks fänn sisselülitamine.

## <a name="device-twins"></a>Seadme kaksikud

Seadme kaksikud talletada seadmega seotud teavet mis:

- Seadme ja tagasi lõpetatakse saate kasutada seadme tingimused ja konfigureerimise sünkroonima.
- Tagasi lõpuks rakenduse abil saate päringu- ja sihtsaitide pikaajalisi toiminguid.

Seadme Double elutsükli lingitud vastav [seade identiteedi][lnk-identity]. Kaksikud on peidetult loodud ja kustutatud uue seadme identiteedi loomisel või kustutatud asjade keskuses.

Seadme Double on JSON dokument, mis sisaldab:

* **Sildid**. JSON dokumenti lugeda ja kirjutada tagasi lõpuks. Sildid ei ole nähtaval seadme rakendused.
* **Soovitud atribuudid**. Seadme konfiguratsiooni või tingimus sünkroonimiseks kasutada koos teatatud atribuudid. Soovitud atribuudid saab seada ainult rakendus uuesti lõpp ja seadme rakenduse saab lugeda. Seadme rakendus samuti teavitatakse reaalajas muudatuste soovitud atribuudid.
* **Teatas atribuudid**. Seadme konfiguratsiooni või tingimus sünkroonimiseks kasutada koos soovitud atribuudid. Teatatud atribuudid saate seada ainult seadme rakendus ja saate lugeda ja, käitades rakenduses tagasi lõpuks.

Lisaks sisaldab seadme Double juurkaustas kirjutuskaitstud atribuutide kaudu vastava identiteedi, mis on toodud [seadme identiteedi registri][lnk-identity].

![][img-twin]

Siin on näide seadme Double JSON dokumendi:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

Juurkausta objekti, on süsteemi atribuudid ja container objektide jaoks `tags` ja nii `reported` ja `desired properties`. Selle `properties` container sisaldab mõningaid kirjutuskaitstud elemente (`$metadata`, `$etag`, ja `$version`) mis on kirjeldatud vastavalt [Double metaandmete] [ lnk-twin-metadata] ja [loodetav kokkulangevus] [ lnk-concurrency] jaotised.

### <a name="reported-property-example"></a>Teatatud atribuudi näide

Ülaltoodud näites sisaldab seadme Double on `batteryLevel` atribuut, mis on teatatud seadme rakendus. Atribuut võimaldab päringu ja seadmetes viimase teatatud aku vastavalt tegutseda. Teine näide oleks seadme rakenduse aruande seadme võimalustest või ühenduvuse suvandid.

Pange tähele, kuidas teatatud atribuudid lihtsustada stsenaariumid, kus tagasi lõpuks huvitatud atribuut teadaolevad viimase väärtuse. Kasutage [seadme pilve sõnumite] [ lnk-d2c] kui tagasi lõpuks vajab töödelda seadme telemeetria järjestus ajatempliga sündmusi, nt kellaaja sarja kujul.

### <a name="desired-configuration-example"></a>Soovitud konfiguratsioon näide

Ülaltoodud näites kuvatakse `telemetryConfig` atribuudid soovitud ja teatatud kasutavad tagasi lõpp ja seadme rakenduse telemeetria konfiguratsiooni selle seadme sünkroonima. Näiteks:

1. Rakenduse tagasi lõpuks määrab soovitud atribuuti soovitud väärtus. Siin on soovitud atribuuti dokumendi osa.

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Seadme rakendus teatatakse muudatus kohe, kui ühendus või esimese taastada. Seadme rakendus siis aruannete värskendatud konfiguratsiooni (või tõrge tingimus abil soovitud `status` atribuudi). Siin on osa teatatud atribuudid.

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. Rakenduse tagasi lõpuks jätta konfiguratsiooni toiming tulemuste jälgimine paljudes seadmetes, [päringute] [ lnk-query] kaksikud.

> [AZURE.NOTE] Ülaltoodud Koodilõigud näited, optimeeritud loetavuse võimalikul viisil kodeerida seadme konfiguratsiooni ja selle olek. Asjade jaoturi ei pane teatud skeemi seadme kaksikud teatatud ja soovitud atribuudid.

Paljudel juhtudel kasutatakse kaksikud sünkroonimiseks toiminguid nagu püsivara värskendused. Viidata [kasutada soovitud atribuudid konfigureerida seadmete] [ lnk-twin-properties] atribuutide sünkroonimiseks ja jälgida pikk kasutamise kohta lisateabe saamiseks töötab toiminguid kõigis seadmetes.

## <a name="back-end-operations"></a>Tagaandmebaas toimingud

Tagasi end toimib Double, kasutades atomic järgmisi toiminguid, HTTP kaudu:

1. Saate **tuua Double ID**. See toiming tagastab selle Double dokumenti, sh sildid ja soovi korral sisu teatatud ja Süsteemiatribuudid.
2. **Osaliselt värskendada Double**. See toiming võimaldab tagasi lõpuks osaliselt värskendada selle Double sildid või soovitud atribuudid. JSON dokumendi, mis lisab või värskendab mainitud vara esitatakse osaliselt värskendada. Atribuutide määramine `null` eemaldatakse. Näiteks järgmine loob uue soovitud atribuudi väärtust `{"newProperty": "newValue"}`, kirjutab üle olemasolevad väärtus `existingProperty` koos `"otherNewValue"`, ja eemaldab täielikult `otherOldProperty`. Muudatusi tehakse muid olemasolevaid soovitud atribuudid või Sildid:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Asendage soovitud atribuudid**. See toiming võimaldab tagasi lõpuks täielikult kirjutate üle kõik olemasolevad soovitud atribuudid ja asendada uute JSON dokumendile `properties/desired`.
4. **Asendage sildid**. Analoogselt soovitud atribuudid asendamiseks selle toimingute võimaldab tagasi lõpuks täielikult Kirjuta olemasolevad sildid ja asendada uute JSON dokumendile `tags`.

Kõik toimingud toetavad [loodetav kokkulangevus] [ lnk-concurrency] ja vaja **ServiceConnect** luba, nagu on määratletud [Turvalisus] [ lnk-security] artikkel.

Lisaks need toimingud, saate tagasi lõpuks päringu abil SQL-like [päringu keel]kaksikud[lnk-query], ja toiminguid suurte andmekomplektide kaksikud kasutades [tööd][lnk-jobs].

## <a name="device-operations"></a>Seadme toimingud

Seadme rakendus töötab kusjuures atomic järgmiste toimingute abil:

1. Saate **tuua Double**. See toiming tagastab selle Double dokumendi sisu (sh sildid ja soovitud, teatatud ja Süsteemiatribuudid) praegu ühendatud seade.
2. **Osaliselt update teatatud atribuudid**. See toiming võimaldab osaline värskenduse praegu ühendatud seadme teatatud atribuudid. See kasutab sama JSON-i värskendamine vormingut tagasi lõpuks vastastikuste osaliselt värskendada soovitud atribuudid.
3. **Järgima soovitud atribuudid**. Praegu ühendatud seadme saate valida soovitud atribuudid värskendused kohe, kui nad juhtub teavitama. Seadme saab samal kujul värskenduse (täielikult või osaliselt asendamine) sooritatud tagasi lõppu.

Kõik toimingud vaja **DeviceConnect** luba, nagu on määratletud [Turvalisus] [ lnk-security] artikkel.

[Azure'i asjade seadme SDK-d] [ lnk-sdks] oleks hõlpsam kasutada paljude keelte ja platvormide eespool nimetatud toiminguid. Lisateavet asjade jaoturi primitiivid soovitud atribuudid sünkroonimiseks üksikasjad leiate [seadme sisselülitamise meilivoo][lnk-reconnection].

> [AZURE.NOTE] Praegu seadme kaksikud pääseb juurde ainult asjade jaoturi MQTT protokolli abil ühendatud seadmete.

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile rohkem teavet oma asjade jaoturi juurdepääsu kontrollimine.

## <a name="tags-and-properties-format"></a>Sildid ja atribuudid Vorming

Sildid, soovitud ja teatatud atribuudid on JSON objektide järgmiste piirangutega.

* Kõigi kiirklahvide (Pääsuklahvide) JSON objektid on tõstutundlikud 128-char Unicode'i stringid. Lubatud märgid välistamine Unicode'i juhtmärgid (segmente C0 ja C1) ja `'.'`, `' '`, ja `'$'`.
* Kõigi väärtuste JSON objekt võib olla järgmist tüüpi JSON: kahendväärtus, arv, stringi, objekti. Massiivi pole lubatud.

## <a name="twin-size"></a>Standard suurus

Asjade jaoturi jõustab 8KB mahupiirang, väärtustel `tags`, `properties/desired`, ja `properties/reported`, välja arvatud kirjutuskaitstud elemendid.
Suurus on arvutatud lugedes kõik märgid, v.a Unicode'i juhtida märgid (segmente C0 ja C1) ja ruumi `' '` kuvamisel stringikonstant väljaspool.
Asjade jaoturi hülgab tõrkega kõik toimingud, mida soovite suurendada nende dokumentide üle.

## <a name="twin-metadata"></a>Double metaandmed

Asjade jaoturi säilitab ajatempli viimase värskenduse iga JSON objekti soovitud ja teatatud atribuudid. Funktsiooni ajatemplid on UTC ja kodeeritud [ISO8601] vormingus `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Näiteks:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

See teave säilitatakse säilitada eemaldada objekti klahvid värskendused igal tasemel (mitte ainult JSON struktuuri lehed).

## <a name="optimistic-concurrency"></a>Loodetav kokkulangevus

Sildid, soovitud teatatud atribuudid ja kõik toetavad loodetav kokkulangevus.
Sildid on ka etag ühe [RFC7232], mis tähistab sildi JSON esituse. Saate selle tagasi lõpuks tingimusvormingu värskenduse toimingu käigus järjepidevuse tagamiseks.

Soovitud ja teatatud atribuudid on etags, kuid on ka `$version` väärtus, mis on tagatud olla suureneva. Analoogselt on etag, et versiooni saab kasutada näiteks (seadmes rakendus teatatud atribuudi) või tagasi lõpuks soovitud atribuudi värskendamine poole jõustamiseks järjepidevuse värskenduste.

Versioonid on ka kasulik, kui järgides agent (nt seadme rakendus, järgides soovitud atribuudid) on sobitada võistlused too tehte tulem ja värskenduse teatis. Jaotises [seadme sisselülitamise meilivoo] [ lnk-reconnection] abil saate rohkem teavet.

## <a name="device-reconnection-flow"></a>Seadme sisselülitamise meilivoo

Asjade jaoturi ei säilitata soovitud atribuudid uuenduste teatiste ühendatud seadmete jaoks. See järgib seade, mis ühendab tuleb alla laadida dokumendi täielik soovitud atribuudid Lisaks tellimise uuenduste teatiste jaoks. Võimalus võistlused uuenduste teatiste ja täielik andmetoomisteenused, tuleb tagada voogu järgmised:

1. Seadme rakendus loob asjade jaoturiga.
2. Seadme rakendus toetab jaoks soovitud atribuudid uuenduste teatiste.
3. Seadme rakendus toob täielik dokumendi jaoks soovitud atribuudid.

Seadme rakendus saate ignoreerida kõik teatised, millel `$version` väiksem või võrdne kui täielik välja dokumendi versiooni. See on võimalik, kuna asjade jaoturi tagab versioonide alati inkrementida.

> [AZURE.NOTE] See loogika on juba rakendatud [Azure'i asjade seadme SDK-d][lnk-sdks]. See kirjeldus on kasulik ainult siis, kui seadme rakendus ei saa kasutada mis tahes Azure'i asjade seadme SDK-d ja peate programmi MQTT kasutajaliidese otse.

## <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud teil on kasutada, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud seadme kaksikud, teile võib huvi pakkuda ka arendaja juhend järgmisi teemasid:

- [Otsest meetodit seadmes][lnk-methods]
- [Mitmes seadmes tööde plaanimine][lnk-jobs]

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õppetükid

- [Kuidas kasutada seadme Double][lnk-twin-tutorial]
- [Kuidas kasutada Double atribuudid][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png