<properties 
    pageTitle="Web App Express (Node.js) | Microsoft Azure'i" 
    description="Õppeteema, põhineb pilvepõhise teenuse õpetuse, mis näitab, kuidas kasutada Express mooduli." 
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






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Teenuse Azure pilveteenuse Express kasutamine Node.js veebirakenduse koostamine

Node.js sisaldab funktsionaalsust vähima core pikkus.
Arendajad sageli kasutada 3 tootja moodulid pakuvad lisafunktsioone Node.js rakenduse väljatöötamisel. Selles õpetuses loote uue rakenduse [kiire][] moodul, mis pakub ning MVC raamistiku loomiseks Node.js veebirakenduste abil.

Allpool on täidetud taotluse kuvatõmmis.

![Veebibrauseris kuvamise Tere Expressi Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Teenus Cloud projekti loomine

Järgmiste toimingute luua uue pilvepõhise teenuse projekti nimega "expressapp":

1. **Menüü Start** või **Avakuvale**, otsige **Windows PowerShelli**jaoks. Lõpetuseks, paremklõpsake **Windows PowerShelli** ja valige käsk **Käivita administraatorina**.

    ![Azure'i PowerShelli ikoon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Kataloogi muuta soovitud **c:\\sõlm** directory ning sisestage järgmised käsud luua uue nimega **expressapp** lahenduse ja web rolli nimega **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Vaikimisi kasutab **Lisa-AzureNodeWebRole** Node.js varasem versioon. Ülaltoodud **Set-AzureServiceProjectRole** lause juhendab Azure'i v0.10.21 sõlme kasutada.  Pange tähele, et parameetrid on tõstutundlikud.  Saate kontrollida Node.js õige versioon on valitud, märkides ruudu **mootorite** vara **WebRole1\package.json**.

##<a name="install-express"></a>Installige Express

1. Installige Express generaator väljastanud järgmine käsk:

        PS C:\node\expressapp> npm install express-generator -g

    Npm käsu väljund peaks sarnanema allpool tulemi. 

    ![Väljund on npm kuvamine Windows PowerShelli installimine kiire käsk.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Muuta kataloogide **WebRole1** kataloogi ja kiire käsu abil saate luua uue rakenduse.

        PS C:\node\expressapp\WebRole1> express

    Teil palutakse kirjutada oma varasem. Sisestage **y** või **Jah** jätkata. Express loob app.js faili ja kausta struktuuri rakenduse jaoks.

    ![Kiire käsu väljund](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Täiendavad sõltuvused määratletud package.json faili installimiseks sisestage järgmine käsk:

        PS C:\node\expressapp\WebRole1> npm install

    ![Funktsiooni npm väljund installimine käsk](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Kopeerige fail **prügikasti/www** **server.js**järgmise käsu abil. See on nii pilveteenusesse leiate selle rakenduse sisenemiskoha.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Kui see käsk on lõpule jõudnud, peaks teil olema **server.js** faili WebRole1 kataloogis.

7.  Muuta **server.js** eemaldada üks soovitud "." järgmine rida märke.

        var app = require('../app');

    Pärast selle muudatuse tegemist rea peaks kuvatama järgmiselt.

        var app = require('./app');

    See muudatus on vaja Kuna me teisaldatud faili (varasema nimega **prügikasti/www-d**,) nõudmata rakenduse failina samas kaustas. Pärast selle muudatuse tegemist **server.js** faili salvestada.

8.  Käivitage rakendus Azure emulaator järgmise käsu abil:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Veebilehele, mis sisaldab Tere tulemast kiire.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Vaate muutmine

Nüüd saate muuta sõnumi "Tere tulemast et Kiire sisse Azure" kuvamiseks.

1.  Sisestage järgmine käsk index.jade faili avamiseks.

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Index.jade faili sisu.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade on vaikimisi vaade mootor Express rakendused. Otsimootori Jade vaate kohta leiate lisateavet teemast [http://jade-lang.com][].

2.  Muuta teksti viimase rea lõppu **Azure**.

    ![Faili index.jade Viimane rida loeb: Tere tulemast kasutama p \#Azure {tiitel}](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Salvestage fail ja väljuge Notepadist.

4.  Värskendage oma brauserit ja te näete soovitud muudatused.

    ![Brauseriaknas, leht sisaldab Tere tulemast Azure Express](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Pärast testimisel, emulaator peatamiseks **Peata-AzureEmulator** cmdlet-käsk kasutage.

##<a name="publishing-the-application-to-azure"></a>Azure'i rakenduse avaldamine

Azure'i PowerShelli aknas kasutada i cmdlet **Avalda-AzureServiceProject** pilveteenusesse rakenduse juurutamine

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Kui juurutamine toiming on lõpule jõudnud, avaneb brauseri ja veebileht kuvada.

![Express lehe kuvamiseks veebibrauseris. URL-i näitab, et nüüd on see majutatud Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Node.js Arenduskeskus](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-Lang.com]: http://jade-lang.com

 
