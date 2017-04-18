> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#:](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Sissejuhatus

Seadme Kaksikud on JSON dokumendid, kus talletatakse seadme olekuteabe (metaandmete konfiguratsioone ja tingimuste). Asjade jaoturi püsib seadme Double iga seadme, millega loote ühenduse asjade jaoturi.

Kasutage seadme twins.

* Talletage seadme metaandmete oma tagasi lõppu.
* Teie seadme rakenduse aruande praeguse oleku teabe nt saadaolevate võimaluste ja tingimuste (nt ühenduvuse meetod).
* Seadme rakendus ja tagasi end pikaajalisi töövoogude (nt püsivara ja konfiguratsiooni värskendused) olek sünkroonida.
* Päringu oma seadme metaandmete, konfigureerimine või riik.

> [AZURE.NOTE] Seadme Kaksikud on mõeldud sünkroonimise ja päringute seadme konfiguratsioone ja tingimused. Kasutage [seadme pilve sõnumite] [ lnk-d2c] järjestus ajatempliga sündmuste (nt telemeetria voole ajapõhiste andurite andmeid) ja [pilveteenuse-seadme meetodite] jaoks[ lnk-methods] jaoks interaktiivset juhtelementi seadmeid, näiteks sisselülitamine fänn kasutaja kontrollitud rakenduse kaudu.

Seadme kaksikud talletatakse soovitud asjade keskuses ja sisaldavad.

* *Sildid*, seadme metaandmete pääsete juurde ainult tagasi lõpuks;
* *soovitud atribuudid*, JSON objektide muudetavate tagasi end ja jälgitav seadmes rakendus; ja
* *teatatud atribuudid*, JSON objektide muudetavate seadme rakenduste ja loetav tagasi lõpetamiseks. Sildid ja atribuudid ei tohi sisaldada massiive, kuid objektide saate pesastada.

![][img-twin]

Lisaks saate rakenduse tagasi lõpuks päringu seadme kaksikud ülaltoodud andmete põhjal.
Viidata [mõistmine seadme kaksikud] [ lnk-twins] rohkem teavet seadme kaksikud ja [asjade jaoturi päringukeele] [ lnk-query] päringute viide.

> [AZURE.NOTE] Sel ajal, seadme kaksikud pääseb juurde ainult asjade jaoturi MQTT protokolli abil ühendatud seadmete. Palun vaadake [MQTT toetavad] [ lnk-devguide-mqtt] artiklist leiate juhised, kuidas teisendada olemasoleva seadme rakenduse kasutamiseks MQTT.

Selles õppetükis saate teada, kuidas soovite:

- Tagaandmebaas rakenduse, mis lisab seadme Double ja jäljendatud seade, mida selle kanali ühenduvuse aruanded nimega *teatatud atribuudi* seadme Double *siltide* loomine.
- Päringu seadmete rakenduste tagasi end sildid ja atribuudid, mis on varem loodud filtrite abil.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md