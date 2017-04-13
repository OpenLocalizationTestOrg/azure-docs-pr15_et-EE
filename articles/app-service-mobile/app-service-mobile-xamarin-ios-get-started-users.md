<properties
    pageTitle="Alustamine Xamarin iOS-i mobiilirakenduste autentimine"
    description="Saate teada, kuidas kasutada mobiilirakenduste Xamarin iOS-i rakenduse kaudu identiteedipakkujad, sh AAD, Google, Facebooki, Twitteri ja Microsoft kasutajate autentimiseks."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Rakenduse Xamarin.iOS autentimise lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

See teema näitab, kuidas rakenduse teenuse Mobile rakenduse kaudu klientrakenduse kasutajate autentimiseks. Selles õppetükis saate lisada autentimine Xamarin.iOS Kiirjuhend projekti identiteedipakkuja juures, mis toetab rakenduse teenuse abil. Pärast edukalt autenditud ja Mobile rakenduse lubatud, kuvatakse väärtus kasutaja ID ja saab juurdepääsu piiratud tabeli andmeid.

Esmalt peate teostama õpetuse [Xamarin.iOS rakenduse loomine]. Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate lisama autentimise laiend paketi projekti. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registreerida rakenduse autentimiseks ja rakenduse teenuste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. klõpsake Visual Studio või Xamarin Studio, käivitada kliendi projekti seadme või emulaator. Veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub. Selle logitakse siluri konsooli. Seega peaksite nägema Visual Studio tõrge väljundi aknas.

&nbsp;&nbsp;Volitamata nurjumise juhtub, sest rakendus proovib juurde pääseda oma mobiilirakenduse kirjutamata autentimata kasutajana. Tabeli *TodoItem* nüüd nõuab autentimist.

Järgmiseks saate uuendab kliendi rakenduse taotluse ressurssidele kirjutamata mobiilirakenduse kaudu autenditud kasutaja.

##<a name="add-authentication-to-the-app"></a>Autentimise lisamine rakendusse

Selles jaotises muudate rakenduse kuvamiseks sisselogimise Kuva enne andmete kuvamise. Rakenduse käivitamisel rakenduse teenust ei ei saa ühendust ja ei pruugita kuvada kõik andmed. Pärast esimest korda, et kasutaja teeb värskendamise žest, kuvatakse sisselogimine; pärast edukat sisselogimist kuvatakse todo üksuste loendit.

1. Projectis klienti, avage fail **QSTodoService.cs** ja lisage järgmine kasutamine lause ja `MobileServiceUser` koos juurdepääsumeetodi QSTodoService klassi:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Lisage uus meetod **autentimise** abil **QSTodoService** järgmised määratlemine.


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Kui kasutate identiteedi pakkuja peale Facebook, edasi **LoginAsync** kohal ühte järgmistest väärtust muuta: _MicrosoftAccount_, _Twitteri_, _Google_või _WindowsAzureActiveDirectory_.

3. Avage **QSTodoListViewController.cs**. Meetodi määratlusi **ViewDidLoad** eemaldamine **RefreshAsync()** kõne lähedal end muutmiseks tehke järgmist.

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Muutke meetodit **RefreshAsync** autentida, kui **kasutaja** atribuudi väärtus on null. Lisage järgmine kood ülaosas meetod määratlus.

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. Visual Studio või Xamarin Studio ühendatud oma Xamarin koostada hosti Mac-arvutis, käivitada kliendi projekti suunamise seadme või emulaator. Veenduge, et rakendus kuvab andmed.

    Sooritada värskendamise žest vahelt üksused, mis põhjustavad sisselogimine kuvada loendit. Kui olete sisestanud edukalt sobiv mandaate, rakenduses kuvatakse todo üksuste loendit ja saate uuendada andmeid.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Xamarin.iOS rakenduse loomine]: app-service-mobile-xamarin-ios-get-started.md
