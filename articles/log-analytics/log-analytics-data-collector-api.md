<properties
    pageTitle="Logige Analytics HTTP Andmekoguja API | Microsoft Azure'i"
    description="Log Analytics HTTP Andmekoguja API abil saate lisada POSTITUSSE JSON andmete Log Analytics hoidlasse kliendilt, mida saate helistada REST API-ga. Selles artiklis kirjeldatakse, kuidas kasutada API ja on näited sellest, kuidas avaldada andmeid, kasutades erinevaid keeli."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Logige Analytics HTTP Andmekoguja API

Azure'i Log Analytics HTTP Andmekoguja API kasutamisel saate lisada postituse JavaScript Object märke (JSON) andmete Log Analytics hoidlasse kliendilt, mida saate helistada REST API-ga. Selle meetodi abil saate saata andmed kolmanda osapoole rakenduste või skriptide, nagu on käitusjuhendi Azure'i automaatika.  

## <a name="create-a-request"></a>Koosolekukutse loomiseks

Järgmised kaks tabelites on iga taotlus Log Analytics HTTP Andmekoguja API vajalike atribuute. Kirjeldame iga atribuut selle artikli üksikasjalikumalt.

### <a name="request-uri"></a>Taotluse URI

| Atribuut | Atribuut |
|:--|:--|
| Meetod | POSTITUSE |
| URI | https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Sisutüüp | rakenduse/json |

### <a name="request-uri-parameters"></a>Taotluse URI parameetrid
| Parameetri | Kirjeldus |
|:--|:--|
| Tellijaid  | Microsoft Operations Management Suite tööruumi ainuidentifikaator. |
| Ressurss    | API ressursinimi: / api/logid. |
| API versioon | Versiooni API kasutada seda teinud. Praegu ei 2016-04-01. |

### <a name="request-headers"></a>Taotluse päised
| Päis | Kirjeldus |
|:--|:--|
| Luba | Luba allkiri. Artikli, lugege selle kohta, kuidas luua mõne HMAC-i-SHA256 päis. |
| Log-tüüpi | Määrake esitatakse andmeid kirjetüüpi. Logi tüüp toetab praegu, vaid alfa-märki. Ei toeta numerics või erimärgid. |
| x-ms-date | Kuupäev, mis taotlust töödelda, RFC 1123 vormingus. |
| kellaaja loodud välja | Andmed, mis sisaldab andmeid üksuse ajatempli välja nimi. Kui määrate välja, siis selle sisu kasutatakse **TimeGenerated**. Kui sellel väljal pole määratud, on vaikimisi **TimeGenerated** aega, mida on märkimisväärselt sõnum. Sõnumi välja sisu tuleks järgida ISO 8601 vormingus YYYY-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Luba

Mis tahes taotluse Log Analytics HTTP Andmekoguja API peab sisaldama on autoriseerimine päis. Taotluse kinnitamiseks peate logima taotluse esmase või teisese võtme tööruumi, mis teeb taotluse abil. Seejärel andke allkiri taotluse osana.   

Siin on autoriseerimine päise vorming:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* on Operations Management Suite tööruumi ainuidentifikaator. *Allkirja* on [Räsi vastavalt sõnumi autentimise koodi (HMAC-i)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , mis on valmistatud taotlus ja seejärel arvutada [SHA256 algoritmi](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx)abil. Seejärel saate kodeerida selle Base64 kodeeringus abil.

Kasutage seda vormingut kodeerida **SharedKey** allkirja stringi:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Siin on näide allkirja stringi:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Kui olete signatuuri stringi, kodeerida selle UTF-8-kodeeringuga stringi HMAC-i-SHA256 algoritmi abil ja seejärel kodeerida Base64 tulemi. Kasutage seda vormingut.

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Järgmistes jaotistes näidised on proovi kood aitavad teil luua mõne autoriseerimine päis.

## <a name="request-body"></a>Koosolekukutse kehasse.

JSON peab olema sõnumi kehasse. See peab sisaldama ühe või mitme kirje atribuudi nimi ja väärtuse paarideks koos järgmises vormingus:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Mitme kirje ühe päringu saate partii, kasutades järgmises vormingus. Kõik kirjed peavad olema sama kirjetüübiga.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Kirje tüüp ja atribuudid

Kui esitate kaudu Log Analytics HTTP Andmekoguja API määratleda kohandatud kirjetüüpi. Praegu ei saa andmeid kirjutada olemasoleva kirjetüüpe, mis on loodud muude andmetüübid ja lahendusi. Log Analytics loeb sissetuleva andmeid ja seejärel loob atribuudid, mis vastavad teie sisestatud väärtuste andmetüübid.

Iga taotluse Log Analytics API peab sisaldama **Log-tüüpi** päist, mille nimi kirje tüüp. Järelliite **_CL** lisatakse automaatselt nime sisestamist eristada seda log muid kohandatud Logi. Näiteks kui sisestate nime **MyNewRecordType**, loob Log Analytics kirje **MyNewRecordType_CL**tüübiga. See aitab tagada, et kasutaja loodud Tippige nimed ja praeguse või tulevase Microsofti lahenduste saadetud konfliktid puuduvad.

Mõne atribuudi andmetüüp tuvastamiseks Log Analytics lisab järelliite atribuudi nimi. Kui atribuut sisaldavad väärtust null, pole kaasatud atribuudi kirje. Selles tabelis on esitatud atribuudi andmetüüp ja vastavate järelliide:

| Atribuudi andmetüüp | Järelliide |
|:--|:--|
| String    | _s |
| Kahendmuutuja   | _b |
| Kahekordne    | _d |
| Kuupäev/kellaaeg | _T |
| GUID      | _g |


Log Analytics kasutab iga atribuudi andmetüüpi, sõltub sellest, kas uue kirje kirjetüüp on juba olemas.

- Kui kirje tüüp pole olemas, loob Log Analytics uus. Log Analytics kasutab JSON tüüp järeldamine iga uue kirje atribuudi andmetüüp.
- Kui kirje tüüpi olemas, proovib Log Analytics loomine olemasoleva atribuudi järgi uus kirje. Kui andmetüübi atribuudi uue kirje ei vasta ja ei saa teisendada tüüp, või kui kirje sisaldab atribuut, mida pole olemas, Log Analytics loob uue atribuudi, mis on oluline järelliide.

Näiteks sõnadest esitamise looks kirje kolme atribuudid, **number_d**, **boolean_b**ja **string_s**:

![Valimi kirje 1](media/log-analytics-data-collector-api/record-01.png)

Kui teie seejärel esitatud järgmine kirje, kõik väärtused, mis on vormindatud stringidena, ei muuda atribuute. Olemasolevate andmete tüüpi saab teisendada need väärtused:

![Valimi kirje 2](media/log-analytics-data-collector-api/record-02.png)

Kuid kui tegite siis selle järgmise esitamise, Log Analytics, luuakse uus atribuudid **boolean_d** ja **string_d**. Ei saa teisendada need väärtused:

![Valimi kirje 3](media/log-analytics-data-collector-api/record-03.png)

Kui saatsite järgmine kirje, siis enne kirje tüüp on loodud, looks Log Analytics kolme atribuudid, **edukate_arv**, **boolean_s**ja **string_s**kirje. See kirje iga algse väärtused on vormindatud stringina:

![Valimi kirje 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Andmete piirangud
On mõned piirangud ümber Log Analytics andmete kogumine API sisestatud andmed.

- Maksimaalselt 30 MB postituse Log Analytics andmete koguja API kohta. See on mahupiirang ühe postituse. Kui andmed ühte postitada, mis ületab 30 MB, peaksite kuni väiksemate suurusega tükkideks andmete tükeldamine ja saatke neile samaaegselt. 
- Kuni 32 KB limiit välja väärtused. Kui välja väärtus on suurem kui 32 KB, kärbitakse andmed. 
- Soovitatav maksimaalne arv konkreetset tüüpi väljad on 50. See on praktiline limiit kasutatavuse ja otsingu kasutuskogemuse perspektiivi.  


## <a name="return-codes"></a>Saatja koodid

HTTP olekukoodi 202 tähendab, et kutse on aktsepteeritud töötlemiseks, kuid töötlemine on veel lõpetamata. See näitab, et toiming on lõpule viidud.

Selles tabelis on loetletud võib tagastada teenuse olek tähiseid täielik kogum.

| Kood | Olek | Tõrkekood | Kirjeldus |
|:--|:--|:--|:--|
| 202 | Aktsepteeritud |  | Kutse on aktsepteeritud. |
| 400 | Vigane päring | InactiveCustomer | Tööruum on suletud. |
| 400 | Vigane päring | InvalidApiVersion | Teenus ei tuvastatud API versioon, mis on määratud. |
| 400 | Vigane päring | InvalidCustomerId | Selle tööruumi ID ei sobi. |
| 400 | Vigane päring | InvalidDataFormat | Esitati lubamatu JSON. Vastuse sisu võib sisaldada rohkem teavet tõrke kõrvaldamiseks. |
| 400 | Vigane päring | InvalidLogType | Logi tüüp määratud keskkonnas erimärgid või numerics. |
| 400 | Vigane päring | MissingApiVersion | API versioon pole määratud. |
| 400 | Vigane päring | MissingContentType | Sisutüübi pole määratud. |
| 400 | Vigane päring | MissingLogType | Nõutav väärtus Logi tüüp pole määratud. |
| 400 | Vigane päring | UnsupportedContentType | Sisutüüp on seatud mitte **rakenduse/json**. |
| 403 | Keelatud | InvalidAuthorization | Teenust ei saa autentida kutse. Veenduge, et tööruumi ID-d ja ühenduse võti on lubatud. |
| 500 | Sisemine serveritõrge | UnspecifiedError | Teenuse ilmnes sisemine tõrge. Proovige uuesti taotluse. |
| 503 | Teenus pole saadaval | ServiceUnavailable | Teenus pole praegu saadaval, Saada kutsed. Proovige uuesti teie taotlus. |

## <a name="query-data"></a>Päringu andmete

Päringu andmete esitatud Log Analytics HTTP Andmekoguja API, kirjete **Tüüp** , mis on võrdne **LogType** väärtus, mis on määratud, otsige **_CL**on lisatud sünkroonimistõrke. Näiteks kui kasutasite **MyCustomLog**, siis te soovite kõigi kirjete koos **Tüüp = MyCustomLog_CL**.


## <a name="sample-requests"></a>Valimi taotlused

Järgmistest jaotistest leiate näidised, kuidas Log Analytics HTTP Andmekoguja API andmete edastamiseks, kasutades erinevaid keeli.

Tehke iga proovi päise luba muutujate määramiseks järgmist.

1. Operations Management Suite portaalis valige paani **sätted** ja valige vahekaart **Ühendatud allikatest** .
2. **Tööruumi ID**paremale, valige Kopeeri ikoon ja seejärel kleepige ID **Tellijaid** muutuja väärtus.
3. **Primaarvõtme**paremale, valige Kopeeri ikoon ja seejärel kleepige ID **Ühiskasutuses klahvi** muutuja väärtus.

Teise võimalusena saate muuta log tüübi ja JSON andmete muutujaid.

### <a name="powershell-sample"></a>PowerShelli näidis

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C# näidis

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Python näidis

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Järgmised sammud

- [Kuvakujundaja](log-analytics-view-designer.md) abil saate luua kohandatud vaadete andmed, mida saate esitada.
