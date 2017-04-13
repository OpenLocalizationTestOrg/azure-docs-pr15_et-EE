<properties 
    pageTitle="Kuidas kasutada io.js Azure rakenduse teenuse Web Apps" 
    description="Saate teada, kuidas teenuses Azure rakenduse web appi kasutamine io.js." 
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
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Kuidas kasutada io.js Azure rakenduse teenuse Web Apps

Populaarsed sõlm kahvel [io.js] funktsioonid erinevate erinevused Joyent's Node.js projekti, sh juhtimismudeli veel avatud, kiirem release tsükli ja uued ja katse JavaScripti funktsioonid kiiremini vastu võtta.

[Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps on palju Node.js versioone eelinstallitud, samuti võimaldab kasutaja esitatud Node.js kahendarvuks. Selles artiklis käsitletakse io.js rakenduse teenuse Web Apps kasutamise kaks võimalust: laiendatud juurutamise skripti, mis konfigureerib automaatselt Azure'i kasutamiseks saadaval io.js uusim versioon, kui ka käsitsi, on io.js kahendarvu üles kasutamine. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Juurutamise skripti abil

Pärast juurutuse Node.js rakenduse rakenduse teenuse veebirakenduste käivitatakse arv väike käsud tagamaks, et keskkond on õigesti konfigureeritud. Juurutamise skripti kasutamisel saab kohandada selle protsessi allalaadimine ja io.js konfigureerimine.

[Io.js juurutamise Script](https://github.com/felixrieseberg/iojs-azure) on saadaval github. Teie veebirakendus io.js lubamiseks lihtsalt kopeerida **.deployment**, **deploy.cmd** ja **IISNode.yml** oma rakenduse juurkausta ja võtta kasutusele Web Apps.  

Esimese faili **.deployment**, juhendab veebirakenduste käivitamiseks **deploy.cmd** juurutamise korral. See skript tavaline juhised Node.js rakendus töötab, kuid laadib alla ka io.js uusim versioon. Lõpuks **IISNode.yml** konfigureerib veebirakenduste kasutada lihtsalt allalaaditud io.js kahendarvu asemel eelinstallitud Node.js kahendarvuks.

> [AZURE.NOTE] Kasutatud io.js kahendarvuks värskendama, vaid ümberkorraldamine rakenduse - skripti alla io.js uus versioon iga kord rakendus on juurutatud.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Käsitsi installimine abil

Käsitsi installimine kohandatud io.js versioon sisaldab ainult kaks toimingut. Esmalt Laadige alla **win-x64** kahendarvu otse [io.js jaotuse]. Nõutav on kaks faili - **iojs.exe** ja **iojs.lib**. Mõlemad failid salvestada kausta, mis teie web app, näiteks **prügikasti/iojs**sees.

Veebirakenduste **iojs.exe** eelinstallitud sõlm versiooni asemel kasutada konfigureerimiseks rakenduse juurtasemel **IISNode.yml** faili loomine ja lisage järgmine rida.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Järgmised sammud

Sellest artiklist saate teada, kuidas kasutada io.js rakenduse teenuse Web Apps abil nii esitatud juurutamise skripte kui ka käsitsi installimine. 

> [AZURE.NOTE] IO.js on raske areng ja värskendatud sagedamini Node.js. Arvu Node.js moodulid ei pruugi töötada io.js - palun konsulteerige [io.js github] tõrkeotsingu jaoks.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

[IO.js]: https://iojs.org
[IO.js jaotuse.]: https://iojs.org/dist/
[IO.js github]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 