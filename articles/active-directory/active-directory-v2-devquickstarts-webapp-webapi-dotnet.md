<properties
    pageTitle="Azure'i AD v2.0 .net-i veebirakenduse | Microsoft Azure'i"
    description="Koostamiseks .NET MVC Web App, et kõned web services abil isikliku Microsofti kontod ja töö või kooli kontod Logi sisse."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Veebi-API .NET web Appist helistamiseks

V2.0 lõpp-punkti saate kiiresti lisada oma veebirakenduste autentimist ja web API-de tugi mõlema isikliku Microsofti konto ja töö-või koolikonto.  Siin me tuleb koostada MVC web appi, et märke kasutajad Microsofti OWIN vahevara abi OpenID Connect abil.  Veebirakenduse sõned OAuth 2.0 juurdepääsu saamiseks web api tagatud OAuth 2.0, mis võimaldab luua, lugeda ja Kustuta antud kasutaja "Ülesandeloend".

Selle õpetuse keskendub peamiselt omandada ja kasutada Accessi sõned web Appis, täielik [siin](active-directory-v2-flows.md#web-apps)kirjeldatud MSAL abil.  Eeltingimused, kui soovite esmalt saate teada, kuidas [lisada lihtsa sisselogimine web appi](active-directory-v2-devquickstarts-dotnet-web.md) või kuidas [tagada õigesti veebi-API](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Proovi kood allalaadimine

Selle õpetuse kood säilitatakse [github](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) - või klooni skelett:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Teise võimalusena saate, [nagu on zip lõplikus rakenduse alla laadida](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) või klooni lõpetatud app:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Rakenduse registreerimine
Luua uue rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige neid [üksikasjalikke juhiseid](active-directory-v2-app-registration.md).  Veenduge, et:

- Kopeeri allapoole määratud rakenduse **Rakenduse Id** peate selle varsti.
- **Rakenduse salajane** , tüüpi **parooli** loomine ja hiljem alla väärtusega kopeerimine
- Lisage oma rakenduse **Web** platvormi.
- Sisestage õige **URI ümber suunata**. Redirect uri näitab, kus autentimine vastuste peaks teid suunatakse – selles õpetuses vaikeväärtus Azure AD `https://localhost:44326/`.


## <a name="install-owin"></a>Installige OWIN
Lisage OWIN vahevara Nugeti pakette selle `TodoList-WebApp` project Package Manager konsooli abil.  Sisselogimine ja väljunud taotlusi probleemi, hallata kasutaja seansi ja muu hulgas kasutaja kohta teabe saamiseks kasutatakse OWIN vahevara.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Kasutaja sisselogimine
Nüüd konfigureerida kasutama [OpenID Connect autentimise protokoll](active-directory-v2-protocols.md#openid-connect-sign-in-flow)OWIN vahevara.  

-   Avatud on `web.config` juurkaustas faili selle `TodoList-WebApp` project ja sisestage oma rakenduse konfigureerimise väärtused on `<appSettings>` jaotis.
    -   Funktsiooni `ida:ClientId` on **Rakenduse Id** määratud rakenduse registreerimise portaalis.
    - Funktsiooni `ida:ClientSecret` on **Rakenduse salajane** loodud registreerimise portaalis.
    -   Funktsiooni `ida:RedirectUri` on sisestatud portaalis **Uri ümber suunata** .
- Avatud on `web.config` juurkaustas faili selle `TodoList-Service` project ja asendada selle `ida:Audience` nagu eespool kirjeldatud sama **Rakenduste Id** -ga.


- Avage fail `App_Start\Startup.Auth.cs` ja lisage `using` laused ülevalt teekide jaoks.
- Rakendada sama faili selle `ConfigureAuth(...)` meetod.  Esitate parameetrid `OpenIDConnectAuthenticationOptions` toimib koordinaadid oma rakenduse Azure AD suhelda.

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
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>MSAL abil saate Accessi sõned
Klõpsake soovitud `AuthorizationCodeReceived` teatis, soovime kasutada [OAuth 2.0 koos OpenID Connect](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) lunastamiseks authorization_code juurdepääsu sümboolse teenusesse ülesannete loendi jaoks.  MSAL aitavad selle protsessi lihtne:

- Installige kõigepealt MSAL eelvaateversiooni.

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- Ja lisada teise `using` lause, et selle `App_Start\Startup.Auth.cs` MSAL faili.
- Nüüd lisage uus meetod, on `OnAuthorizationCodeReceived` sündmuseohjuri.  Selle sündmuseohjuri kasutatakse MSAL omandada juurdepääsu sümboolse ülesannete loendi API ja salvestab luba MSAL's Turbeloa vahemälu hiljem.

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- Veebirakenduste, MSAL on laiendatav Turbeloa vahemälu kasutatavate märkide talletamiseks.  See näide rakendab on `NaiveSessionCache` mis kasutab http seansi salvestusruumi vahemälu sõned abil.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Kõne veebi-API
Nüüd on aeg tegelikult kasutada access_token, ostsite sammus 3.  Avage web appi `Controllers\TodoListController.cs` faili, mis teeb CRUD taotlused ülesannete loendi API-ga.

- Saate MSAL uuesti siin access_tokens tuua MSAL vahemälu.  Esmalt lisage soovitud `using` avalduse MSAL seda faili.

    `using Microsoft.Identity.Client;`

- Klõpsake soovitud `Index` toimingu, kasutage MSAL `AcquireTokenSilentAsync` meetod, et saada access_token, mida saab kasutada andmete lugemiseks Ülesandeloend teenuse:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Valimi lisab tulemuseks sõnet funktsiooni HTTP GET-päring nimega soovitud `Authorization` päis, mis Ülesandeloend teenuse kasutab taotluse kinnitamiseks.
- Kui Ülesandeloend teenus tagastab soovitud `401 Unauthorized` vastust access_tokens sisse MSAL enam ei sobi mingil põhjusel.  Sel juhul tuleks kukutage mis tahes access_tokens MSAL vahemälu ja kasutaja kuvada teade, et nad võivad vajada uuesti, mis Turbeloa omandamise voogu uuesti sisse logida.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Samamoodi kui MSAL ei saa tagastada ka access_token mingil põhjusel, peaks paluge kasutajal uuesti sisse logida.  See on sama lihtne nagu mis tahes püüdmine `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Funktsiooni täpselt sama `AcquireTokenSilentAsync` kõne on implementd klõpsake soovitud `Create` ja `Delete` toimingud.  Veebirakenduste, saate seda meetodit MSAL saada access_tokens iga kord, kui neid vajate rakenduse.  MSAL hoolitseb hankimisega, vahemällu talletamine ja värskendamine sõned teie eest.

Lõpuks luua ja käivitada rakenduse!  Logige sisse Microsofti Account või konto Azure AD ja pöörake tähelepanu sellele, kuidas kasutaja identiteet kajastub ülemisel navigeerimisribal.  Lisamine ja kasutaja Ülesandeloend OAuth 2.0 turvatud API kõned toimingu kuvamiseks mõnda üksust kustutada.  Nüüd on teil web appi ja veebi-API, mõlemad tagatud kasutades valdkonna standard protokoll, saate oma isiklike ja töö/kool kontodega kasutajad autentida.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [on esitatud allpool](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Järgmised sammud

Täiendavad ressursid, vaadake:
- [V2.0 arendaja juhend >>](active-directory-appmodel-v2-overview.md)
- [Sildi "azure-active directory" StackOverflow >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
