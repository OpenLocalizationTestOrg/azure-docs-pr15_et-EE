<properties 
    pageTitle="Kõneposti, VoIP ja SMS sõnumside Azure Twilio abil" 
    description="Saate teada, kuidas helistada ja Azure SMS-teenuse Twilio API sõnumi saatmine. Koodinäiteid Node.js kirjutatud." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Kõneposti, VoIP ja SMS sõnumside Azure Twilio abil

Sellest juhendist näitab, kuidas luua rakendusi, mida Twilio ja Azure node.js suhelda.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Mis on Twilio?

Twilio on API platvorm, mis on lihtne teha ja kõnede vastuvõtmine, saata ja saata tekstsõnumeid ja manustamine VoIP helistate sisse Brauseripõhine ja kohalikke mobiilirakenduste arendajatele.  Vaatame korraks minna üle, kuidas see toimib enne sukeldumise.

### <a name="receiving-calls-and-text-messages"></a>Kõnede ja tekstsõnumite

Twilio võimaldab arendajatel [osta programmeeritavat telefoninumbrite] [ purchase_phone] mida saab kasutada nii saata ja vastu võtta kõnede ja tekstsõnumite.  Kui arvu Twilio saab sissetuleva kõne- või teksti, Twilio saadab oma veebirakenduse HTTP POST või GET-päringu palub teil juhised selle kohta, kuidas hallata kõne või teksti.  Vastab teie serveri Twilio's HTTP-päring [TwiML][twiml], XML-sildid, mis sisaldavad juhised, kuidas hallata kõne või teksti lihtsate.  Näiteid TwiML näeme vaid hetk.

### <a name="making-calls-and-sending-text-messages"></a>Helistamist ja sõnumite saatmiseks

HTTP-päringud tegemisega Twilio veebi teenus API arendajate saata tekstsõnumeid või algatada väljaminevad kõned.  Väljaminevate kõnede, peate määrama arendaja URL, mis tagastab TwiML juhised, kuidas hallata Väljaminev kõne, kui seade on ühendatud.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Manustamine VoIP võimaluste UI code (JavaScripti, iOS või Android)

Twilio pakub kliendipoolne Tarkvaraarenduskomplektist, mis võib omakorda kõik töölaua veebibrauseris, iOS-i rakendus või Androidi telefoni VoIP.  Selles artiklis me keskenduda VoIP helistamiseks brauseris kasutamise kohta.  Lisaks Twilio JavaScripti Tarkvaraarenduskomplektist, mis töötavad brauseris, tuleb kasutada väljaandmisel märgiks"võimalus" JavaScripti kliendile serveripoolse rakenduse (meie node.js rakendus).  Lugege lisateavet kasutamise kohta VoIP node.js [Twilio arendaja ajaveebist][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Registreeru Twilio (Microsoft allahindlus)

Enne Twilio teenuste kasutamisel tuleb esimene [konto jaoks registreeruda][signup].  Microsoft Azure'i kliendid saavad soodushinnaga – [veenduge, et registreeruda siin][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Luua ja kasutusele võtta mõne node.js Azure'i veebisait

Järgmiseks peate töötavate Azure'i node.js veebisaidi loomiseks.  [Selle ametlik dokumentatsiooni asub siin][azure_new_site].  Kõrge, teete järgmist:

* Kui teil pole seda juba Azure'i konto kasutajaks
* Azure'i halduskonsoolis abil saate luua uue veebisaidi.
* Andmeallika juhtelement tuge (loodame, et kasutasite git)
* Faili loomine `server.js` lihtsa node.js veebirakendusega
* Selle lihtsa rakenduse Azure juurutamine

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Twilio mooduli konfigureerimine

Järgmiseks hakkavad kirjutamine lihtsa node.js rakendus, mis kasutab Twilio API-ga.  Enne alustamist, tuleb konfigureerida meie Twilio konto identimisteave.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Süsteemi keskkonna muutujate Twilio mandaadi konfigureerimine

Selleks, et autenditud taotlusi Twilio tagasi lõpuks vastu, läheb vaja meie konto SID ja auth luba, mis toimivad kasutajanimi ja parool, mida meie Twilio konto määramine. Turvaliselt konfigureerida järgmisi kasutamiseks sõlm mooduli Azure on süsteemi keskkonna muutujaid, mille saate määrata otse Azure administraatorikonsoolis kaudu.

Valige veebisaidi node.js ja klõpsake linki "KONFIGUREERIMINE".  Kui te kerige natuke, kuvatakse ala, kus saate määrata konfiguratsiooni atribuudid rakenduse.  Sisestage mandaat Twilio kontot ([teie Twilio armatuurlaual leitud][twilio_dashboard]) nagu on näidatud – veenduge, et nimi neid "TWILIO_ACCOUNT_SID" ja "TWILIO_AUTH_TOKEN" vastavalt:

![Azure'i halduskonsoolis][azure-admin-console]

Kui olete konfigureerinud nende muutujate, uuesti rakenduse Azure konsooli.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Deklareerimise Twilio moodul package.json

Järgmiseks tuleb luua package.json, meie sõlm mooduli sõltuvused [npm]kaudu hallata.  "Server.js" faili loodud Azure/node.js õpetuse samal tasemel looge fail nimega "package.json".  Selle faili sees paigutada järgmist:

  {"nimi": "rakenduse nimi", "versioon": "0.0.1", "Privaatne": tõene, "skriptide": {"start": "sõlm server"}, "sõltuvused": {"kiire": "3.1.0", "ejs": "*", "twilio": "*"}}

See kinnitab twilio mooduli sõltuvus, samuti populaarsed [kiire web framework] [ express] ja EJS malli mootor.  Okay, nüüd meil on kõik korras – Vaatame mõned koodi kirjutamine!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Väljamineva meili kaudu helistamine

Loome lihtsa vormi, mis paigutab valisime arvu kõne.  Avage server.js ja sisestage järgmine kood.  Võtke arvesse, kus on kirjas "CHANGE_ME" - Azure'i veebisaidi nime panna seal.

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Seejärel looge kaust nimega "vaated" – see kaust sees, looge fail nimega "index.ejs" sisuga järgmist:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Nüüd juurutada veebisaidi Azure ja avage oma Avaleht.  Mida peaks oskama tekstivälja sisestage oma telefoninumber ja vastu võtta teie Twilio arv kõne!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Sõnumi saatmine

Nüüd vaatame häälestada kasutajaliidese ja töötlemise loogika vormi saatke sõnum.  Avada "server.js" ja lisage järgmine kood pärast viimast kõne "app.post".

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

"Views/index.ejs" lisada mõne muu vormi esimene esitada arvu ja sõnumi teksti alusel:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Juurutage uuesti rakenduse Azure ja peaks nüüd olema võimalus esitada, mis moodustavad ja saatke sõnum enda (või mis tahes teie sõpradele)!

<a id="nextsteps"/>
## <a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud põhitõdesid node.js ja Twilio abil saate luua rakendusi, mis suhelda.  Kuid nendes näidetes vaevalt nullist, mis on võimalik Twilio ja node.js pind.  Twilio funktsiooniga node.js lisateabe saamiseks vaadake järgmistest allikatest:

* [Ametlik mooduli dokumendid][docs]
* [VoIP node.js rakendustega õpetus][voipnode]
* [Votr - reaalajas SMS hääletamine taotluse node.js ja CouchDB (kolm osa)][votr]
* [Sidumiseks programmeerimise node.js brauseris][pair]

Loodame, et teile meeldib häkkimine node.js ja Twilio Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[NPM]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



