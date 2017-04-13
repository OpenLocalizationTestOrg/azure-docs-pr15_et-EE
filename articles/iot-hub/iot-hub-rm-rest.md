<properties
    pageTitle="Soovitud asjade jaoturi abil REST API loomine | Microsoft Azure'i"
    description="Järgige selle õpetuse soovitud asjade keskuse loomiseks REST API kasutamise alustamine."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Õpetus: Loo asjade jaoturi, mis C# programmi ja REST API abil

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Sissejuhatus

Saate kasutada [asjade jaoturi ressursi pakkuja REST API] [ lnk-rest-api] luua ja hallata Azure asjade jaoturi programmiliselt. Selle õpetuse näidatakse, kuidas luua ka asjade jaoturi programmist C# asjade jaoturi ressursi pakkuja REST API abil.

> [AZURE.NOTE] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [Azure'i ressursihaldur ja klassikaline](../resource-manager-deployment-model.md).  Selles artiklis antakse ülevaade Azure'i ressursihaldur juurutamise mudeli abil.

Selle õpetuse tegemiseks vajate järgmist:

- Microsoft Visual Studio 2015.
- Aktiivne Azure'i konto. <br/>Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.
- [Microsoft Azure'i PowerShelli 1.0] [ lnk-powershell-install] või uuem versioon.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projekti ettevalmistamine

1. Visual Studio Visual C# Windows projekti **Konsooli rakendus** projekti malli abil loomine. Projekti **CreateIoTHubREST**nimi.

2. Solution Exploreris projektiga paremklõpsake ja valige **Halda NuGet-paketid**.

3. Nugeti Package Manager, märkige ruut **kaasa väljalaske-eelne**ja otsida **Microsoft.Azure.Management.ResourceManager**. **Installige**, **Vaata muudatused** nuppu **OK**, klõpsake käsku **nõustun** litsentside kinnitamiseks.

4. Nugeti Package Manager otsida **Microsoft.IdentityModel.Clients.ActiveDirectory**.  **Installida**, nuppu **Vaata muudatused läbi** **OK**klõpsake **nõustun** litsentsi kinnitamiseks.

6. Program.cs, asendage olemasolev **abil** laused järgmist:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Program.cs, lisage järgmised staatilised muutujad kohatäite väärtuste asendamine. Tegite selles õpetuses märkme **ApplicationId**, **SubscriptionId**, **TenantId**ja **parool** . **Ressursi rühma nimi** on nimi, ressursirühma asjade jaoturi loomisel saate kasutada, võib olla olemasoleva ressursirühm või uus. **Asjade jaoturi nimi** on nimi asjade jaoturi loote, nt **MyIoTHub** (selle nimi peab olema globaalselt kordumatu ja nii, et see sisaldaks oma nimi või nimetähed). **Juurutamise nimi** on nimi, nt **Deployment_01**juurutamise.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Soovitud asjade keskuse loomiseks kasutada REST API-ga

Kasutage [Asjade jaoturi REST API] [ lnk-rest-api] ressursirühma soovitud asjade keskuse loomiseks. REST API abil saate muuta olemasolevaid asjade jaoturiga.

1. Lisage Program.cs järgmisel viisil:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Järgmine kood lisamiseks **CreateIoTHub** meetodi abil autentimise luba päised **HttpClient** objekti loomiseks tehke järgmist.

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Lisage järgmine kood **CreateIoTHub** meetodiga asjade keskuse loomine ja luua JSON esituse kirjeldamiseks (asukohad, mis toetavad asjade jaoturi praeguse loendi leiate [Azure'i oleku][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. **CreateIoTHub** meetodit ülejäänud taotluse Azure, märkige ruut vastus ja tuua URL-i abil saate jälgida juurutamise tööülesande järgmine kood lisamiseks tehke järgmist.

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. **CreateIoTHub** meetodit kasutada **asyncStatusUri** aadressi tuua eelmises etapis ootama lõpuleviimiseks juurutamiseks lõppu järgmine kood lisamiseks tehke järgmist.

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. **CreateIoTHub** meetodi abil tuua asjade jaoturi olete loonud ja printige konsooli võtmed lõppu järgmine kood lisamiseks tehke järgmist.

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Lõpetamine ja käivitage rakendus

Nüüd saate rakenduse lõpule viia, helistades **CreateIoTHub** meetod enne koostamine ja käivitage see.

1. Lisage järgmine kood **Esilehele** meetod lõppu.

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Klõpsake **koostamine** ja seejärel **luua lahenduse**. Vigade.

3. Klõpsake **silumine** ja seejärel **Käivitage silumine** rakenduse käivitamiseks. Võib kuluda mitu minutit juurutamiseks käivitamiseks.

4. Saate kontrollida, et rakenduse lisatud uus asjade jaoturi külastades [Azure portaali] [ lnk-azure-portal] ja ressursside või **Get-AzureRmResource** PowerShelli cmdleti abil loendi vaatamine.

> [AZURE.NOTE] Selles näites rakenduse lisab mõne S1 Standard asjade jaoturi jaoks, mis on arve. Kui olete lõpetanud, saate kustutada asjade jaoturi [Azure portaali] kaudu[ lnk-azure-portal] või **Eemaldamine – AzureRmResource** PowerShelli cmdleti abil, kui olete lõpetanud.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete juurutanud soovitud asjade jaoturi abil REST API-ga, võite täpsemaks analüüsimiseks:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
