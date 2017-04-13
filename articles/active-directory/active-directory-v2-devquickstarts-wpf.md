<properties
pageTitle="Azure Active Directory v2.0 .NET rakenduste | Microsoft Azure'i"
description="Kuidas koostada .NET kohalikke rakenduse, et kasutajad nii isikliku Microsofti Account ja töö-või koolikonto märke."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Logi sisse Windowsi töölaua rakenduse lisamine

Kus on v2.0 lõpp-punkti, saate kiiresti lisada töölauarakenduste toega mõlema isikliku Microsofti konto autentimine ja töö-või koolikonto.  See lubab ka rakenduse suhelda turvaliselt kirjutamata web api kui ka [Microsoft Graphi](https://graph.microsoft.io) ja mõned [Office 365 ühendatud API -d](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Kõik Azure Active Directory (AD) stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

[.NET kohalikke rakenduste seadmes, mis töötavad](active-directory-v2-flows.md#mobile-and-native-apps)Azure AD pakub Microsofti Identity Authentication Library või MSAL.  MSAL on ainus eesmärk elu on hõlpsalt oma rakenduse helistamiseks veebiteenuste sõned saamiseks.  Näidata, kui lihtne on siin me tuleb koostada .NET WPF Ülesandeloend rakenduse mis:

- Kasutaja sisse logib ja saab juurdepääsu sõned [OAuth 2.0 autentimise protokoll](active-directory-v2-protocols.md#oauth2-authorization-code-flow)abil.
- Turvaline kõned on kirjutamata Ülesandeloend veebiteenus, mis on ka tagatud OAuth 2.0.
- Kasutaja logib.

## <a name="download-sample-code"></a>Proovi kood allalaadimine

Selle õpetuse kood säilitatakse [github](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) - või klooni skelett:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Lõpetatud app on esitatud ka õppeteema lõpus.

## <a name="register-an-app"></a>Rakenduse registreerimine
Luua uue rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige neid [üksikasjalikke juhiseid](active-directory-v2-app-registration.md).  Veenduge, et:

- Kopeeri allapoole määratud rakenduse **Rakenduse Id** peate selle varsti.
- Lisage oma rakenduse **Mobile** platvormi.

## <a name="install--configure-msal"></a>Installimine ja konfigureerimine MSAL
Nüüd, kui teil on registreeritud Microsofti rakendus, saate installida MSAL ja kirjutage oma identiteedi seotud kood.  Et MSAL saaksid suhelda v2.0 lõpp-punkti, peate pakkuda rakenduse registreerimise kohta leiate teavet.

-   Alustage, lisades MSAL TodoListClient projekti Package Manager konsooli abil.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Avage TodoListClient projekti `app.config`.  Elementide väärtuste asendamine soovitud `<appSettings>` jaotis kajastamiseks rakenduse registreerimise portaali sisestatud väärtused.  Iga kord, kui seda kasutatakse MSAL viitab teie koodi need väärtused.
    -   Funktsiooni `ida:ClientId` on **Rakenduse Id** oma rakenduse kopeeritud portaalist.

- Avage projekt TodoList-teenus, `web.config` projekti juurkaustas.  
    - Asendada selle `ida:Audience` väärtus portaalist sama **Rakenduse Id** -ga.

## <a name="use-msal-to-get-tokens"></a>MSAL abil saate sõned
On põhimõte MSAL taha, et iga kord, kui teie rakendus peab juurdepääsu sümboolse, siis lihtsalt helistada `app.AcquireToken(...)`, ja MSAL teeb ülejäänud.  

-   Klõpsake soovitud `TodoListClient` projekti avatud `MainWindow.xaml.cs` ja otsige üles soovitud `OnInitialized(...)` meetod.  Esimese asjana oma rakenduse käivitamine `PublicClientApplication` -MSAL tema esmase klassi tähistav algsete rakenduste.  See on, kus te kaotate MSAL Azure AD suhelda ja juhendada selle vahemälu sõned koordinaate.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Kui rakendus käivitub, soovime ja vaadake, kui kasutaja on juba logitud rakendusse.  Kuid me ei taha autonoomsest sisselogimise UI veel - teeme kasutaja, klõpsake nuppu "Logi sisse" seda teha.  Samuti on `OnInitialized(...)` meetod:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Kui kasutaja pole sisse logitud ja need nuppu "Logi sisse", soovime autonoomsest sisselogimise Kasutajaliidese ja on kasutaja, sisestage oma kasutajanimi ja parool.  Rakendada sisselogimise nupu sündmuseohjuri:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Kui kasutaja edukalt märgid sisse, saadetakse teile MSAL ja vahemälu märgiks teile ja jätkake kõne on `GetTodoList()` confidence meetod.  On alles saada kasutaja ülesanded on rakendada selle `GetTodoList()` meetod.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Käivita

Palju õnne! Nüüd on teil töötamise .NET WPF rakendus, mis sisaldab kasutajate autentimiseks ja turvaliselt kõne Web API-de kasutamine OAuth 2.0 võimalus.  Käivitage oma nii projektide ja logige sisse Microsofti konto või töökoha või kooli kontoga.  Teise kasutaja Ülesandeloend ülesanded lisada.  Logige välja ja logige uuesti sisse teise kasutaja vaadata nende Ülesandeloend.  Sulgege rakendus ja käivitage see uuesti.  Pöörake tähelepanu sellele, kuidas kasutaja seansi jääb samaks – selle põhjuseks on asjaolu rakenduse vahemälu märkide kohalik fail.

MSAL hõlbustab levinud identiteedi funktsioonid rakenduse, lisada nii isiklikku ja kontode abil.  See hoolitseb musta töö teile - vahemälu halduse, OAuthi protokolli tugi, esitades kasutaja sisselogimine UI, värskendamine aegunud sõned ja palju muud.  Kõik, mida peaksite teadma on ühe API kõne, `app.AcquireTokenAsync(...)`.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [soovitud ZIP siin on saadaval](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)või saate klooni seda GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Järgmised sammud

Nüüd saate teisaldada täpsemate teemade peale.  Võite proovida:

- [Turvaliseks TodoListService Web API v2.0 lõpp-punkti abil](active-directory-v2-devquickstarts-dotnet-api.md)

Täiendavad ressursid, vaadake:  

- [V2.0 arendaja juhend >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "msal" sildi >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
