<properties
    pageTitle="Alustamine: Azure'i AD logige sisse- ja väljalogimine node.js abil"
    description="Kuidas koostada on node.js Express MVC Web App, mis ühendab Azure AD jaoks Logi sisse."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="brandwe"/>

# <a name="nodejs-web-app-sign-in--sign-out-with-azure-ad"></a>NodeJS Web App sisselogimine ja logige välja ja Azure AD


Siin kasutame pass:

- Kasutaja abil Azure AD rakendusse sisse logida.
- Kasutaja kohta leiate teavet kuvada.
- Logige kasutaja rakenduse välja.

**Pass** on autentimine vahevara Node.js jaoks. Väga paindlik ja muutuv, pass saate tuleb tähelepandamatult panna igal Express põhineva või veebirakenduse Resitify. Strateegiad tugi autentimist kasutades kasutajanime ja parooli, Facebooki, Twitteri ja täielik kogum. Meil on välja töötatud strateegia Microsoft Azure Active Directory. Me selle mooduli installimiseks ja seejärel lisage Microsoft Azure Active Directory `passport-azure-ad` lisandmooduli.

Selleks, peate:

1. Rakenduse registreerida.
2. Häälestada rakenduse kasutamiseks pass-azure-ad strateegia.
3. Sisselogimine ja väljunud taotlusi probleemi Azure AD pass abil.
4. Kasutaja andmeid välja printida.

Selle õpetuse kood säilitatakse [github](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) - või klooni skelett:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Lõplikus rakendust pakutakse ka õppeteema lõpus.

## <a name="1-register-an-app"></a>1. register rakendus
- Logige sisse teenusekomplekti Azure haldusportaali.
- Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**.
- Valige rentniku, kuhu soovite rakenduse registreerida.
- Klõpsake vahekaarti **rakendused** ja klõpsake nuppu Lisa alla sahtlis.
- Järgige viipasid ja luua mõne uue **veebirakenduse ja/või WebAPI**.
    - Rakenduse **nimi** kirjeldada oma rakenduse lõppkasutajad
    -   **Sisselogimise URL** on rakenduse base URL.  Funktsiooni skelett vaikimisi ' http://localhost:3000/auth openid tagasi ".
    - **Rakenduse ID URI** on rakenduse jaoks kordumatut tunnust.  Soovite on kasutada `https://<tenant-domain>/<app-name>`, nt`https://contoso.onmicrosoft.com/my-first-aad-app`
- Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü konfigureerimine.

## <a name="2-add-pre-requisities-to-your-directory"></a>2 eel-requisities lisamine kataloogi

Käsurea, muuta kataloogide root kausta kui ei ole juba olemas ja käivitage järgmine käsk:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`

- Lisaks peate meie `passport-azure-ad` ka:

- `npm install passport-azure-ad`

See installib teekide sõltub selle pass azure ad.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. häälestamine rakenduse kasutamiseks pass-sõlm-js strateegia
Siin kuvatakse configure Express vahevara kasutama protokolli OpenID Connect autentimist.  Pass kasutatakse probleemi sisselogimine ja väljunud taotlusi, kasutaja seansi haldamine ja kasutajale, muu hulgas kohta teabe saamiseks.

-   Alustamiseks avage soovitud `config.js` projekti juurkaustas fail ja sisestage oma rakenduse konfigureerimise väärtused on `exports.creds` jaotis.
    -   Funktsiooni `clientID:` on **Rakenduse Id** määratud rakenduse registreerimise portaalis.
    -   Funktsiooni `returnURL` on sisestatud portaalis **Uri ümber suunata** .
    - Funktsiooni `clientSecret` on salajane loodud portaalis

- Järgmiseks Avage `app.js` faili selle proejct juurkaustas ja lisada järgmised tooted kõne autonoomsest soovitud `OIDCStrategy` strateegia, mis on kaasas`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// add a logger

var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Pärast seda, kasutage strateegia me lihtsalt viidata meie login taotlusi reageerimine

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2) 
// 
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    if (!profile.email) {
      return done(new Error("No email found"), null);
    }
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Pass kasutab kõigi strateegiad (Twitteri, Facebooki, jne), et kõik strateegia poolt kinni on sarnane. Strateegia vaadates näete võtame funktsioon, mis on märgiks ja valmis parameetrid. Strateegia kohusetundlikult naaseb meile pärast seda, kas kõik selle kasutaja töö. Kui see ei soovi me kasutaja talletada ja Kätkö luba, et me ei pea seda enam küsi.


> [AZURE.IMPORTANT] 
Ülaltoodud kood võtab iga kasutaja kohta, mis juhtub autentida meie server. Seda nimetatakse automaatne registreerimine. Tootmisserverid ei soovite lubada kõigil ilma neid läbida otsustate registreerimise protsess. See on tavaliselt mustri, näete tarbija rakendustes, kes võimaldab teil registreerida Facebooki, aga siis küsida täitmine täiendavat teavet. Kui see ei olnud valimi rakenduse, me võib olla lihtsalt ekstraktimist e-posti Turbeloa objekti, mis on tagastatud ja seejärel küsitakse nende täitmine täiendavat teavet. Kuna see on test lihtsalt lisame mälu-andmebaasi server.

- Seejärel lisame meetodid, mis võimaldab silma peal sisseloginud kasutajatel pass nõutud. See hõlmab jaotamine ja deserializing kasutaja teave:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
   log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};
```

- Järgmiseks laadimiseks kiire mootori koodi lisamine. Siin näete kasutame vaikimisi /views ja /routes mustri, mida pakub.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Lõpuks lisamine marsruudib, mis annab välja tegelik login taotluste soovitud `passport-azure-ad` mootor:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return
app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. abil pass probleemi sisselogimine ja väljunud taotlusi Azure AD

Rakenduse abil OpenID Connect autentimise protokoll v2.0 lõpp-punkti suhelda nüüd õigesti konfigureeritud.  `passport-azure-ad`on tehtud eest kõik käsitöö autentimise sõnumeid, kontrollimine sõnet Azure AD ja säilitades kasutaja seansi inetu üksikasjad.  Selles on teie kasutajate jaoks juurdepääsutase võimalus sisse logida, logige välja ja täiendava teabe kogumine sisselogitud kasutaja kohta.

- Esmalt võimaldab lisada vaikimisi, login, konto ja välju meetodid meie `app.js` faili:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Vaatame need üksikasjalikult:
    -   Funktsiooni `/` marsruutimiseks suunab index.ejs vaate, läbides selle kasutaja taotluse (kui see on olemas)
    - Funktsiooni `/account` marsruutimiseks on esimene ***veenduge, et saaksime on autenditud*** (me rakendab all) ja seejärel andke kasutaja taotluse nii, et saame kasutaja kohta lisateabe.
    - Funktsiooni `/login` marsruutimiseks helistavad koosolekule meie azuread-openidconnect Autentija kaudu `passport-azuread` ja kui see ei õnnestu saavad ümber suunata kasutaja tagasi /login
    - Funktsiooni `/logout` lihtsalt helistab soovitud logout.ejs (ja marsruutimine) mis kustutab küpsised ja kasutajale siis uuesti index.ejs tagasi


- Viimane osa `app.js`, lisame EnsureAuthenticated meetod, mida kasutatakse `/account` kohal.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}
```

- Lõpuks tegelikult loome server ise sisse `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. loomine vaadete ja marsruudib express kuvamiseks meie kasutaja veebisaidil

Meil on meie `app.js` lõpule jõudnud. Nüüd tuleb lihtsalt marsruudib ja vaateid, mis näitab, saame nii kasutajate kui ka pidet teabe lisamine soovitud `/logout` ja `/login` marsruudib oleme loonud.

- Looge selle `/routes/index.js` marsruutimiseks jaotises juurkaustas.

```JavaScript
/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Looge selle `/routes/user.js` jaotises juurkaustas marsruutimiseks

```JavaScript
/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Neid lihtsaid marsruudib lihtsalt mööda taotluse meie vaadete, sh kasutaja, kui see on olemas.

- Looge selle `/views/index.ejs` juurkaustas jaotises vaade. See on lihtne leht, mis kõne meie Logi sisse ja välja logida meetodid ja võimaldavad ostke kontoteave. Teade me kasutame tingimuslike `if (!user)` kasutajana sisse kutse on jõudnud on tõendusmaterjalide kogumiseks meil on sisse logitud kasutajale.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Looge selle `/views/account.ejs` vaadata jaotises juurkaustas nii, et me saame vaadata täiendavat teavet mis `passport-azuread` on sellele kasutajale taotluse.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Lõpuks teeme selle ilme päris paigutuse lisamisega. Luua selle ' / views/layout.ejs' vaade all olevat juurkaust

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/account">Account</a> | 
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Lõpuks luua ja käivitada rakenduse! 

Käivitage `node app.js` ja liikuge`http://localhost:3000`


Logige sisse töökoha või kooli kontoga või Microsofti Account ja pöörake tähelepanu sellele, kuidas kasutaja identiteet kajastub /account loend.  Nüüd on teil on tagatud kasutades valdkonna standard protokoll saate kasutajate oma isiklike ja töö/kool kontode autentimiseks web app.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [soovitud ZIP siin on saadaval](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip)või saate klooni seda GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```


Nüüd saate teisaldada täpsemate teemade peale.  Võite proovida:

[Turvaline Web API Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
