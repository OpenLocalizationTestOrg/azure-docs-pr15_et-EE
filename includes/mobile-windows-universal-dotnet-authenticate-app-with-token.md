
1. Lisage failis MainPage.xaml.cs projekti **abil** järgmistest.

        using System.Linq;      
        using Windows.Security.Credentials;

2. Asendage **AuthenticateAsync** meetod järgmine kood:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    Selles versioonis **AuthenticateAsync**rakendus proovib kasutada **PasswordVault** talletatud identimisteabe teenuse. Tavaline sisselogimine on ka läbi korral pole salvestatud mandaati.

    >[AZURE.NOTE]Vahemällu salvestatud luba on aegunud ja Turbeloa aegumise pärast autentimist võib tekkida ka, kui kasutusel on rakenduse. Siit saate teada, kuidas kindlaks teha, kui märgiks on aegunud, lugege teemat [aegunud autentimine märkide kontrollida](http://aka.ms/jww5vp). Lahendus töötlemise luba aeguvad sõned seotud tõrgete, vt postituse [SDK hallatavate vahemälu ja töötlemise aegunud märkide Azure Mobile teenused](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Taaskäivitage rakendus kaks korda.

    Teade, et esimesel käivitamisel, logige sisse pakkuja uuesti nõutakse. Siiski teise taaskäivitamisel kasutatakse vahemällu talletatud identimisteabe ja sisselogimist mööduda. 
