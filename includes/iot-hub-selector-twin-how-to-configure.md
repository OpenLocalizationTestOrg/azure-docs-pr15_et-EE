> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Sissejuhatus

Aastal [alustada asjade Interneti jaotur kaksikud][lnk-twin-tutorial], sa õppinud, kuidas seada seadme metaandmete teie lahendus kolp, *Sildid*, aruande seadme tingimustes *kirjeldatud omadused*, kasutades seadme rakendus ja päringu teabe, kasutades SQL-like keeles.

Sel juhendaja õpid, kuidas kasutada ning koos *teatatud majutusasutust*, eemalt seadistada seadme rakendused on twin *soovitud omadused* . Täpsemalt See õpetus näitab kuidas twin on teatatud ja soovitud omadused lubada multi-Step konfiguratsiooni seade rakenduse seadete ja pakkuda lahendus selle toimingu oleku tagasi lõpuks nähtavus kõikides seadmetes.

Kõrgel tasemel, järgib õpetamisel seadmehalduse *soovitav seisund muster* . Põhiidee, see mudel on, et lahendus kolp, määrake soovitud hallatavate seadmete asemel seotud käsud. See lülitab seadme eest kindlaks parim viis jõuda soovitud riik (väga oluline IoT juhul kui kindla seadme mõjutada võimet viivitamata täita seotud käsud), samal ajal pidevalt aruandluse kolp, praegust olukorda ja võimalike tõrke tingimustega. Soovitav seisund muster on instrumentaalne seadmed, suurte andmehulkade haldamiseks, kuna see võimaldab kolp on täielik nähtavust konfigureerimisprotsessi riigi kõigis seadmetes.
Muster soovitud riigi rolli lisateabe leiate seadme haldamine [Ülevaade Azure IoT Hub seadme haldamine][lnk-dm-overview].

> [AZURE.NOTE] Juhul kui seadmeid kontrollitakse rohkem interaktiivseid mood (Lülita fänn rakenduse kasutaja reguleeritav), võite kasutada [pilve seadme meetodid][lnk-methods].

Sel juhendaja, taotluse tagasi lõpuks muutused telemeetria configuration target seadme ja tekkimine, seadme rakendus tuleneb mitmeetapilist protsessi rakendamiseks konfiguratsiooni värskendamise (nt nõudma tarkvara moodul uuesti), mille õpetamisel simuleerib lihtne viivitusega).

Back-end salvestab konfiguratsiooni seade twin soovitud omadused järgmiselt:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Kuna koosseisud võib olla keeruline objektid, tavaliselt määratakse neile kordumatud koodid (hashes või [GUID'e][lnk-guid]) lihtsustada nende võrdlused.

Seadme rakendus teatab oma praeguse konfiguratsiooni peegeldamine soovitud vara **telemetryConfig** teatatud omadused:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Pange tähele, kuidas teatatud **telemetryConfig** täiendav vara on **olek**, saab aru riigi osa konfiguratsiooni.

Uue soovitud konfiguratsiooni saabumisel esitab seadme rakendus ootel konfiguratsiooni muutes teabe:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Siis mingil hilisemal ajal seadme rakendus aruande häired selle töös edu ajakohastatakse eespool nimetatud vara.
Pange tähele, kuidas kolp on võimalik igal ajal konfigureerimisprotsessi oleku päring kõikides seadmetes.

Selles juhendis näidatakse, kuidas et:

- Luua simuleeritud seade, mis saab konfiguratsiooni värskendusi kolp ja lahendid mitme värskenduse *teatatud* atribuutidena konfiguratsiooni uuendamise protsessile.
- Luua lõppfaasi app, mis uuendab seade soovitud konfiguratsiooni ja seejärel küsib konfiguratsiooni uuendamise protsessile.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier