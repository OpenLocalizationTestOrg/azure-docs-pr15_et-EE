<properties
    pageTitle="Määrab Node.js versioon"
    description="Saate teada, kuidas määrata Node.js Azure veebisaitide ja pilveteenustega kasutavad versiooni"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Azure'i rakendus, mis määrab Node.js versioon

Kui hosting Node.js rakenduse, mida soovite tagada, et teie rakendus kasutab teatud versiooni Node.js. On mitu võimalust selleks Azure rakendused.

##<a name="default-versions"></a>Vaikimisi versioonid

Node.js versioonid, mis on esitatud Azure värskendatakse pidevalt. Kui teisiti, mis on määratud vaikeversiooniks soovitud `WEBSITE_NODE_DEFAULT_VERSION` muutuja kasutatakse. See on vaikeväärtus alistamiseks järgige järgmistes jaotistes käesoleva artikli

> [AZURE.NOTE] Kui olete hosting rakenduse Azure'i pilveteenuses (veebis või töötaja roll) ja see on esimene kord, kui olete juurutanud rakendus, proovib kasutada Node.js sama versioon, kuna olete installinud oma arenduskeskkond kui see vastab vaikimisi versioon saadaval Azure Azure.

##<a name="versioning-with-packagejson"></a>Versioonimise abil package.json

Saate määrata Node.js kasutatakse järgmist lisamisega **package.json** faili versioon:

    "engines":{"node":version}

Kui *versioon* on teatud versiooninumber kasutada. Saate saate määrata keerukamaid tingimuste versioon, näiteks:

    "engines":{"node": "0.6.22 || 0.8.x"}

Kuna 0.6.22 ei ole saadaval majutusteenuse keskkond versioonid, 0,8, rea, mis on saadaval saab kõrgeim versiooni asemel kasutada - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Versioonimise veebisaitide rakenduse sätete abil
Kui olete rakenduse veebisaidi hosting, saate seada keskkonna muutuja **WEBSITE_NODE_DEFAULT_VERSION** soovitud versiooni. 

##<a name="versioning-cloud-services-with-powershell"></a>Versioonimise pilveteenustega PowerShelli abil

Kui hosting rakenduse pilveteenus ja on Azure PowerShelli kaudu rakenduse juurutamine, saate alistada Node.js vaikeversiooniks **Set-AzureServiceProjectRole** PowerShelli cmdleti abil. Näiteks:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Pange tähele, et parameetrid ülaltoodud lause on tõstutundlikud.  Saate kontrollida, märkides ruudu sisse oma rolli **package.json** **mootorite** atribuut on valitud Node.js õiget versiooni.

**Get-AzureServiceProjectRoleRuntime** abil saate alla laadida rakenduste majutatud nimega pilveteenus Node.js versioonide loendit.  Kontrollige Node.js projekti sõltub versioon on selles loendis.

##<a name="using-a-custom-version-with-azure-websites"></a>Azure'i veebisaitidega kohandatud versiooni abil

Azure'i on vaikimisi mitut versiooni Node.js, võite kasutada versiooni, mis ei esitata vaikimisi. Kui teie taotlus on majutatud nimega Azure'i veebisait, saate selle saavutamiseks **iisnode.yml** -faili abil. Järgmised toimingud tutvustavad Azure'i veebilehel kohandatud versiooni Node.Js kasutamise protsess on:

1. Looge uus kaust ja seejärel looge **server.js** faili kataloogi raames. **Server.js** fail sisaldab järgmist:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    See kuvab Node.js versioon, siis saate tutvuda.

2. Uue veebisaidi loomine ja selle saidi nimi. Näiteks järgmine kasutab [Azure käsurea tööriistade] uue nimega **mywebsite**Azure veebisaidi loomine ja seejärel lubada Git hoidla veebisaidi jaoks.

        azure site create mywebsite --git

3. Looge uus kaust nimega **prügikasti** kataloogi **server.js** faili sisaldavas laps.

4. Laadige alla teatud versiooni **node.exe** (Windowsi versioon), mida soovite oma rakendusega kasutada. Näiteks järgmine kasutab **curl** versioon 0.8.1 allalaadimine:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Salvestage fail **node.exe** varem loodud kausta **Prügikast** .

5. **Iisnode.yml** faili loomine samas kaustas **server.js** failina ja seejärel lisage **iisnode.yml** faili sisu on järgmine:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Tee on järgmine kuhu **node.exe** fail oma projekti paigutatakse pärast on avaldatud rakenduse Azure'i veebisaidile.

6. Rakenduse avaldada. Näiteks, kuna olen loonud uue veebisaidi koos parameetriga – git varasemas versioonis, järgmised käsud kuvatakse rakenduse failide lisamine minu kohaliku Git hoidla ja seejärel murravad veebisaidi hoidla:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Kui rakendus on avaldatud, avage veebisaidi brauseris. Peaksite nägema teadet, mis kinnitab "Tere tulemast Azure töötava sõlm versioonilt: v0.8.1".

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui soovite määrata Node.js teie rakendus kasutab versiooni, saate teada, kuidas [töötada moodulid], [luua ja juurutada Node.js veebisaidi]ja [Azure käsurea tööriistade Mac ja Linux kasutamise]kohta.

Lisateavet leiate teemast [Node.js Arenduskeskus](/develop/nodejs/).

[Kuidas kasutada Azure käsurea tööriistad Mac ja Linux]: xplat-cli-install.md
[Azure'i käsurea tööriistad]: xplat-cli-install.md
[töötamine moodulid]: nodejs-use-node-modules-azure-apps.md
[luua ja juurutada Node.js veebisait]: web-sites-nodejs-develop-deploy-mac.md
