<properties
    pageTitle="Kuidas installida ja konfigureerida Azure PowerShell"
    description="Siit saate teada, kuidas installida ja konfigureerida Azure PowerShelli."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Kuidas installida ja konfigureerida Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShelli</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Mis on Azure PowerShelli?
Azure'i PowerShelli on mooduleid, mis pakuvad Azure'i Windows PowerShelli cmdlet-käskude. Cmdlet-käskude abil saate luua, testida, juurutada ja hallata lahenduste ja teenustega Azure'i platvormi kaudu. Enamikul juhtudel saab kasutada samu toiminguid, mida Azure portaali, nt loomine ja konfigureerimine pilveteenustega, virtuaalmasinates, virtuaalne võrkude ja veebirakenduste cmdlet-käsud.

## <a name="how-versioning-works"></a>Versioonimine

Azure'i PowerShelli kasutab semantilise Versioonimine, mis tähendab, et kui versioon A > B versioon, siis on kõige ajakohasemat API-de versioon. Ka see tähendab, et muudatuste põhiversioon keskväärtus on serveris muuta ühe või mitme cmdlet-käsud.  Jah, näiteks versioon 1.7.0 on kiirparandus server muutmine Azure PowerShelli versioonides 1.x käsitlema.

Azure'i PowerShelli semantilise Versioonimine tavade kohta leiate lisateavet teemast semantilise Versioonimine määratlus veebisaidil: http://semver.org
 
Viimane API-de saamiseks kasutage versiooni 2.x. Kuid kui teil on kirjutatud skriptide vastu versioon 1.x ja te ei soovi võtta server muudatused versioon 2.x kirjeldatud 2.x [Väljalaskemärkmed](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), siis peaksite installima 1.7.0.

Versiooni lahknevuse võib põhjustada kui profiilis mooduli uusim versioon on installitud, ja seejärel laaditakse moodul, mis sõltub selle varasemas versioonis. Lihtsaim viis probleemi lahendamiseks installige uusim msi on. Funktsiooni MSI migreerimispaketiga automaatselt moodulid varasemaid versioone.
 
###<a name="installing-module-versions-side-by-side"></a>Installimise mooduli versioonid – kõrvuti

Versioon 2.1.0 (ja AzureStack versioon 1.2.6) esimese mooduli versiooni eesmärk olema installitud ja -kõrvuti kasutada. Kuna Azure PowerShelli kasutab kahendarvu moodulid, peate avamine uues aknas PowerShelli ja **Impordi moodul** abil importida AzureRM cmdlet-käsud konkreetse versiooni:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Versioonide enne 2.1.0 (v.a 1.2.6) ei tööta hästi kõrvuti Azure PowerShelli mooduli varasemate versioonidega. Varasema versiooni Azure PowerShelli moodulid, nagu on eespool käsu abil laadimisel mitteühilduvad versioonide **AzureRM.Profile** mooduli laaditakse, tulemuseks cmdlet-käskude palutakse sisse logida iga kord, kui käivitate cmdlet-käsu, isegi juhul, kui olete sisse loginud.

Lihtsaim viis lahendada see on installige uusim Azure PowerShelli soovitud WebPI feed või MSI-moodulid installitud galeriist varasemad versioonid eemaldatakse. 

Pange tähele, et Azure'i nii AzureRM moodulid sõltuvused ühist, et kui kasutate ühte värskendamisel mõlemad moodulid, peate värskendama mõlemad. Varasemates versioonides loodud Azure mooduli on kõrvuti mooduli varasemad versioonid AzureRM mooduli laadimine on sama küsimus.

<a id="Install"></a>
## <a name="step-1-install"></a>Samm 1: installimine

Järgnevalt on kaks võimalust, mille abil saate installida Azure PowerShelli. Saate installida selle PowerShelli Galerii või selle WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Azure'i PowerShelli galeriist PowerShelli installimine

Eelistatud viis on PowerShelli galeriis. Peate PowerShellGet mooduli PowerShelli galeriid kasutada. See on saadaval: [PowerShellGallery.com](https://www.powershellgallery.com/)

Installige Azure'i PowerShelli 1.3.0 või suurem galeriist PowerShelli abil administraatoriõigustes Windows PowerShelli või PowerShell integreeritud skriptimise keskkonnas (ISE) viip, kasutades järgmised käsud:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Lisateavet nende käsud

- **Installi-mooduli AzureRM** installib rollup mooduli Azure'i ressursihaldur cmdlet-käsud. Mooduli AzureRM sõltub 
- iga Azure'i ressursihaldur mooduli konkreetse versiooni vahemik. Kaasatud versioon vahemiku tagab, et pole server mooduli muudatused saab kaasata sama põhiversioon AzureRM moodulid installimisel. Kui installite AzureRM moodul, mis tahes Azure'i ressursihaldur moodul, mis pole varem installitud alla ja galeriist PowerShelli installitud. Azure'i PowerShelli moodulid kasutavad semantilise Versioonimine kohta leiate lisateavet teemast [semver.org](http://semver.org). 
- **Installi-moodul Azure'i** installib Azure mooduli. See moodul on Teenusehaldus: Azure'i PowerShelli mooduli 0.9.x. See ei tohiks oluliste muudatuste ja oleks võimalik eelmise versiooni Azure mooduli omavahel ära.

###<a name="installing-azure-powershell-from-webpi"></a>WebPI Azure PowerShelli installimine

Installida Azure PowerShelli 1,0 ja suuremad WebPI on sama, nagu see oli 0.9.x. [Azure'i PowerShelli](http://aka.ms/webpi-azps) alla laadida ja käivitage installimine. Kui teil on Azure PowerShelli 0.9.x installitud, versiooni täiendamise käigus desinstallitakse 0.9.x. Kui installisite Azure PowerShelli moodulid PowerShelli galeriist, eemaldatakse installer automaatselt moodulid enne installimist tagada ühtsete Azure PowerShelli keskkonnas.

> [AZURE.NOTE] Kui teil on varem installitud Azure moodulid galeriist PowerShelli, installer automaatselt eemaldada neid. See takistab segadust kohta, millised versioonid on installitud ja kus need asuvad. Tavaliselt installib PowerShelli Galerii moodulid **%ProgramFiles%\WindowsPowerShell\Modules**sisse. Seevastu WebPI installer installib *Azure moodulid *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Kui installimisel ilmneb tõrge, saate käsitsi eemaldamine on Azure* kaustu **%ProgramFiles%\WindowsPowerShell\Modules** kausta ja proovige uuesti installida.

Kui installimine on lõpule jõudnud, oma ```$env:PSModulePath``` säte peaks sisaldama kataloogid, mis sisaldavad Azure PowerShelli cmdlet-käsud.

> [AZURE.NOTE] PowerShelli teadaolev probleem on **$env: PSModulePath** , mis võivad tekkida WebPI installimisel. Kui teie arvuti nõuab taaskäivitamist süsteemi uuendused või teiste seadmete tõttu, võib see põhjustada värskenduste **$env: PSModulePath** tee, kuhu on installitud Azure PowerShelli ei kaasata. Sellisel juhul võidakse kuvada sõnumi "cmdlet ei tuvastatud", kui proovite pärast installimine või täiendamine Azure PowerShelli cmdlet-käskude kasutamine. Sel juhul tuleks seadme taaskäivitamine probleemi lahendada.

Kui teile kuvatakse teade umbes järgmist, kui proovite laadida või käivitada cmdlet-käsud:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

See saab korrigeerida arvuti taaskäivitada või cmdlet-käskude importimine C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ järgmiselt (kus XXXX on installitud PowerShelli versioon:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Samm 2: alustamine
Cmdlet-käskude käivitada standard Windows PowerShelli konsooli või kaudu PowerShell integreeritud skriptimise keskkonnas (ISE).
Meetodi abil saate avada ükskõik kumma konsooli sõltub teie arvutis töötab Windowsi versioon:

- Vähemalt operatsioonisüsteemis Windows 8 või Windows Server 2012, saate kasutada sisseehitatud otsing. Kuval **käivitamine** tippimine power. See tagastab jäävates rakenduste loend, mis sisaldab Windows PowerShelli. Konsooli avamiseks klõpsake kas app. (Kui soovite kinnitada **avakuvale** rakenduse, paremklõpsake ikooni.)

- Varem Windows 8 või Windows Server 2012 versiooniga arvutis, kasutage **menüü Start**. Klõpsake menüü **Start** käsku **Kõik programmid**, käsku **tarvikud**, klõpsake kausta **Windows PowerShelli** ja klõpsake **Windows PowerShelli**.

Samuti saate käivitada **Windows PowerShell ISE** teha paljusid samu toiminguid, mida soovite teha Windows PowerShelli konsooli käsud ja kiirklahvide abil. Kasutage ISE, Windows PowerShelli konsooli Cmd.exe väljale **Käivita** , tippige, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Käsud, mis aitavad teil alustada.

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Samm 3: ühenduse loomine
Cmdlet-käskude peate tellimuse nii, et nad saavad hallata oma teenuste. Kui teil pole veel üks, saate Azure tellimuse ostmiseks. Juhised leiate teemast [Azure ostmine](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Tippige **Logi sisse – AzureRmAccount**

2. Tippige meiliaadress ja parool, mis on teie kontoga seotud. Azure'i autendib ja salvestab mandaadi andmete ja seejärel akna sulgeda.

--VÕI--

Logige sisse oma töö või kooli kontoga.

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Kui teil on rohkem kui ühe rentnikuga seotud organisatsioonikonto, määrake TenantId parameeter.

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] See-Interaktiivne sisselogimine meetod toimib ainult töökoha või kooli kontoga. Töökoha või kooli kontoga on kasutaja, mis on hallata oma töö või kooli ja Azure Active Directory eksemplar oma töö või kooli jaoks määratletud. Kui teil on praegu töökoha või kooli kontoga ja kasutate Microsofti kontoga sisse logida Azure tellimuse, saate hõlpsalt luua järgmiste juhiste abil.

> 1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida ja klõpsake **Active Directory**.

> 2. Kui kataloogi, valige **kataloogi loomine** ja sisestage nõutav teave.

> 3. Valige kataloogi ja kasutaja lisamine. Uue kasutaja saab sisse logida töökoha või kooli kontoga. Kasutaja loomise ajal esitatakse nii e-posti aadressi kasutaja ja ajutist parooli abil. Salvestada see teave, kui seda kasutatakse juhis 5 allpool.

> 4. Azure'i klassikaline portaalis, klõpsake nuppu **sätted** ja seejärel valige **Administraatorid**. Valige **Lisa**ja lisage uus kasutaja koostöö administraatorina. See võimaldab hallata Azure tellimuse töökoha või kooli kontoga.

> 5. Lõpuks Logi Azure klassikaline portaali ja seejärel logige uuesti sisse, kasutades töö või koolikontoga. Kui see on esimest korda logimine selle kontoga, palutakse teil parooli muutmine.

> Microsoft Azure'i töökoha või kooli kontoga sisselogimisel kohta leiate lisateavet teemast [Microsoft Azure'i organisatsiooni kasutajaks](./active-directory/sign-up-organization.md).

> Azure autentimise ja tellimuste haldamise kohta lisateabe saamiseks vt [kontode haldamine, tellimused ja administraatorirollid](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Konto ja tellimuse üksikasjade kuvamine

Teil võib olla mitu kontot ja tellimuste Azure PowerShelli kasutamiseks saadaval. Saate lisada mitu kontot käivitades **Lisa-AzureRmAccount** rohkem kui üks kord.

Azure'i kontode kuvamiseks tippige **Get-AzureAccount**.

Azure'i tellimuste kuvamiseks tippige **Get-AzureRmSubscription**.

##<a id="Help"></a>Abi otsimine##

Järgmiste ressursside aitavad teatud cmdlet-käsud:


-   Kaudu jooksul konsooli, saate kasutada toote spikrisüsteemist. **Abi saamiseks** cmdlet-käsk võimaldab see süsteem. 

- Ühenduse abi, proovige need populaarsed Foorumid.

 - [MSDN-i Azure Foorum]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Lisateave


Leiate järgmistest teemadest leiate lisateavet cmdlet-käskude abil.

Windows PowerShelli kaudu lihtne juhised leiate teemast [Windows PowerShelli kasutamine](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Viide cmdlet-käskude kohta leiate teemast [Azure cmdleti viide](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Näidisskriptid ja juhised aitavad teil saate teada, kuidas hallata Azure skriptimise abil leiate teemast [Skripti keskele](http://go.microsoft.com/fwlink/p/?LinkId=321940).

