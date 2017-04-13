<properties
   pageTitle=".Net-i installimine pilvepõhise teenuse rolli | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas käsitsi installida .NET framework pilvepõhise teenuse Web ja töötaja rollid"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Pilveteenuse teenuse rolli .net-i installimine 

Selles artiklis kirjeldatakse, kuidas installida .NET framework pilvepõhise teenuse Web ja töötaja rollid. Järgmiste juhiste abil saate installida .NET 4.6.1 Azure'i Külastajate OS pere 4. Külastajate OS väljalasete uusimat teavet teemast [Azure Külastajate OS väljastab ja SDK ühilduvus maatriks](cloud-services-guestos-update-matrix.md).

.Net-i installida, oma veebi ja töötaja rolli hõlmab .NET installijapaketi Cloud projekti ja käivitada installiprogrammi selle rolli käivitus tööülesannete osana.  

## <a name="add-the-net-installer-to-your-project"></a>.Net-i installiprogrammi lisamine projekti
- Laadige alla soovitud web Installeri .NET Frameworki soovite installida
    - [.NET 4.6.1 web Installeri](http://go.microsoft.com/fwlink/?LinkId=671729)
- Web rolli
  1. **Solution Exploreris**jaotises **rollid** pilvepõhise teenuse Projectis paremklõpsake oma rolli ja valige **Lisa > uus kaust**. Looge kaust nimega *prügikasti*
  2. Paremklõpsake kausta **Prügikast** ja valige **Lisa > olemasoleva kirje**. Valige .net-i installiprogrammi ja lisab selle kausta Prügikast.
- Töötaja rolli
  1. Paremklõpsake oma rolli ja valige **Lisa > olemasoleva kirje**. Valige .net-i installiprogrammi ja lisada selle rolli. 

Faile lisada sellisel viisil rolli sisu kausta automaatselt lisatud pilvepõhise teenuse paketi ja juurutatud virtuaalse masina ühtsete asukohta. Korrake seda toimingut kõigi veebi ja töötaja rollide oma pilveteenuses nii, et kõik rollid on installer koopia.

> [AZURE.NOTE] .NET 4.6.1 tuleks installida oma pilveteenuses rolli isegi siis, kui teie taotlus on suunatud .NET 4.6. Azure'i Külastajate OS sisaldab värskendused [3098779](https://support.microsoft.com/kb/3098779) ja [3097997](https://support.microsoft.com/kb/3097997). .NET 4.6 peal värskendused installimist võib põhjustada probleeme käivitamisel .NET rakenduste, seega peaksite installima otse .NET 4.6.1 .NET 4.6 asemel. Lisateavet leiate teemast [KB 3118750](https://support.microsoft.com/kb/3118750).

![Roll sisu Installeri failid][1]

## <a name="define-startup-tasks-for-your-roles"></a>Käivitus tööülesanded oma rollide määratlemine
Käivitus tööülesanded võimaldavad toiminguid enne rollid. .NET Frameworki installimise osana käivitus tööülesande tagab raames installimist enne mis tahes rakenduse koodis on käivitada. Käivitamise kohta lisateabe saamiseks lugege teemat tööülesannete: [Käivitamine käivitus tööülesannete Azure](cloud-services-startup-tasks.md). 

1. Lisage *ServiceDefinition.csdef* faili **WebRole** või **WorkerRole** sõlme all kõikide rollide järgmine tekst:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Ülaltoodud konfiguratsiooni käivituvad konsooli käsk *install.cmd* nii, et ta saab installida .NET Frameworki administraatoriõigused. Konfiguratsiooni ka loob mõne LocalStorage *NETFXInstall*nimi. Startup skripti seab kasutama kohalikku ressurss, et .NET Frameworki Installeri laaditakse alla ja selle ressursi installitud kausta temp. See on oluline selle ressursi mahu määramine vähemalt 1024MB tagada raames installitakse õigesti. Lisateavet käivitus teemast tööülesannete: [levinud pilveteenuses käivitus tööülesanded](cloud-services-startup-tasks-common.md) 

2. Faili **install.cmd** loomine ja lisamine kõikide rollide paremale klõpsake rolli alusel ja valides **Lisa > olemasoleva kirje...**. Nii, et kõik rollid peaks nüüd olema Installeri .net-i kui ka install.cmd fail.
    
    ![Roll sisu kõik failid][2]

    > [AZURE.NOTE] Selle faili loomiseks kasutada lihtsat tekstiredaktorit, nt Notepadis. Kui Visual Studio abil saate luua tekstifaili ja pange sellele siis nimeks "cmd" faili siiski võib sisaldada UTF-8 bait tellimuse märgi ja töötab skripti esimene rida tulemuseks viga. Kui Visual Studio abil saate luua lisada faili lahku on REM (märkus) faili esimene rida nii, et seda ignoreeritakse käitamisel. 

3. **Install.cmd** faili järgmise skripti lisamiseks tehke järgmist.

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Installi skripti kontrollib, kas määratud .NET Frameworki versioon on juba installitud arvutis, päringute register. Kui .net-i versioon pole installitud, siis .net Web Installeri on käivitatud. Mis tahes probleemide tõrkeotsinguks logib skripti kõik tegevuse fail nimega *startuptasklog-(currentdatetime) .txt* talletatud *InstallLogs* kohaliku salvestusruumi.

    > [AZURE.NOTE] Skripti endiselt näitab, kuidas installida .NET 4.5.2 või .NET 4.6 järjepidevuse. Ei ole vaja .NET 4.5.2 käsitsi installida, kui see on juba Azure'i Külastajate OS-is saadaval. Asemel .NET 4.6 installimist peaksite installima otse .NET 4.6.1 [KB 3118750](https://support.microsoft.com/kb/3118750)tõttu.
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Diagnostika käivitus tööülesande logid Bloobivahemälu salvestusruumi edastamiseks konfigureerimine 
Lihtsustamiseks, mis tahes installimise tõrkeotsing saate konfigureerida Azure diagnostika mis tahes käivitus skript või .net-i installiprogrammi bloobimälu loodud logifailide edastamiseks. Seda moodust saab vaadata logid lihtsalt logifailide allalaadimisega bloobimälu asemel vajaduseta kaugtöölaua üheks roll.

Diagnostika Ava *diagnostics.wadcfgx* konfigureerimine ja lisada **kataloogide** sõlme all järgmist: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

See tuleb konfigureerida azure diagnostika diagnostika salvestusruumi konto *netfx-installi* bloobimälu ümbrises kõik failid jaotises *NETFXInstall* ressursside kataloogis *log* edastada.

## <a name="deploying-your-service"></a>Oma teenuse juurutamine 
Oma teenuse juurutamisel käivitus tööülesanded Käivita ja installida .NET Frameworki, kui see pole juba installitud. Teie rollid on hõivatud oleku ajal installib ja võib-olla isegi kui Frameworki installi nõuab taaskäivitage raames. 

## <a name="additional-resources"></a>Lisaressursid

- [.NET Frameworki installimine][]
- [Kuidas: .NET Frameworki versioon on installitud][]
- [.NET Frameworki installide tõrkeotsing][]

[Kuidas: .NET Frameworki versioon on installitud]: https://msdn.microsoft.com/library/hh925568.aspx
[.NET Frameworki installimine]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[.NET Frameworki installide tõrkeotsing]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
