<properties
    pageTitle="Azure'i AD v2.0 NodeJS Veebiteenuste | Microsoft Azure'i"
    description="Kuidas koostada NodeJS Web API aktsepteerib sõned nii isikliku Microsofti Account ja töökoha või õppeasutuse konto."
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
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>Turvaline Web API-ga node.js abil

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

Azure Active Directory v2.0 lõpp-punkti, saate kaitsta Web API abil [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) Accessi sõned, mis võimaldab kasutajatel nii isikliku Microsofti kontoga ja töö või kooli kontod turvaline juurdepääs veebi-API-ga.

**Pass** on autentimine vahevara Node.js jaoks. Väga paindlik ja muutuv, pass saate tuleb tähelepandamatult panna igal Express põhineva või veebirakenduse Resitify. Strateegiad tugi autentimist kasutades kasutajanime ja parooli, Facebooki, Twitteri ja täielik kogum. Meil on välja töötatud strateegia Microsoft Azure Active Directory. Me selle mooduli installimiseks ja seejärel lisage Microsoft Azure Active Directory `passport-azure-ad` lisandmooduli.

## <a name="download"></a>Laadi alla
Selle õpetuse kood säilitatakse [github](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).  Jälgida, saate [alla laadida rakenduse skelett nimega on zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) - või klooni skelett:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Lõplikus rakendust pakutakse ka õppeteema lõpus.


## <a name="1-register-an-app"></a>1. register rakendus
Luua uue rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või järgige neid [üksikasjalikke juhiseid](active-directory-v2-app-registration.md).  Veenduge, et:

- Kopeeri allapoole määratud rakenduse **Rakenduse Id** peate selle varsti.
- Lisage oma rakenduse **Mobile** platvormi.
- Kopeerige allapoole **Suunake URI** portaalist. Kasutage vaikeväärtust, milleks on `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: Laadige alla oma platvormile node.js
Selle valimi edukalt kasutamiseks peab teil olema mõne Node.js töötamine installi.

Installige Node.js [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: installi MongoDB kellelegi oma

Selle valimi edukalt kasutamiseks peab teil olema mõne MongoDB töötamine installi. Kasutame MongoDB mitmes eksemplaris server meie REST API püsiv teha.

Installige MongoDB [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] See teid läbi endale kasutada MongoDB, mis kirjutamise ajal on vaikimisi installi ja serveri lõpp-punktid: mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: installige Restify moodulid veebi-API-ga

Me kasutame Resitfy koostamiseks meie REST API-ga. Restify on minimaalne ja paindlikus Node.js rakenduse raamistiku saadud Express, mis on funktsioonide jaoks peale ühendamine REST API-de komplekti.

### <a name="install-restify"></a>Installi Restify

Käsurea, muuta kataloogide azuread kataloogi. Kui **azuread** kausta pole olemas, looge see.

`cd azuread`- või -`mkdir azuread;`

Tippige järgmine käsk:

`npm install restify`

See käsk installib Restify.

#### <a name="did-you-get-an-error"></a>Kas kuvatakse tõrketeade?

Teatud opsüsteemides npm kasutamisel võidakse kuvada tõrketeade tõrge: EPERM chmod "/ usr/kohaliku/prügikast /..." ja taotluse proovige konto administraatorina. Sellisel juhul käsu sudo käivitamiseks npm kõrgema taseme õigusi.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Kas teile kuvati tõrke DTrace kohta?

Restify installimisel võidakse kuvada umbes selline:

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


Restify võimaldab võimsaid ülejäänud telefonikõned DTrace jälgi. Paljude operatsioonisüsteemide ei ole saadaval DTrace. Saate nende vigade turvaline ignoreerida.


Selle käsu väljund peaks kuvatama umbes järgmine:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: installi Passport.js sisse veebi-API-ga

[Pass](http://passportjs.org/) on autentimine vahevara Node.js jaoks. Väga paindlik ja muutuv, pass saate tuleb tähelepandamatult panna igal Express põhineva või veebirakenduse Resitify. Strateegiad tugi autentimist kasutades kasutajanime ja parooli, Facebooki, Twitteri ja täielik kogum. Meil on välja töötatud strateegia Azure Active Directory. Töötame selle mooduli installimiseks ja lisage Azure Active Directory strateegia lisandmooduli.

Käsurea, muuta kataloogide azuread kataloogi.

Sisestage järgmine käsk installida passport.js

`npm install passport`

Käsu väljund peaks kuvatama umbes järgmine:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: veebi-API-ga pass-Azure-AD lisamine

Seejärel lisame OAuthi strateegia, pass-azuread, komplekti strateegiaid, mis kasutajaga pass Azure Active Directory abil. See Rest API valimi me kasutame esitaja märkide strateegia.

> [AZURE.NOTE] Kuigi OAuth2 raamistiku, mis tahes teadaolevad loa tüüp välja antud, ainult teatud Turbeloa saanud kasutamist. Lõpp-punktid, mis on muutunud teada olema esitaja sõned kaitsta. Esitaja sõned on kõige suuresti välja luba OAuth2 tüüp ja palju rakendusi eeldatakse, et esitaja sõned on ainult luba välja tüüp.

Käsurea, muuta kataloogide azuread kataloogi

Tippige järgmine käsk installida Passport.js pass-azure-ad moodul:

`npm install passport-azure-ad`

Käsu väljund peaks kuvatama umbes järgmine:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: veebi-API-ga MongoDB moodulid lisamine

Me kasutame MongoDB nimega meie andmesalve seetõttu, läheb vaja installida nii ulatuslikult kasutatud mudelid ja skeemid haldamise lisandmooduli nimega Mongoose, kui ka andmebaasi draiver MongoDB, nimetatakse ka MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: installida täiendavad moodulid

Järgmiseks tuleb meil installida ülejäänud moodulid.


Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`


Sisestage installida järgmised moodulid node_modules kataloogis järgmised käsud:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: luua mõne server.js oma sõltuvused

Server.js faili pakkuda meie funktsiooni enamik meie veebi-API server. Meil lisada meie kood enamik seda faili. Tootmiseks oleks refaktoorime väiksemate failide, nt eraldi marsruudib ja kontrollerid funktsioone. Selleks, et see demo kasutame server.js seda funktsiooni.

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Luua mõne `server.js` meie lemmik redaktoris faili ja lisada järgmine teave:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Salvestage fail. Me ilmuvad taas sellele.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: Azure'i AD sätete salvestamiseks config faili loomine

Failile kood läheb konfiguratsiooni parameetrid oma Azure Active Directory portaali Passport.js. Veebi-API lisamisel esimene osa on kiirtutvustus portaali loodud need väärtused. Me selgitame, mida panna järgmiste parameetrite väärtused, kui olete koodi.


Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Luua mõne `config.js` meie lemmik redaktoris faili ja lisada järgmine teave:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Nõutav väärtused

*IdentityMetadata*: see on, kus pass-azure-ad näeb ka IdP samuti võtmed JWT sõned valideerimiseks konfigureerimine andmete jaoks. Saate tõenäoliselt soovite muuta seda, kui Azure Active Directory abil.

*sihtrühm*: redirect URI portaalist.

> [AZURE.NOTE]
Me roll sagedaste meie võtmed. Veenduge, et teil on alati tõmbamine "openid_keys" URL-i ja rakenduse saab Interneti.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: lisamine server.js faili konfigureerimine

Lugege need väärtused Config faili äsja loodud üle meie rakendus Meil. Selle tegemiseks lihtsalt lisada .config fail nõutavad ressursi meie rakenduses ja seadma nende config.js dokumendi globaalsed muutujad

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Avage oma `server.js` meie lemmik redaktoris faili ja lisada järgmine teave:

```Javascript
var config = require('./config');
```
Lisage uus jaotis, et `server.js` järgmine kood:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Samm 12: The MongoDB mudeli ja Moongoose skeemi teabe lisamine

Kõik selles ettevalmistamine nüüd alustada pöörates nagu me Tuul need kolm faili koos teenusega REST API-ga.

See selgituse me kasutame MongoDB talletamiseks ülesanded, nagu on kirjeldatud ***sammu 4***.

Kui te ei mäleta lõime samm 11 config.js failist, nimetatakse me meie andmebaasi *tasklist*, mis olid Panime meie mogoose_auth_local ühenduse URL-i lõpus. Te ei pea eelnevalt selle andmebaasi loomiseks MongoDB, see loob see meile esimese Käivita meie serverirakenduse (kui see pole veel olemas).

Nüüd kus oleme olete ütles server mis MongoDB andmebaas, mida soovite kasutada, läheb vaja luua mudeli ja -skeemifailid meie Serveri tööülesanded mõned täiendavad koodi kirjutamiseks.

#### <a name="discussion-of-the-model"></a>Arutelu mudeli

Meie skeemi mudel on väga lihtne ja laiendada seda vastavalt vajadusele.

NIMI – nimi, kellele on määratud tööülesanne. ***String***

TÖÖÜLESANDE - ise. ***String***

KUUPÄEV – kuupäev, mis on tööülesande tähtaja. ***Kuupäeva ja kellaaja*** lisamine

VALMIS – kui tööülesanne on lõpule viidud, või mitte. On ***KAHENDVÄÄRTUS***

#### <a name="creating-the-schema-in-the-code"></a>Skeemi loomisel kood


Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Avage oma `server.js` meie lemmik redaktoris faili ja lisada konfiguratsiooni kirje all järgmine teave:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
See MongoDB serveriga ühenduse loomise ja jaotamiseks tagasi skeemi objekti meile.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>Skeemi kasutamisel kood meie andmemudeli loomine

Allpool Kirjutasite eespool koodi, lisage järgmine kood:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Nagu näete koodi, meie skeemi loomine ja seejärel kasutame kogu koodi meie andmete talletamiseks, kui me määratleda meie ***marsruudib***mudeli objekti loomine.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>Samm 13: Meie lisada meie tööülesande REST API-server

Nüüd kus oleme andmebaasi mudel töötamiseks, vaatame lisada kasutame meie REST API-server.

### <a name="about-routes-in-restify"></a>Marsruudib kohta rakenduses Restify

Marsruudib töötada Restify täpselt samamoodi need Express virnas abil teha. Määratlege marsruudib eeldatavat klientrakendustes helistamiseks URI abil. Tavaliselt määratleda oma marsruudib hoitakse eraldi failis. Meie eesmärkide puhul Panime meie marsruudib server.js faili. Soovitame, et te neid tegur tootmisotstarbeks oma faili.

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


See on kõige olulisemaid tasemel muster. Resitfy (ja Express) pakuvad palju süvitsi functionaltiy nagu määratletakse taotluse tüübid ja tehes keerukate marsruutimise kogu muu lõpp-punktid. Meie eesmärkide puhul me hoida lennuliinide väga lihtsalt.

#### <a name="add-default-routes-to-our-server"></a>Meie server vaikimisi marsruudib lisamine

Oleme nüüd lisada lihtsa CRUD loomine, laadi, värskenduse ja kustutada.

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Avage oma `server.js` meie lemmik redaktoris faili ja lisada andmebaasikirjete, olete teinud eespool all järgmine teave:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Osa jaoks soovitud marsruudib tõrketöötluse

On mõistlik lisada mõne tõrge töötlemise nii, et saaksime suhelda kliendile meil viisil tekkinud probleem seda saavad aru.

Järgmine kood all olete kirjutanud kohal koodi lisamiseks tehke järgmist.

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


## <a name="step-14-create-your-server"></a>Samm 14: Looge oma serveri!

Meil on meie andmebaasi määratletud, meil on meie marsruudib kohas ja Viimane asi, mida teha on lisada meie serveri eksemplar, mida haldab meie kõned.

Restify (ja kiire) on palju sügav kohandamine saate teha REST API server, kuid uuesti kasutame kõige seadistamine meie eesmärkide puhul.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: marsruudib (ilma autentimine nüüd) lisamine

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: enne OAuthi toe lisamiseks vaatame käivitada server.

Oma serveri katsetada, enne kui lisame autentimine

Lihtsaim viis selleks on käsurea curl abil. Enne seda, läheb vaja lihtsa kasuliku, mis võimaldab teil sõeluda JSON väljundit. Selleks, et installida json tööriista nagu alltoodud näiteid kasutada.

`$npm install -g jsontool`

See installib JSON tööriista globaalselt. Nüüd, kui olete saavutada, mis – mängime server:

Esmalt veenduge, et teie monogoDB isntance töötab.

`$sudo mongod`

Seejärel muuta kataloogi ja käivitage curling.

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Seejärel lisame tööülesande sel viisil:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

Vastus peaks olema:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
Ja me loendis tööülesanded Brandon sellisel viisil.

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Kui see kõik töötab, oleme valmis OAuthi lisada REST API-serverisse.

**Teil on REST API serveris MongoDB!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: meie REST API serveri autentimine lisamine

Nüüd kui töötava REST API (palju õnne, btw!) vaatame leida mistõttu on kasulik Azure AD vastu.

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: oidcbearerstrategy, mis on kaasas pass-azure-ad kasutamine

Meil on nii palju ehitatud tüüpiline ülejäänud TODO server ilma igasuguse autoriseerimine. See on, kui Alustame paneb, et koos.

Kõigepealt, on vaja näitamaks, et soovime kasutada pass. Pange see õigus pärast teie serveri konfiguratsiooni:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
API-de kirjutamisel tuleks alati andmete linkimine midagi kordumatu märgiks, et kasutaja ei saa võltsida. Kui see server salvestab TODO üksused, talletatakse neid põhineb kasutaja luba (nn token.sub kaudu), millele Panime "omanik" välja Tellimuse ID-d. See tagab, et ainult kasutaja pääseb juurde oma ülesanded ja keegi teine on juurdepääs ülesanded, sisestatud. Puudub puuduvad "omanik" API Väliskasutaja saate taotleda teise isiku ülesanded isegi juhul, kui need on autenditud.

Järgmiseks kasutame pass-azure-ad kaasneva avatud ID ühenduse esitaja strateegia. Lihtsalt pilk kood praegu, ma selgitada varsti. Pange see pärast mida riikide ettevõtet kohal:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Pass kasutab kõigi strateegiad (Twitteri, Facebooki, jne), et kõik strateegia poolt kinni on sarnane. Strateegia vaadates näete võtame funktsioon, mis on märgiks ja valmis parameetrid. Strateegia kohusetundlikult naaseb meile pärast seda, kas kõik selle kasutaja töö. Kui see ei soovi me kasutaja talletada ja Kätkö luba, et me ei pea seda enam küsi.

> [AZURE.IMPORTANT]
Ülaltoodud kood võtab iga kasutaja kohta, mis juhtub autentida meie server. Seda nimetatakse automaatne registreerimine. Tootmisserverid ei soovite lubada kõigil ilma neid läbida otsustate registreerimise protsess. See on tavaliselt mustri, näete tarbija rakendustes, kes võimaldab teil registreerida Facebooki, aga siis küsida täitmine täiendavat teavet. Kui see ei olnud käsurea programmi, me võib olla lihtsalt ekstraktimist e-posti Turbeloa objekti, mis on tagastatud ja seejärel küsitakse nende täitmine täiendavat teavet. Kuna see on test lihtsalt lisame mälu-andmebaasi server.

### <a name="2-finally-protect-some-endpoints"></a>2 viimaks kaitse mõned lõpp-punktid

Saate kaitsta lõpp-punktid passport.authenticate() Helista määrates protokolli, mida soovite kasutada.

Meie tee midagi huvitavamaks meie serveri koodi redigeerimine:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: serveri rakendus uuesti ja veenduge, et see hülgab saate

Kasutame `curl` uuesti, et näha, kui meil on nüüd OAuth2 kaitse vastu meie lõpp-punktid. Teeme selle enne runnning ühte meie klientide SDK-d selle lõpp-punkti suhtes. Päised, tagastatakse peaks olema piisavalt meile alla õige tee.

Esmalt veenduge, et teie monogoDB isntance töötab.

    $sudo mongod

Seejärel muuta kataloogi ja käivitage curling.

    $ cd azuread
    $ node server.js

Proovige lihtsa POSTITUSELE.

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 on vastus otsite siin, mis näitab, et pass kiht üritab ümber suunata Autoriseerin lõpp-punkti, mis on täpselt, mida soovite.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Palju õnne! Teil on REST API teenuse abil OAuth2!

Olete läks, kui saate selle serveri e OAuth2 ühilduvad klient kasutamata. Peate läbima mõne täiendavad kiirtutvustus.

Kui lihtsalt otsite teavet, kuidas rakendada REST API-ga Restify ja OAuth2 abil, on teil rohkem kui piisavalt kood säilitada oma teenuse arendamise ja õppida, kuidas luua näites.

## <a name="next-steps"></a>Järgmised sammud

Viide, lõpule viidud näidis (ilma väärtuste määramine) [soovitud ZIP siin on saadaval](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip)või saate klooni seda GitHub kaudu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Nüüd saate teisaldada täpsemate teemade peale.  Võite proovida:

[Turvaline Node.js web Appi abil v2.0 lõpp-punkti >>](active-directory-v2-devquickstarts-node-web.md)

Täiendavad ressursid, vaadake:
- [V2.0 arendaja juhend >>](active-directory-appmodel-v2-overview.md)
- [Sildi "azure-active directory" StackOverflow >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Meie toodete värskendusi turvalisus

Soovitame teil saada teatisi, kui turvalisus ilmnevad külastades [selle lehe](https://technet.microsoft.com/security/dd252948) ja turbeteatised nõu tellimise.
