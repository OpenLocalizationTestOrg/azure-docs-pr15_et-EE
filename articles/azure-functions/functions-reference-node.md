<properties
    pageTitle="Azure'i funktsioonide NodeJS tootearendusmaterjal | Microsoft Azure'i"
    description="Mõista, kuidas töötada Azure'i funktsioonide abil NodeJS."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i funktsioone, funktsioonide, event töötlus, webhooks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure'i funktsioonide NodeJS tootearendusmaterjal

> [AZURE.SELECTOR]
- [C# skript](../articles/azure-functions/functions-reference-csharp.md)
- [F # skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Azure'i funktsioonide sõlm/JavaScript kogemus on lihtne eksportida funktsioon, mis on mõne `context` objekti runtime suhtlemiseks ja vastuvõtmise ja andmete sidumiste kaudu.

Selles artiklis eeldatakse, et loetud [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md).

## <a name="exporting-a-function"></a>Funktsiooni eksportimine

Kõik JavaScripti funktsioonid tuleb eksportida ühe `function` kaudu `module.exports` Runtime otsida funktsiooni ja käivitage see. See funktsioon peab alati lisada mõne `context` objekti.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Sidumiste, `direction === "in"` edastatakse mööda, tähendab, saate kasutada funktsiooni argumentidena [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) käsitlema dünaamiliselt uue sisendina (nt abil `arguments.length` kordamiseks üle kõik teie sisendina). See funktsioon on väga mugav, kui teil pole täiendavad sisendina koos käivitamiseks, kui pääsete ootuspäraselt päästik andmete ilma viitamine oma `context` objekti.

Argumendid on alati edasi mööda funktsiooni järjestuses esinevad *function.json*, isegi siis, kui te ei määra neid ekspordi-lauses. Kui teil on näiteks `function(context, a, b)` ja muuta seda, et `function(context, a)`, saate siiski väärtus `b` funktsioon koodi viidates `arguments[3]`.

Kõigi sidumiste, olenemata suunas, on ka edasi mööda soovitud `context` objekti (vt allpool). 

## <a name="context-object"></a>konteksti objekt

Runtime kasutab mõnda `context` objekti andmete edastamiseks oma funktsioonist ja võimaldavad teil suhelda runtime.

Konteksti objekt on alati funktsiooni esimene parameeter ning alati tuleks lisada kuna see on meetodid, näiteks `context.done` ning `context.log` mis runtime õigesti kasutamiseks on vajalik. Saate nimi objekti, mida soovite (st `ctx` või `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>Context.Bindings

Funktsiooni `context.bindings` objekti kogub kõik teie sisend- ja andmed. Andmed lisatakse peale selle `context.bindings` objekti kaudu soovitud `name` atribuudi siduvad. Näiteks järgmine sidumine määratlus toodud *function.json*, pääsete juurde järjekorra kaudu sisu `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

Funktsiooni `context.done` funktsioon runtime ütleb, et olete valmis käivitatud. See on oluline, helistada, kui olete valmis funktsiooniga; kui te ei, veel kunagi runtime teada, teie funktsioon lõpule viidud. 

Funktsiooni `context.done` funktsiooni abil saate edastada tagasi kasutaja määratletud tõrge runtime, samuti atribuudi kotti atribuute, mis kirjutab atribuutide kohta on `context.bindings` objekti.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>Context.log(Message)

Funktsiooni `context.log` meetod, mis võimaldab väljund log laused, mis on seotud koos logimine eesmärgil. Kui kasutate `console.log`, oma sõnumid kuvatakse ainult protsessi taseme logimiseks, mis ei ole kasulik.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

Funktsiooni `context.log` meetod toetab sama parameetri-vormingus, mida toetab sõlm [util.format meetod](https://nodejs.org/api/util.html#util_util_format_format) . Jah, näiteks koodi umbes järgmine:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

saate kirjutada umbes järgmine:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP päästikute: context.req ja context.res

Puhul HTTP päästikute, kuna see on levinud mustri kasutamiseks `req` ja `res` HTTP koosolekukutsete ja kutsele vastamise objektide otsustasime hõlpsalt juurde pääsemiseks need objektil kontekstis asemel saate kasutada täielikku sunnib `context.bindings.name` mustri.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Sõlm versioon ja pakettide

Sõlm versioon on praegu lukus veebisaidil `5.9.1`. Me uurib lisamise tugi veel versioonid ja muutes selle konfigureerida.

Saate lisada pakettide oma funktsiooni *package.json* faili üleslaadimisel oma funktsioon kausta funktsioon rakenduse failisüsteemis. Faili üleslaadimine juhiseid, [Azure'i funktsioonide arendaja viide teema](functions-reference.md#fileupdate)jaotisest **funktsioon rakenduse failide värskendamine** . 

Saate kasutada ka `npm install` sisse funktsioon rakenduse SCM (Kudu) käsurea liides:

1. Liikuge: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klõpsake **konsooli silumine > CMD**.

3. Liikuge `D:\home\site\wwwroot\<function_name>`.

4. Käivitage `npm install`.

Kui teil on vaja paketid on installitud, saate neid importida oma funktsioon tavalisel viisil (st kaudu `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Keskkonna muutujad

Mõne keskkonnas kui ka rakenduse väärtuse seadmine saamiseks kasutage `process.env`, nagu on näidatud järgmises näites kood:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>TypeScript/CoffeeScript tugi

Pole veel otsest tugi automaatne-koostamise TypeScript/CoffeeScript kaudu käitusajal nii, et kõik oleks vaja välised käitusajal juurutamise ajal. 

## <a name="next-steps"></a>Järgmised sammud

Lisateavet järgmistest teemadest:

* [Azure'i funktsioonide tootearendusmaterjal](functions-reference.md)
* [Azure'i funktsioonide C# tootearendusmaterjal](functions-reference-csharp.md)
* [Azure'i funktsioonide F # tootearendusmaterjal](functions-reference-fsharp.md)
* [Azure'i funktsioonide päästikute ja seosed](functions-triggers-bindings.md)
