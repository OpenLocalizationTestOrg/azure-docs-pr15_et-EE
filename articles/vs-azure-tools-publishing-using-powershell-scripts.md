<properties
   pageTitle="Windows PowerShelli skriptide abil saate avaldada arendaja ja testi keskkonnas | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Windows PowerShelli skriptide Visual Studio arengu avaldamine ja testida keskkonnas."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Windows PowerShelli skriptide abil arendaja avaldamine ja testida keskkonnas

Veebirakenduse loomisel rakenduses Visual Studio saate luua Windows PowerShelli skripti, mille abil saate hiljem avaldamise veebisaidi Azure Web App Azure'i rakendust Service või virtuaalse masina automatiseerimiseks. Saate redigeerida ja laiendamine Windows PowerShelli skripti Visual Studio redaktoris vastavalt oma vajadustele või skripti integreerida olemasolevate Koosta, testi ja skriptide avaldamine.

Nende skriptide saate säte kohandatud saidi ajutiselt kasutatavad versioonid (tuntud ka kui arendaja ja testige keskkondades). Näiteks võib häälestada oma veebisaidi Azure virtuaalne arvutis või vahekausta pesa veebisaidil, käivitage test komplekti, paljundada viga, testige vigu parandada, prooviversiooni pakutud muutmine või kohandatud keskkonna demo või esitluse häälestamine konkreetse versiooni. Pärast skripti, mis avaldab projekti loomist looge identse keskkonnas, käivitades uuesti skripti vastavalt vajadusele või käivitage skript oma kohandatud keskkonna testimiseks loomiseks oma veebirakenduse koostamine.

## <a name="what-you-need"></a>Mida on vaja

- Azure'i SDK 2.3 või uuem versioon. Lisateabe saamiseks vaadake [Visual Studio allalaadimine](http://go.microsoft.com/fwlink/?LinkID=624384) .

Teil pole vaja luua web projektide skripte Azure SDK. See funktsioon on web projektide, mitte web rollide pilveteenused.

- Azure'i PowerShelli 0.7.4 või uuem versioon. Lisateabe saamiseks vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md) .

- [Windows PowerShelli 3.0](http://go.microsoft.com/?linkid=9811175) või uuem versioon.

## <a name="additional-tools"></a>Täiendavad tööriistad

Täiendavad tööriistu ja ressursse Azure arengu PowerShelli Visual Studio töötamiseks on saadaval. Lugege teemat [PowerShelli Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Avalda skriptide loomise

Saate luua avalda skriptide virtual masinat, mis majutab oma veebisaidile, kui loote uue projekti järgmised [juhised](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md). Samuti saate [luua skriptide veebirakendustes avaldamine teenuses Azure rakenduse](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Skriptide, mis loob Visual Studio

Visual Studio loob lahendus taseme kaust nimega **PublishScripts** , mis sisaldab Windows PowerShelli kaks faili, avalda skripti virtuaalse masina või veebisaidi ja moodul, mis sisaldab funktsioone, mida saate kasutada skriptide. Visual Studio loob faili ka JSON-vormingus, mis määrab juurutate projekti üksikasjad.

### <a name="windows-powershell-publish-script"></a>Avaldamine Windows PowerShelli skripti

Avalda skripti sisaldab teatud avalda juhised veebisaidi või virtuaalse masina juurutamine. Visual Studio pakub süntaksi värvimine Windows PowerShelli arengu. Spikker funktsioonid on saadaval ja vabalt muudetavate funktsioonide skripti muutmisega vajadusele.

### <a name="windows-powershell-module"></a>Windows PowerShelli moodul

Windows PowerShelli moodul, mis loob Visual Studio sisaldab funktsioone, mis kasutab skripti avalda. Need on Azure PowerShelli funktsioonid ja on mõeldud muuta. Lisateabe saamiseks vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md) .

### <a name="json-configuration-file"></a>JSON konfiguratsioonifail

JSON-fail on loodud **konfiguratsioone** kausta ja see sisaldab konfiguratsiooni andmed, mis määrab täpselt ressursside juurutada Azure. Visual Studio loob faili nimi on projekti-nime-WAWS-dev.json kui olete loonud veebisaidi või projekti nime-VM-dev.json, kui olete loonud virtuaalse masina. Siin on näide JSON konfiguratsioonifail, mis luuakse siis veebisaidi loomine. Enamik väärtused on iseenesestmõistetavad. Selle nimi on loodud Azure, seega ei kattu oma projekti nime.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Kui loote virtuaalse masina, JSON konfiguratsioonifail näeb välja umbes järgmine. Pange tähele, et pilveteenus luuakse virtuaalse masina ümbris. Virtuaalse masina sisaldab tavaline lõpp-punktid web Accessi HTTP ja HTTPS-i kaudu, kui ka lõpp-punktide jaoks Web juurutada, mis võimaldab teil avaldamine veebisaidil oma kohalikus arvutis, kaugtöölaua ja Windows PowerShelli kaudu.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Saate redigeerida JSON konfiguratsiooni muutmiseks, mis juhtub, kui skripte avalda. Funktsiooni `cloudService` ja `virtualMachine` jaotised on nõutavad, kuid te saate kustutada selle `databases` jaotise, kui seda ei vaja. Valikulised atribuudid, mis on vaikimisi konfiguratsioonifailis Visual Studio loob tühja; need, mis on väärtused vaikimisi konfiguratsioonifailis on nõutav.

Kui teil on veebisaidi, mis sisaldab mitut juurutamise keskkonnas (tuntud slots) asemel ühe tootmiskoha Azure, saate kaasata pesa nimi nimi veebisaidi JSON konfiguratsioonifailis. Näiteks kui teil on veebileht, mis on nimega **Minu sait** ja pesa selle nimega **testimiseks** klõpsake URI on saidi minu sait-test.cloudapp.net, kuid õige nime kasutamiseks konfiguratsioonifailis on mysite(test). Saate teha ainult seda, kui veebisaidi ja slots juba olemas teie tellimus. Kui nad pole olemas, käitades skripti ilma pesa, veebisaidi loomine ja seejärel pesa [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)loomine ja seejärel käivitage skript muudetud selle nimi. Web Appsi juurutuse pesa kohta leiate lisateavet teemast [lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Kuidas avalda skriptide käitamiseks

Kui olete kunagi kasutanud enne Windows PowerShelli skripti, peate määrama esmalt täitmise poliitika skriptide käitamiseks. See on turbefunktsioon, et takistada kasutajatel opsüsteemi Windows PowerShelli skriptide, kui need on ründevara või viiruste, mis on seotud skriptide.

### <a name="run-the-script"></a>Käivitage skript

1. Saate luua oma projekti Web juurutada pakett. Web juurutada pakett on tihendatud arhiivi (zip-fail), mis sisaldavad faile, mida soovite kopeerida veebisaidile või virtuaalse masina. Saate luua pakettide Web juurutamine Visual Studios mis tahes veebirakenduse jaoks.

![Looge Web paketi juurutamine](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Lisateavet leiate teemast [kohta: Web juurutamise paketi loomine Visual Studios](https://msdn.microsoft.com/library/dd465323.aspx). Saate automatiseerida oma Web juurutada paketi loomine jaotises **määramine ja ulatub avalda skriptide** käesolevas teemas kirjeldatud.

1. **Solution Exploreris**Avage skripti kontekstimenüü ja valige **, kuidas avada PowerShell ISE**.

1. Kui olete käivitanud Windows PowerShelli skriptide selles arvutis esimest korda, avage käsuviiba aken administraatori õigustega ja tippige järgmine käsk:

`Set-ExecutionPolicy RemoteSigned`

1. Logige sisse Azure'i järgmise käsu abil.

`Add-AzureAccount`

Kui kuvatakse vastav viip, lisage oma kasutajanimi ja parool.

Pange tähele, et kui teil automatiseerimiseks skripti, seda meetodit Azure mandaatide ei tööta. Selle asemel kasutage .publishsettings faili mandaati. Üks kord ainult teie kasutage käsku **Get-AzurePublishSettingsFile** faili alla laadida Azure, ja seejärel kasutage **Impordi-AzurePublishSettingsFile** faili importimine. Üksikasjalikud juhised leiate teemast [Kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md).

1. (Valikuline) Kui soovite luua Azure ressursse, nt virtuaalse masina, andmebaasi ja veebisaidi ilma avaldada oma veebirakenduse, kasutage käsku **Avalda-WebApplication.ps1** on **-konfiguratsiooni** argumendi seatud JSON konfiguratsioonifail. See käsurea kasutab JSON konfiguratsioonifail ressursside loomine. Kuna seda kasutab vaikesätteid muude käsurea argumendid, loob ressursside, kuid ei avaldada oma veebirakenduse. – Verbose suvand annab teile rohkem teavet, mis toimub.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Kasutage käsku **Avalda-WebApplication.ps1** nagu on näidatud järgmistes näidetes autonoomsest skripti ja avaldada oma veebirakenduse. Kui vajate Alista vaikesätted, mis tahes muud argumendid, nt tellimuse nime, avaldada paketi nimi, virtuaalse masina identimisteabe või andmebaasi serveri identimisteavet, saate määrata parameetrid. Kasutage funktsiooni **– Verbose** suvand avaldamise protsessi edenemist kohta lisateabe saamiseks.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Kui loote virtuaalse masina, käsk näeb välja umbes järgmine. Selles näites mitme andmebaaside jaoks identimisteabe määramine. Nende skriptide loodud virtuaalmasinates jaoks SSL-sert ei ole usaldusväärse juurorgan. Seega peate kasutama **– AllowUntrusted** suvand.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Skripti saate luua andmebaase, kuid see ei looda andmebaasi serverid. Kui soovite luua andmebaasi server, saate funktsiooni **New-AzureSqlDatabaseServer** Azure mooduli.

## <a name="customizing-and-extending-the-publish-scripts"></a>Kohandamise ja skriptide avalda laiendamine

Saate kohandada avalda skripte ja JSON konfiguratsioonifail. Windows PowerShelli moodul **AzureWebAppPublishModule.psm1** funktsioonide pole mõeldud muuta. Kui soovite määrata ka erineva andmebaasi või muuta mõne virtuaalse masina atribuudid, JSON konfiguratsiooni faili redigeerimiseks. Kui soovite laiendavad skripti automatiseerimiseks koostamise ja testimine projekti, saate juurutada funktsioon ootavad **Avalda-WebApplication.ps1**.

Automatiseerimiseks koostamise projekti, lisada koodi, mis nõuab MSBuild, et `New-WebDeployPackage` nagu on näidatud järgmises näites kood. Käsk MSBuild tee erineb sõltuvalt olete installinud Visual Studio versiooni. Saada õige tee, saate kasutada funktsiooni **Get-MSBuildCmd**, nagu on näidatud järgmises näites.

### <a name="to-automate-building-your-project"></a>Projekti koostamise automatiseerimiseks

1. Lisage soovitud `$ProjectFile` parameeter jaotises globaalne param.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Kopeerige funktsiooni `Get-MSBuildCmd` teie skripti faili.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Asendage `New-WebDeployPackage` koos järgmine kood ja asendada kohatäidetes rea ehitamine `$msbuildCmd`. Järgmine kood on Visual Studio 2015. Kui kasutate Visual Studio 2013, muuta **VisualStudioVersion** vara all `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Kõne on `New-WebDeployPackage` funktsiooni enne selle rea: `$Config = Read-ConfigFile $Configuration` veebirakendustes või `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` virtuaalmasinates jaoks.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Kohandatud skript kasutamine läbimise käsureal autonoomsest soovitud `$Project` argument, nagu järgmises näites käsurea.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Automatiseerimiseks katsetamine rakenduse, lisada koodi `Test-WebApplication`. Kindlasti kommenteerige välja **Avalda-WebApplication.ps1** , kus need funktsioonid on nn read. Kui te ei esita rakendamist, saate käsitsi luua oma projekti Visual Studio ja seejärel käivitage avalda skript Azure avaldada.

## <a name="publishing-function-summary"></a>Avaldamise funktsiooni Kokkuvõte

Spikri kuvamiseks saate kasutada Windows PowerShelli käsuviibas funktsioone, kasutage käsku `Get-Help function-name`. Aidake sisaldab parameetri spikker ja näited. Sama abi tekst on ka skripti lähtefailid **AzureWebAppPublishModule.psm1** ja **Avaldamine – WebApplication.ps1**. Teie Visual Studio keeles on lokaliseeritud skripte ja abi.

**AzureWebAppPublishModule**

|Funktsiooni nimi|Kirjeldus|
|---|---|
|Lisage AzureSQLDatabase|Loob uue Azure SQL-i andmebaasi.|
|Lisage AzureSQLDatabases|Loob SQL Azure'i andmebaasid JSON configuation fail, mille Visual Studio genereeritud väärtused.|
|Lisage AzureVM|Loob Azure virtuaalse masina ja tagastab URL-i juurutatud VM. Funktsiooni häälestab eeltingimused ja seejärel kõned **New-AzureVM** funktsiooni (Azure'i moodul) luua uue virtuaalse masina.|
|Lisage AzureVMEndpoints|Lisab uue Sisestuskeel lõpp-punktid virtuaalse masina ja tagastab virtuaalse masina koos uue lõpp-punkti.|
|Lisage AzureVMStorage|Loob uue Azure storage konto praeguselt tellimuselt. Konto nimi algab väärtusega "devtest", millele järgneb kordumatu tähtedest ja numbritest koosnev string. Tagastab funktsioon uue salvestusruumi konto nimi. Määrake asukoht või mõne osaleja rühma salvestusruumi uus konto.|
|Lisage AzureWebsite|Loob veebisaidi määratud nimi ja asukoht. See funktsioon nõuab **New-AzureWebsite** Azure mooduli. Kui tellimus ei sisalda juba määratud nimega veebisait, see funktsioon loob veebisaidi ja tagastab veebisaidi objekti. Muul juhul annab vastuseks `$null`.|
|Varundus-tellimus|Salvestab praeguse Azure tellimuses soovitud `$Script:originalSubscription` muutuv skripti ulatuses. See funktsioon salvestab praeguse Azure tellimuse (mis on saadud `Get-AzureSubscription -Current`) ja selle konto salvestusruumi ja tellimus, mida on muudetud selle skripti (talletatud muutuja `$UserSpecifiedSubscription`) ja selle salvestusruumi konto skripti ulatus. Kui salvestate väärtused, saate kasutada funktsiooni, näiteks `Restore-Subscription`, algse praeguse tellimuse ja salvestusruumi konto taastamiseks praeguse oleku, kui praegune olek on muutunud.|
|Otsi-AzureVM|Saab määratud Azure virtuaalse masina.|
|DevTestMessageWithTime-vormingus|Kuupäeva ja kellaaja sõnumile prepends. See funktsioon on mõeldud sõnumite kirjutatud tõrge ja Verbose voogu.|
|Get-AzureSQLDatabaseConnectionString|Koondab ühendusstringi Azure'i SQL-andmebaasiga ühenduse loomiseks.|
|Get-AzureVMStorage|Tagastab määratud asukohta või osaleja rühmas (väiketähed) mustriga nimi "devtest*" esimese salvestusruumi konto nimi. Kui soovitud "devtest*" salvestusruumi konto ei vasta sellele, asukoha või osaleja rühma, funktsioon ignoreerib seda. Määrake asukoht või mõne osaleja rühma.|
|Get-MSDeployCmd|Tagastab käsu MsDeploy.exe tööriista.|
|Uue AzureVMEnvironment|Leiab või loob virtuaalse masina tellimus, mis vastab JSON konfiguratsioonifail väärtused.|
|Avaldamine WebPackage|Kasutab MsDeploy.exe ja veebi avaldamine pakett. Juurutamiseks ressursse veebisaidile ZIP-fail. See funktsioon ei luua mis tahes väljundi. Kõne suunamiseks MSDeploy.exe nurjumisel funktsiooni põhjustab erandi. Üksikasjalikumat väljundi saamiseks kasutage funktsiooni **-Paljusõnaline** suvand.|
|Avaldamine WebPackageToVM|Kinnitab parameetrite väärtused ja seejärel nõuab **Avalda-WebPackage** funktsioon.|
|Lugemis-ConfigFile|Konfiguratsioonifail JSON valideerib ja tagastab räsi valitud väärtuste tabeli.|
|Tellimuse-taastamine|Lähtestab praeguse tellimuse algse tellimuse.|
|Test-AzureModule|Annab vastuseks `$true` installitud Azure mooduli versioon on 0.7.4 või uuem versioon. Annab vastuseks `$false` kui mooduli pole installitud või varasemas versioonis. See funktsioon on parameetreid pole.|
|Test-AzureModuleVersion|Annab vastuseks `$true` Azure mooduli versioon on 0.7.4 või uuem versioon. Annab vastuseks `$false` kui mooduli pole installitud või varasemas versioonis. See funktsioon on parameetreid pole.|
|Test-HttpsUrl|Teisendab sisestatud URL-i System.Uri objekti. Annab vastuseks `$True` kui URL on absoluutne ja selle värviskeemi on https. Annab vastuseks `$false` , kui URL on suhteline ja selle kava ei ole HTTPS või Sisestuskeel stringi ei saa teisendada URL-i.|
|Liikme-test|Annab vastuseks `$true` kui atribuut või meetod on objekti liige. Muul juhul annab vastuseks `$false`.|
|Kirjutage-ErrorWithTime|Kirjutab praeguse kellaaja eesliide kuvatakse tõrketeade. See funktsioon nõuab **Vorming – DevTestMessageWithTime** funktsioon prepend aeg enne sõnumi kirjutamine tõrge voogesitamiseks.|
|Kirjutage-HostWithTime|Kirjutab sõnumi praeguse kellaaja eesliide host programm (**Kirjutamine-Host**). Mõju host programmi kirjutamine muutub. Enamik programme majutavate Windows PowerShelli kirjutada need sõnumid väljundit.|
|Kirjutage-VerboseWithTime|Kirjutab Paljusõnaline sõnumi eesliide praeguse kellaaja. Kuna see nõuab **Kirjutamine-Verbose**sõnumi kuvatakse ainult siis, kui skript töötab koos parameetriga **Verbose** või kui **VerbosePreference** eelistus seatakse **Jätka**.|

**Avaldamine WebApplication**

|Funktsiooni nimi|Kirjeldus|
|---|---|
|Uue AzureWebApplicationEnvironment|Loob Azure ressursse, nt veebisaidile või virtuaalse masina.|
|Uue WebDeployPackage|See funktsioon pole rakendatud. See funktsioon projekti koostamiseks saate lisada käske.|
|Avaldamine AzureWebApplication|Veebirakenduse Azure avaldab.|
|Avaldamine WebApplication|Loob ja kasutab veebirakenduste, virtuaalmasinates, SQL andmebaase ja Visual Studio web projekti salvestusruumi kontod.|
|Test-WebApplication|See funktsioon pole rakendatud. See funktsioon testimiseks rakenduse saate lisada käske.|

## <a name="next-steps"></a>Järgmised sammud

Lisateavet PowerShelli skriptimise [skriptimine Windows PowerShelli abil](https://technet.microsoft.com/library/bb978526.aspx) lugemise ja muude Azure PowerShelli skriptide [Skripti kaudu](https://azure.microsoft.com/documentation/scripts/)vaadata.
