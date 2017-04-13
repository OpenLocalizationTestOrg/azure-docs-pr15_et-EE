<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure'i"
    description="Kuidas koostada veebirakenduse, mis nõuab veebi-API Azure Active Directory B2C abil."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure'i AD B2C: Kõne veebi-API .NET web Appist

Azure Active Directory (Azure AD) B2C abil saate lisada võimsad iseteenindusliku identiteedi haldusfunktsioonid veebirakenduste ja veebi API-de paari toiminguga. Selles artiklis arutada, kuidas luua .NET mudeli-vaade – kontrolleril (MVC) "Ülesandeloend" web appi, mis nõuab veebi-API esitaja sõned abil

See artikkel ei hõlma kuidas rakendada sisselogimist, registreerumise ja Azure AD B2C profiili juhtimine. Keskendutakse helistaja web API-d, kui kasutaja on juba autenditud. Kui te pole seda veel teinud, peab algama eesliitega [.NET web app töötamise alustamine õpetuse](active-directory-b2c-devquickstarts-web-dotnet.md) Azure AD B2C põhitoimingute kohta.

## <a name="get-an-azure-ad-b2c-directory"></a>Saada on Azure AD B2C kataloog

Enne kui saate kasutada Azure AD B2C, saate luua või rentniku.  Kataloogi on ümbris kõikide kasutajate, rakendused, rühmad ja palju muud.  Kui teil pole ühte juba enne [B2C luua](active-directory-b2c-get-started.md) sellest juhendist jätkata.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate B2C kataloogis rakenduse loomine. See annab Azure AD teabest rakenduse turvaliselt suhelda. Sel juhul web appi nii veebi-API esindab ühe **Rakenduse ID**, kuna need sisaldavad üks loogiline rakendus. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md). Kindlasti järgmist.

- Rakenduse **web app/veebi-API** kaasata.
- Sisestage `https://localhost:44316/` **vastus URL-i**. See on vaikimisi URL-i seda proovi kood.
- Kopeerige **Rakenduse ID** , mis määratakse teie rakendus. On vaja seda hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

Azure'i AD B2C, iga kasutusvõimalused on määratletud [poliitika](active-directory-b2c-reference-policies.md). Selles veebirakenduse sisaldab kolme identiteedi kogemusi: registreeruda, logige sisse ja Redigeeri profiili. Peate looma ühe korra iga tüüpi [poliitika artiklis](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kirjeldatud. Kui loote kolme poliitikad, ärge unustage:

- Valige oma registreerumise poliitika **kuvatav nimi** ja muud registreerumise atribuudid.
- Valige **kuvatav nimi** ja **Objekti ID** rakenduse nõuded iga poliitika. Soovi korral saate kasutada ka muud nõuded.
- Kopeerige iga poliitika **nimi** pärast selle loomist. Sellel peab olema eesliite `b2c_1_`. Peate hiljem poliitika nimed.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kui olete loonud oma kolme poliitikad, olete valmis oma rakenduse.

Pange tähele, et see artikkel ei hõlma äsja loodud poliitikad kasutamise kohta. Teavet selle kohta, kuidas toimivad poliitikad Azure AD B2C, alustage [õpetuse .NET web app töötamise alustamine](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Laadige alla kood

See õpetus [säilitatakse github](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet)kood. Valimi koostamiseks, kui lähete, saate [alla laadida skelett projekti ZIP-faili](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Samuti saate klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Lõpetatud app on ka [ZIP-faili saadaval](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) või on `complete` haru sama hoidla.

Kui laadite proovi kood, avage Visual Studio .sln fail alustada.

## <a name="configure-the-task-web-app"></a>Tööülesande web appi konfigureerimine

Saada `TaskWebApp` Azure AD B2C suhelda tuleb sisestada mõned levinud parameetrid. Rakenduses on `TaskWebApp` projekti avatud selle `web.config` root projekti failide ja väärtuste asendamine soovitud `<appSettings>` jaotis. Saate jätta selle `AadInstance`, `RedirectUri`, ja `TaskServiceUrl` väärtused, nagu need on.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Saada juurdepääsu sõned ja kõne tööülesande API

Selles jaotises arutada, kuidas kasutada web API, mis on ka turvatud Azure AD B2C juurdepääsuks logige sisse Azure AD B2C saadud luba.

See artikkel ei sisalda andmeid, kuidas tagada API. Lisateavet selle kohta, kuidas veebi-API turvaliselt kontrollib taotlusi, kasutades Azure AD B2C, tutvuge [veebi-API alustamine artikkel](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Luba rakendusse salvestamine

Esmalt (abil oma poliitikate mõni) autentida ja Azure AD B2C ühte luba saadud.  Kui te pole kindel, kuidas käivitada poliitikad, minge tagasi ja proovige selle [õpetuse .NET web app töötamise alustamine](active-directory-b2c-devquickstarts-web-dotnet.md) Azure AD B2C põhitoimingute kohta.

Avage fail `App_Start\Startup.Auth.cs`.  On üks oluline muutmine, peate tegema selle `OpenIdConnectAuthenticationOptions` -peate seadma `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

return new OpenIdConnectAuthenticationOptions
{
    // For each policy, give OWIN the policy-specific metadata address, and
    // set the authentication type to the id of the policy
    MetadataAddress = String.Format(aadInstance, tenant, policy),
    AuthenticationType = policy,

    // These are standard OpenID Connect parameters, with values pulled from web.config
    ClientId = clientId,
    RedirectUri = redirectUri,
    PostLogoutRedirectUri = redirectUri,
    Notifications = new OpenIdConnectAuthenticationNotifications
    {
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Saada märgiks kontrolörid

Funktsiooni `TasksController` vastutab suhtlemine veebi-API, API lugeda, luua ja kustutage ülesanded HTTP päringuid saata.  Becuase API on tagatud Azure AD B2C, peate esmalt tuua salvestatud ülaltoodud etappi luba.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

Funktsiooni `BootstrapContext` sisaldab märku, et ostsite oma B2C poliitika täites sisselogimine.

### <a name="read-tasks-from-the-web-api"></a>Loe tööülesannete veebi-API

Kui teil on märgiks, saate selle manustada HTTP `GET` taotleda selle `Authorization` päise turvaliselt helistamiseks `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Loomine ja kustutamine veebi-API toimingud

Järgivad sama saatmisel `POST` ja `DELETE` taotluste veebi-API, kasutades funktsiooni `BootstrapContext` toomiseks luba sisselogimine. Me rakendanud loomine toimingu jaoks. Võite proovida viimistlus kustutamine toimingut `TasksController.cs`.

## <a name="run-the-sample-app"></a>Käivitage rakendus näidis

Lõpuks koostamine ja käivitage rakendus. Logida sisse logida ja sisse logitud kasutajale tööülesannete loomine. Logige välja ja logige sisse mõne teise kasutaja. Kasutaja jaoks tööülesannete loomine. Pange tähele, kuidas tööülesanded on talletatud kasutaja kohta API, kuna API ekstraktib luba saab kasutaja identiteet.

Viite lõplikus näide [on esitatud ZIP-faili](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Te saate ka klooni: GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
