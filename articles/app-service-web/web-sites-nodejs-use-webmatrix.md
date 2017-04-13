<properties 
    pageTitle="Koostage ja Juurutage Node.js web Appi abil WebMatrix Azure" 
    description="Õpetus õpetab, kuidas kasutada WebMatrix Node.js rakenduste arendamise ja selle juurutama Azure'i rakenduse teenuse Web Apps." 
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


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Koostage ja Juurutage Node.js web Appi abil WebMatrix Azure

Selle õpetuse näidatakse, kuidas kasutada WebMatrix Node.js rakenduste arendamise ja selle juurutama [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. WebMatrix on tasuta web arengu tööriist Microsoftilt, mis sisaldab kõike, mida vajate veebisaidi või web app arengu. WebMatrix sisaldab mitmeid funktsioone, mida oleks lihtne kasutada Node.js, sh koodi lõpetamise, eelnevalt loodud mallide ja redaktori tugi Jade, vähem ja CoffeeScript. Lugege lisateavet [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Selle juhendi vormistamisel on teil Node.js web appi teenuses Azure rakendus töötab.
 
Allpool on täidetud taotluse kuvatõmmis.

![Azure sõlme veebisait][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="sign-into-azure"></a>Azure sisselogimine

Järgmiste juhiste abil luua veebirakenduse teenuses Azure rakendus.

1. Käivitage WebMatrix
2. Kui see on esimene kord kasutasite WebMatrix, palutakse teil Azure'i sisse logida.  Muul juhul saate klõpsake nuppu **Logi sisse** ja valige käsk **Lisa konto**.  Valige **logige sisse** oma Microsofti Account.

    ![Konto lisamine][addaccount]

3. Kui olete registreerunud Azure'i konto, logite sisse oma Microsofti Account:

    ![Azure sisselogimine][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Sisseehitatud malli abil Azure saidi loomine

1. Avakuvale ja klõpsake nuppu **Uus** ja valige galeriist malli uue saidi loomine **Saidimalli Galerii** .

    ![Uue saidi Mall galeriist][sitefromtemplate]

2. Dialoogiboksis **malli saidil,** valige **sõlm** ja seejärel valige **Saidi kiire**. Lõpuks klõpsake nuppu **edasi**. Kui teil on puudu mis tahes eeltingimuste **Kiire** saidimalli, palutakse teil installige need.

    ![valige Kiire Mall][webmatrix-templates]

3. Kui te Azure'i sisse logitud, nüüd on teil võimalus luua rakenduse teenuse veebirakenduse, kohaliku saidile.  Valige kordumatu nimi ja valige andmekeskusega, kus soovite luua oma rakenduse teenuse veebirakenduse. 

    ![Azure saidi loomine][nodesitefromtemplateazure]
    
4. Kui WebMatrix lõpule jõudnud, hoone kohaliku saidi ja rakenduse teenuse veebirakenduse loomine, kuvatakse WebMatrix IDE.

    ![WebMatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Azure'i rakenduse avaldamine

1. WebMatrix, **Home** lindi saidi **Avaldamine eelvaade** dialoogiboksi kuvamiseks nuppu **Avalda** .

    ![avaldamine eelvaade][webmatrix-node-publishpreview]

2. Klõpsake nuppu **edasi**. Kui avaldamine on lõpule jõudnud, kuvatakse rakenduse teenuse web appi URL-i WebMatrix IDE allosas

    ![avaldamine on lõpule viidud][webmatrix-publish-complete]

3. Klõpsake linki rakenduse teenuse web appi otse brauseris avada.

    ![Veebirakenduse Express][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Rakenduse uuesti avaldada ja muutmine

Saate muuta ja rakenduse uuesti avaldada. Siin teete lihtne muuta pealkirja failis **index.jade** ja rakenduse uuesti avaldada.

1. Valige **failid**WebMatrix, ja seejärel laiendage kausta **vaated** . **Index.jade** faili avamiseks topeltklõpsake seda.

    ![index.jade WebMatrix vaatamine][webmatrix-modify-index]

2. Lõigu joone muutmiseks järgmist:

        p Welcome to #{title} with WebMatrix on Azure!

3. Muudatuste salvestamiseks ja seejärel klõpsake ikooni avalda. Lõpuks klõpsake nuppu **Jätka** **Avaldamine eelvaade** dialoogiboksi ja oodake, kuni värskendus avaldada.

    ![avaldamine eelvaade][webmatrix-republish]

4. Kui avaldamine on lõpule viidud, kasutage tagastatud avalda protsess on lõppenud, värskendatud rakenduse teenuse web appi kuvamiseks linki.

    ![Azure sõlme web app][webmatrix-node-completed]

##<a name="next-steps"></a>Järgmised sammud

Node.js versioonides, mis on esitatud Azure ja kuidas määrata koos rakenduse versiooni kohta leiate lisateavet teemast [täpsustades Node.js versioon Azure rakenduses](../nodejs-specify-node-version-azure-apps.md).

Kui teil esineb probleeme rakenduse pärast seda kasutusele võetud Azure, vaadake, [Kuidas silumine Node.js veebirakenduse teenuses Azure rakenduse](web-sites-nodejs-debug.md) teavet diagnoosimise probleem.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 