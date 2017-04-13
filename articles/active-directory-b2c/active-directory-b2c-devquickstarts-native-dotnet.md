<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure'i"
    description="Kuidas luua Windowsi töölaua rakendus, mis sisaldab sisselogimist, registreerumise, ja kasutajaprofiilide haldamise Azure Active Directory B2C abil."
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

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure'i AD B2C: Koostada töölauarakendust Windowsi

Azure Active Directory (Azure AD) B2C abil saate lisada oma paari toiminguga töölauarakenduse võimsate iseteenindusliku identiteedi haldusfunktsioonid. Selles artiklis näitab teile, kuidas luua .NET Windows Presentation Foundationi (WPF) "Ülesandeloend" rakenduse, mis sisaldab kasutaja sisse logida, sisselogimine ja kasutajaprofiilide haldamine. Rakenduse kaasatakse tugi sisse logida ja sisselogimise kasutajanimi või e-posti abil. See sisaldab ka tugi sisse logida ja logige sisse, kasutades suhtlusvõrgu kontod nagu Facebookis ja Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Saada on Azure AD B2C kataloog

Enne kasutamist Azure AD B2C, saate luua või rentniku.  Kataloogi on ümbris kõigi kasutajate, rakendused, rühmad ja muu. Kui teil pole ühte juba enne [B2C luua](active-directory-b2c-get-started.md) sellest juhendist jätkata.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate B2C kataloogis rakenduse loomine. See annab Azure AD teabest rakenduse turvaliselt suhelda. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md).  Kindlasti järgmist.

- Lisage **omakliendi** rakendus.
- Kopeerige **ümber suunata URI** `urn:ietf:wg:oauth:2.0:oob`. See on vaikimisi URL-i seda proovi kood.
- Kopeerige **Rakenduse ID** , mis määratakse teie rakendus. Teil on vaja hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

Azure'i AD B2C, iga kasutusvõimalused on määratletud [poliitika](active-directory-b2c-reference-policies.md). Seda proovi kood sisaldab kolme identiteedi kogemusi: registreeruda, logige sisse ja Redigeeri profiili. Peate looma iga tüübi poliitika [poliitika artiklis](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kirjeldatud. Kui loote kolme poliitikat, ärge unustage:

- Valige **Kasutaja ID registreerumise** või **e-posti registreerumise** identiteedi pakkujate tera.
- Valige oma registreerumise poliitika **kuvatav nimi** ja muud registreerumise atribuudid.
- Valige **kuvatav nimi** ja **Objekti ID** taotluste nimega rakenduse nõuded iga poliitika. Soovi korral saate kasutada ka muud nõuded.
- Kopeerige iga poliitika **nimi** pärast selle loomist. Sellel peab olema eesliite `b2c_1_`.  Peate need poliitika nimed hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kui olete loonud kolm poliitikad, olete valmis oma rakenduse.

## <a name="download-the-code"></a>Laadige alla kood

See kuueosalisest [säilitatakse github](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet)kood. Valimi koostamiseks, kui lähete, saate [alla laadida skelett projekti ZIP-faili](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Samuti saate klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Lõpetatud app on [saadaval ZIP-faili](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) ka või selle `complete` haru sama hoidla.

Kui laadite proovi kood, avage Visual Studio .sln fail alustada. Funktsiooni `TaskClient` projekt on kasutaja suhtleb WPF töölauarakendus olemas. Selleks et selles õpetuses, selle kõned tagaandmebaas tööülesande web API, majutatud Azure, mis salvestab iga kasutaja Ülesandeloend.  Teil pole vaja luua veebi-API, meil on juba see töötab teie eest.

Lisateavet selle kohta, kuidas veebi-API turvaliselt kontrollib taotlusi, kasutades Azure AD B2C, tutvuge [veebi-API alustamine artikkel](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Käivitada poliitika
Rakenduse suhtleb Azure AD B2C saatmisega autentimise sõnumid, mis soovivad osana HTTP-päringu käivitada poliitika määramine. .Net-i töölauarakenduste, saate eelvaade Microsoft autentimine teek (MSAL) abil saate OAuth 2.0 autentimine sõnumite saatmine, käivitada poliitikate ja saada märgid, mis kõne web API-d.

### <a name="install-msal"></a>Installige MSAL
Lisage MSAL, et selle `TaskClient` projekti Visual Studio Package Manager konsooli abil.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Sisestage oma B2C üksikasjad
Avage fail `Globals.cs` ja asendage iga atribuudi väärtused ise. See tund on kasutusel `TaskClient` viide, mida kasutatakse väärtustega.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Funktsiooni PublicClientApplication loomine
Esmane klassi MSAL on `PublicClientApplication`. See tund tähistab rakenduse Azure AD B2C süsteemi. Rakenduse initalizes, näiteks loomisel `PublicClientApplication` sisse `MainWindow.xaml.cs`. Seda saab kasutada kogu aken.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Kontaktiga registreerumise meilivoo
Kui kasutaja valib märgid üles, soovite käivitada registreerumise kulgemist, mis kasutab loodud registreerumise poliitika. MSAL abil saate lihtsalt helistada `pca.AcquireTokenAsync(...)`. Te kaotate parameetrid `AcquireTokenAsync(...)` kindlaks teha, millised luba saate, kasutatakse autentimise taotluse ja muu poliitika.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Kontaktiga meilivoo sisselogimine
Saate algatada on voog töönumbril registreerumise meilivoo samal viisil. Kui kasutaja sisse logib, veenduge sama kõne MSAL, et sel ajal, kasutades teie poliitika.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Kontaktiga on Redigeeri profiili meilivoo
Klõpsake uuesti käivitamiseks Redigeeri profiili poliitika samal viisil:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Kõigil juhtudel MSAL kas tagastab on luba `AuthenticationResult` või põhjustab erandi. Iga kord, kui märgiks toomine MSAL, saate kasutada funktsiooni `AuthenticationResult.User` objekti rakenduses, nt UI kasutaja andmeid värskendada. ADAL vahemälu ka mujal rakenduse kasutamiseks luba.


### <a name="check-for-tokens-on-app-start"></a>Märkide rakenduse avakuval kontrollimine
Samuti saate MSAL silma peal kasutaja sisselogimine olekus.  See rakendus on soovime kasutaja väljalogimiseks isegi juhul, kui nad rakenduse sulgeda ja uuesti avada.  Tagasi sees on `OnInitialized` alistada, kasutage MSAL's `AcquireTokenSilent` meetodi abil otsida vahemälus talletatud sõned:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>Tööülesande API kõne
Nüüd olete kasutanud MSAL käivitada poliitikate ja saada sõned.  Kui soovite kasutada ühte nende tähiste tööülesande API helistamiseks, saate uuesti MSAL's `AcquireTokenSilent` meetodi abil otsida vahemälus talletatud sõned:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Kui kõne `AcquireTokenSilentAsync(...)` õnnestus ja märgiks on leitud vahemälu, saate lisada luba, et selle `Authorization` HTTP-päringu päis. Tööülesande veebi-API kasutatakse kinnitamiseks taotluse lugeda kasutaja Ülesandeloend see päis:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Kasutaja väljalogimine
Lisaks saate MSAL lõpetamiseks kasutaja seansi rakendusega, kui kasutaja valib **Logi välja**.  Kui kasutate MSAL, see on võimalik kõik sõned Turbeloa vahemälu tühjendamine:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Käivitage rakendus näidis

Lõpuks koostamine ja käivitage valimi.  Registreeruge rakenduse, kasutades e-posti aadress või kasutaja nimi. Logige välja ja logige uuesti sisse sama kasutajana. Teise kasutaja profiili redigeerimine. Logige välja ja registreeruda mõne teise kasutaja abil.

## <a name="add-social-idps"></a>Lisage social sisepagulaste reintegreerimine

Rakendus toetab praegu, vaid kasutaja sisse logida ja logige sisse kasutavate **kohalikud kontod**. Need on talletatakse teie B2C kontod, mis kasutavad kasutajanime ja parooli. Azure AD B2C abil saate lisada oma koodi muutmata tugi muude identiteedipakkujad (IDPs).

Rakenduse suhtlusvõrgu sisepagulaste reintegreerimine lisamiseks alustage artiklitest üksikasjalikke juhiseid järgides. Iga IDP soovite toetada, tuleb teil registreerida rakenduse, et süsteemi ja saada kliendi ID-ga.

- [Kui mõni IDP Facebooki häälestamine](active-directory-b2c-setup-fb-app.md)
- [Kui mõni IDP Google häälestamine](active-directory-b2c-setup-goog-app.md)
- [Kui mõni IDP Amazon häälestamine](active-directory-b2c-setup-amzn-app.md)
- [Kui ka IDP LinkedIni häälestamine](active-directory-b2c-setup-li-app.md)

Pärast selle identiteedipakkujad B2C kataloogi lisamist peate iga uue ümberasustatud isikule, lisada oma kolme poliitika redigeerimine [poliitika artiklis](active-directory-b2c-reference-policies.md)kirjeldatud. Pärast seda, kui salvestate oma poliitikad, käivitage rakendus uuesti. Peaksite nägema uue IDPs, mis on lisatud sisselogimise ja kogemusi registreerumise suvandid iga teie identiteedi.

Saate oma poliitikate katsetada ja jälgida oma valimi rakenduse mõju. Lisamine või eemaldamine sisepagulaste reintegreerimine, töödelda rakenduse nõuded või registreerumise atribuute muuta. Proovida, kuni näete, kuidas poliitika, taotluste autentimise ja MSAL siduda.

Viite lõplikus näide [on esitatud ZIP-faili](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Te saate ka klooni GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
