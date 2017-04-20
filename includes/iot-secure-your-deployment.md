# <a name="securing-your-iot-deployment"></a>IoT juurutamise tagamine

Selles artiklis detail järgmise taseme tagamise infrastruktuuri Azure asjade Interneti-põhise asjade Interneti (IoT). See ühendab konfigureerimist ja juurutamist iga komponendi tasandil üksikasjades. See sisaldab ka võrdlusi ja valikuid konkureerivate meetodite vahel.

Tagada Azure IoT juurutamine võib jaotada järgmiste valdkonnas:

- **Seadme turvalisus**: tagada asjade Interneti seade, kuigi looduses juurutamist.
- **Ühenduse turvalisus**: IoT seadme ja IoT Rummu vahel kõik andmed on konfidentsiaalsed ning kaitstud.
- **Pilveteenuse turvalisust**: abil andmete turvalisuse tagamiseks, kuigi see liigub ja on salvestatud pilves.

![Kolmes turvalisuse valdkonnas][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Seadme ettevalmistamine ja autentimine

Azure Lot Suite võimaldab kinnitada IoT seadmete poolt kahest allpool toodud meetodist:

- Pakkudes kordumatu võti (turvalube) iga seade, mis saab sellega, et asjade Interneti jaotur suhelda.

- Kohta-seadme abil [X.509 sert] [ lnk-x509] ja et IoT Hub seadme autentimiseks. Autentimise tagatakse, et seadme privaatvõti on teadmata väljaspool seadet alati, pakkudes suuremat turvalisust.

Turvalisuse turbelubade meetod pakub iga kõne tehtud sellega, et asjade Interneti keskusse täiendavaid sümmeetriline võti iga kõne autentimist. X.509 autentimise võimaldab autentimist IoT seadme füüsilise kihi osana TLS ühenduse loomine. Turvalisuse luba põhinev meetod saab ilma X.509 autentimist, mis on vähem turvaline muster. Valida ise meetod on peamiselt tingitud kui turvaline autentimine seade peab olema, ja turvaline ladustamine (et isiklikku võtit turvaliselt hoida) seadme olemasolu.

## <a name="iot-hub-security-tokens"></a>Asjade Interneti jaotur turvaluba

Asjade Interneti keskus kasutab turbelubade seadmed ja teenused, vältida saatmine võtmed võrgus autentimiseks. Lisaks on turvaluba piiratud aja ja ulatuse. Azure'i IoT Rummu SDK-d automaatselt genereerida märgid ei ole vaja mingeid hõlmata. Mõnel juhul siiski nõuda kasutaja loomiseks turvaluba otse. Need hõlmavad otsest kasutamist MQTT, AMQP või HTTP küljele või rakendamise Turbelubade teenuse muster.

Turbeloa ja selle kasutamist täpsemalt leiate järgmised artiklid:

-   [Turvalisuse turbelubade struktuur][lnk-security-tokens]
-   [Kasutades SAS märkide seade][lnk-sas-tokens]

Iga IoT Jaoturil on [Seadme identiteedi registri] [ lnk-identity-registry] mis saab luua teenus, järjekorras, mis sisaldab lennu ajal pilve seade sõnumeid, näiteks seadme kohta ressursside ja juurdepääsu seadme poole suunatud tulemusnäitajad. Asjade Interneti jaotur identiteedi register pakub turvaliseks hoidmiseks seadme identiteeti ja seadistamiseks lahendus. Kohaselt tuleb seadme identiteeti saab lisada loendit luba või Blokeeri loend, mis võimaldab täielikku kontrolli üle seadmele juurdepääsu. Senist anda üksikasjalikumat teavet seadme identiteedi registri ülesehitus ja toetatud tegevuste.

[Asjade Interneti Keskus toetab protokolle, näiteks MQTT, AMQP ja HTTP][lnk-protocols]. Kõigi protokolli kasutada turvaluba IoT seadmest IoT keskusse erinevalt:

- AMQP: SASL ja AMQP nõuetel põhinev tagatis ({policyName}@sas.root.{iothubName} puhul Rummu tasandil märgid; {deviceId} seadme hõlmavad märkide korral).

- MQTT: Ühendage paketi kasutab {deviceId} {ClientId} {IoThubhostname} / {deviceId} **kasutajanime** ja **parooli** väli märgiks SAS.

- HTTP: Kehtiv luba on loa taotluse header.

IoT Hub seadme identiteedi registri saab seadistada seadme kohta turvamandaat ja juurdepääsu kontroll. Aga kui IoT lahendust juba märkimisväärseid investeeringuid [kohandatud seadme identiteedi registri ja autentimisseadmete kava][lnk-custom-auth], see saab integreerida olemasoleva infrastruktuuri IoT Hubi kasutamise luues turbelubade teenus.

### <a name="x509-certificate-based-device-authentication"></a>X.509 sertifikaati põhinev seade autentimine

[Device põhineva X.509 certificate] kasutamist[ lnk-use-x509] ja selle seotud era- ja avaliku võtmepaari võimaldab täiendavat autentimist füüsilise kihi. Privaatvõtit hoitakse turvaliselt seadme ja pole tuvastatav väljaspool seadet. X.509 sert sisaldab teavet seadme, näiteks seadme ID ja muud korralduslikud üksikasjad. Serdi allkirja luuakse privaatvõtme abil.

Kõrgetasemeline seadmega tehtavaid makseid voolu:

- Seosta füüsilise seadmega – seadme identiteedi ja/või serti X.509 ajal seadme tootmisel või tellides seadmega seotud identifikaatori.

- Luua vastav kanne identiteedi IoT keskuses – seade ja sellega seotud seadme teabest IoT Hub seadme registris.

- Turvaliselt hoida X.509 serdi pöidlajälg IoT Hub seadme registri.

### <a name="root-certificate-on-device"></a>Juursert seadmes

Küll TLS turvaline koos asjade Interneti jaotur, asjade Interneti seade autendib asjade Interneti jaoturit kasutades juursert, mis kuulub seadme SDK. C kliendi SDK sertifikaadi asub kaustas "\\c\\certs" all on repo juurkausta. Kuigi need juursertide on pikaajaliste, nad ikka võib kehtivuse või tunnistatakse kehtetuks. Kui uuendamine seadme sertifikaat on mingil viisil, ei saa seade ühendada hiljem IoT jaoturi (või pilve teenus). Olles et juursert update kui juurutamist taastab seade IoT tõhusalt leevendada seda riski.

## <a name="securing-the-connection"></a>Tagada ühenduse

Interneti-ühenduse vahel IoT seadmest ja asjade Interneti jaotur on tagatud kasutades transpordikihi turbe (TLS) standardiga. Azure'i IoT toetab [TLS 1.2][lnk-tls12], TLS 1.1 ja TLS 1.0 selles järjekorras. TLS 1.0 toetust antakse ainult tagasiühilduvuse. Soovitatakse kasutada TLS 1.2, sest see tagab kõige.

Azure'i Lot Suite toetab järgmisi salakiri Suites, selles järjekorras.

| Salakiri sviit | Pikkus |
|--------------|--------|
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (ekvivalentides 7680 bitti RSA) FS | 256    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (ekvivalentides 3072 bitti RSA) FS | 128    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (ekvivalentides 7680 bitti RSA) FS           | 256    |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (ekvivalentides 3072 bitti RSA) FS           | 128    |
| TLS\_RSA\_koos\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_koos\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_koos\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_koos\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_koos\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_koos\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_koos\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Tagada pilve

Azure'i IoT Hub võimaldab [juurdepääsu reguleerimise põhimõtted] mõiste[ lnk-protocols] iga turvavõtme jaoks. Ta kasutab õigusi alljärgnevate juurdepääsu igale IoT Rummu lõpp. Õigusi piirata funktsionaalsust vastavalt IoT jaoturiga.

- **RegistryRead**. Seadme identiteedi register annab lugemisõigus. Lisateabe saamiseks vaadake [Seadme identiteedi registri][lnk-identity-registry].

- **RegistryReadWrite**. Toetused lugeda ja kirjutada juurdepääs seadme andmed registrisse. Lisateabe saamiseks vaadake [Seadme identiteedi registri][lnk-identity-registry].

- **ServiceConnect**. Toetuste juurdepääs pilve teenus suunatud side- ja järelevalve lõpp. Näiteks annab loa lõppfaasi pilveteenuste teade kuvatakse seadme pilv, pilve seadme sõnumite saatmiseks ja vastuvõtmiseks vastavad kohaletoimetamine tunnustati.

- **DeviceConnect**. Antakse juurdepääs seadme poole suunatud side lõpp-punktid. Näiteks annab loa pilve seadme sõnumite seadme pilve sõnumite saatmiseks ja vastuvõtmiseks. Seda õigust kasutavad seadmed.

On kaks võimalust saada **DeviceConnect** lubade IoT jaoturi kohta [turbelubade][lnk-sas-tokens]: seadme identiteedi võtit või ühiskasutusega juurdepääsu poliitika võti. Lisaks on oluline märkida, et kõik funktsioonid kättesaadavad seadmed ei puutu kujunduselt näitajatega eesliitega `/devices/{deviceId}`.

[Veebiteenuste komponente saab luua ainult turvaluba] [ lnk-service-tokens] kasutades jagatud juurdepääsu poliitikad vastavad õigused anda.

Azure'i IoT keskus ja muud teenused, mis võib olla üheks lahenduseks lubada juhtimine kasutades Azure Active Directory kasutajad.

Andmete tarvitavad Azure IoT keskus saab tarbida mitmesuguseid Azure Stream Analytics ja Azure'i bloobimälu. Need teenused võimaldavad juurdepääsu haldamine. Loe need teenused ja Valikud allpool:

- [Azure'i DocumentDB][lnk-docdb]: skaleeritav, täielikult indekseeritud andmebaasi teenust poolstruktureeritud andmeid, mis haldab metaandmete seadmete olete ette, nagu atribuudid, konfiguratsiooni ja turvalisusega seotud omadusi. DocumentDB on suure jõudlusega ja kõrge efektiivsusega, skeemi diagnostika indekseerimine andmete ja rikas SQL päringu liides.

- [Azure Stream Analytics][lnk-asa]: reaalajas oja töötlemine pilves, mis võimaldab teil kiiresti arendada ja odav analüüsilahenduse paljastada reaalajas majanduspsühholoogia seadmed, sensorid, infrastruktuuri ja rakendusi kasutada. Täielikult haldaja teenuse andmeid saab skaalal mis tahes ajal endiselt saavutada kõrge tootlikkusega, madal latentsus ja elastsus.

- [Azure'i rakendust teenuste][lnk-appservices]: pilv platvormivärskenduse ehitada võimas web ja mobiilne apps, mis ühendavad andmetele kõikjal; pilves või kohapeal. Ehitamine kaasates mobiilne apps, iOS, Android ja Windows. Integreerida oma tarkvara kui teenus (SaaS) ja ettevõtte out-of-the-box ühenduse kümneid pakutavaid pilveteenuseid ja ettevõtte. Koodi oma lemmik keel ja IDE (.NET, NodeJS, PHP, Python või Java) ehitada veebirakendusi ja API-d kiiremini kui kunagi varem.

- [Loogika rakenduste][lnk-logicapps]: The loogika rakenduste funktsioon Azure'i rakendust Service aitab integreerida oma IoT lahendust oma ärivaldkonna olemasolevate süsteemide ja töövoo protsesside automatiseerimiseks. Loogika rakendusi lubab disaini töövoo, mis algavad päästikut ja seejärel sooritamist juhiseid — eeskirju ja meetmeid, mis võimas ühendused abil saate integreerida äritegevuse käigus. Loogika rakendused pakuvad out-of-the-box suur ökosüsteemi SaaS, pilvepõhist ühenduvust ja kohapeal rakendusi.

- [Azure bloobimälu][lnk-blob]: usaldusväärne, ökonoomne salvestusruumi seadmete saada pilv andmete jaoks.

## <a name="conclusion"></a>Järeldus

Käesolevas artiklis antakse ülevaade rakendamist tasemel üksikasjad projekteerimine ja juurutamine kasutades Azure IoT asjade Interneti infrastruktuuri. Iga komponendi turvaliseks konfigureerimise oluline tagada üldine asjade Interneti infrastruktuuri. Saadaval Azure IoT KHG tagavad teatud paindlikkust ja valik; siiski iga valik võib põhjustada turvaohtu. On soovitatav, et need valikud hinnata riski/kulude kalkulatsiooniga kaudu.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
