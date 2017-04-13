<properties
    pageTitle="Soovitud asjade jaoturi PowerShelli ja Azure ressursihaldur malli abil saate luua | Microsoft Azure'i"
    description="Järgige selle õpetuse soovitud asjade keskuse loomiseks PowerShelliga Azure'i ressursihaldur mallide kasutamise alustamine."
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
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Soovitud asjade jaoturi PowerShelli kaudu loomine

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Sissejuhatus

Saate luua ja hallata Azure asjade jaoturi programmiliselt Azure'i ressursihaldur. Selle õpetuse näidatakse, kuidas luua ka asjade jaoturi PowerShelliga Azure'i ressursihaldur malli kasutamiseks.

> [AZURE.NOTE] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [Azure'i ressursihaldur ja klassikaline](../resource-manager-deployment-model.md).  Selles artiklis antakse ülevaade Azure'i ressursihaldur juurutamise mudeli abil.

Selle õpetuse lõpuleviimiseks vajate järgmist:

- Aktiivne Azure'i konto. <br/>Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.
- [Microsoft Azure'i PowerShelli 1.0] [ lnk-powershell-install] või uuem versioon.

> [AZURE.TIP] Artikli [Azure PowerShelli kasutamine Azure ressursihaldur] [ lnk-powershell-arm] abil saate rohkem teavet selle kohta, kuidas luua Azure ressursse PowerShelli skriptide ja Azure ressursihaldur mallide abil. 

## <a name="connect-to-your-azure-subscription"></a>Ühenduse loomine Azure tellimuse

Sisestage PowerShelli käsuviiba Azure tellimuse sisse logida järgmine käsk:

```
Login-AzureRmAccount
```

Järgmised käsud abil saate tuvastada, kus saate juurutada soovitud asjade jaoturi ja praegu toetatud API versioone:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Looge ressursirühma sisaldama teie asjade jaoturi järgmine käsk kasutamine asjade jaoturi toetatud asukohad. Selles näites loob nimetatakse **MyIoTRG1**ressursirühma.

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Esitage mõni Azure ressursihaldur mall on asjade keskuse loomiseks

JSON malli abil saate luua ka asjade jaoturi ressursirühma. Mõni Azure ressursihaldur mall abil saate muuta olemasolevaid asjade jaoturiga.

1. Kasutage tekstiredaktorit Azure'i ressursihaldur nimetatakse **template.json** järgmised ressursside määratlemine uue standard asjade keskuse loomiseks malli loomiseks. Selles näites lisab asjade jaoturi piirkonnas **Ida-USA** , loob kaks rühmi (**cg1** ja **cg2**) sündmuse jaoturi ühilduv lõpp-punkti ja kasutab **2016-02-03** API versioon. Selle malli loodab ka asjade jaoturi nimi parameetrina nimega **hubName**edasi. Asukohad, mis toetavad asjade jaoturi praeguse loendi leiate [Azure'i oleku][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Azure'i ressursihaldur malli teie kohalikus arvutis faili salvestada. Selles näites eeldab salvestate selle kausta nimega **c:\templates**.

3. Käivitage järgmine käsk juurutamiseks oma uue asjade jaoturi, läbides nimi oma asjade jaoturi parameetrina. Selles näites on asjade jaoturi nime **abcmyiothub** (Pange tähele, et see nimi peab olema globaalselt kordumatu nii, et see sisaldaks oma nimi või nimetähed).

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Väljund kuvatakse loodud asjade jaoturi klahvid.

5. Saate kontrollida, et rakenduse lisatud uus asjade jaoturi külastades [Azure portaali] [ lnk-azure-portal] ja ressursside või **Get-AzureRmResource** PowerShelli cmdleti abil loendi vaatamine.

> [AZURE.NOTE] Selles näites rakenduse lisab mõne S1 Standard asjade jaoturi jaoks, mis on arve. Saate kustutada asjade jaoturi [Azure portaali] kaudu[ lnk-azure-portal] või **Eemaldamine – AzureRmResource** PowerShelli cmdleti abil, kui olete lõpetanud.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete juurutanud asjade jaoturi, mis on Azure ressursihaldur malli abil PowerShelliga, võite täpsemaks analüüsimiseks:

- [Asjade jaoturi ressursi pakkuja REST API]võimaluste kohta vt[lnk-rest-api].
- Lugege [Azure'i ressursihaldur ülevaade] [ lnk-azure-rm-overview] Lisateavet Azure ressursihaldur võimaluste kohta.

Asjade jaoturi jaoks arendamise kohta leiate lisateavet teemast järgmist:

- [C SDK tutvustus][lnk-c-sdk]
- [Asjade jaoturi SDK-d][lnk-sdks]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
