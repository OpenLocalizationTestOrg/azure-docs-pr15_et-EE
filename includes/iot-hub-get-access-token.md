## <a name="obtain-an-azure-resource-manager-token"></a>Saada Azure ressursihaldur loaga

Azure Active Directory tuleb autentida kõik ülesanded, mida ressursse kasutades Azure ressursihaldur. Nt kasutab parooliga autentimist, muud võimalused vt [Azure ressursihaldur autentimist taotlusi][lnk-authenticate-arm].

1. Lisada järgmine kood **Main** meetod Program.cs märgiks toomiseks Azure AD Rakenduse id ja parooli abil.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Luua **ResourceManagementClient** objekti, mis kasutab luba lisada järgmine kood **Main** meetod lõppu:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Luua või saada viide, kasutatava ressursi rühma:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx