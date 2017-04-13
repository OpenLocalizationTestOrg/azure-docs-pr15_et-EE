<properties
    pageTitle="Azure AD .NET alustamine | Microsoft Azure'i"
    description="Kuidas koostada .NET MVC Web App, mis ühendab Azure AD jaoks Logi sisse."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.net-i Web App sisselogimine ja Azure AD Logi välja

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD muudab lihtne ja arusaadav tellida oma veebirakenduse identiteetide haldus, andes ühe sisselogimise ja väljunud vaid paar rida koodi.  ASP.net-i veebirakendustes, saate selle saavutamiseks Microsofti kogukonna juhitud OWIN vahevara kaasatud .NET Framework 4.5 rakendamise abil.  Siin kasutame OWIN abil:
-   Kasutaja kasutamine Azure AD identiteedipakkuja rakendusse sisse logida.
-   Kasutaja kohta leiate teavet kuvada.
-   Logige kasutaja rakenduse välja.

Selleks, peate:

1. Azure AD Rakenduse registreerimine
2. Häälestada rakenduse OWIN autentimise müügivõimaluste kasutamiseks.
3. Sisselogimine ja väljunud taotlusi probleemi Azure AD OWIN abil.
4. Kasutaja andmeid välja printida.

Alustamiseks, [laadige rakendus luukere](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) või [allalaadimine on lõpule viidud valimi](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Peate mõne Azure AD rentniku, kuhu rakenduse registreerida.  Kui teil pole seda juba teinud, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. rakenduse registreerimine Azure AD*
Rakenduse kasutajate autentimiseks lubamiseks esmalt peate registreerida oma rentniku uus rakendus.

- Logige sisse teenusekomplekti Azure haldusportaali.
- Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**.
- Valige rentniku, kuhu soovite rakenduse registreerida.
- Klõpsake vahekaarti **rakendused** ja klõpsake nuppu Lisa alla sahtlis.
- Järgige viipasid ja luua mõne uue **veebirakenduse ja/või WebAPI**.
    - Rakenduse **nimi** kirjeldada oma rakenduse lõppkasutajad
    -   **Sisselogimise URL** on rakenduse base URL.  Funktsiooni skelett vaikimisi `https://localhost:44320/`.
    - **Rakenduse ID URI** on rakenduse jaoks kordumatut tunnust.  Soovite on kasutada `https://<tenant-domain>/<app-name>`, nt`https://contoso.onmicrosoft.com/my-first-aad-app`
- Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü konfigureerimine.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2 häälestamine rakenduse OWIN autentimise müügivõimaluste kasutamiseks*
Siin kuvatakse configure OWIN vahevara kasutama protokolli OpenID Connect autentimist.  OWIN kasutatakse probleemi sisselogimine ja väljunud taotlusi, hallata kasutaja seansi ja kasutajale, muu hulgas kohta teabe saamine.

-   Alustamiseks OWIN vahevara Nugeti pakettide lisada projekti Package Manager konsooli abil.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Lisada hõlmav tund OWIN käivitus nimega projekti `Startup.cs` õige, klõpsake nuppu projekti--> **Lisa** --> **Uue üksuse** --> otsing "OWIN".  Käivitub OWIN vahevara selle `Configuration(...)` meetod rakenduse käivitamisel.
-   Klassi deklaratsiooni muutmine `public partial class Startup` -oleme juba rakendanud see tund osa jaoks teise faili.  Klõpsake soovitud `Configuration(...)` meetod teha kõne ConfgureAuth(...) autentimine veebirakenduse jaoks häälestada  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Avage fail `App_Start\Startup.Auth.cs` ja rakendada selle `ConfigureAuth(...)` meetod.  Esitate parameetrid `OpenIDConnectAuthenticationOptions` toimib koordinaadid oma rakenduse Azure AD suhelda.  Peate ka küpsise autentimine häälestamine – OpenID Connect vahevara küpsiste hõlmab all.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Lõpetuseks, avage soovitud `web.config` root projekti failide ja sisestage oma konfiguratsioon väärtusi on `<appSettings>` jaotis.
    -   Oma rakenduse `ida:ClientId` on Guid kopeeritud Azure portaali etappi 1.
    -   Funktsiooni `ida:Tenant` on Azure AD rentniku, nt "contoso.onmicrosoft.com" nimi.
    -   Teie `ida:PostLogoutRedirectUri` näitab Azure AD, kui kasutaja suunatakse pärast väljunud taotlus on edukalt lõpule jõudnud.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Kasutage OWIN väljaandmisel sisselogimine ja väljunud taotlusi Azure AD*
Rakenduse abil OpenID Connect autentimise protokoll Azure AD suhelda nüüd õigesti konfigureeritud.  OWIN on tehtud eest kõik käsitöö autentimise sõnumeid, sõnet Azure AD kontrollimine ja säilitades kasutaja seansi inetu üksikasjad.  Selles on teie kasutajate jaoks juurdepääsutase võimalus sisse- ja väljalogimine.

- Saate autoriseerida oma kontrollerid nõuda enne teatud lehekülje avamine sisse logib kasutaja silte.  Avatud `Controllers\HomeController.cs`, ja lisage soovitud `[Authorize]` silti, et teave kontrolleril.

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
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Nüüd avage `Views\Shared\_LoginPartial.cshtml`.  See on, kus kuvatakse kasutaja oma rakenduse sisselogimine ja väljunud linkide kuvamiseks ja kasutajanimi vaates välja printida.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
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

## <a name="4--display-user-information"></a>*4. kuvatakse Kasutajateave*
Kui kasutajad OpenID Connect autentimine, tagastab Azure AD on id_token rakendus, mis sisaldab "nõuded" või kinnitused kasutaja kohta.  Saate rakenduse isikupärastamiseks nõuded:

- Avage soovitud `Controllers\HomeController.cs` faili.  Pääsete oma kontrollerid kaudu kasutaja nõuded on `ClaimsPrincipal.Current` turvalisus peamine eesmärk.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Lõpuks luua ja käivitada rakenduse!  Kui te pole seda veel teinud, siis nüüd on aeg luua uus kasutaja oma rentniku koos mõne *. onmicrosoft.com domeeni.  Logige kasutaja ja pöörake tähelepanu sellele, kuidas kasutaja identiteet kajastub ülemisel navigeerimisribal.  Logige välja ja logige uuesti sisse oma rentniku teise kasutajana.  Kui te ei tunne eriti ambitsioonikate, registreerida ja käivitage rakendus (koos eraldi clientId) ning vaadake teemast ühekordse sisselogimise muus eksemplaris tegelikkuses.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [on esitatud allpool](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Nüüd saate teisaldada täpsemate teemade peale.  Võite proovida:

[Turvaline Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
