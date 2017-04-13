<properties
    pageTitle="Proovige: SQL-andmebaasi Kasutamine C# SQL-andmebaasi loomiseks | Microsoft Azure'i"
    description="Proovige SQL-andmebaasi SQL-i ja C# rakenduste arendamise ja C# .net-i SQL-andmebaasi teegi kasutamise Azure'i SQL-andmebaasi loomine."
    keywords="Proovige sql, SQL-i c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Proovige: SQL-andmebaasi Kasutamine C# SQL-andmebaasi loomine SQL-andmebaasi teegiga .net-i jaoks


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-get-started.md)
- [C#:](sql-database-get-started-csharp.md)
- [PowerShelli](sql-database-get-started-powershell.md)

Saate teada, kuidas kasutada C# SQL Azure'i andmebaasi loomine [Microsoft Azure'i SQL-i halduse teegi .net-i jaoks](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Selles artiklis kirjeldatakse, kuidas luua ühe andmebaasi SQL-i ja C#. Luua elastne andmebaasi, lugege teemat [Create kohapeal on elastne andmebaasi](sql-database-elastic-pool-create-csharp.md).

Azure SQL-i andmebaasi halduse teegi .NET pakub on [Azure ressursihaldur](../azure-resource-manager/resource-group-overview.md)-põhiste API mähkida [Ressursihaldur vastavalt SQL-i andmebaasi REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Palju uusi funktsioone SQL-andmebaas on toetatud ainult siis, kui kasutate [Azure ressursihaldur juurutamise mudeli](../azure-resource-manager/resource-group-overview.md), seega peaksite kasutama alati Viimane **Azure SQL-i andmebaasi teegis .net-i ([dokumendid](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Vanema [klassikaline juurutamise mudeli vastavalt teekide](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) toetatakse ainult tagasiühilduvuse, soovitame kasutada uuem vastavalt ressursihaldur teekide.

Selles artiklis toodud juhiseid tegemiseks vajate järgmist:

- Azure'i tellimuse. Kui teil on vaja Azure tellimuse lihtsalt **Tasuta konto** selle lehe ülaosas nuppu ja siis naaske artiklit lõpuleviimiseks.
- Visual Studio. Visual Studio tasuta koopia, vt [Visual Studio allalaadimine](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] Selles artiklis loob uue tühja andmebaasi SQL-i. Muutke *CreateOrUpdateDatabase(...)* meetodit järgmised valimis kopeerida andmebaase, mastaapimiseks andmebaase, andmebaasi loomine rakenduskausta jne. Lisateavet leiate teemast [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) ja [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) tunnid.



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Konsooli rakenduse loomine ja installige vajalikud teegid

1. Käivitage Visual Studio.
2. Klõpsake menüü **fail** > **uue** > **projekti**.
3. C# **Konsooli teenuserakenduse** loomine ja pange sellele: *SqlDbConsoleApp*


SQL-andmebaasi loomiseks C# laadida nõutav juhtimine teeke ( [package manager konsooli](http://docs.nuget.org/Consume/Package-Manager-Console)abil):

1. Klõpsake **menüü Tööriistad** > **Nugeti Package Manager** > **Package Manager konsooli**.
2. Tippige `Install-Package Microsoft.Azure.Management.Sql –Pre` installida uusim [Microsoft Azure'i SQL-i halduse teek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Tippige `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` installida [Microsoft Azure'i ressursihaldur teek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Tippige `Install-Package Microsoft.Azure.Common.Authentication –Pre` installida [Microsoft Azure'i levinud Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Selle artikli näited sünkroonse vormi iga API taotluse ja blokeerida kuni aluseks oleva teenuse kõne lõpetamise ülejäänud. Saadaval on asünkroonse meetodit.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Luua SQL-andmebaasi server, tulemüüri reegel ja SQL-andmebaasi - C# näide

Järgmises näites luuakse ressursirühma, server, tulemüüri reegel ja SQL-andmebaasi. Vaatamiseks [loomine põhisumma ressursid juurdepääsu teenuse](#create-a-service-principal-to-access-resources) saamiseks selle `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` muutujaid.

Asendage **Program.cs** sisu järgmine ja värskendada selle `{variables}` oma rakenduse väärtustega (ei sisalda soovitud `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Teenuse põhisumma Accessi ressursside loomine

Järgmist PowerShelli skripti loob Active Directory (AD) rakenduse ja teenuse põhilise, mida läheb vaja autentida meie C# rakendus. Skripti väljundid läheb vaja eelnev C# valimi väärtused. Üksikasjalikku teavet leiate teemast [Azure PowerShelli kasutamine põhisumma ressursid juurdepääsu teenuse](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete proovinud SQL-andmebaasi ja C# andmebaasi, olete valmis järgmistest artiklitest:

- [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu tegemine](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-andmebaas](https://azure.microsoft.com/documentation/services/sql-database/)
- [Andmebaasi klassi](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
