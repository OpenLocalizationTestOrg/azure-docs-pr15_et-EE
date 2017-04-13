<properties 
pageTitle="Azure'i pilveteenustega käivitus ülesannete läbiviimisel | Microsoft Azure'i" 
description="Käivitus ülesanded aitavad oma rakenduse teenus cloud keskkonna ettevalmistamiseks. See õpetab teile, kuidas käivitus tööülesannete töötavad ja kuidas neid" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Kuidas konfigureerida ja käivitada käivitus ülesanded pilveteenus

Käivitus tööülesannete abil saate toiminguid teha enne rollid. Installimise osa, registreerimisel COM-komponendid, registrivõtmed seadmine või kaua töötavad protsessi alates kaasamine toimingute kohta, mida võiksite teha.

>[AZURE.NOTE] Käivitus tööülesanded pole rakendatav Virtuaalmasinates, ainult pilvepõhise teenuse Web ja töötaja rollid.

## <a name="how-startup-tasks-work"></a>Kuidas töötavad käivitus tööülesanded

Käivitus ülesanded on toimingud, mis on võetud enne oma rollid alguse ja on määratletud [ServiceDefinition.csdef] faili, kasutades [tööülesande] elemendi [käivitus] elemendi sees. Sageli käivitus ülesanded oleksid paketi faile, kuid neid saab konsooli rakendusi või paketi failid, mis algavad PowerShelli skriptide.

Keskkonna muutujate teabe edastamiseks käivitus tööülesande ja kohalikku saab kasutada teabe käivitus tööülesande välja. Näiteks mõne muutuja saate määrata tee programmi, mida soovite installida ja faile saab kirjutada kohaliku salvestusruumi, mis saab siis lugeda hiljem oma rollid.

Tööülesande käivitamisel saate logida teavet ja tõrgete **TEMP** keskkonna muutuja määratud kataloogi. Käivitus ülesanne, **TEMP** keskkonna muutuja laheneb *C:\\ressursid\\temp\\[guid]. [ rolename]\\RoleTemp* kataloog käivitamisel pilve.

Käivitus tööülesanded saate teostada mitu korda taaskäivitamisega vahel. Näiteks käivitatakse käivitus tööülesande iga kord, kui roll ringlusse ja rolli ringlusse ei pruugi alati lisada uuesti. Käivitus tööülesanded tuleb kirjutada nii, et neid probleemideta korduvalt kasutada.

Käivitus tööülesannete lõpus peab olema **errorlevel** (või välju kood) null startup protsessi lõpuleviimiseks. Kui käivitus tööülesande lõpus on null **errorlevel**, ei käivitu roll.


## <a name="role-startup-order"></a>Roll käivitus järjestuses

Järgmiselt on loetletud rolli käivitamisel kord Azure.

1. Näiteks on märgitud **algus** ja ei saa liikluse.

2. Kõik käivitus tööülesanded täidetakse vastavalt nende **taskType** atribuut.
    - **Lihtsa** tööülesannete täidetakse sünkroonselt, ükshaaval.
    - **Tausta** ja **esiplaani** tööülesanded käivitatakse asünkroonselt, paralleelselt käivitus tööülesannet.  
       
    > [AZURE.WARNING] IIS-i ei pruugi olla täielikult konfigureeritud käivitamise etapis käivitus tööülesande nii rolli seotud andmed ei pruugi olla saadaval. Käivitus ülesanded, mis nõuavad roll andmete tuleks kasutada [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).

3. Roll Majutaja protsess käivitatakse ja IIS-i saidi loomist.

4. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) meetodit nimetatakse.

5. Näiteks on märgitud **lõpetatuks** ja liikluse marsruuditakse eksemplari.

6. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) meetodit nimetatakse.


## <a name="example-of-a-startup-task"></a>Näide: ülesande käivitamine

Käivitus tööülesanded on määratletud [ServiceDefinition.csdef] faili **tööülesande** elementi. **Käsureal** atribuut määrab nime ja parameetrite käivitus pakkfail või konsooli käsk **ExecutionContextis** atribuut määrab õiguste tase käivitus tööülesande ja **taskType** atribuut saate määrata, kuidas tööülesande teostada.

Selles näites on keskkonna muutuja, **MyVersionNumber**, käivitus ülesande jaoks loodud ja määratud väärtuse "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

Järgmises näites **Startup.cmd** pakkfail kirjutab rea "praegune versioon on 1.0.0.0" StartupLog.txt faili TEMP keskkonna muutuja määratud kataloogis. Funktsiooni `EXIT /B 0` joone tagab, et käivitus tööülesande lõpeb **errorlevel** , on null.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] Visual Studio paketi käivitamine faili atribuudi **väljundkausta Kopeeri** seadma **Kopeeri alati** olla kindel, et projekti Azure oma käivitus pakkfail on õigesti juurutatud (**approot\\prügikasti** Web rollid ja **approot** töötaja rollide jaoks).

## <a name="description-of-task-attributes"></a>Tööülesande atribuutide kirjeldus

Järgnevalt **tööülesande** elemendi [ServiceDefinition.csdef] faili atribuute:

**käsureal** – saate määrata ülesande käivitamisel käsurea.

- Käsk, valikuline käsurea parameetrid, mis algab käivitus tööülesanne.
- Sageli see on cmd või .bat pakkfail failinimi.
- Tööülesanne on sõltuv selle AppRoot\\juurutamise kausta Prügikast. Keskkonna muutujate on laiendatud ei tee ja faili tööülesande määramisel. Kui keskkonna laiendamine on vajalik, saate luua väike cmd skripti, mis nõuab tööülesande käivitamine.
- Saab konsooli rakendus või pakkfail, mis käivitab [PowerShelli skripti](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**ExecutionContextis** – saate määrata õiguste tase käivitus ülesande jaoks. Õiguste tase saate piiratud või tõstetud.

- **piiratud**  
Käivitus käivitub sama privileegidega rolliga. Kui **ExecutionContextis** atribuuti [Runtime] element on **piiratud**, kasutatakse kasutaja õigusi.

- **laiendatud**  
Käivitus käivitub administraatori õigustega. See võimaldab käivitus tööülesannete installida rakendused, IIS-i konfigureerimine muuta, registri muudatuste tegemine ja muid administraatori taseme ülesandeid, ilma õiguste tase ise roll.  

> [AZURE.NOTE] Õiguste tase käivitus ülesande ei pea olema sama mis roll ise.

**taskType** – saate määrata nii, nagu ülesande käivitamine on täidetud.

- **lihtne**  
Tööülesannete täidetakse sünkroonselt, ühe korraga nii, et [ServiceDefinition.csdef] failis määratud. Kui üks **lihtne** käivitus ülesanne lõpeb **errorlevel** , on null, käivitatakse järgmine **lihtne** käivitus ülesanne. Kui veel **lihtne** käivitus ülesandeid täita pole, siis roll ise käivitamise.   

    > [AZURE.NOTE] Kui **lihtne** ülesanne lõpus on null **errorlevel**, näiteks on blokeeritud. Järgmise **lihtsa** käivitus tööülesandeid ja roll ise, ei käivitu.

    Veenduge, et teie pakkfail lõpeb **errorlevel** , on null, käsu `EXIT /B 0` oma Pakktöötlus faili lõpus.

- **tausta**  
Tööülesannete täidetakse asünkroonselt, paralleelselt käivitus roll.

- **esiplaani**  
Tööülesannete täidetakse asünkroonselt, paralleelselt käivitus roll. Võtme vahe on **nuppude Esiplaan** ja **taust** tööülesande on, et **esiplaani** tööülesande takistab roll ringlussevõtu või sulgemise kuni tööülesanne pole enam kasutuses. See piirang pole **tausttoimingud** .

## <a name="environment-variables"></a>Keskkonna muutujad

Keskkonna muutujate on viis teabe edastamiseks tööülesande käivitamine. Näiteks saate panna bloobimälu, mis sisaldab rakenduse installimiseks või pordinumbrid ja oma rolli kasutavate sätted juhtimine funktsioonide käivitus tööülesande tee.

On kahte tüüpi keskkonna muutujate käivitus tööülesannete; staatilise keskkonna muutujate ja keskkonna muutujate liikmed [RoleEnvironment] klassi põhjal. Mõlemad on [ServiceDefinition.csdef] faili jaotises [keskkonna] ja mõlemad kasutada [muutuv] atribuut elemendi ja **nimi** .

Staatilise keskkonna muutujate kasutab [muutuv] elemendi atribuudi **väärtust** . Ülaltoodud näites loob keskkonna muutuja **MyVersionNumber** on staatiline väärtus "**1.0.0.0**". Teine näide oleks luua **StagingOrProduction** keskkonna muutuja, mille saate käsitsi määrata väärtustega "**lavastus**" või "**tootmise**" erinevate käivitus toiminguid **StagingOrProduction** keskkonna muutuja väärtuse alusel.

Keskkonna muutujate põhjal liikmed RoleEnvironment klassi Kasuta [muutuv] elemendi atribuudi **väärtust** . Selle asemel on tütarelement [RoleInstanceValue] vastav **XPath** atribuudi väärtuseks saab luua keskkonna muutuja, mis põhineb [RoleEnvironment] klassi teatud liige. Väärtuste **XPath** atribuudi juurdepääsu erinevad [RoleEnvironment] väärtused leiate [siit](cloud-services-role-config-xpath.md).



Keskkonna muutuja, mis on "**true**", kui eksemplari töötab Arvuta emulaator ja "**false**" pilveteenuses käivitamisel loomiseks kasutada näiteks [Muutuja] ja [RoleInstanceValue] järgmisi elemente:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Järgmised sammud
Saate teada, kuidas teha mõned [põhitoimingud käivitus](cloud-services-startup-tasks-common.md) oma pilveteenuses.

[Paketi](cloud-services-model-and-package.md) oma pilveteenuses.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Ülesanne]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Käivitus]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Käitusaja]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Keskkonnas]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Muutuja]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx