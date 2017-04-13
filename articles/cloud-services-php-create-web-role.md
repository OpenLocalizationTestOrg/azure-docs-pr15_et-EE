<properties
    pageTitle="PHP web ja töötaja rollid | Microsoft Azure'i"
    description="Juhend Azure pilveteenuses PHP web ja töötaja rollide loomine ja konfigureerimine PHP runtime."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Kuidas luua PHP web ja töötaja rollid

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab teile, kuidas luua PHP veebis või töötaja rollid Windows arengu keskkonnas, valida mõne kindla versiooni PHP "sisseehitatud" versioonid, mis on saadaval, muuta PHP konfiguratsiooni, lubada laiendid ja lõpuks võtta kasutusele Azure. See ka kirjeldatakse, kuidas konfigureerida veebis või töötaja rolli kasutada PHP käitusaja (koos kohandatud konfiguratsiooni ja laiendid), mis pakub.

## <a name="what-are-php-web-and-worker-roles"></a>Mis on PHP web ja töötaja rollid?

Azure'i pakub kolme arvutada mudelite töötavad rakendused: Azure'i rakendust Service, Azure'i Virtuaalmasinates ja Azure pilveteenustega. Kõigi kolme andmemudelid toetavad PHP. Pilveteenustega, mis sisaldab web ja töötaja rollid, annab *platvormi teenust (PaaS)*. Pilveteenuse teenuses web rolli pakub spetsiaalne Internet Information Services (IIS) web server host veebi rakendustele. Töötaja roll saab käitada asünkroonne, pikaajalisi või järgmised toimingud kasutaja suhtluse või sisestatud sõltumatu.

Nende suvandite kohta leiate lisateavet teemast [majutusteenuse suvandid, mis on esitatud Azure arvutada](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Azure'i SDK php allalaadimine

[Azure'i SDK php] koosneb mitmest osast. Selles artiklis kasutatakse kaks neist: Azure'i PowerShelli ja Azure emulators. Nende kahest osast saab installida Microsoft Web Installer platvormi kaudu. Lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Pilveteenustega projekti loomine

Luua projekti Azure'i teenus on esimene samm PHP veebis või töötaja rolli loomine. Azure'i teenus projekt, mis toimib loogilise ümbris veebi ja töötaja rollid ja see sisaldab projekti [teenuse määratlus (.csdef)] ja [teenuse konfigureerimine (.cscfg)] failid.

Azure'i teenus uue projekti loomiseks Azure PowerShelli Käivita administraatorina ja käivitage järgmine käsk:

    PS C:\>New-AzureServiceProject myProject

See käsk loob uue kataloogi (`myProject`) mis saate lisada web ja töötaja rollid.

## <a name="add-php-web-or-worker-roles"></a>Lisage PHP veebis või töötaja rollid

Projekti PHP web rolli lisada, käivitage järgmine käsk: jooksul projekti juurkaust:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Töötaja rolli, kasutage järgmist käsku:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] Funktsiooni `roleName` on valikuline parameeter. Kui see on välja jäetud, luuakse automaatselt rolli nimi. Esimese web ülesanne loodud `WebRole1`, teine `WebRole2`, jne. Esimese töötaja ülesanne loodud `WorkerRole1`, teine `WorkerRole2`, jne.

## <a name="specify-the-built-in-php-version"></a>Määrake sisseehitatud PHP versioon

Kui lisate PHP veebis või töötaja rolli projekti, projekti failid on muuta nii, et installitakse juurutamist iga veebis või töötaja rakenduse eksemplari PHP. PHP, mis installitakse vaikimisi versiooni kuvamiseks käivitage järgmine käsk:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Ülaltoodud käsu väljund sarnanevad mis on näidatud allpool. Selles näites on `IsDefault` lipu väärtuseks `true` PHP 5.3.17, mis näitab, et see oleks installitud PHP vaikeversiooniks.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Saate seada PHP käitusaja versioon PHP versiooni, mis on loetletud. Näiteks, et määrata PHP versioon (rolli nimega `roleName`) 5.4.0, kasutage järgmine käsk:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Saadaval PHP versioonid võib tulevikus muutuda.

## <a name="customize-the-built-in-php-runtime"></a>Sisseehitatud PHP käitusaja kohandamine

Teil on PHP käitusaja, mis installitakse teie järgige eespool, sh muutmine konfiguratsiooni üle täielik kontroll `php.ini` sätted ja laiendid, mis võimaldab.

Sisseehitatud PHP käitusaja kohandamiseks tehke järgmist.

1. Lisage uus kaust, nimega `php`, kuni soovitud `bin` kataloogi teie web roll. Töötaja rolli, lisage see selle rolli juurkaust.
2. Klõpsake soovitud `php` kausta, luua teise kausta nimega `ext`. Mis tahes sellele `.dll` laiendamine failid (nt `php_mongo.dll`), mida soovite lubada selle kausta.
3. Lisada mõne `php.ini` salvestada selle `php` kausta. Luba kõik kohandatud laiendid ja määrata mis tahes PHP suuniseid selle faili. Näiteks, kui soovite muuta `display_errors` sisse ja luba selle `php_mongo.dll` laiend, sisu oma `php.ini` fail on järgmised:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Mis tahes sätteid, mis pole selgesõnaliselt sätestatud soovitud `php.ini` faili pakuvad saavad automaatselt määrata vaikeväärtused. Siiski pidage meeles, et saate lisada täielik `php.ini` faili.

## <a name="use-your-own-php-runtime"></a>Kasutage oma PHP käitusaja
Mõnel juhul mitte valides sisseehitatud PHP käitusaja ja seda on eespool kirjeldatud konfigureerimise soovite esitada oma PHP runtime. Näiteks saate kasutada sama PHP käitusaja veebis või töötaja roll, mida kasutate oma arenduskeskkond. See on hõlpsam veenduge, et rakendus ei muutu käitumine tootmiskeskkonnas.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Web rolli kasutada oma PHP käitusaja konfigureerimine

Web rolli kasutada PHP käitusaja, mis pakub konfigureerimiseks toimige järgmiselt.

1. Luua Azure'i teenus projekti ja lisage PHP web rolli eelnevalt kirjeldatud selles teemas.
2. Luua mõne `php` kausta soovitud `bin` kausta, mis on web rolli juurkaust ja seejärel lisage oma PHP käitusaja (kõik kahendfaile, failid, alamkaustad jne) on `php` kausta.
3. (VALIKULINE) Kui teie PHP käitusaja kasutab [Microsoft SQL Server PHP draiverid][sqlsrv drivers], kas soovite konfigureerida oma web rolli installida [SQL Server 2012 kohalikke kliendi] [ sql native client] kui see on ette valmistatud. Selleks lisada [sqlncli.msi x64 installer] selle `bin` web rolli juurkaust kausta. Käivitus skripti, mis on kirjeldatud järgmises toimingus vaikselt käitatakse installer roll on ette valmistatud. Kui teie PHP käitusaja ei kasuta Microsoft Drivers php SQL serveri, saate eemaldada järgmine rida skripti näidatud järgmise juhise juurde:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Määratleda käivitus tööülesande, mis konfigureerib [Internet Information Services (IIS)] [ iis.net] kasutada oma PHP käitusaja käsitlema taotlused `.php` lehed. Selle tegemiseks avage soovitud `setup_web.cmd` faili (sisse selle `bin` fail web rolli juurkaust) tekstiredaktoris ja asendada selle sisu järgmise skripti:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Web rolli juurkaust oma rakenduse faile lisada. See on selle veebiserverisse juurkaust.

6. Rakenduse, nagu on kirjeldatud allpool jaotises [Avalda rakenduse](#how-to-publish-your-application) avalda.

> [AZURE.NOTE] Funktsiooni `download.ps1` script (sisse selle `bin` kausta web rolli juurkaust) kustutamist pärast kasutamise oma PHP käitusaja eespool kirjeldatud juhiseid.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Töötaja roll kasutada oma PHP käitusaja konfigureerimine

Töötaja roll kasutada PHP käitusaja, mis pakub konfigureerimiseks toimige järgmiselt.

1. Luua Azure'i teenus projekti ja lisage PHP töötaja roll eelnevalt kirjeldatud selles teemas.
2. Luua mõne `php` töötaja rolli juurkaust, kausta ja seejärel lisage oma PHP käitusaja (kõik kahendfaile, failid, alamkaustad jne) soovitud `php` kausta.
3. (VALIKULINE) Kui teie PHP käitusaja kasutab [Microsoft SQL Server PHP draiverid][sqlsrv drivers], kas soovite konfigureerida oma töötaja rolli installida [SQL Server 2012 kohalikke kliendi] [ sql native client] kui see on ette valmistatud. Selleks lisada töötaja rolli juurkaust [sqlncli.msi x64 installer] . Käivitus skripti, mis on kirjeldatud järgmises toimingus vaikselt käitatakse installer roll on ette valmistatud. Kui teie PHP käitusaja ei kasuta Microsoft Drivers php SQL serveri, saate eemaldada järgmine rida skripti näidatud järgmise juhise juurde:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Määratleda käivitus tööülesande, mis lisab teie `php.exe` käivitatava töötaja rolli tee keskkonnas kui roll on ette valmistatud muutujana. Selleks, et avada selle `setup_worker.cmd` faili (töötaja rolli juurkaust) tekstiredaktoris ja asendada selle sisu järgmise skripti:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Töötaja rolli juurkaust oma rakenduse faile lisada.

6. Rakenduse, nagu on kirjeldatud allpool jaotises [Avalda rakenduse](#how-to-publish-your-application) avalda.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Arvuta ja salvestusruumi emulators rakenduse käivitamiseks

Azure'i emulators pakuvad kohaliku keskkond, kus saate testida Azure rakenduse enne juurutamist pilveteenusesse. On mõned erinevused emulators ja Azure keskkonnas. Seda paremini mõista, vt [kasutamine Azure storage emulaator katsetamine](./storage/storage-use-emulator.md).

Pange tähele, et peab teil olema installitud kohalikult kasutama Arvuta emulaator PHP. Arvuta emulaator kasutama kohalikku PHP installi rakenduse käivitada.

Projekti käivitamiseks emulators, käivitage järgmine käsk: projekti juurkaust:

    PS C:\MyProject> Start-AzureEmulator

Kuvatakse väljund umbes järgmine:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Saate vaadata, avades veebibrauseris sirvimise väljund kohaliku aadressil emulaator teie rakendus (`http://127.0.0.1:81` eeltoodud näites väljund).

Emulators peatamiseks selle käsu:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Rakenduse avaldamine

Rakenduse avaldada, peate esmalt importida oma avaldamine sätted [Impordi-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdleti abil. Seejärel saate avaldada oma rakenduse [Avalda-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdleti abil. Sisselogimise kohta leiate artiklist [Kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [PHP Arenduskeskus](/develop/php/).

[Azure'i SDK php]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[teenuse määratlus (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[teenuse konfigureerimine (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[SQLNCLI.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
