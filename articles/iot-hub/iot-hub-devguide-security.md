<properties
 pageTitle="Arendaja juhend - asjade jaoturi juurdepääsu reguleerimine | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja tutvustus – kuidas asjade jaoturi juurdepääsu ja hallata turvalisus"
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

# <a name="control-access-to-iot-hub"></a>Asjade jaoturi juurdepääsu reguleerimine

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse oma asjade jaoturi turvaliseks suvandid. Asjade jaoturi kasutab *õigused* anda juurdepääsu iga asjade jaoturi lõpp-punktid. Asjade jaoturi, mis põhineb funktsionaalsuse juurdepääsu piiramiseks õigused.

Selles artiklis kirjeldatakse:

- Erinevate õigused, et saate anda juurdepääsu oma asjade jaoturi seadme või tagaandmebaas rakendusse.
- Autentimis- ja märkide kasutab kontrollida õigusi.
- Kuidas piirata juurdepääsu teatud ressursid ulatus identimisteabe.
- Asjade jaoturi tugi X.509 serdid.
- Kohandatud seadme autentimise menetlustele olemasoleva seadme identiteedi registrite või autentimise skeeme.

### <a name="when-to-use"></a>Millal kasutada

Peab teil olema juurdepääsuõigused mis tahes asjade jaoturi lõpp-punktid. Näiteks seade peab sisaldama märgiks sisaldav turvalisus identimisteabe koos iga asjade jaoturi saadab sõnumi.

## <a name="access-control-and-permissions"></a>Juurdepääsu reguleerimine ja õigused

Saate anda [õigused](#iot-hub-permissions) järgmisel viisil:

* **Jaoturi taseme jagatud juurdepääsupoliitikaid**. Ühiskasutusega juurdepääsupoliitikaid anda suvalist kombinatsiooni [õigused](#iot-hub-permissions). Saate määratleda poliitikate [Azure portaali][lnk-management-portal], või kasutades programmiliselt [asjade jaoturi ressursi pakkuja REST API-de][lnk-resource-provider-apis]. Äsja loodud asjade jaoturi on järgmine vaikepoliitikaid.

    - **iothubowner**: kõik õigustega poliitika.
    - **teenus**: poliitika ServiceConnect õigustega.
    - **seadme**: poliitika DeviceConnect õigustega.
    - **registryRead**: poliitika RegistryRead õigustega.
    - **registryReadWrite**: poliitika õigused RegistryRead ja RegistryWrite.


* **Seadme kohta turvalisus mandaat**. Iga asjade jaoturi sisaldab [seadme identiteedi registri][lnk-identity-registry]. See register iga seadme jaoks saate konfigureerida turvalisus identimisteavet, mida vastav seade lõpp-punktid rakendatud **DeviceConnect** õiguste andmine.

Näiteks klõpsake tüüpilised asjade lahendus:

- Seadme juhtimise osa kasutab *registryReadWrite* poliitika.
- Sündmuse protsessor komponent kasutab *teenuse* poliitika.
- Käitusaja seadme business loogika komponent kasutab *teenuse* poliitika.
- Üksikute seadmete ühenduse asjade jaoturi identiteedi registris talletatud mandaadi abil.

## <a name="authentication"></a>Autentimine

Azure'i asjade jaoturi annab lõpp-punktid kontrollides märgiks ühiskasutusega juurdepääsupoliitikaid ja seadme identiteedi registri turvalisus mandaat.

Turvalisus mandaat, nt sümmeetriliste võtmed, saadetakse kunagi kaabel.

> [AZURE.NOTE] Azure'i asjade jaoturi ressursi pakkuja on turvatud Azure tellimuse kaudu on kõigi teenusepakkujate [Azure'i ressursihaldur][lnk-azure-resource-manager].

Ehitada ja turvalisus märkide kasutamise kohta leiate lisateavet teemast [asjade jaoturi turvalisus sõned][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protocol (protokoll) üksikasjad

Iga toetatud protokolli, nt MQTT, AMQP ja HTTP, edastus sõned erineval viisil.

MQTT kasutamisel ühendamine pakett sisaldab soovitud deviceId ClientId, {iothubhostname} / {deviceId} väli Username ja SAS luba väljale parool. {iothubhostname} peaks olema täielik CName asjade jaoturi (nt contoso.azure devices.net).

[AMQP]kasutamisel[lnk-amqp], asjade jaoturi toetab [SASL tavaline] [ lnk-sasl-plain] ja [AMQP taotluste-põhine-turvalisuse][lnk-cbs].

Kui kasutate AMQP taotluste-põhine-turvalisuse, standard täpsustab, kuidas nende tähiste edastada.

SASL tavaline, saab **kasutajanimi** :

* `{policyName}@sas.root.{iothubName}`Kui jaoturi taseme sõned abil.
* `{deviceId}@sas.{iothubname}`Kui seadme ulatusega sõned abil.

Mõlemal juhul parooli väli sisaldab luba, nagu on kirjeldatud [asjade jaoturi turvalisus sõned][lnk-sas-tokens].

HTTP rakendab autentimise **autoriseerimine** taotluse päises kehtiv luba kaasamise abil.

#### <a name="example"></a>Näide

Username (DeviceId pole tõstutundlik):`iothubname.azure-devices.net/DeviceId`

Parooli (genereerida SAS seadme Exploreriga):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure'i asjade jaoturi SDK-d] [ lnk-sdks] automaatselt genereerida sõned teenusega ühenduse loomise korral. Mõnel juhul on SDK-d ei toeta kõik protokollid või kõigi autentimismeetodite.

### <a name="special-considerations-for-sasl-plain"></a>TAVALINE SASL silmas pidada

SASL LIHTTEKSTI kasutamisel AMQP ühenduse asjade jaoturiga klient saate kasutada ühe märgiks iga TCP-ühenduse. Luba aegumisel TCP ühendus teenuse katkestab ja käivitab uuesti ühenduse loomise. Selline käitumine ajal rakenduse tagaandmebaas osa, mitte probleemne on seadme-side rakenduse väga kahjulik järgmistel põhjustel.

*  Lüüside tavaliselt ühenduse nimel mitmed seadmed. Kui kasutate SASL tavaline, neil erinevate TCP ühenduse iga seadme asjade jaoturiga ühenduse loomiseks. Selle stsenaariumi märkimisväärselt suureneb power ja võrgu ressursse ja suurendab latentsus iga seadme ühendust.
* Ressurssidega seadmed on kahjustanud kasutamisele ressursid iga Turbeloa aegumise pärast uuesti.

## <a name="scope-hub-level-credentials"></a>Ulatus taseme keskuse mandaati

Jaoturi taseme turbepoliitikate saab ulatus loomisega sõned URI piiratud ressursiga. Lõpp-punkti seadmest, seadme pilve sõnumite saatmiseks on näiteks **/devices/ {deviceId} / sõnumite/sündmused**. Samuti saate jaoturi taseme ühiskasutusega juurdepääsu poliitika **DeviceConnect** õigustega logida märgiks, mille resourceURI on **/devices/ {deviceId}**. Seda moodust loob märgiks, mida saab kasutada seadme **deviceId**nimel sõnumite saatmine ainult.

See on sarnane [sündmuse jaoturi Publisheri poliitika][lnk-event-hubs-publisher-policy], ja võimaldab teil rakendada kohandatud autentimismeetodite.

## <a name="security-tokens"></a>Turvalisus sõned

Asjade jaoturi kasutab turvalisus sõned autentida seadmed ja vältida, saates klahvid kaabel teenused. Lisaks turvalisus sõned on piiratud aja kehtivust ja ulatust. [Azure'i asjade jaoturi SDK-d] [ lnk-sdks] automaatselt genereerida sõned ei ole vaja erilist konfiguratsiooni. Mõnel juhul siiski nõuda kasutajalt luua ja kasutada turvalisus sõned otse. Nendeks otsene kasutamine MQTT, AMQP või HTTP pinna või rakendamine Turbeloa teenuse muster, nagu on selgitatud teemas [kohandatud seadme autentimise][lnk-custom-auth].

Asjade jaoturi võimaldab ka seadmete autentimiseks asjade jaoturi abil [X.509 serdid][lnk-x509]. 

### <a name="security-token-structure"></a>Turvalisus Turbeloa struktuur
Turvalisus sõned abil saate anda aja piiratud juurdepääsu seadmed ja meie teenuste teatud funktsioonid asjade keskuses. Saate ühendada vaid kinnitatud seadmed ja teenuste tagamiseks allkirjastamist turvalisus sõned ühiskasutusega kiirklahv poliitika või seadme identiteedi identiteedi registris talletatud sümmeetriline võti.

Märgiks allkirjastatud ühiskasutusega juurdepääsu poliitika võtme toetused Accessi kõik funktsioonid, mis on seotud ühiskasutusega juurdepääsuõigused poliitika. Teisalt, annab märgiks allkirjastatud seadme identiteedi sümmeetriline võti ainult seotud seadme identiteedi **DeviceConnect** õigust.

Turbeloa on järgmises vormingus:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Need on oodatud väärtusega.

| Väärtus | Kirjeldus |
| ----- | ----------- |
| {allkirja} | Mõne HMAC-i-SHA256 allkirja stringi vormi: `{URL-encoded-resourceURI} + "\n" + expiry`. **Tähtis**: võti on kaudu base64 dekodeerida ja kasutada klahvi HMAC-i-SHA256 arvutus sooritamiseks. |
| {resourceURI} | URI eesliide (kui lõigu) lõpp-punktid, mida saab kasutada koos selle märgiks, alustades asjade jaoturi (pole protocol) hostname (hostinimi). Näiteks`myHub.azure-devices.net/devices/device1` |
| {aegumise} | UTF8 stringid alates epohhi 00:00:00 UTC 1 Jaanuar 1970 sekundite arv. |
| {URL-kodeeringuga-resourceURI} | Väiksem juhul URL-i kodeerimine väiketähtedes ressursi URI |
| {policyName} | Selle märgiks viidatud ühiskasutusega juurdepääsu poliitika nimi. Puuduvad märgid, mis viitavad seadme-registri identimisteabe puhul. |

**Märkus eesliide**: The URI eesliide on arvutatud lõigu, mitte märgi. Näiteks `/a/b` on eesliite `/a/b/c` , kuid mitte `/a/bc`.

Järgmised Node.js koodilõigu näitab funktsiooni nimetatakse **generateSasToken** , mis arvutab kaudu sisendeid luba `resourceUri, signingKey, policyName, expiresInMins`. Järgmiste jaotiste üksikasjalikult, kuidas lähtestada erinevaid sisendeid erinevate Turbeloa kasutamine juhtudel.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Võrdlus, nagu on vastav Python kood on Turbeloa loomiseks:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Kuna kehtivuse luba on kinnitatud asjade jaoturi arvutites, on oluline, et kella masina, mis loob luba triiv minimaalsete.

### <a name="use-sas-tokens-in-a-device-client"></a>Seadme kliendi SAS märkide kasutamine

On kaks võimalust saada **DeviceConnect** õigused vastavusse asjade jaoturi koos turvalisus märkide: kasutada [sümmeetriline seadme võtit registrist seadme identiteedi](#use-a-symmetric-key-in-the-identity-registry)või [ühiskasutusega kiirklahv poliitika](#use-a-shared-access-policy).

Pidage meeles, et kõik funktsioonid puuetega inimestele juurdepääsetavate seadmetest on esitatud kujundus lõpp-punktid eesliitega `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Ainult nii, et asjade jaoturi autendib kindla seadme kasutab seadme identiteedi salajase võtme. Juhul kui ühiskasutusega juurdepääsu poliitika saab kasutada seadme funktsionaalsust, lahendus peate arvesse võtma väljastanud usaldusväärne alamkomponent nimega Turbeloa osa.

Seadme suunatud lõpp-punktid on (sõltumata protocol).

| Lõpp-punkti | Funktsioonid |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Seadme pilve sõnumeid saata. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Pilveteenuse-seadme sõnumeid. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Identiteedi registri sümmeetriline võti kasutamine

Seadme identiteedi sümmeetriline võti kasutamisel luua märgiks on policyName (`skn`) luba element on välja jäetud.

Näiteks kõigi seadme funktsioonidele juurdepääsemiseks loodud märgiks peaks olema järgmiste parameetrite abil:

* ressursi URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* sisselogimise võti: sümmeetriline alus selle `{device id}` identiteedi,
* poliitika nimi puudub
* aegumise igal ajal.

Näide, kasutades funktsiooni sõlm oleks:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Tulemus, mis annab juurdepääsu kõik funktsioonid device1, oleks:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] On võimalik luua turvalist märgiks [Seadme Exploreri].NET tööriista abil[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Kasutage ühiskasutusega juurdepääsu poliitika

Ühiskasutusega juurdepääsu poliitikast märgiks loomisel poliitika nimi välja `skn` kasutatud poliitika nimi peab olema seatud. Samuti on nõutav, et poliitika annab õiguse **DeviceConnect** .

On kaks peamist võtted kaudu ühiskasutusse antud juurdepääsupoliitikaid seadme funktsioonidele juurdepääsemiseks.

* [pilvepõhine protokolli lüüside][lnk-endpoints],
* [sümboolne teenuste] [ lnk-custom-auth] rakendada kohandatud autentimise skeeme.

Kuna ühiskasutusega juurdepääsu poliitika potentsiaalselt ühenduse nagu mis tahes seadme juurdepääsu anda, on oluline kasutada õige ressursi URI turvalisus sõned loomisel. See on eriti oluline Turbeloa teenuseid, mis on piiritlemiseks kindla seadme abil ressursi URI luba. Siinkohal on väiksem protokolli lüüside nagu nad on juba vahendamisel liikluse kõigi seadmete jaoks.

Näiteks looks Turbeloa teenuse abil varem loodud ühiskasutusega juurdepääsu poliitika nimega **seadme** märgiks järgmisi:

* ressursi URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* sisselogimise võti: üks, selle `device` poliitika,
* poliitika nimi: `device`,
* aegumise igal ajal.

Näide, kasutades funktsiooni sõlm oleks:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Tulemus, mis annab juurdepääsu kõik funktsioonid device1, oleks:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Lüüsi protokolli võiks kasutada samamoodi kõigis seadmetes lihtsalt seadmine ressursside URI- `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Kasutage turvalisus sõnet teenuse komponendid

Teenuse komponendid saate luua ainult turvalisus sõned andmise vajalikud õigused, nagu on selgitatud varem ühiskasutusega juurdepääsupoliitikaid abil.

Need on teenuse funktsioone, mis on esitatud lõpp-punktid kohta:

| Lõpp-punkti | Funktsioonid |
| ----- | ----------- |
| `{iot hub host name}/devices` | Luua, värskendada, tuua ja kustutada seadme identiteedid. |
| `{iot hub host name}/messages/events` | Seadme pilve sõnumeid. |
| `{iot hub host name}/servicebound/feedback` | Saada tagasisidet cloud-seadme sõnumite jaoks. |
| `{iot hub host name}/devicebound` | Pilveteenuse-seadme sõnumeid saata. |

Näiteks teenuse genereerimine abil varem loodud ühiskasutusega juurdepääsu poliitika nimega **registryRead** looks märgiks järgmiste parameetrite abil:

* ressursi URI: `{IoT hub name}.azure-devices.net/devices`,
* sisselogimise võti: üks, selle `registryRead` poliitika,
* poliitika nimi: `registryRead`,
* aegumise igal ajal.

    var lõpp-punkti = "myhub.azure-devices.net/devices";   var policyName = "seade";   var policyKey =...;

    var luba = generateSasToken (lõpp-punkti, policyKey, policyName 60);

Tulemus, mille soovite anda juurdepääsu kõik seadme identiteedid lugeda, oleks:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Toetatud X.509 serdid

Saate mis tahes x.509 vastav sert autentimiseks seadme asjade jaoturi. See hõlmab:

-   **Mõne olemasoleva x.509 vastav sert**. Seade võib juba olla x.509 vastav sert, mis sellega seotud. Seadme saate seda kasutada asjade jaoturi autentimiseks.

-   **X-509 omas loodud ja iseallkirjastatud sert**. Ettevõttesiseseid deployer või seadme tootjalt saate need serdid luua ja talletada vastav privaatvõti (ja sert) seadmes. Saate kasutada tööriistu, nt [OpenSSL] [ lnk-openssl] ja [Windows SelfSignedCertificate] [ lnk-selfsigned] kasuliku selleks.

-   **CA allkirjastatud x.509 vastav sert**. Samuti saate x.509 vastav sert, mis on loodud ja allkirjastatud sertimine sertimiskeskuse (CA) väljastatud seadme tuvastamine ja autentimiseks asjade jaoturi seade.

Seadme kasutada ka x.509 vastav sert või turvalisuse luba autentimine, kuid mitte mõlemat.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Mõne X.509 kliendi sert seadme registreerimine

[Azure'i asjade teenuse SDK C#] [ lnk-service-sdk] toetab (versioon 1.0.8+), mis kasutab mõnda kliendi x.509 vastav sert autentimise seadme registreerimine. Muude API-d, nt impordi/ekspordi seadmete toetavad ka X.509 kliendi serdid.

### <a name="c-support"></a>C\# tugi

Klassi **RegistryManager** võimaldab programmiline seadme registreerimiseks. Eelkõige **AddDeviceAsync** ja **UpdateDeviceAsync** meetodite kasutaja registreerida ja uuendada seadme registris asjade jaoturi seadme identiteedi lubamine. Need kaks võimalust võtta **seadme** eksemplari sisendina. **Seadme** klassi kuuluvad **autentimise** atribuut, mis võimaldab kasutajal määrata esmaseid ja teiseseid x.509 vastav sert thumbprints. Funktsiooni sõrmejälje tähistab SHA-1 räsi x.509 vastav sert, (talletatakse abil kahendarvu DER kodeering). Kasutajatel on võimalik esmane sõrmejälje või sekundaarne sõrmejälje või mõlemad. Esmaseid ja teiseseid thumbprints on toetatud, et hallata serdi edastamiseks stsenaariumid.

> [AZURE.NOTE] Asjade jaoturi ei nõua või kogu X.509 kliendi sert, ainult sõrmejälje salvestada.

Siin on näide C\# koodilõigu kasutab serti X.509 kliendi seadme registreerimiseks:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Kasuta X.509 kliendi serti ajal käitusaja toimingud

[Azure'i asjade seadme SDK .net-i] [ lnk-client-sdk] (versioon 1.0.11+) toetab X.509 kliendi sertide kasutamist.

### <a name="c-support"></a>C\# tugi

Klassi **DeviceAuthenticationWithX509Certificate** toetab  **DeviceClient** eksemplarid, mis kasutab X.509 kliendi serti.

Siin on näide koodilõik.

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Kohandatud seadme autentimine

Saate kasutada asjade jaoturi [seadme identiteedi registri] [ lnk-identity-registry] seadme kohta turvalisus mandaadi konfigureerimine ja juurdepääs juhtelemendi abil [sõned][lnk-sas-tokens]. Siiski kui asjade lahenduse juba märgatavat investeeringu kohandatud seadme identiteedi registri ja/või autentimise värviskeemi, saate integreerida selle infrastruktuuri asjade jaoturi abil loomisega *Turbeloa teenus*. Sel viisil, saate muude asjade funktsioonide teie lahendus.

Turbeloa teenus on kohandatud pilveteenuses. Seda kasutatakse asjade jaoturi *ühiskasutusega juurdepääsu poliitika* **DeviceConnect** õigustega *seadme ulatusega* tähiste loomiseks. Nende tähiste lubada ühenduse loomiseks oma asjade jaoturi seade.

  ![Turbeloa teenuse mustri juhised][img-tokenservice]

Turbeloa teenuse mustri Põhitoimingud on järgmised.

1. Luua oma asjade jaoturi **DeviceConnect** õigustega asjade jaoturi jagatud juurdepääsu poliitika. Saate luua selle poliitika [Azure portaali] [ lnk-management-portal] või programmiliselt. Turbeloa teenus kasutab seda poliitika logige sõned, loob see.
2. Kui seade peab teie asjade jaoturi juurdepääsu, taotleb allkirjastatud luba Turbeloa teenusepakkuja. Oma kohandatud seadme identiteedi registri/autentimise skeem määratlemiseks Turbeloa teenuse loomiseks luba kasutava seadme ID autentimist seade.
3. Turbeloa teenus tagastab märgiks. Luba on loodud, kasutades `/devices/{deviceId}` nimega `resourceURI`, koos `deviceId` seade, mis on kinnitatud. Turbeloa teenuse kasutab ühiskasutusega juurdepääsu poliitika ehitada luba.
4. Seadme kasutab luba otse asjade jaoturi.

> [AZURE.NOTE] Saate kasutada .net-i klassi [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] või Java klassi [IotHubServiceSasToken] [ lnk-java-sas] loomiseks märgiks Turbeloa teenust.

Turbeloa teenuse saate seada Turbeloa aegumise vastavalt soovile. Luba aegumisel asjade jaoturi severs seadme ühendust. Seejärel seade peab taotluse uue loa Turbeloa teenuse. Kui kasutate lühike aegumise aeg, suurendab nii seadmes kui ka Turbeloa teenuse laadi.

Ühenduse loomiseks oma jaoturi seade, endiselt lisama asjade jaoturi seadme identiteedi registrit – isegi juhul, kui seade on märgiks ja mitte seadme klahvi kasutamine suhtlemiseks. Seetõttu, saate jätkata kasutada seadme kohta juurdepääsu reguleerimine lubamine või keelamine seadme identiteedid [asjade jaoturi identiteedi registri] [ lnk-identity-registry] kui seade autendib märgiks. See leevendab kasutamise sõned pikk aegumise korda.

### <a name="comparison-with-a-custom-gateway"></a>Kohandatud lüüsi võrdlus

Turbeloa teenuse muster on soovitatav viis rakendada kohandatud identiteedi registri/kinnitamine värviskeemi asjade jaoturi. On soovitatav, kuna asjade jaoturi jätkuvalt käsitleda enamiku liikluse lahendus. Siiski on juhul, kui kohandatud autentimise skeem nii tihedalt Protocol (protokoll), et töötlemine liikluse (*kohandatud lüüsi*) teenus pole vaja. Järgmine näide on [transpordikihi Turve (TLS) ja eelnevalt jagatud võtmed (PSKs)][lnk-tls-psk]. Lisateabe saamiseks vt [protokolli lüüsi] [ lnk-protocols] teema.

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile rohkem teavet oma asjade jaoturi juurdepääsu kontrollimine.

## <a name="iot-hub-permissions"></a>Asjade jaoturi õigused

Järgmises tabelis on loetletud abil saate oma asjade jaoturi juurdepääsu õigusi.

| Õiguste            | Märkmete |
| --------------------- | ----- |
| **RegistryRead**      | Seadme identiteedi registri toetused lugemisõigus. Lisateavet leiate teemast [seadme identiteedi registri][lnk-identity-registry]. |
| **RegistryReadWrite** | Toetused lugeda ja kirjutada juurdepääs seadme identiteedi register. Lisateavet leiate teemast [seadme identiteedi registri][lnk-identity-registry]. |
| **ServiceConnect**    | Toetused juurde Cloud teenus suunatud suhtluse ja jälgimise lõpp-punktid. Näiteks seda määrab õigus tagaandmebaas pilveteenustega seadme pilve sõnumeid, cloud-seadme sõnumite saatmiseks ja tuua vastavate kohaletoimetamise litsents. |
| **DeviceConnect**     | Antakse juurdepääs seadme suunatud side lõpp-punktid. Näiteks seda määrab õigus cloud-seadme sõnumite seadme pilve sõnumite saatmiseks ja vastuvõtmiseks. See õigus kasutavad seadmed. |

## <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud teil on kasutada, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Nüüd on teada, kuidas määrata juurdepääsu asjade jaoturi, võib olla huvitatud arendaja juhend järgmisi teemasid:

- [Riigi ja konfiguratsioone sünkroonimine seadme kaksikud abil][lnk-devguide-device-twins]
- [Otsest meetodit seadmes][lnk-devguide-directmethods]
- [Mitmes seadmes tööde plaanimine][lnk-devguide-jobs]

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õppetükid

- [Azure'i asjade jaoturi kasutamise alustamine][lnk-getstarted-tutorial]
- [Kuidas saata cloud-seadme sõnumite asjade jaoturi abil][lnk-c2d-tutorial]
- [Protsessi asjade jaoturi seadme cloud sõnumite][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
