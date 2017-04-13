<properties
    pageTitle="Azure'i AD v2.0 .net-i Veebiteenuste | Microsoft Azure'i"
    description="Kuidas koostada .NET MVC Web Api, mida aktsepteerib sõned nii isikliku Microsofti Account ja töökoha või õppeasutuse konto."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Turvaline on MVC veebi-API

Azure Active Directory v2.0 lõpp-punkti, saate kaitsta Web API abil [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) Accessi sõned, mis võimaldab kasutajatel nii isikliku Microsofti kontoga ja töö või kooli kontod turvaline juurdepääs veebi-API-ga.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

ASP.net-i Web API-d, saate selle saavutamiseks Microsofti OWIN vahevara kaasatud .NET Framework 4.5 abil.  Siin kasutame OWIN koostamiseks "Loendite" MVC Web API, mis võimaldab klientidel luua ja lugeda tööülesannete kasutaja Ülesandeloend.  Veebi-API kinnitada, et sissetulevad taotlused sisaldavad lubatud juurdepääsu luba ja hüljata taotlused, mis edastavad valideerimine kaitstud liini.  See näide ehitatud Visual Studio 2015 abil.

## <a name="download"></a>Laadi alla
Selle õpetuse kood säilitatakse [github](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) - või klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Skelett rakendus sisaldab lihtsa API trafaretset koodi, kuid on kadunud identiteedi seotud andmeühikuga. Kui te ei soovi, et jälgida, saate selle asemel klooni või [lõpuleviidud valimi alla laadida](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Rakenduse registreerimine
Luua uue rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige neid [üksikasjalikke juhiseid](active-directory-v2-app-registration.md).  Veenduge, et:

- Kopeeri allapoole määratud rakenduse **Rakenduse Id** peate selle varsti.

Selle visual studio lahendus sisaldab ka on "TodoListClient", mis on lihtne WPF rakendus.  Funktsiooni TodoListClient saab näidata, kuidas kasutaja märgid sisse ja kuidas saate kliendi probleemi taotlusi veebi-API-ga.  Sel juhul on TodoListClient nii on TodoListService esindavad sama rakendus.  Funktsiooni TodoListClient konfigureerimiseks peate ka:

- Lisage oma rakenduse **Mobile** platvormi.


## <a name="install-owin"></a>Installige OWIN

Nüüd, kui olete registreerunud rakendus, peate häälestamine rakenduse valideerimiseks, sissetulevad taotlused ja sõned v2.0 lõpp-punkti suhelda.

- Alustamiseks avage lahendus ja lisada OWIN vahevara Nugeti pakettide TodoListService projekti Package Manager konsooli abil.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuthi autentimise konfigureerimine

- Lisada mõne tunni OWIN käivitus TodoListService projekti nimega `Startup.cs`.  Õige, klõpsake nuppu projekti--> **Lisa** --> **Uue üksuse** --> otsing "OWIN".  Käivitub OWIN vahevara selle `Configuration(…)` meetod rakenduse käivitamisel.
- Klassi deklaratsiooni muutmine `public partial class Startup` -oleme juba rakendanud see tund osa jaoks teise faili.  Klõpsake soovitud `Configuration(…)` meetod teha kõne ConfgureAuth(...) autentimine veebirakenduse jaoks häälestamiseks.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Avage fail `App_Start\Startup.Auth.cs` ja rakendada selle `ConfigureAuth(…)` meetod, mis loob Web API v2.0 lõpp-punkti sõnet kinnitamiseks.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Nüüd saate `[Authorize]` atribuute kaitsta teie kontrollerid ja OAuth 2.0 esitaja autentimise toimingud.  Kaunistamine on `Controllers\TodoListController.cs` klassi mõne Autoriseerin silt.  See sunnib kasutaja enne selle lehe sisse logida.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Kui mõni volitatud helistaja edukalt viitab üks soovitud `TodoListController` API-d, peate toimingu helistaja teave.  OWIN pakub juurdepääsu taotluste esitaja luba kaudu sees on `ClaimsPrincpal` objekti.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Lõpetuseks, avage soovitud `web.config` root TodoListService projekti failide ja sisestage oma konfiguratsioon väärtusi on `<appSettings>` jaotis.
  - Teie `ida:Audience` on **Rakenduse Id** rakenduse portaalis sisestatud.

## <a name="configure-the-client-app"></a>Kliendi rakenduse konfigureerimiseks
Enne tegelikkuses teenuse Todo loendi kuvamiseks peate Todo loendi kliendi konfigureerida nii, et see saab sõned toomine v2.0 lõpp-punkti ja teenuse helistada.

- TodoListClient projekt, avage `App.config` ja sisestage oma konfiguratsioon väärtused on `<appSettings>` jaotis.
  - Teie `ida:ClientId` rakenduse Id kopeeritud portaalist.

Lõpuks puhastada, luua ja käivitada iga projekti!  Nüüd on teil .NET MVC Web API, mida aktsepteerib sõned nii isikliku Microsofti konto kaudu ja töökoha või õppeasutuse konto.  Logige soovitud TodoListClient ja kõne veebi api tööülesanded lisamiseks kasutaja Ülesandeloend.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [soovitud ZIP siin on saadaval](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)või saate klooni seda GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Järgmised sammud
Nüüd saate teisaldada täiendavad teemade peale.  Võite proovida:

[Veebi-API Web Appist helistamiseks >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Täiendavad ressursid, vaadake:
- [V2.0 arendaja juhend >>](active-directory-appmodel-v2-overview.md)
- [Sildi "azure-active directory" StackOverflow >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
