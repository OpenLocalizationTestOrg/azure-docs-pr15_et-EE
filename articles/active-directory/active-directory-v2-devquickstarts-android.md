<properties
    pageTitle="Azure Active Directory v2.0 Androidi | Microsoft Azure'i"
    description="Kuidas koostada Androidi rakendust, milles on nii isikliku Microsofti kontoga ning töö või kooli kontode ja kõnede Graph API kasutajad kolmanda osapoole teekide abil."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Sisselogimine Androidi rakenduse abil muu teegi Graph API-ga kasutades v2.0 lõpp-punkti lisamine

Microsofti identity platvormi kasutab avatud standardeid nagu OAuth2 ja OpenID ühendamine. Arendajad saate kasutada mis tahes teegi kasutajad soovivad meie teenuste integreerimine. Et arendajad meie platvormi kasutada muude teekidega, oleme kirjutanud mõne juhendavad tutvustused umbes selline näidata, kuidas konfigureerida ühenduse Microsofti identity platvormi kolmanda osapoole teeke. Enamik teekide [RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) , et saate luua ühenduse Microsofti identity platvormi.

Rakendus, mis loob juhendis kasutajad saavad logige sisse oma organisatsiooni ja seejärel otsige ise oma ettevõtte Graph API abil.

Kui olete uus OAuth2 või OpenID ühendus, suure osa sellest valimi ei oleks teile. Soovitame teil lugeda [2.0 Protokollid - OAuthi 2.0 autoriseerimine kood Flow](active-directory-v2-protocols-oauth-code.md) tausta jaoks.

> [AZURE.NOTE] Meie platvormi funktsioonid, mis on avaldise OAuth2 või OpenID Connect standardeid, näiteks juurdepääsu ja Intune poliitika haldus nõuavad meie avatud allikas Microsoft Azure'i identiteedi teekide kasutamist.

V2.0 lõpp-punkti ei toeta kõik stsenaariumid Azure Active Directory ja funktsioonid.

> [AZURE.NOTE] Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Koodi saate alla laadida GitHub
Selle õpetuse kood säilitatakse [github](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) - või klooni skelett:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Saate alla laadida ka lihtsalt valimi ja kohe tööle asuda.

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Rakenduse registreerimine
Luua uue rakenduse [rakenduse registreerimise portaali](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige üksikasjalikud juhised, kuidas [registreerida app v2.0 lõpp-punkti](active-directory-v2-app-registration.md).  Veenduge, et:

- **Rakenduse Id** , mis on määratud rakenduse, kuna peate kiiresti kopeerida.
- Lisage oma rakenduse **Mobile** platvormi.

> Märkus: Rakenduse registreerimise portaal pakub **Ümber suunata URI** väärtus. Siiski selles näites kasutage vaikeväärtust, milleks on `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>NXOAuth2 kolmanda osapoole Raamatukogu alla laadida ja tööruumi loomine

See selgituse kasutate OIDCAndroidLib GitHub, mis on OAuth2 teek, mis põhineb OpenID Connect koodi Google kaudu. See rakendatakse kohalikus rakenduses profiili ja toetab autoriseerimine lõpp-punkti Kasutaja. Need on asjad, mida peate Microsofti identity platvormi integreerida.

Klooni OIDCAndroidLib repo arvutiga.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Androidi Studio keskkonna häälestamine

1. Androidi Studio uue projekti loomine ja aktsepteerige vaikesätted viisardi.

    ![Androidi Studios uue projekti loomine](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Sihtrakenduse Androidi seadmete](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Mobile toimingu lisamine](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Häälestada oma projekti moodulid, kloonitud repo projekti asukohta teisaldada. Saate luua ka projekti ja seejärel klooni selle projekti kohta.

    ![Projekti moodulid](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Avage projekt moodulid sätted kontekstimenüü abil või Ctrl + Alt + Maj + S otsetee abil.

    ![Projekti moodulid sätted](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Eemaldada vaikimisi rakenduse mooduli, kuna soovite projekti container sätted.

    ![Vaikimisi rakenduse mooduli](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Importimine moodulid kloonitud repo praeguse projektiga seotud.

    ![Projekti importimine gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![uue mooduli lehe loomine](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Korrake neid juhiseid jaoks soovitud `oidlib-sample` mooduli.

7. Kontrollige oidclib sõltuvused on `oidlib-sample` mooduli.

    ![oidclib sõltuvuse oidlib valimiga mooduli](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Klõpsake nuppu **OK** ja oodake, kuni gradle sünkroonimine.

    Teie settings.gradle peaks välja nägema.

    ![Kuvatõmmis settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Veenduge, et valimi rakenduse koostamine valimi töötab õigesti.

    Te ei saa kasutada seda Azure Active Directory veel. Vajame konfigureerimiseks esmalt mõni lõpp-punktid. See on teil Androidi Studio on probleeme enne alustamist valimi rakenduse kohandamise tagamiseks.

10. Luua ja käivitada `oidlib-sample` mis Sihtvaluuta Androidi Studios.

    ![Edenemise oidlib valimiga koostamine](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Kustutamine kuvatakse `app ` kausta, mis jäeti, kui te eemaldada mooduli Projekt, sest Androidi Studio ei kustutata turvalisuse huvides.

    ![Faili struktuur, mis sisaldab kataloogi rakendus](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Saate avada menüü **Redigeerimine konfiguratsioone** eemaldamiseks Käivita konfiguratsiooni, mis jäeti mooduli eemaldamisel projekti.

    ![Konfiguratsioone menüü Redigeeri](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![käivitage rakendus konfigureerimine](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Lõpp-punktid proovi konfigureerimine

Nüüd, kui teil on selle `oidlib-sample` töötab edukalt, redigeerimine saada see töötamise Azure Active Directory mõned lõpp-punktid.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Konfigureerige oma kliendi oidc_clientconf.xml faili redigeerimine

1. Kuna kasutate ainult, et saada märgiks ja kõne Graph API OAuth2 puhul, määrake kliendi teha ainult OAuth2. OIDC tulevad hiljem näiteks.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Konfigureerige oma kliendi ID registreerimine portaalis vastuvõetud.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Konfigureerida oma redirect URI üks allpool.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Konfigureerige oma otsinguulatuste, et peate selleks, et pääseda Graph API.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Funktsiooni `User.Read` väärtus `oidc_scopes` võimaldab teil lugeda lihtne profiil allkirjastatud kasutaja.
Saate lisateavet kõik saadaolevad otsinguulatuste veebisaidil [Microsoft Graphi õiguste otsinguulatuste](https://graph.microsoft.io/docs/authorization/permission_scopes).

Kui soovite selgitused kohta `openid` või `offline_access` kui otsinguulatuste OpenID ühenduse loomine, lugege teemat [2.0 Protokollid - OAuthi 2.0 autoriseerimine kood Flow](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Konfigureerige oma kliendi lõpp-punktid oidc_endpoints.xml faili redigeerimine

- Avage soovitud `oidc_endpoints.xml` fail ja teha järgmisi muudatusi:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Nende lõpp-punktid kunagi peaks muuta, kui kasutate OAuth2 oma Protocol (protokoll).

> [AZURE.NOTE]
Lõpp-punktide `userInfoEndpoint` ja `revocationEndpoint` praegu ei toeta Azure Active Directory. Kui jätate need vaikeväärtus example.com, teile meelde, et need ei ole saadaval valimis :-)


## <a name="configure-a-graph-api-call"></a>Graph API kõne konfigureerimine

- Avage soovitud `HomeActivity.java` fail ja teha järgmisi muudatusi:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Siin lihtne Graph API kõne tagastab meie teabe.

Need on kõik muudatused, mida peate tegema. Käivitage selle `oidlib-sample` rakendus ja klõpsake nuppu **Logi sisse**.

Kui olete edukalt autenditud, nuppu **Taotleda kaitstud ressursi** testimiseks Graph API kõne.

## <a name="get-security-updates-for-our-product"></a>Meie toote jaoks turbevärskenduste saamine

Soovitame teil külastades [Turbe Tehnokeskus](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise turvalisus juhtumite kohta teatiste saamiseks.
