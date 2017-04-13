<properties
    pageTitle="Azure'i AD B2C | Microsoft Azure'i"
    description="Kuidas koostada .NET Web API Azure Active Directory B2C, turvatud OAuth 2.0 Accessi sõned abil autentimise abil."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Koostada .NET veebi-API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C, kus saate secure veebi-API OAuth 2.0 Accessi sõned abil. Nende tähiste luba oma kliendi rakenduste Azure AD B2C API autentida. Selles artiklis kirjeldatakse, kuidas .NET mudeli-vaade – kontrolleril (MVC) "Ülesandeloend" API, mis võimaldab kasutajatel CRUD tööülesannete loomiseks. Web API on turvatud abil Azure AD B2C ja võimaldab ainult autenditud kasutajatel hallata nende Ülesandeloend.

## <a name="create-an-azure-ad-b2c-directory"></a>Mõni Azure AD B2C kataloogi loomine

Enne kui saate kasutada Azure AD B2C, saate luua või rentniku. Kataloogi on ümbris kõigi kasutajate, rakendused, rühmad ja muu. Kui teil pole ühte juba enne [B2C kataloogi loomine](active-directory-b2c-get-started.md) sellest juhendist jätkata.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate B2C kataloogis rakenduse loomine. See annab Azure AD teabest rakenduse turvaliselt suhelda. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md). Kindlasti järgmist.

- Rakenduse **web appi** või **veebi-API** kaasata.
- Kasutage **ümber suunata ressursiidentifikaator** `https://localhost:44316/` web app. See on vaikeasukoha web appi kliendi selle proovi kood.
- Kopeerige **rakenduse ID** , mis määratakse teie rakendus. Peate hiljem.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

Azure'i AD B2C, iga kasutusvõimalused on määratletud [poliitika](active-directory-b2c-reference-policies.md). Kliendi selle proovi kood sisaldab kolme identiteedi kogemusi: registreeruda, logige sisse ja Redigeeri profiili. Peate iga tüübi poliitika loomine [poliitika artiklis](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kirjeldatud. Kui loote oma kolme poliitikad, ärge unustage:

- Valige **Kasutaja ID registreerumise** või **e-posti registreerumise** identiteedi pakkujate tera.
- Valige oma registreerumise poliitika **kuvatav nimi** ja muud registreerumise atribuudid.
- Valige **kuvatav nimi** ja **Objekti ID** taotluste nimega rakenduse nõuded iga poliitika. Soovi korral saate kasutada ka muud nõuded.
- Kopeerige iga poliitika **nimi** pärast selle loomist. Peate need poliitika nimed hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kui olete loonud kolm poliitikad, olete valmis oma rakenduse.

## <a name="download-the-code"></a>Laadige alla kood

See õpetus [säilitatakse github](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)kood. Valimi koostamiseks, kui lähete, saate [alla laadida skelett projekti ZIP-faili](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Samuti saate klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Lõpetatud app on ka [ZIP-faili saadaval](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) või on `complete` haru sama hoidla.

Kui laadite proovi kood, avage Visual Studio .sln fail alustada. Lahendus fail sisaldab kahte projekti: `TaskWebApp` ja `TaskService`. `TaskWebApp`on kasutaja suhtleb MVC veebirakenduse. `TaskService`on rakenduse tagaandmebaas veebi-API, mis salvestab iga kasutaja Ülesandeloend.

## <a name="configure-the-task-web-app"></a>Tööülesande web appi konfigureerimine

Kui kasutaja suhtleb `TaskWebApp`, kliendi saadab taotlused Azure AD ja saab tagasi märgid, mida saab kasutada helistamiseks on `TaskService` web API. Kasutaja sisselogimine ja ülevaate sõned, on vaja sisestada `TaskWebApp` mõned rakenduse teabega. Rakenduses on `TaskWebApp` projekti avatud selle `web.config` root projekti failide ja väärtuste asendamine soovitud `<appSettings>` jaotis.  Saate jätta selle `AadInstance`, `RedirectUri`, ja `TaskServiceUrl` väärtuste-on.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

See artikkel ei sisalda building on `TaskWebApp` klient.  Saate teada, kuidas koostada web appi, kasutades Azure AD B2C, vaadake [meie .NET web appi õpetuse](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Turvaline API

Kui teil on klient, mis nõuab API kasutajate nimel, saate turvaline `TaskService` OAuth 2.0 esitaja sõned abil. Oma API saate aktsepteerida ja kinnitage sõned .net-i (OWIN) teeki Microsofti avatud Web kasutajaliidese abil.

### <a name="install-owin"></a>Installige OWIN
Alustage installimist OWIN OAuthi autentimine müügivõimaluste:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Sisestage oma B2C üksikasjad
Avatud on `web.config` juurkaustas faili selle `TaskService` project ja väärtuste asendamine soovitud `<appSettings>` jaotis. Need väärtused kasutatakse kogu API ja OWIN teek.  Saate jätta selle `AadInstance` väärtus ei muutu.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Lisage soovitud OWIN käivitus klassi
Lisada mõne OWIN käivitus klassi soovitud `TaskService` projekti nimega `Startup.cs`.  Paremklõpsake projekt, valige **Lisa** ja **Uus üksus**ja seejärel otsige OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0 autentimise konfigureerimine
Avage fail `App_Start\Startup.Auth.cs`, ja rakendada selle `ConfigureAuth(...)` meetod:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Turvaline tööülesande domeenikontrolleri
Kui rakendus on konfigureeritud kasutama OAuth 2.0 autentimine, saate oma veebi-API turvaline, lisades mõne `[Authorize]` silti, et tööülesanne kontrolleril. See on lehel, kus kõik ülesannete loendi stringimanipuleerimisfunktsioonide toimub nii, et teil peaks secure kogu domeenikontrolleri klassi tasemel. Saate lisada ka selle `[Authorize]` silti, et üksikute toimingute jaoks kohandatud rohkem võimalusi.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Luba Kasutajateave toomine
`TasksController`tööülesannete talletab andmebaasis, kus iga tööülesande on seotud kasutaja, kellele tööülesanne "kuulub". Omanik on tähistatud kasutaja **objekti ID-d**. (Sellepärast, et teil on vaja lisada objekti ID rakendusena taotluste kõigis oma poliitikate.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Käivitage rakendus näidis

Lõpuks koostamine ja käivitage nii `TaskWebApp` ja `TaskService`. Registreeruge rakenduse abil soovitud meiliaadress või kasutajanimi. Luua kasutaja Ülesandeloend mõningaid ülesandeid ja pöörake tähelepanu sellele, kuidas nad on jätkunud API isegi siis, kui olete lõpetada ja taaskäivitage klient.

## <a name="edit-your-policies"></a>Teie poliitika redigeerimine

Pärast seda, kui teil on kinnitatud API Azure AD B2C abil, saate proovida oma rakenduse poliitikate ja vaadata efektid (või puudub selle) API kohta. Saate rakenduse nõuete poliitika töödelda ja mis on saadaval veebi-API kasutajateabe muutmise. Mis tahes väidab, et lisate on kättesaadav .NET MVC web API funktsiooni `ClaimsPrincipal` objekti, nagu on kirjeldatud selles artiklis varem.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
