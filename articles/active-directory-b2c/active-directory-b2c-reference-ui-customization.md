<properties
    pageTitle="Azure Active Directory B2C: Kasutaja kasutajaliidese (UI) kohandamine | Microsoft Azure'i"
    description="Azure Active Directory B2C kasutaja kasutajaliidese (UI) kohandamisfunktsioonid teema"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Kohandamine Azure AD B2C kasutajaliidese (UI)

Kasutusvõimalused on kõige olulisem tarbija suunatud rakenduses. See on hea rakendus ja suur ja ainult aktiivse tarbija ja tõeliselt tööle neist vahel. Azure Active Directory (Azure AD) B2C võimaldab tarbija registreerumise, sisselogimist (*vt allpool märkust*) profiili redigeerimine, kohandamine ja parooli lähtestamise lehtede ühe piksli-täiuslik juhtelemendiga.

> [AZURE.NOTE]
Praegu kohaliku kontoga sisselogimine lehtede, accompaning parooli lähtestamiseks lehed ja kinnitamine meilisõnumid saab kohandada ainult [ettevõtte tootemargi funktsiooni](../active-directory/active-directory-add-company-branding.md) abil, mitte menetlustele, mis on selles artiklis kirjeldatud.

Selles artiklis ei loe kohta:

- Lehe kasutaja kasutajaliidese (UI) kohandamise funktsiooni.
- Tööriista, mis aitavad teil testida lehe UI kohandamise funktsiooni abil meie valimi sisu.
- Core Kasutajaliidese elementidele igat tüüpi leht.
- Head tavad selle funktsiooni kasutamisel.

## <a name="the-page-ui-customization-feature"></a>Funktsiooni lehe UI kohandamine

Lehe UI kohandamise funktsiooni, saate kohandada ilme ja tarbija registreerumise, sisselogimine, paroolide lähtestamine ja profiili redigeerimine lehtede (konfigureerimisega [poliitikate](active-directory-b2c-reference-policies.md)). Kui teie taotlus ja lehtede vahel liikumise kätte Azure AD B2C on tarbijatele sujuvalt kogemusi.

Erinevalt muude teenustega, kus Kasutajaliidese suvandid on piiratud või saadaval ainult API-de kaudu, kasutab Azure AD B2C nüüdisaegne (ja lihtsam) lähenemine lehe UI kohandamine.

Siin on, kuidas see toimib: Azure'i AD B2C töötab teie tarbija brauseris koodi ja kasutab tänapäevane lähenemine, nimetatakse [Cross-päritolu ressursside ühiskasutus (CORS)](http://www.w3.org/TR/cors/) URL-i, kus saate määrata poliitika sisu alla laadida. Saate määrata eri erinevate lehtede URL-e. Koodi liidab Kasutajaliidese elemendid: Azure'i AD B2C laaditud oma URL-i sisu ja kuvab lehe oma tarbija. Kõik, mida peate tegema on:

1. Luua regulaarne HTML5 sisu on `<div id="api"></div>` elemendi (tuleb tühja element) asub kuskil on `<body>`. Selle elemendi märgid, kus Azure AD B2C sisu lisatakse.
2. Majutada oma sisu HTTPS-i lõpp-punkti (koos CORS lubatud).
3. Kasutajaliidese elemendid, mis lisab Azure AD B2C laad.

## <a name="test-out-the-ui-customization-feature"></a>Katsetada funktsiooni UI kohandamine

Kui soovite proovida UI kohandamise funktsiooni abil meie valimi HTML-i ja CSS-i sisu, oleme lisanud saate [lihtsate helper tööriista](active-directory-b2c-reference-ui-customization-helper-tool.md) , mis lisatud ja konfigureerib teie Azure'i bloobimälu valimi sisu.

> [AZURE.NOTE]
Saate majutada oma UI sisu suvalist: web serverites CDN-ID, AWS S3, ühiskasutuse failisüsteemi jne. Kui sisu on majutatud avalikult kättesaadavaks HTTPS-i lõpp-punkti (koos CORS lubatud), Oletegi valmis!. Kasutame Azure'i bloobimälu illustreeriv eesmärgil.

### <a name="the-most-basic-html-content-for-testing"></a>Kõige olulisemaid HTML-sisu testimiseks

Allpool on kõige olulisemaid HTML-i sisu, et saate seda võimalust testida. Kasutage sama helper tööriista varem esitatud üles laadida ja selle sisu oma Azure'i bloobimälu konfigureerimine. Seejärel saate kontrollida, tavaline, mitte stiliseeritud nupud ja vormi väljad igal leheküljel on kuvatud ja funktsionaalne.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Iga tüüpi lehe core Kasutajaliidese elemendid

Järgmistest jaotistest leiate näiteid HTML5 fragmendid, mis ühendab Azure AD B2C soovitud `<div id="api"></div>` element, mis asub teie sisu. **Lisage need fragmendid HTML 5 sisu.** Azure'i AD B2C teenuse lisab need käitusajal. Nendes näidetes abil saate kujundada ise laadilehti.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure'i AD B2C sisu lisada "identiteedi pakkuja valiku leht"

See leht sisaldab loendit identiteedipakkujad, mida kasutaja saab valida registreerumise või Logi sisse. Need on nagu Facebookis ja Google + social identiteedipakkujad või kohalikud kontod (e-posti aadress või kasutaja nime põhjal).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure'i AD B2C sisu lisada "kohalik konto registreerumise leht"

See leht sisaldab registreerumisvorm, et kasutajal on kohalik konto, mis põhineb e-posti aadress või kasutajanimi sisselogimisel täitmiseks. Vormi võivad sisaldada erinevaid Sisestuskeel juhtelemente, nt Sisestuskeel tekstivälja, väljale parooli sisestamise, nupp, ühe – valige rippmenüüst ja hulgivaliku ruudud.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure'i AD B2C sisu lisada "" suhtlusvõrgu konto registreerumise leht"

See leht sisaldab registreerumisvorm, mida tarbija abil olemasolevale kontole: social identiteedipakkuja Facebooki või Google + sisselogimisel täitmiseks. Sellel lehel on sarnane kohaliku konto registreerumise lehe (näidatud eelmises jaotises) välja arvatud parooli sisestamise väljad.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure'i AD B2C sisu lisada "ühendatud registreerumise või sisselogimise leht"

Selle lehe tegeleb nii registreerumise & sisselogimine tarbijate, kes saavad kasutada social identiteedipakkujad nagu Facebookis või Google + või kohalikud kontod.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure'i AD B2C sisu lisada "mitmikautentimise leht"

Sellel lehel saate kasutajad kinnitama nende telefoninumbrid (abil teksti või heli) registreerumise või sisselogimise ajal.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure'i AD B2C sisu lisada "" Lehe tõrge"

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Asja meeles pidada, kui hoone oma sisu

Kui kavatsete kasutada lehe UI kohandamise funktsiooni, tutvuge järgmiste juhiste täitmisel:

- Ei Azure AD B2C vaikimisi sisu kopeerida ja proovige seda muuta. See on parim koostamiseks HTML5 sisu algusest peale ise luua ja kasutada vaikesisu abimaterjalina.
- Kõik lehed (välja arvatud tõrkelehtede) kätte sisselogimist, registreerumise ja profiili redigeerimine poliitikad, on laadilehti, mis pakub alistada vaikesäte laadilehti, millele lisame järgmiste lehtede sisse selle <head> fragmendid. Kõik lehed on registreerumise või sisselogimise parooli lähtestamine poliitikate ja kõik poliitikate tõrge lehtede served, on teil esitada kogu otsingukeskusi ise.
- Turvalisuse põhjustel me ei luba teil lisada mis tahes JavaScripti oma sisu. Enamik vajalik peaks olema saadaval välja kasti. Kui ei, siis kasutage [Kasutaja kõneposti](http://feedback.azure.com/forums/169401-azure-active-directory) taotleda uusi funktsioone.
- Toetatud brauseri versiooni:
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (piiratud)
    - Internet Explorer 8 (piiratud)
    - Google Chrome 43,0
    - Google Chrome 42,0
    - Mozilla Firefox 38,0
    - Mozilla Firefox 37,0
