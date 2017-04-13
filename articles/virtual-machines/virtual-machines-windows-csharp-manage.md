<properties
    pageTitle="Hallata VMs Azure'i ressursihaldur ja C# abil | Microsoft Azure'i"
    description="Hallata virtuaalmasinates Azure'i ressursihaldur ja C# abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Azure'i Virtuaalmasinates Azure'i ressursihaldur ja C abil haldamine#  

Selles artiklis tööülesannete näitavad, kuidas hallata virtuaalmasinates, nt alates, peatamine ja värskendamine. Selle artikli toimingu lõpuleviimiseks ressursirühma kuuluma virtuaalse masina.

Selle artikli toimingu lõpuleviimiseks peate.

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Mõne autentimise luba](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Visual Studio projekti loomine ja installige paketid

Nugeti on kõige lihtsam võimalust installida teeke, mida soovite selle artikli tööülesanded lõpuni. Teekide, mille saate installida selle artikli on Azure Active Directory Authentication Library ja arvutada ressursi pakkuja teek. Nende juhiste saamiseks teekide Visual Studio:

1. Klõpsake menüü **fail** > **uue** > **projekti**.

2. **Korduvkasutatava** > **Visual C#**, valige **Konsooli rakendus**, sisestage nimi ja projekti asukoht ja seejärel klõpsake nuppu **OK**.

3. Paremklõpsake Solution Exploreris projekti nime ja seejärel käsku **Halda NuGet-paketid**.

4. *Active Directory* otsinguväljale, klõpsake nuppu **Installi** Active Directory Authentication Library paketi ja paketi installimiseks järgige.

5. Valige lehe ülaosas **Väljalaske-eelne kaasata**. Välja Otsing, tippige *Microsoft.Azure.Management.Compute* klõpsake **installida** arvutada .NET teekide ja paketi installimiseks järgige.

Nüüd olete valmis alustama oma virtuaalmasinates haldamine teekide abil.

## <a name="set-up-the-project"></a>Projekti häälestamine

Nüüd, kui rakendus on loodud ja teekide on installitud, saate luua märgiks rakenduse teabe abil. Selle märgiks kasutatakse taotlusi Azure'i ressursihaldur autentida.

1. Avage loodud projekti Program.cs fail ja seejärel lisage need abil faili üles laused:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Lisage muutujate Esilehele meetod programmi klassi määramiseks nimi ressursirühma ja nimi ning virtuaalse masina oma tellimuse ID:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Tellimuse ID leiate käivitades Get-AzureRmSubscription.
    
3. Luba, mida on vaja luua mandaadi saamiseks lisada seda meetodit programmi klassi:

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
    
4. Mandaadi loomiseks lisamine põhi meetod Program.cs järgmine kood:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Salvestage fail Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Virtuaalse masina kohta teabe kuvamine

1. Projekti varem loodud programmi klassi seda meetodit lisamiseks tehke järgmist.

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Salvestage fail Program.cs.

4. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

    Kui käivitate selle meetodi, peaksite nägema midagi, nagu järgmises näites:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Virtuaalse masina peatamine

Soovi korral saate peatada virtuaalse masina kahel viisil. Saate peatada virtuaalse masina ja hoidke kõik selle sätted, kuid seda edasi või peatada virtuaalse masina ja selle deallocate. Kui virtuaalse masina on deallocated, on ka kõik sellega seotud ressursid deallocated ja arvelduse lõpetatakse seda.

1. Kommentaaride välja mis tahes kood, et varem lisatud Esilehele meetod, välja arvatud koodi mandaadi.

2. Programmi klassi seda meetodit lisamiseks tehke järgmist.

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Kui soovite Deallocate virtuaalse masina, muutke väljalülitamiseks klõpsake kõne edastamiseks järgmine kood:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Salvestage fail Program.cs.

5. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

    Peaksite nägema peatamiseni virtual masina Muuda olekut. Kui käivitasite meetodit helistaja Deallocate olek on peatatud (deallocated).

## <a name="start-a-virtual-machine"></a>Käivitada

1. Kommentaaride välja mis tahes kood, et varem lisatud Esilehele meetod, välja arvatud koodi mandaadi.

2. Programmi klassi seda meetodit lisamiseks tehke järgmist.

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Salvestage fail Program.cs.

5. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

    Peaksite nägema virtuaalse masina oleku muutmine töötab.

## <a name="restart-a-running-virtual-machine"></a>Taaskäivitage käivitatud virtuaalse masina

1. Kommentaaride välja mis tahes kood, et varem lisatud Esilehele meetod, välja arvatud koodi mandaadi.

2. Programmi klassi seda meetodit lisamiseks tehke järgmist.

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Salvestage fail Program.cs.

5. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

## <a name="resize-a-virtual-machine"></a>Virtuaalse masina suuruse muutmine

Selles näites kirjeldatakse, kuidas käivitatud virtuaalse masina suuruse muutmiseks.

1. Kommentaaride välja mis tahes kood, et varem lisatud Esilehele meetod, välja arvatud koodi mandaadi.

2. See meetod lisada programmi klassi:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Salvestage fail Program.cs.

5. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

    Peaksite nägema Standard_A1 virtuaalse masina Muuda suurust.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Virtuaalse masina kettal andmete lisamine

Selles näites kirjeldatakse, kuidas lisada andmed kettale töötava virtuaalse masina.

1. Kommentaaride välja mis tahes kood, et varem lisatud Esilehele meetod, välja arvatud koodi mandaadi.

2. Programmi klassi seda meetodit lisamiseks tehke järgmist.

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Salvestage fail Program.cs.

5. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

## <a name="delete-a-virtual-machine"></a>Virtuaalse masina kustutamine

1. Kommentaaride välja mis tahes kood, et varem lisatud Esilehele meetod, välja arvatud koodi mandaadi.

2. Programmi klassi seda meetodit lisamiseks tehke järgmist.

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Meetod, mille just lisasite helistamiseks lisamine Esilehele meetod järgmine kood:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Salvestage fail Program.cs.

5. Klõpsake nuppu **Käivita** Visual Studio ja seejärel logige sisse sama kasutajanimi ja parool, mida kasutate oma tellimuse kasutamise Azure AD.

## <a name="next-steps"></a>Järgmised sammud

Kui probleemid juurutamine, võib vaadata [tõrkeotsingu ressursside rühma juurutuste Azure'i portaal](../resource-manager-troubleshoot-deployments-portal.md)
