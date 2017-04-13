<properties
    pageTitle="Azure AD .NET alustamine | Microsoft Azure'i"
    description="Kuidas koostada .NET MVC Web API, mis ühendab Azure AD autentimise ja luba."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Kaitse Web API-ga esitaja sõnet Azure AD abil

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Kui olete hoone rakendus, mis võimaldab juurdepääsu kaitstud ressursse soovite teada, kuidas kaitsta liigseid juurdepääsu nende ressursse.
Azure AD muudab lihtne ja arusaadav veebi-API, mis on vaid mõned koodiread OAuthi esitaja 2.0 Accessi sõned abil kaitsta.

ASP.net-i veebirakendustes, saate selle saavutamiseks Microsofti ühenduse juhitud OWIN vahevara kaasatud .NET Framework 4.5 rakendamise abil.  Siin kasutame OWIN, et koostada "Loendite" veebi-API, mis:
-   Määrab, millised API on kaitstud.
-   Kinnitab, et Web API kõned sisaldavad lubatud juurdepääs Turbeloa.

Selleks, peate:

1. Azure AD Rakenduse registreerimine
2. Häälestada rakenduse OWIN autentimise müügivõimaluste kasutamiseks.
3. Klientrakenduse helistamiseks abil teha loendi Web API konfigureerimine

Alustamiseks, [laadige rakendus luukere](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) või [allalaadimine on lõpule viidud valimi](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Iga on lahenduse Visual Studio 2013.  Peate mõne Azure AD rentniku, kuhu rakenduse registreerida.  Kui teil pole seda juba teinud, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. rakenduse registreerimine Azure AD*
Rakenduse turvamiseks esmalt peate oma rentniku rakenduse loomine ja Azure AD teabe mõned võtme tekstilõigule.

-   [Azure'i haldusportaal](https://manage.windowsazure.com) sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige, kuhu rakenduse registreerida rentniku.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua mõne uue **veebirakenduse ja/või WebAPI**.
    -   Rakenduse lõppkasutajatele kirjeldada rakenduse **nimi** .  Sisestage "loendi teenuse".
    -   **Suunake Uri** on värviskeemi ja stringi kombinatsioon, mida kasutaks tagastada kõik märgid rakenduse nõutav Azure AD. Sisestage `https://localhost:44321/` selle väärtuse jaoks.
-   Kui olete registreerimine, liikuge **konfigureerimine** menüü ja otsige üles **Rakenduse ID URI** väljal.  Sisestage kood rentniku kohased see väärtus, nt`https://contoso.onmicrosoft.com/TodoListService`
- Salvestage konfiguratsiooni.  Portaali avatuks – peate samuti registreerida klientrakenduse varsti.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2 häälestamine rakenduse OWIN autentimise müügivõimaluste kasutamiseks*

Nüüd, kui olete registreerunud rakenduse Azure AD, peate häälestamine rakenduse valideerimiseks, sissetulevad taotlused ja sõned Azure AD suhelda.

-   Alustamiseks avage lahendus ja lisada OWIN vahevara Nugeti pakettide TodoListService projekti Package Manager konsooli abil.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Lisada mõne tunni OWIN käivitus TodoListService projekti nimega `Startup.cs`.  Õige, klõpsake nuppu projekti--> **Lisa** --> **Uue üksuse** --> otsing "OWIN".  Käivitub OWIN vahevara selle `Configuration(…)` meetod rakenduse käivitamisel.
-   Klassi deklaratsiooni muutmine `public partial class Startup` -oleme juba rakendanud see tund osa jaoks teise faili.  Klõpsake soovitud `Configuration(…)` meetod teha kõne ConfgureAuth(...) autentimine veebirakenduse jaoks häälestamiseks.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Avage fail `App_Start\Startup.Auth.cs` ja rakendada selle `ConfigureAuth(…)` meetod.  Esitate parameetrid `WindowsAzureActiveDirectoryBearerAuthenticationOptions` toimib koordinaadid oma rakenduse Azure AD suhelda.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Nüüd saate `[Authorize]` atribuute kaitsta teie kontrollerid ja JWT esitaja autentimise toimingud.  Kaunistamine on `Controllers\TodoListController.cs` klassi mõne Autoriseerin silt.  See sunnib kasutaja enne selle lehe sisse logida.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Kui mõni volitatud helistaja edukalt viitab üks soovitud `TodoListController` API-d, peate toimingu helistaja teave.  OWIN pakub juurdepääsu taotluste esitaja luba kaudu sees on `ClaimsPrincpal` objekti.  
- Web API-de jaoks üldine tingimus on domeenist "otsinguulatuste" luba kohal - see tagab, et lõppkasutaja on nõustunud Todo loendi teenuse vajalikke õigusi:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Lõpetuseks, avage soovitud `web.config` root TodoListService projekti failide ja sisestage oma konfiguratsioon väärtusi on `<appSettings>` jaotis.
  - Funktsiooni `ida:Tenant` on Azure AD rentniku, nt "contoso.onmicrosoft.com" nimi.
  - Teie `ida:Audience` on rakenduse ID URI taotluse Azure portaali sisestatud.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. konfigureerimine kliendi rakendus ja teenuse käivitamine*
Enne tegelikkuses teenuse Todo loendi kuvamiseks peate Todo loendist kliendi konfigureerida nii, et see saab sõned toomine AAD ja teenuse helistada.

- Liikuge tagasi [Azure'i haldusportaal](https://manage.windowsazure.com)
- Loo uus rakendus oma Azure AD rentniku ja valige **Kohalikke klientrakendusega** tulemuseks küsimus.
    -   Rakenduse **nimi** kirjeldab rakenduse lõppkasutajatele
    -   Sisestage `http://TodoListClient/` **Ümber suunata Uri** väärtus.
- Kui olete registreerimine, AAD määrata rakenduse kordumatu **Kliendi ID-d**. Peate seda väärtust järgmiste juhiste juurde, seega kopeerige see menüü konfigureerimine.
- Ka **konfigureerimine** vahekaardil, otsige üles jaotis "Õigused soovite muud rakendused". Klõpsake nuppu "Lisa rakendus." Valige "Kuva" rippmenüüst "Kõik rakendused" ja valige ülemine märge. Otsige üles ja klõpsake abil teha loendi teenust ja all märge rakenduse lisamiseks klõpsake. Valige "Accessi abil ei loendi teenust", "Delegeeritud õigused" rippmenüüst ja salvestada konfiguratsiooni.


- Visual Studio, avage `App.config` soovitud TodoListClient rakenduses project ja sisestage oma konfiguratsioon väärtused on `<appSettings>` jaotis.
  - Funktsiooni `ida:Tenant` on Azure AD rentniku, nt "contoso.onmicrosoft.com" nimi.
  - Teie `ida:ClientId` rakenduse ID kopeeritud Azure'i portaal.
  - Teie `todo:TodoListResourceId` on rakenduse ID URI abil teha loendi rakenduse Azure portaali sisestatud.

Lõpuks puhastada, luua ja käivitada iga projekti!  Kui te pole seda veel teinud, siis nüüd on aeg luua uus kasutaja oma rentniku koos mõne *. onmicrosoft.com domeeni.  Loendite kliendile kasutaja sisselogimine ja lisage põhitoiminguid teha loendisse kasutaja.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Te saate nüüd viia veel täiendavad identiteedi stsenaariumid.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
