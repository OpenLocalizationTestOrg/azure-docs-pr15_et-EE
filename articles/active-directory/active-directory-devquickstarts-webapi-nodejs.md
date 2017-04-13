<properties
    pageTitle="Azure'i AD NodeJS alustamine | Microsoft Azure'i"
    description="Kuidas koostada Node.js ülejäänud Web API, mis ühendab Azure AD autentimiseks."
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

# <a name="getting-started-with-web-api-for-node"></a>Alustamine: veebi-API sõlm

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

**Pass** on autentimine vahevara Node.js jaoks. Väga paindlik ja muutuv, pass saate tuleb tähelepandamatult panna igal Express põhineva või veebirakenduse Resitify. Strateegiad tugi autentimist kasutades kasutajanime ja parooli, Facebooki, Twitteri ja täielik kogum. Meil on välja töötatud strateegia Microsoft Azure Active Directory. Me selle mooduli installimiseks ja seejärel lisage Microsoft Azure Active Directory `passport-azure-ad` lisandmooduli.

Selleks, peate:

1. Azure AD Rakenduse registreerimine
2. Häälestada rakenduse kasutamiseks pass azure-ad-pass lisandmooduli.
3. Klientrakenduse helistamiseks abil teha loendi Web API konfigureerimine

Selle õpetuse kood säilitatakse [github](https://github.com/Azure-Samples/active-directory-node-webapi).

> [AZURE.NOTE] See artikkel ei hõlma kuidas rakendada Logi sisse, registreerumise ja Azure AD B2C profiili juhtimine.  Keskendutakse helistaja web API-d, kui kasutaja on juba autenditud.  Kui te pole seda veel teinud, saate peab algama eesliitega [Kuidas integreerimine Azure Active Directory dokumendi](./develop/active-directory-how-to-integrate.md) Azure Active Directory põhitoimingute kohta.


Oleme välja kõik see töötab näide GitHub jaotises on MIT Litsents lähtekoodi nii julgelt klooni (või isegi parem, kahvel!) ja tagasiside ja tõmmake taotlused.

## <a name="about-nodejs-modules"></a>Node.js moodulid kohta

Me kasutame Node.js moodulid ülevaate. Moodulid on koormatavate JavaScripti pakette, mille rakenduse teatud funktsioone. Tavaliselt installitakse moodulid NPM installikaust Node.js NPM käsurea tööriista abil, kuid mõned moodulid, nt HTTP moodulit, kaasatakse tuum Node.js pakett.
Installitud moodulid salvestatakse teie Node.js installikaust juurtasemel kataloogis node_modules. Iga mooduli kataloogis node_modules säilitab oma node_modules kausta, mis sisaldab mis tahes moodulid, et see sõltub ja iga nõutav mooduli on node_modules kataloog. See rekursiivse struktuuri osa directory tähistab sõltuvus ahelas.

See sõltuvus ahelas struktuur, on tulemuseks suurem rakenduse andmed väiksemates vähem ruumi, kuid see tagab, et kõik sõltuvused on täidetud ja et kasutatud arendatakse moodulid versiooni kasutatakse ka valmistamisel. See muudab tootmise rakenduse käitumine nõuaksid vähem lisatööd ja takistab versioonimise probleeme, mis võivad kasutajaid mõjutada.

## <a name="1-register-a-azure-ad-tenant"></a>1. registreerida Azure AD rentniku jaoks

Selle valimi kasutamiseks on vaja Azure Active Directory rentniku. Kui te pole kindel, millist rentniku on ja kuidas soovite saada üks, lugege teemat [Kuidas saada on Azure AD rentniku](active-directory-howto-tenant.md).

## <a name="2-create-an-application"></a>2 rakenduse loomine

Nüüd on vaja luua rakenduse kataloogis, mis annab Azure AD osa teabest rakenduse turvaliselt suhelda.  Kliendi rakendus nii veebi-API esindab ühe **Rakenduse ID** sel juhul, kuna need sisaldavad üks loogiline rakendus.  Rakenduse loomiseks järgige [neid juhiseid](active-directory-how-applications-are-added.md). Kui olete hoone ka ärivaldkonna rakenduse [need täiendavad juhised võib olla kasulik](active-directory-applications-guiding-developers-for-lob-applications.md).

Kindlasti järgmist.

- Logige sisse teenusekomplekti Azure haldusportaali.
- Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**.
- Valige rentniku, kuhu soovite rakenduse registreerida.
- Klõpsake vahekaarti **rakendused** ja klõpsake nuppu Lisa alla sahtlis.
- Järgige viipasid ja luua mõne uue **veebirakenduse ja/või WebAPI**.
    - Rakenduse **nimi** kirjeldada oma rakenduse lõppkasutajad
    - **Sisselogimise URL** on rakenduse base URL.  Proovi kood vaikimisi `https://localhost:8080`.
    - **Rakenduse ID URI** on rakenduse jaoks kordumatut tunnust.  Soovite on kasutada `https://<tenant-domain>/<app-name>`, nt`https://contoso.onmicrosoft.com/my-first-aad-app`
- Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü konfigureerimine.

- MEELDETULETUSE: **Rakenduse salajane** rakenduse loomine ja kopeerige see alla.  Peate varsti.
- Meeldetuletus: Kopeerimine **Rakenduse ID** , mis määratakse teie rakendus alla.  On vaja see varsti.


## <a name="3-download-nodejs-for-your-platform"></a>3. Laadige alla oma platvormile node.js
Selle valimi edukalt kasutamiseks peab teil olema mõne Node.js töötamine installi.

Installige Node.js [http://nodejs.org](http://nodejs.org).

## <a name="4-install-mongodb-on-to-your-platform"></a>4. installida MongoDB kellelegi oma

Selle valimi edukalt kasutamiseks peab teil olema mõne MongoDB töötamine installi. Kasutame MongoDB mitmes eksemplaris server meie REST API püsiv teha.

Installige MongoDB [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] See teid läbi endale kasutada MongoDB, mis kirjutamise ajal on vaikimisi installi ja serveri lõpp-punktid: mongodb://localhost


## <a name="5-install-the-restify-modules-in-to-your-web-api"></a>5. installige Restify moodulid veebi-API-ga

Me kasutame Resitfy koostamiseks meie REST API-ga. Restify on minimaalne ja paindlikus Node.js rakenduse raamistiku saadud Express, mis on funktsioonide jaoks peale ühendamine REST API-de komplekti.

### <a name="install-restify"></a>Installi Restify

Käsurea, muuta kataloogide azuread kataloogi. Kui **azuread** kausta pole olemas, looge see.

`cd azuread - or- mkdir azuread; cd azuread`

Tippige järgmine käsk:

`npm install restify`

See käsk installib Restify.

#### <a name="did-you-get-an-error"></a>KAS KUVATAKSE TÕRKETEADE?

Teatud opsüsteemides npm kasutamisel võidakse kuvada tõrketeade tõrge: EPERM chmod "/ usr/kohaliku/prügikast /..." ja taotluse proovige konto administraatorina. Sellisel juhul käsu sudo käivitamiseks npm kõrgema taseme õigusi.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>KAS TEILE KUVATI TÕRKE DTRACE KOHTA?

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
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="6-install-passportjs-in-to-your-web-api"></a>6. installige Passport.js klõpsake veebi-API-ga

[Pass](http://passportjs.org/) on autentimine vahevara Node.js jaoks. Väga paindlik ning muutuv, pass saate tuleb tähelepandamatult panna igal Express põhineva või veebirakenduse Resitify. Strateegiad tugi autentimist kasutades kasutajanime ja parooli, Facebooki, Twitteri ja täielik kogum. Meil on välja töötatud strateegia Azure Active Directory. Me selle mooduli installimiseks ja seejärel lisage Azure Active Directory strateegia lisandmooduli.

Käsurea, muuta kataloogide azuread kataloogi.

Sisestage järgmine käsk installida passport.js

`npm install passport`

Käsu väljund peaks kuvatama umbes järgmine:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="7-add-passport-azure-ad-to-your-web-api"></a>7. lisada pass-Azure-AD veebi-API-ga

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



## <a name="8-add-mongodb-modules-to-your-web-api"></a>8. lisada MongoDB moodulid veebi-API-ga

Me kasutame MongoDB nimega meie andmesalve seetõttu, läheb vaja installida nii ulatuslikult kasutatud mudelid ja skeemid haldamise lisandmooduli nimega Mongoose, kui ka andmebaasi draiver MongoDB, nimetatakse ka MongoDB.


* `npm install mongoose`

## <a name="9--install-additional-modules"></a>9. installida täiendavad moodulid

Järgmiseks tuleb meil installida ülejäänud moodulid.


Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`


Sisestage installida järgmised moodulid node_modules kataloogis järgmised käsud:

* `npm install assert-plus`
* `npm install bunyan`
* `npm update`


## <a name="10-create-a-serverjs-with-your-dependencies"></a>10. luua mõne server.js oma sõltuvused

Faili server.js pakkuda enamik meie funktsionaalsust meie veebi-API server. Lisame meie kood enamik seda faili. Tootmiseks oleks refaktoorime väiksemate failide, nt eraldi marsruudib ja kontrollerid funktsioone. Selleks, et see demo kasutame server.js seda funktsiooni.

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Luua mõne `server.js` meie lemmik redaktoris faili ja lisada järgmine teave:

```Javascript
    'use strict';

    /**
    * Module dependencies.
    */

    var fs = require('fs');
    var path = require('path');
    var util = require('util');
    var assert = require('assert-plus');
    var bunyan = require('bunyan');
    var getopt = require('posix-getopt');
    var mongoose = require('mongoose/');
    var restify = require('restify');
    var passport = require('passport');
  var BearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Salvestage fail. Me ilmuvad taas sellele.

## <a name="11-create-a-config-file-to-store-your-azure-ad-settings"></a>11:. Azure AD sätete salvestamiseks config faili loomine

Failile kood läheb konfiguratsiooni parameetrid oma Azure Active Directory portaali Passport.js. Veebi-API lisamisel esimene osa on kiirtutvustus portaali loodud need väärtused. Me selgitame, mida panna järgmiste parameetrite väärtused, kui olete koodi.


Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Luua mõne `config.js` meie lemmik redaktoris faili ja lisada järgmine teave:

```Javascript
 exports.creds = {
     mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
     clientID: 'your client ID',
     audience: 'your application URL',
    // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
  // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
     identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
     validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
     passReqToCallback: false,
     loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

 };


```
Salvestage fail.

## <a name="12-add-configuration-to-your-serverjs-file"></a>12. lisamine server.js faili konfigureerimine

Lugege need väärtused Config faili äsja loodud üle meie rakendus Meil. Selle tegemiseks lihtsalt lisada .config fail nõutavad ressursi meie rakenduses ja seadma nende config.js dokumendi globaalsed muutujad

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

Avage oma `server.js` meie lemmik redaktoris faili ja lisada järgmine teave:

```Javascript
var config = require('./config');
```
Lisage uus jaotis, et `server.js` järgmine kood:

```Javascript
var options = {
    // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback,
    loggingLevel: config.creds.loggingLevel

};

// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;

// Our logger
var log = bunyan.createLogger({
    name: 'Azure Active Directory Bearer Sample',
         streams: [
        {
            stream: process.stderr,
            level: "error",
            name: "error"
        },
        {
            stream: process.stdout,
            level: "warn",
            name: "console"
        }, ]
});

  // if logging level specified, switch to it.
  if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
```

Salvestage fail.



## <a name="13-add-the-mongodb-model-and-schema-information-using-moongoose"></a>13. MongoDB mudeli ja Moongoose skeemi teabe lisamine

Kõik selles ettevalmistamine nüüd alustada pöörates nagu me Tuul need kolm faili koos teenusega REST API-ga.

See selgituse me kasutame MongoDB talletamiseks ülesanded, nagu on kirjeldatud ***sammu 4***.

Kui te ei mäleta on `config.js` faili lõime ***Samm*** 11 me kutsutud meie andmebaasi `tasklist` mis olid Panime meie mogoose_auth_local ühenduse URL-i lõpus. Te ei pea eelnevalt selle andmebaasi loomiseks MongoDB, see loob see meile esimese Käivita meie serverirakenduse (kui see pole veel olemas).

Nüüd kus oleme olete ütles server mis MongoDB andmebaas, mida soovite kasutada, läheb vaja luua mudel ja -skeemifailid meie Serveri tööülesanded mõned täiendavad koodi kirjutamiseks.

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
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

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

## <a name="14-add-our-routes-for-our-task-rest-api-server"></a>14. lisada meie meie tööülesande REST API-server

Nüüd kus oleme andmebaasi mudel töötamiseks, lisada marsruudib, kasutame meie REST API-server.

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

### <a name="1-add-default-routes-to-our-server"></a>1. vaikimisi marsruudib lisamine meie server

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
        req.log.warn('createTodo: missing task');
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

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initalize the database as stated in the README?");
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

### <a name="2-next-lets-add-some-error-handling-in-our-apis"></a>2 edasi, lisame meie API-de käitlemine viga:

```

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


## <a name="15-create-your-server"></a>15. luua oma Server!

Meil on meie andmebaasi määratletud, meil on meie marsruudib kohas ja Viimane asi, mida teha on lisada meie serveri eksemplar, mida haldab meie kõned.

Restify (ja kiire) on palju sügav kohandamine saate teha REST API server, kuid uuesti kasutame kõige seadistamine meie eesmärkide puhul.

```Javascript
/**
 * Our Server
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
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
```

## <a name="16-adding-the-routes-to-the-server-without-authentication-for-now"></a>16. lisamine soovitud marsruudib server (ilma autentimine nüüd)

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

## <a name="17-before-we-add-oauth-support-lets-run-the-server"></a>17. enne OAuthi toe lisamiseks vaatame käitada.

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
HTTP/1.1 200 OK
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

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

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


## <a name="18-add-authentication-to-our-rest-api-server"></a>18. lisamine meie REST API serveri autentimine

Nüüd kui töötava REST API (palju õnne, btw!) vaatame leida mistõttu on kasulik Azure AD vastu.

Käsurea, muutke kataloogide **azuread** kausta, kui see juba ei seal.

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: OIDCBearerStrategy, mis on kaasas pass-azure-ad kasutamine

Meil on nii palju ehitatud tüüpiline ülejäänud TODO server ilma igasuguse autoriseerimine. See on, kui Alustame paneb, et koos.

Kõigepealt, on vaja näitamaks, et soovime kasutada pass. Pange see õigus pärast teie serveri konfiguratsiooni:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
API-de kirjutamisel tuleks alati andmete linkimine midagi kordumatu märgiks, et kasutaja ei saa võltsida. Kui see server salvestab TODO üksused, talletatakse neid põhineb kasutaja luba (nn token.oid kaudu), millele Panime välja "omanik" objekti ID-d. See tagab ainult kasutaja pääseb juurde oma ülesanded ja keegi teine on juurdepääs ülesanded, sisestatud. Puudub puuduvad "omanik" API Väliskasutaja saate taotleda teise isiku ülesanded isegi juhul, kui need on autenditud.

Järgmiseks kasutage Alustame esitaja strateegia, mis on kaasas pass-azure-ad. Lihtsalt pilk kood praegu, ma selgitada varsti. Pange see pärast mida riikide ettevõtet kohal:

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


var bearerStrategy = new BearerStrategy(options,
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

passport.use(bearerStrategy);
```

Pass kasutab kõigi strateegiad (Twitteri, Facebooki, jne), et kõik strateegia poolt kinni on sarnane. Strateegia vaadates näete võtame funktsioon, mis on märgiks ja valmis parameetrid. Strateegia kohusetundlikult naaseb meile pärast seda, kas kõik selle kasutaja töö. Kui see ei soovi me kasutaja talletada ja Kätkö luba, et me ei pea seda enam küsi.

> [AZURE.IMPORTANT]
Ülaltoodud kood võtab iga kasutaja kohta, mis juhtub autentida meie server. Seda nimetatakse automaatne registreerimine. Tootmisserverid ei soovite lubada kõigil ilma neid läbida otsustate registreerimise protsess. See on tavaliselt mustri, näete tarbija rakendustes, kes võimaldab teil registreerida Facebooki, aga siis küsida täitmine täiendavat teavet. Kui see ei olnud käsurea programmi, me võib olla lihtsalt ekstraktimist e-posti Turbeloa objekti, mis on tagastatud ja seejärel küsitakse nende täitmine täiendavat teavet. Kuna see on test lihtsalt lisame mälu-andmebaasi server.

### <a name="2-finally-protect-some-endpoints"></a>2 viimaks kaitse mõned lõpp-punktid

Saate kaitsta lõpp-punktid, määrates selle `passport.authenticate()` kõne alustamine Protocol (protokoll), mida soovite kasutada.

Meie tee midagi huvitavamaks meie serveri koodi redigeerimine:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. serveri rakenduse uuesti käivitada, ja veenduge, et see hülgab saate

Kasutame `curl` uuesti, et näha, kui meil on nüüd OAuth2 kaitse vastu meie lõpp-punktid. Teeme selle enne runnning ühte meie klientide SDK-d selle lõpp-punkti suhtes. Päised, tagastatakse peaks olema piisavalt meile alla õige tee.

Esmalt veenduge, et teie monogoDB eksemplari töötab.

  $sudo mongod

Seejärel muuta kataloogi ja käivitage curling.

  $ CD-le azuread $ sõlm server.js

Proovige lihtsa POSTITUSELE.

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 on vastus otsite siin, mis näitab, et pass kiht proovib ümber suunata Autoriseerin lõpp-punkti, mis on täpselt, mida soovite.

## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Palju õnne! Teil on REST API teenuse abil OAuth2!

Olete läks, kui saate selle serveri e OAuth2 ühilduvad klient kasutamata. Peate läbima mõne täiendavad kiirtutvustus.

Kui lihtsalt otsite teavet, kuidas rakendada REST API-ga Restify ja OAuth2 abil, on teil rohkem kui piisavalt kood säilitada oma teenuse arendamise ja õppida, kuidas luua näites.

Kui olete huvitatud järgmiste juhiste juurde ADAL reisi, siis siin on mõned toetatud ADAL kliendid, soovitame teil tööd jätkata.

Lihtsalt klooni arendaja oma arvutisse alla ja toodud selle kiirtutvustus konfigureerimine.

[ADAL iOS-i](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL Androidi jaoks](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
