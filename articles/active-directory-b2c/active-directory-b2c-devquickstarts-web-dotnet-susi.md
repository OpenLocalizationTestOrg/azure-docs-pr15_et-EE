<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure'i"
    description="Kuidas koostada veebirakendus, mis on sisse logida, sisselogimise ja parooli lähtestamise Azure Active Directory B2C abil."
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

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure'i AD B2C: Registreerumise ja ASP.net-i Web App sisselogimine

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C abil saate lisada oma web app paari toiminguga võimsaid iseteenindusliku identiteedi haldusfunktsioonid. Selles artiklis arutada, kuidas luua ASP.net-i veebirakenduse, mis sisaldab kasutaja sisse logida, sisselogimise ja parooli lähtestada. Rakendus sisaldab tugi registreerumise ja logige sisse, kasutades kasutajanimi või e-posti ja suhtlusvõrgu kontod nagu Facebookis ja Google abil.

Selles õpetuses erineb [meie muude .net-i juhend](active-directory-b2c-devquickstarts-web-dotnet.md) , et see kasutab [registreerumist või sisselogimine poliitika](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) pakuvad kasutajaks registreerumine & sisselogimine ühe nupu, kasutamise asemel kaks (üks registreerumise ja üks Logi sisse).  Lühidalt registreerumise või Logi sisse poliitika võimaldab kasutajatel logige sisse olemasolevasse kontosse, kui nad on üks või looge uus, kui see on oma rakenduse kasutamine esimest korda.

## <a name="get-an-azure-ad-b2c-directory"></a>Saada on Azure AD B2C kataloog

Enne kasutamist Azure AD B2C, saate luua või rentniku. Kataloogi on ümbris kõigi kasutajate, rakendused, rühmad ja muu.  Kui teil pole ühte juba enne [B2C luua](active-directory-b2c-get-started.md) sellest juhendist jätkata.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate B2C kataloogis rakenduse loomine. See annab Azure AD teabest rakenduse turvaliselt suhelda. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md).  Kindlasti järgmist.

- Rakenduse **web app/veebi-API** kaasata.
- Sisestage `https://localhost:44316/` nimega **URI ümber suunata**. See on vaikimisi URL-i seda proovi kood.
- Kopeerige **Rakenduse ID** , mis määratakse teie rakendus alla.  Teil on vaja hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

Azure'i AD B2C, iga kasutusvõimalused on määratletud [poliitika](active-directory-b2c-reference-policies.md). Seda proovi kood sisaldab identiteedi kahes versioonis: **registreerumise ja sisselogimisaadressi**ja **parooli lähtestamine**.  Peate looma ühe korra iga tüüpi [poliitika artiklis](active-directory-b2c-reference-policies.md)kirjeldatud. Kui loote kahes poliitikad, kindlasti järgmist.

- Valige **Kasutaja ID registreerumise** või **e-posti registreerumise** identiteedi pakkujate tera.
- Valige oma registreerumise & sisselogimise poliitika **kuvatav nimi** ja muud registreerumise atribuudid.
- Valige **kuvatav nimi** taotluste rakendusena nõude iga poliitika. Soovi korral saate kasutada ka muud nõuded.
- Kopeerige iga poliitika **nimi** pärast selle loomist. Peate hiljem poliitika nimed.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kui olete loonud oma kaks poliitikat, olete valmis oma rakenduse.

## <a name="download-the-code-and-configure-authentication"></a>Laadige alla koodi ja autentimise konfigureerimine

[Github säilitatakse](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI)see proovi kood. Valimi koostamiseks, kui lähete, saate [alla laadida skelett projekti ZIP-faili](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Samuti saate klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Lõplikus valim on ka [ZIP-faili saadaval](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) või on `complete` haru sama hoidla.

Kui laadite proovi kood, avage Visual Studio .sln fail alustada.

Rakenduse suhtleb Azure AD B2C saatmisega HTTP autentimise päringuid, mis soovib käivitada taotluse osana poliitika määramine. .Net-i veebirakenduste, saate Microsofti OWIN teegi OpenID Connect autentimise päringuid, käivitada poliitikad, hallata kasutaja seansid ja palju muud.

Alustamiseks lisada OWIN vahevara Nugeti pakettide projekti Visual Studio Package Manager konsooli abil.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Järgmiseks avage soovitud `web.config` root projekti failide ja sisestage oma rakenduse konfigureerimise väärtused on `<appSettings>` jaotis, asendades all väärtused ise.  Võib-olla jätate selle `ida:RedirectUri` ja `ida:AadInstance` väärtused on, muutmata.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Järgmiseks lisada mõne OWIN käivitus klassi nimega projekti `Startup.cs`. Paremklõpsake projekt, valige **Lisa** ja **Uus üksus**ja seejärel otsige "OWIN." Muuta ainekursuse deklaratsiooni `public partial class Startup`. Me rakendada selle klassi osa teie jaoks muu faili. Käivitub OWIN vahevara selle `Configuration(...)` meetod rakenduse käivitamisel. Selle meetodi helistada `ConfigureAuth(...)`, kus saate seadistada autentimise oma rakenduse.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Avage fail `App_Start\Startup.Auth.cs` ja rakendada selle `ConfigureAuth(...)` meetod.  Esitate parameetrid `OpenIdConnectAuthenticationOptions` olla koordinaadid oma rakenduse Azure AD suhelda. Samuti vajate häälestamiseks küpsise autentimine. OpenID Connect vahevara küpsiste säilitamiseks kasutaja seansi muu hulgas.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
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
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Azure AD autentimise päringuid saata
Rakenduse suhelda Azure AD B2C OpenID Connect autentimine protokolli abil nüüd õigesti konfigureeritud.  OWIN on tehtud eest kõik käsitöö autentimise sõnumeid, sõnet Azure AD kontrollimine ja säilitades kasutaja seansi üksikasjad.  Selles on iga kasutaja meilivoo algatada.

Kui kasutaja valib **Login** või **olete parooli unustanud?** web Appis seostatud toimingu rakendatakse `Controllers\AccountController.cs`. Korral saate kasutada sisseehitatud OWIN meetodite õige poliitika käivitamiseks:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Teostamise registreerumise käigus või poliitika sisse logida, kui kasutajal on võimalus klõpsake mõnda **olete parooli unustanud?** link.  Sellisel juhul saadab Azure AD B2C rakenduse teatud tõrketeade, mis näitab, et see peaks käivitada parooli lähtestamiseks poliitika.  Saate hõivata see viga `Startup.Auth.cs` abil soovitud `AuthenticationFailed` teade:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Lisaks konkreetselt kasutada poliitika, saate kasutada ka `[Authorize]` käivitatakse poliitika, kui kasutaja on sisse logitud oma domeenikontrollerid silti. Avatud `Controllers\HomeController.cs` ja lisage soovitud `[Authorize]` silti, et taotluste kontrolleril.  Valib OWIN Viimane poliitika konfigureeritud, millal on `[Authorize]` silt on tulemus.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

OWIN abil saate kasutaja rakendusest väljalogimine. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Kasutaja teabe kuvamine
Kui kasutajate autentimiseks, kasutades OpenID ühendus, tagastab Azure AD rakendus, mis sisaldab **taotluste**mõne ID luba. Need on kinnitused kasutaja kohta. Saate rakenduse isikupärastamiseks taotluste.  

Avage soovitud `Controllers\HomeController.cs` faili. Pääsete oma kontrollerid kaudu kasutaja nõuded on `ClaimsPrincipal.Current` turvalisus peamine eesmärk.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Saate kasutada kõiki, mis rakenduse saab samal viisil.  Loendi kõigi nõuete rakenduse saab on saadaval **taotluste** lehel.

## <a name="run-the-sample-app"></a>Käivitage rakendus näidis

Lisaks saate luua ja käivitada rakenduse. Registreeruge rakenduse, kasutades e-posti aadress või kasutaja nimi. Logige välja ja logige uuesti sisse sama kasutajana. Teise kasutaja profiili redigeerimine. Logige välja ja teise kasutajana sisse logida. Pange tähele, et **taotluste** vahekaardil kuvatud teave vastab teave teie poliitika kohta konfigureeritud.

## <a name="add-social-idps"></a>Lisage social sisepagulaste reintegreerimine

Rakendus toetab praegu, vaid kasutaja sisse logida ja logige sisse, kasutades **kohalikud kontod**. Need on talletatakse teie B2C kontod, mis kasutavad kasutajanime ja parooli. Azure'i AD B2C abil saate lisada muid **identiteedipakkujad** (IDPs) oma koodi muutmata.

Rakenduse suhtlusvõrgu sisepagulaste reintegreerimine lisamiseks alustage artiklitest üksikasjalikke juhiseid järgides. Iga IDP soovite toetada, tuleb teil registreerida rakenduse, et süsteemi ja saada kliendi ID-ga.

- [Kui mõni IDP Facebooki häälestamine](active-directory-b2c-setup-fb-app.md)
- [Kui mõni IDP Google häälestamine](active-directory-b2c-setup-goog-app.md)
- [Kui mõni IDP Amazon häälestamine](active-directory-b2c-setup-amzn-app.md)
- [Kui ka IDP LinkedIni häälestamine](active-directory-b2c-setup-li-app.md)

Pärast selle identiteedipakkujad B2C kataloogi lisamist peate iga uue ümberasustatud isikule, lisada oma kolme poliitika redigeerimine [poliitika artiklis](active-directory-b2c-reference-policies.md)kirjeldatud. Pärast seda, kui salvestate oma poliitikad, käivitage rakendus uuesti.  Peaksite nägema uue IDPs, mis on lisatud sisselogimise ja kogemusi registreerumise suvandid iga teie identiteedi.

Saate oma poliitikate katsetada ja jälgida, kuidas oma valimi rakenduse. Lisamine või eemaldamine sisepagulaste reintegreerimine, töödelda rakenduse nõuded või registreerumise atribuute muuta. Proovida, kuni näete, kuidas poliitika, taotluste autentimise ja OWIN siduda.

Viide, lõpule viidud valimi (ilma väärtuste määramine) [on esitatud ZIP-faili](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Samuti saate selle klooni GitHub kaudu:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
