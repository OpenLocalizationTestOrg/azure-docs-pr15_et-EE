<properties
    pageTitle="Minirakenduse juurutamiseks Azure'i rakendust Service"
    description="Saate teada, kuidas Azure'i rakendust Service rakenduse juurutamiseks."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Minirakenduse juurutamiseks Azure'i rakendust Service

Selles artiklis aitab teil otsustada, on parim valik juurutada failid oma veebirakenduse, mobiilirakenduse kirjutamata või API rakenduse [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714), ja seejärel juhendab teid asjakohased ressursid juhiseid teatud soovitud suvandit.

## <a name="overview"></a>Azure'i rakendust Service juurutamise ülevaade

Azure'i rakenduse teenus säilitab rakendus raames teile (ASP.net-i, PHP, Node.js jne). Mõned raamistiku on lubatud, samas kui teised, nt Java ja Python vaikimisi, võib tekkida vajadus lihtsa märge konfiguratsiooni lubamiseks. Lisaks saate kohandada oma rakenduse framework, nt PHP versioon või bitness oma Runtime. Lisateabe saamiseks vt [rakenduse Azure'i rakendust Service konfigureerimine](web-sites-configure.md).

Kuna te ei pea muretsema web serveri või rakenduse Frameworki kohta, rakenduse juurutamine rakenduse teenusega on juurutamine oma kood, kahendfaile, sisu failide ja nende vastavate kataloogi struktuuri, [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure (või **/saidi/wwwroot App_Data töö/** kataloogi jaoks WebJobs). Teenuse rakendus toetab juurutamise järgmistest suvanditest: 

- [FTP või FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Kasutage oma lemmik FTP või FTPS lubatud tööriist ja failide teisaldamine Azure, [FileZilla](https://filezilla-project.org) kõiki võimalusi pakkuva interaktiivset [NetBeans](https://netbeans.org)nagu soovite. See on täiesti faili üleslaadimise. Pole täiendavad osutavad teenuse rakendus, nt versiooni kontrollimine, struktuuri failihaldus jne. 

- [Kudu (Git/Mercurial või OneDrive'i ja Dropboxi)](https://github.com/projectkudu/kudu/wiki/Deployment): rakenduse teenuse [juurutamise otsimootori](https://github.com/projectkudu/kudu/wiki) kasutamine. Vajutage oma koodi Kudu otse mis tahes hoidla. Iga kord, kui kood on lükata, sh versiooni kontrollimine, pakett taastamine, MSBuild ja [web konksud](https://github.com/projectkudu/kudu/wiki/Web-hooks) pidev kasutuselevõtu-ja muude toimingute automatiseerimine pakub kudu lisatud teenused. Kudu juurutamise mootor toetab 3 erinevat tüüpi juurutamise allikast:   
    * Sisu sünkroonimine OneDrive'i ja Dropboxi kaudu   
    * Hoidla põhinev pidev juurutamise automaatse sünkroonimise GitHub, Bitbucket ja Visual Studio Team Services kaudu  
    * Hoidla-põhiste juurutus, nii et kohaliku git käsitsi sünkroonimine  

- [Web juurutada](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Deploy koodi rakenduse teenuse otse oma lemmik Microsofti tööriistade nagu Visual Studio abil sama mis instrumentaarium automatiseerib juurutamise IIS-i serverid. See tööriist toetab ainult erinevus juurutus, andmebaasi loomine, teisendusteta ühendusstringi jne. Web Deploy erineb Kudu Binaarfailid on ehitatud enne katsetama Azure. Sarnaselt FTP, pole täiendavad osutavad rakenduse teenus.

Web populaarse arendamise vahendid toetavad üks või mitu järgmiste protsesside juurutamine. Kui valite tööriista määratleb juurutamise protsesse, saate kasutada, sõltub tegelik DevOps funktsioonid teie käsutuses juurutamise protsessi ja valite kindla tööriistade kombinatsiooni. Näiteks kui teete Web juurutamine [Visual Studio Azure'i SDK](#vspros), ehkki automatiseerimine ei toomine Kudu, saate pakett taastamine ja MSBuild automatiseerimine Visual Studios. 

>[AZURE.NOTE] Need protsessid juurutamise ei tegelikult [Azure ressursside](../resource-group-template-deploy-portal.md) mis rakenduse võib vajalik olla. Siiski lingitud juhiseid artikleid kuvamine rakenduse ettevalmistamise ja oma koodi juurutama-lõpuni. Leiate ettevalmistamine Azure ressursse [automatiseerimine juurutamise käsurea tööriistade abil](#automate) jaotises Veel suvandeid.
     
## <a name="ftp"></a>Failide kopeerimine Azure'i käsitsi FTP juurutada
Kui kasutasite käsitsi kopeerimine veebisisu veebiserverisse, saate mõne [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) kasuliku faile, nagu Windows Exploreris või [FileZilla](https://filezilla-project.org/)kopeerimiseks.

Failide käsitsi kopeerimine spetsialistidele on:

- Tuttava ja minimaalsete keerukuse FTP instrumentaarium jaoks. 
- Teab täpselt, kus failide saabuvad.
- Turvalisuse FTPS abil.

Failide käsitsi kopeerimine miinuste on:

- On teada, kuidas juurutada failide õige kataloogide rakenduse teenus. 
- Pole versiooni tagasipööramine kui tõrkeid ilmneda juhtelementi.
- Sisseehitatud juurutamise ajalugu juurutamise seotud probleemide tõrkeotsinguks.
- Võimalikud pikk juurutamise korda, kuna paljud FTP tööriistad ei pakkuda ainult erinevus kopeerimine ja lihtsalt kopeerige kõik failid.  

### <a name="howtoftp"></a>Juurutamise failide käsitsi Azure kopeerimise abil
Failide kopeerimine Azure'i hõlmab toodud mõned lihtsad toimingud:

1. Eeldades, et teil on juba loodud juurutamise identimisteave FTP ühenduse teabe saamiseks **sätted** > **Atribuudid**ja seejärel kopeerige väärtused **FTP/juurutamise kasutaja**, **FTP Host Name**ja **FTPS hosti nimi**. Kopeerige **FTP/juurutamise kasutaja** kasutaja väärtuse kuvatud Azure portaali, sh tagamiseks proper kontekstis FTP-server rakenduse nime järgi.
2. Kasutage kliendilt FTP ühenduse teavet te kogutud ühenduse loomiseks oma rakenduse.
3. Failide ja nende vastavate kataloogi struktuuri kopeerimiseks [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure (või **/saidi/wwwroot App_Data töö/** kataloogi jaoks WebJobs).
4. Liikuge sirvides oma rakenduse URL, et rakendus töötab õigesti. 

Lisateabe saamiseks vaadake järgmisest allikast:

* [PHP MySQL web app luua ja juurutada FTP abil](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Juurutada cloud kausta sünkroonimine
Hea alternatiiv [failide käsitsi kopeerimine](#ftp) sünkroonitakse failide ja kaustade rakenduse teenuse salvestusruumi pilveteenuses nt OneDrive'i ja Dropboxi kaudu. Pilveteenuse kausta sünkroonimise kasutab Kudu protsessi juurutamiseks (vt [juurutamise protsesside ülevaade](#overview)).

Spetsialistidele cloud kausta sünkroonimise, on järgmised.

- Lihtsad juurutamise. Teenuseid nagu OneDrive'i ja Dropboxi pakkuda töölaua sünkroonimine klientidele, nii, et teie kohaliku töötavat kausta on ka juurutamise kataloogi.
- Ühe klõpsuga juurutus.
- Kudu juurutamise engine kõik funktsioonid on saadaval (nt pakett taastamine, automatiseerimine).

Miinuste cloud kausta sünkroonimise, on järgmised.

- Pole versiooni tagasipööramine kui tõrkeid ilmneda juhtelementi.
- Ilma automaatse juurutamise käsitsi sünkroonimine on nõutav.

### <a name="howtodropbox"></a>Kuidas juurutada cloud kausta sünkroonimine
[Azure portaali](https://portal.azure.com), saate määrata kausta sisu sünkroonimine oma OneDrive'i või Dropboxi salvestusruumi, rakenduse koodi ja selle kausta sisu töötamine ja rakenduse teenusega sünkroonimiseks klõpsake nupu.

* [Sisu sünkroonimine pilveteenuse kaust, kuhu Azure'i rakendust Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Pilvepõhine andmeallika juhtelemendi kataloogiteenusest pidevalt juurutamine
Kui teie arendusmeeskonnale kasutab pilvepõhist allikas koodi haldus (SCM) teenus nagu [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)või [BitBucket](https://bitbucket.org/), saate konfigureerida rakendus teenuse integreerida oma andmebaasi ja juurutada pidevalt. 

Spetsialistidele rakendades pilvepõhist andmeallika juhtelemendi teenusest on:

- Versiooni juhtelemendi lubamiseks tagasipööramine.
- Võimalus konfigureerida pidev juurutuse Git (ja Mercurial vajaduse korral) hoidlate. 
- Haru kohased juurutus, saate juurutada eri harude erinevaid [slots](web-sites-staged-publishing.md).
- Kõik funktsioonid Kudu juurutamise engine on saadaval (nt juurutamise Versioonimine, tagasipööramine, pakett taastamine, automatiseerimine).

CON rakendades pilvepõhist andmeallika juhtelemendi teenusest on:

- Mõned teadmisi vastava SCM teenuse nõutav.

###<a name="vsts"></a>Juurutamise pidevalt teenusest pilvepõhist andmeallika juhtelement
[Azure'i portaalis](https://portal.azure.com)saate konfigureerida pidev juurutamise GitHub, Bitbucket ja Visual Studio Team Services kaudu.

* [Pidev juurutamise Azure rakenduse teenusega](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Kohaliku git juurutamine
Kui teie arendusmeeskonnale kasutab asutusesisese kohaliku allika halduse (SCM) teenus, mis põhineb Git, saate konfigureerida selle rakenduse teenusega juurutamise allikana. 

Rakendades kohaliku git spetsialistidele on:

- Versiooni juhtelemendi lubamiseks tagasipööramine.
- Haru kohased juurutus, saate juurutada eri harude erinevaid [slots](web-sites-staged-publishing.md).
- Kudu juurutamise engine kõik funktsioonid on saadaval (nt juurutamise Versioonimine, tagasipööramine, pakett taastamine, automatiseerimine).

Rakendades kohaliku git CON on:

- Mõned teadmised vastav SCM süsteemi nõutav.
- Pidev juurutamiseks võtmed lahendusi. 

###<a name="vsts"></a>Juurutamise kohaliku git
[Azure'i portaalis](https://portal.azure.com)saate konfigureerida kohaliku Git juurutamise.

* [Kohaliku Git juurutamise Azure rakenduse teenusega](app-service-deploy-local-git.md). 
* [Mis tahes git /Hg Repo veebirakenduste avaldamine](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Juurutamine on IDE abil
Kui kasutate juba [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) ka [Azure SDK](https://azure.microsoft.com/downloads/)või muude IDE komplektide nagu [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org)ja [Huvitav idee](https://www.jetbrains.com/idea/), saate juurutada Azure otse oma IDE sees. See suvand on väga käepärane arendaja individuaalsed.

Visual Studio toetab kõiki kolme juurutamise protsesside (FTP, Git ja Web juurutada), sõltuvalt teie eelistus ajal muid interaktiivset saate võtta kasutusele rakenduse teenuse, kui neil FTP või Git integreerimine (vt [juurutamise protsesside ülevaade](#overview)).

Rakendades abil on IDE spetsialistidele on:

- Funktsiooni tööriistasid oma lõpuni rakenduste elutsükli potentsiaalselt minimeerimine Töötada, silumine, jälgida ja Juurutage Azure kõik liikumata väljaspool teie IDE rakendus. 

Rakendades abil on IDE miinuste on:

- Lisatud keerukuse instrumentaarium sisse.
- Nõuab veel mõne projekti jaoks andmeallika juhtelemendi süsteem.

<a name="vspros"></a>Täiendavad spetsialistidele, juurutamine Visual Studio abil Azure'i SDK on:

- Azure'i SDK muudab Azure ressursse esimese klassi kodanike Visual Studios. Loomine, kustutamine, redigeerimine, käivitamine, ja rakendused, päringu SQL-i tagaandmebaas, live silumine Azure rakendus ja palju muud. 
- Reaalajas Azure koodi failide redigeerimiseks.
- Reaalajas silumine rakenduste Azure.
- Integreeritud Azure explorer.
- Erinevus ainult juurutus. 

###<a name="vs"></a>Juurutamise otse Visual Studio

* [Azure'i ja ASP.net-i kasutamise alustamine](web-sites-dotnet-get-started.md). Kuidas luua ja juurutada ASP.net-i MVC web projekti Visual Studio ja Web juurutamine abil.
* [Kuidas juurutada Azure WebJobs Visual Studio abil](websites-dotnet-deploy-webjobs.md). Kuidas seadistada nii, et nad juurutamine nagu WebJobs konsooli rakendus projektid.  
* [Liikmelisuse, OAuthi, ja SQL-andmebaasi veebirakenduste turvaline ASP.net-i MVC 5 minirakenduse juurutamine](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Kuidas luua ja juurutada ASP.net-i MVC web projekti SQL-andmebaasi Visual Studio Web juurutada ja üksuse raamistiku koodi esimese migratsioon abil.
* [ASP.net-i Web juurutamine Visual Studio abil](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). 12 osa kuueosalisest sarjast, mis hõlmab täielikumat valikut juurutamise tööülesannete kui teised selles loendis. Mõned Azure juurutamise funktsioonid on lisatud, kuna õpetuse kirjutatud, kuid hiljem lisada märkmeid selgitatakse, mis on puudu.
* [ASP.net-i veebisaidile Azure'i Visual Studio 2012 Git hoidla otse juurutamine](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Selgitatakse, kuidas juurutada ASP.net-i web projektis Visual Studio abil lisandmooduli Git Git ja ühendusjooni Azure Git hoidla koodi kinnitamiseks. Visual Studio 2013 alustamist Git tugi on valmis ja ei nõua lisandmooduli installimine.

###<a name="aztk"></a>Juurutamise Eclipse ja huvitav mõte Azure tööriistakomplektid abil

Microsoft võimaldab juurutada veebirakenduste Azure'i otse Eclipse ja huvitav [Azure'i tööriistakomplekt Eclipse](../azure-toolkit-for-eclipse.md) ja [Azure tööriistakomplekt huvitav](../azure-toolkit-for-intellij.md)kaudu. Järgmised õpetused kirjeldavad toiminguid, mis on seotud juurutamine lihtne "Tere" maailma Web Appi abil kas IDE Azure.

*  [Tere tulemast maailma Web Appi Azure Eclipse loomine](./app-service-web-eclipse-create-hello-world-web-app.md). Selle õpetuse näidatakse, kuidas Azure'i tööriistakomplekt Eclipse abil luua ja juurutada Tere maailma veebirakenduse Azure.
*  [Tere tulemast maailma veebirakenduse Azure huvitav loomine](./app-service-web-intellij-create-hello-world-web-app.md). Selle õpetuse näidatakse, kuidas Azure'i tööriistakomplekt jaoks huvitav abil luua ja juurutada Tere maailma veebirakenduse Azure.


## <a name="automate"></a>Käsurea tööriistade abil automatiseerida juurutamine

* [Juurutus, nii et MSBuild automatiseerimine](#msbuild)
* [Failide kopeerimine FTP ja skriptide](#ftp)
* [Juurutus, nii et Windows PowerShelli automatiseerimine](#powershell)
* [Juurutus, nii et .net-i halduse API automatiseerimine](#api)
* [Azure'i juurutada käsurea liides (Azure'i CLI)](#cli)
* [Deploy käsureal Web juurutamine](#webdeploy)
* [FTP paketi skriptide abil](http://support.microsoft.com/kb/96269).
 
Mõne muu juurutamise võimaluseks on kasutada pilvepõhist teenust, näiteks [Octopus juurutamine](http://en.wikipedia.org/wiki/Octopus_Deploy). Lisateabe saamiseks vt [juurutamine ASP.net-i rakendused Azure veebisaitide](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Juurutus, nii et MSBuild automatiseerimine

Kui kasutate [Visual Studio IDE](#vs) arengu, saate [MSBuild](http://msbuildbook.com/) midagi, mida saate teha oma IDE automatiseerimiseks. Saate konfigureerida MSBuild failide kopeerimine [Web juurutamine](#webdeploy) või [FTP/FTPS](#ftp) abil. Web Deploy saate automatiseerida paljud muud juurutamise seotud ülesanded, andmebaaside juurutamist.

Vaadake käsurea juurutamise MSBuild kasutamise kohta leiate lisateavet järgmistest teemadest:

* [ASP.net-i Web juurutamine Visual Studio abil: käsurea juurutamise](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Kümnes sarja õppeteemad Azure Visual Studio abil juurutamise kohta. Näitab, kuidas kasutada käsurea juurutamiseks pärast häälestamise avaldamine profiilid Visual Studios.
* [Sees Microsoft Koosta Engine: MSBuild ja Team Foundation koostamine abil](http://msbuildbook.com/). Paberil aadressiraamat, mis sisaldab peatükke juurutamiseks MSBuild kasutamise kohta.

###<a name="powershell"></a>Juurutus, nii et Windows PowerShelli automatiseerimine

[Windows PowerShelli](http://msdn.microsoft.com/library/dd835506.aspx)kaudu saate teha MSBuild või FTP juurutamise funktsioonid. Kui te seda teha, saate ka Windows PowerShelli cmdlet-käsud, mis lihtsustamiseks haldamise Azure'i REST API kõne kogum.

Lisateavet järgmistest teemadest:

* [Lingitud GitHub hoidla web minirakenduse juurutamine](app-service-web-arm-from-github-provision.md)
* [Web Appi abil SQL-andmebaasi ettevalmistamine](app-service-web-arm-with-sql-database-provision.md)
* [Ettevalmistamine ja microservices ootuspäraselt sisse Azure juurutamine](app-service-deploy-complex-application-predictably.md)
* [Koosteüksuse tegelike Cloud rakenduste Azure - automatiseerida kõik](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-raamat Peatükk, mis selgitab, kuidas näidatud e-raamat valimi rakendus kasutab Windows PowerShelli skriptide keskkonnas Azure testi luua ja juurutada selle. Täiendavad Azure PowerShelli dokumentatsiooni jaotisest [ressursse](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) .
* [Windows PowerShelli skriptide avaldamiseks arendaja ja testi keskkonnas abil](../vs-azure-tools-publishing-using-powershell-scripts.md). Kuidas kasutada Windows PowerShelli skriptide juurutamise mis loob Visual Studio.

###<a name="api"></a>Juurutus, nii et .net-i halduse API automatiseerimine

Saate kirjutada C# koodi juurutamiseks MSBuild või FTP ülesannete täitmiseks. Kui te seda teha, pääsete Azure REST API saidi halduse funktsioonide haldamine.

Lisateabe saamiseks vaadake järgmisest allikast:

* [Kõik Azure'i haldamine teekide ja .NET automatiseerimiseks](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). .Net-i halduse API ja lingid rohkem dokumente tutvustus.

###<a name="cli"></a>Azure'i juurutada käsurea liides (Azure'i CLI)

Windowsi, Maci või Linuxi masinad käsurea abil saate juurutada FTP. Kui te seda teha, võite kasutada ka Azure ülejäänud halduse API Azure'i CLI abil.

Lisateabe saamiseks vaadake järgmisest allikast:

* [Azure'i käsk rea tööriistad](/downloads/#cmd-line-tools). Portaali lehe Azure.com käsurea tööriista teavet.

###<a name="webdeploy"></a>Deploy käsureal Web juurutamine

[Web juurutada](http://www.iis.net/downloads/microsoft/web-deploy) on Microsofti tarkvara, mitte üksnes nutikad faili sünkroonimine funktsioone, kuid ka saate teha või koordineerimine muude juurutamise seotud toiminguid ei saa automaatselt, kui kasutate FTP IIS-i juurutamiseks. Näiteks Web juurutada saate juurutada uue andmebaasi või andmebaasi värskendused koos oma veebirakenduse. Web Deploy saate vähendada ka aega, et värskendada olemasoleval saidil, kuna see võib arukalt kopeerida ainult muudetud failid. Microsoft Visual Studio ja Team Foundation Server on Web juurutada sisseehitatud tugi, kuid samuti saate Web juurutada otse käsurea kaudu juurutamise automatiseerimiseks. Web Deploy käsud on väga võimas, kuid õppimise kõverat võib olla järsku.

Lisateabe saamiseks vaadake järgmisest allikast:

* [Lihtsa veebirakenduste: juurutamise](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Ajaveebi, David Ebbo kirjutas hõlbustamiseks kasutada Web juurutamine tööriista kohta.
* [Juurutamise tööriist](http://technet.microsoft.com/library/dd568996). Ametlik dokumentatsiooni Microsoft TechNeti saidil. Aasta, kuid endiselt hea koht alustamiseks.
* [Kasutamine veebis juurutamine](http://www.iis.net/learn/publish/using-web-deploy). Ametlike dokumentide saidil Microsoft IIS.NET. Ka aasta, kuid hea koht alustamiseks.
* [ASP.net-i Web juurutamine Visual Studio abil: käsurea juurutamise](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild on kasutatud Visual Studio build mootor ja seda saab kasutada ka käsurea kaudu veebirakenduste veebirakenduste juurutamiseks. Selles õpetuses on osa sari, mis on peamiselt Visual Studio juurutamise kohta.

##<a name="nextsteps"></a>Järgmised sammud

Mõnel juhul võite saama edasi ja tagasi ja vaheldumisi hõlpsalt aktiveerida peatuspaikade tootmise rakenduse versiooni. Lisateavet leiate teemast [Web Apps kasutuselevõtt etapiviisilise](web-sites-staged-publishing.md).

Kohas, millel on varundus ja taaste plaan on oluline osa mis tahes juurutamise töövoog. Rakenduse teenuse kohta lisateavet varundamine ja taastamine funktsiooni leiate teemast [Web Appsi varukoopiad](web-sites-backup.md).  

Rakenduse teenuse juurutamise juurdepääsu haldamiseks Azure Rollipõhine juurdepääsu reguleerimine kasutamise kohta leiate lisateavet teemast [RBAC ja Web Appi Publishing](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
