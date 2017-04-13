<properties
    pageTitle="Azure'i AD Xamarin alustamine | Microsoft Azure'i"
    description="Kuidas koostada Xamarin rakendus, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de kasutamine OAuthi."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Azure AD integreerida Xamarin rakendus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin võimaldab kirjutada mobiilirakendused C#, mis töötab iOS-i, Androidi ja Windowsi (mobiilsideseadmetes ja arvutites). Kui olete rakenduse abil Xamarin hoone, Azure AD muudab lihtne ja arusaadav teile autentida teie kasutajad saavad Active Directory kontod. See lubab ka rakenduse turvaliselt kasutamine mis tahes veebi-API, mis on kaitstud Azure AD, nt Office 365 API-d või Azure API-ga.

Xamarin rakendusi, mis peavad juurde pääsema kaitstud ressursse, pakub Azure AD Active Directory Authentication Library või ADAL. ADAL on ainus eesmärk elu on hõlpsalt oma rakenduse access sõned saada. Näidata, kui lihtne on siin me tuleb koostada "Directory otsija" rakenduse mis:

-   Töötab iOS-i, Android, Windowsi töölaua, Windows Phone ja Windowsi poest.
- Ühe portable klassiteek (PCL) kasutab kasutajate autentimiseks ja Azure AD Graph API sõned hankimine
-   Otsib kasutajatele, kellel on antud UPN-i kataloogi.

Täieliku töötamine rakenduse loomiseks peate:

2. Xamarin arengu keskkonna häälestamine
2. Rakenduse registreerima Azure AD.
3. Installimine ja konfigureerimine ADAL.
5. ADAL abil saate sõned Azure AD.

Alustamiseks, [skelett projekti alla laadida](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) või [lõpuleviidud valimi alla laadida](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Iga on lahenduse Visual Studio 2013. Peate Azure AD rentniku, kus saate luua kasutajaid ja rakenduse registreerimine. Kui teil pole veel rentniku, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. Xamarin arengu keskkonna häälestamine*
Kuna õppeteema sisaldab projektide iOS-i, Androidi ja Windowsi, peate Visual Studio ja Xamarin koos. Vajalik keskkond loomiseks järgige täieliku [häälestamise ja](https://msdn.microsoft.com/library/mt613162.aspx) installige Visual Studio ja Xamarin MSDN-is. Need juhised sisaldavad materjali, mis võimaldab teil läbi vaadata lisateavet Xamarin kui olete oodanud paigaldajaid lõpuleviimiseks. 

Kui olete lõpetanud vajalikud häälestamise, avage lahenduse Visual Studios alustada. Kuus projektide leiate: viis platvormi kohased projektide ja ühe portable ainekursuse teek, mis jagatakse üle kõik platvormid,`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. register Directory otsija rakendus*
Saada sõned rakenduse lubamiseks esmalt peate selle oma Azure AD rentniku registreerida ja selle juurdepääsu lubamine Azure AD Graph API.

-   [Azure'i haldusportaal](https://manage.windowsazure.com) sisselogimine
-   Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**
-   Valige, kuhu rakenduse registreerida rentniku.
-   Klõpsake vahekaarti **rakendused** , ja klõpsake nuppu **Lisa** alla sahtlis.
-   Järgige viipasid ja luua uue **Kohalikke klientrakendusega**.
    -   Rakenduse **nimi** kirjeldab rakenduse lõppkasutajatele
    -   **Suunata Uri** on värviskeemi ja stringi kasutavate Azure AD Turbeloa vastuste tagastamiseks. Sisestage väärtus, nt `http://DirectorySearcher`.
-   Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator. Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü **konfigureerimine** .
- Ka **konfigureerimine** vahekaardil, otsige üles jaotis "Õigused soovite muud rakendused". "Azure Active Directory" rakenduse, lisada **juurdepääs teie ettevõtte kataloogi** luba jaotises **Delegeeritud õigused**. See võimaldab rakenduse päringu Graph API kasutajate jaoks.

## <a name="2-install--configure-adal"></a>*2 installida ja konfigureerida ADAL*
Nüüd, kui teil on rakenduse Azure AD, saate installida ADAL ja kirjutage oma identiteedi seotud kood. Selleks ADAL saama Azure AD suhelda, peate esitama registreerimise rakenduse kohta leiate teavet.
-   Alustage, lisades ADAL projekte lahendus Package Manager konsooli abil.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Peaks märkate kaks teegi viited lisatakse iga projekti - PCL osa ADAL- ja platvormi kohased osa.

-   Avage DirectorySearcherLib projekti `DirectorySearcher.cs`. Klassi liikme väärtusi kajastamiseks Azure portaali sisestatud väärtusi muuta. Iga kord, kui seda kasutatakse ADAL viitab teie koodi need väärtused.
    -   Funktsiooni `tenant` on Azure AD rentniku, nt contoso.onmicrosoft.com Domeen.
    -   Funktsiooni `clientId` on rakenduse portaalist kopeerisite clientId.
    - Funktsiooni `returnUri` on redirectUri, sisestatud portaalis, nt `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. saada sõned AAD ADAL kasutamine*
*Peaaegu* kõik rakenduse autentimine loogika seisneb `DirectorySearcher.SearchByAlias(...)`. Vajaliku platvormi kohased projektide läheb kontekstipõhine parameeter on `DirectorySearcher` PCL.

- Kõigepealt Avage `DirectorySearcher.cs` ja lisage uus parameeter on `SearchByAlias(...)` meetod. `IPlatformParameters`on kontekstipõhine parameeter, mis kapseloi platvormi kohased objektid, mis ADAL tuleb autentida.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Järgmiseks lähtestada selle `AuthenticationContext` -ADAL kasutaja esmase ainekursuse. See on, kus te kaotate ADAL suhelda Azure AD läheb vaja koordinaate. Klõpsake kõne `AcquireTokenAsync(...)`, mida aktsepteerib selle `IPlatformParameters` objekti ja käivitub autentimise voogu vajalikud märgiks naasmiseks rakenduse.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`esmalt proovib juurde naasmiseks märgiks taotletud ressurss (Graph API sel juhul) küsimata kasutajal sisestada oma volitusi (vahemällu või värskendamine vana sõned) kaudu. Kui see on vajalik, kuvatakse see kasutaja Azure AD Logi lehel nõutud Luba alati.


- Seejärel saate manustada Graph API taotluse päises luba juurdepääsu luba:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Mis on kõik jaoks soovitud `DirectorySearcher` PCL ja rakenduse kasutaja identiteet seotud koodi.  Jääb helistamiseks on `SearchByAlias(...)` meetod iga platvormi vaadetes ja kus vajadusel lisada koodi õigesti käsitlemise UI elutsükli.

####<a name="android"></a>Android:
- Klõpsake `MainActivity.cs`, kõne lisamine `SearchByAlias(...)` klõpsake nuppu Valige sündmuseohjuri:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Samuti vajate alistada soovitud `OnActivityResult` elutsükli meetod, mis tahes autentimise suunata suunab tagasi sobivat meetodit.  ADAL annab abimees meetodi nii Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windowsi töölaua:
- Klõpsake `MainWindow.xaml.cs`, lihtsalt helistada `SearchByAlias(...)` läbides on `WindowInteropHelper` soovitud töölauarakenduses `PlatformParameters` objekti:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- Klõpsake `DirSearchClient_iOSViewController.cs`, iOS `PlatformParameters` objekti võtab lihtsalt vaate kontrolleril viide:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Windowsi universaalne:
- Universaalne Windows, avage `MainPage.xaml.cs` ja rakendada selle `Search` meetod, mis kasutab helper meetodi ühiskasutuses projekti värskendamiseks UI vastavalt vajadusele.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Palju õnne! Nüüd on teil töö Xamarin rakendus, mis sisaldab kasutajate autentimiseks ja turvaliselt kõne Web API-de kasutamine OAuth 2.0 viit eri keskkondades. Kui te pole seda veel teinud, siis nüüd on aeg asustamiseks oma rentniku mõned kasutajatega. Käivitage DirectorySearcher rakenduse ja logige sisse ühe nende kasutajate. Otsige teised kasutajad saavad UPN-i põhjal.

ADAL lihtne lisada levinud identiteedi funktsioonid rakenduse. See hoolitseb musta töö teile - vahemälu halduse, OAuthi protokolli tugi, esinemise kasutaja kasutajanimega UI, värskendamine aegunud sõned ja palju muud. Kõik, mida peaksite teadma on ühe API kõne, `authContext.AcquireToken*(…)`.

Viide, lõpule viidud näidis (ilma väärtuste määramine) on esitatud [allpool](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Te saate nüüd liikuda täiendavad identiteedi stsenaariumid. Võite proovida:

[Turvaline .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
