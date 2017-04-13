<properties
    pageTitle="Azure'i teatis jaoturi abil saatmise turvaline tõuketeatised"
    description="Saate teada, kuidas saatmiseks Androidi rakenduse turvaline Tõuketeatiste Azure. Kirjutatud Java ja C# koodi näidised."
    documentationCenter="android"
    keywords="tõuketeatised teatis Tõuketeatiste, push sõnumite Androidi tõuketeatised"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Azure'i teatis jaoturi abil saatmise turvaline tõuketeatised

> [AZURE.SELECTOR]
- [Windowsi universaalne](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS-i](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Ülevaade

> [AZURE.IMPORTANT] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Tõuketeatised teatis tugi Microsoft Azure võimaldab teil lihtne kasutada, paljudele, mastaabitud kontorist tõuketeatised sõnumi taristu, mis lihtsustab rakendamise Tõuketeatiste tarbija ja ettevõtte puhul mobiilside kaudu juurdepääsu.

Tõttu nõuetele või turvalisus piiranguid, mõnikord rakenduse võib soovite kaasata midagi teate, et ei saa edastada standard tõuketeatised teatis taristu kaudu. Selles õppetükis kirjeldatakse, kuidas saavutada sama kogemus saatmisega tundliku teabe turvaline, autenditud kliendi Androidi seade ja rakenduse taustväärtus vahelise ühenduse kaudu.

Kõrge, voo on järgmine:

1. Rakenduse tagaandmebaas:
    - Salvestab turvaline last Tagaandmebaasi.
    - Saadab ID-d see teatis (pole turvaline teave saadetakse) Androidi seade.
2. Rakendus töötab seadme taustal, kui nad saavad teate:
    - Androidi seadmes kontaktide paluda turvaline last tagaandmebaas.
    - Rakenduse saate kuvada soovitud last kujul seadmesse teatis.

See on oluline märkida, et eelmise voogu (ja selle õpetuse) me oletame, et seade talletab mõne autentimise luba kohalike salvestusruumi, pärast kasutaja logib sisse. See tagab täielikult tõrgeteta, kui seadme saate tuua teate turvaline last selle märgiks abil. Kui rakenduse salvestada seadme autentimine märkide või kui nende tähiste saate aegunud, seadme rakendus, saades tõuketeatised teatis peaks olema kuvatud üldise teatise viipa kasutaja käivitage rakendus. Rakenduse siis autendib kasutaja ja kuvab teate last.

Selle õpetuse näitab, kuidas turvaline Tõuketeatiste saatmine. See põhineb õpetuse [Kasutajad teatavad](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) , et teil tuleb täita juhised selles õpetuses esmalt kui te pole seda veel teinud.

> [AZURE.NOTE] Selle õpetuse eeldab, et olete loonud ja oma teate jaoturi konfigureeritud, nagu on kirjeldatud [Alustamine teatis jaoturi (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Androidi projekti muutmine

Nüüd, kui saate muuta oma rakenduse tagaandmebaas lihtsalt *id* tõuketeatised teatise saata, tuleb muuta oma Androidi käsitlemiseks teatise ja kõne tagasi oma tagaandmebaas kuvada turvalist sõnumit alla laadida.
Selle eesmärgi saavutamiseks tuleb veenduge, et teie Androidi oskab autentimiseks ise oma tagaandmebaas saab tõuketeatised teatisi.

Me nüüd muuta *sisselogimise* kulgemist selleks, et salvestada ühiskasutusega eelistustes rakenduse autentimise päise väärtus. Analoogne menetlustele saab kasutada mis tahes autentimise luba (nt OAuthi sõned) rakendus on kasutada kasutajatunnust nõudmata talletamiseks.

1. Androidi projektis lisage järgmised konstantide **MainActivity** klassi ülaosas.

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Veel **MainActivity** klassis, värskendage selle `getAuthorizationHeader()` meetod, mis sisaldab järgmine kood:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Lisage järgmine `import` laused **MainActivity** faili ülaosas:

        import android.content.SharedPreferences;

Nüüd saame muudab sündmuseohjuri, mida nimetatakse kui teate kättesaamise.

4. Klõpsake **MyHandler** klassi muutmine soovitud `OnReceive()` meetod, mis sisaldab:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Lisage soovitud `retrieveNotification()` meetod, asendades kohatäite `{back-end endpoint}` koos oma tagaandmebaas juurutamisel saadud tagaandmebaas lõpp-punkti:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


See meetod helistab teie rakendus tagaandmebaas teate sisu, mis on talletatud ühiskasutusega eelistused mandaadi abil tuua ja kuvab selle tavaline teatis. Teate näeb välja täpselt nagu mis tahes tõuketeatised teatis rakenduse kasutajale.

Pange tähele, et eelistada toime tagasi-lõpuks puuduvad autentimise päise atribuudi või hülgamiseks. Teatud juhtudel käsitsemise sõltuvad enamasti oma target kasutusvõimalused. Üheks võimaluseks on teatis koos üldise viip kasutaja autentida tuua tegeliku teate kuvamiseks.

## <a name="run-the-application"></a>Käivitage rakendus

Rakenduse käivitamiseks tehke järgmist.

1. Veenduge, et **AppBackend** on juurutatud Azure. Visual Studio kasutamisel **AppBackend** Veebiteenuste rakenduse käivitada. ASP.net-i veebileht, mis kuvatakse.

2. Eclipse, käivitage rakendus füüsilise Androidi seadmega või emulaator.

3. Androidi rakenduse kasutajaliides, sisestage kasutajanimi ja parool. Need võivad olla mis tahes stringi, kuid need peavad olema sama väärtuse.

4. Androidi UI nuppu **Logi sisse**. Klõpsake nuppu **saada tõuketeatised**.
