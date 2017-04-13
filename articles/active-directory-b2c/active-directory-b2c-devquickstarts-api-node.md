<properties
    pageTitle="Azure'i AD B2C: Secure veebi-API Node.js abil | Microsoft Azure'i"
    description="Kuidas koostada Node.js veebi-API, mida aktsepteerib sõnet B2C rentniku jaoks"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure'i AD B2C: Secure veebi-API Node.js abil

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C, kus saate secure veebi-API OAuth 2.0 Accessi sõned abil. Nende tähiste luba oma kliendi rakenduste Azure AD B2C API autentida. Selles artiklis kirjeldatakse, kuidas "Ülesandeloend" API, mis võimaldab kasutajatel lisada ja tööülesannete loendi loomiseks. Web API on turvatud abil Azure AD B2C ja võimaldab ainult autenditud kasutajatel hallata nende Ülesandeloend.

> [AZURE.NOTE]  See näide on kirjutatud olema ühendatud meie [iOS-i B2C valimi rakenduse](active-directory-b2c-devquickstarts-ios.md)abil. Tehke praeguses walk-through esmalt ja seejärel järgige koos selle proovi.

**Pass** on autentimine vahevara Node.js jaoks. Paindlik ja modulaarne, pass tähelepandamatult installimist mis tahes Express-põhine või veebirakenduse Restify. Strateegiad toetab autentimist kasutades kasutajanime ja parooli, Facebooki, Twitteri ja täielik kogum. Meil on välja töötatud strateegia Azure Active Directory (Azure AD). Selle mooduli installimist ja seejärel lisage Azure AD `passport-azure-ad` lisandmooduli.

Selle valimi tegemiseks peate:

1. Rakenduse registreeruma Azure AD.
2. Häälestada rakenduse kasutamiseks pass `azure-ad-passport` lisandmooduli.
3. Konfigureerige klientrakendusega helistamiseks "Ülesandeloend" veebi-API.


## <a name="get-an-azure-ad-b2c-directory"></a>Saada on Azure AD B2C kataloog

Enne kasutamist Azure AD B2C, saate luua või rentniku.  Kataloogi on ümbris kõik kasutajad, rakendused, rühmad ja palju muud.  Kui teil pole seda juba, jätkake [luua B2C kataloogi](active-directory-b2c-get-started.md) enne.

## <a name="create-an-application"></a>Rakenduse loomine

Järgmiseks peate rakenduse loomine B2C kataloogis, mis annab Azure AD osa teabest rakenduse turvaliselt suhelda. Sel juhul kliendi rakendus nii veebi-API esindavad ühe **Rakenduse ID**, kuna need sisaldavad üks loogiline rakendus. Rakenduse loomiseks järgige [neid juhiseid](active-directory-b2c-app-registration.md). Kindlasti järgmist.

- Kaasa on **web app/web api** rakenduse
- Sisestage `http://localhost/TodoListService` **vastus URL-i**. See on vaikimisi URL-i seda proovi kood.
- **Rakenduse salajane** rakenduse loomine ja selle kopeerimine. Neid andmeid hiljem vaja. Pange tähele, et see väärtus peab olema [XML-i põgenes](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) enne selle kasutamist.
- Kopeerige **Rakenduse ID** , mis määratakse teie rakendus. Neid andmeid hiljem vaja.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Oma poliitikate loomine

Azure'i AD B2C, iga kasutusvõimalused on määratletud [poliitika](active-directory-b2c-reference-policies.md). See rakendus sisaldab identiteedi kahes versioonis: registreeruda kursustele ja logige sisse. Peate looma ühe korra iga tüüpi [poliitika artiklis](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kirjeldatud.  Kui loote oma kolme poliitikad, ärge unustage:

- Valige oma registreerumise poliitika **kuvatav nimi** ja muud registreerumise atribuudid.
- Valige **kuvatav nimi** ja **Objekti ID** rakenduse nõuded iga poliitika.  Soovi korral saate kasutada ka muud nõuded.
- Kopeerige **nimi** , iga poliitika pärast selle loomist. Sellel peab olema eesliite `b2c_1_`.  Poliitika nimed hiljem vaja.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kui olete loonud oma kolme poliitikad, olete valmis oma rakenduse.

Teavet selle kohta, kuidas toimivad poliitikad Azure AD B2C, alustage [õpetuse .NET web app töötamise alustamine](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Laadige alla kood

See õpetus [säilitatakse github](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS)kood. Valimi koostamiseks, kui lähete, saate [alla laadida skelett projekti ZIP-faili](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Samuti saate klooni skelett:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Lõpetatud app on ka [ZIP-faili saadaval](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) või on `complete` haru sama hoidla.

## <a name="download-nodejs-for-your-platform"></a>Laadige alla oma platvormile Node.js

Selle valimi edukalt kasutamiseks peate mõne Node.js töötamine installi. 

Installige Node.js [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Installige MongoDB teie platvorm

Selle valimi edukalt kasutamiseks peate mõne MongoDB töötamine installi. MongoDB abil muuta oma REST API püsivate kogu serveri eksemplari.

Installige MongoDB [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] See walk-through eeldab vaikimisi installi ja serveri lõpp-punktid kasutamiseks MongoDB, mis on kirjutamise ajal `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Installige Restify moodulid oma veebi-API

Restify abil luua oma REST API-ga. Restify on minimaalne ja paindlikus Node.js rakenduse raamistiku saadud Express. See on funktsioonide jaoks REST API-de peal ühendamine komplekti.

### <a name="install-restify"></a>Installi Restify

Muuta käsureal, et kataloogi `azuread`. Kui soovitud `azuread` kataloogi pole olemas, looge see.

`cd azuread`või`mkdir azuread;`

Sisestage järgmine käsk:

`npm install restify`

See käsk installib Restify.

#### <a name="did-you-get-an-error"></a>Kas kuvatakse tõrketeade?

Mõned opsüsteemid, kui kasutate `npm`, kuvatakse tõrketeade `Error: EPERM, chmod '/usr/local/bin/..'` ja käivitate konto administraatorina. Kui see probleem esineb, kasutage funktsiooni `sudo` käsk `npm` kõrgema taseme õigusi.

#### <a name="did-you-get-a-dtrace-error"></a>Kas teile kuvati DTrace tõrketeade?

Restify installimisel võidakse kuvada umbes see tekst.

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify pakub võimas vahend jälgimise ülejäänud helistab DTrace abil. Paljude operatsioonisüsteemide ei ole saadaval DTrace. Saate nende vigade turvaline ignoreerida.

Käsu väljund peaks kuvatama sarnane see tekst:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Installige pass oma veebi-API

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas.

Installige pass abil järgmine käsk:

`npm install passport`

Käsu väljund peaks sarnanema see tekst:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Teie veebi-API pass-azuread lisamine

Järgmisena lisage OAuthi strateegia abil `passport-azuread`, komplekti Azure AD kasutajaga pass strateegiad. Kasutage strateegia esitaja märkide valimis REST API-ga.

> [AZURE.NOTE] Kuigi OAuth2 raamistiku, mis tahes teadaolevad loa tüüp välja antud, ainult teatud Turbeloa saanud laialdane kasutamine. Märkide kaitsmine lõpp-punktid on esitaja sõned. Seda tüüpi sõned on enim väljastatud OAuth2. Mitme rakendusi eeldatakse, et esitaja sõned on ainult luba välja tüüp.

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas.

Installige pass `passport-azure-ad` mooduli abil järgmine käsk:

`npm install passport-azure-ad`

Käsu väljund peaks sarnanema see tekst:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Teie veebi-API MongoDB moodulid lisamine

See näide kasutab MongoDB andmete salvestamiseks. Mis kaarte Mangust mõne levinuma lisandmooduli haldamise mudelid ja skeemid.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Täiendavad moodulid installimine

Seejärel installige ülejäänud moodulid.

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

Installige moodulid oma `node_modules` directory:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Luua server.js faili oma sõltuvused

Funktsiooni `server.js` faili pakub enamik funktsioone serverisse Veebiteenuste jaoks. 

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

Luua mõne `server.js` toimetaja faili. Lisage järgmine teave:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Salvestage fail. Naasete selle juurde hiljem.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Azure AD sätete salvestamiseks config.js faili loomine

Selle koodi faili edastab konfiguratsiooni parameetrid: Azure'i AD portaalis on `Passport.js` faili. Veebi-API lisamisel esimene osa on walk-through portaali loodud need väärtused. Selgitame, mida panna järgmiste parameetrite väärtused, kui kopeerite kood.

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

Luua mõne `config.js` toimetaja faili. Lisage järgmine teave:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Nõutav väärtused

`clientID`: Veebi-API rakenduse kliendi ID.

`IdentityMetadata`: See on koht, kus `passport-azure-ad` otsib konfiguratsiooni andmete jaoks identiteedipakkuja juures. Otsib ka valideerimiseks JSON web sõned võtmed. 

`audience`: Ressursiidentifikaator (URI) mis tuvastab helistaja rakenduse portaalist. 

`tenantName`: Oma rentniku nimi (nt **contoso.onmicrosoft.com**).

`policyName`: Poliitika, mida soovite valideerida serverisse tulevad märkide. See peaks olema sama poliitika kasutatav kliendi rakenduse Logi sisse.

> [AZURE.NOTE] Meie B2C eelvaate jaoks kasutada sama poliitika nii kliendi ja serveri häälestus. Kui teil on juba lõpetatud on walk-through ja loodud need poliitikad, te ei pea selleks uuesti. Kuna olete lõpule jõudnud, kuvatakse walk-through, peate ei tohiks luua uue poliitika kliendi juhendid saidil.

## <a name="add-configuration-to-your-serverjs-file"></a>Lisage server.js faili konfigureerimine

Lugeda olevad väärtused on `config.js` loodud faili, lisage soovitud `.config` vajalikku ressurssi oma rakenduse nime esitusviis ja seejärel seadke globaalsed muutujad neid on `config.js` dokumendi.

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

Avage soovitud `server.js` toimetaja faili. Lisage järgmine teave:

```Javascript
var config = require('./config');
```
Kui soovite uue jaotise lisamine `server.js` , mis sisaldavad järgmist:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Seejärel lisame kohatäiteid, saame meie helistaja rakenduste kasutajad.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Esmalt minna ja meie puuraidur liiga loomine.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Mongoose abil MongoDB mudeli ja -skeemifailid teabe lisamine

Varasemate ettevalmistamine tasub ära, kui te neid kolme faile koos REST API teenuse tuua.

See walk-through kasutada MongoDB hoida oma ülesandeid, nagu varem.

Klõpsake soovitud `config.js` faili, mida nimetatakse oma andmebaasi **tasklist**. Selle nimi on ka lõpus, mida saate panna selle `mongoose_auth_local` ühenduse URL-i. Te ei pea eelnevalt selle andmebaasi loomiseks MongoDB. See loob andmebaasi teile esimene serveri rakenduse käivitamine.

Pärast seda, kui annate server millist MongoDB andmebaasi kasutama, peate luua mudel ja tööülesannete serveri skeemi mõned täiendavad koodi kirjutamiseks.

### <a name="expand-the-model"></a>Kui seate mudeli laiendamine

See skeemi mudel on lihtne. Saate laiendada vastavalt vajadusele.

`owner`: Kes on määratud tööülesanne. See objekt on **string**.  

`Text`: Ülesanne, ise. See objekt on **string**.

`date`: Kuupäev, mis on tööülesande tähtaja. See objekt on soovitud **kuupäev ja kellaaeg**.

`completed`: Kui tööülesannet on lõpule viidud. See objekt on on **kahendväärtus**.

### <a name="create-the-schema-in-the-code"></a>Kood skeemi loomine

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

Avage soovitud `server.js` faili redaktor. Järgmine teave all konfiguratsiooni kirje lisamiseks tehke järgmist.

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Esmakordsel loomisel skeemi ja seejärel luua mudeli objekti, kui määratlete oma **marsruudib**kogu koodi andmete talletamiseks kasutatav.

## <a name="add-routes-for-your-rest-api-task-server"></a>Lisada serverisse REST API-ga

Nüüd, kui teil on andmebaasi mudeli töötamiseks, lisada kasutate REST API-serverisse.

### <a name="about-routes-in-restify"></a>Marsruudib rakenduses Restify kohta

Marsruudib töötada samal viisil töötavate, kui nad kasutavad Express virnas Restify. Marsruudib määratlemine eeldatavat klientrakendustes helistamiseks URI. 

Tüüpilised mustri Restify marsruudi on:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify ja Express pakuvad palju süvitsi funktsioonide, nt rakenduse failitüüpide määratlemine ja tehes keerukate marsruutimise kogu muu lõpp-punktid. Selleks et selles õpetuses, me hoida lennuliinide lihtne.

#### <a name="add-default-routes-to-your-server"></a>Lisage oma vaikimisi marsruudib

Nüüd lisage meie REST API lihtsa CRUD marsruudib **loomine** ja **loend** . Muud marsruudib leiate funktsiooni `complete` haru proovi.

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

Avage soovitud `server.js` toimetaja faili. Allpool kohal tehtud andmebaasi kirjeid lisada järgmine teave:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Tõrketöötluse lennuliini jaoks

Lisage mõni tõrge töötlemise nii, et saate suhelda nii, et seda saavad aru kliendile ilmneb probleeme.

Lisage järgmine kood:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Saate luua oma serveri

Nüüd on määratletud andmebaasi ja oma marsruudib kasutusele. Viimane asi, mida saate teha on lisada serveri eksemplar, mida haldab teie kõnesid.

Restify ja Express sügav kohandamise REST API server, kuid me kasutame kõige seadistamine siin. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Lisada server (autentimata)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Lisage oma REST API autentimine

Nüüd, kui teil on töötab REST API-server, saate selle kasulik Azure AD vastu.

Muuta käsureal, et kataloogi `azuread`, kui see pole juba olemas:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Kasutage OIDCBearerStrategy, mis on kaasas pass-azure-ad


> [AZURE.TIP]
Kui kirjutate API-d, tuleks alati andmete linkimine midagi kordumatu märgiks, et kasutaja ei saa võltsida. Kui server salvestab ToDo üksused, see nii põhineb soovitud **objekti ID** kasutaja luba (nn token.oid kaudu), mis läheb väljale "omanik". See väärtus tagab, et ainult kasutaja pääseb juurde oma ToDo üksusi. Puudub puuduvad "omanik," API Väliskasutaja saate taotleda teiste ToDo üksuste isegi juhul, kui need on autenditud.

Järgmiseks kasutage esitaja strateegia, mis on kaasas `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Pass kasutab sama muster kõik selle strateegiad. Saate edastada, et see on `function()` , millel on `token` ja `done` parameetrid. Strateegia naaseb teie pärast seda, kui see on valmis kõigi oma töö. Peaksite kasutajal talletada ja luba salvestada, et te ei pea seda enam küsi.

> [AZURE.IMPORTANT]
Ülaltoodud kood võtab igale kasutajale, kes juhtub autentida serverisse. Selle protsessi tuntakse autoregistration. Klõpsake tootmisserverid, ei lase kõik kasutajad Accessi API ilma neid läbida registreerimise käigus. See toiming on tavaliselt mustri, näete tarbija rakendustes, mis võimaldavad teil registreerida, kasutades Facebooki, aga siis küsida täitmine täiendavat teavet. Kui see programm ei olnud käsurea programmi, me võib olla ekstraktimist e-posti Turbeloa objekti, mis on tagastatud ja seejärel küsitakse kasutajad tohiksid täiendavat teavet. Kuna tegemist on valimi, lisame andmebaasi-mälu.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Kinnitamaks, et see hülgab teie serveri rakenduse käivitamine

Saate kasutada `curl` kuvamiseks, kui teil on nüüd OAuth2 kaitse lõpp-punktide suhtes. Päised, tagastatakse peaks olema piisavalt, et teada saada, et olete õigel teel.

Veenduge, et teie MongoDB töötab.

    $sudo mongodb

Muuta kataloogi ja käivitage server:

    $ cd azuread
    $ node server.js

Uues aknas terminal, käivitamine`curl`

Proovige lihtsa POSTITUSELE.

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Tõrke 401 on soovitud vastuse. See näitab, et pass kiht proovib Autoriseerin lõpp-punkti ümber suunata.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Nüüd on teil REST API teenus, mis kasutab OAuth2

REST API rakendatud Restify ja OAuthi abil! Nüüd on piisavalt koodi nii, et saate jätkata oma teenuse arendamise ja luua selle näite. Olete läinud kui saate selle serveri e OAuth2 ühilduv klient kasutamata. Selle järgmise juhise jaoks kasutada ka täiendavad walk-through nagu meie [ühenduse loomine mõne veebi-API, kasutades iOS-is B2C](active-directory-b2c-devquickstarts-ios.md) kiirtutvustus.


## <a name="next-steps"></a>Järgmised sammud

Saate nüüd liikuda täpsemate teemade, näiteks:

[Ühenduse veebi-API B2C iOS-i abil](active-directory-b2c-devquickstarts-ios.md)