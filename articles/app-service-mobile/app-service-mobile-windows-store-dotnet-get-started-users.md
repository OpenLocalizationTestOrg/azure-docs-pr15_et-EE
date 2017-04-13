<properties
    pageTitle="Autentimise lisamine rakenduse Universal platvormil (UWP) | Azure'i mobiilirakendused"
    description="Saate teada, kuidas kasutada Azure rakenduse teenuse mobiilirakenduste rakenduse Universal platvormil (UWP), kasutades erinevaid identiteedipakkujad, sh kasutajate autentimiseks: AAD, Google, Facebooki, Twitteri ja Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Rakenduse Windowsi autentimist lisamine

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Selles teemas näidatakse, kuidas lisamiseks oma mobiilirakenduse pilvepõhist autentimist. Selles õpetuses lisamist autentimise Universal platvormil (UWP) Kiirjuhend projekti mobiilirakenduste kaudu identiteedipakkuja juures, mis toetab Azure'i rakendust Service. Pärast on edukalt autenditud ja oma mobiilirakenduse kirjutamata lubatud, kuvatakse kasutaja ID väärtus.

Selle õpetuse põhineb Kiirjuhend mobiilirakenduste kohta. Esmalt peate teostama õpetusega [Alustamine mobiilirakenduste kohta](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Registreerimist rakenduse autentimiseks ja rakenduse teenuse konfigureerimine

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Autenditud kasutaja õiguste piiramine

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nüüd saate kontrollida oma kirjutamata anonüümse juurdepääsu keelamist. Projekti UWP rakenduse käivitamine projekti seadmine, juurutada ja käivitage rakendus; Veenduge, et 401 (volitamata) olek koodiga töötlemata erandi tõstetakse pärast rakendus käivitub. See juhtub, sest rakendus proovib juurde pääseda oma Mobile rakenduse koodi autentimata kasutajana, kuid tabeli *TodoItem* nüüd nõuab autentimist.

Järgmiseks värskendate rakenduse kasutajate autentimiseks enne taotluse ressursse rakenduse teenusepakkuja.

##<a name="add-authentication"></a>Autentimise lisamine rakendusse

1. Funktsiooni UWP rakenduse project faili MainPage.cs ja lisada järgmised koodilõigu Avaleht klassi.
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Järgmine kood autendib kasutaja Facebooki sisselogimist. Kui kasutate identiteedi pakkuja peale Facebooki, muuta **MobileServiceAuthenticationProvider** kohal väärtus väärtus pakkuja.

3. Kommentaari välja või kustutada olemasolevate **OnNavigatedTo** meetod alistada kõne **ButtonRefresh_Click** meetod (või **InitLocalStoreAsync** meetod). See takistab andmete laadimist enne, kui kasutaja on autenditud. Järgmiseks tuleb lisada **logige sisse** nupp rakendus, mis käivitab autentimist.

4. Lisage järgmised koodilõigu Avaleht klassi.

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Avage MainPage.xaml projekti fail, otsige element, mis määratleb nupp **Salvesta** ja asendage see järgmine kood:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Käivitage rakendus, klõpsake nuppu **Logi sisse** ja teie valitud identiteedipakkuja juures rakendusse sisse logida klahvi F5. Kui teie sisselogimine on eduka, rakendus töötab vigadeta ja teil on võimalik päringu oma taustväärtus ja andmete värskendused.


##<a name="tokens"></a>Kliendi autentimine luba talletamine

Eelmise näite ilmnes standard sisselogimine, mis nõuab kliendi pöörduda identiteedipakkuja nii teenuse rakenduse iga kord, kui rakendus käivitub. Mitte ainult on selle meetodi ebaefektiivne, saate tekib kasutus-seotud probleemid peaks paljud kliendid proovivad käivitage rakendus, samal ajal. Parem lähenemine on tagastatud rakenduse teenust autoriseerimine luba vahemälu ja proovige kasutada kõigepealt enne kasutamist pakkuja-põhine sisselogimine.

>[AZURE.NOTE]Saate vahemälu luba välja rakenduse teenuste sõltumata sellest, kas kasutate kliendi hallatavate või teenuse haldusega autentimist. Selle õpetuse kasutab teenuse haldusega autentimist.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui te täitnud õppeteema elementaarautentimine, kaaluge jätkuva ühte järgmistest õpetused edasi.

+ [Tõuketeatiste lisamiseks rakenduse](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Saate teada, kuidas lisada push teatised rakenduse tugi ja konfigureerida oma Mobile'i rakendus kirjutamata saatmiseks tõuketeatisi Azure'i teatis jaoturi abil.

+ [Rakenduse ühenduseta sünkroonimise lubamine](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Saate teada, kuidas lisada ühenduseta režiimi tugi rakenduse abil ka Mobile'i rakendus taustväärtus. Ühenduseta sünkroonimine võimaldab lõppkasutajad suhelda mobiilirakenduses&mdash;vaatamine, lisamine või muutmine andmete&mdash;isegi siis, kui seal on pole võrguühendust.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

