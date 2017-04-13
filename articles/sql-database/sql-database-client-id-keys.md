<properties
   pageTitle="Tuua nõutav rakenduse juurde pääseda SQL-andmebaasi koodi autentimiseks | Microsoft Azure'i"
   description="Looge teenuse põhilise juurdepääsuks SQL-andmebaasi kood."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Rakenduse juurde pääseda SQL-andmebaasi koodi autentimiseks vajalikud väärtused hankimine

Saate luua ja hallata SQL-andmebaasi koodi peate registreerima rakenduse Azure Active Directory (AAD) domeeni tellimuse, kui olete loonud oma Azure ressursse.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Luua teenuse põhisumma ressursid Accessi rakendusest

Peab teil olema uusim [Azure PowerShelli](https://msdn.microsoft.com/library/mt619274.aspx) installitud ja töötab. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

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




## <a name="see-also"></a>Vt ka

- [C# SQL-i andmebaasi loomine](sql-database-get-started-csharp.md)
- [Ühenduse loomine SQL-andmebaasi Azure Active Directory autentimist kasutades](sql-database-aad-authentication.md)


