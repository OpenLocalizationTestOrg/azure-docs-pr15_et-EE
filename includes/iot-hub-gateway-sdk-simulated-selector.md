> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

See teid läbi [simuleeritud seadme pilve laadida proov] näitab, kuidas kasutada [Azure asjade Interneti Gateway SDK] [ lnk-sdk] saata seadme pilve telemeetria IoT keskusse simuleeritud seadmed.

See teid läbi hõlmab:

1. **Arhitektuur**: arhitektuuri oluline teave simuleeritud seadme pilve laadida proovi.

2. **Ehitada ja Käivita**: ehitada ja lastakse etapid.

## <a name="architecture"></a>Arhitektuur

Simuleeritud seadme pilve laadida proovi ülevaate SDK abil saate luua lüüsi, mis saadab telemeetria simuleeritud seadmed IoT jaoturiga. Simuleeritud seadmed ei saa ühendust otse IoT keskusse, kuna:

- Seadmetes kasutatakse Sideprotokoll IoT jaoturi kaudu.
- Seadmed ei ole piisavalt targad, et meeles pidada isiku poolt asjade Interneti jaotur.

Lüüsi lahendab need probleemid simuleeritud seadmete järgmiselt:

- Lüüsi mõistab protokolli kasutavad simuleeritud seadmed saab seadme pilve telemeetria seadmed ja edastab need sõnumid IoT keskus kasutab protokolli jaoturi kaudu.
- Lüüsi talletab IoT Rummu identiteetide nimel simuleeritud seadmed ja toimib proxy kui simuleeritud seadmed IoT Rummu sõnumeid saata.

Järgnev skeem kujutab koosnevad proovi, sealhulgas gateway mooduleid:

![][1]


> [AZURE.NOTE] Moodulite läbinud sõnumeid otse üksteisele. Moodulite sõnumite avaldamiseks sisemine maaklerile, mis toimetab sõnumeid teiste moodulite tellimus mehhanismi nagu järgmises diagrammis näidatud. Vaadake lisateavet jaotisest [asjade Interneti Gateway SDK alustada][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Protokolli allaneelamine moodul

See moodul on saada andmeid seadmete, lüüsi kaudu ja pilve. Proovi moodul läbitakse neli ülesannet:

1.  See loob simuleeritud temperatuur andmete.
    
    Märkus: kui kasutate otse seadmed, moodul oleks andmeid lugeda nende füüsiliste seadmete.

2.  See paneb simuleeritud temperatuuri andmeid sõnumi sisu.

3.  See lisab võlts MAC aadress kinnisvara simuleeritud temperatuuri andmeid sisaldav sõnum.

4.  See muudab sõnumi kättesaadavaks järgmise mooduli ahelas.

> [AZURE.NOTE] Moodul nimega **protokolli X allaneelamine** joonis nimetatakse **simuleeritud seade** , lähtekoodi.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT Rummu ID moodul

See moodul skaneerib sõnumeid, mis sisaldavad atribuut, mis sisaldab lisatud protokolli allaneelamine moodul simuleeritud seadme MAC-aadressi. Kui moodul leiab sellise vara, lisab teise atribuudi IoT Hub seadme võti sõnum ja annab seejärel sõnumi kasutada järgmise moodulisse ahelas. See on, kuidas proovi seostab IoT Hub seadme identiteeti simuleeritud seadmed. Arendaja loob kaardistamine MAC aadressid ja asjade Interneti jaotur käsitsi konfigureerimise moodul osana. 

> [AZURE.NOTE]  See proov kasutab MAC aadress seadme unikaalne identifikaator ja korrelatsioonis IoT Hub seadme identiteedi. Siiski saate kirjutada oma mooduli, mis kasutab erinevaid ainuidentifikaator. Näiteks peate seadmed unikaalne seerianumbrid või koguda andmeid, mis on unikaalne seade nimi varjatud seda, mida saate kasutada IoT Hub seadme identiteedi määramiseks.

### <a name="iot-hub-communication-module"></a>Asjade Interneti keskus kommunikatsiooni moodul

See moodul võtab sõnumeid IoT Hub seadme identiteedi pandud eelmise moodul ja saadab sõnumi sisu HTTP kaudu IoT keskusse. HTTP on üks kolme protokolli IoT jaoturi kaudu.

Mitte seotud asjade Interneti jaotur simuleeritud seadmete, see moodul avab ühe HTTP-ühenduse IoT Rummu värav ja multiplexes üle seoses simuleeritud seadmete ühendused. See võimaldab ühtset juurdepääsu mitmetes muudes seadmetes, simuleeritud või mitte, kui oleks võimalik, kui see avatud unikaalne ühenduse iga seadme ühendamiseks.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Seadme pilve laadida simuleeritud näidis]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md