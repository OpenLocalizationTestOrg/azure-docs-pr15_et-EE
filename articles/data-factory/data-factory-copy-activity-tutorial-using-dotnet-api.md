<properties 
    pageTitle="Õpetus: Luua müügivõimaluste Kopeeri tegevuse kasutades .net-i API | Microsoft Azure'i" 
    description="Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse .NET API abil." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Õpetus: Luua müügivõimaluste Kopeeri tegevuse API .net-i abil
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopeerige viisard](data-factory-copy-data-wizard-tutorial.md)
- [Azure'i portaal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure'i ressursihaldur Mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-GA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-I API-GA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Selle õpetuse näidatakse, kuidas luua ja jälgida Azure'i andmed factory, mis API .net-i abil. Müügivõimaluste rakenduses andmete factory kasutab Kopeeri tegevuse Azure'i bloobimälu andmete kopeerimine Azure'i SQL-andmebaasi.

Kopeeri tegevuse sooritab Azure'i andmed Factory andmete liikumine. Tegevuse on tootja globaalselt saadaval teenust, mida saate kopeerida andmeid erinevate andmete poed turvaline, usaldusväärseid ja scalable viisil. Vt [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel Kopeeri tegevuse üksikasjad.   

> [AZURE.NOTE] 
> See artikkel ei sisalda andmeid Factory .net-i API. Vt lisateavet andmete Factory .NET SDK [Andmete Factory .NET API viide](https://msdn.microsoft.com/library/mt415893.aspx) . 

## <a name="prerequisites"></a>Eeltingimused
- Läbida [õpetuse ülevaade ja eeltingimused](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) ülevaate õpetuse ja täitke juhised **nõutav** . 
- Visual Studio 2012 või 2013 või 2015
- Laadige alla ja installige [Azure'i .NET SDK](http://azure.microsoft.com/downloads/)
- Azure'i PowerShelli. Järgige juhiseid teemas [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) Azure PowerShelli oma arvutisse installida. Azure'i PowerShelli abil saate luua rakenduse Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Azure Active Directory rakenduse loomine
Azure Active Directory rakenduse loomine, teenuse põhilise rakenduse loomine ja selle määrata **Andmete Factory kaasautori** roll.  

1. Käivitage **PowerShelli**. 
1. Käivitage järgmine käsk ja sisestage kasutajanimi ja parool, mida kasutate Azure portaali sisse logida.
    
        Login-AzureRmAccount   
2. Käivitage järgmine käsk, et kuvada kõik tellimused selle konto jaoks.

        Get-AzureRmSubscription 
3. Käivitage järgmine käsk valige tellimus, mida soovite töötada. Asendage ** &lt;NameOfAzureSubscription** &gt; Azure tellimuse nime. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Märkus Selle käsu väljund **SubscriptionId** ja **TenantId** alla. 
4. Azure'i ressursirühm, nimega **ADFTutorialResourceGroup** on PowerShelli järgmise käsu abil luua.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Kui ressursirühma juba olemas, saate määrata, kas ajakohastada (Y) või hoida (N). 

    Kui kasutate muu ressursirühm, peate kasutama nime asemel ADFTutorialResourceGroup ressursirühma selles õpetuses.
5. Azure Active Directory rakenduse loomine. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Kui kuvatakse järgmine tõrketeade, määrata erinevaid URL ja käivitage käsk uuesti. 

        Another object with the same value for property identifierUris already exists.

6. Saate luua teenuse põhilise reklaami. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Teenuse põhilise lisada **Andmed Factory kaasautori** roll. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Saada rakenduste ID-ga.

        $azureAdApplication

    Pange kirja rakenduse ID (alates väljund**applicationID** ).

Teil peaks pärast nelja väärtused järgmiselt: 

- Rentniku ID
- Tellimuse ID
- Rakenduse ID 
- Parooli (määratud esimese käsu)   

## <a name="walkthrough"></a>Kiirtutvustus
1. Visual Studio 2012/2013/2015 kaudu, luua C# .net-i konsooli rakendus.
    1. Käivitage **Visual Studio** 2012-2013-2015.
    2. Klõpsake menüüd **fail**, käsku **Uus**ja valige **Project**.
    3. Laiendage **Mallid**ja valige **Visual C#**. Ülevaate, saate kasutada C#, kuid saate kasutada mis tahes .NET keel.
    4. Valige loendist paremal projekti tüüpi **Konsooli rakendus** .
    5. Sisestage **DataFactoryAPITestApp** nimi.
    6. Valige **C:\ADFGetStarted** soovitud asukohta.
    7. Klõpsake nuppu **OK** , et luua projekt.
2. Klõpsake nuppu **Tööriistad**, osutage **Nugeti Package Manager**ja klõpsake **Package Manager konsooli**.
3.  **Paketi Manager konsooli**, tehke järgmist. 
    1.  Andmete Factory paketi installimiseks järgmine käsk:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Käivitage järgmine käsk installida Azure Active Directory pakett (kasutate Active Directory API kood):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Saate lisada **App.config** faili **appSetttings** järgmisest jaotisest. Neid sätteid kasutavad helper meetodit: **GetAuthorizationHeader**. 

    Väärtuste asendamine ** &lt;rakenduse ID&gt;**, ** &lt;parooli&gt;**, ** &lt;Tellimuse ID&gt;**, ja ** &lt;rentniku ID&gt; ** oma väärtustega. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Saate lisada projekti lähtefaili (Program.cs) **abil** järgmistest.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Lisage järgmine kood, mis loob **DataPipelineManagementClient** klassi **Main** meetod eksemplar. Selle objekti abil saate luua andmete factory, lingitud teenus, sisend- ja andmekomplektide ja müügivõimaluste. Selle objekti kasutada ka sektoritele andmekomplekt käitusajal jälgimiseks.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Asendage väärtus **resourceGroupName** Azure ressursi rühma nime. 
    > 
    > Saate värskendada andmeid factory (**dataFactoryName**) olema kordumatu nimi. Andmete factory nimi peab olema kordumatu globaalselt. Nimede reeglid andmete Factory artefakte [Andmete Factory - nime andmise reeglid](data-factory-naming-rules.md) teemat. 

7. Lisage järgmine kood, mis loob **andmete factory** **Main** meetod.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Lisage järgmine kood, mis loob, on **Azure Storage lingitud teenuse** **Main** meetod. 

    > [AZURE.IMPORTANT] Asendage **storageaccountname** ja **accountkey** nimi ja Azure Storage konto võti. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Lisage järgmine kood, mis loob, on **lingitud Azure SQL-i teenuse** **Main** meetod.
 
    > [AZURE.IMPORTANT] Asendage oma Azure SQL-i server, andmebaasi, kasutaja ja parooli nimed **serverinimi**, **databasename**, **kasutajanimi**ja **parool** .  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Lisage järgmine kood, mis loob **sisend- ja andmekomplektide** **Main** meetod. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Lisage järgmine kood selle **loob ja aktiveerib müügivõimaluste** **Main** meetod. See kohaletoimetamisel on **CopyActivity** , mis viib **BlobSource** allikana ja **BlobSink** valamu.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Lisage järgmine kood **Main** meetod saada andmete sektorit väljundi andmekogumi olekut. Ainult sektorit oodatud selles näites on.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Lisage järgmine kood saada käivitamiseks üksikasjad andmete sektorit **Main** meetod.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Lisage järgmised **Esilehele** meetod **programmi** klassi helper meetodit.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. Solution Exploreris laiendamine projekt (**DataFactoryAPITestApp**), paremklõpsake **Viited**ja klõpsake nuppu **Lisada viide**. Märkige ruut "**System.Configuration**" komplekti ja klõpsake nuppu **OK**. 
16. Koostada konsooli rakendus. Klõpsake menüü **koostamine** ja klõpsake **Lahenduse luua**. 
16. Veenduge, et vähemalt ühte faili **adftutorial** ümbrises Azure'i bloobimälus. Kui ei, järgmise sisuga Notepad **Emp.txt** faili loomine ja üleslaadimine adftutorial ümbrises.

        John, Doe
        Jane, Doe
     
17. Käivitage valimi, klõpsates **silumine** -> menüüs**Start silumine** . Kui näete, et **saada käivitada üksikasjad andmete vormindamine**, oodake paar minutit ja vajutage sisestusklahvi **ENTER**. 
18. Veenduge, et andmete factory **APITutorialFactory** luuakse järgmised esemeid Azure portaali abil. 
    - Lingitud teenus: **LinkedService_AzureStorage** 
    - Andmekomplekti: **DatasetBlobSource** ja **DatasetBlobDestination**.
    - Müügivõimaluste: **PipelineBlobSample** 
18. Veenduge, et "**emp**" tabelis määratud SQL Azure'i andmebaas loodud kahe töötaja kirjeid.

## <a name="next-steps"></a>Järgmised sammud

- Lugege läbi [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis pakub üksikasjalikku teavet Kopeeri tegevuse kasutasite õpetuses.
- Vt lisateavet andmete Factory .NET SDK [Andmete Factory .NET API viide](https://msdn.microsoft.com/library/mt415893.aspx) . See artikkel ei sisalda andmeid Factory .net-i API. 

 
