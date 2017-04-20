## <a name="typical-output"></a>Tüüpiline väljund

Allpool on toodud näide logifaili Hello World proovi kirjalik väljund. Newline ja kaart on lisatud loetavuse:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Koodilõigud

Siin sektsioonis räägitakse Hello World proovi koodi oluline osa.

### <a name="gateway-creation"></a>Lüüsi loomine

Arendaja kirjutama *gateway protsessi*. See programm loob sisemise infrastruktuuri (ta), laadib moodulite ja seab kõik korralikult. SDK pakub **Gateway_Create_From_JSON** funktsiooni, et saaksite bootstrap värav JSON faili. **Gateway_Create_From_JSON** kasutamiseks peate läbima see tee JSON faili, mis määrab moodulite laadimiseks. 

Ei leia koodi gateway protsessi Hello World valimisse kaasatud [main.c] [ lnk-main-c] faili. Allpool väljavõte näitab loetavust, lühendatud versiooni gateway protsessi kood. See programm loob lüüs ja siis ootab kasutaja **ENTER** klahvi vajutada, enne värav ette pisarad. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

JSON sätete fail sisaldab loetelu moodulite laadimiseks. Iga mooduli peate määrama a:

- **module_name**: moodul kordumatu nimi.
- **module_path**: Raamatukogu, mis sisaldab mooduli tee. Linux on nii et faili, Windows on .dll faili.
- **args**: moodul peab mis tahes konfiguratsiooniteave.

JSON-fail sisaldab ka moodulid, mis kandub ta seoseid. Link on kaks omadused:
- **Allikas**: mooduli nimi: selle `modules` lõik, või "\*".
- **valamu**: mooduli nimi: selle `modules` lõik

Iga link määratleb sõnumi marsruudi ja suunas. Sõnumid moodul `source` tarnitakse moodulisse `sink`. Et `source` võib olla seatud "\*", mis näitab, et sõnumid iga moodul on edastatud `sink`.

Järgmine näide kirjeldab JSON sättefaili Hello World proovi seadistada Linux. Iga sõnumi toodetud moodul `hello_world` moodul tarbitakse `logger`. Kas on vajalik moodul argument sõltub mooduli kujundus. Selles näites puuraidur moodul kasutab argumenti, mis on väljund faili tee ja Hello World moodul ei võta väiteid:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Tere maailma moodul sõnumi kirjastamine

Leidmiseks kasutada "hello world" moodul ["hello_world.c"] sõnumite avaldamiseks koodi[ lnk-helloworld-c] faili. Allpool väljavõte kuvatakse muudetud versiooni täiendavaid kommentaare ja mõned tõrketöötluse koodi loetavuse eemaldada:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Tere maailma moodul sõnumi töötlemisel

Hello World moodul kunagi peab töötlema sisalduv ta avaldada teiste moodulitega. Seetõttu rakendamine sõnumi tagasikutse Hello World moodul nr-op funktsioon.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Puuraidur moodul sõnumi avaldamiseks ja töötlemiseks

Puuraidur moodul saab sõnumeid ta ja kirjutab neid faili. See avaldab kunagi ühtegi sõnumit. Seetõttu puuraidur mooduli kood kunagi nõuab **Broker_Publish** funktsioon.

**Logger_Recieve** funktsioon [logger.c] [ lnk-logger-c] faili on maakler tugineb sõnumite kohaletoimetamiseks puuraidur moodul. Allpool väljavõte kuvatakse muudetud versiooni täiendavaid kommentaare ja mõned tõrketöötluse koodi loetavuse eemaldada:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Järgmised sammud

Asjade Interneti Gateway SDK kasutamise kohta leiate jaotisest järgmisega:

- [Asjade Interneti Gateway SDK-seadme pilve sõnumeid saata Linuxi, simuleeritud seadmega][lnk-gateway-simulated].
- [Azure'i asjade Interneti Gateway SDK] [ lnk-gateway-sdk] github.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md