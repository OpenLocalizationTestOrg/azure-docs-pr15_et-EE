
1. Valige **Project Exploreri** Androidi Studios ToDoActivity.java faili avada ja lisage impordi järgmistest.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Saate lisada **ToDoActivity** klassi järgmisel viisil: 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    See loob uue meetodi käsitlema autentimist. Kasutaja autenditakse, kasutades Google sisselogimist. Kuvatakse dialoog, mis kuvab autenditud kasutaja ID-d. Te ei saa jätkata ilma positiivne autentimist.

    > [AZURE.NOTE] Kui kasutate mõnda muud identiteedipakkuja, edasi ühte järgmistest eeltoodud meetodi **login** väärtust muuta: _MicrosoftAccount_, _Facebooki_, _Twitteri_või _windowsazureactivedirectory_.

3. **OnCreate** meetod, lisage järgmine rida koodi pärast instantiates koodis on `MobileServiceClient` objekti.

        authenticate();

    See Helista alustab autentimist.

4. Liikumine pärast ülejäänud kood `authenticate();` **onCreate** meetod uus **createTable** meetod, mis näeb välja umbes järgmine:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Menüüst **käivitada** , klõpsake nuppu **Käivita rakendus** , käivitage rakendus ja logige sisse oma valitud identiteedipakkuja. 

    Kui olete edukalt sisseloginud, rakendus peaks töötama vigadeta ja peaks oskama päringu taustväärtus teenuse ja andmete värskendused.