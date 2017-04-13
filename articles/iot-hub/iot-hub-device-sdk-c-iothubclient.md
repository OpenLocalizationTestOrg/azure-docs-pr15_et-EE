<properties
    pageTitle="Azure'i asjade seadme SDK C - IoTHubClient | Microsoft Azure'i"
    description="Lisateavet C Azure'i asjade seadmes SDK IoTHubClient teegi kasutamine"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure'i asjade seadme SDK C – Lisateavet leiate teemast IoTHubClient

[Esimene artikkel](iot-hub-device-sdk-c-intro.md) selle sarja kasutusele **Microsoft Azure'i asjade seadme SDK c**. Selles artiklis selgitatakse SDK on kaks arhitektuuri kihti. Funktsiooni Base on suhtlemine asjade jaoturi otse haldav **IoTHubClient** teek. Olemas on ka **serializer** teek, mis koostab peal, et sariväljaanne osutamiseks. Selles artiklis anname täiendavad üksikasjad **IoTHubClient** teek.

Eelmise artiklis kirjeldatud, kuidas kasutada **IoTHubClient** teegi sõnumite asjade jaoturi sündmuste saatmiseks ja vastuvõtmiseks. Selles artiklis selgitatakse, kuidas saate täpsemalt hallata, *kui* saate saata ja vastu võtta andmeid, tutvustame teile **madalama taseme API-de**laiendab selle arutelu. Saate ka selgitab, kuidas lisada atribuudid sündmused (ja sõnumite toomiseks) atribuudi töötlemise **IoTHubClient** teegi funktsioonide abil. Lõpuks pakume täiendavad selgitus võimalust sõnumit saanud asjade keskuse kaudu.

Selle artikli lõpus on paar mitmesugused teemad, sh seadme identimisteabe ja kuidas muuta **IoTHubClient** konfiguratsiooni suvandid kaudu toimimist kohta rohkem kataks.

Kasutame **IoTHubClient** SDK näidised, et selgitada järgmisi teemasid. Kui soovite jälgida, leiate selle **iothub\_kliendi\_valimi\_http** ja **iothub\_kliendi\_valimi\_amqp** rakendusi, mis sisalduvad Azure asjade seadme SDK jaoks C. kõik kirjeldatud järgmistes jaotistes on näidanud nende proovide.

Saate leiate **Azure'i asjade seadme SDK c** [Microsoft Azure'i asjade SDK-d](https://github.com/Azure/azure-iot-sdks) GitHub hoidla ja [C API viite](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)API üksikasjade kuvamine.

## <a name="the-lower-level-apis"></a>Madalama taseme API-d

Eelmise artiklis kirjeldatud põhilised toimimise **IotHubClient** kontekstis on **iothub\_kliendi\_valimi\_amqp** rakendus. Näiteks see selgitati, kuidas lähtestada teegi abil järgmine kood.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

See ka kirjeldatud, kuidas saata sündmuste see funktsioon kõne abil.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Artiklis kirjeldatud ka registreerumine funktsiooni tagasihelistamise sõnumeid, tehke järgmist.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

See artikkel ilmnes ka Kuidas tasuta kasutades koodi näiteks järgmisi ressursse.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Samas on iga nende API-de companion funktsioonid.

-   IoTHubClient\_ll:\_CreateFromConnectionString

-   IoTHubClient\_ll:\_SendEventAsync

-   IoTHubClient\_ll:\_SetMessageCallback

-   IoTHubClient\_LL\_hävitada


Kõigi ülesannete hulka "LL:" API nimi. Muud kui, et parameetrid iga need funktsioonid on samad nende-LL kolleegidega. Siiski nende funktsioonide käitumine erineb üks oluline viis.

Kui helistate **IoTHubClient\_CreateFromConnectionString**, aluseks teekide luua uus teema, mis töötab taustal. Selles teemas sündmusi saadab ja võtab vastu sõnumeid asjade keskuse kaudu. Pole sellist teemat luuakse "LL" API-de töötamisel. Arendaja mugavuse huvides on teema tausta loomine. Te ei pea muretsema konkreetselt sündmuste sõnumite saatmine ja vastuvõtmine asjade jaoturis – see juhtub automaatselt taustal. Seevastu ll ":" API-de teile konkreetsete juhtida asjade keskuses suhtlemine Kui teil on vaja.

Seda paremini mõista vaatame näide:

Kui helistate **IoTHubClient\_SendEventAsync**, mida te tegelikult teete paneb sündmuse puhvrit. Tausta teemat loodud helistamisel **IoTHubClient\_CreateFromConnectionString** pidevalt jälgib seda puhvrit ja saadab andmed, mida see sisaldab asjade jaoturi. See juhtub tausta, et peamine teema on tööde teiste samal ajal.

Samamoodi kui registreerimist tagasihelistamise funktsiooni abil sõnumite **IoTHubClient\_SetMessageCallback**, te olete juhendamine SDK olema tausta autonoomsest tagasihelistamise funktsiooni, kui sõltumatu peamine teema, vastuvõetud sõnumi teema.

"Kõik" API-d ei loo teemat tausta. Selle asemel tuleb uue API konkreetselt saata ja vastu võtta andmete asjade keskuse kaudu käivitada. See on näidatud järgmises näites.

Funktsiooni **iothub\_kliendi\_valimi\_http** rakendus, mis sisaldab SDK näitab madalama taseme API-d. Selle proovi saadame sündmuste asjade jaoturi koodi näiteks järgmist:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Esimesed kolm looge sõnum ja viimane rida saadab sündmus. Siiski, nagu eelnevalt mainitud, "saatmine" sündmus tähendab, et andmed paigutatakse lihtsalt puhvrit. Midagi on võrgus edastatud, kui me kutsume **IoTHubClient\_ll:\_SendEventAsync**. Selleks, et tegelikult sissepääsu asjade jaoturi andmed, kellele helistate **IoTHubClient\_ll:\_DoWork**, nagu järgmises näites:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Järgmine kood (kaudu soovitud **iothub\_kliendi\_valimi\_http** rakenduse) seni helistab **IoTHubClient\_ll:\_DoWork**. Iga kord, kui **IoTHubClient\_ll:\_DoWork** on nimetatakse, saadab teatud sündmuste puhvri asjade jaoturi ja toob ootel sõnumi saatmist seade. Viimasel juhul tähendab, et kui me registreeritud tagasihelistamise funktsiooni sõnumite jaoks, siis selle tagasihelistamise kutsumisel (eeldades, et kõik sõnumid, järjekorda). Me oleks registreeritud tagasihelistamise funktsiooni koodi näiteks järgmist:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Põhjus mis **IoTHubClient\_ll:\_DoWork** sageli nimetatakse esitatavaks on see, et iga kord, kui seda nimetatakse, saadab *mõned* puhverdatud sündmuste asjade jaoturi ja toob *järgmise* sõnumi ootele soovitud seade. Iga kõne pole tagatud, et saada kõik puhverdatud sündmuste või ootele tuua kõik sõnumid. Kui soovite saada kõik sündmused puhvri ja jätkake muu töötlemine saate asendada selle tsükkel kood, nt mõni järgmistest:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Järgmine kood helistab **IoTHubClient\_ll:\_DoWork** seni, kuni kõik sündmused puhvri saadetud asjade jaoturi. Pange tähele, et see ei tähenda ka, et kõik järjekorras olevad sõnumid on saanud. Osa selle põhjuseks on, et "kõik" sõnumite vaatamine pole nii tarkadeks toimingu. Mis juhtub, kui saate tuua "kõik" sõnumid, kuid seejärel veel üks saadetakse seadmega kohe pärast? Paremini toime tulla, mis on programmeeritud ajalõpp. Näiteks võib funktsiooni sõnumi tagasihelistamise lähtestada on timer iga kord, kui see on vaja järgida. Seejärel saate kirjutada loogika jätkamist, kui näiteks pole sõnumeid on saanud viimase *X* sekundi.

Kui olete lõpetanud ingressing sündmused ja sõnumeid, veenduge, et vastava funktsiooni puhastamiseks ressursid.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Sellise tõrke ilmnemisel on ainult üks komplekt API-de saatmiseks ja vastuvõtmiseks tausta teemat ja veel mõni API-d, mis teeb sama tausta teemat ilma andmeid. Paljud arendajad eelistab LL API, kuid madalama taseme API-d on kasulik, kui arendaja soovib konkreetsete üle võrgu ülekannetes. Näiteks mõne seadme koguda andmeid ja ainult sissepääsu sündmuste määratud intervalliga (nt üks kord tunnis või kord päevas). Madalama taseme API-de korral saate otseselt juhtelemendile kui saata ja vastu võtta andmete asjade keskuse kaudu. Teised lihtsalt eelistab lihtsad, mis pakuvad madalama taseme API-d. Kõik toimub peamine teema, mitte mõne töö toimub taustal.

Sellest, millist mudeli valimist kindlasti ühtlustamist sisse mis API-d, saate kasutada. Kui alustate helistades **IoTHubClient\_ll:\_CreateFromConnectionString**, veenduge, et kasutate ainult vastavate madalama taseme API-de jaoks järeltegevuse töö:

-   IoTHubClient\_ll:\_SendEventAsync

-   IoTHubClient\_ll:\_SetMessageCallback

-   IoTHubClient\_LL\_hävitada

-   IoTHubClient\_ll:\_DoWork

Samuti on tõene vastand. Kui alustate koos **IoTHubClient\_CreateFromConnectionString**, siis kasutage LL API mis tahes täiendavaid töötlemiseks.

Leiate Azure'i asjade seadmes SDK C, on **iothub\_kliendi\_valimi\_http** taotlus täieliku näide madalama taseme API-d. Funktsiooni **iothub\_kliendi\_valimi\_amqp** rakenduse saab viidata täielik näide, on mitte - LL API-de jaoks.

## <a name="property-handling"></a>Atribuut töötlemine

Seni kui saatmise andmete kirjelduse, saame olete viidata, sõnumi kehasse. Võtame näiteks järgmine kood:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Selles näites saadab sõnumi asjade jaoturi teksti "Tere". Siiski lubab asjade jaoturi lisatakse iga sõnumi atribuudid. Atribuudid on nimi ja väärtuse paarideks, sõnumile manustatud. Näiteks me saame muuta eelmise koodi atribuudi sõnumile manustamiseks:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Alustame helistades **IoTHubMessage\_atribuudid** ja saata see sõnum pidet. Me naasta on mõne **kaardi\_toime** lahtriviide, mis võimaldab meil alustada lisamist atribuudid. Viimase saab teha helistades **kaardi\_AddOrUpdate**, mis võtab kaardi viide\_PIDET, atribuudi nimi ja atribuudi väärtust. See API-ga lisame nagu me nii palju atribuudid.

Kui sündmus lugeda **Sündmuse jaoturi**kaudu, vastuvõtja loetlemine atribuutide ja tuua oma vastavad väärtused. Näiteks .NET-is seda soovite saavutada [Atribuudid saidikogumi EventData objektil,](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx)kasutades.

Eelmises näites me ei manustamise atribuudid saadame asjade jaoturi sündmus. Atribuudid võivad olla seotud asjade jaoturi saadud sõnumeid. Kui soovime sõnumi atribuutide toomiseks, saame meie tagasihelistamise funktsioon kasutada näiteks järgmine kood:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Kõne suunamine **IoTHubMessage\_atribuudid** tagastab selle **kaardi\_toime** viide. Võtame siis viite **kaardi\_GetInternals** saada viide massiivi nime ja väärtuse paarideks (kui ka atribuutide arv). Sel hetkel on lihtne küsimus nummerdamine atribuudid avamiseks soovime väärtused.

Teil pole oma rakenduse kasutamise. Juhul, kui vajate pannud sündmusi või sõnumite toomiseks, **IoTHubClient** teegi lihtne.

## <a name="message-handling"></a>Sõnumi töötlemine

Nagu eelnevalt, kui sõnumid saabuvad asjade keskuse kaudu vastab **IoTHubClient** teegi tuginedes registreeritud tagasihelistamise funktsiooni. On selle funktsiooni, et mõned täiendavad selgitused väärib saatja jaotusparameeter. Siin on funktsiooni tagasihelistamise väljavõte soovitud **iothub\_kliendi\_valimi\_http** proovi taotluse:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Pange tähele, et on tagastuse tüüp **IOTHUBMESSAGE\_likvideerimise\_TULEMI** ja käesoleval juhul me tagasi **IOTHUBMESSAGE\_aktsepteeritud**. On meil saate tulu see funktsioon väärtustele, mida muuta, kuidas **IoTHubClient** teegi reageerib sõnumi tagasihelistamise. Siin on võimalusi.

-   **IOTHUBMESSAGE\_aktsepteeritud** – sõnum on edukalt saadetud. Teegi **IoTHubClient** käivitub pole funktsiooni tagasihelistamise sama sõnumiga.

-   **IOTHUBMESSAGE\_tagasi lükatud** – sõnumit ei töödeldud ja ei soovita seda teha tulevikus. Teegi **IoTHubClient** peaks ei kasuta sama sõnumiga tagasihelistamise funktsiooni.

-   **IOTHUBMESSAGE\_hüljatud** – sõnumit ei töötlemine õnnestus, kuid **IoTHubClient** teegi peaks autonoomsest funktsiooni tagasihelistamise sama sõnumiga.

Kaks esimest saatja koode, **IoTHubClient** teegi saadab sõnumi asjade jaoturi, mis näitab, et sõnumi kustutamiseks seadme kuhjuda ja ei toimetatud kohale uuesti. Mõju on sama (sõnum on kustutatud seadme järjekorda), kuid kas sõnum on aktsepteeritud või hüljatud on endiselt salvestatud.  Salvestamise eristamine on kasulik sõnumi saatjate, kes saab tagasisidet kuulata ja teada saada, kui seade on aktsepteeritud või hüljatud kindla sõnumi.

Viimases sõnum saadetakse ka asjade jaoturi, kuid see näitab, et sõnum tuleks taastarnimiseni. Tavaliselt kuvatakse sõnumi loobuda, kui ilmneb mõni tõrge, kuid soovite proovida töödelda sõnum uuesti. Seevastu tagasilükkamise sõnumi on asjakohane, kui teil ilmneb tõrge parandamatu (või kui otsustate lihtsalt te ei soovi sõnumit töödelda).

Igal juhul arvestage eri saatja koodi nii, et soovite teegist **IoTHubClient** käitumise saab panna.

## <a name="alternate-device-credentials"></a>Alternatiivsete seadme identimisteave

Nagu varem, esimene asi, mida teha, kui **IoTHubClient** teegis on saada vastuseks **IOTHUB\_kliendi\_toime** kõne näiteks järgmine:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Argumendid **IoTHubClient\_CreateFromConnectionString** on meie seade ja parameeter, mis näitab kasutame suhelda asjade jaoturi protokoll ühendusstring. Ühendusstringi on vorming, mis kuvatakse järgmiselt:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

On nelja liiki selle stringi andmed: asjade jaoturi nimi, asjade jaoturi järelliite, seadme ID ja ühiskasutusega juurdepääsu võti. Teil saada täielik domeeninimi (FQDN), on asjade jaoturi Azure'i portaalis oma asjade jaoturi eksemplari loomisel – see annab teile asjade jaoturi nimi (esimene osa FQDN) ja asjade jaoturi järelliide (ülejäänud FQDN). Saate seadme ID-d ja kiirklahv ühiskasutusse, kui registreerute seadme asjade jaoturi ( [eelmise artiklis](iot-hub-device-sdk-c-intro.md)kirjeldatud).

**IoTHubClient\_CreateFromConnectionString** annab teile üks viis, kuidas lähtestada teek. Soovi korral saate luua uue **IOTHUB\_kliendi\_toime** , kasutades neid üksikute parameetrite asemel ühendusstring. See on saavutada järgmine kood:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

See millega sama mis **IoTHubClient\_CreateFromConnectionString**.

See võib tunduda selge, et mida soovite kasutada **IoTHubClient\_CreateFromConnectionString** selle asemel, et see veel Paljusõnaline meetod lähtestamine. Pidage meeles, kui registreerute seadme asjade keskuses mis teil on seadme ID ja seadme võtit (mitte ühendusstringi). [Eelmine artikkel](iot-hub-device-sdk-c-intro.md) **Seadmehaldur** SDK tööriist kasutab teekide **Azure'i asjade teenuse SDK** luua ühendusstring seadme ID, seadme võtit ja asjade jaoturi hosti nimi. Nii helistamiseks **IoTHubClient\_LL\_Loo** võib olla soovitatav, kuna see säästab ühendusstringi loomise etappi. Kasutada sõltumata sellest, millist on mugav.

## <a name="configuration-options"></a>Konfiguratsiooni suvandid

Siiani kõik kirjeldatud kohta, kuidas **IoTHubClient** teegi toimib kajastab selle vaikekäitumise. Siiski on mitu võimalust, mida saate seada teegi töötamise viisi muutmine. See on võimalik kasutamine on **IoTHubClient\_LL\_SetOption** API. Selles näites vaatame.

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

On paar võimalust, mis on tavaliselt kasutatakse.

-   **SetBatching** (kahendmuutuja) – kui **true**, siis andmete saadetud asjade jaoturi saadetakse pakettidena. Kui **false**, siis sõnumid saadetakse ükshaaval. Vaikimisi on **false**. Pange tähele, et **SetBatching** võimalus kehtib ainult HTTP-protokolli, mitte MQTT või AMQP protokollid.

-   **Ajalõpp** (allkirjastamata int) – see väärtus on esitatud millisekundites. Kui HTTP-päring või vastuse vastu võtab rohkem kui seekord, siis ühenduse ajalõpp saatmine.

Partiide valik on oluline. Vaikimisi teegi ingresses sündmuste eraldi (ühe sündmuse on mis tahes te kaotate **IoTHubClient\_ll:\_SendEventAsync**). Kui partiide valik on **tõene**, kogub teek nii palju sündmusi, sest see on võimalik puhvri (kuni asjade jaoturi aktsepteerib sõnumi maksimaalse mahu).  Sündmuse pakett saadetakse asjade jaoturi ühe HTTP kõne (üksikute sündmuste kogumisse JSON massiivi sisse). Lubada partiide tavaliselt põhjustab suur jõudluse tõstmiseks Kuna te olete vähendada võrgu vormimallikujundaja. See vähendab läbilaskevõime Kuna saadate HTTP päised abil soovitud sündmus paketi kogumit, mitte iga üksiku sündmuse päiste kogum. Kui teil on teatud põhjus teisiti teha, tavaliselt peaksite partiide lubamiseks.

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis kirjeldatakse üksikasjalikult leitud **Azure'i asjade seadme SDK c** **IoTHubClient** teegi toimimist. Selle teabe, peaks olema **IoTHubClient** teegi võimaluste hea mõistmine. [Järgmine artikkel](iot-hub-device-sdk-c-serializer.md) annab sarnase üksikasjalike **serializer** teek.

Asjade jaoturi jaoks arendamise kohta leiate lisateavet teemast [Asjade jaoturi SDK-d][lnk-sdks].

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
