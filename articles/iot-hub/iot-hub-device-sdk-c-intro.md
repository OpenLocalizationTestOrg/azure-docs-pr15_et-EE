<properties
    pageTitle="Azure'i asjade seadme SDK c abil | Microsoft Azure'i"
    description="Lisateavet ja c. proovi kood Azure'i asjade seadme SDK töötamise alustamine"
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

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Azure'i asjade seadme SDK c tutvustus

**Azure'i asjade seadme SDK** on loodud aitab lihtsustada sündmuste saatmise ja vastuvõtmise sõnumite **Azure'i asjade jaoturi** teenuse kogum. On erinevaid versioone Tarkvaraarenduskomplektist, iga suunamise teatud platvormi, kuid selles artiklis kirjeldatakse **Azure'i asjade seadme SDK c**.

Azure'i asjade seadme SDK C on kirjutatud ANSI C maksimeerimiseks teisaldamist (C99). See muudab sobivad hästi sõitmiseks arvu platvormide ja seadmete, eriti siis, kui ketas minimeerimine ja mälukasutus on prioriteet.  

On mitmeid platvormid, millel on testitud SDK, (vt selle [Azure'i sertifitseeritud asjade seadme kataloogi](https://catalog.azureiotsuite.com/) üksikasjad). Kuigi see artikkel sisaldab proovi kood töötab Windowsi platvormi juhendavad tutvustused, pidage meeles, et selles artiklis kirjeldatud kood on täpselt sama üle vahemiku, Toetatud platvormid.

Selles artiklis saate kasutusele arhitektuur Azure'i asjade seadme SDK c. Kuidas lähtestada seade teeki, saata sündmuste asjade jaoturi kui ka sõnumite saadud demonstreerime. Selle artikli teave peaks olema piisavalt SDK kasutamise alustamine, kuid annab Lisateavet teekide viitu.

## <a name="sdk-architecture"></a>SDK arhitektuur

Saate leiate **Azure'i asjade seadme SDK c** [Microsoft Azure'i asjade SDK-d](https://github.com/Azure/azure-iot-sdks) GitHub hoidla ja [C API viite](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)API üksikasjade kuvamine.

Teekide uusima versiooni leiate selle hoidla **juhtslaidi** haru:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

See hoidla sisaldab terve pere Azure'i asjade seadme SDK-d. See artikkel on Azure asjade seadme SDK *c* **c** kausta leitav kohta.

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* Core rakendamise SDK leiate funktsiooni **iothub\_kliendi** kausta, mis sisaldab kõige madalam API kiht SDK rakendamine: **IoTHubClient** teek. **IoTHubClient** teek sisaldab API-de rakendamise töötlemata sõnumside sõnumite saatmiseks asjade jaoturi kui ka selle sõnumeid. See teek kasutamisel Teie vastutate rakendamiseks sõnumi sariväljaanne (lõpuks abil serializer valimi allpool kirjeldatud), kuid muud üksikasjad suhelda asjade jaoturi käsitletakse teie eest.
* **Serializer** kaust sisaldab helper funktsioone ja näidised, mis näitab, kuidas andmeid serialiseerida enne saatmist Azure'i asjade jaoturi abil kliendi teek. Pange tähele selle serializer kasutamine ei ole kohustuslik ja ainult pakutud. Kui kasutate **serializer** teeki, alustate määratlemine mudelit, mis määrab sündmuste asjade jaoturi kui eeldate, et see saadud sõnumeid saata. Kui seate mudeli on määratletud, pakub SDK API pind, mis võimaldab teil hõlpsasti töötada sündmused ja sõnumite ilma muretsema sariväljaanne üksikasjad.
Teegi sõltub muude avatud allika teekide eeldab, et transport mitu Protokollid (MQTT, AMQP) abil.
* Teegi **IoTHubClient** sõltub muude avatud allika teekide:
   * [Azure'i C ühiskasutuses kasuliku](https://github.com/Azure/azure-c-shared-utility) teek, mis pakub levinud funktsioonid põhitoimingud (stringi, loendi stringimanipuleerimisfunktsioonide IO, nt jne...) vaja üle mitme Azure'i seotud C SDK-d
   * Kliendi pool rakendamine AMQP ressursi piirang seadmete jaoks optimeeritud [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) teek.
   * [Azure'i uMQTT](https://github.com/Azure/azure-umqtt-c) teek, mis on üldotstarbeline teegi MQTT protokolli rakendamise ja ressursside piirang seadmete jaoks optimeeritud.

Kõik on kergem mõista, vaadates näide kood. Järgmistes jaotistes teile mõned lihtsad rakendused, mis sisalduvad SDK kaudu. See peaks teile hea tunne erinevate võimaluste arhitektuuri kihid SDK kui ka selle API-de tööpõhimõtted tutvustus.

## <a name="before-running-the-samples"></a>Enne käivitamist näidised

Enne käivitamist näidised Azure'i asjade seadme SDK C peate looma eksemplari teenuse Azure tellimuse kui teil pole juba on üks ja 2 ülesannete täitmiseks:
* teie arenduskeskkond ettevalmistamine
* Hankige seadme mandaat.

Kui teil on vaja luua eksemplari Azure'i asjade jaoturi Azure tellimuse, järgige juhiseid [siin](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

[Readme-faili](https://github.com/Azure/azure-iot-sdks/tree/master/c) , mis on kaasas SDK leiate juhised oma arenduskeskkond ettevalmistamine ja saada seadme mandaat.
Järgmised jaotised sisaldavad mõned täiendavad kommentaaris neid juhiseid.

### <a name="preparing-your-development-environment"></a>Teie arenduskeskkond ettevalmistamine

Paketid on esitatud mõned platvormide (nt Nugeti for Windows või apt_get Debiani ja Ubuntu) ja näidised kasutamiseks need paketid, kui need on saadaval, on üksikasjalikult järgmisi juhiseid koostamiseks teek ja näidised otse vormi koodi.

Esmalt peate saada koopia SDK GitHub ja siis koostada allikas. Saate **juhtslaidi** kontorist [GitHub hoidla](https://github.com/Azure/azure-iot-sdks)allika koopia.

Kui allalaaditud allika koopia, peate täitma SDK artiklis ["Ettevalmistamine oma arenduskeskkond"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)kirjeldatud juhiseid.


Järgnevalt on toodud mõned näpunäited, mis aitavad teil täita ettevalmistamine juhendis kirjeldatud juhiseid.

-   Kui installite **CMake** kasuliku, valige suvand **CMake** lisamiseks süsteemi tee **Kõik kasutajad** ( **praeguse kasutaja** töötab ka lisamise):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Enne, kui avate **jaoks VS2015 arendaja Käsuviip**, installige käsurea Gittools. Nende tööriistade installimiseks tehke järgmist:

    1. Käivitage **Visual Studio 2015** installiprogrammi (või valida **Microsoft Visual Studio 2015** **programmid ja funktsioonid** Juhtpaneel ja valige **Muuda**).
    
    2. Veenduge, et valitud funktsiooni **Git for Windows** installer, kuid võite ka **GitHub Visual Studio laiend** ruudu esitada IDE integreerimine.

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Tööriistade installimiseks häälestusviisardi lõpuleviimine.

    4. Funktsiooni **PATH** muutuja Git tööriistad **prügikasti** kataloogi lisada. Opsüsteemis Windows, see näeb välja umbes järgmine:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Kui olete kõik ["Ettevalmistamine oma arenduskeskkond"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) lehel kirjeldatud juhiseid lõpule viinud, olete valmis koostada valimi rakendused.

### <a name="obtaining-device-credentials"></a>Seadme mandaadi saamiseks

Nüüd, kui teie arenduskeskkond on häälestatud, on järgmiseks ei saada seadme mandaadikomplekt.  Seade on asjade jaoturi juurdepääsu, peate esmalt lisama seadme asjade jaoturi seadme register. Kui lisate oma seadme saate seadme identimisteavet, mida vajate selleks, et seade on võimalik ühendada asjade jaoturiga kogumist. Vaatame järgmises jaotises toodud valimi rakendusi oodatud identimisteabe **seadme ühendusstringi**kujul.

On paar tööriistu, mis on esitatud SDK avatud allika hoidlas asjade jaoturi haldamise hõlbustamiseks. See rakendus nimega seadme Explorer, teine on soovitud node.js rist platvormi CLI tööriista nimega iothub-Exploreri Windowsiga. Saate lisateavet nende tööriistade [siin](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Mida me kaudu näidised töötab Windows selle artikli, kasutame seadme Exploreri tööriist. Kuid abil saate iothub-Exploreri kui eelistate CLI tööriistad.

[Seadme Exploreri](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) tööriist kasutab Azure asjade teenuse teekide asjade jaoturi, sh seadmete lisamise kohta saate kasutada mitmesuguseid funktsioone. Kui kasutate seadme Exploreri seadme lisamiseks, saate vastava ühendusstringi. Teil on vaja seda teha valimi rakendusi käivitada ühendusstring.

Juhuks, kui te pole veel protsessi juba tuttav, Järgnevalt kirjeldatakse, kuidas seadme Exploreri abil saate lisada mõne seadme ja seadme ühendusstringi hankimine.

Seadme Exploreri tööriista [SDK vabastage lehe](https://github.com/Azure/azure-iot-sdks/releases)kohta leiate Windows Installeri. Kuid abil saate käivitada ka otse oma kood, avades **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** **Visual Studio 2015** ja koostamise lahendus.

Programmi käivitamisel kuvatakse see liides:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Sisestage esimesele väljale oma **Asjade jaoturi ühendusstring** ja klõpsake nuppu **Värskenda**. See konfigureerib tööriist nii, et asjade jaoturi suhtlemiseks.

Kui asjade jaoturi ühendusstring on konfigureeritud, klõpsake vahekaarti **haldus** :

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

See on, kus saate hallata oma asjade keskuses registreeritud seadmete.

Saate luua seadme, klõpsates nuppu **Loo** . Eelsisestatav klahvid (esmaseid ja teiseseid) komplekt kuvatakse dialoog. Kõik, mida teha, on Sisestage **Seadme ID** ja seejärel klõpsake nuppu **Loo**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Kui seade on loodud, värskendatakse registreeritud kõigis seadmetes, millest ühte äsja loodud seadmete loend. Kui paremklõpsate uude seadmesse, kuvatakse see menüü.

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Kui valite suvandi **kopeerige ühendusstring valitud seadme jaoks** , on lõikelauale kopeeritud seadme ühendusstring. Ühendusstringi koopia säilitamiseks. Peate seda, kui valim rakendusi, mis on kirjeldatud järgmistes jaotistes.

Kui olete ülal esitatud juhiseid, olete valmis alustama, töötavad mõned koodi. Mõlema näidised on pidev ülaosas peamine faili, mis võimaldab teil sisestage ühendusstring. Näiteks vastava rea soovitud **iothub\_kliendi\_valimi\_amqp** rakendus kuvatakse järgmiselt.

```
static const char* connectionString = "[device connection string]";
```

Kui soovite jälgida, sisestage siia oma seadme ühendusstring, Kompileeri lahendus ja teid saab käitada valimi.

## <a name="iothubclient"></a>IoTHubClient

Sees on **iothub\_kliendi** kausta azure-asjade Interneti-SDK-d hoidlas, on **näidised** kausta, mis sisaldab rakendus nimega **iothub\_kliendi\_valimi\_amqp**.

Windowsi versioon on **iothub\_kliendi\_valimi\_ampq** taotlus sisaldab järgmisi Visual Studio lahendus:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

See lahendus sisaldab ühe projekti. Tuleb märkida, on see lahendus installitud neli NuGet-paketid:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Peate alati **Microsoft.Azure.C.SharedUtility** paketi, kui töötate SDK. Kuna see valimi tugineb AMQP, peate lisama ka **Microsoft.Azure.uamqp** ja **Microsoft.Azure.IoTHub.AmqpTransport** paketid (on samaväärne pakette MQTT ja HTTP). Kuna valimi kasutab **IoTHubClient** teeki, peate lisama **Microsoft.Azure.IoTHub.IoTHubClient** paketi ka teie lahendus.

Saate otsida valimi rakenduse rakendamiseks soovitud **iothub\_kliendi\_valimi\_amqp.c** lähtefail.

Selle rakenduse valimi kasutame sõelub, mida on vaja kasutada **IoTHubClient** teeki.

### <a name="initializing-the-library"></a>Teegi lähtestamine

> [AZURE.NOTE] Enne alustamist on teekidega töötamise, peate teha mõned platvormi teatud lähtestamine. Näiteks kui kavatsete kasutada AMQP Linux saate lähtestada OpenSSL teegi. Näidiste [GitHub hoidla](https://github.com/Azure/azure-iot-sdks) helistage kasuliku funktsioon **platform_init** kui kliendi käivitub ja kõne funktsiooni **platform_deinit** enne väljumist. Need funktsioonid on deklareeritud failis "platform.h" päis. Nende funktsioonide määratlused uurima oma target platvormi [hoidla](https://github.com/Azure/azure-iot-sdks) , määratlemaks, kas tuleb kaasata kõik platvormi lähtestamine kood oma kliendi jaoks.

Teekidega töötamise alustamiseks tuleb esmalt eraldada soovitud asjade jaoturi kliendi pidet:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Pange tähele, et me ei läbides koopia meie seadme ühendusstringi funktsioonile (üks me saadud seadme Explorerist). Me määrata ka protokoll, mille soovime kasutada. Selles näites kasutatakse AMQP, kuid MQTT ja HTTP on ka soovitud suvand.

Kui teil on kehtiv **IOTHUB\_kliendi\_toime**, saate alustada helistada API-d sündmuste sõnumite saatmiseks ja vastuvõtmiseks asjade keskuse kaudu. Vaatame seda edasi.

### <a name="sending-events"></a>Sündmuste saatmine

Sündmuste saata asjade jaoturi nõuab, et te tehke järgmist:

Esmalt looge sõnum:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Sõnumi edasi saata:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Iga kord, kui saadate sõnumi, saate määrata viide tagasihelistamise funktsiooni, kui andmed on saadetud:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Pange tähele, et kõne on **IoTHubMessage\_Destroy** funktsioon, kui olete lõpetanud ja sõnum. Te peate selle kõne vabastamiseks eraldatud sõnumi loomisel.

### <a name="receiving-messages"></a>Sõnumite vastuvõtmine

Sõnumi on asünkroonne toiming. Esmalt registreerimist pakkumist kasutada, kui seade saab sõnumi:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Viimane parameeter on kehtetu kursor kohta, mida iganes soovite. Valimis, see on viit täisarv, kuid keerukamaid andmete viit võib olla. See võimaldab funktsiooni tagasihelistamise sõitmiseks ühiskasutuse oleku helistajaga selle funktsiooni.

Kui seade saab sõnumi, kasutatakse funktsiooni registreeritud tagasihelistamise:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Pange tähele, et kasutate selle **IoTHubMessage\_GetByteArray** funktsioon sõnumit, mis selles näites on string alla laadida.

### <a name="uninitializing-the-library"></a>Uninitializing teek

Kui olete lõpetanud sündmuste saatmise ja vastuvõtmise sõnumid, te saate lähtestamise asjade teek. Probleemi selleks järgmised funktsioon kõne:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

See vabastada ka eelnevalt eraldatud ressursse, on **IoTHubClient\_CreateFromConnectionString** funktsioon.

Nagu näete, on lihtne saata sündmused ja teegiga **IoTHubClient** sõnumeid. Teegi tegeleb üksikasju suhtlemine asjade jaoturi, sh millist kasutama protokolli (arendaja seisukohast see on lihtne konfiguratsioon suvand).

Teegi **IoTHubClient** pakub täpse kontrolli serialiseerida seadme saadab asjade jaoturi sündmuste kohta. Mõnel juhul on see eelis, kuid muudel juhtudel on mõni rakendamise üksikasjad, millega te ei soovi asjaga. Kui see on nii, kaaluge **serializer** teeki, mille järgmises jaotises kirjeldatakse.

## <a name="serializer"></a>Serializer

Põhimõtteliselt **serializer** teegi asub **IoTHubClient** teegi SDK peal. Asjade jaoturi aluseks suhtlemiseks kasutab **IoTHubClient** teek, kuid see lisab modelleerimise võimalusi eemaldada koormust tegelemiseks sõnumi sariväljaanne arendaja. Kuidas see töötab teegi, on parim näitab näide.

**Serializer** hoidla azure-asjade Interneti-SDK-d kaustas on **näidised** kausta, mis sisaldab rakendus nimega **simplesample\_amqp**. Selle valimi Windowsi versioon sisaldab järgmisi Visual Studio lahendus.

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Sarnaselt eelmise valimi sellest sisutüübist sisaldab mitmeid Nugeti pakette.

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Oleme näinud enamik eelmise valimis, kuid **Microsoft.Azure.IoTHub.Serializer** on uus. See on vajalik, kui me kasutame **serializer** teek.

Saate otsida valimi rakenduse rakendamine on **simplesample\_amqp.c** faili.

Järgmistes jaotistes teile selle valimi põhiosadele kaudu.

### <a name="initializing-the-library"></a>Teegi lähtestamine

Teegiga **serializer** töötamise alustamiseks tuleb helistada lähtestamine API-d.

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Kõne edastamiseks selle **serializer\_käivitamise** funktsioon on ühekordse kõne ja kasutatakse lähtestamine aluseks teek. Seejärel saate kõne on **IoTHubClient\_CreateFromConnectionString** funktsioon, mis on sama API nagu **IoTHubClient** proovi. Kõne seda määrab oma seadme ühendusstringi (see on ka siis, kui valite protokolli soovite kasutada). Pange tähele, et see näide kasutab AMQP transport, kuid oleks kasutada HTTP.

Lõpuks kõne on **Loo\_mudel\_EKSEMPLARI** funktsioon. Pange tähele, et **ilmajaama** nimeruumi mudeli ja **ContosoAnemometer** on mudeli nimi. Kui mudel eksemplar on loodud, saate alustada sõnumite sündmuste saatmine ja vastuvõtmine. Siiski on oluline, et aru saada, milliseid mudel.

### <a name="defining-the-model"></a>Kui seate mudeli määratlemine

Mudeli **serializer** teegis määratleb sündmusi, mis seadme saavad saata asjade jaoturi ja vastavaid sõnumeid, nimega *toimingud* modelleerimine keel, mille vastu võtta. Määratlete mudeli kasutamine C makrod nimega hulk on **simplesample\_amqp** proovi taotluse:

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

Funktsiooni **alustada\_NIMERUUM** ja **END\_NIMERUUM** makrode nii võtta nimeruumi mudeli argumendina. Eeldatakse, et midagi makrode vahel on määratluse mudelid kasutavate andmestruktuurid ja teie geoloogilises.

Selles näites on ühe mudeli nimega **ContosoAnemometer**. See mudel määratleb sündmustest, mida seadme saavad saata asjade jaoturi: **DeviceId** ja **tuulekiirus**. Samuti määratletakse kolme toimingud (sõnumid) saab seadme: **TurnFanOn**, **TurnFanOff**ja **SetAirResistance**. Iga sündmuse tüüp on ja iga toimingu on nimi (ja soovi korral parameetrite kogumi).

Sündmuste ja toiminguid, mis on määratletud mudeli määratleda API pind, mille abil saate saata sündmuste asjade jaoturi, samuti saadetakse seadme-sõnumitele vastamine. See on kõige paremini mõista kaudu näiteks.

### <a name="sending-events"></a>Sündmuste saatmine

Kui seate mudeli määratleb sündmuste kohta asjade jaoturi saate saata. Selles näites, mis tähendab, et üks järgmistest sündmustest, mis on määratletud **WITH_DATA** makro abil. Näiteks, kui soovite saada **tuulekiirus** sündmuse asjade jaoturiga, on paar toimingut tehes see juhtub. Esimene on määrata soovime saata.

```
myWeather->WindSpeed = 15;
```

Meil varem määratletud mudeli võimaldab meil seda teha on **kirjel**liige. Järgmiseks me serialiseerida soovime saata sündmuse:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Järgmine kood serializes sündmuse puhvrit (viidatud **sihtkoht**). Lõpuks saadame sündmuse asjade jaoturi abil järgmine kood:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

See on valimi rakendus, mis saadab meie sarjadesse jaotatud sündmuse asjade jaoturi abimees funktsiooni:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Järgmine kood on väga sarnane, mida me kuvati, klõpsake soovitud **iothub\_kliendi\_valimi\_amqp** rakenduse, kus loodud sõnumi baitide massiivis ja seejärel kasutatud **IoTHubClient\_SendEventAsync** asjade jaoturi saatmiseks. Pärast seda me lihtsalt on sõnumi pidet vabastamiseks ja seeriasertide andmete puhvri me eraldatud varasemas versioonis.

Teine viimase parameeter **IoTHubClient\_SendEventAsync** on viide tagasihelistamise funktsioon, mida kutsutakse, kui andmed on edukalt saadetud. Siin on näide tagasihelistamise funktsiooni:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Teine parameeter on viit kasutaja kontekstis; sama kursori me edasi **IoTHubClient\_SendEventAsync**. Sel juhul kontekstis on lihtne vastuolus, kuid see võib olla midagi, mida soovite.

See on kõik sündmused saatmist on. Ainus vasakule katmiseks on sõnumeid, tehke järgmist.

### <a name="receiving-messages"></a>Sõnumite vastuvõtmine

Töötab samamoodi nagu sõnumitele tööta **IoTHubClient** teegis sõnumi vastu. Esmalt registreerimist sõnumi tagasihelistamise funktsiooni:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Seejärel kirjutage tagasihelistamise funktsiooni, kui teate, mis on saanud:

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

Järgmine kood on trafarett – see on sama mis tahes lahendus. Seda funktsiooni saab sõnumi ja hoolitseb marsruutimiseks vastava funktsiooni – kõne edastamiseks **käivitamine\_KÄSK**. Selles etapis sõltub nimetatakse funktsiooni määratlus on meie mudel toimingud.

Kui määratlete toimingu mudelist, palutakse teil rakendada funktsioon, mida nimetatakse, kui teie seade saab vastav teade. Näiteks kui mudelisse määratleb toimingut:

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

Pange tähele, et funktsiooni nimi vastab toimingu mudeli nimi ja et funktsiooni parameetrid vastavad määratud toimingu parameetrid. Esimene parameeter on alati vaja ja see sisaldab viit eksemplari meie mudel.

Kui seade saab sõnumi, mis vastab teie allkirja, nimetatakse vastava funktsiooni. Seetõttu peale võttes trafarett kood **IoTHubMessage**lisada, sõnumeid on vaid paari lihtsa funktsiooni iga toimingu määratletud mudelist määratlemine.

### <a name="uninitializing-the-library"></a>Uninitializing teek

Kui olete lõpetanud andmete saatmise ja vastuvõtmise sõnumeid, te saate lähtestamise asjade teegi:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Kõik need kolm funktsiooni joondatakse eespool kirjeldatud kolme lähtestamine funktsioonidega. Helistamiseks neid API-de tagab, et saate varem eraldatud.

## <a name="next-steps"></a>Järgmised sammud

See artikkel hõlmab **Azure'i asjade seadme SDK c**teekide kasutamise põhialused. See annab teile piisavalt teavet mõista Tarkvaraarenduskomplektist, mis sisaldab selle arhitektuur ja kuidas alustada Windows näidised töötamine. Järgmise artikkel jätkab SDK kirjeldus, mis selgitab [IoTHubClient teegi kohta rohkem](iot-hub-device-sdk-c-iothubclient.md).

Asjade jaoturi jaoks arendamise kohta leiate lisateavet teemast [Asjade jaoturi SDK-d][lnk-sdks].

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
