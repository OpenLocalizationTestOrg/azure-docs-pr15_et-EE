<properties
    pageTitle="Azure Active Directory B2C: Kõne veebi-API Androidi rakenduse | Microsoft Azure'i"
    description="Selles artiklis näitab teile, kuidas luua Androidi 'Ülesandeloend' rakendus, mis nõuab Node.js veebi-API OAuth 2.0 esitaja sõned abil. Androidi rakendust nii veebi-API kasutamine Azure Active Directory B2C hallata kasutajate identiteete ja kasutajate autentimiseks."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure'i AD B2C: Kõne veebi-API Androidi rakendusest

> [AZURE.WARNING] Selle õpetuse jaoks on vaja mõned olulised värskendused, spetsiaalselt, et eemaldada B2C ADAL Android kasutamine.  Me avaldada värske juhised Azure AD B2C kasutamine Androidi rakendustega järgmise nädala ja soovitame ettevõttest välja seni.  Kuid kui soovite lihtsalt proovida asju välja, Julgelt jätkake järgmises artiklis.



Azure Active Directory (Azure AD) B2C abil saate lisada oma Androidi rakendused ja veebi API-de paari toiminguga võimsaid iseteenindusliku identiteedi haldusfunktsioonid. Selles artiklis näitab teile, kuidas luua Androidi "Ülesandeloend" rakendus, mis nõuab Node.js veebi-API OAuth 2.0 esitaja sõned abil. Androidi rakendust nii veebi-API abil Azure AD B2C haldamiseks kasutaja identiteedi ja autentimiseks.

See Kiirjuhend eeldab, et web API, mis on kaitstud Azure AD B2C täielikult töötada. Meil on ehitatud ühte .NET-ja Node.js kasutamiseks. Selle walk-through eeldab, et Node.js veebi-API valimi on konfigureeritud. Lisateabe saamiseks lugege teemat [Azure AD B2C veebi-API Node.js õpetuse jaoks](active-directory-b2c-devquickstarts-api-node.md).

Androidi kliendid peavad juurde pääsema kaitstud ressursse, pakub Azure AD Active Directory autentimise Raamatukogu (ADAL). ADAL ainus eesmärk on saada juurdepääsu sõned rakenduse hõlpsalt. Näidata, kui lihtne on, sellest juhendist me tuleb koostada rakenduse Androidi ülesannete loendi mis:

- Accessi märgid, mis kõne Ülesandeloend API [OAuth 2.0 autentimise protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)abil saab.
- Saab kasutajate ülesanded.
- Kasutajate välja märgid.

> [AZURE.NOTE] See artikkel ei hõlma rakendada sisselogimist, registreerumise kohta ja profiili haldus Azure AD B2C abil. See keskendub helistamisel web API-d, kui kasutaja on autenditud. Kui te pole seda veel teinud, peab algama eesliitega [.NET web app töötamise alustamine õpetuse](active-directory-b2c-devquickstarts-web-dotnet.md) Azure AD B2C põhitoimingute kohta.

## <a name="get-an-azure-ad-b2c-directory"></a>Saada on Azure AD B2C kataloog

Enne kasutamist Azure AD B2C, saate luua või rentniku. Kataloogi on ümbris kõigi kasutajate, rakendused, rühmad ja muu. Kui teil pole ühte juba enne [B2C luua](active-directory-b2c-get-started.md) sellest juhendist jätkata.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate B2C kataloogis rakenduse loomine. See annab Azure AD teabest ja rakenduse. Rakenduse nii veebi-API esindavad ühe **Rakenduse ID** sel juhul, kuna need sisaldavad üks loogiline rakendus. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md). Kindlasti järgmist.

- Kaasa või **veebirakenduse**/**veebi-API** rakenduse.
- Sisestage `urn:ietf:wg:oauth:2.0:oob` **vastus URL-i**. See on vaikimisi URL-i seda proovi kood.
- **Rakenduse salajane** rakenduse loomine ja selle kopeerimine. Teil on vaja hiljem. Pange tähele, et see väärtus peab olema [XML-i põgenes](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) enne selle kasutamist.
- Kopeerige **Rakenduse ID** , mis määratakse teie rakendus. On vaja seda hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Azure'i AD B2C, iga kasutusvõimalused on määratletud [poliitika](active-directory-b2c-reference-policies.md). See rakendus sisaldab kolme identiteedi kogemusi: registreeruda, logige sisse ja Logige Facebooki abil sisse.  Peate looma ühe korra iga tüüpi [poliitika artiklis](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kirjeldatud. Kui loote oma kolme poliitikad, ärge unustage:

- Valige oma registreerumise poliitika **kuvatav nimi** ja muud registreerumise atribuudid.
- Valige **kuvatav nimi** ja **Objekti ID** rakenduse nõuded iga poliitika. Soovi korral saate kasutada ka muud nõuded.
- Kopeerige iga poliitika **nimi** pärast selle loomist. Sellel peab olema eesliite `b2c_1_`.  Peate need poliitika nimed hiljem.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kui olete loonud kolm poliitikad, olete valmis koostamiseks oma rakenduse.

Pange tähele, et see artikkel ei hõlma äsja loodud poliitikaid kasutamise kohta. Teavet selle kohta, kuidas toimivad poliitikad Azure AD B2C, alustage [õpetuse .NET web app töötamise alustamine](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Laadige alla kood

See õpetus [säilitatakse github](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android)kood. Valimi koostamiseks, kui lähete, saate [alla laadida skelett projekti ZIP-faili](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Samuti saate klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Teil palutakse alla laadida luukere selles õpetuses lõpuleviimiseks.** Täielikult toimiv Androidi rakenduse rakendamise keerukuse tõttu on luukere Kasutuskogemuse kood, mis käivitub, kui olete lõpetanud selles õpetuses. See on arendajatele aja säästmiseks mõõt. Kasutuskogemuse kood ei ole teema, kuidas lisada B2C Androidi rakenduse üles.

Lõpetatud app on ka [ZIP-faili saadaval](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) või on `complete` haru sama hoidla.

Maven loomiseks saate kasutada `pom.xml` ülatasemel.

  1. Järgige [eeltingimused](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)jaotises Maven Androidi jaoks häälestamiseks.
  2. Saate häälestada emulaator SDK 21.
  3. Kui te kloonitud repo juurkausta minek
  4. Käivitage käsk `mvn clean install`.
  5. Muuta kataloogi Kiirjuhend valimi `cd samples\hello`.
  6. Käivitage käsk `mvn android:deploy android:run`.

Peaksite nägema Käivita rakendus. Sisestage testkasutaja mandaati proovima.

Java Archive (JAR) pakettide esitatakse ka Androidi arhiivi Selgesõnalisemad paketi kõrval.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Laadige alla Androidi ADAL ja lisada selle oma Androidi Studio tööruumi

Teil on võimalusi, kuidas seda kasutada Androidi projektis:

* Saate importida Eclipse ning link teegi rakenduse lähtekoodi.
* Kui kasutate Androidi Studio, saate kasutada AAR paketi vorming ja viidata kahendfaile.

### <a name="option-1-binaries-via-gradle-recommended"></a>Variant 1: Kahendfaile kaudu Gradle (soovitatav)

Saate kahendfaile Maven keskse Repo. AAR paketi saab lisada projekti Androidi Studios (nt `build.gradle`) sellisel viisil:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Variant 2: AAR Maven kaudu

Kui kasutate funktsiooni `m2e` lisandmooduli Eclipse, saate määrata sõltuvus sisse oma `pom.xml` faili:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Võimalus 3: Andmeallika kaudu Git (lõpuks)

Lähtekoodi SDK kaudu Git saamiseks sisestage:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Kasutage funktsiooni haru **lähenemise.**

## <a name="set-up-your-configuration-file"></a>Häälestada oma konfiguratsioonifail

Kasutage konfiguratsiooni, mille seadistasite varasemates B2C portaalis Androidi projekti konfigureerimiseks.

Avatud `helpes/Constants.java` ja sisestage järgmised väärtused:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Otsinguulatuste, mida te kaotate server, millega soovite taotleda tagasi serverist, kui kasutaja sisse logib. B2C eelvaate jaoks olete oma ostuõiguse `client_id`. Siiski see eeldatakse, et muuta `read scopes` tulevikus. Selle dokumendi värskendatakse, mille ilmnemisel.
- `ADDITIONAL_SCOPES`: Täiendavate otsinguulatuste, mida soovite kasutada rakenduse. Eelduste kohaselt edaspidi kasutada.
- `CLIENT_ID`: Rakenduse ID teil portaalist.
- `REDIRECT_URL`: Kui teie oodatav luba sisestatakse tagasi uuesti.
- `EXTRA_QP`: Midagi eest, mida soovite edastada serveri URL-kodeeringuga vormingus.
- `FB_POLICY`: Poliitika on saate kasutada. See on kõige olulisem osa sellest walk-through.
- `EMAIL_SIGNIN_POLICY`: Poliitika on saate kasutada. See on kõige olulisem osa sellest walk-through.
- `EMAIL_SIGNUP_POLICY`: Poliitika on saate kasutada. See on kõige olulisem osa sellest walk-through.

## <a name="add-references-to-android-adal-to-your-project"></a>Androidi ADAL viited lisamine projekti

> [AZURE.NOTE]  ADAL Android kasutab soovidele vastavalt mudelit, mis autonoomsest autentimist. Kavatsused "näha üle" rakenduse töö tegemiseks. Selle kogu valimi ja kõik ADAL for Android, saate hallata kavatsused ning edastavad teavet nende vahel.

Esmalt räägi Androidi rakenduse, sh juhatades, mida soovite kasutada paigutust. Nende kavatsused selgitatakse üksikasjalikult hiljem selles õpetuses.

Värskendage oma projekti `AndroidManifest.xml` faili lisada kõiki oma kavatsused:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Nagu näete, määrake viis tegevusi. Kasutage kõik need.

- `AuthenticationActivity`: See pärineb ADAL ja pakub sisselogimise veebivaade.
- `LoginActivity`: Kuvatakse teie poliitika ja nuppude iga poliitika.
- `SettingsActivity`: Kasutage seda käitusajal rakenduse sätete muutmine.
- `AddTaskActivity`: Saate selle abil saate ülesanded lisada oma REST API, mis on kaitstud Azure AD.
- `ToDoActivity`: See on tööülesannete kuvava.

## <a name="create-the-sign-in-activity"></a>Looge tegevuse sisselogimine

Loo põhi tegevus ja pange selle nimeks `LoginActivity`.

Looge fail nimega `LoginActivity.java`.

Peate lähtestada tegevuse ja lisada sinna mõned nupud, mis kuvatakse teie UI. See on teile tuttavad, kui olete kirjutanud enne Androidi kood.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Nüüd olete loonud nupud, mis kõne oma `ToDoActivity` soovidele (mis nõuab ADAL, kui teil on vaja märgiks). Ta selleks kasutada oma tegevuse ja lisatud täiendav parameeter. Täiendav parameeter on vastu võetud selle `intent.putExtra()` meetod. Saate määratleda `"thePolicy"` abil, mis teie määratud `Constants.java`. See soovidele teada, milliseid autentimisel kasutada.

## <a name="create-the-settings-activity"></a>Looge tegevuse sätted

See on tegevus, mis kuvab teie sätted UI.

Looge fail nimega `SettingsActivity.java` lihtne loomine, lugemine, värskendamine ja kustutamine (CRUD) toimingud.

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Lisa ülesanne tegevuse loomine

Selle toiminguga saate tööülesande lisamiseks oma REST API lõpp-punkti.

Looge fail nimega `AddTaskActivity.java` ja kirjutage järgmine.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Tegevuse ülesannete loendi loomine

See on kõige olulisemad tegevuse. Saate kasutavad seda märgiks: Azure'i AD poliitika ja kõne tööülesande REST API serveri seega abil.

Looge fail nimega `ToDoActivity.java` ning Kirjutage järgmine. (Kõnesid hiljem selgitatakse.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Te olete märganud, et see sõltub viise, mida pole veel kirjutada. Need sisaldavad `updateLoggedInUser()`, `clearSessionCookie()`, ja `getTasks()`. Teile kirjutada need hiljem, leiate sellest juhendist. Nüüd võite ignoreerida turvaline Androidi Studios tõrked.

Selgitus parameetrid:

  - `SCOPES`: Nõutav otsinguulatuste, mida soovite juurdepääsu taotlemine. B2C Preview, see on sama, mis `client_id`, kuid see peaks tulevikus muutuda.
  - `POLICY`Kui soovite autentida: poliitika.
  - `CLIENT_ID`: Nõutav, Azure AD portaalis tulevad.
  - `redirectUri`: Saab häälestada paketi nime. See pole vajalik kohta on `acquireToken` kõne.
  - `getUserInfo()`: Kas kasutaja on juba vahemälus otsimiseks soovitud viisil. Parameetri kirjeldatakse ka Küsi kasutajalt, kui kasutaja ei leita või kasutaja juurdepääsu luba on kehtetu. See meetod on kirjutatud hiljem sellest juhendist.
  - `PromptBehavior.always`: Aitab küsima mandaat vahemälu ja küpsise vahele jätta.
  - `Callback`: Nimetatakse pärast märgiks vahetatakse autoriseerimine näeb välja umbes järgmine. See on objekti `AuthenticationResult`, mis sisaldab juurdepääsu luba, aegumiskuupäeva ja ID Turbeloa teavet.

> [AZURE.NOTE]  Microsoft Intune'i ettevõtteportaali rakendus pakub ta komponent ja võib olla kasutaja seadmesse installitud. Rakenduse üle kõik rakendused seadme pakub ühekordse sisselogimise (SSO) juurdepääs. Arendajate peaks olema valmis Intune'i lubama. ADAL Androidi jaoks kasutamine ta konto, kui seal on loodud Autentija ühe kasutajakonto. Ta kasutamiseks peab arendaja registreerida eriline `redirectUri` jaoks ta kasutada. `redirectUri`on msauth://packagename/Base64UrlencodedSignature vormingus. Saate avada `redirectUri` skripti abil oma rakenduse `brokerRedirectPrint.ps1` või kasutades API kõne `mContext.getBrokerRedirectUri()`. Allkirja on seotud allkirjastamiseks sertide Google Play poest.

 Saate vahele jätta ta kasutaja abil:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] See B2C Kiirjuhend keerukus vähendamiseks on otsustanud ta meie valimis vahele jätta.

Järgmisena looge helper meetodid, et saada luba eraldi autentimise kõnede tööülesannet API ajal.

Sama `ToDoActivity.java` faili, kirjutage järgmine.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Lisada ka meetodid, mis "seatud" ja "get" `AuthenticationResult` (mis on teie luba) soovitud globaalse `Constants`. Ehkki `ToDoActivity.java` kasutab `sResult` selle puhul peate lisada järgmised võimalused. Kui te ei tee, teie muu tegevuse ei pääse luba töötamiseks (nt ülesande lisamine `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Meetodi tagastamiseks kasutaja ID loomine

ADAL for Android tähistab kujul kasutaja lisamine `UserIdentifier` objekti. See haldab kasutaja. Objekti abil saate tuvastada, kas teie kõnesid kasutatakse sama kasutaja. Selle teabe abil saate saate toetuvad vahemälu, mitte serveriga uue helistamine. Selle hõlbustamiseks lõime selle `getUserInfo()` meetod, mis tagastab `UserIdentifier`. Kasutage seda koos `acquireToken()`. Loodud on `getUniqueId()` meetod, mis tagastab ID-d `UserIdentifier` vahemälu.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Kirjutage helper meetodid

Mõned helper meetodid aitavad küpsiste kustutamiseks ja sisestage seejärel kirjuta `AuthenticationCallback`. Nende meetodite kasutatakse üksnes valim veenduge, et olete puhta oleku helistamisel oma `ToDo` tegevuse.

Sama faili nimega `ToDoActivity.java`, kirjutage järgmine.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Tööülesande API kõne

Pärast seda, kui teil on oma tegevuse ostke sõned valmis, saate kirjutada oma API tööülesande serverit.

`getTasks`pakub massiiv, mis tähistab teie serveri tööülesanded.

Alustage `getTasks`.

Sama faili nimega `ToDoActivity.java`, kirjutage järgmine.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Kirjutage ka meetod, mis lähtestada oma tabeli esimene Käivita.

Sama faili nimega `ToDoActivity.java`, kirjutage järgmine.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Näete, et see kood jaoks on vaja oma tööd teha mõned täiendavad meetodid. Kirjutage neid edasi.

### <a name="create-an-endpoint-url-generator"></a>Mõne URL-i lõpp-punkti generaator loomine

Peate lõpp-punkti URL, mida te saate ühenduse luua. Seda teha sama klassi fail.

**Sama faili** nimega `ToDoActivity.java`, kirjutage järgmine.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Pange tähele, et juurdepääsu luba lisamine taotluse järgmises jaotises kirjeldatud kood.

## <a name="write-some-ux-methods"></a>Kirjutage mõned Kasutuskogemuse meetodid

Androidi peab teil teha mõned kontekstiatribuuti kasutada rakendust. Need on `createAndShowDialog` ja `onResume()`. See on teile tuttavad, kui olete kirjutanud enne Androidi kood.

Sama faili nimega `ToDoActivity.java`, kirjutage järgmine.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Järgmiseks hallata oma dialoogiboksi kontekstiatribuuti.

Sama faili nimega `ToDoActivity.java`, kirjutage järgmine.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Nüüd peaks teil on `ToDoActivity.java` faili, mis kogub. Kogu projekt peaks koguma ka sel hetkel.

## <a name="run-the-sample-app"></a>Käivitage rakendus näidis

Lõpuks koostamine ja käivitage rakendus Androidi Studio või Eclipse. Registreerumine või rakenduse sisse logida. Sisselogitud kasutaja tööülesannete loomine. Logige välja ja teise kasutajana logige uuesti sisse, siis tööülesannete loomine ja selle kasutaja jaoks.

Märkate, et tööülesanded on talletatud kasutaja kohta API, kuna API ekstraktib juurdepääsu luba, mis on saadud kasutaja identiteet.

Viite lõplikus näide [on esitatud ZIP-faili](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Samuti saate selle klooni GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Oluline teave


### <a name="encryption"></a>Krüptimine

ADAL krüptib märgid ja poe `SharedPreferences` vaikimisi. Saate vaadata selle `StorageHelper` klassi üksikasjade kuvamiseks. Androidi **jaoks 4.3(API18) AndroidKeyStore** turvaline säilitamine privaatvõtmete kasutusele võtta. ADAL kasutab mis API18 ja kohal. Kui soovite kasutada ADAL alumise SDK versioonid, tuleb sisestada salajane klahvi `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Veebivaade küpsised

Androidi veebivaade tühjendage küpsised, kui rakendus on suletud. Saate sellega hakkama järgmine proovi kood.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Lisateavet leiate teemast küpsiste kohta](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
