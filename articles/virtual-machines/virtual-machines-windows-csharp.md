<properties
    pageTitle="Juurutada Azure ressursse, kasutades C# | Microsoft Azure'i"
    description="Siit saate teada, C# ja Azure ressursihaldur abil loomine Microsoft Azure'i ressursid."
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
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Azure'i ressursid abil C juurutamine# 

Selles artiklis kirjeldatakse Azure ressursse, kasutades C# loomise kohta.

Peate esmalt veenduge, et olete järgmised toimingud:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx) installida
- Veenduge, et [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) või [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855) installimise
- Hankida soovitud [autentimise luba](../resource-group-authenticate-service-principal.md)

Kulub umbes 30 minutit tehke järgmist.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Samm 1: Visual Studio projekti loomine ja teekide installimine

Nugeti on on kõige lihtsam viis installida teeke, mida soovite selle õpetuse lõpuni. Teekide, mida vajate Visual Studio saamiseks tehke järgmist.

1. Klõpsake menüü **fail** > **uue** > **projekti**.

2. **Korduvkasutatava** > **Visual C#**, valige **Konsooli rakendus**, sisestage nimi ja projekti asukoht ja seejärel klõpsake nuppu **OK**.

3. Paremklõpsake Solution Exploreris projekti nime ja seejärel käsku **Halda NuGet-paketid**.

4. *Active Directory* otsinguväljale, klõpsake nuppu **Installi** Active Directory Authentication Library paketi ja paketi installimiseks järgige.

5. Valige lehe ülaosas **Väljalaske-eelne kaasata**. Välja Otsing, tippige *Microsoft.Azure.Management.Compute* klõpsake **installida** arvutada .NET teekide ja paketi installimiseks järgige.

6. Välja Otsing, tippige *Microsoft.Azure.Management.Network* klõpsake **installida** võrgu .NET teekide ja paketi installimiseks järgige.

7. Välja Otsing, tippige *Microsoft.Azure.Management.Storage* klõpsake **installida** salvestusruumi .NET teekide ja paketi installimiseks järgige.

8. Tippige väljale Otsing *Microsoft.Azure.Management.ResourceManager* , klõpsake **installida** ressursside haldamine teekide jaoks.

Nüüd olete valmis alustama, teekide abil saate luua rakenduse.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Samm 2: Looge identimisteavet, mida kasutatakse autentida taotlused

Nüüd saate vormindada rakenduse teavet, et varem loodud identimisteavet, mida kasutatakse autentida taotlusi Azure'i ressursihaldur sisse.

1. Avage loodud projekti Program.cs fail ja seejärel lisage need abil faili üles laused:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Luba, mida on vaja luua, seda meetodit lisada programmi klassi:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {KliendiID} asendamine Azure Active Directory ainuidentifikaator rakenduse {kliendi salajane} kiirklahv AD rakendus ja {rentniku-id} tellimuse identifikaatoriga rentniku. Rentniku leiab käivitades Get-AzureRmSubscription. Kiirklahv leiate Azure'i portaalis.

3. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod Program.cs faili järgmine kood:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Salvestage fail Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Samm 3: Ressursi pakkujad registreerida ja ressursside loomine

### <a name="register-the-providers-and-create-a-resource-group"></a>Registreerida pakkujate ja ressursside rühma loomine

Kõik ressursid peavad sisaldama ressursirühma. Enne rühma saate lisada ressursse, tuleb tellimuse registreerida ressursi pakkujad.

1. Muutujate lisamiseks Esilehele meetod programmi klassi nimed, mida soovite kasutada ressursside määramiseks tehke järgmist.

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Asendage kõik muutujaga nimed ja identifikaator, mida soovite kasutada. Tellimuse ID leiate käivitades Get-AzureRmSubscription.

2. Ressursi rühma loomine ja registreerimist pakkujate, lisada seda meetodit programmi klassi:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod järgmine kood:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

[Salvestusruumi konto](../storage/storage-create-storage-account.md) on vaja virtuaalse masina jaoks loodud virtuaalse kõvaketta faili salvestada.

1. Salvestusruumi konto loomiseks lisada seda meetodit programmi klassi:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod programmi klassi järgmine kood:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Avaliku IP-aadressi loomine

Avaliku IP-aadress on vaja virtuaalse masina suhelda.

1. Avaliku IP-aadress virtuaalse masina loomiseks lisada seda meetodit programmi klassi:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod programmi klassi järgmine kood:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Virtuaalse võrgu loomine

Virtuaalse masina, mis on loodud mudeliga ressursihaldur juurutamise peab olema virtuaalse võrku.

1. Lisada mõne alamvõrgu ja virtuaalse võrgu loomiseks seda meetodit programmi klassi:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod programmi klassi järgmine kood:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Võrgu liidese loomine

Virtuaalse masina peab võrgu liidese virtuaalse võrgus suhelda.

1. Võrgu liidese loomiseks lisada seda meetodit programmi klassi:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod programmi klassi järgmine kood:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Luua mõne kättesaadavus

Kättesaadavus komplekti lihtsam haldamine virtuaalmasinates, teie rakendus kasutab säilitamine.

1. Kättesaadavus hulga loomiseks lisada seda meetodit programmi klassi:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod programmi klassi järgmine kood:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Luua virtuaalse masina

Nüüd, kui olete loonud kõik täiendavad ressursid, saate luua virtuaalse masina.

1. Virtuaalse masina loomiseks lisada seda meetodit programmi klassi:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Selle õpetuse loob töötab opsüsteem Windows Serveri versioon. Muid kujutisi valimise kohta lisateabe saamiseks vt [navigeerimine ja valige Azure virtuaalse masina pilte Windows PowerShelli ja Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

2. Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod järgmine kood:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Samm 4: Kustutamine ressursside

Kuna ostmisega kasutada Azure ressursse, on alati hea tava kustutamine ressursse, mis ei ole enam vaja. Kui soovite kustutada selle virtuaalmasinates ja kõik täiendavad ressursid, on kõik, mida teha, kustutada ressursirühma.

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

2.  Meetod, mis varem lisatud helistamiseks lisamine Esilehele meetod järgmine kood:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Juhis 5: Konsooli rakenduse käivitada.

1. Konsooli rakenduse käivitamiseks klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

2. Pärast iga olekukoodi tagastatakse iga ressursi loomiseks vajutage sisestusklahvi **Enter** . Pärast virtuaalse masina on loodud, tehke enne klahvi Enter, et kustutada kõik ressursid järgmise juhise juurde.

    Kulub umbes viis minutit selle konsooli rakenduse käivitamiseks täielikult algusest lõpuni. Enne, kui vajutate sisestusklahvi, kustutamise ressursid alustamiseks, teil võib kuluda mõni minut kinnitamiseks ressursside Azure portaali loomine enne kustutamist.

3. Ressursside oleku vaatamiseks liikuge auditilogide Azure'i portaalis:

    ![Auditilogide Azure'i portaalis sirvimine](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Järgmised sammud

- Ära malli abil saate luua virtuaalse masina abil teabe [Deploy on Azure virtuaalse masina C# ja ressursihaldur malli abil](virtual-machines-windows-csharp-template.md).
- Saate teada, kuidas hallata virtuaalse masina, vaadates [haldamine virtuaalmasinates Azure'i ressursihaldur ja PowerShelli abil](virtual-machines-windows-csharp-manage.md)loodud.
