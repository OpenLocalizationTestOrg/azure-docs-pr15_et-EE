<properties
    pageTitle="Voo Analytics halduse .NET SDK | Microsoft Azure'i"
    description="Alustamine voo Analytics halduse .NET SDK. Saate teada, kuidas häälestada ja analytics tööde: projekti, sisendeid, väljundeid ja teisendused loomine."
    keywords=".net SDK, analytics API"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>.NET SDK haldus: Häälestamine ja analytics tööde Azure'i voo Analytics API .net-i abil

Saate teada, kuidas häälestada Käivita analytics tööd, mis voo Analytics API kasutades .net-i halduse .NET SDK abil. Projekti häälestamine, luua sisend- ja allikad, muutusi ja käivitamine ja peatamine tööde haldamine. Voogesituseks töökohta analytics andmete bloobimälu või mõni sündmus keskuse kaudu.

Vaadake [voo Analytics API .net-i halduse dokumentides](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure'i voo Analytics on täielikult hallatav teenus, mis hõlmab latentsusajaga, väga kättesaadav, skaleeritav, keerukate sündmuse töötlemiseks üle streaming andmed pilveteenuses. Voo Analytics võimaldab kliendid häälestamiseks streaming töö andmevoogu analüüsimiseks ja võimaldab neid juhtida reaalajas analytics lähedal.  


## <a name="prerequisites"></a>Eeltingimused
Enne alustamist selles artiklis, peab teil olema järgmised:

- Installige Visual Studio 2012 või 2013.
- Laadige alla ja installige [Azure'i .NET SDK](https://azure.microsoft.com/downloads/).
- Teie tellimus Azure'i ressursi rühma loomine. Järgmine on näide Azure PowerShelli skripti. Azure'i PowerShelli leiate teemast [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Häälestada mõne Sisestuskeel lähte- ja väljundi kasutama. Edasi juhiseid teemast [Lisamine sisendina](stream-analytics-add-inputs.md) valimi sisendit häälestamiseks ja [Lisada väljundeid](stream-analytics-add-outputs.md) valimi väljundi häälestamine.


## <a name="set-up-a-project"></a>Projekti häälestamine

Loomiseks kasutage mõnda analytics töö voo Analytics API .net-i, häälestage esmalt oma projekti jaoks.

1. Looge Visual Studio C# .net-i konsooli rakendus.
2. Konsooli paketi Manager, käivitage järgmised käsud Nugeti pakettide installimiseks. Esimene on Azure voo Analytics halduse .NET SDK. Teine on Azure Active Directory klient, mida kasutatakse autentimiseks.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Lisage App.config faili **appSettings** järgmisest jaotisest.

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Väärtuste asendamine **SubscriptionId** ja **ActiveDirectoryTenantId** Azure'i tellimus ja rentniku ID-d. Saate need väärtused, käitades järgmine Azure PowerShelli cmdlet-käsk:

        Get-AzureAccount

5. Projekti lähtefaili (Program.cs) **abil** järgmistest lisamiseks tehke järgmist.

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Lisage mõne helper autentimise meetodit.

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
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


## <a name="create-a-stream-analytics-management-client"></a>Voo Analytics halduse kliendi loomine

**StreamAnalyticsManagementClient** objekti, mis võimaldab teil hallata töö ja töö komponendid, nt sisendi, väljundi ja transformatsioon.

Lisage järgmine kood **Main** meetodi:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

**ResourceGroupName** muutuja väärtus peab olema sama, mis teie loodud või piirkonnadiagrammi nõutav juhistes ressursirühma nimi.

Mandaadi esitluse osa töökohtade automatiseerimiseks viidata [teenuse põhilise Azure'i ressursihaldur autentimist](../resource-group-authenticate-service-principal.md).

Käesoleva artikli ülejäänud jaotiste eeldavad, et järgmine kood on **Esilehele** meetod alguses.

## <a name="create-a-stream-analytics-job"></a>Voo Analytics töö loomine

Järgmine kood tekitab voo Analytics töö jaotises ressursirühm, mille olete määratlenud. Tuleb lisada mõne sisendi, väljundi ja transformatsioon töö hiljem.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Voo Analytics Sisestuskeel andmeallika loomine

Järgmine kood loob bloobimälu Sisestuskeel Reaallika tüüp ja CSV sariväljaanne voo Analytics sisendandmete allikas. Sündmuse jaoturi sisendandmete allikas loomiseks kasutage **EventHubStreamInputDataSource** **BlobStreamInputDataSource**asemel. Samuti saate kohandada sariväljaanne Sisestuskeel andmeallika tüüp.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Sisendandmete allikatest, bloobimälu või mõni sündmus keskuse kaudu on seotud konkreetse töö. Erinevate tööde sama sisendandmete allikas kasutamiseks peate meetodit uuesti kutsuda ja määrata eri töö nimi.


## <a name="test-a-stream-analytics-input-source"></a>Testige voo Analytics sisendandmete allikas

**TestConnection** meetod kontrollib, kas voo Analytics töö saab luua ühenduse sisendandmete allikas kui ka muude aspektide teatud Sisestuskeel andmeallika tüüp. Näiteks bloobimälu Sisestuskeel allikas mõne varasema juhises loodud, meetodit kontrollib Salvestusruumikonto nimi ja paari saab kasutada salvestusruumi kontoga ühenduse loomiseks, samuti kontrollige määratud container on olemas.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Voo Analytics väljundi sihtrakenduse loomine

Mõne väljundi sihtrakenduse loomiseks on väga sarnane loomise voo Analytics sisendandmete allikas. Sisendandmete allikatest, nt väljundi sihtmärgid on seotud konkreetse töö. Erinevate tööde sihtkohas sama väljundi kasutamiseks peate kõne meetodit uuesti ja määrata eri töö nimi.

Järgmine kood tekitab mõne väljundi target (SQL Azure'i andmebaas). Saate kohandada väljundi target andmetüüp ja/või sariväljaanne tüüp.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testige voo Analytics väljundi sihtkoht

Voo Analytics väljundi sihtkoht on ka **TestConnection** meetodit testimiseks ühendused.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Voo Analytics teisendus loomine

Järgmine kood loob voo Analytics teisendus päringu "valige *: sisendit" ja määrab eraldada ühiku streaming voo Analytics töö. Streaming üksuste kohandamise kohta leiate lisateavet teemast [skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Sisestus- ja väljundi, nagu on soovitud teisendused seotud kindla voo Analytics töö dokument on loodud, klõpsake jaotises.

## <a name="start-a-stream-analytics-job"></a>Voo Analytics töö alustamine
Pärast voo Analytics töö ja selle kogumi, output(s) ja transformatsioon loomist saate alustada töö helistades **alustada** meetod.

Järgmine proovi kood käivitamise voo Analytics töökoha kohandatud väljundi algusaeg väärtuseks 12 detsember 2012 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Voo Analytics töö peatamine
Töötava voo Analytics töö saate peatada, helistades **peatada** meetod.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Voo Analytics töö kustutamine
**Kustuta** meetod kustutab töö kui ka aluseks oleva sub ressursse, sh kogumi, output(s) ja transformatsioon töö.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Tootetugi
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Järgmised sammud

Olete Õppimine põhitõed .NET SDK abil luua ja käivitada analytics tööd. Lisateabe saamiseks vaadake järgmist:

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics halduse .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
