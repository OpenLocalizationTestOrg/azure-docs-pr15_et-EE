<properties
    pageTitle="Autentimise mobiilirakenduste Xamarin Android kasutamise alustamine"
    description="Saate teada, kuidas kasutada mobiilirakenduste Xamarin Androidi rakenduse kaudu identiteedipakkujad, sh AAD, Google, Facebooki, Twitteri ja Microsoft kasutajate autentimiseks."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Rakenduse Xamarin.Android autentimise lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Selles teemas näidatakse, kuidas kasutajad Mobile'i rakendus klientrakenduse kaudu autentida. Selles õppetükis saate lisada autentimise Kiirjuhend projekti identiteedipakkuja juures, mis toetab Azure mobiilirakenduste kaudu. Pärast on edukalt autenditud ja volitatud rakenduses Mobile, kuvatakse kasutaja ID-d väärtus.

Selle õpetuse põhineb Kiirjuhend Mobile'i rakendus. Ka esmalt peate teostama õpetuse [Xamarin.Android rakenduse loomine]. Kui te ei kasuta allalaaditud Lühijuhend serveri projekt, peate lisama autentimise laiend paketi projekti. Serveri laiend pakettide kohta leiate lisateavet teemast [kirjutamata .net-i serveri SDK Azure'i mobiilirakenduste töötamine](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Registreerida rakenduse autentimiseks ja rakenduse teenuste konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Visual Studio või Xamarin Studio, käivitada kliendi projekti seadme või emulaator. Veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub. See juhtub, sest rakendus proovib juurde pääseda oma Mobile'i rakendus kirjutamata autentimata kasutajana. Tabeli *TodoItem* nüüd nõuab autentimist.

Järgmiseks saate uuendab kliendi rakenduse taotluse ressurssidele kirjutamata mobiilirakenduse kaudu autenditud kasutaja.

##<a name="add-authentication"></a>Autentimise lisamine rakendusse

Rakendust värskendatakse, et kasutajad, koputage nuppu **Logi sisse** ja autentimiseks enne kuvatakse andmed.

1. Saate lisada **TodoActivity** klassi järgmine kood:

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    See loob uue meetodi autentida kasutaja ja meetod sündmuseohjuri uus nupp **Logi sisse** . Ülaltoodud näites kood kasutaja autenditakse Facebook login abil. Dialoogiboksi abil kuvatakse üks kord autenditud kasutaja ID-d.

    > [AZURE.NOTE] Kui kasutate identiteedi pakkuja peale Facebooki, edasi **LoginAsync** kohal ühte järgmistest väärtust muuta: _MicrosoftAccount_, _Twitteri_, _Google_või _WindowsAzureActiveDirectory_.

3. **OnCreate** meetod, kustutamine või kommentaari välja järgmine rida koodi.

        OnRefreshItemsSelected ();

4. Activity_To_Do.axml faili, lisage järgmised *LoginUser* nupu määratlus enne olemasoleva *AddItem* nupp.

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Strings.xml ressursifaili järgmine element lisamiseks tehke järgmist.

        <string name="login_button_text">Sign in</string>

6. Visual Studio või Xamarin Studio, käivitada kliendi projekti seadme või emulaator ja logige sisse oma valitud identiteedipakkuja.

    Kui olete edukalt sisseloginud, rakenduses kuvatakse teie sisselogimise ID ja todo üksuste loendit ja saate uuendada andmeid.


<!-- URLs. -->
[Xamarin.Android rakenduse loomine]: app-service-mobile-xamarin-android-get-started.md
