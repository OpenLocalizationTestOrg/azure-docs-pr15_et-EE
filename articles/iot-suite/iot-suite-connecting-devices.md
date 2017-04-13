<properties
   pageTitle="Ühendage seade C kasutamine Windows | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse seadmega ühendada Azure'i asjade komplekti eelkonfigureeritud remote jälgimise lahendus rakendus töötab Windows c abil."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Ühendage seade remote jälgimise eelkonfigureeritud lahendus (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Opsüsteemis Windows C valimi lahenduse loomine

Järgmised sammud näitavad, kuidas luua kliendi rakendus kirjutatud C, mis suhtleb jälgida eelkonfigureeritud lahenduse Visual Studio abil.

Visual Studio 2015 starter projekti loomine ja lisamine asjade jaoturi seadme kliendi Nugeti pakettide.

1. Looge Visual Studio 2015 C konsooli rakendus Visual C++ **Win32 konsooli rakendus** malli abil. Projekti **RMDevice**nimi.

2. **Win32-rakendus viisardi**lehel **Rakenduste sätted** veenduge, et **konsooli rakendus** on valitud, ja tühjendage ruut **Precompiled päise** - ja **Turvalisus arengu elutsükli (SDL) kontrollib**.

3. Kustuta failid stdafx.h, targetver.h ja stdafx.cpp **Solution Exploreris**.

4. Nimetage fail RMDevice.cpp **Solution Exploreris**RMDevice.c.

5. **Solution Exploreris** **RMDevice** projekti paremklõpsake ja seejärel klõpsake **haldamine NuGet-paketid**. Klõpsake nuppu **Sirvi**, siis otsida ja installida järgmised Nugeti paketid projekti:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. **Solution Exploreris** **RMDevice** projekti paremklõpsake ja seejärel klõpsake projekti **Atribuudi lehtede** dialoogiboks **Atribuudid** . Lisateavet leiate teemast [Visual C++ projekti atribuutide seadmine][lnk-c-project-properties]. 

7. Klõpsake **Linker** kausta ja seejärel nuppu atribuudileht **sisestatud** .

8. Saate lisada **Täiendavad sõltuvused** atribuut **crypt32.lib** . Salvestamiseks klõpsake nuppu **OK** ja seejärel **OK** uuesti projekti atribuudiväärtused.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Saate määrata käitumise asjade jaoturi seade

Asjade jaoturi kliendi teekide abil saate määrata seade saadab asjade jaoturi ja ta saab asjade jaoturi käske sõnumite vormingu mudel.

1. Visual Studio, avage RMDevice.c fail. Olemasoleva `#include` laused järgmine kood:

    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Lisage järgmised muutuv deklaratsiooni pärast selle `#include` laused. Asendage kohatäidet [seadme Id] ja [seadme klahv] seadme remote jälgimisega seotud lahenduse armatuurlaualt väärtustega. Kasutage asjade jaoturi Hostname armatuurlaualt asendamiseks [IoTHub nimi]. Näiteks kui teie asjade jaoturi Hostname on **contoso.azure-devices.net**, asendage [IoTHub nimi] **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Lisage järgmine kood määratleda mudelit, mis lubab suhelda asjade jaoturi seade. Selle mudeli saate määrata, et seade saadab temperatuur, temperatuur, niiskus ja seadme id telemeetria. Seadme saadab metaandmete seadme kohta asjade jaoturi, sh Käsuloend, mis toetab seade. See seade ei vasta käske **SetTemperature** ja **SetHumidity**:

    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);

    DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
    );

    DECLARE_STRUCT(DeviceProperties,
    ascii_char_ptr, DeviceID,
    _Bool, HubEnabledState
    );

    DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
    );

    END_NAMESPACE(Contoso);
    ```

## <a name="implement-the-behavior-of-the-device"></a>Rakendada seadme käitumist

Nüüd lisage kood, mis rakendab mudelis määratletud käitumist.

1. Lisage järgmised funktsioonid, mis käivitada, kui seade saab käske **SetTemperature** ja **SetHumidity** asjade keskuse kaudu.

    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }

    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```

2. Järgmised funktsiooni, mis saadab sõnumi asjade jaoturi lisamiseks tehke järgmist.

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
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
    }
    ```

3. Järgmised funktsiooni konksud sariväljaanne teegi SDK lisamiseks tehke järgmist.

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

4. Lisada järgmised funktsiooni ühenduse asjade jaoturi, saata ja vastu võtta sõnumeid, keskuse kaudu ühenduse katkestada. Pange tähele, kuidas seade saadab metaandmeid, sh käsud, mis toetab asjade jaoturi ühenduse loomisel. Selle metaandmete võimaldab värskendada oleku seade **töötab** armatuurlaual lahendus.

    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;

        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;

            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {

              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;

              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                }

                STRING_delete(commandsMetadata);
              }

              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;

              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;

                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }

                ThreadAPI_Sleep(1000);
              }
            }

            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
    
    Viide, on valimi **DeviceInfo** sõnum saadetakse asjade jaoturi käivitamisel:

    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
    
    Viite siit saadetud asjade jaoturi valimi **telemeetria** sõnum.

    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
    
    Viide, siin on näide asjade jaoturi saadud **käsk** :
    
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```

5. Asendage **põhifunktsiooni** kasutada funktsiooni **remote_monitoring_run** järgmine kood:

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Klõpsake **koostamine** ja seejärel **Lahenduse luua** koostamiseks seadmes rakendus.

7. **Solution Exploreris**, paremklõpsake **RMDevice** projekt, klõpsake **silumine**ja klõpsake nuppu Proovi käivitamiseks **uue seansi käivitamine** . Konsooli kuvab sõnumite rakenduse asjade jaoturi valimi telemeetria saadab ja võtab vastu käsud.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx