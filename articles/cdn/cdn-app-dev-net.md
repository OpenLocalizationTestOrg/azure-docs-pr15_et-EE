<properties
    pageTitle="Alustamine Azure'i CDN teegi .net-i jaoks | Microsoft Azure'i"
    description="Saate teada, kuidas kirjutada haldamiseks Azure'i CDN abil Visual Studio .net-i rakendusi."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure'i CDN arengu kasutamise alustamine

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET-I](cdn-app-dev-net.md)

[Azure'i CDN teegi .net-i](https://msdn.microsoft.com/library/mt657769.aspx) abil automatiseerida loomist ja haldamist CDN profiilid ja lõpp-punktid.  Selles õppeteemas tutvustatakse loomine lihtne .net-i konsooli rakendus, mis näitab, mitu toimingud.  Selles õpetuses eesmärk ei kirjeldata kõigis Azure'i CDN teegi .net-i jaoks.

Peate Visual Studio 2015 selles õpetuses lõpuleviimiseks.  [Visual Studio ühenduse 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) on allalaadimiseks saadaval.

> [AZURE.TIP] [Lõpetatud projekti selles õpetuses](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) on saadaval MSDN-i alla laadida.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Projekti loomine ja lisamine Nuget-paketid

Nüüd, kui oleme loodud ressursirühma meie CDN profiilid ja antud Azure AD rakendusel haldamiseks CDN profiilid ja selles rühmas lõpp-punktid, alustame meie teenuserakenduse loomine.

Jooksul Visual Studio 2015, klõpsake **faili**, **New** **Project...** uue projekti dialoogiboksi avamiseks.  Laiendage **Visual C#**ja seejärel valige vasakul paanil **Windows** .  Klõpsake keskmisel paanil **Konsooli rakendus** .  Projekti nime ja seejärel klõpsake nuppu **OK**.  

![Uue projekti](./media/cdn-app-dev-net/cdn-new-project.png)

Meie projekti saab kasutada mõne Azure teeke sisalduvate Nuget-paketid.  Lisada need, et projekt.

1. Klõpsake menüü **Tööriistad** , **Nugeti Package Manager**, seejärel **Package Manager konsooli**.

    ![Nugeti pakettide haldamine](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Konsooli paketi Manager käivitada installimise **Active Directory autentimise Raamatukogu (ADAL)**järgmine käsk:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Käivita installida **Azure CDN teegis**järgmist:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Suuniseid, konstandid, peamine meetod ja helper meetodid

Vaatame saada põhistruktuur meie kirjutatud programm.

1. Tagasi menüü Program.cs asendage funktsiooni `using` suuniseid ülaosas järgmist:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Tuleb määratleda mõned konstantide meie meetodid kasutatakse.  Klõpsake soovitud `Program` klassi, kuid enne soovitud `Main` meetod, lisage järgmine tekst.  Asendage kohatäited, sh selle ** &lt;vahele&gt;**, vastavalt vajadusele oma väärtustega.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Klassi tasemel määratleda neid kahte muutujat.  Me kasutame neid hiljem kindlaks teha, kui meie profiili ja lõpp-punkti juba olemas.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Asendada selle `Main` meetod järgmiselt:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Mõne muu meetodi meie kavatsete Küsi "Jah/ei" küsimused ja kasutajale.  Lisage hõlbustamiseks mis pisut järgmisel viisil:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nüüd, kui on kirjutatud meie programmi põhistruktuur, loome meetodid nimetatakse funktsiooni `Main` meetod.

## <a name="authentication"></a>Autentimine

Me ei saaks kasutada Azure CDN juhtimine teek, läheb vaja meie teenuse põhilise autentimiseks ja saada mõne autentimise luba.  See meetod kasutab ADAL toomiseks luba.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Kui kasutate kasutaja autentimine on `GetAccessToken` meetod, mis näeb välja veidi teistsugused.

>[AZURE.IMPORTANT] Kui otsustasite üksikute kasutaja autentimise asemel peamine teenus on kasutada ainult seda proovi kood.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Asendage `<redirect URI>` koos redirect URI Azure AD Rakenduse registreerimisel sisestatud.

## <a name="list-cdn-profiles-and-endpoints"></a>Loendi CDN profiilid ja lõpp-punktid

Nüüd olete valmis CDN toimingute tegemiseks.  Esimese asjana meie meetod ei on loend Profiilid ja meie ressursirühm lõpp-punktid ja meie konstantide määratud profiili ja lõpp-punkti nimed vaste leidmisel teeb märkme selle hiljem, nii et me ei proovite luua duplikaate.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profiilid ja lõpp-punktid loomine

Järgmiseks loome profiili.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Kui profiili on loodud, loome lõpp.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Ülaltoodud näites määratakse lõpp-punkti nimega *Contoso* hostinimi päritolu `www.contoso.com`.  Peaksite muutma selle osutamiseks oma origin hostname (hostinimi).

## <a name="purge-an-endpoint"></a>Likvideerite lõpp

Eeldades, et lõpp-punkti on loodud, on levinud ühe tööülesande, mis võib tahame teha meie programmi puhastamine meie lõpp-punkti sisu.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] Stringi Ülaltoodud näites `/*` tähistab soovin likvideerite kõik juurkaustas lõpp-punkti tee.  See on võrdne kontrollimine **Likvideerite kõik** Azure portaali "likvideerite" dialoogiboks. Klõpsake soovitud `CreateCdnProfile` meetod loodud meie profiili **Azure'i CDN Verizon** profiili, mis koodi kasutamise `Sku = new Sku(SkuName.StandardVerizon)`, nii et see on eduka.  Siiski ei toeta **Azure CDN kaudu Akamai** profiilid **Likvideerite kõik**, nii, et kui mul on kasutades Akamai profiil on selles õpetuses, ma pean teatud teid puhastada.

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profiilid ja lõpp-punktid kustutamine

Viimase meetodite kustutatakse meie lõpp-punkti ja profiili.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Programmi

Nüüd saame kompileerida ja programmi käivitamiseks klõpsake nuppu **Start** , Visual Studios.

![Programm töötab](./media/cdn-app-dev-net/cdn-program-running-1.png)

Kui programm jõuab ülaltoodud viip, peaks oskama ressursirühma Azure portaali naasmiseks ja leiate, et profiili on loodud.

![Edu!](./media/cdn-app-dev-net/cdn-success.png)

Kinnitame saab siis käivitage programm ülejäänud juhised.

![Programmi lõpuleviimine](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Järgmised sammud

Kui soovite vaadata lõpetatud projekti jaotistes, [Laadige alla valimi](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c)põhjal.

Vaadata täiendavaid dokumente Azure'i CDN haldamine dokumenditeegis .net-i jaoks leiate [MSDN-i viide](https://msdn.microsoft.com/library/mt657769.aspx).

Saate hallata oma CDN ressursse [PowerShelli abil](./cdn-manage-powershell.md).


