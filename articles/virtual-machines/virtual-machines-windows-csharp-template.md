<properties
    pageTitle="Juurutamine VM C# ja ressursihaldur malli abil | Microsoft Azure'i"
    description="Siit saate teada, kuidas mõni Azure VM juurutamine C# ja ressursihaldur malli abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Juurutada Azure virtuaalse masina C# ja ressursihaldur malli abil

Ressursi rühmad ja mallide abil saate hallata kõiki ressursse koos rakenduse toetavad. Selles artiklis kirjeldatakse, kuidas kasutada Visual Studio ja C# autentimise häälestamise kohta leiate malli loomine ja seejärel juurutada Azure ressursse, mis on loodud malli abil.

Peate esmalt veenduge, et olete teinud häälestus järgmiselt:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx) installida
- Veenduge, et [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) või [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855) installimise
- Hankida soovitud [autentimise luba](../resource-group-authenticate-service-principal.md)
- Ressursi rühma [Azure PowerShelli](../resource-group-template-deploy.md), [Azure'i CLI](../resource-group-template-deploy-cli.md)või [Azure portaali](../resource-group-template-deploy-portal.md)loomine.

Kulub umbes 30 minutit tehke järgmist.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Samm 1: Visual Studio projekti, mallifail ja parameetrite faili loomine

### <a name="create-the-template-file"></a>Malli faili loomine

Mõni Azure ressursihaldur Mall võimaldab teil juurutada ja hallata Azure ressursid koos. Mall on ressursid ja seotud juurutamise parameetrite JSON kirjeldust.

Visual Studio, tehke järgmist.

1. Klõpsake menüü **fail** > **uue** > **projekti**.

2. **Korduvkasutatava** > **Visual C#**, valige **Konsooli rakendus**, sisestage nimi ja projekti asukoht ja seejärel klõpsake nuppu **OK**.

3. Paremklõpsake Solution Exploreris projekti nime, klõpsake nuppu **Lisa** > **Uus üksus**.

4. Klõpsake nuppu Veeb, valige JSON-fail, sisestage *VirtualMachineTemplate.json* nime ja seejärel klõpsake nuppu **Lisa**.

5. Nõutav skeemi elemendi ja nõutav contentVersion elemendi lisamine avamise ja sulgemise sulgudes VirtualMachineTemplate.json faili:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parameetrite](../resource-group-authoring-templates.md#parameters) alati need pole kohustuslikud, kuid need võimaldavad malli juurutamisel väärtuste sisestamiseks. Pärast contentVersion elemendi elemendi parameetrid ja selle tütarelemendid lisamine

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Muutujaid](../resource-group-authoring-templates.md#variables) saab kasutada malli määrata väärtused, mis võivad muutuda sageli või väärtused, mis on vaja luua kombinatsiooni parameetrite väärtused. Pärast jaotist parameetrite muutujate elemendi lisamiseks tehke järgmist.

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Ressursse](../resource-group-authoring-templates.md#resources) , näiteks virtuaalse masina, virtuaalse võrgu ja salvestusruumi konto on määratletud järgmine mall. Pärast jaotist muutujate ressursid jaotise lisamiseks tehke järgmist.

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Salvestage Mall loodud fail.

### <a name="create-the-parameters-file"></a>Parameetrite faili loomine

Ressursi parameetrite, mis on määratletud malli väärtuste määramiseks loote parameetrite fail, mis sisaldab väärtusi, mida kasutatakse malli juurutamisel. Visual Studio, tehke järgmist.

1. Paremklõpsake Solution Exploreris projekti nime, klõpsake nuppu **Lisa** > **Uus üksus**.

2. Klõpsake nuppu Veeb, valige JSON-fail, sisestage *Parameters.json* nime ja seejärel klõpsake nuppu **Lisa**.

3. Avage parameters.json fail ja seejärel lisage see JSON sisu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Selles artiklis loob töötab opsüsteem Windows Serveri versioon. Muid kujutisi valimise kohta lisateabe saamiseks vt [navigeerimine ja valige Azure virtuaalse masina pilte Windows PowerShelli ja Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

4. Parameetrite loodud faili salvestada.

## <a name="step-2-install-the-libraries"></a>Samm 2: Installida teekide

Nugeti on on kõige lihtsam viis installida teeke, mida soovite selle õpetuse lõpuni. Peate Azure'i allikate haldamine kogu ja Azure Active Directory Authentication Library ressursside loomine. Visual Studio need teegid saamiseks tehke järgmist.

1. Paremklõpsake Solution Exploreris projekti nime, klõpsake nuppu **Halda Nugeti pakettide**ja seejärel klõpsake nuppu Sirvi.

2. *Active Directory* otsinguväljale, klõpsake nuppu **Installi** Active Directory Authentication Library paketi ja paketi installimiseks järgige.

4. Valige lehe ülaosas **Väljalaske-eelne kaasata**. Välja Otsing, tippige *Microsoft.Azure.Management.ResourceManager* klõpsake **installida** Microsoft Azure'i ressursside haldamine teekide ja paketi installimiseks järgige.

Nüüd olete valmis alustama, teekide abil saate luua rakenduse.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Samm 3: Looge identimisteavet, mida kasutatakse autentida taotlused

Azure Active Directory rakendus on loodud ja autentimine teek on installitud. Nüüd saate vormindada rakenduse teabe identimisteavet, mida kasutatakse taotlusi Azure'i ressursihaldur autentida.

1. Avage loodud projekti Program.cs fail ja seejärel lisage need abil faili üles laused:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Programmi klassi luba, mida on vaja luua mandaadi saamiseks selle meetodi lisamiseks tehke järgmist.

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {KliendiID} asendamine Azure Active Directory ainuidentifikaator rakenduse {kliendi salajane} kiirklahv AD rakendus ja {rentniku-id} tellimuse identifikaatoriga rentniku. Rentniku leiab käivitades Get-AzureRmSubscription. Kiirklahv leiate Azure'i portaalis.

3. Mandaadi loomiseks lisage Main meetod Program.cs faili järgmine kood:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Salvestage fail Program.cs.

## <a name="step-4-deploy-the-template"></a>Samm 4: Malli

Selles etapis tuleb kasutada varem loodud ressursirühm, kuid saate luua ka [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) ja [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) tunnid abil ressursirühma.

1. Esilehele meetod programmi klassi määramiseks varem loodud ressursside nimed, juurutamise nime ja oma tellimuse ID muutujate lisamiseks tehke järgmist.

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Asendage väärtus groupName ressursi rühma nime. Asendage väärtus deploymentName juurutamise jaoks soovitud nimi. Tellimuse ID leiate käivitades Get-AzureRmSubscription.

2. Programmi klassi juurutada ressursside ressursirühma määratletud malli abil seda meetodit lisamiseks tehke järgmist.

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Kui soovite malli salvestusruumi konto, saate asendada TemplateLink atribuudi malli atribuut.

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Juhis 5: Kustutamine ressursside

Kuna ostmisega kasutada Azure ressursse, on alati hea tava kustutamine ressursse, mis ei ole enam vaja. Teil pole vaja iga ressursi eraldi kustutada ressursirühma. Kustutage ressursirühma ja kustutatakse kõik selle ressursid.

1.  Ressursirühma kustutamiseks lisada seda meetodit programmi klassi:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Samm 6: Konsooli rakenduse käivitada.

1.  Konsooli rakenduse käivitamiseks klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama identimisteavet, mida kasutate oma tellimuse kasutamise Azure AD.

2.  Pärast aktsepteeritud olek kuvatakse, vajutage sisestusklahvi **Enter** .

    Kulub umbes viis minutit selle konsooli rakenduse käivitamiseks täielikult algusest lõpuni. Enne, kui vajutate sisestusklahvi, kustutamise ressursid alustamiseks, teil võib kuluda mõni minut kinnitamiseks ressursside Azure portaali loomine enne kustutamist.

3. Ressursside oleku vaatamiseks liikuge auditilogide Azure'i portaalis:

    ![Auditilogide Azure'i portaalis sirvimine](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Järgmised sammud

- Kui juurutamise probleeme, oleks vaadata [tõrkeotsingu ressursside rühma juurutuste Azure'i portaalis](../resource-manager-troubleshoot-deployments-portal.md)järgmise juhisega.
- Saate teada, kuidas hallata virtuaalse masina, vaadates [haldamine virtuaalmasinates Azure'i ressursihaldur ja PowerShelli abil](virtual-machines-windows-csharp-manage.md)loodud.
