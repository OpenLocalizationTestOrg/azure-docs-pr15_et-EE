<properties
    pageTitle="Azure AD .NET alustamine | Microsoft Azure'i"
    description="Kuidas luua .NET Windowsi töölaua rakendus, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de kasutamine OAuthi."
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


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Azure AD integreerida Windowsi töölaua WPF rakenduse

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Kui arendate töölauarakendus, Azure AD muudab lihtne ja arusaadav teile autentida teie kasutajad saavad Active Directory kontod.  See lubab ka rakenduse turvaliselt kasutamine mis tahes veebi-API, mis on kaitstud Azure AD, nt Office 365 API-d või Azure API-ga.

.NET kohalikke klientidele, kes vajavad juurdepääsu kaitstud ressursse, leiate Azure'i AD Active Directory Authentication Library või ADAL.  ADAL on ainus eesmärk elu on hõlpsalt oma rakenduse access sõned saada.  Näidata, kui lihtne on siin me tuleb koostada .NET WPF Ülesandeloend rakenduse mis:

-   Saab kasutada sõned Azure AD Graph API [OAuth 2.0 autentimine protokolli](https://msdn.microsoft.com/library/azure/dn645545.aspx)abil helistamine.
-   Otsib kataloog kasutajatele, kellel on antud alias (pseudonüüm).
-   Märke kasutajad välja.

Täieliku töötamine rakenduse loomiseks peate:

2. Rakenduse registreerima Azure AD.
3. Installimine ja konfigureerimine ADAL.
5. ADAL abil saate sõned Azure AD.

Alustamiseks, [laadige rakendus luukere](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) või [allalaadimine on lõpule viidud valimi](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Peate Azure AD rentniku, kus saate luua kasutajaid ja rakenduse registreerimine.  Kui teil pole veel rentniku, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. register DirectorySearcher rakendus*
Saada sõned rakenduse lubamiseks Kõigepealt peate registreeruma seda oma Azure AD rentniku ja selle juurdepääsu lubamine Azure AD Graph API.

-   Azure'i haldusportaal sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige, kuhu rakenduse registreerida rentniku.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua uue **Kohalikke klientrakendusega**.
    -   Rakenduse **nimi** kirjeldab rakenduse lõppkasutajatele
    -   **Suunake Uri** on värviskeemi ja stringi kasutavate Azure AD Turbeloa vastuste tagastamiseks.  Sisestage väärtus, mis on teatud rakenduse, nt `http://DirectorySearcher`.
-   Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaatori.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü **konfigureerimine** .
- Ka **konfigureerimine** vahekaardil, otsige üles jaotis "Õigused soovite muud rakendused".  "Azure Active Directory" rakenduse, lisada **juurdepääs teie ettevõtte kataloogi** luba jaotises **Delegeeritud õigused**.  See võimaldab rakenduse päringu Graph API kasutajate jaoks.

## <a name="2-install--configure-adal"></a>*2 installida ja konfigureerida ADAL*
Nüüd, kui teil on rakenduse Azure AD, saate installida ADAL ja kirjutage oma identiteedi seotud kood.  Selleks ADAL saama Azure AD suhelda, peate esitama registreerimise rakenduse kohta leiate teavet.
-   Alustage, lisades ADAL DirectorySearcher projekti Package Manager konsooli abil.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Avage DirectorySearcher projekti `app.config`.  Elementide väärtuste asendamine soovitud `<appSettings>` jaotis kajastamiseks Azure portaali sisestatud väärtused.  Iga kord, kui seda kasutatakse ADAL viitab teie koodi need väärtused.
    -   Funktsiooni `ida:Tenant` on Azure AD rentniku, nt contoso.onmicrosoft.com Domeen.
    -   Funktsiooni `ida:ClientId` on rakenduse portaalist kopeerisite clientId.
    -   Funktsiooni `ida:RedirectUri` on uuesti registreeritud portaali url.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. saada sõned AAD ADAL kasutamine*
On põhimõte ADAL taha, et iga kord, kui teie rakendus peab juurdepääsu sümboolse, seda lihtsalt helistab `authContext.AcquireTokenAsync(...)`, ja ADAL teeb ülejäänud.  

-   Klõpsake soovitud `DirectorySearcher` projekti avatud `MainWindow.xaml.cs` ja otsige üles soovitud `MainWindow()` meetod.  Esimese asjana oma rakenduse lähtestamine `AuthenticationContext` -ADAL kasutaja esmase klassi.  See on, kus te kaotate ADAL tuleb Azure AD suhelda ja juhendada selle vahemälu sõned koordinaate.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Nüüd otsige üles soovitud `Search(...)` meetod, mis tugineb, kui kasutaja cliks "Otsing" nuppu rakenduse UI.  Selle meetodiga GET-päringu Azure AD Graph API päringusse kasutajatele, kelle UPN-i algab antud otsingutermin.  Selleks, et päringu Graph API, tuleb kaasata ka access_token sisse, kuid selle `Authorization` päise kutse – see on koht, kus on ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Kui teie rakendus taotleb märgiks helistades `AcquireTokenAsync(...)`, ADAL proovib juurde naasmiseks märgiks küsimata kasutaja mandaat.  Kui ADAL määrab, et kasutaja peab saada märgiks sisse logida, see kuvada sisselogimise dialoogiboksi, koguda kasutaja mandaat ning tagastavad märgiks kinnitamisel eduka.  Kui ADAL ei saa tagastada märgiks mingil põhjusel, see põhjustada mõne `AdalException`.
- Pange tähele, et selle `AuthenticationResult` objekt sisaldab on `UserInfo` objekt, mida saab kasutada rakenduse vajalikud teabe kogumiseks.  Klõpsake soovitud DirectorySearcher `UserInfo` kasutatakse kohandada rakenduse UI kasutaja id-ga.

- Kui kasutaja klõpsab nuppu "Logi välja", soovime tagada, et järgmise kõne `AcquireTokenAsync(...)` küsib kasutaja sisse logida.  ADAL, see on sama lihtne kui Turbeloa vahemälu tühjendamine:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Juhul, kui kasutajal pole nuppu "Logi välja", kas soovite säilitada kasutaja seansi jaoks järgmine kord, kui nad käivitada soovitud DirectorySearcher.  Kui rakendus käivitab, saate kontrollida mõne olemasoleva luba ADAL's Turbeloa vahemälu ja UI vastavalt uuendada.  Rakenduses selle `CheckForCachedToken()` meetod teise helistamine `AcquireTokenAsync(...)`, läbides seekord soovitud `PromptBehavior.Never` parameeter.  `PromptBehavior.Never`ütleb ADAL, et kasutaja küsitaks ei Logi sisse ja ADAL peaks hoopis põhjustada erandi, kui seda ei saa tagastada märgiks.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Palju õnne! Nüüd töötav WPF .NET-i rakendus, mis on võimalus kasutajate autentimiseks, helistage turvaline Web API-de kasutamine OAuth 2.0 ja saada põhiteavet kasutaja.  Kui te pole seda veel teinud, siis nüüd on aeg asustamiseks oma rentniku mõned kasutajatega.  Käivitage DirectorySearcher rakenduse ja logige sisse ühe nende kasutajate.  Otsige teised kasutajad saavad UPN-i põhjal.  Sulgege rakendus ja käivitage see uuesti.  Pange tähele, kuidas kasutaja seansi jääb samaks.  Logige välja ja uuesti sisse logida teise kasutajana.

ADAL lihtne lisada kõik need levinud identiteedi funktsioonid rakenduse.  See hoolitseb musta töö teile - vahemälu halduse, OAuthi protokolli tugi, esinemise kasutaja kasutajanimega UI, värskendamine aegunud sõned ja palju muud.  Kõik, mida peaksite teadma on ühe API kõne, `authContext.AcquireTokenAsync(...)`.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Te saate nüüd liikuda täiendavad stsenaariumid.  Võite proovida:

[Turvaline .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
