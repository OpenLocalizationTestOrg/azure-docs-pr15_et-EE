<properties 
    pageTitle="Node.js rakenduse abil Socket.io | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada socket.io majutatud Azure'i node.js rakenduses." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Luua Node.js jututoa rakenduse abil Socket.IO on Azure pilveteenuses

Socket.IO pakub reaalajas suhtlemine node.js server ja klientide vahel. Selles õpetuses juhendab teid hosting on turvasoklite. IO vastavalt Azure rakendus vestlus. Socket.IO kohta leiate lisateavet teemast <http://socket.io/>.

Allpool on täidetud taotluse kuvatõmmis.

![Brauseriaknas kuvamise majutatud Azure'i teenus][completed-app]  

## <a name="prerequisites"></a>Eeltingimused

Veenduge, et edukalt lõpule näide selles artiklis on installitud järgmised tooted ja versioonid:

* [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) installimine
* Installige [Node.js](https://nodejs.org/download/)
* [Python 2.7.10 versiooni](https://www.python.org/) installimine

## <a name="create-a-cloud-service-project"></a>Teenus Cloud projekti loomine

Järgmiste juhiste loomine pilveteenuses teenuse project Socket.IO rakenduse majutavad.

1. **Menüü Start** või **Start Screen**, otsige **Windows PowerShelli**jaoks. Lõpetuseks, paremklõpsake **Windows PowerShelli** ja valige käsk **Käivita administraatorina**.

    ![Azure'i PowerShelli ikoon][powershell-menu]

2. Looge kaust nimega **c:\\sõlm**. 
 
        PS C:\> md node

3. Kataloogi muuta soovitud **c:\\sõlm** kataloog
 
        PS C:\> cd node

4. Sisestage uue nimega **chatapp** lahenduse ja töötaja roll nimega **WorkerRole1**loomiseks järgmised käsud:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Kuvatakse järgmise vastuse.

    ![Uue azureservice ning lisada azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Jututuba, nt allalaadimine

Selle projekti kasutame jututoa näide [Socket.IO GitHub hoidla]. Järgmiste toimingute näide alla laadida ja selle projekti varem loodud lisada.

1.  Luua hoidla kohaliku eksemplari, kasutades nuppu **klooni** . Samuti võite alla laadida projekti **ZIP** nupp.

    ![Brauseriaknas kuvamise https://github.com/LearnBoost/socket.io/tree/master/examples/chat, kus ZIP allalaadimise ikoon on esile tõstetud][chat-example-view]

3.  Liikuge directory kohaliku andmebaasi struktuuri, kuni jõuate soovitud **näited\\vestlus** kataloogi. Kopeerige see kataloogi sisu on **C:\\sõlm\\chatapp\\WorkerRole1** varasemas versioonis loodud kausta.

    ![Näiteid sisu kuvamise Exploreris\\jututoa directory ekstraktimist arhiivist;][chat-contents]

    Esiletõstetud üksuste pildil on kopeeritud failid on **näited\\vestlus** kataloog

4.  Klõpsake soovitud **C:\\sõlm\\chatapp\\WorkerRole1** kataloogi **server.js** faili kustutada, ja seejärel muutke **app.js** faili **server.js**. Selle toiminguga eemaldatakse varem loodud cmdlet-käsu **Lisa-AzureNodeWorkerRole** **server.js** vaikefail ja asendab see jututuba, nt rakenduse fail.

### <a name="modify-serverjs-and-install-modules"></a>Muutke Server.js ja installige moodulid

Enne testimisel Azure emulaator peame teatud vaheversioonid muudatused. Tehke faili server.js tehke järgmist.

1.  Avage fail **server.js** Visual Studio või mis tahes tekstiredaktorit.

2.  Otsige üles **mooduli sõltuvused** jaotis server.js alguses ja muuta rida, mis sisaldab **sio = require('.. //.. lib//socket.IO ")** et **sio = require('socket.io')** nagu allpool näidatud:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Rakenduse kuulab õige port tagamiseks avage server.js Notepadis või oma lemmik editor ja muutke järgmine rida, asendades **3000** **process.env.port** nagu allpool näidatud:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Pärast muudatuste salvestamiseks **server.js**, moodulid installimiseks järgige järgmisi juhiseid ja testige Azure emulaator rakendus.

1.  Kataloogi **Azure PowerShelli**abil muuta selle **C:\\sõlm\\chatapp\\WorkerRole1** directory ja kasutage järgmist käsk moodulid nõutud selle rakenduse installimiseks:

        PS C:\node\chatapp\WorkerRole1> npm install

    See installib package.json failis loetletud moodulid. Kui käsk on lõpule jõudnud, peaksite nägema väljund sarnaneb järgmisega:

    ![Funktsiooni npm väljund installimine käsk][The-output-of-the-npm-install-command]

4.  Kuna selles näites algselt osa Socket.IO GitHub hoidla ja Socket.IO teegi otseselt viidatud suhteline tee, Socket.IO pole viidatud failis package.json, nii, et saaksime peate selle installima väljastanud järgmine käsk:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testige ja juurutamine

1.  Käivitage emulaator väljastanud järgmine käsk:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Avage brauser ja liikuge **http://127.0.0.1**.

3.  Brauseriakna avamisel sisestage hüüdnimi ja seejärel vajutage enter.
    See on kõik, mida teatud hüüdnimi postitada. Mitme kasutaja funktsionaalsuse testimiseks avage täiendavate brauseriaknad sama URL-i ja sisestage erinevate nimede all.

    ![Kahe brauseriaknad User1 ja User2 vestluse sõnumite kuvamine](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Pärast testimisel, peatada emulaator väljastanud järgmine käsk:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Azure'i rakenduse juurutamine, kasutage **Avalda-AzureServiceProject** cmdlet-käsk. Näiteks:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Veenduge, et kasutada kordumatu nimi, muidu ei õnnestu avalda protsess. Kui juurutamise on lõpule jõudnud, brauseris avada ja liikuge juurutatud teenuse.
    > 
    > Kui saate tõrketeate selle kohta, et imporditud avalda profiilis pole esitatud tellimuse nime, saate alla laadida ja tellimuse enne juurutamist Azure avaldamise profiili importida. Vaadake jaotist **rakenduse Azure juurutamine** [luua ja juurutada Node.js rakendus on Azure pilveteenusesse](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Brauseriaknas kuvamise majutatud Azure'i teenus][completed-app]

    > [AZURE.NOTE] Kui saate tõrketeate selle kohta, et imporditud avalda profiilis pole esitatud tellimuse nime, saate alla laadida ja tellimuse enne juurutamist Azure avaldamise profiili importida. Vaadake jaotist **rakenduse Azure juurutamine** [luua ja juurutada Node.js rakendus on Azure pilveteenusesse](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Rakenduse töötab nüüd Azure ja saate relee vestluse sõnumid Socket.IO abil erinevate klientide vahel.

> [AZURE.NOTE] Lihtsa, see valim on piiratud jututoas ühendatud samas eksemplaris kasutajate vahel. See tähendab, et kui pilveteenusesse loob kahe töötaja rolli aknad, kasutaja saab ainult koos teistega sama töötaja rolli eksemplari ühendatud vestelda. Mastaapimiseks rakenduse töötamise rolli mitmes eksemplaris, võib nagu teenuse siini tehnoloogia abil ühiskasutusse anda Socket.IO laoseisu mitmes eksemplaris. Näiteid leiate [Azure'i SDK Node.js GitHub hoidla](https://github.com/WindowsAzure/azure-sdk-for-node)teenuse siini järjekorrad ja teemade kasutus näidised.

##<a name="next-steps"></a>Järgmised sammud

Selles õpetuses te teada, kuidas luua lihtsa vestlus rakenduse majutatud Azure'i pilveteenuses. Saate teada, kuidas see rakendus on Azure veebisaidi majutada, lugege teemat [koostamine on Node.js vestlus taotluse Socket.IO on Azure veebisaidil][chatwebsite].

Lisateabe saamiseks vt ka [Node.js Arenduskeskus](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub hoidla]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
