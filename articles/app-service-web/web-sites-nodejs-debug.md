<properties
    pageTitle="Kuidas Node.js veebirakenduse teenuses Azure rakenduse silumine"
    description="Saate teada, kuidas silumine Node.js veebirakenduse teenuses Azure rakendus."
    tags="azure-portal"
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

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Kuidas Node.js veebirakenduse teenuses Azure rakenduse silumine

Azure'i pakub sisseehitatud diagnostika aidata silumine [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714) veebirakendustes Node.js rakendused. Selles artiklis saate teada, kuidas logimise stdout ja stderr, tõrketeabe kuvamine brauseris ja kuidas alla laadida ja vaadata logifailid.

Diagnostika Node.js rakenduste majutatud Azure pakub [IISNode]. Kuigi selles artiklis käsitletakse kõige levinumad sätted diagnostika teabe kogumiseks, see ei ole täielik viide IISNode töötamiseks. IISNode töötamise kohta leiate lisateavet teemast [IISNode Readme] github.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Logimise lubamine

Vaikimisi sisaldab rakendust Service web app ainult diagnostikateabe juurutuste, näiteks kui juurutate web appi, kasutades Git. See teave on kasulik, kui teil on probleeme juurutamisel, näiteks ei saa viidata **package.json**, mooduli installimisel, või kui kasutate juurutamise kohandatud skript.

Stdout ja stderr logimise lubamiseks peate Node.js rakenduse juurtasemel **IISNode.yml** faili loomine ja lisamine järgmist:

    loggingEnabled: true

See võimaldab logimine stderr ja stdout Node.js rakenduse kaudu.

**IISNode.yml** faili saab kasutada kontrollida, kas sõbralik vead või vigu arendaja tagastatakse brauseri tõrke ilmnemisel. Arendaja tõrgete lubamiseks lisage järgmine rida **IISNode.yml** faili.

    devErrorsEnabled: true

Kui see suvand on lubatud, IISNode tagastab viimase 64K, teave saadetakse stderr asemel sõbralik tõrge, näiteks "ilmnes sisemine serveritõrge".

> [AZURE.NOTE] DevErrorsEnabled on kasulik, kui probleemide tuvastamisel arendamise käigus, võimaldaks tootmiskeskkonnas võib põhjustada saadetakse lõppkasutajad arengu tõrkeid.

Kui **IISNode.yml** faili ei pole oma rakenduses juba olemas, peate oma veebirakenduse pärast avaldamist värskendatud rakendus. Kui muudate sätteid **IISNode.yml** olemasoleva faili, mis on varem avaldatud lihtsalt, ei ole uuesti on nõutav.

> [AZURE.NOTE] Kui teie web Appis loodi Azure'i käsurea tööriistad või Azure PowerShelli cmdlet-käskude abil, luuakse automaatselt vaikimisi **IISNode.yml** faili.

Veebirakenduse taaskäivitamiseks valige web appi [Azure portaali](https://portal.azure.com)ja klõpsake **uuesti** nuppu.

![taaskäivitage nupp][restart-button]

Kui teie arenduskeskkond Azure'i käsurea tööriistad on installitud, abil saate taaskäivitage veebirakenduse järgmine käsk:

    azure site restart [sitename]

> [AZURE.NOTE] LoggingEnabled ja devErrorsEnabled on kõige sagedamini kasutatav IISNode.yml konfiguratsiooni suvandid hõivamine diagnostikateavet, saab konfigureerida erinevaid võimalusi majutusteenuse keskkonna IISNode.yml. Konfiguratsiooni suvandid täieliku loendi leiate teemast [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) faili.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Juurdepääs logid

Diagnostikalogide saab kasutada kolme võimalust; Abil soovitud File Transfer Protocol (FTP), alla Zip-arhiivist või nimega live värskendatud voo log (tuntud ka kui saba). Alla Zip arhiivi logifailide või vaatamine reaalajas voo jaoks on vaja Azure käsurea tööriistu. Need saab installida, kasutades järgmine käsk:

    npm install azure-cli -g

Kui installitud, pääseb menüü Tööriistad käsu "azure". Kõigepealt tuleb konfigureerida käsurea tööriistade kasutamine Azure tellimuse. Selle ülesande kohta leiate teemast soovitud **allalaadimise ja impordi avaldamine sätted** [kasutamine Azure käsurea tööriistade kohta](../xplat-cli-connect.md) artikli jaotises.

###<a name="ftp"></a>FTP

Diagnostikateabe FTP kaudu juurdepääsemiseks külastage [Azure portaali](https://portal.azure.com), valige oma veebirakenduse ja valige **ARMATUURLAUD**. Jaotises **Kiirlingid** **FTP DIAGNOSTIKALOGID** ja **FTPS DIAGNOSTIKALOGIDE** lingid pakuvad juurdepääsu logid FTP-protokolli abil.

> [AZURE.NOTE] Kui pole eelnevalt konfigureeritud kasutajanime ja parooli FTP või juurutus, saate seda teha **Kiirjuhend** haldamise lehe kaudu, valides **häälestamine juurutamise mandaat**.

Armatuurlaua tagastatud FTP URL on **LogFiles** kaust, mis sisaldab järgmist sub kataloogide.

* [Juurutamise meetod](web-sites-deploy.md) – kui kasutate juurutamise meetod, nt Git directory sama nime luuakse ja see sisaldab juurutuste seotud teavet.

* nodejs - Stdout ja stderr teabe jäädvustata läbivalt kogu rakenduse kaudu (kui loggingEnabled on täidetud.)

###<a name="zip-archive"></a>ZIP arhiiv

Laadige alla Zip-arhiivist diagnostika logid, kasutage järgmine käsk: Azure'i käsurea Tööriistad:

    azure site log download [sitename]

Laadige alla **diagnostics.zip** praegust kausta. See Arhiiv sisaldab kataloogi järgmine struktuur:

* juurutuste - juurutuste rakenduse kohta käiva teabe Logi

* LogFiles

    * [Juurutamise meetod](web-sites-deploy.md) – kui kasutate juurutamise meetod, nt Git directory sama nime luuakse ja see sisaldab juurutuste seotud teavet.

    * nodejs - Stdout ja stderr teabe jäädvustata läbivalt kogu rakenduse kaudu (kui loggingEnabled on täidetud.)

###<a name="live-stream-tail"></a>Reaalajas voo (saba)

Reaalajas voo diagnostikateave Logi kuvamiseks tehke Azure'i käsurea tööriistade järgmine käsk:

    azure site log tail [sitename]

Sündmuste logi on värskendatud serveris esinevad voo tagasi. Selle voo tagastavad juurutamine ning stdout ja stderr teavet (kui loggingEnabled on täidetud.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Järgmised sammud

Selles artiklis saate teada, kuidas lubada ja Azure diagnostika teabele juurde. Kuigi see teave on kasulik mõistmine probleemid, mis ilmnevad oma rakendusega, võib osutage mooduli kasutate või mille Node.js kasutatav brauserikiirklahv rakenduse teenuse versioon on kasutatud juurutamise keskkonna erinevas probleem.

Töötamine moodulid Azure Lisateavet [Abil Node.js moodulid Azure'i rakendustega](../nodejs-use-node-modules-azure-apps.md).

Rakenduse versiooni Node.js täpsustamiseks leiate teemast [täpsustades Node.js versioon Azure rakenduses].

Lisateabe saamiseks vt ka [Node.js Arenduskeskus](/develop/nodejs/).

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode seletusfail]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Azure'i rakendus, mis määrab Node.js versioon]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
