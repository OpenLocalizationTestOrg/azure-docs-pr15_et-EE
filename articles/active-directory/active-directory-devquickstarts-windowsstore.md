<properties
    pageTitle="Azure AD Windowsi poe alustamine | Microsoft Azure'i"
    description="Kuidas luua Windowsi poe rakendus, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de OAuthi abil."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Integreerimine Azure AD Windowsi poe rakendus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Kui arendate Windowsi poe rakendus, Azure AD muudab lihtne ja arusaadav teile autentida teie kasutajad saavad Active Directory kontod.  See lubab ka rakenduse turvaliselt kasutamine mis tahes veebi-API, mis on kaitstud Azure AD, nt Office 365 API-d või Azure API-ga.

Windowsi poe töölauarakendusi, mida on vaja juurdepääsu kaitstud ressursse, pakub Azure AD Active Directory Authentication Library või ADAL.  ADAL on ainus eesmärk elu on hõlpsalt oma rakenduse access sõned saada.  Näidata, kui lihtne on siin me tuleb koostada "Directory otsija" Windowsi poe rakenduse mis:

-   Saab kasutada sõned Azure AD Graph API [OAuth 2.0 autentimine protokolli](https://msdn.microsoft.com/library/azure/dn645545.aspx)abil helistamine.
-   Otsib kasutajatele, kellel on antud UPN-i kataloogi.
-   Märke kasutajad välja.

Täieliku töötamine rakenduse loomiseks peate:

2. Rakenduse registreerima Azure AD.
3. Installimine ja konfigureerimine ADAL.
5. ADAL abil saate sõned Azure AD.

Alustamiseks, [skelett projekti alla laadida](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) või [lõpuleviidud valimi alla laadida](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Iga on lahenduse Visual Studio 2015.  Peate Azure AD rentniku, kus saate luua kasutajaid ja rakenduse registreerimine.  Kui teil pole veel rentniku, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. register Directory otsija rakendus*
Saada sõned rakenduse lubamiseks esmalt peate selle oma Azure AD rentniku registreerida ja selle juurdepääsu lubamine Azure AD Graph API.

-   [Azure'i haldusportaal](https://manage.windowsazure.com) sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige, kuhu rakenduse registreerida rentniku.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua uue **Kohalikke klientrakendusega**.
    -   Rakenduse **nimi** kirjeldab rakenduse lõppkasutajatele
    -   **Suunata Uri** on värviskeemi ja stringi kasutavate Azure AD Turbeloa vastuste tagastamiseks.  Sisestage väärtus kohatäite praegu, nt `http://DirectorySearcher`.  Me asendada selle väärtuse hiljem.
-   Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü **konfigureerimine** .
- Ka **konfigureerimine** vahekaardil, otsige üles jaotis "Õigused soovite muud rakendused".  "Azure Active Directory" rakenduse, lisada **Accessi kataloogi sisselogitud kasutaja** õiguste jaotises **Delegeeritud õigused**.  See võimaldab rakenduse päringu Graph API kasutajate jaoks.

## <a name="2-install--configure-adal"></a>*2 installida ja konfigureerida ADAL*
Nüüd, kui teil on rakenduse Azure AD, saate installida ADAL ja kirjutage oma identiteedi seotud kood.  Selleks ADAL saama Azure AD suhelda, peate esitama registreerimise rakenduse kohta leiate teavet.
-   Alustage, lisades ADAL DirectorySearcher projekti paketi halduri konsooli abil.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Avage DirectorySearcher projekti `MainPage.xaml.cs`.  Väärtuste asendamine soovitud `Config Values` piirkond kajastamiseks Azure portaali sisestatud väärtused.  Iga kord, kui seda kasutatakse ADAL viitab teie koodi need väärtused.
    -   Funktsiooni `tenant` on Azure AD rentniku, nt contoso.onmicrosoft.com Domeen.
    -   Funktsiooni `clientId` on rakenduse portaalist kopeerisite clientId.
-   Nüüd on vaja leida oma Windowsi poe rakenduse tagasihelistamise uri.  Määrate katkestuspunkti selle real on `MainPage` meetod:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Lahendus koostada, veenduge, et kõik paketi viited on taastatud.  Kui paketid on puudu, avage Nugeti Package Manager ja taastada paketid.
- Käivitage rakendus ning kopeerige kõrvale väärtus `redirectUri` kui katkestuspunkt on tulemus.  See peaks välja nägema umbes

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Tagasi menüü **konfigureerimine** rakenduse Azure'i haldusportaal asendada selle väärtuse **RedirectUri** väärtus.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. saada sõned AAD ADAL kasutamine*
On põhimõte ADAL taha, et iga kord, kui teie rakendus peab juurdepääsu sümboolse, seda lihtsalt helistab `authContext.AcquireToken(…)`, ja ADAL teeb ülejäänud.  

-   Esimese asjana oma rakenduse käivitamine `AuthenticationContext` -ADAL kasutaja esmase ainekursuse.  See on, kus te kaotate ADAL tuleb Azure AD suhelda ja juhendada selle vahemälu sõned koordinaate.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Nüüd otsige üles soovitud `Search(...)` meetod, mis tugineb, kui kasutaja klõpsab nuppu "Otsing" rakenduse UI.  Selle meetodiga GET-päringu Azure AD Graph API päringusse kasutajatele, kelle UPN-i algab antud otsingutermin.  Selleks, et päringu Graph API, tuleb kaasata ka access_token sisse, kuid selle `Authorization` päis kutse – see on koht, kus on ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Kui teie rakendus taotleb märgiks helistades `AcquireTokenAsync(...)`, ADAL proovib juurde naasmiseks märgiks küsimata kasutaja mandaat.  Kui ADAL määratleb, et saada märgiks sisselogimiseks peab kasutaja, see on login väljalogimisel koguda kasutaja mandaat ja tagastada märgiks kinnitamisel eduka.  Kui ADAL ei saa tagastada märgiks mingil põhjusel on `AuthenticationResult` olek on tõrge.

- Nüüd on aeg kasutada funktsiooni access_token, vaid ostsite.  Ka selle `Search(...)` meetod manustamine luba päises autoriseerimine Graph API saada taotlus:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Saate kasutada ka funktsiooni `AuthenticationResult` objekti kasutaja kohta teabe kuvamiseks oma rakenduses, nt kasutaja id:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Lisaks saate ADAL kasutaja rakendus ka välja logima.  Kui kasutaja klõpsab nuppu "Logi välja", soovime tagada, et järgmise kõne `AcquireTokenAsync(...)` Kuva märk vaates.  ADAL, see on sama lihtne kui Turbeloa vahemälu tühjendamine:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Palju õnne! Nüüd on oma Windowsi poe rakendus, mis sisaldab kasutajate autentimiseks, helistage turvaline Web API-de OAuth 2.0 kasutamise võimalus ja saada põhiteavet kasutaja.  Kui te pole seda veel teinud, siis nüüd on aeg asustamiseks oma rentniku mõned kasutajatega.  Käivitage DirectorySearcher rakenduse ja logige sisse ühe nende kasutajate.  Otsige teised kasutajad saavad UPN-i põhjal.  Sulgege rakendus ja käivitage see uuesti.  Pange tähele, kuidas kasutaja seansi jääb samaks.  Logi välja (paremklõpsates allosas oleval ribal kuvamiseks) ja teise kasutajana logige uuesti sisse.

ADAL lihtne lisada kõik need levinud identiteedi funktsioonid rakenduse.  See hoolitseb musta töö teile - vahemälu halduse, OAuthi protokolli tugi, esinemise kasutaja kasutajanimega UI, värskendamine aegunud sõned ja palju muud.  Kõik, mida peaksite teadma on ühe API kõne, `authContext.AcquireToken*(…)`.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Te saate nüüd liikuda täiendavad identiteedi stsenaariumid.  Võite proovida:

[Turvaline .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
