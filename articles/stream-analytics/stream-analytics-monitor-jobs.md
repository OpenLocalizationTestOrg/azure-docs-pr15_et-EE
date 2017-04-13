<properties
    pageTitle="programmiliselt jälgida töö voo Analytics | Microsoft Azure'i"
    description="Saate teada, kuidas programmiliselt jälgida voo Analytics töö loodud REST API-d, Azure'i SDK või PowerShelli kaudu."
    keywords=".net kuvar, töö kuvaris, jälgimise app"
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


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Programmiliselt loomine voo Analytics töö jälgimine
 Selles artiklis näitab, kuidas lubada voo Analytics töö jälgimine. Voo Analytics REST API-d, Azure'i SDK või PowerShelli kaudu loodud töökohtade ei ole on jälgimise vaikimisi.  Saate käsitsi lubada see Azure'i portaalis liikumise projekti jälgimine lehele ja klõpsake nuppu Luba või selle protsessi automatiseerida selles artiklis toodud juhiseid järgides. Andmed kuvatakse "Jälgimine" menüüs Azure'i portaalis oma voo Analytics töö.

![töö jälgimine klõpsake vahekaarti](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Eeltingimused
Enne alustamist selles artiklis, peab teil olema järgmised:

- Visual Studio 2012 või 2013.
- Laadige alla ja installige [Azure'i .NET SDK](https://azure.microsoft.com/downloads/).
- Olemasoleva voo Analytics-töö, mis tuleb jälgimine lubatud.

## <a name="setup-a-project"></a>Projekti häälestamine

1.  Looge Visual Studio C# .net-i konsooli rakendus.
2.  Konsooli paketi Manager, käivitage järgmised käsud Nugeti pakettide installimiseks. Esimene on Azure voo Analytics halduse .NET SDK. Teine on Azure kuvari Tarkvaraarenduskomplektist, mis kasutab jälgida. Viimane on Azure Active Directory klient, mida kasutatakse autentimiseks.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  Saate lisada App.config faili appSettings järgmisest jaotisest.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Väärtuste asendamine *SubscriptionId* ja *ActiveDirectoryTenantId* Azure'i tellimus ja rentniku ID-d. Saate need väärtused, käitades järgmine PowerShelli cmdlet-käsk:

    ```
    Get-AzureAccount
    ```
4.  Lisage järgmine laused abil projekti lähtefaili (Program.cs).

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Lisa sisestusmeetod autentimise helper.

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

## <a name="create-management-clients"></a>Looge klientide haldamine
Järgmine kood loob vajalikud muutujad ja haldus kliendid.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Olemasoleva voo Analytics tööd jälgida

Järgmine kood võimaldab **olemasoleva** voo Analytics tööd jälgimine. Esimene osa koodi teostab GET-voo Analytics töö kohta teabe toomiseks voo Analytics teenuse vastu päringu. Kasutab atribuudi "Id" (vormilt GET-päringu) parameetrina panna meetodi teine pool koodi, mis saadab panna taotlus ülevaateid teenuse lubamiseks voo Analytics töö jälgimine.

> [AZURE.WARNING]
> Kui olete varem lubanud jälgimine erinevate voo Analytics tööd, kas Azure portaali kaudu või programmiliselt kaudu kuvatakse allpool kood, **on soovitatav, et esitate sama salvestusruumi konto nimi, kui olete varem lubanud jälgimine.**
>
> Salvestusruumi konto on lingitud piirkond, mis on loodud voo Analytics tööd, ei ole eriti töö ise.
>
> Kõik voo Analytics töö (ja muud Azure ressursid), et sama piirkonna jagada selle salvestusruumi konto jälgimisega seotud andmete talletamiseks. Kui annate erinevate salvestusruumi konto, see võib põhjustada ootamatuid küljel oma voo Analytics töökohta ja/või muud Azure ressursid jälgimiseks.
>
> Salvestusruumi konto nimi, mida kasutatakse asendamine ```“<YOUR STORAGE ACCOUNT NAME>”``` allpool peaks olema salvestusruumi konto, mis on sama tellimuse nagu voo Analytics töö, mida on lubamine jälgimine.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Tootetugi
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
