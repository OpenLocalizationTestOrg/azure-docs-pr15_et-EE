
1. Avage fail ühiskasutusse antud projekti MainPage.cs ja lisada järgmised koodilõigu Avaleht klassi.
    
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

3. Kommentaari välja või kustutage olemasolev **OnNavigatedTo** meetod alistada **RefreshTodoItems** meetod kõne.

    See takistab andmete laadimist enne, kui kasutaja on autenditud. Järgmiseks tuleb lisada **logige sisse** nupp rakendus, mis käivitab autentimist.

4. Klassi Avaleht järgmist koodilõigu lisamiseks tehke järgmist.

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Projectis Windowsi poe rakenduse MainPage.xaml projekti faili avada ja lisamine **nuppu** järgmine element just enne element, mis määratleb nupp **Salvesta** .

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. Windows Phone Store app projekti, lisage järgmised **nupp** elemendi **ContentPanel**pärast **tekstivälja** elemendi:

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Ühiskasutusega App.xaml.cs projekti faili avada ja lisage järgmine kood:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Kui **OnActivated** meetod on juba olemas, lisage soovitud `#if...#endif` kood plokk.

9. Windowsi poe rakenduse käivitada, klõpsake nuppu **Logi sisse** ja teie valitud identiteedipakkuja juures rakendusse sisse logida klahvi F5. 

    Kui olete edukalt sisseloginud, rakendus peaks töötama vigadeta ja peaks oskama päringu oma taustväärtus ja andmete värskendused.

10. Paremklõpsake Windows Phone Store app projekti, saate määrata **käivitus projekti**ja seejärel korrake eelmist toimingut kontrollida, kas Windows Phone poe rakendus töötab õigesti.  

 