<properties 
    pageTitle="Kasutades .NET SDK Azure Mobile kaasamine teenuse API-d juurdepääsu" 
    description="Kirjeldab, kuidas Azure Mobile kaasamine teenuse API-de Mobile kaasamine .NET SDK abil"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Kasutades .NET SDK Azure Mobile kaasamine teenuse API-d juurdepääsu

Azure'i Mobile kaasamine seab kogumi API-de seadmed, hallata REACHi/Push kampaaniat jne. Nende API-de suhelda ka pakume teile [ärplema faili](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , et saate kasutada tööriistu SDK-sid luua oma eelistatud keeles. Soovitame [AutoRest](https://github.com/Azure/AutoRest) tööriista abil saate luua oma SDK meie ärplema failist. 

Oleme loonud .NET SDK sarnasel viisil, mis võimaldab teil suhelda nende API-de kasutamine C# ümbrisesse ja pole teil vaja teha autentimist Turbeloa läbirääkimiste ja värskendage ise.  

Selle valimi kontrollimisel .NET SDK kasutamiseks järgige juhiseid määramine:

1. Kõigepealt peate setup autentimise jaoks oma API-de kasutamine Azure Active Directory kirjeldatud [allpool](mobile-engagement-api-authentication.md#authentication). Need juhised lõpus peaks teil olema lubatud **SubscriptionId**, **TenantId** **ApplicationId** ja **salajane**. 

2. Kasutame näidata töötamise .NET SDK koos stsenaarium, mis teadaanne turunduskampaania loomine lihtsa Windows konsooli rakendus. Nii avada Visual Studio ja luua **Konsooli rakendus**.   

3. Järgmiseks peate alla laadima .NET Tarkvaraarenduskomplektist, mis pole saadaval, kui **Microsoft Azure'i kaasamine teegis** Nugeti Galerii [siin](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Kui installite selle Nugeti Visual Studios, peate veenduge, et on märgitud paketi otsimise ajal **kaasata väljalaske-eelne** suvand kontrolli:

    ![][1]

4. Klõpsake soovitud `Program.cs` faili, lisage järgmised nimeruumid:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Järgmiseks peate määratlema järgmised konstantide, mida me kasutame autentimis- ja suheldes, kus te loote teadaanne turunduskampaania kaasamine mobiilirakenduse:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Määratlege soovitud `EngagementManagementClient` muutuja, mida me kasutame kõne Mobile kaasamine SDK meetodite:

        static EngagementManagementClient engagementClient; 

7. Lisage järgmised oma `Main` meetod:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Määratleda järgmisel viisil, mis hoolitseb käivitamine on `EngagementManagementClient` esmalt autentimist ja seejärel seostada ise Mobile kaasamine rakendusega, kus kavatsete teadaanne turunduskampaania loomine:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Pange tähele, et peate kasutama **Rakenduse ressursinimi** Azure'i haldusportaal rakendusenimi parameetri jaoks määratletud. 

9. Lõpetuseks määratlemine CreateCampaign meetod, mis hoolitseb varem käivitub EngagementClient abil on lihtne **igal ajal**luua & **teatis ainult** turunduskampaania tiitli ja teade: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Käivitage rakendus konsooli ja kuvatakse järgmine kampaaniat luua.

    **Turunduskampaania Id "159" on loodud ja see on kujul "Mustand"**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
