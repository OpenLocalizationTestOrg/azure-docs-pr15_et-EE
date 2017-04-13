<properties
    pageTitle="Python veebi ja töötaja rollid Visual Studio | Microsoft Azure'i"
    description="Ülevaade Python Tools for Visual Studio abil luua Azure pilveteenustega, sh web rollid ja töötaja rollid."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Python veebi ja töötaja rollid Python Tools for Visual Studio

Selles artiklis antakse ülevaade Python veebi ja töötaja rollid, kasutades [Python Tools for Visual Studio][]abil. Saate teada, kuidas luua ja juurutada lihtsa pilveteenuses, mis kasutab Python Visual Studio abil.

## <a name="prerequisites"></a>Eeltingimused

 - Visual Studio 2013 või 2015
 - [Python Tools for Visual Studio][] (PTVS)
 - [Azure'i SDK tööriistad VS 2013][] või [Azure SDK tööriistad VS 2015][]
 - [Python 2.7 32-bitine][] või [Python 3.5 32-bitine versioon][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Mis on Python veebi ja töötaja rollid?

Azure'i pakub kolme arvutada mudelite töötavad rakendused: [Azure'i rakendust Service veebirakenduste funktsiooni][execution model-web sites], [Azure'i Virtuaalmasinates][execution model-vms], ja [Azure pilveteenustega][execution model-cloud services]. Kõigi kolme andmemudelid toetavad Python. Pilveteenustega, mis sisaldavad veebi ja töötaja rollid, pakkuda *teenust (PaaS)*. Pilveteenuse teenuses web rolli pakub spetsiaalne Internet Information Services (IIS) web server host veebi rakenduste, töötaja roll käitamise asünkroonne, pikaajalisi või järgmised toimingud kasutaja suhtluse või sisestatud sõltumatu.

Lisateavet leiate teemast [mis on pilv?].

> [AZURE.NOTE]*Otsimine luua lihtsa veebilehe?*
Kui teie stsenaariumi hõlmab lihtsalt lihtsa veebilehe ees, kaaluge kerge veebirakenduste funktsiooni teenuses Azure rakendus. Saate hõlpsasti uuendada pilveteenusesse veebisaidi kasvab ja muuta oma nõuetele. Vaadake teemat <a href="/develop/python/">Python Arenduskeskus</a> artiklite, mis hõlmavad Azure'i rakendust Service funktsiooni Veebirakenduste arendamine.
<br />


## <a name="project-creation"></a>Projekti loomine

Visual Studio, saate valida dialoogiboksis **Uus projekt** jaotises **Python** **Azure'i pilveteenuses** .

![Uue projekti dialoogiboks](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Azure'i pilveteenus viisardis, saate luua uue veebi ja töötaja rollid.

![Azure'i pilvepõhise teenuse dialoogiboks](./media/cloud-services-python-ptvs/new-service-wizard.png)

Töötaja rolli Mall sisaldab trafaretset koodi ühendamine Azure storage konto või Azure'i teenus siini.

![Pilve teenuse lahendus](./media/cloud-services-python-ptvs/worker.png)

Igal ajal saate lisada mõne olemasoleva pilveteenusesse veebis või töötaja rollid.  Saate lisada olemasolevaid projekte teie lahendus või uusi faile luua.

![Roll käsk Lisa](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Oma pilveteenuses võib sisaldada erinevates keeltes rakendanud rollid.  Näiteks saate määrata Python web rolli rakendada, kasutades Django, Python või C# töötaja rollid.  Saate hõlpsasti suhelda oma teenuse siini järjekorrad või salvestusruumi järjekorrad abil rollide vahel.

## <a name="install-python-on-the-cloud-service"></a>Pilveteenusesse Python installimine

>[AZURE.WARNING] Häälestamise skriptid, mis installitakse koos Visual Studio (selles artiklis viimati värskendati ajal) ei tööta. Selles jaotises kirjeldatakse minna.

Häälestamise skriptide peamine probleem on, et nad ei saa installida python. Esmalt määratleda kaks [käivitus tööülesannete](cloud-services-startup-tasks.md) [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) faili. Esimese tööülesande (**PrepPython.ps1**) laadib alla ja installib Python käitusaja. Teine (**PipInstaller.ps1**) käivitub pip sõltuvusi, võib-olla peate installimiseks.

Allpool skriptide on kirjutatud suunamise Python 3.5. Kui soovite kasutada versiooni 2.x Python, määratud **PYTHON2** muutuv faili **sisse** kaks käivitus tööülesanded ja käitusaja ülesande: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

**PYTHON2** ja **PYPATH** muutujate peab töötaja käivitus tööülesande lisada. **PYPATH** muutuja kasutatakse ainult siis, kui **PYTHON2** muutuja on seatud väärtusele **kohta**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Valimi ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Järgmisena Looge **PrepPython.ps1** ja **PipInstaller.ps1** failid on **. / salvenumbreid** kausta teie roll.

#### <a name="preppythonps1"></a>PrepPython.ps1

See skript installib python. Kui **PYTHON2** keskkonda muutuja väärtuseks olema installitud **ja seejärel Python 2.7** , muidu Python 3.5 installitakse.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

See skript helistab pip ja installib need kõik sõltuvused **requirements.txt** faili. Kui **PYTHON2** keskkonda muutuja väärtuseks **seejärel Python 2.7** kasutatakse, vastasel juhul kasutatakse Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>LaunchWorker.ps1 muutmine

>[AZURE.NOTE] Puhul **töötaja rolli** projekti **LauncherWorker.ps1** fail on vaja täita käivitus fail. **Web rolli** projekti, selle asemel määratletud käivitus faili projekti atribuudid.

**Bin\LaunchWorker.ps1** loodi algselt palju prep tööd teha, kuid see ei toimi. Asendage järgmise skripti faili sisu.

See skript helistab **worker.py** faili python projekti kaudu. Kui **PYTHON2** keskkonda muutuja väärtuseks **seejärel Python 2.7** kasutatakse, vastasel juhul kasutatakse Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Visual Studio Mallid olete loonud **ps.cmd** faili selle **. / salvenumbreid** kausta. See shell script nõuab tähelepanu PowerShelli ümbris skriptide ja pakub logimine PowerShelli ümbris nimega nime põhjal. Kui loodud fail, siin on, mis peaks olema seda. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Kohalik käivitamine

Kui seate oma pilvepõhise teenuse project käivitus projekti ja vajutage klahvi F5, käivituvad kohaliku Azure emulaator pilveteenusesse.

Kuigi PTVS toetab käivitamine emulaator, silumine (nt katkestuspunktid) ei tööta.

Silumine oma veebi ja töötaja rollid, saate määrata rolli projekti käivitamise projektiga ja silumine, mis selle asemel.  Samuti saate mitu käivitus projektid.  Paremklõpsake lahendus ja seejärel valige **Seadmine käivitus projektid**.

![Lahendus Käivitusatribuutide projekti](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Azure'i avaldamine

Avaldada, paremklõpsake pilvepõhise teenuse projekti lahendus ja seejärel valige **Avalda**.

![Microsoft Azure'i avaldamine sisselogimine](./media/cloud-services-python-ptvs/publish-sign-in.png)

Järgige viisardi juhiseid. Kui soovite, lubage Kaugtöölaud. Kaugtöölaua on kasulik, kui teil on vaja midagi silumine.

Kui olete valmis konfigureerimine sätted, klõpsake nuppu **Avalda**.

Edasiminekut kuvatakse väljundi aknas, siis kuvatakse akna Microsoft Azure'i tegevuste Logi.

![Microsoft Azure'i tegevuste Logi aken](./media/cloud-services-python-ptvs/publish-activity-log.png)

Juurutamise võtab mitu minutit, siis töötab teie web ja/või töötaja rollid Azure!

### <a name="investigate-logs"></a>Logide uurimine

Pärast pilvepõhise teenuse virtuaalse masina käivitub ja installib Python, saate vaadata logid, et leida sõnumid, mis tahes tõrge. Need logid asuvad selle **C:\Resources\Directory\\{rolli} \LogFiles** kausta. **PrepPython.err.txt** on vähemalt üks tõrge selle kui skript proovib tuvastada, kui installitud on Python ja **PipInstaller.err.txt** võib kurdavad pip aegunud versiooni.

## <a name="next-steps"></a>Järgmised sammud

Üksikasjalikumat teavet Visual Studio Python tööriistad rollide web ja töötaja töötamise kohta leiate teemast PTVS dokumentatsiooni kohta:

- [Pilveteenuse teenuse projektid][]

Azure'i teenused oma veebi ja töötaja rollid, kasutades Azure Storage või teenuse siini, nt kasutamise kohta lisateabe saamiseks lugege järgmisi artikleid.

- [Teenuse bloobimälu][]
- [Tabeli teenus][]
- [Järjekorra teenus][]
- [Teenus siini järjekorrad][]
- [Teenuse siini Teemad][]


<!--Link references-->

[Mis on pilv?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Teenuse bloobimälu]: ../storage/storage-python-how-to-use-blob-storage.md
[Järjekorra teenus]: ../storage/storage-python-how-to-use-queue-storage.md
[Tabeli teenus]: ../storage/storage-python-how-to-use-table-storage.md
[Teenus siini järjekorrad]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Teenuse siini Teemad]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Pilveteenuse teenuse projektid]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure'i SDK tööriistad VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure'i SDK tööriistad VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitine versioon]: https://www.python.org/downloads/
[Python 3.5 32-bitine versioon]: https://www.python.org/downloads/
