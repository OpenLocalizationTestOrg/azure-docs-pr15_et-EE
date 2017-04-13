<properties
    pageTitle="Azure'i rakendust Service Node.js veebirakendustega alustamine"
    description="Saate teada, kuidas Node.js rakenduse web appi teenuses Azure rakenduse juurutamine."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Azure'i rakendust Service Node.js veebirakendustega alustamine

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Selle õpetuse näitab, kuidas luua lihtne [Node.js] rakendus ja selle juurutama [Azure'i rakendust Service] käsurea keskkonnast, nt cmd.exe või bash. Mis tahes operatsioonisüsteem, mida saab käitada Node.js saate järgida selles õpetuses juhiseid.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Eeltingimused

- [Node.js]
- [Bower]
- [Yeoman]
- [Git]
- [Azure'i CLI]
- Microsoft Azure'i konto. Kui teil pole kontot, saate [tasuta prooviversiooni kasutajaks] või [aktiveerida oma Visual Studio abonendi eelised].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Luua ja juurutada lihtne Node.js web app

1. Avage oma valik käsurea terminal ja [generaator Yeoman Expressi]installida.

        npm install -g generator-express

2. `CD`töötamine kataloogiga ja luua kiire rakendus, kasutades järgmist süntaksit:

        yo express
        
    Valige küsimisel järgmistest suvanditest:  

    `? Would you like to create a new directory for your project?`**Jah**  
    `? Enter directory name`**{rakendusenimi}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Ükski**  
    `? Select a database to use:`**Ükski**  
    `? Select a build tool to use:`**Grunt**

3. `CD`Uus rakendus root kataloogi ja käivitage see veendumaks, et see töötab teie arenduskeskkond:

        npm start

    Liikuge brauseris <http://localhost:3000> veenduge, et näete Express avalehel. Kui olete kontrollinud teie rakendus töötab õigesti, kasutage `Ctrl-C` seda peatada.
    
1. ASM režiimi muutmine ja Azure (peate [Azure'i CLI](#prereq)) sisse logida.

        azure config mode asm
        azure login

    Järgige küsimuse jätkamiseks login Microsofti kontoga, mis on Azure tellimuse brauseris.

2. Veenduge, et teil on endiselt juurkaust rakenduse ja seejärel luua rakenduse rakendust Service ressursi Azure kordumatu rakenduse nimega järgmise käsu abil. Näide: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Järgige viip valige Azure regioon, kus juurutada. Kui te olete kunagi häälestanud Git/FTP juurutamise identimisteabe Azure tellimuse, ka palutakse neid luua.

3. Rakenduse juurkaustas./config/config.js faili avada ja muuta tootmise port `process.env.port`; teie `production` atribuudi selle `config` objekti peaks välja nägema järgmine näide:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    See võimaldab Node.js rakenduse Web taotlusi vaikeport selle iisnode kuulab vastata.
    
4. Avage./package.json ja lisage soovitud `engines` atribuudi määramiseks [soovitud Node.js versiooni](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Muudatuste salvestamiseks ja seejärel oma rakenduse juurutamine Azure git abil.

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express generaator pakub juba .gitignore faili, nii et teie `git push` ei tarbimine läbilaskevõime üleslaadimisel on node_modules / kataloog.

5. Lõpuks käivitada reaalajas Azure rakenduse brauseris:

        azure site browse

    Nüüd näete oma Node.js veebirakenduse reaalajas teenuses Azure rakendus töötab.
    
    ![Näide sirvides juurutatud rakendusega.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Värskendage oma Node.js veebirakenduse

Värskenduste tegemiseks oma Node.js veebirakenduse töötavad rakenduse teenuse lihtsalt käivitage `git add`, `git commit`, ja `git push` nagu kui te esmalt juurutatud oma veebirakenduse.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Kuidas rakendus teenuse kasutajaks Node.js rakenduse

Azure'i rakendust Service kasutab [iisnode] Node.js rakenduste käivitamiseks. Azure'i CLI ja Kudu mootor (Git juurutus) koostööd teile sujuv kogemus, kui arendamise ja juurutamine Node.js rakenduste käsurea kaudu. 

- `azure site create --git`tuvastab levinud Node.js mustri server.js või app.js ja loob teie juurkaust on iisnode.yml. Saate kohandada iisnode seda faili.
- Veebisaidil `git push azure master`, Kudu automatiseerib juurutamise järgmisi toiminguid:

    - Kui package.json on hoidla root, käivitage `npm install --production`.
    - Luua Web.config, iisnode, mis viitab teie start skripti package.json (nt server.js või app.js) jaoks.
    - Kohandada Web.config rakenduse abil sõlm-inspektor silumine valmis.
    
## <a name="use-a-nodejs-framework"></a>Kasutage Node.js raames

Kui kasutate populaarsed Node.js raamistikku, nagu [Sails.js] [ SAILSJS] või [MEAN.js] [ MEANJS] arendada rakendused, saate need rakenduse teenusega juurutada. Populaarsed Node.js raamistiku on nende teatud quirks ja nende pakett sõltuvuste hoida saada värskendatud. Kuid rakenduse teenuse muudab stdout ja stderr logid teie jaoks saadaval nii, et saate toimuvaga täpselt oma rakendusega ja muutke vastavalt sellele. Lisateabe saamiseks lugege teemat [stdout ja stderr logid iisnode kaudu](#iisnodelog).

Järgmised õppetükid näitab teile, kuidas töötada kindlat framework rakenduse teenuses:

- [Azure'i rakendust Service juurutama Sails.js web app]
- [Rakendus Node.js vestlus Socket.IO teenuses Azure rakenduse loomine]
- [Kuidas kasutada io.js Azure rakenduse teenuse Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Teatud Node.js mootori kasutamiseks

Tüüpilised töövoog, saate määrata rakenduse teenust kasutada teatud Node.js mootor nagu tavaliselt package.json sisse.
Näiteks:

    "engines": {
        "node": "6.6.0"
    }, 

Kudu juurutamise engine määratleb Node.js mootori kasutada järgmises järjestuses:

- Esmalt vaadake iisnode.yml, et näha, kas `nodeProcessCommandLine` on määratud. Kui jah, siis kasutada.
- Järgmiseks pilk package.json, et näha, kas `"node": "..."` on määratud soovitud `engines` objekti. Kui jah, siis kasutada.
- Valige vaikimisi vaikimisi Node.js versioon.

>[AZURE.NOTE] Soovitatav on soovitud Node.js mootor konkreetselt määratleda. Saate muuta Node.js vaikeversiooniks ja võidakse kuvada tõrgete oma Azure web Appis Kuna Node.js vaikeversiooniks pole sobiv oma rakenduse.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Iisnode stdout ja stderr logid toomine

Iisnode logide lugemiseks toimige järgmiselt.

> [AZURE.NOTE] Pärast neid juhiseid, ei pruugi logifailid olemas kuni ilmneb tõrge.

1. Avage iisnode.yml fail, mille leiate Azure'i CLI.

2. Kaks järgmiste parameetrite seadmiseks tehke järgmist. 

        loggingEnabled: true
        logDirectory: iisnode
    
    Koos kindlaks rakenduse teenuses panna selle väljundi stdout ja stderror on D:\home\site\wwwroot iisnode\**iisnode** kataloogi.

3. Muudatuste salvestamiseks ja seejärel vajutage muudatuste Azure Git järgmised käsud:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode on nüüd konfigureeritud. Järgmised sammud näitavad, kuidas need logid juurdepääsu.
     
4. Brauseris juurde oma rakenduse, mis on Kudu silumine konsooli.

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Seda URL-i erineb web app URL-i lisamine "*.scm.*" DNS-i nimi. Kui jätate selle URL-i lisada, kuvatakse tõrge nr 404.

5. Liikuge D:\home\site\wwwroot\iisnode

    ![Liikumine iisnode logifailide asukohta.][iislog-kudu-console-find]

6. Klõpsake ikooni **redigeerimine** , Logi, mida soovite lugeda. Võite ka klõpsata **alla laadida** või **kustutada** kui soovite.

    ![Iisnode log faili avamiseks.][iislog-kudu-console-open]

    Nüüd näete, aitavad silumine juurutamise rakenduse teenuse Logi.
    
    ![Läbivaatamise iisnode faili.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Oma rakenduse sõlm-inspektor silumine

Kui kasutate sõlm-inspektor silumine Node.js rakendusi, saate oma rakenduse teenuse live rakenduse. Sõlm-inspektor on eelinstallitud iisnode installi rakenduse teenuse jaoks. Ja kaudu Git juurutamisel automaatselt loodud kaudu Kudu Web.config sisaldab juba kõik konfiguratsioon, tuleb teil lubada sõlm-inspektor.

Sõlm-inspektor lubamiseks tehke järgmist.

1. Avage oma hoidla juursaidil iisnode.yml ja määrake järgmiste parameetrite abil. 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Muudatuste salvestamiseks ja seejärel vajutage muudatuste Azure Git järgmised käsud:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Nüüd lihtsalt liikuge oma rakenduse käivitamine faili oma package.json start skripti määratletud/debug lisatud URL-i abil. Näiteks

        http://{appname}.azurewebsites.net/server.js/debug
    
    Või,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Veel ressursse

- [Azure'i rakendus, mis määrab Node.js versioon](../nodejs-specify-node-version-azure-apps.md)
- [Parimate tavade ja tõrkeotsingu Node.js rakenduste Azure juhend](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Kuidas Node.js veebirakenduse teenuses Azure rakenduse silumine](web-sites-nodejs-debug.md)
- [Azure'i rakendustega Node.js moodulid abil](../nodejs-use-node-modules-azure-apps.md)
- [Azure'i rakendust Service veebirakenduste: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js Arenduskeskus](/develop/nodejs/)
- [Teenuses Azure rakenduse web Appsi kasutamise alustamine](app-service-web-get-started.md)
- [Super salajase Kudu konsooli silumine uurimine]

<!-- URL List -->

[Azure'i CLI]: ../xplat-cli-install.md
[Azure'i rakendust Service]: ../app-service/app-service-value-prop-what-is.md
[Aktiveerige oma Visual Studio abonendi eelised]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Rakendus Node.js vestlus Socket.IO teenuses Azure rakenduse loomine]: ./web-sites-nodejs-chat-app-socketio.md
[Azure'i rakendust Service juurutama Sails.js web app]: ./app-service-web-nodejs-sails.md
[Super salajase Kudu konsooli silumine uurimine]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Kiire Yeoman generaator]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Kuidas kasutada io.js Azure rakenduse teenuse Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[tasuta prooviversiooni kasutajaks registreerumine]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
