<properties
    pageTitle="Soovitud asjade jaoturi abil Azure'i CLI loomine | Microsoft Azure'i"
    description="Järgige selles artiklis on asjade jaoturi abil Azure'i käsurea liides loomiseks."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Soovitud asjade jaoturi abil Azure'i CLI loomine

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Sissejuhatus

Saate luua ja hallata Azure asjade jaoturi programmiliselt Azure'i käsurea liides. Selles artiklis kirjeldatakse, kuidas Azure'i CLI luua ka asjade jaoturi abil.

Selle õpetuse lõpuleviimiseks vajate järgmist:

- Aktiivne Azure'i konto. Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.
- [Azure'i CLI 0.10.4] [ lnk-CLI-install] või uuem versioon. Kui teil on juba Azure'i CLI saate kinnitada praegune versioon käsuviibale järgmine käsk:
```
    azure --version
```

> [AZURE.NOTE] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [Azure'i ressursihaldur ja klassikaline](../resource-manager-deployment-model.md). Azure'i CLI peab olema Azure'i ressursihaldur režiimis:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Saate häälestada Azure'i konto ja tellimuse 

1. Tippige järgmine käsk Käsuviip login
```
    azure login
```
Kasutage autentimiseks soovitatud veebibrauseri ja koodi.

2. Kui teil on mitu Azure tellimust, andke ühenduse Azure'i juurdepääs mandaadiga seotud kõigi Azure'i tellimused. Saate vaadata Azure'i tellimused, samuti millest üks on vaikimisi, kasutades käsku
```
    azure account list 
```

Seadmiseks tellimuse konteksti, mille soovite käivitada ülejäänud käskude kasutamine

```
    azure account set <subscription name>
```

3. Kui teil on, saate luua ühe nimega **exampleResourceGroup** ressursirühma 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] [Azure'i CLI Azure ressursid ja ressursside rühmade haldamiseks kasutada] artikli[ lnk-CLI-arm] leiate Azure'i CLI abil saate hallata Azure ressursside kohta lisateabe saamiseks. 


## <a name="create-an-iot-hub"></a>Soovitud asjade keskuse loomine

Nõutavad parameetrid:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Kõik parameetrid kuvamiseks saadaval loomiseks saate Spikker käsk Käsuviip
```
    azure iothub create -h 
```
Lihtne näide:

 Soovitud asjade keskuse loomiseks nimetatakse **exampleIoTHubName** ressursi rühma **exampleResourceGroup** lihtsalt käivitage järgmine käsk
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] See Azure CLI käsk loob mõne S1 Standard asjade jaoturi jaoks, mis on arve. Saate kustutada asjade jaoturi **exampleIoTHubName** järgmine käsk abil 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Järgmised sammud
Asjade jaoturi jaoks arendamise kohta leiate lisateavet teemast järgmist:
- [Asjade jaoturi SDK-d][lnk-sdks]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Azure'i portaal haldamine asjade jaoturi abil][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
