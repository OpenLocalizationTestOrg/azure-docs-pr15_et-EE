
Eelmise näite ilmnes standard sisselogimine, mis nõuab kliendi pöörduda identiteedipakkuja nii kirjutamata Azure'i teenus iga kord, kui rakendus käivitub. Mitte ainult on selle meetodi ebaefektiivne, saate käivitada kasutus seotud probleemi peaks paljud kliendid proovivad käivitage rakendus, samal ajal. Parem lähenemine on tagastatud Azure teenuse autoriseerimine luba vahemälu ja proovige kasutada kõigepealt enne kasutamist pakkuja-põhine sisselogimine. 

>[AZURE.NOTE]Saate vahemälu luba välja kirjutamata Azure'i teenus sõltumata sellest, kas kasutate kliendi hallatavate või teenuse haldusega autentimist. Selle õpetuse kasutab teenuse haldusega autentimist.


1. Avage fail ToDoActivity.java ja lisada impordi järgmistest:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Lisage soovitud järgmistel liikmetel on `ToDoActivity` klassi.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Failis ToDoActivity.java lisamine on järgmine definitsiooni selle `cacheUserToken` meetod.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    See meetod salvestab kasutaja id ja luba eelistuste faili, mis privaatseks märgitud. See peaks juurdepääsu vahemälu kaitsta nii, et muude rakenduste seadmes on juurdepääsu luba, kuna eelistus on liivakastikoodi rakenduse. Kui keegi saab juurdepääsu seadet, on võimalik, et nad võivad pääseda Turbeloa vahemälu muul viisil. 

    >[AZURE.NOTE]Kui teie andmetele juurdepääsu Turbeloa on väga tundlikuks ja keegi teine võib pääseda seade, saate täiendavalt luba krüptimise abil kaitsta. Täiesti turvaline lahendus on aga selles Õpetuses ja sõltuvad teie turbenõuetele väljapoole.


4. Failis ToDoActivity.java lisamine on järgmine definitsiooni selle `loadUserTokenCache` meetod.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Failis *ToDoActivity.java* asendada selle `authenticate` meetod järgmine meetod, mis kasutab Turbeloa vahemälu. Kui soovite kasutada konto peale Google, muutke sisselogimise pakkuja.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Rakendus ja testige autentimine, kasutades kehtiv konto luua. Käivitage see vähemalt kaks korda. Esimese ajal peaks kuvatakse viip ja Turbeloa vahemälu luua. Pärast seda, iga Käivita proovib laadimine Turbeloa vahemälu autentimiseks ja te ei tohiks nõuda login.



