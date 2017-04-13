<properties
    pageTitle="Azure'i AD v2.0 .NET Web Appi | Microsoft Azure'i"
    description="Kuidas koostada .NET MVC Web App, et kasutajad nii isikliku Microsofti Account ja töö-või koolikonto märke."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>.NET MVC web app sisselogimine lisamine

V2.0 lõpp-punkti abil saate kiiresti lisada oma veebirakenduste, mis toetab nii isikliku Microsofti kontod autentimis- ja töö-või koolikonto.  ASP.net-i veebirakendustes, saate selle saavutamiseks Microsofti OWIN vahevara kaasatud .NET Framework 4.5 abil.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

 Siin me tuleb koostada web appi, mis kasutab OWIN kasutaja sisse logida, kuvatakse kasutaja kohta leiate teavet ja logige kasutaja välja rakenduse.
 
 ## <a name="download"></a>Laadi alla
Selle õpetuse kood säilitatakse [github](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) - või klooni skelett:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Lõpetatud app on esitatud ka õppeteema lõpus.

## <a name="register-an-app"></a>Rakenduse registreerimine
Luua uue rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige neid [üksikasjalikke juhiseid](active-directory-v2-app-registration.md).  Veenduge, et:

- Kopeeri allapoole määratud rakenduse **Rakenduse Id** peate selle varsti.
- Lisage oma rakenduse **Web** platvormi.
- Sisestage õige **URI ümber suunata**. Redirect uri näitab, kus autentimine vastuste peaks teid suunatakse – selles õpetuses vaikeväärtus Azure AD `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installimine ja OWIN autentimise konfigureerimine
Siin kuvatakse configure OWIN vahevara kasutama protokolli OpenID Connect autentimist.  OWIN kasutatakse probleemi sisselogimine ja väljunud taotlusi, hallata kasutaja seansi ja kasutajale, muu hulgas kohta teabe saamine.

-   Alustamiseks avage soovitud `web.config` projekti juurkaustas fail ja sisestage oma rakenduse konfigureerimise väärtused on `<appSettings>` jaotis.
    -   Funktsiooni `ida:ClientId` on **Rakenduse Id** määratud rakenduse registreerimise portaalis.
    -   Funktsiooni `ida:RedirectUri` on sisestatud portaalis **Uri ümber suunata** .

-   Järgmiseks lisada projekti Package Manager konsooli abil OWIN vahevara NuGet-paketid.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Lisada mõne "OWIN käivitus Class" projekti nimega `Startup.cs` õige, klõpsake nuppu projekti--> **Lisa** --> **Uue üksuse** --> otsing "OWIN".  Käivitub OWIN vahevara selle `Configuration(...)` meetod rakenduse käivitamisel.
-   Klassi deklaratsiooni muutmine `public partial class Startup` -oleme juba rakendanud see tund osa jaoks teise faili.  Klõpsake soovitud `Configuration(...)` meetod teha kõne ConfigureAuth(...) autentimine veebirakenduse jaoks häälestada  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Avage fail `App_Start\Startup.Auth.cs` ja rakendada selle `ConfigureAuth(...)` meetod.  Esitate parameetrid `OpenIdConnectAuthenticationOptions` toimib koordinaadid oma rakenduse Azure AD suhelda.  Peate ka küpsise autentimine häälestamine – OpenID Connect vahevara küpsiste hõlmab all.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Päringuid autentimine
Rakenduse abil OpenID Connect autentimise protokoll v2.0 lõpp-punkti suhelda nüüd õigesti konfigureeritud.  OWIN on tehtud eest kõik käsitöö autentimise sõnumeid, kontrollimine sõnet Azure AD ja säilitades kasutaja seansi inetu üksikasjad.  Selles on teie kasutajate jaoks juurdepääsutase võimalus sisse- ja väljalogimine.

- Saate autoriseerida oma kontrollerid nõuda enne teatud lehekülje avamine sisse logib kasutaja sildid.  Avatud `Controllers\HomeController.cs`, ja lisage soovitud `[Authorize]` silti, et teave kontrolleril.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   OWIN abil saate otse oma koodi probleemi pärit.  Avatud `Controllers\AccountController.cs`.  SignIn() ja SignOut() toimingud välja OpenID Connect ülesanne ja väljunud taotlusi, vastavalt.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Nüüd avage `Views\Shared\_LoginPartial.cshtml`.  See on, kus kuvatakse kasutaja oma rakenduse sisselogimine ja väljunud linkide kuvamiseks ja kasutajanimi vaates välja printida.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="display-user-information"></a>Kasutaja teabe kuvamine
Kui kasutajad OpenID Connect autentimine, tagastab v2.0 lõpp-punkti on id_token rakendus, mis sisaldab [taotluste](active-directory-v2-tokens.md#id_tokens)või kinnitused kasutaja kohta.  Nende nõuete abil saate oma rakenduse isikupärastamine:

- Avage soovitud `Controllers\HomeController.cs` faili.  Pääsete oma kontrollerid kaudu kasutaja nõuded on `ClaimsPrincipal.Current` turvalisus peamine eesmärk.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Käivita

Lõpuks luua ja käivitada rakenduse!   Logige sisse töökoha või kooli kontoga või Microsofti Account ja pöörake tähelepanu sellele, kuidas kasutaja identiteet kajastub ülemisel navigeerimisribal.  Nüüd on teil on tagatud kasutades valdkonna standard protokoll saate kasutajate oma isiklike ja töö/kool kontode autentimiseks web app.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [soovitud ZIP siin on saadaval](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)või saate klooni seda GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Järgmised sammud

Nüüd saate teisaldada täpsemate teemade peale.  Võite proovida:

[Turvaline Web API abil soovitud v2.0 lõpp-punkti >>](active-directory-devquickstarts-webapi-dotnet.md)

Täiendavad ressursid, vaadake:
- [V2.0 arendaja juhend >>](active-directory-appmodel-v2-overview.md)
- [Sildi "azure-active directory" StackOverflow >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
