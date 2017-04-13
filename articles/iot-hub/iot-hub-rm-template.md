<properties
    pageTitle="Soovitud asjade jaoturi ARM Mall ja C# abil luua | Microsoft Azure'i"
    description="Järgige selle õpetuse C# programmi soovitud asjade keskuse loomiseks Azure'i ressursihaldur mallide kasutamise alustamine."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Looge asjade jaoturi, mis on Azure ressursihaldur malliga C# programmi abil

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Sissejuhatus

Saate luua ja hallata Azure asjade jaoturi programmiliselt Azure'i ressursihaldur. Selle õpetuse näidatakse, kuidas Azure'i ressursihaldur malli loomiseks soovitud asjade jaoturi C# programmi kasutamiseks.

> [AZURE.NOTE] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [Azure'i ressursihaldur ja klassikaline](../resource-manager-deployment-model.md).  Selles artiklis antakse ülevaade Azure'i ressursihaldur juurutamise mudeli abil.

Selle õpetuse tegemiseks vajate järgmist:

- Microsoft Visual Studio 2015.
- Aktiivne Azure'i konto. <br/>Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.
- [Azure Storage konto] [ lnk-storage-account] kus saate talletada oma Azure'i ressursihaldur mallifailid.
- [Microsoft Azure'i PowerShelli 1.0] [ lnk-powershell-install] või uuem versioon.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projekti ettevalmistamine

1. Visual Studio Visual C# Windows projekti **Konsooli rakendus** projekti malli abil loomine. Projekti **CreateIoTHub**nimi.

2. Solution Exploreris projektiga paremklõpsake ja valige **Halda NuGet-paketid**.

3. Nugeti Package Manager, märkige ruut **kaasa väljalaske-eelne**ja otsida **Microsoft.Azure.Management.ResourceManager**. **Installige**, **Vaata muudatused** nuppu **OK**, klõpsake käsku **nõustun** litsentside kinnitamiseks.

4. Nugeti Package Manager otsida **Microsoft.IdentityModel.Clients.ActiveDirectory**.  **Installida**, nuppu **Vaata muudatused läbi** **OK**klõpsake **nõustun** litsentsi kinnitamiseks.

5. Program.cs, asendage olemasolev **abil** laused järgmist:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Program.cs, lisage järgmised staatilised muutujad kohatäite väärtuste asendamine. Tegite selles õpetuses märkme **ApplicationId**, **SubscriptionId**, **TenantId**ja **parool** . **Teie Azure'i Salvestusruumikonto nimi** on Azure Storage konto nimi, kus saate talletada Azure'i ressursihaldur malli failid. **Ressursi rühma nimi** on nimi, ressursirühma asjade jaoturi loomisel saate kasutada, võib olla olemasoleva ressursirühm või uus. **Juurutamise nimi** on nimi, nt **Deployment_01**juurutamise.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Esitage mõni Azure ressursihaldur mall on asjade keskuse loomiseks

JSON Mall ja parameetri faili abil luua ka asjade jaoturi ressursirühma. Mõni Azure ressursihaldur mall abil saate muuta olemasolevaid asjade jaoturiga.

1. Solution Exploreris projekti paremklõpsake, klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Uus üksus**. Lisage JSON fail nimega **template.json** projekti.

2. Asendage **template.json** sisu järgmised ressursside määratlemine standard asjade jaoturi lisamiseks **Ida-USA** piirkonnas (regioonid, mis toetavad asjade jaoturi praeguse loendi leiate [Azure'i oleku][lnk-status]):

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

3. Solution Exploreris projekti paremklõpsake, klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Uus üksus**. Lisage JSON fail nimega **parameters.json** projekti.

4. Asendage **parameters.json** sisu parameetri järgmine teave, mis määrab nimi uue asjade jaoturi nagu **{initsiaalid} mynewiothub**. Arvestage, et asjade jaoturi nimi peab olema kordumatu globaalselt nii, et see sisaldaks oma nimi või nimetähed).

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. **Server Explorer**ühenduse Azure tellimuse ja Azure Storage konto loomine ümbris nimega **Mallid**. Paanil **Atribuudid** **Mallid** container **avaliku lugemisõigus** õiguste määramine **bloobimälu**.

6. **Server Explorer**, paremklõpsake oleval **Mallid** ja seejärel klõpsake **Vaate bloobimälu Container**. **Bloobimälu üleslaadimiseks** nuppu, valige kaks faili, **parameters.json** ja **templates.json**ja klõpsake nuppu **Ava** JSON failide üleslaadimiseks **Mallid** ümbrises. URL-id, mis sisaldavad andmeid, JSON plekid on:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Lisage Program.cs järgmisel viisil:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. **CreateIoTHub** meetodit Mall ja parameetri failide Azure'i ressursihaldur esitada järgmine kood lisamiseks tehke järgmist.

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. **CreateIoTHub** meetod, mis kuvab olek ja uus asjade jaoturi klahvid järgmine kood lisamiseks tehke järgmist.

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Lõpetamine ja käivitage rakendus

Nüüd saate rakenduse lõpule viia, helistades **CreateIoTHub** meetod enne koostamine ja käivitage see.

1. Lisage järgmine kood **Esilehele** meetod lõppu.

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Klõpsake **koostamine** ja seejärel **luua lahenduse**. Vigade.

3. Klõpsake **silumine** ja seejärel **Käivitage silumine** rakenduse käivitamiseks. Võib kuluda mitu minutit juurutamiseks käivitamiseks.

4. Saate kontrollida, et rakenduse lisatud uus asjade jaoturi külastades [Azure portaali] [ lnk-azure-portal] ja ressursside või **Get-AzureRmResource** PowerShelli cmdleti abil loendi vaatamine.

> [AZURE.NOTE] Selles näites rakenduse lisab mõne S1 Standard asjade jaoturi jaoks, mis on arve. Saate kustutada asjade jaoturi [Azure portaali] kaudu[ lnk-azure-portal] või **Eemaldamine – AzureRmResource** PowerShelli cmdleti abil, kui olete lõpetanud.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete juurutanud asjade jaoturi, mis on Azure ressursihaldur malli abil C# programmi, võite täpsemaks analüüsimiseks:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
