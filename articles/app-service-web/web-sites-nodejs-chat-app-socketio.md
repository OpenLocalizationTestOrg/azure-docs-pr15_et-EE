<properties
    pageTitle="Rakendus Node.js vestlus Socket.IO teenuses Azure rakenduse loomine"
    description="Õpetus, mis näitab node.js web Appis majutatud Azure'i socket.io abil."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Rakendus Node.js vestlus Socket.IO teenuses Azure rakenduse loomine

Socket.IO pakub reaalajas suhtlus node.js server ja WebSockets kasutavate klientide vahel. Samuti toetab Fall muude transport (nt pika küsitlused), mis vanemate brauserites töötada. Selle õpetuse sõelub majutusteenuse kui Azure web app rakendus Socket.IO vastavalt vestlus, ja näitavad, kuidas mastaapimiseks rakenduse abil [Azure'i Redis vahemälu]. Socket.IO kohta leiate lisateavet teemast <http://socket.io/>.

> [AZURE.NOTE] Selles ülesandes toiminguid rakendamine [Rakenduse teenuse veebirakenduste]; pilveteenustega, vt [koostamine on Node.js vestlus taotluse Socket.IO Azure pilveteenuse teenuses].

## <a name="download-the-chat-example"></a>Jututuba, nt allalaadimine

Selle projekti kasutame jututoa näide [Socket.IO GitHub hoidla]. Järgmiste toimingute näiteks alla laadida ja selle projekti varem loodud lisada.

1.  Laadige alla mõne [ZIP või GZ arhiivitakse väljaanne] Socket.IO projekti (versioon 1.3.5 kasutati selle dokumendi jaoks)

1.  Arhiivi ja koopia ekstraktida soovitud **näited\\vestlus** kataloogi uude asukohta. Näiteks ** \\sõlm\\vestlus**.

## <a name="modify-appjs-and-install-modules"></a>Muutke app.js ja installige moodulid

1.  Nimetage fail **index.js** **app.js**. See võimaldab Azure'i avastada, et see on Node.js rakendus.

1.  Avage tekstiredaktoris **app.js** fail. Muutke rea, mis sisaldab `var io = require('../..')(server);` nagu allpool näidatud:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Avage fail **package.json** ja lisada socket.io jaotises `dependencies`, nagu allpool näidatud:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Käsurea, muuta selle ** \\sõlm\\vestlus** directory ja kasutage npm moodulid nõutud selle rakenduse installimiseks:

        npm install

    See installib moodulid alamkausta nimega **node_modules**.

## <a name="create-an-azure-web-app"></a>Azure'i Web rakenduse loomine

Järgmiste juhiste abil luua Azure web app, Git avaldamise lubamine ja seejärel lubada WebSocket tugi web appi.

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure tasuta prooviversioon</a>.

1. Installige Azure'i käsurea liides (Azure'i CLI) ja Azure tellimuse ühenduse. Vt [installida ja konfigureerida Azure CLI](../xplat-cli-install.md).

1. Kui see on teie esimest korda häälestamise hoidla Azure, peate looma sisselogimisteave. : Azure'i CLI, sisestage järgmine käsk:

        azure site deployment user set [username] [password]

1. Muutke soovitud ** \\node\chat** directory ja järgmine käsk Uus Azure'i loomiseks kasutada web app ja kohaliku Git hoidla. See käsk loob ka on Git remote nimega "azure".

        azure site create mysitename --git

    'Mysitename' peate asendama veebirakenduse jaoks kordumatu nimi.

1. Kinnita olemasolevaid faile kohaliku hoidlasse abil järgmised käsud:

        git add .
        git commit -m "Initial commit"

1. Tõuketeatised failide Azure'i veebirakenduste hoidlasse järgmine käsk:

        git push azure master

    Küsimise korral sisestage oma kasutajanimi ja parool etappi 2. Saate olekuteade nagu moodulid imporditakse serveris. Kui see toiming on lõppenud, rakenduse majutatud teie Azure web Appis.

    > [AZURE.NOTE] Mooduli installimisel võib-olla märkate vigu mis "imporditud projekti... ei leitud". Need turvaline võib eirata.

1. Socket.IO kasutab WebSockets, mis on Azure vaikimisi pole lubatud. Kasutage web sockets lubamiseks järgmine käsk:

        azure site set -w

    Kui kuvatakse vastav viip, sisestage nimi veebirakenduse.

    >[AZURE.NOTE]
    >"Azure'i saidi määrata -w" käsk on ainult versiooniga 0.7.4 töö- või suurema Azure'i käsurea liides. Samuti saate lubada WebSocket tugi [Azure portaali](https://portal.azure.com).
    >
    >Azure'i portaalis WebSockets lubamiseks klõpsake veebirakenduse keelest veebirakenduste, käsku **Kõik sätted** > **rakenduse sätted**. Klõpsake jaotises **Web Sockets**, **klõpsake**nuppu. Klõpsake nuppu **Salvesta**.

1. Veebirakenduse Azure vaatamiseks kasutage käivitada teie veebibrauser ja liikuge majutatud veebirakenduse järgmine käsk:

        azure site browse

Rakenduse töötab nüüd Azure ja saate relee vestluse sõnumid Socket.IO abil erinevate klientide vahel.

## <a name="scale-out"></a>Välja skaala

Socket.IO rakendusi saate mastaabitud välja, kasutades __adapterit__ levitada sõnumeid ja sündmuste vahel mitu rakenduse eksemplari. Kui määratud on mitu adapterit, saab [socket.io-redis] adapterit hõlpsalt Azure'i Redis vahemälu funktsiooni abil.

> [AZURE.NOTE] Täiendavad vajadus skaleerimist Socket.IO lahendus on sticky seansid tugi. Sticky seansid on vaikimisi Azure Web Apps kaudu Azure'i taotlemine marsruutimist. Lisateavet leiate teemast [Eksemplari osaleja Azure veebisaitide].

### <a name="create-a-redis-cache"></a>Redis vahemälu luua

Toimingute [loomine Azure Redis vahemälu vahemälu] luua uue vahemälu.

> [AZURE.NOTE] __Host name__ ja salvestada __primaarvõtme__ jaoks vahemälu, nagu need on vaja järgmist.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Redis ja socket.io-redis moodulid lisamine

1. Mõne käsurea kaudu muuta selle __ \\sõlm\\vestlus__ directory ja kasutage järgmine käsk.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] See käsk määratud versioonid on kasutatakse katsetamiseks artiklit versioonid.

1. Muutke __app.js__ faili lisada järgmised read kohe pärast`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Asendage __redishostname__ ja __rediskey__ hosti nimi ja võti Redis vahemälu.

    See on avalda loomine ja tellida kliendi varem loodud Redis vahemällu. Klientide seejärel kasutatakse koos Socket.IO Redis vahemälu kasutamiseks mõeldud sõnumeid ja sündmuste vahel rakenduse eksemplari konfigureerimine

    > [AZURE.NOTE] Kuigi __socket.io-redis__ adapterit saavad suhelda otse Redis, praegune versioon ei toeta nõutav Azure Redis vahemälu autentimine. Nii algse ühenduse loomist __redis__ mooduli kasutamise ja seejärel kliendi edastatakse __socket.io-redis__ adapterit.
    >
    > Azure'i Redis vahemälu toetab pordi 6380 turvalist ühendust, moodulid selles näites kasutatakse ei toeta turvalist ühendust 7/14/2014. Ülaltoodud kood kasutab vaikimisi, 6379 ebaturvaliste porti.

1. Muudetud __app.js__ salvestamine

### <a name="commit-changes-and-redeploy"></a>Muutuste ja ümberkorraldamine

Käsurea rakenduses on __ \\sõlm\\vestlus__ directory, saate kasutada järgmisi käske muutuste ja ümberkorraldamine rakenduse.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Kui server on lükatud muudatusi, saate oma saidi üle mitmes eksemplaris skaala järgmise käsu abil.

    azure site scale instances --instances #

Kus __#__ on luua eksemplaride arv.

Saate oma veebirakenduse ühendada mitmest brauserites või arvutist kinnitamaks, et sõnumid saadetakse õigesti kõik kliendid.

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="connection-limits"></a>Ühenduse piirangud

Azure'i Web Apps on saadaval mitut SKU-de jaoks, mis määravad ressurssidest saidile. See hõlmab lubatud WebSocket ühenduste arv. Lisateabe saamiseks vaadake [Web Appsi hinnad lehe].

### <a name="messages-arent-being-sent-using-websockets"></a>Sõnumite pole saadetakse WebSockets abil

Kui kliendi brauserid säilitada langev tagasi pikk küsitlused WebSockets kasutamise asemel, võib olla üks järgmistest tõttu.

* **Proovige transport soovite lihtsalt WebSockets piiramine**

    Selleks kasutada WebSockets sõnumside transport Socket.IO, peab serveri ja kliendi toetama WebSockets. Kui ühe või teise ei ole, kuvatakse Socket.IO rääkida teise transport, nt pika küsitlusi. Transport kasutatavaid Socket.IO vaikeloendi on ` websocket, htmlfile, xhr-polling, jsonp-polling`. Saate jõustada neid kasutada ainult WebSockets, lisades järgmine kood **app.js** faili pärast rida, mis sisaldab `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Pange tähele, et ei saa vanemad brauserid, mis ei toeta WebSockets eespool kood on aktiivne, nagu see piirab side WebSockets ainult saidiga ühendust.

* **SSL-i kasutamine**

    WebSockets tugineb mõne väiksema kasutatud HTTP päised, nt **täiendamine** päis. Mõned vahe võrguseadmete, nt web puhverserverite, võivad need päiste eemaldamine. Selle probleemi vältimiseks saate luua ühenduse WebSocket SSL.

    Lihtne viis selleks on Socket.IO, et konfigureerida `match origin protocol`. See teeb Socket.IO turvamiseks sama, mis on pärit HTTP-või HTTPS taotluse veebilehe WebSockets teatis. Kui brauser kasutab HTTPS-i URL oma veebisaidile, kuvatakse järgmise WebSocket videoside Socket.IO turvatud SSL.

    Selles näites lubamiseks selle konfiguratsiooni muutmiseks lisada järgmine kood **app.js** faili pärast rida, mis sisaldab `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Kinnitage web.config sätted**

    Azure'i veebirakenduste majutada Node.js rakenduste abil sissetulevad taotlused marsruutida Node.js rakenduse **fail** . Funktsiooni õigesti Node.js rakendustega WebSockets, peavad sisaldama **web.config** järgmine kirje.

        <webSocket enabled="false"/>

    See keelab IIS-i WebSockets moodul, mis sisaldab oma WebSockets- ja vastuolus Node.js teatud WebSocket moodulid nagu Socket.IO rakendamine. Kui see rida puudub või on seatud `true`, võib põhjus, miks WebSocket transport ei tööta rakenduse.

    Tavaliselt ei sisalda Node.js rakenduste **web.config** faili, seega Azure veebisaitide automaatselt genereerida ühte Node.js rakenduste juurutamisel need. Kuna see fail luuakse automaatselt serveris, peate kasutama FTP- või veebisaidi URL FTPS faili vaatamiseks. Leiate FTP ja FTPS URL-ide klassikaline portaalis teie saidi jaoks, valides oma veebirakenduse ja seejärel linki **armatuurlaud** . URL-id, kuvatakse jaotises **kiire ülevaade** .

    > [AZURE.NOTE] **Fail** on loodud ainult Azure veebisaitide kui rakenduse ei paku. Kui annate **web.config** faili rakenduse projekti juurkaustas, seda kasutab Azure Web Apps.

    Kui kirje puudub või on seatud väärtusele `true`, siis peaks mõne **web.config** juurkaustas Node.js rakenduse loomine ja määrake väärtuseks `false`.  Viide, kuvatakse allpool on vaikimisi **web.config** rakendus, mis kasutab **app.js** sisenemiskoha.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Kui teie rakendus kasutab peale **app.js**sisendpunkti, peate asendama õige sisenemiskoha kõigi esinemiskordade **app.js** . Näiteks, asendades **app.js** **server.js**.

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus], kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis saate õppinud, kuidas majutatud Azure web app rakenduse jututoa loomine. Saate ka selle rakenduse Azure pilveteenuse teenust majutada. Kuidas see juhised leiate teemast [koostamine on Node.js vestlus taotluse Socket.IO Azure pilveteenuse teenuses].

Lisateabe saamiseks vt ka [Node.js Arenduskeskus].

## <a name="whats-changed"></a>Mis on muutunud

* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused].

<!-- URL List -->

[Azure'i Redis vahemälu]: /documentation/services/redis-cache/
[Rakenduse teenuse Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Veebilehe rakendused hinnad]: http://go.microsoft.com/fwlink/?LinkId=511643
[Luua Node.js jututoa rakenduse abil Socket.IO on Azure pilveteenuses]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure'i rakendust Service ja Azure olemasolevad teenused mõju]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js Arenduskeskus]: /develop/nodejs/
[Proovige rakenduse teenus]: http://go.microsoft.com/fwlink/?LinkId=523751
[Eksemplari osaleja Azure veebisaitide]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Vahemälu luua Azure'i Redis vahemälus]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO-redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub hoidla]: https://github.com/socketio/socket.io
[ZIP- või GZ arhiivitakse väljaanne]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
