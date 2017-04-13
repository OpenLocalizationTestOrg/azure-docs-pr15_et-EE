<properties
    pageTitle="MySQL-i ressursikeskuse pakkuja Azure'i virnas juurutamine | Microsoft Azure'i"
    description="Üksikasjalikud juhised juurutada MySQL-i ressursikeskuse pakkuja Azure'i virnas."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>MySQL-i ressursikeskuse pakkuja Azure'i virnas veebirakenduste kasutamiseks klõpsake juurutamine

> [AZURE.NOTE] Järgmine teave kehtib ainult Azure virnas TP1 juurutuste.

Selles artiklis jälgimiseks MySQL-i ressursikeskuse pakkuja häälestamise üksikasjalikud juhised Azure'i virnas tõendada mõistet (POC) nii, et saate hakata [MySQL-i andmebaaside](azure-stack-mysql-rp-deploy-short.md) kohta Azure virnas, sh selle kirjutamata MySQL-i kasutamine WordPress saitide jaoks loodud [Azure Web](azure-stack-webapps-deploy.md)apps kasutamine.

## <a name="set-up-steps-before-you-deploy"></a>Enne juurutamist juhiseid häälestamine

Enne juurutamist ressursi pakkuja, peate:

- Windows Server vaikepildi koos .NET 3.5 on
- Internet Explorer (IE) täiustatud turvalisus väljalülitamine
- Installige uusim versioon Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Windows Server, sh 3,5 .NET pildi loomine

Kui laadisite Azure'i virnas bittide pärast 2/23/2016 Windows Server 2012 R2 base vaikepilt sisaldab .NET 3.5 framework selle alla laadida ja selle uuemates versioonides saate selle sammu vahele jätta.

Kui laadisite enne 2/23/2016, peate looma Windows Server 2012 R2 andmekeskuse VHD .NET 3.5 pildiga ja on platvormi pilt hoidlas pilti.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Lülita IE täiustatud turvalisus ja luba küpsised

Ressursi pakkuja juurutamiseks käivitate teenuse PowerShell integreeritud skriptimise keskkonnas (ISE) administraatorina, nii, et peate esmalt lubama küpsiste ja JavaScripti Internet Exploreri profiili abil saate sisse logida Azure Active Directory nii administraator ja kasutajale sisselogimist.

**IE välja lülitada tõhustatud turvalisus:**

1. Azure'i virnas tõendada mõiste (PoC) arvutisse AzureStack/administraatorina sisse logida ja avage serveri haldur.

2. **IE täiustatud turvalisus konfiguratsiooni** välja lülitada nii administraatoritele ja kasutajatele.

3. **ClientVM.AzureStack.local** virtual arvutisse administraatorina sisse logida ja avage serveri haldur.

4. **IE täiustatud turvalisus konfiguratsiooni** välja lülitada nii administraatoritele ja kasutajatele.

**Küpsiste lubamine**

1. Windowsi avakuvale, klõpsake käsku **Kõik rakendused**, valige **Windowsi tarvikud**, paremklõpsake **Internet Exploreri**, osutage käsule **veel**ja valige **Käivita administraatorina**.

2. Kui kuvatakse vastav viip, kontrollige **soovitatav kasutada turvalisus**ja seejärel klõpsake nuppu **OK**.

3. Internet Exploreris valige **Tööriistad (hammasrattaikoon)** &gt; **Interneti-suvandid** &gt; vahekaarti **Privaatsus** .

4. Klõpsake nuppu **Täpsemalt**, veenduge, et mõlemad nupud **Aktsepteeri** valitud, klõpsake nuppu **OK**ja seejärel uuesti nuppu **OK** .

5. Sulgege Internet Explorer ja taaskäivitage PowerShell ISE administraatorina.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Installige Azure'i virnas ühilduvad väljaanne Azure PowerShell

1. Desinstallige kõik olemasolevad Azure PowerShelli oma kliendi VM.

2. Logige sisse Azure'i virnas POC masina AzureStack/administraatorina.

3. Kaugtöölaua kasutamisel sisselogimiseks **ClientVM.AzureStack.local** virtual arvutisse administraatorina.

4. Avage juhtpaneel, klõpsake käsku **Desinstalli programm** &gt; klõpsake **Azure PowerShelli** &gt; klõpsake käsku **desinstalli**.

5. [Uusim Azure PowerShelli, mis toetab Azure virnas alla laadida](http://aka.ms/azstackpsh) ja installida.

    PowerShelli installimist käivitada kontrolli PowerShelli skripti, veenduge, et saate luua ühenduse oma Azure'i virnas eksemplari (peaks kuvatama veebilehele Logi sisse).

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Ressursi pakkuja juurutamise PowerShelli bootstrap

1. Azure'i virnas POC kaugtöölaua ühenduse clientVm.AzureStack.Local ja logige sisse azurestack\\azurestackuser.

2. [MySQL-i RP kahendfaile alla laadida](http://aka.ms/masmysqlrp) faili ja eraldada selle D:\\MySQLRP.

3. D: käivitada\\MySQLRP\\Bootstrap.cmd faili administraatorina (azurestack\administrator).

    Selle tegemisel avaneb Bootstrap.ps1 faili PowerShell ISE.

4. Kui windows PowerShell ISE laadimise lõpule jõudnud, klõpsake nuppu "Start" või vajutage klahvi F5.

    Kaks põhilist vahekaarti laaditakse iga sisaldavad skripte ja failide pakkuja MySQL-i ressursikeskuse juurutamiseks.

## <a name="prepare-prerequisites"></a>Ettevalmistused eeltingimused

Klõpsake vahekaarti **Ettevalmistamine eeltingimused** :

- Nõutav sertide loomine
- MySQL-i kahendfaile allalaadimiseks oma Azure'i virnas
- Azure'i virnas salvestusruumi kontot esemeid üleslaadimine
- Galerii üksuste avaldamine

### <a name="create-the-required-certificates"></a>Nõutav sertide loomine
See **New-SslCert.ps1** skript lisab selle \_. D: AzureStack.local.pfx SSL-i serdi\\MySQLRP\\eeltingimused\\BlobStorage\\Container kausta. Serdi tagab suhtlemine ressursi pakkuja ja Azure ressursihaldur kohaliku eksemplari.

1. **Eeltingimused ettevalmistamine** põhi vahekaardil **New-SslCert.ps1** vahekaarti ja käivitage see.

2. Tippige küsimus, mis kuvatakse, PFX parool, mida kaitseb privaatvõti ja **märkige see parool**. Peate hiljem.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>MySQL-i kahendfaile allalaadimiseks oma Azure'i virnas

1. Valige vahekaart **Allalaadimine-MySqlServer.ps1** ja käivitage see.
2. Vastava viiba kuvamisel nuppu **Jah** kinnitage dialoogiboksi EULA aktsepteerimiseks.

    See käsk liidab kaks zip-failide D:\MySql\Prerequisites\BlobStorage\Container kausta.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Kõik esemeid seotud salvestusruumi konto Azure virnas üles laadida

1. Klõpsake vahekaarti **Üles-Microsoft.MySql-RP.ps1** ja käivitage see.

2. Tippige dialoogiboksi Windows PowerShelli mandaati taotluse Azure'i virnas teenuse administraatori identimisteave.

3. Azure Active Directory rentniku ID küsimise korral sisestage oma Azure Active Directory rentniku täielik domeeninimi: näiteks microsoftazurestack.onmicrosoft.com.

    Mandaadi küsib hüpikakent.

    > [AZURE.TIP] Kui vahekaardid ei kuvata, saate kas pole sisse lülitatud IE tõhustatud turvalisus lubada JavaScripti selle arvuti ja kasutaja või te ei ole vastu IE küpsised. Vaadake [juhiseid enne juurutamist häälestamine](#set-up-steps-before-you-deploy).

4. Sisestage mandaat Azure'i virnas teenuse administraator ja seejärel klõpsake nuppu **Logi sisse**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Avaldada Galerii üksusi hiljem ressursside loomine

Valige menüü **Avalda-GalleryPackages.ps1** ja käivitage see. See skript liidab kaks turuplatsi üksuste Azure'i virnas POC portaali turuplats, mille abil saate juurutada andmebaasi ressursid turuplatsi kirjetena.

## <a name="deploy-the-mysql-resource-provider-vm"></a>MySQL-i ressursikeskuse pakkuja VM juurutamine

Nüüd, kui olete valmis Azure'i virnas PoC vajalikud serdid ja turuplatsi üksusi, saate juurutada SQL serveri ressursi pakkuja. Klõpsake vahekaarti **juurutamine MySQL-i pakkuja** :

   - Pakkuda juurutamise protsessi viitav faili JSON väärtused
   - Ressursi pakkuja juurutamine
   - Kohalikud DNS-i värskendamine
   - SQL serveri ressursi pakkuja adapterit registreerimine

### <a name="provide-values-in-the-json-file"></a>Sisestage väärtused JSON-failis

Klõpsake **Microsoft.MySqlprovider.Parameters.JSON**. See fail on Azure ressursihaldur malli peab õigesti juurutada Azure virnas parameetrid.

1. Täitke **tühjad** parameetrid JSON-failis.

    - Veenduge, et esitate **adminusername** ja **adminpassword** MySQL-i ressursikeskuse pakkuja VM.

    - Veenduge, et parooli ette **SetupPfxPassword** parameetri [ettevalmistamine prequisites](#prepare-prerequisites) etapis tehtud üles.

    - Veenduge, et esitate **basicAuthUserName** - ja **basicAuthPassword** parameetrid. **Märkige nende väärtuste.** Peate need hiljem ressursi pakkuja registreerimiseks.

2. Klõpsake nuppu **Salvesta**.

### <a name="deploy-the-resource-provider"></a>Ressursi pakkuja juurutamine

1. Klõpsake vahekaarti **Deploy-Microsoft.Mysql-provider.PS1** ja käivitage skript.
2. Tippige oma rentniku nimi Azure Active Directory, kui seda küsitakse.
3. Hüpikakna, esitada Azure'i virnas teenuse administraatori identimisteave.

Täielik juurutamise võib kuluda mõni väga kasutatud Azure'i virnas POCs 15 ja 45 minutit.

### <a name="update-the-local-dns"></a>Kohalikud DNS-i värskendamine

1. Klõpsake vahekaarti **Register-Microsoft.MySQL-fqdn.ps1** ja käivitage skript.
2. Azure Active Directory rentniku ID küsimise korral sisestage oma Azure Active Directory rentniku täielik domeeninimi: näiteks **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>SQL-i RP ressursi pakkuja registreerimine

1. Klõpsake vahekaarti **Register-Microsoft.My-provider.ps1** , ja käivitage skript.

2. Mandaadi küsimise korral kasutada märgitud **basicAuthUserName** ja **basicAuthPassword** parameetrid.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Azure'i virnas portaalis juurutamise kontrollimine

1. Funktsiooni ClientVM välja logida ja uuesti sisse logida, kui **AzureStack\User**.

2. Töölaual, klõpsake **Azure virnas POC portaali** ja teenuse administraatori portaali sisselogimine

3. Veenduge, et juurutamise õnnestus. Klõpsake nuppu **Sirvi** &gt; **Ressursi rühmad**, valige ressursirühm, mida kasutasite (vaikimisi **MySQLRP**), ning seejärel veenduge, et Essentialsi osa tera (Ülemine pool), loeb **juurutamise õnnestus**.


4. Veenduge, et registreerimine õnnestus. Klõpsake nuppu **Sirvi** &gt; **Ressursi teenusepakkuja**ja otsige **MySQL-i kohaliku aja järgi**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Esimese MySQL-i andmebaasi testimiseks juurutamise loomine

1. Logige sisse portaali Azure'i virnas POC teenus.

2. Klõpsake soovitud **+** nupp &gt; **kohandatud** &gt; **MySQL-i Server ja andmebaasid**.

3. Täitke vorm andmebaasi üksikasjadega.

    **Kirjutage "serveri nimi" tuleb sisestada.** Andmebaasi ühendused string sisaldab "Serverinime" osana kasutajanimi: näiteks ** "user@ <ServerName>"**. Kas soovite sisestada kasutajanimi vormingus andmebaasiga ühenduse loomisel: näiteks siis, kui juurutate MySQL-i abil veebisaidi loomine Azure veebisaidi ressursi pakkuja


## <a name="next-steps"></a>Järgmised sammud

Proovige muude [teenuste PaaS](azure-stack-tools-paas-services.md) nagu [SQL serveri ressursi pakkuja](azure-stack-sql-rp-deploy-short.md) ja [veebirakenduste ressursi pakkuja](azure-stack-webapps-deploy.md).
