<properties
    pageTitle="Alustamine autentimise mobiilirakenduste Xamarin.Forms rakenduses | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada mobiilirakenduste Xamarin vormide rakenduse kaudu identiteedipakkujad, sh AAD, Google, Facebooki, Twitteri ja Microsoft kasutajate autentimiseks."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Rakenduse Xamarin.Forms autentimise lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Ülevaade

See teema näitab, kuidas rakenduse teenuse Mobile rakenduse kaudu klientrakenduse kasutajate autentimiseks. Selles õpetuses lisate autentimine Xamarin.Forms Kiirjuhend projekti identiteedipakkuja juures, mis toetab rakenduse teenuse abil. Pärast edukalt autenditud ja Mobile rakenduse lubatud, kuvatakse väärtus kasutaja ID ja saab juurdepääsu piiratud tabeli andmeid.

##<a name="prerequisites"></a>Eeltingimused

Parima tulemuse selles õpetuses, soovitame esmalt lõpule viia õpetuse [Xamarin.Forms rakenduse loomine](app-service-mobile-xamarin-forms-get-started.md) . Kui olete selle õpetuse, on teil Xamarin.Forms projekt, mis on mitme platvormi TodoList rakendus.

Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate lisama autentimise laiend paketi projekti. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registreerima rakenduse autentimiseks ja rakenduse teenuste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Portable klassiteek autentimise lisamine

Mobiilirakenduste kasutab [LoginAsync] laiendamine meetod [MobileServiceClient] sisse logima kasutaja autentimise rakendus. See näide kasutab serveri haldusega autentimise kulgemist, rakenduse pakkuja sisselogimise kasutajaliidese kuvava. Lisateabe saamiseks vt [serveri haldusega autentimist](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Hõlpsamini tootmise rakenduse andmiseks võite kaaluda hoopis [Kliendi hallatavad autentimist](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow)kasutades. 

Autentimiseks Xamarin.Forms projekti, saate määratleda liidest **IAuthenticate** rakenduse jagamise Klassiteek. Saate värskendada ka kasutajaliides, mis on määratletud kaasaskantav klassi Raamatukogu **sisselogimise** nupu, mida kasutaja klõpsab alustamiseks autentimine lisada. Pärast edukat autentimist, laaditakse andmed mobiilirakenduse taustväärtus.

Peate rakendama **IAuthenticate** kasutajaliidese iga platvormi rakenduse ei toeta.


1. Visual Studio või Xamarin Studio, lisage avatud projekti **Portable** nimi, mis on kaasaskantav klassi Raamatukogu projekti, kus App.cs järgmine `using` lause:

        using System.Threading.Tasks;

2. App.cs, lisage järgmine `IAuthenticate` liidese määratlus vahetult enne selle `App` klassi määratlus.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Järgmised staatilise liikmete lisamine **rakenduse** klassi liides platvormi teatud rakendamist.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Kaasaskantav klassi Raamatukogu projekti TodoList.xaml avamine, lisage järgmine **nupp** element *buttonsPanel* paigutus elementi, pärast olemasoleva nupp: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    See nupp käivitab oma mobiilirakenduse kirjutamata autentimine serveri haldusega.

5. Kaasaskantav klassi Raamatukogu projekti TodoList.xaml.cs avamine ja seejärel lisage järgmine väli on `TodoList` klassi:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Asendage **OnAppearing** meetod järgmine kood:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    See tagab, et ainult värskendamisel teenuse pärast seda, kui kasutaja on autenditud.

7. Klassi **TodoList** järgmised sündmuseohjuri **Clicked** sündmuse lisamiseks tehke järgmist.

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Muudatuste salvestamiseks ja taastada kaasaskantav klassi Raamatukogu projekti kinnitatava tõrkeid.


##<a name="add-authentication-to-the-android-app"></a>Androidi rakenduse lisamine autentimine

Selles jaotises kirjeldatakse, kuidas rakendada **IAuthenticate** kasutajaliidese Androidi rakenduse project. Selle jaotise vahele jätta, kui teil on toetada Android-seadmetes.

1. Visual Studio või Xamarin Studio, paremklõpsake **droid** projekt, siis **käivitus projekti seadmine**.

2. Vajutage klahvi F5, et alustada projekti siluris, siis veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub. See juhtub siis soovitud taustväärtus juurdepääsu on piiratud ainult autoriseeritud kasutajad.

3. Avage Androidi projekti MainActivity.cs ja lisage järgmine `using` laused:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Värskendage **MainActivity** klassi rakendada **IAuthenticate** kasutajaliides, järgmiselt.

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Värskendada **MainActivity** ainekursust, lisades **MobileServiceUser** välja ja **autentimise** meetodit, mida on vaja **IAuthenticate** kasutajaliides, järgmiselt:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Kui kasutate identiteedi pakkuja peale Facebooki, valige muu väärtuse [MobileServiceAuthenticationProvider].

6. Lisada järgmine kood **OnCreate** meetodit **MainActivity** klassi enne kõne `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    See tagab, et enne laadimise rakendus on lähtestatud Autentija.

7. Taastada rakendus, käivitage see ja seejärel logige sisse autentimisteenuse pakkuja valitud ja veenduge, et teil on võimalik Accessi andmed autenditud kasutajana.

##<a name="add-authentication-to-the-ios-app"></a>IOS-i rakendus autentimise lisamine

Selles jaotises kirjeldatakse, kuidas rakendada **IAuthenticate** kasutajaliidese iOS-i rakenduse project. Kui teil on toetada iOS-seadmete selle jaotise vahele jätta.

1. Visual Studio või Xamarin Studio, paremklõpsake **iOS-i** projekt, siis **käivitus projekti seadmine**.

2. Vajutage klahvi F5 siluris projekti alustamine, siis veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub. See juhtub siis soovitud taustväärtus juurdepääsu on piiratud ainult autoriseeritud kasutajad.

4. Avage AppDelegate.cs iOS-i projekti ja lisage järgmine `using` laused:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Värskendage **AppDelegate** klassi rakendada **IAuthenticate** kasutajaliides, järgmiselt.

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Värskendada **AppDelegate** ainekursust, lisades **MobileServiceUser** välja ja **autentimise** meetodit, mida on vaja **IAuthenticate** kasutajaliides, järgmiselt:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Kui kasutate identiteedi pakkuja peale Facebooki, valige muu väärtuse [MobileServiceAuthenticationProvider].

6. Järgmine rida koodi lisamine **FinishedLaunching** meetodit enne kõne `LoadApplication()`: 

        App.Init(this);

    See tagab, et Autentija on lähtestatud enne rakenduse.

7. Taastada rakendus, käivitage see ja seejärel logige sisse autentimisteenuse pakkuja valitud ja veenduge, et teil on võimalik Accessi andmed autenditud kasutajana.


##<a name="add-authentication-to-windows-app-projects"></a>Autentimist lisamine Windowsi rakenduse projektid

Selles jaotises kirjeldatakse, kuidas rakendada **IAuthenticate** kasutajaliidese Windows 8.1 ja Windows Phone 8.1 rakendus projektid. Samad juhised kehtivad Universal platvormil (UWP) projektide jaoks. Selle jaotise vahele jätta, kui teil on toetada Windowsi jaoks.

1. Paremklõpsake Visual Studios, kas **WinApp** või **WinPhone81** projekt, siis **käivitus projekti seadmine**.

2. Vajutage klahvi F5 siluris projekti alustamine, siis veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub. See juhtub siis soovitud taustväärtus juurdepääsu on piiratud ainult autoriseeritud kasutajad.

3. Avage Windowsi rakenduse projekti MainPage.xaml.cs ja lisage järgmine `using` laused:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Asendage `<your_Portable_Class_Library_namespace>` koos nimeruumi teegi jagamise tunni jaoks.

4. Värskendage **Avaleht** klassi rakendada **IAuthenticate** kasutajaliides, järgmiselt.

        public sealed partial class MainPage : IAuthenticate


5. Värskendada **Avaleht** ainekursust, lisades **MobileServiceUser** välja ja **autentimise** meetodit, mida on vaja **IAuthenticate** kasutajaliides, järgmiselt:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Kui kasutate identiteedi pakkuja peale Facebooki, valige muu väärtuse [MobileServiceAuthenticationProvider].

6. Lisage järgmine rida koodi ehitaja **Avaleht** klassi enne kõne `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Asendage `<your_Portable_Class_Library_namespace>` koos nimeruumi teegi jagamise tunni jaoks.  
    Kui see on WinApp projekti, saate vahele jätta samm 8 allapoole. Järgmiseks kehtib ainult WinPhone81 projekt, kus peate täitma tagasihelistamise Logi sisse.

7. (Valikuline) Projectis **WinPhone81** rakenduse avamine App.xaml.cs ja lisage järgmine `using` laused:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Asendage `<your_Portable_Class_Library_namespace>` koos nimeruumi teegi jagamise tunni jaoks.

8.  **Rakenduse** klassi järgmised **OnActivated** meetod alistada lisamiseks tehke järgmist.

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Kui meetod alistada juba olemas, lisage lihtsalt tingimusvormingu koodi ülaltoodud koodilõigu.

7. Taastada rakendus, käivitage see ja seejärel logige sisse autentimisteenuse pakkuja valitud ja veenduge, et teil on võimalik Accessi andmed autenditud kasutajana.

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui te täitnud õppeteema elementaarautentimine, kaaluge jätkuva ühte järgmistest õpetused edasi.

+ [Tõuketeatiste lisamiseks rakenduse](app-service-mobile-xamarin-forms-get-started-push.md)  
  Saate teada, kuidas lisada push teatised rakenduse tugi ja konfigureerida oma mobiilirakenduse kirjutamata saatmiseks tõuketeatisi Azure'i teatis jaoturi abil.

+ [Rakenduse ühenduseta sünkroonimise lubamine](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil ka Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

