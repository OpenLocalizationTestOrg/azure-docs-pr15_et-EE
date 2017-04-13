<properties
    pageTitle="Node.js alustamisjuhendit | Microsoft Azure'i"
    description="Saate teada, kuidas lihtsa Node.js veebirakenduse loomine ja selle juurutama on Azure pilveteenuses."
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Luua ja juurutada Node.js rakendus on Azure pilveteenusesse

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET-I](cloud-services-dotnet-get-started.md)

Selle õpetuse näitab, kuidas luua lihtne Node.js rakendus töötab Azure'i pilveteenuses. Pilveteenustega on koosteüksuste scalable cloud rakenduste Azure. Need võimaldavad eraldamine ja sõltumatu haldamiseks ja skaala-out ees- ja komponentide rakenduse.  Pilveteenustega ette robustne spetsiaalne virtuaalse masina majutusteenuse iga rolli usaldusväärselt.

Lisateavet pilveteenustega ja kuidas neid võrrelda Azure veebisaitide ja virtuaalmasinates, leiate [Azure'i veebisaitide, pilveteenustega ja Virtuaalmasinates võrdlus].

>[AZURE.TIP] Kas soovite luua lihtsa veebilehe? Kui teie stsenaariumi lihtsalt lihtsa veebilehe ees, kaaluge [kerge web appi]. Saate hõlpsasti uuendada pilveteenusesse oma veebirakenduse kasvab ja muuta oma nõuetele.

Selle õpetuse järgides koostate lihtsa veebirakenduse majutatud web roll. Saate rakenduse kohalik testimiseks kasutada Arvuta emulaator ja seejärel juurutada PowerShelli käsurea tööriistade abil.

Rakendus on lihtne "Tere, maailm" rakendus:

![Tere, maailm veebilehe kuvamine veebibrauseris][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Eeltingimused

> [AZURE.NOTE] Selle õpetuse kasutab, mis nõuab Windows Azure'i PowerShelli.

- Installima ja konfigureerima [Azure PowerShelli].
- Laadige alla ja installige [Azure'i SDK .NET 2,7]. Valige installi häälestamine.
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Luua projekti Azure'i pilveteenuses

Tehke uue Azure pilveteenuses projekti koos lihtsa Node.js tellingud loomiseks järgmist.

1. Käivitage **Windows PowerShelli** administraatorina; **Menüü Start** või **Avakuvale**, otsige **Windows PowerShelli**jaoks.

2. Tellimuse [PowerShelli ühendamine] .

3. Sisestage järgmine PowerShelli cmdlet-käsk loomiseks projekti loomiseks.

        New-AzureServiceProject helloworld

    ![Käsu New-AzureService helloworld tulem][The result of the New-AzureService helloworld command]

    Cmdlet-käsu **New-AzureServiceProject** loob põhistruktuuri avaldamise Node.js rakenduse pilveteenusesse. See sisaldab juhiks Azure failid. Cmdlet muudab töötamise kataloogi ka teenuse kataloogi.

    Cmdlet loob järgmised failid:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** ja **ServiceDefinition.csdef**: Azure'i seotud failide rakenduse avaldamiseks vajalike. Lisateabe saamiseks lugege teemat [Azure majutatud teenuse loomise ülevaade].

    -   **deploymentSettings.json**: talletatud kohalik sätteid, mis kasutavad juurutamise Azure PowerShelli cmdlet-käsud.

4.  Sisestage järgmine käsk Lisa uus web roll.

        Add-AzureNodeWebRole

    ![Käsu Lisa-AzureNodeWebRole väljund][The output of the Add-AzureNodeWebRole command]

    **Lisa-AzureNodeWebRole** cmdlet-käsk loob lihtsa Node.js rakendus. See muudab ka **.csfg** ja **.csdef** faile lisada uue rolli seadistusi.

    > [AZURE.NOTE] Kui te ei määra roll nimi, kasutatakse vaikenime. Nime saate sisestada esimese parameetrina cmdlet-käsk:`Add-AzureNodeWebRole MyRole`

Node.js rakendus on määratletud selle faili **server.js**, asub kataloogis web rolli (vaikimisi**WebRole1** ). Siin on kood:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Järgmine kood on põhiosas sama, mis "Tere, maailm" valimi [nodejs.org] veebisaidil, välja arvatud kasutab cloud keskkonnas antud pordi number.

## <a name="deploy-the-application-to-azure"></a>Azure'i rakenduse juurutamine

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Azure'i avaldamise sätete allalaadimine

Juurutada Azure'i rakendust, peate esmalt alla laadima avaldamise sätted Azure tellimuse.

1.  Käivitage järgmine Azure PowerShelli cmdlet-käsk:

        Get-AzurePublishSettingsFile

    Seda kasutatakse brauseri avalda sätted allalaadimise lehele liikumiseks. Logige sisse Microsofti Account võidakse teilt. Kui jah, kasutage oma Azure tellimusega seotud konto.

    Salvestage faili asukohta, saate hõlpsasti allalaaditud profiili.

2.  Sisestage järgmine cmdlet-käsk importimiseks allalaaditud avaldamise profiili:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Pärast importimist avalda sätted, kaaluge kustutamist .publishSettings allalaaditud faili, kuna see sisaldab teavet, mis võimaldaks kellegi juurdepääsu teie kontole.

### <a name="publish-the-application"></a>Rakenduse avaldamine

Avaldada, käivitage järgmine käsk:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-Teenuse nimi** määrab juurutamise jaoks. See peab olema kordumatu nimi, muidu ei õnnestu avalda protsess. Käsk **Get-kuupäeva** tihvtid kuupäeva/kellaaja string, mis peaksid olema kordumatu nimi.

- **-Asukohta** määrab andmekeskuses, mis on majutatud rakenduse. Saadaval andmekeskuste loendi kuvamiseks kasutage **Get-AzureLocation** cmdlet-käsk.

- **-Käivitada** brauseriaken ja viib majutatud teenuse kui juurutamise on lõpule jõudnud.

Kui avaldamine on loodud, kuvatakse järgmise sisuga vastuse.

![Käsu Avalda-AzureService väljund][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Võib kuluda rakenduse juurutamine ja need on saadaval, kui esimese published mitu minutit.

Kui juurutamise on lõppenud, brauseriaknas avamiseks ja liikuge pilveteenusesse.

![Brauseri aknas Tere tulemast maailma lehe; URL-i näitab lehe majutatakse Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Rakenduse töötab nüüd Azure.

**Avalda-AzureServiceProject** cmdlet-käsk teeb järgmist:

1.  Loob paketi juurutamiseks. Pakett sisaldab kõik failid kausta rakenduse.

2.  Loob uue **salvestusruumi konto** , kui üks olemas. Azure'i salvestusruumi kontot kasutatakse juurutamisel rakenduse pakett talletamiseks. Kui juurutamise on töö lõpetanud, saate salvestusruumi konto turvaline kustutada.

3.  Loob uue **pilveteenuses** , kui üks juba olemas. **Pilveteenuses** on rakenduse majutab Azure juurutamisel ümbris. Lisateabe saamiseks lugege teemat [Azure majutatud teenuse loomise ülevaade].

4.  Azure'i avaldab juurutamise pakett.


## <a name="stopping-and-deleting-your-application"></a>Peatamine ja rakenduse kustutamine

Pärast rakenduse juurutamine, võite välja lülitada nii, et saate vältida kulude eest. Azure'i arved veebis rolli aknad serveri aja tarbitud tunnis. Kui teie taotlus on juurutatud, isegi kui linnanimede ei tööta ja on peatatud olekus tarbib serveri aeg.

1.  Klõpsake aknas Windows PowerShelli abil järgmine cmdlet-käsk eelmises jaotises loodud teenuse juurutamise lõpetamiseks tehke järgmist.

        Stop-AzureService

    Teenuse peatamine võib kuluda mitu minutit. Kui teenus on peatatud, kuvatakse teade, et see on lõpetanud.

    ![Käsk Peata-AzureService olek][The status of the Stop-AzureService command]

2.  Teenuse kustutamiseks helistada järgmine cmdlet-käsk:

        Remove-AzureService

    Küsimise korral sisestage **Y** teenuse kustutamiseks.

    Teenuse kustutamine võib kuluda mitu minutit. Kui teenus on kustutatud kuvatakse teade, mis näitab, et teenus on kustutatud.

    ![Käsk Eemalda-AzureService olek][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Teenuse kustutamisel ei kustutata salvestusruumi konto, mis on loodud teenuse algselt avaldamisel ja teil jätkuvalt kasutada Storage toimub. Kui midagi on kasutusel talletamist, võite selle kustutada.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Node.js Arenduskeskus].

<!-- URL List -->

[Azure'i veebisaite, pilveteenustega ja Virtuaalmasinates võrdlus]: ../app-service-web/choose-web-site-cloud-service-vm.md
[kerge web Appi abil]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure'i PowerShelli]: ../powershell-install-configure.md
[Azure'i SDK .net-i 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[PowerShelli ühendamine]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Majutatud teenuse Azure loomise ülevaade]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js Arenduskeskus]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
