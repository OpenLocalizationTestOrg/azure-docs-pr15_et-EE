<properties
    pageTitle="Azure'i AD Androidi alustamine | Microsoft Azure'i"
    description="Kuidas luua Androidi rakendust, mis ühendab Azure AD jaoks Logi sisse ja Azure AD helistab kaitstud API-de kasutamine OAuthi."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Azure AD integreerida Androidi rakendus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Kui arendate töölauarakendus, Azure AD muudab lihtne ja arusaadav autentida, kasutades oma Active Directory kontod kasutajate jaoks.  See lubab ka rakenduse turvaliselt kasutamine mis tahes veebi-API, mis on kaitstud Azure AD, nt Office 365 API-d või Azure API-ga.

Androidi kliendid peavad juurde pääsema kaitstud ressursse, pakub Azure AD Active Directory Authentication Library või ADAL.  ADAL on ainus eesmärk elu on hõlpsalt oma rakenduse access sõned saada.  Näidata, kui lihtne on siin me tuleb koostada rakenduse Androidi Ülesandeloend mis:

-   Saab kasutada sõned ülesannete loendi API [OAuth 2.0 autentimise protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)abil helistamine.
-   Saab kasutaja Ülesandeloend
-   Märke kasutajad välja.

Enne alustamist peate Azure AD rentniku, kus saate luua kasutajaid ja rakenduse registreerimine.  Kui teil pole veel rentniku, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

> [AZURE.TIP] Proovige meie uut [arendaja portaali](https://identity.microsoft.com/Docs/Android) , mis aitab teil saada tööle Azure Active Directory vaid paari minutiga eelvaadet!  Arendaja portaali juhendab teid rakenduse registreerimisel ja integreerimine Azure AD oma kood.  Kui olete lõpetanud, on teil lihtne rakendus, mis saab autentida oma rentniku ja on kirjutamata kasutajatele, et saate aktsepteerida sõned ja teha valideerimine. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Samm 1: Alla laadida ja käivitada Node.js REST API TODO valimi Server

See näide on kirjutatud spetsiaalselt töötamiseks suhtes meie olemasoleva valim koostamise ühe rentniku ülesande REST API Microsoft Azure Active Directory jaoks. See on eelduseks Quick Start.

Häälestamise kohta teabe saamiseks külastage meie olemasoleva näidised siit:

* [Microsoft Azure Active Directory valimi REST API teenuse Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Samm 2: Registreerige oma Microsoft Azure AD rentniku veebi-API-ga

**Mida teen?**

*Microsoft Active Directory toetab kahte tüüpi rakenduste lisamine. Web pakkuda teenuseid ning kasutajate ja rakenduste (kas veebis või seadmes, kus töötab rakendus) API-d juurdepääsu nende veebi-API-d. Selles etapis tuleb on registreerimisel käitate kohalikult testimiseks selle valimi Web API-ga. Tavaliselt on see veebi-API oleks ülejäänud teenus, mis pakub funktsionaalsust soovite juurde pääseda rakenduse. Microsoft Azure Active Directory saate kaitsta mis tahes lõpp-punkti!*

*Siin oleme eeldades, et teil on registreerimisel TODO REST API viidatud kohale, kuid see töötab mis tahes Web API soovite Azure Active Directory kaitsta.*

Juhised Web API registreerida Microsoft Azure AD

1. [Azure'i haldusportaal](https://manage.windowsazure.com)sisse logida.
2. Klõpsake vasakul puhasväärtuse Active Directory
3. Klõpsake nuppu directory rentniku, kuhu soovite rakenduse valimi registreerida.
4. Klõpsake vahekaarti rakendused.
5. Sahtlis, klõpsake nuppu Lisa.
6. Klõpsake nuppu "Lisa rakendus minu ettevõte on arendamise".
7. Sisestage valige rakendus, näiteks "TodoListService", "Web rakenduse ja/või Web API" sõbralik nimi ja klõpsake nuppu edasi.
8. Sisselogimise URL-i, sisestage base URL proovi, mis on vaikimisi `https://localhost:8080`.
9. Rakenduse ID URI, sisestage `https://<your_tenant_name>/TodoListService`, asendades `<your_tenant_name>` oma Azure AD rentniku nimi.  Registreerimise lõpuleviimiseks klõpsake nuppu OK.
10. Samas on Azure portaali rakenduse vahekaarti konfigureerimine.
11. **Kliendi ID väärtuse leidmine ja kopeerige see kõrvale**, tuleb teil seda hiljem rakenduse konfigureerimisel.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Samm 3: Registreerida valimi Androidi Omakliendi rakendus

Registreerumine oma veebirakenduse on esimene samm. Järgmiseks peate ka rakenduse Azure Active Directory räägib. See võimaldab rakenduse suhelda lihtsalt registreeritud veebi-API

**Mida teen?**  

*Nagu eespool, toetab Microsoft Azure Active Directory lisamise kahte tüüpi rakendusi. Web pakkuda teenuseid ning kasutajate ja rakenduste (kas veebis või seadmes, kus töötab rakendus) API-d juurdepääsu nende veebi-API-d. Selles etapis tuleb on selles näites taotluse esitamine. Mida peate tegema selleks, et see rakendus saama taotluse juurdepääs veebi-API lihtsalt registreeritud. Azure Active Directory keeldub isegi luba küsida sisselogimise juhul, kui see on registreeritud rakenduse! Mis on osa mudeli turvalisus.*

*Siin oleme eeldades, et teil on selle kohal viidatud valimi rakenduse registreerimisel, kuid see töötab kõigi rakenduste arendamise on.*

**Miks ma olen pannes veebi-API-ga ja kohaldamine ühe rentniku?**

*Kui teil on arvasid, võib koostate rakendus, mis kasutab välise API, mis on registreeritud Azure Active Directory rentnikust teise. Kui seda palutakse klientide API rakenduse kasutamisega nõustute. Kena osa on, Active Directory Authentication Library iOS-i hoolitseb selle nõusoleku! Nagu me täpsemaid funktsioone, kuvatakse see on oluline osa tööd juurde pääseda komplekti Microsoft APIs Azure'i ja Office'i kui ka teenuse pakkuja. Nüüd, kuna te registreeritud Web API ja alusel samal rentnikukontol ei kuvata nõusoleku viipasid. See on tavaliselt juhul, kui arendate rakendus lihtsalt oma ettevõtte jaoks kasutada.*

1. [Azure'i haldusportaal](https://manage.windowsazure.com)sisse logida.
2. Klõpsake vasakul puhasväärtuse Active Directory
3. Klõpsake nuppu directory rentniku, kuhu soovite rakenduse valimi registreerida.
4. Klõpsake vahekaarti rakendused.
5. Sahtlis, klõpsake nuppu Lisa.
6. Klõpsake nuppu "Lisa rakendus minu ettevõte on arendamise".
7. Sisestage rakenduse, näiteks "TodoListClient-Android", valige "kohalikke klientrakendus" sõbralik nimi ja klõpsake nuppu edasi.
8. Sisestage URI ümber suunata `http://TodoListClient`.  Klõpsake nuppu valmis.
9. Klõpsake vahekaarti konfigureerimine rakenduse.
10. Kliendi ID väärtuse leidmine ja kopeerige see kõrvale, peate selle hiljem rakenduse konfigureerimisel.
11. Klõpsake "Õigused soovite muud rakendused", "Lisa rakendus."  Valige "Muu" rippmenüüst "Kuva" ja valige ülemine märge.  Otsige üles & klõpsake soovitud TodoListService ning klõpsake rakenduse lisamiseks all märkeruutu.  Valige "Juurdepääs TodoListService", "Delegeeritud õigused" rippmenüüst ja salvestada konfiguratsiooni.



Maven loomiseks saate kasutada funktsiooni pom.xml kõrgeima taseme

  * Klooni selle repo teie valitud kausta:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Järgige juhiseid artiklis [Prerequests jaotis häälestamine oma maven Androidi jaoks](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * SDK 19 setup emulaator
  * Minge juurkausta, kus saate kloonitud repo
  * Käivitage käsk: mvn clean installimine
  * Muuta kataloogi valimi Kiirjuhend: samples\hello CD-le
  * Käivitage käsk: mvn android: android: Käivita juurutamine
  * Peaksite nägema rakenduse käivitamine
  * Sisestage testkasutaja mandaati proovida!

Jar pakettide esitatakse ka aar paketi kõrval.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Samm 4: Allalaadimine Androidi ADAL ning lisada oma Eclipse tööruumi

Oleme tehtud lihtne, et teil on mitu võimalust, kasutada selle teegi Androidi projektis:

* Saate importida Eclipse ja link selle teegi rakenduse lähtekoodi.
* Kui Android Studio, saate kasutada *aar* paketi vorming ja viidata kahendfaile.

####<a name="option-1-source-zip"></a>Variant 1: Allikas Zip

Koopia lähtekoodi allalaadimiseks klõpsake "Laadige alla ZIP" lehe paremas servas või klõpsake [siin](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Variant 2: Allika Git kaudu

Saada SDK kaudu git lähtekoodi lihtsalt tüüp:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Võimalus 3: Kahendfaile Gradle kaudu

Saate kahendfaile Maven keskse Repo. AAR paketi saate kaasata AndroidStudio projekti järgmiselt.

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
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Suvand 4: aar Maven kaudu

Kui kasutate m2e lisandmooduli Eclipse, saate määrata sõltuvus pom.xml faili:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Suvand 5: jar paketi kena tegevust jälle kaustas
Saate jar faili toomine maven repotehing ja kukutage *kena tegevust jälle* kausta projektis. Peate ka projekti vajalikud ressursid kopeerida, kuna jar paketid ei sisalda neid.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Juhis 5: Viited Androidi ADAL lisamine projekti


2. Projekti viite lisamiseks ja määrata, kui ka Androidi teek. Kui te pole kindel, kuidas seda teha, [lisateabe saamiseks klõpsake siin] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Projekti sõltuvus silumine rakenduses project sätete lisamine

4. Värskendage oma projekti AndroidManifest.xml faili lisada.

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Luua oma peamine tegevus AuthenticationContext eksemplari. Selle kõne üksikasjad on selle README väljapoole, kuid hea algus vaadates [Androidi kohalikke kliendi näidis](https://github.com/AzureADSamples/NativeClient-Android)kuvatakse. Allpool on näide:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Märkus: mContext on oma tegevust välja

8. Kopeerige see kood plokk käsitlema AuthenticationActivity lõppu, kui kasutaja sisestab identimisteabe ja võtab vastu kood.

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Küsige luba, et määratleda pakkumist

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Lõpuks küsida märgiks selle tagasihelistamise abil:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Selgitus parameetrid:

  * Ressursi on nõutav ja soovite juurdepääsu ressursile.
  * Clientid on nõutavad ja AzureAD portaalis tulevad.
  * Saate häälestada oma packagename nimega redirectUri. Ei ole vajalik acquireToken kõne kohta.
  * PromptBehavior aitab küsima mandaat vahemälu ja küpsise vahele jätta.
  * Pärast kood vahetatakse märgiks tagasihelistamise nimi.

  Funktsiooni tagasihelistamise on AuthenticationResult, mis on accesstoken objekti, aegunud, ja idtoken teave.

Valikuline: **acquireTokenSilent**

Saate helistada **acquireTokenSilent** reageerimine vahemällu talletamine ja Turbeloa värskendamine. Sünkroonimise versioon ka pakub. See aktsepteerib parameetrina kasutajanimi.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Ta**: Microsoft Intune'i ettevõtteportaali rakendus annab ta komponent. ADAL ei kasuta ta konto, kui seal on üks kasutajakonto on loodud Autentija arendaja ja ei soovi selle vahele jätate. Arendaja vahele jätta ja kasutajale ta:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Arendaja peab ta kasutuse teisiti redirectUri registreerimiseks. RedirectUri on msauth://packagename/Base64UrlencodedSignature vormingus. Saate oma redirecturi saamiseks rakenduse abil skripti "brokerRedirectPrint.ps1" või kasutada API kõne mContext.getBrokerRedirectUri. Allkirja on seotud teie allkirja serdid.

 Praeguse ta mudel on ühe kasutaja jaoks. AuthenticationContext pakub API meetod, et saada ta kasutaja.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Ta kasutaja saab tagastatud, kui konto on lubatud.

 Oma rakenduse manifesti peaks olema õigused kasutada AccountManager kontod: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Juhendis kasutamisel peaks olema, mida peate tegema edukalt integreerimine Azure Active Directory. Veel näiteid selle töötamise, külastage selle AzureADSamples / hoidla github.

## <a name="important-information"></a>Oluline teave

### <a name="customization"></a>Kohandamine

Teegi projekti ressursid saate üle kirjutada oma rakenduse ressursse. See juhtub siis, kui teie rakendus on hoone. Seetõttu saate kohandada autentimise tegevuse paigutus soovitud viisil. Peate veenduge, et hoida juhtelementide ID. selle ADAL uses(Webview).

### <a name="broker"></a>Ta

Ta komponent toimetatakse koos Microsoft Intune'i ettevõtteportaali rakendus. Konto luuakse konto haldur. Konto tüüp on "com.microsoft.workaccount". See võimaldab ainult ühe SSO konto. See loob pärast seadet ülesanne ühte SSO küpsise selle kasutaja jaoks.

### <a name="authority-url-and-adfs"></a>Asutuse Url ja ADFS-i

ADFS-i ei tuvastata toodanguna STS, seega peate lülitage eksemplari Discovery ja edastama veebisaidil AuthenticationContext ehitaja false.

Asutuse URL-i peab STS eksemplar ja rentniku nimi: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Päringute vahemälu üksused

ADAL saadaval koos mõne lihtsa vahemälu vaikimisi vahemälu SharedPreferences päringu funktsioone. Saate praeguse vahemälu AuthenticationContext abil:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Saate sisestada ka oma vahemälu rakendamist, kui soovite seda kohandada.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL pakub võimalus täpsustage kiiret käitumine. PromptBehavior.Auto avaneb UI kui värskendamise luba ei sobi ja kasutajale mandaat on nõutav. PromptBehavior.Always vahele jätta vahemälu kasutus ja Kuva alati UI.

### <a name="silent-token-request-from-cache-and-refresh"></a>Vaikne Turbeloa taotluse vahemälu ja värskendamine

Kasutage seda meetodit UI pop häälestamine ja nõua tegevus. See ei annaks Turbeloa vahemälu, kui see on saadaval. Kui luba on aegunud, proovib see värskendamine. Kui värskendamine luba on aegunud või nurjus, see tagasi AuthenticationException.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Saate kõne selle meetodi sünkroonimine. Saate seada tühi tagasihelistamise või kasutada acquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnostika

Järgnevalt on esmane andmeallikate diagnoosimise probleemide kohta.

+ Erandid
+ Logide
+ Võrgu jälgi

Lisaks tähele, et korrelatsiooni ID-d on keskse diagnostika teegis. Saate määrata oma korrelatsiooni ID-de kohta soovi kui soovite, et mõne ADAL taotleda muude toimingute koodi. Kui te ei määra korrelatsiooni ID-d, siis ADAL loob juhuslik üks ja kõik lisata Logi sõnumid ja võrgu korrelatsiooni id-ga. Enda loodud id muudatused iga taotluse.

#### <a name="exceptions"></a>Erandid

See on ilmselt esimene diagnostika. Me Proovige sisestada kasulik tõrketeated. Kui te ei leia üks, mis pole kasulik esitage küsimus ja andke meile teada. Sisestage seadme teave, nt mudeli ja SDK #.

#### <a name="logs"></a>Logide

Saate konfigureerida teegi luua Logi sõnumid, mille abil saate aidata diagnoosida probleeme. Saate konfigureerida logimine tagasihelistamise, mida ADAL hakkab kasutama iga sõnumi Logi välja käepärast, kui see on loodud konfigureerimiseks järgmised helistamine.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Sõnumite saate kohandatud logifaili kirjutada, nagu allpool näha. Kahjuks on standardne kuidagi saada logid seadmest. On mõned teenused, mis aitavad teil seda. Te saate ka Looge oma, näiteks serveri faili saata.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Logimise tasemed

+ Error(Exceptions)
+ WARN(Warning)
+ Teave (teadmiseks)
+ Verbose (rohkem üksikasju)

Saate määrata log umbes järgmine:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Kõik Logi sõnumid saadetakse logcat lisaks mis tahes kohandatud log kontekstiatribuuti.
Nagu näha belog saate hankida faili vormi logcat Logi sisse.

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Veel näiteid adb cmds kohta: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Võrgu jälgi

Erinevate tööriistade abil saate jäädvustada ADAL genereeritud HTTP-liiklust.  See on kõige kasulikumad, kui olete tuttav OAuthi Protocol (protokoll) või kui peate sisestama diagnostikateabe Microsoftile ega muudest kanalitest tugi.

Viiuldaja on kõige lihtsam HTTP jälgimise tööriista.  Järgmiste linkide abil saate setup see kuni õigesti kirje ADAL võrguliiklust.  Selleks, et olla kasulik on vaja konfigureerida viiuldaja või mõnda muud tööriista, nt Charles krüptimata SSL-liikluse salvestada.  Märkus: Jälgi sellisel viisil loodud võivad sisaldada suuremate õigustega teavet nagu Accessi sõned, kasutajanimed ja paroolid.  Kui kasutate moodustab, ühiskasutus nende jälgi 3 isikutega.  Kui peate sisestama jälitusteave kellelegi toe saamiseks, probleemi esilekutsumine kasutajanimed ja paroolid, mida te ei heiduta ühiskasutus koos ajutine kontoga.

+ [Kuidas häälestada viiuldaja Androidi jaoks](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Konfigureerimine viiuldaja reeglid ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dialoogiboksi režiim
acquireToken meetod ilma tegevuse toetab dialoogiboksi küsimus.

### <a name="encryption"></a>Krüptimine

ADAL krüptib märgid ja poe SharedPreferences vaikimisi. Saate vaadata StorageHelper klassi üksikasjade kuvamiseks. Androidi kasutusele AndroidKeyStore 4.3(API18) privaatvõtmete turvaline säilitamine. ADAL kasutab mis API18 ja kohal. Kui soovite kasutada ADAL alumise SDK versioonid, tuleb sisestada AuthenticationSettings.INSTANCE.setSecretKey salajane klahv

### <a name="oauth2-bearer-challenge"></a>Oauth2 esitaja ülesanne

AuthenticationParameters klassi pakub funktsiooni saada soovitud authorization_uri Oauth2 esitaja ülesanne.

### <a name="session-cookies-in-webview"></a>Seansi küpsiste Webview

Androidi webview tühjendage küpsised, kui rakendus on suletud. Saavad hakkama see proovi kood allpool:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Lisateavet küpsiste kohta: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Ressursi alistab

ADAL Raamatukogu sisaldab inglise stringide järgmised kaks ProgressDialog sõnumite jaoks.

Rakenduse peaks kirjutatakse üle, kui lokaliseeritud stringide on soovitud.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM dialoogiboks
ADAL versioon 1.1.0 toetab NTLM dialoogiboksi, mis on töödeldud onReceivedHttpAuthRequest sündmuse WebViewClient kaudu. Dialoogiboksi paigutus ja stringide saab kohandada.

### <a name="cross-app-sso"></a>Rist-rakenduse SSO-d
Saate teada, [Kuidas lubada rist-rakenduse SSO Android abil ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
