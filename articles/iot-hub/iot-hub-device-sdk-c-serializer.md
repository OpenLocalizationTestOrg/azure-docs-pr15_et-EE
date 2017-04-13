<properties
    pageTitle="Azure'i asjade seadme SDK C - Serializer | Microsoft Azure'i"
    description="Lisateavet C Azure'i asjade seadmes SDK Serializer teegi kasutamine"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure'i asjade seadme SDK C – Lisateavet leiate teemast serializer

[Esimene artikkel](iot-hub-device-sdk-c-intro.md) selle sarja kasutusele **Azure'i asjade seadme SDK c**. Järgmise artiklis esitatud [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md)rohkem üksikasjalikku kirjeldust. Selles artiklis on lõpule jõudnud ulatus SDK esitada ülejäänud osa veel üksikasjalik kirjeldus: **serializer** teek.

Sissejuhatav artiklis kirjeldatud, kuidas kasutada **serializer** teegi sündmusi sõnumite saatmiseks ja vastuvõtmiseks asjade keskuse kaudu. Selles artiklis me laiendada selle arutelu täielikuma selgituse, kuidas oma andmeid **serializer** makro keele modelleerimise esitada. Artikkel ka sisaldab rohkem üksikasju selle kohta, kuidas teegis serializes sõnumite (ja mõnel juhul, kuidas saate määrata sariväljaanne käitumine). Kirjeldame mõningaid parameetreid saate muuta loote mudelid suurust määratleda.

Lõpuks artiklit kontrollida mõned teemad, mis eelmise artikleid, nt sõnumi ja atribuudi töötlemine. Kui me teada, need funktsioonid töötavad **serializer** teegi abil, nagu nad **IoTHubClient** teegiga samal viisil.

Kõik selles artiklis kirjeldatud põhineb **serializer** SDK näidised. Kui soovite jälgida, leiate selle **simplesample\_amqp** ja **simplesample\_http** rakendusi lisada Azure asjade seadme SDK c.

Saate leiate **Azure'i asjade seadme SDK c** [Microsoft Azure'i asjade SDK-d](https://github.com/Azure/azure-iot-sdks) GitHub hoidla ja [C API viite](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)API üksikasjade kuvamine.

## <a name="the-modeling-language"></a>Keele modelleerimine

[Sissejuhatav artiklis](iot-hub-device-sdk-c-intro.md) toodud selle sarja kasutusele **Azure'i asjade seadme SDK c** – näites toodud keele modelleerimine on **simplesample\_amqp** rakendus:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Nagu näete, põhineb keele modelleerimine C makrod. Alustamist alati definitsiooni koos **hakake\_NIMERUUM** ja lõpevad alati **END\_NIMERUUM**. On tavaline nimeruumi nimi oma ettevõtte või, nagu järgmises näites projekti, millega töötate.

Mis läheb nimeruumi sees on mudeli määratlusi. Sel juhul on ühe näidis kuvatakse tuulemõõdu. Veel kord mudelit saab nimeks midagi, kuid tavaliselt on see nimega seadmes või soovite vahetada asjade jaoturi andmete tüüpi.  

Mudelid sisaldavad määratlus sündmusi, saate samuti saate asjade jaoturis ( *toimingud*) vastu võetud sõnumitest sissepääsu asjade jaoturi ( *andmed*). Nagu näete, näiteks, võistlusalal on tüüp ja nimi; toimingud on nimi ja valikuliste parameetrite (iga tüüp).

Mis on selle valimi ei näidatud on täiendavad andmetüüpe, mis ei toeta SDK. Käsitleme selle edasi.

> [AZURE.NOTE] Asjade jaoturi viitab andmete seadme saadab sellele *sündmusi*, samal ajal keele modelleerimine viitab selle *andmed* (määratletud **WITH_DATA**abil). Samuti asjade jaoturi viitab andmete saadate ja seadmed *sõnumite*ajal keele modelleerimine viitab see *toimingud* (määratletud **WITH_ACTION**abil). Pange tähele, et järgmiste terminite tähendust võib selle artikli vaheldumisi kasutada.

### <a name="supported-data-types"></a>Toetatud andmetüübid

**Serializer** teegis loodud mudelid on toetatud järgmised andmetüübid:

| Tüüp                    | Kirjeldus                            |
|-------------------------|----------------------------------------|
| kahekordne                  | kahekordseks arvutustäpsuse ujuv punktide arv |
| int                     | 32-bitise täisarvulise                         |
| hõljumine                   | ühe arvutustäpsuse ujuv punktide arv |
| pikk                    | pikk täisarv                           |
| int8\_t                 | 8-bitise täisarvulise                          |
| Int16\_t                | 16-bitise täisarvulise                         |
| Int32\_t                | 32-bitise täisarvulise                         |
| Int64\_t                | 64-bitise täisarvulise                         |
| bool                    | kahendmuutuja                                |
| ASCII\_char\_ptr        | ASCII string                           |
| EDM\_kuupäeva\_aja\_OFFSET | kuupäev aja offset                       |
| EDM\_GUID               | GUID                                   |
| EDM\_KAHENDARVU             | kahendarvuks                                 |
| DEKLAREERIDA\_KIRJEL         | keerukas andmetüüp                      |

Alustame viimase andmetüüpi. Funktsiooni **DEKLAREERIMISLAUSET\_KIRJEL** võimaldab määratleda keerukate andmetüüpidega, mis on lihtsad tüüpi rühmitustega. Nende rühmade võimaldavad määratleda mudelit, mis näeb välja umbes järgmine:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Meie mudel sisaldab ühte sündmuse tüüp **TestType**. **TestType** on keerukas tüüp, mis sisaldab mitu liiget, mis näitavad ühiselt lihtsad tüübid ei toeta **serializer** keele modelleerimine.

Mudeli selliselt, saame kirjutada andmeid saata asjade jaoturi, mis kuvatakse järgmine kood:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Põhimõtteliselt, me väärtuse määramisel igale liikmele **testi** struktuuri ja seejärel helistamiseks **SendAsync** saata **testi** andmete sündmuse pilveteenusesse. **SendAsync** on helper funktsiooni, mis saadab ühe andmete sündmuse asjade jaoturi.

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

See funktsioon serializes antud andmete sündmus ja saadab asjade jaoturi abil **IoTHubClient\_SendEventAsync**. See on sama koodi kirjeldatud eelmise artikleid (**SendAsync** kapseloi loogika mugav funktsioon sisse).

Ühe muude helper funktsiooni kasutatakse eelmine kood on **GetDateTimeOffset**. See funktsioon muudab aja tüüp väärtus **EDM\_kuupäeva\_aja\_OFFSET**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Kui koodi käivitamiseks saadetakse asjade jaoturi järgmine teade:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Pange tähele, et sariväljaanne on JSON, mis on loodud **serializer** teegi vorming. Samuti võtke arvesse, et iga liige serialiseeritud JSON objekt vastab liikmete **TestType** meil määratletud meie mudel. Väärtused ka täpselt vastama need, mida kasutatakse koodi. Pange tähele, et kahendarvu andmed on base64-kodeeringuga: "AQID" on selle base64 kodeerimine {0x01, 0x02, 0x03}.

Selles näites näitab **serializer** teegi kasutamise eelis – seda lubab meile saata JSON pilves, ilma vajaduseta konkreetselt tegeleda sariväljaanne meie rakenduses. Kõik me ei pea muretsema on seadmine andmete sündmuste väärtused meie mudel ja seejärel lihtsa API-de saata nende sündmuste pilveteenusesse.

Selle teabe me saame määratleda mudelid, mis sisaldavad toetatud andmetüübid, sh keerukate tüüpi (me isegi sisaldab keerukate tüübid keerukate muid) vahemikus. Siiski ta seeriasertide JSON loodud Ülaltoodud näites toob esile oluline. *Kuidas* saadame andmete teegiga **serializer** määrab täpselt, kuidas funktsiooni JSON on loodud. See on mis käsitleme edasi.

## <a name="more-about-serialization"></a>Lisateavet leiate teemast sariväljaanne

Eelmises jaotises tõsta esile **serializer** teegis loodud väljund näide. Selles jaotises selgitame, kuidas teegi serializes andmed ja kuidas seda käitumist sariväljaanne API-de abil saate määrata.

Eelnevalt sariväljaanne arutelu, et töötame koos uue mudeli termostaat põhjal. Kõigepealt loome annab mõned tausta seda stsenaariumi me proovite aadress.

Soovime termostaat, et temperatuuri- ja mudel. Iga tekstilõigu andmeid saab saata asjade jaoturi teisiti. Vaikimisi termostaat ingresses temperatuur sündmuse iga 2 minuti järel; niiskus sündmus on sissetunginud iga 15 minuti järel. Kui kumbki sündmus on sissetunginud, peab sisaldama ajatempli, mis näitab aega, kuvatakse vastav õhutemperatuuri või oli mõõta.

Antud stsenaariumi, demonstreerime kahe eri viisid andmete modelleerimise ja selgitame efekti, et modelleerimine on sarjadesse jaotatud väljund.

### <a name="model-1"></a>Mudeli 1

Siin on esimese versiooni mudelit, mis toetab eelmises näites:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Märkus sündmustest andmeid mudel sisaldab: ** **temperatuuri** - ja**. Erinevalt eelmistes näidetes, iga sündmuse tüüp on määratletud struktuur **DEKLAREERIMISLAUSET\_KIRJEL**. **TemperatureEvent** sisaldab temperatuuri mõõtmine ja ajatempel; **HumidityEvent** sisaldab niiskus mõõtühikute ja ajatempel. See mudel annab meile loomulikus võimalus eespool kirjeldatud andmeid mudel. Kui saadame sündmuse pilveteenusesse, saadame kas temperatuur/ajatempli või paari niiskus/ajatempli.

Saadame temperatuur sündmuse pilverakendusse teenuse kood, nt mõni järgmistest:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Me kasutada raske koodiga väärtusi temperatuuri ja niiskuse proovi kood, kuid oletage, et meil on tegelikult toomine need väärtused proovide vastavate andurite termostaat.

Ülaltoodud kood kasutab **GetDateTimeOffset** helper, mis võeti kasutusele varem. Tühjendage hiljem muutub põhjustel järgmine kood konkreetselt eraldab jaotamine ja saata sündmus. Eelmise koodi serializes temperatuur sündmuse puhvrit sisse. Seejärel **sendMessage** on abimees funktsiooni (kaasatud **simplesample\_amqp**), mis saadab sündmuse asjade jaoturi:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Järgmine kood on **SendAsync** helper, siis me ei minna üle uuesti siin eelmises jaotises kirjeldatud alamhulk.

Kui eelmine kood saata temperatuur sündmuse käitamiseks sarjadesse jaotatud vormi sündmuse saatmist asjade jaoturi:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Me saadate temperatuuri, mis tüüpi **TemperatureEvent** ja selle kirjel sisaldab liikme **temperatuur** ja **kellaaeg** . See kajastub otse sarjadesse jaotatud andmetega.

Samuti saadame niiskus sündmuse see kood:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Sarjadesse jaotatud vorm, mis on saadetud asjade jaoturi kuvatakse järgmiselt:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Ka see on ootuspäraselt.

Mudeli, võite ette kujutada, kuidas täiendavat sündmused saab hõlpsalt lisada. Määratlete abil veel struktuurid **DEKLAREERIMISLAUSET\_KIRJEL**, ja kaasata vastava sündmuse mudeli kasutamine **asendaja\_andmete**.

Nüüd vaatame mudelit muuta nii, et see sisaldaks samad andmed, kuid erinev struktuur.

### <a name="model-2"></a>Mudeli 2

Kaaluge selle alternatiivse mudeli üks kohal.

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Sellisel juhul olete me kõrvaldada selle **DEKLAREERIMISLAUSET\_KIRJEL** makrod ja lihtsalt määratlemisel meie stsenaariumi, kasutades lihtsat tüüpi keele modelleerimine andmete üksused.

Lihtsalt hetkeks vaatame Ignoreeri **soovitud sündmus** . Siin on see kõrvale, sissepääsu **temperatuur**kood:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Asjade jaoturi saadab järgmine sarjadesse jaotatud sündmus järgmine kood:

```
{"Temperature":75}
```

Ja saatmise niiskus sündmuse kood kuvatakse järgmiselt:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Järgmine kood saadab see asjade jaoturi:

```
{"Humidity":45}
```

Siiani on veel mingeid üllatusi. Nüüd vaatame muutmine SERIALIZE makro kasutamine.

Makro **SERIALIZE** võib kuluda mitu andmed sündmused argumentidena. See võimaldab meil serialiseerida ** **temperatuuri** - ja** sündmuse koos ja saadab asjade jaoturi ühe kõne:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Võite arvata, on tulemus järgmine kood, millele saadetakse andmehulkade sündmuste asjade jaoturi:

[

{"Temperatuur": 75},

{"Niiskus": 45}

]

Teisisõnu, võib-olla eeldate, et see kood on sama nimega saatmise ** **temperatuuri** - ja** eraldi. See on lihtsalt mugavuse nii sündmuste edastamiseks **SERIALIZE** sama kõne. Kuid see pole täheregistrit. Selle asemel saadab ülaltoodud kood asjade jaoturi sündmuse ühe andmeid:

{"Temperatuur": 75, "niiskus": 45}

See võib tunduda kummaline, kuna meie mudel määratleb ** **temperatuuri** - ja** *eraldi* sündmustest:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Veel punkti, me ei mudeli neid sündmusi, kus on sama struktuur ** **temperatuuri** - ja** :

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Kui soovime kasutada seda mudelit, oleks kergem mõista, kuidas ** **temperatuuri** - ja** ära saata sama sarjadesse jaotatud sõnumis. Siiski võib olla pole selge miks see töötab nii, kui te kaotate nii andmete sündmuste **SERIALIZE** näidise 2.

Selline käitumine on kergem mõista, kui teate, mis teeb **serializer** teegi oletused. See arusaamiseks minge tagasi meie mudel:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Selle objekti rakendusse terminite mudeli mõelda. Sel juhul me ei modelleerimine füüsilise seadmega (termostaat) ja selle seadme sisaldab nagu ** **temperatuuri** - ja**atribuute.

Saadame kogu riigi kood, nt mõni järgmistest mudeli:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Eeldades, et temperatuur väärtused, määratakse niiskus ja kellaaeg, näeme sündmuse, nagu see saadetakse asjade jaoturi:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Mõnikord võib ainult soovite saata mudeli *osa* atribuute (see on eriti väärtuse true, kui mudeli sisaldab palju andmeid sündmused) pilveteenusesse. See on kasulik saata alamhulka andmete sündmusi, näiteks varasemas näites:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

See loob täpselt sama sarjadesse jaotatud sündmuse ajal, kui me oli määratletud on **TemperatureEvent** liikmega **temperatuur** ja **kellaaeg** , vaid siis, kui me ei mudeliga 1. Sel juhul olid luua täpselt sama sarjadesse jaotatud sündmuse abil muu mudeli (mudel 2), kuna me kutsutud **SERIALIZE** erineval viisil.

Oluline on see, et kui te kaotate mitme andmete sündmuste **SERIALIZE,** siis eeldatakse, et iga sündmuse on atribuut ühe JSON-objekt.

Parim lahendus sõltub sellest, mida ja kuidas mõelda mudelisse. Kui saadate "sündmused" pilve ja iga sündmuse sisaldab määratletud atribuute, teeb esimene lähenemine mõttes palju. Sel juhul kasutaksite **DEKLAREERIMISLAUSET\_KIRJEL** määratlemine iga sündmuse struktuuri ja seejärel lisada need oma mudeli soovitud **asendaja\_andmete** makro. Saatke iga sündmuse nagu esimese eeltoodud näites. Seda moodust oleks ainult läheb ühe andmete sündmuse **SERIALIZER**.

Kui asute kaaluma mudelisse objekti rakendusse viisil, siis teine lähenemine võib sobib teile. Sel juhul elementide määratletud abil **asendaja\_andmete** on "atribuudid" objekti. Mis tahes alamhulga sündmused edasi **SERIALIZE** , mis teile meeldib, olenevalt sellest, kui teie "objekt" riigi, mida soovite saata pilveteenusesse.

Madal lähenemine on õige või vale. Ainult olema teadlik mingitest **serializer** teegi tööpõhimõte ja valige modelleerimine lähenemine, mis sobib kõige paremini teie vajadustele.

## <a name="message-handling"></a>Sõnumi töötlemine

Siiani selles artiklis on ainult arutada asjade jaoturi saatmise sündmused ja pole adresseeritud sõnumeid. On põhjus on selles, et mida läheb vaja teada sõnumeid on suuresti hõlmatud [varasemas artiklis](iot-hub-device-sdk-c-intro.md). Tagasi kutsuda artiklist sõnumite töötlemiseks registreerumine funktsiooni tagasihelistamise teade:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Seejärel kirjutamise tagasihelistamise funktsiooni, kui teate, mis on saanud:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Selle rakendamine **IoTHubMessage** nõuab teatud funktsiooni iga toimingu mudelist. Näiteks kui mudelisse määratleb toimingut:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Peate määratlema selle allkirjaga funktsiooni:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** nimetatakse siis, kui see sõnum saadetakse teie seade.

Me pole ülevaade veel on sõnumi sarjadesse jaotatud versioon välja näeb. Teisisõnu, kui soovite oma seadmesse **SetAirResistance** sõnumi saatmine, mis näeb?

Kui soovite sõnumi saata seadme, oleks seda teha Azure'i asjade teenuse SDK kaudu. Soovite teada, millised stringi saatmiseks autonoomsest teatud toimingu. Sõnumi vorming üldine kuvatakse järgmiselt:

```
{"Name" : "", "Parameters" : "" }
```

Saadate serialiseeritud JSON objekt koos kahe atribuudid: **nimi** on nimi toimingu (sõnum) ja **parameetrite** sisaldab selle toimingu parameetrid.

Näiteks viidata **SetAirResistance** saate saata selle sõnumi seadme:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Toimingu nimi peab täpselt vastama mudelist määratletud toiming. Parameetrite nimed peavad vastama ka. Samuti võtke arvesse Tõstutundlikkus. Suur on alati **nimi** ja **Parameetrid** . Veenduge, et Erista suurtähti toimingu nime ja parameetrite mudelist. Selles näites on toimingu nimi "SetAirResistance" ja ei "setairresistance".

Selles jaotises kirjeldatakse kõike, mida peaksite teadma, kui sündmuste saatmise ja vastuvõtmise sõnumite **serializer** teek. Enne kui lähevad, vaatame katta mõned parameetreid saate ise konfigureerida, et kontrollida, kui suur on mudelisse.

## <a name="macro-configuration"></a>Makro konfigureerimine

Kui kasutate **Serializer** teegi SDK, millega peaks arvestama oluline osa on leitud azure-c jagatud – kasuliku teek.
Kui teil on Azure-asjade Interneti-SDK-d hoidla GitHub--Rekursiivsed suvandiga kaudu, siis leiate selle ühiskasutusega kasuliku teegi siin:

```
.\\c\\azure-c-shared-utility
```

Kui teil on pole teeki, leiate [siit](https://github.com/Azure/azure-c-shared-utility).

Ühiskasutusega kasuliku teegi leiate järgmises kaustas:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

See kaust sisaldab nimetatakse lahenduse Visual Studios **makro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

See lahendus programmi genereeritud soovitud **makro\_utils.h** faili. On vaikimisi makro\_kaasas SDK utils.h fail. See lahendus võimaldab teil muuta mõningaid parameetreid ja seejärel uuesti päise faili järgmiste parameetrite põhjal.

Kahe võtme parameetrid olla seotud on **nArithmetic** ja **nMacroParameters** , mis on määratletud need kaks rida leitud makro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Need väärtused on väärtused, mis on kaasas SDK. Iga parameetri on järgmine tähendust.

-   nMacroParameters – määrab ühe DEKLAREERIMISLAUSET võib olla mitu parameetrite\_mudeli makro määratlus.

-   nArithmetic – määrab lubatud mudeli liikmete arv.

Need parameetrid on oluline põhjus on, sest need määrata, kui suur võib olla mudelisse. Näiteks, kaaluge selle mudeli määratlus.

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Nagu eelnevalt mainitud **DEKLAREERIMISLAUSET\_mudel** C makro. Mudeli nimed ja **asendaja\_andmete** lause (veel mõne muu makro) on parameetrid **DEKLAREERIMISLAUSET\_mudel**. määratleb **nMacroParameters** , saate lisada mitu parameetrite **DEKLAREERIMISLAUSET\_mudel**. Tõhusalt, see määratleb, kui palju andmeid sündmus ja toimingu deklaratsiooni saate määrata. Sellisel kujul vaikimisi piiri 124 tähendab see umbes 60 toimingud ja andmete sündmuste kombinatsiooni mudel saate määratleda. Kui proovite seda limiiti ületada, kuvatakse koostaja tõrkeid, mis näevad välja umbes järgmine:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

**NArithmetic** parameeter on rohkem kui rakenduse töös makro keel.  Seda määrab võib olla mudelisse, sh **DECLARE_STRUCT** makrode liikmete arv. Kui alustate koostaja tõrkeid, nagu see kuvatakse, siis peaksite proovima suurendab **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Kui soovite muuta need parameetrid, muutke väärtusi makro\_utils.tt faili, Kompileeri makro\_utils\_h\_generator.sln lahenduse ja Käivita kompileeritud programm. Kui te seda teete, uus makro\_utils.h fail on loodud ja paigutada selle. \\levinud\\inc kataloogi.

Selleks, et kasutada makro uue versiooni\_utils.h, eemaldage **serializer** Nugeti pakett teie lahendus ja selle asemel kaasata **serializer** Visual Studio projekti. See võimaldab koostada vastu serializer teegi lähtekoodi oma kood. See rühm sisaldab värskendatud makro\_utils.h. Kui soovite seda **simplesample\_amqp**, Alustuseks Nugeti pakett serializer teegi eemaldamine lahendus:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Lisage oma Visual Studio lahendus selle projekti:

> . \\c\\serializer\\koostamine\\windows\\serializer.vcxproj

Kui olete lõpetanud, teie lahendus peaks välja nägema selline:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Nüüd kui kompileerida oma lahenduse värskendatud makro\_utils.h on kaasatud oma kahendarvuks.

Pange tähele, et need väärtused, mis on piisavalt ei ületa koostaja piirangud. Selles osas on **nMacroParameters** peamine parameeter, mille abil asjaga. C99 spec saate määrata, et makro määratlus on lubatud vähemalt 127 parameetrid. Microsoft koostaja järgib spec täpselt (ja on piiratud 127), seega ei saa te suurendamiseks **nMacroParameters** Lisaks vaikimisi. Teiste koostajad võib võimaldavad teha (nt GNU koostaja toetab kõrgema piirmäära).

Siiani oleme käsitlenud peaaegu kõike peate teadma teegiga **serializer** koodi kirjutamist. Enne, vaatame uuesti mõned teemad eelmise artikleid, mis võivad tekkida kohta.

## <a name="the-lower-level-apis"></a>Madalama taseme API-d

Valimi rakendus, mille värskendustest selles artiklis on **simplesample\_amqp**. See näide kasutab seda kõrgema taseme (selle mitte-"LL") API-de sündmuste saata ja vastu võtta sõnumeid. Kui kasutate neid API-d, käivitatakse tausta teemat mis hoolitseb nii sündmuste sõnumite saatmine ja vastuvõtmine. Siiski saate kõrvaldada lõime tausta ja võtta konkreetsete juhtida, millal sündmuste või saadate sõnumeid, mis pilvest madalama taseme (LL) API-d.

[Eelmise artiklis](iot-hub-device-sdk-c-iothubclient.md)kirjeldatud on funktsioonide kogum, mis koosneb kõrgema taseme API:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_hävitada

Nende API-d on näidatud **simplesample\_amqp**.

Olemas on ka mõni analoogne madalama taseme API-de.

-   IoTHubClient\_ll:\_CreateFromConnectionString

-   IoTHubClient\_ll:\_SendEventAsync

-   IoTHubClient\_ll:\_SetMessageCallback

-   IoTHubClient\_LL\_hävitada

Pange tähele, et madalama taseme API-de töötada täpselt samamoodi, nagu on kirjeldatud eelmiste artiklite. Esimese komplekti API-de saate kasutada siis, kui soovite tausta jutulõnga sündmuste saatmise ja vastuvõtmise sõnumite. Teine API-de määramine kasutada siis, kui soovite konkreetsete juhtida, millal te saata ja vastu võtta andmete asjade keskuse kaudu. Kas kogum API-de töö võrdselt hästi **serializer** teek.

Näiteks madalama taseme API-de kasutamise **serializer** teegiga, leiate selle **simplesample\_http** rakendus.

## <a name="additional-topics"></a>Täiendavad Teemad

Mõned teemad mainida uuesti on atribuut töötlemise, alternatiivse seadme identimisteabe ja konfiguratsiooni suvandite abil. Need on kõik teemad, mis [eelmises artiklis](iot-hub-device-sdk-c-iothubclient.md). Peaasi on, et kõik need funktsioonid töötavad teegiga **serializer** samamoodi nagu nad teegiga **IoTHubClient** . Näiteks kui soovite manustamine atribuudid sündmuse mudelisse, saate kasutada **IoTHubMessage\_atribuudid** ja **kaardi**\_**AddorUpdate**, nii nagu eespool kirjeldatud:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Kas sündmus on loodud **serializer** teegist või käsitsi loodud **IoTHubClient** teek pole oluline.

Alternatiivsete seadme mandaadi, kasutades **IoTHubClient\_LL\_loomine** töötab sama hästi nimega **IoTHubClient\_CreateFromConnectionString** eraldamiseks on **IOTHUB\_kliendi\_toime**.

Lõpuks **serializer** teegi kasutamisel saate määrata konfiguratsiooni võimalusi **IoTHubClient\_LL\_SetOption** lihtsalt tegite **IoTHubClient** teegi kasutamise korral.

Funktsioon, mis on kordumatu **serializer** teek on lähtestamine API-d. Enne alustamist teegis, kellele helistate **serializer\_käivitamise**:

```
serializer_init(NULL);
```

Seda saab teha vaid enne kui helistate **IoTHubClient\_CreateFromConnectionString**.

Samamoodi kui olete lõpetanud töötamise teeki, saate teha viimase kõne on **serializer\_deinit**:

```
serializer_deinit();
```

Muul juhul kõik muud funktsioonid ülalpool töötab **serializer** teegis teek **IoTHubClient** samamoodi nagu. Mis tahes järgmiste teemade kohta leiate lisateavet teemast [eelmises artiklis](iot-hub-device-sdk-c-iothubclient.md) toodud selle sarja.

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis kirjeldatakse üksikasjalikult **serializer** teegi sisalduvad **Azure asjade seadme SDK c**ainulaadne aspekte. Teave on esitatud võiks olla hea mõistmine mudelite abil sündmuste sõnumite saatmiseks ja vastuvõtmiseks asjade keskuse kaudu.

Sellega lõpeb ka kolmeosalise sarja **Azure'i asjade seadme SDK c**rakenduste arendamise kohta. See peaks olema piisavalt teavet, et saate alustada vaid teile põhjalik tundmine on API-de tööpõhimõtted. Lisateabe saamiseks on mõned näited Tarkvaraarenduskomplektist, mis ei hõlma. Muul juhul [SDK dokumendid](https://github.com/Azure/azure-iot-sdks) on hea ressurss lisateabe saamiseks.


Asjade jaoturi jaoks arendamise kohta leiate lisateavet teemast [Asjade jaoturi SDK-d][lnk-sdks].

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
